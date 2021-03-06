# 17. MultiSyncWaits

![image](https://user-images.githubusercontent.com/41619898/71492127-4a8a8500-2878-11ea-84df-cdf3cd28a46b.png)

##### arhSyncs[] 배열 인덱스 1의 이벤트 객체를 수동 리셋 이벤트로 생성시키고, 콘솔에서 "event"를 입력해 이벤트 커널 객체를 시그널 상태로 만들어 본 경우이다.

##### 이벤트에 대한 대기에서 깨어나 이벤트 관련 처리를 수행한 후 스레드는 다시 루프를 돌아 대기 함수로 돌아가지만, 수동 리셋 이벤트이므로 이미 시그널 상태로 남아 있기 때문에 대기 없이 바로 대기 함수를 빠져나와 버린다.

##### 이 과정이 계속해서 반복되며, 또한 이벤트 객체의 인덱스가 뮤텍스나 세마포어의 배열상의 인덱스보다 빠르기 때문에 뮤텍스나 세마포어의 시그널을 받을 여지도 없이 무한이 이벤트 시그널링에 대해서만 반응하는 상태로 빠진다.

##### 이벤트 객체가 계속 시그널 상태로 남아있기 때문에 멈춤 없이 이벤트 처리 루틴만 엄청 빠른 속도로 무한히 반복되는 것인데, 만약 이와 같이 수동 리셋 이벤트로 처리했다면 대기 함수에서 탈출 후 작업을 한 다음 이벤트 객체를 다시 넌시그널 상태로 만들어야만 스레드는 다음 번의 대기 함수에서 잠들 수 있게 된다.

```c++
		...
         
		dwWaitCode -= WAIT_OBJECT_0;
		switch (dwWaitCode)
		{
			case 1 : // 이벤트 시그널
				cout << " +++++ Event Singnaled!!!" << endl;
				ResetEvent(parSyncs[dwWaitCode]);
			break;
                
			...
```

##### 이와 같이 ResetEvent를 호출해 넌시그널 상태로 만들거라면 굳이 수동 리셋 이벤트로 가져갈 이유없이 자동 리셋 이벤트를 이용하면 더 간단한 코드를 작성할 수 있다.

```
arhSyncs[1] = CreateEvent(NULL, FALSE, FALSE, NULL);
```

##### 결국 수동 리셋 이벤트는 여러 스레드가 하나의 이벤트 객체를 바라보며 동시에 시그널을 받고자 할 때 사용한다.

### 따라서 시그널 상태가 된 후 작업이 끝나면 루프를 돌아 대기 상태로 들어가기 전에 반드시 ResetEvent를 호출해 넌시그널 상태로 만들어줘야 한다는 것이다.