[toc]

# Thread 기본과 동기화

- `pthread_join`: 메인 스레드에서 해당 스레드가 종료되고, 종료 상태값을 가지고 추가 처리가 가능
- `pthread_detach`: 해당 스레드가 종료되면 즉시 해제함



## :heavy_check_mark: 스레드 디태치

- 해당 스레드가 종료될 경우, 즉시 관련 리소스를 해제(free)한다.
  - pthread_join를 기다리지 않고, 종료 즉시 리소스를 해제한다.

```c
// thread: detach할 스레드 식별자
int pthread_detach(pthread_t thread)
```



### 코드 예제

![image-20210311210911136](assets/image-20210311210911136.png)

![image-20210311211124970](assets/image-20210311211124970.png)



**pthreadsimple4**

> Thread1 - detach
>
> Thread2 - join

![image-20210311211158594](assets/image-20210311211158594.png)





## :heavy_check_mark: Pthread 뮤텍스 - 상호 배제 기법

### 뮤텍스 선언과 초기화

```c
pthread_mutex_t mutex_lock = PTHREAD_MUTEX_INITIALIZER;
```



### 뮤텍스 락 걸기/풀기

```c
int pthread_mutex_lock(pthread_mutex_t *mutex);
int pthread_mutex_unlock(pthread_mutex_t *mutex);
```



### 코드예제

![image-20210311211555923](assets/image-20210311211555923.png)

![image-20210311211718426](assets/image-20210311211718426.png)



**mutex 제거 버전**

![image-20210311211829598](assets/image-20210311211829598.png)

![image-20210311211858450](assets/image-20210311211858450.png)