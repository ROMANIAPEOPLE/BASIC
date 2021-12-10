<details>
<summary>접기/펼치기 버튼</summary>
<div markdown="1">

|제목|내용|
|--|--|
|1|1|
|2|10|

</div>
</details>

---

<details>
<summary>ThreadPoolTaskExecutor</summary>
<div markdown="1">

### ThreadPoolTaskExecutor
![스크린샷 2021-12-10 오전 11 24 04](https://user-images.githubusercontent.com/39195377/145506775-7d3609f5-e628-4d25-95c0-da0b7bcb2858.png)


##### corePoolSize : 동시에 실행시킬 쓰레드의 갯수를 의미, default : 1

##### maxPoolSize : 쓰레드 풀의 최대 사이즈를 의미, default : Integer.MAX_VALUE

##### queueCapacity : 큐의 사이즈를 지정하며 default 값은 Integer.MAX_VALUE



#### *주의사항

최초에 core size 만큼 동작하다가 더 이상 처리할 스레드가 부족할 경우 max size만큼 스레가 증가하지 않는다.

`queueCapacity` 크기 만큼 queue에서 대기하다가, 큐가 꽉 차게 되면 그 때 max 사이즈 만큼 스레드를 생성해서 처리한다.

만약 위 예시처럼 설정한다면 **최초 5개의 스레드에서 작업을 처리하다가 스레드가 부족할 경우 100개의 작업까지 큐에서 대기한 후 , 100개가 넘어가면 최대 10개의 스레드를 추가로 사용하게 된다.**



#### RejectedExecutionException 

core 스레드를 전부 사용하고, 큐까지 꽉 찬 상태에서 max size로 설정한 스레드까지 모두 사용해서 스레드가 부족하게 된다면 해당 예외가 발생한다.

##### RejectedExecutionHandler를 구현한 클래스 

- AbortPolicy

  - 기본 설정
  - RejectedExecutionException을 발생시킵니다.

- DiscardOldestPolicy

  - 오래된 작업을 skip 합니다.
  - 모든 task가 무조건 처리되어야 할 필요가 없을 경우 사용합니다.

- DiscardPolicy

  - 처리하려는 작업을 skip 합니다.
  - 역시 모든 task가 무조건 처리되어야 할 필요가 없을 경우 사용합니다.

- CallerRunsPolicy

  - shutdown 상태가 아니라면 ThreadPoolTaskExecutor에 **요청한 thread에서 직접 처리**합니다.

  - **!!예외와 누락 없이 최대한 처리하려면 `CallerRunsPolicy`로 설정**

![스크린샷 2021-12-10 오전 11 24 20](https://user-images.githubusercontent.com/39195377/145506794-9f439bc4-b594-4433-a448-f9f418d08fe3.png)

---

#### Shutdown

Spring Boot Actuator를 이용해서 종료를 시켜보면 호출 즉시 application이 **바로 종료**되는 것을 확인할 수 있다.

```http
POST http://localhost:8888/actuator/shutdown
```

이렇게 즉시 종료되면 아직 처리되지 못한 task는 유실된다. 유실 없이 마지막까지 다 처리하고 종료되길 원한다면 설정을 추가해야 한다.



---

#### Timeout

만약 모든 작업이 처리되길 기다리기 힘든 상황이라면 최대 종료 대기 시간(Timeout)을 설정할 수 있다.

![스크린샷 2021-12-10 오전 9.53.18](/Users/kh/Library/Application Support/typora-user-images/스크린샷 2021-12-10 오전 9.53.18.png)



</div>
</details>
