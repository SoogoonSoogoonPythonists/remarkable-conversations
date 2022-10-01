## FastAPI 사용자 모임

[FastAPI 사용자 모임](https://open.kakao.com/o/gm7tYg6c) 방에서 나온 내용 중 일부를 공유합니다.

---

> sqlalchemy 질문이 있습니다. 
> 
> scoped_session을 사용하지 않아도 매번 session을 계속 새로 생성해서 사용하는거면 다중스레드에서도 매번 새로운 session을 만드니까 문제없는거 아닌가?
> 
> -> 놉, session객체 자체는 새로 생성하지만 멀티스레드 환경에서 동일한 데이터 영역(threading.thread)을 공유하기 때문에 문제가 생길수 있다. 그렇기 때문에 하나의 스레드당 하나의 session을 사용하는 scoped_session을 사용하자. scoped_session은 threading.local을 사용하기 떄문에 각 스레드마다 하나의 session을 보유하기 때문에 thread-safety하게 사용할 수 있다.
>
> 라고 이해 했는데 맞을까요??
>
> threading.local을 사용하기 떄문에 각 스레드마다 하나의 session을 보유하기 때문에 -> 스레드 각자의 stack 메모리 공간에 session을 만들기 때문에 thread 끼리 영향이 없음.

> 혹시나 도움이 될까 싶어… 자문자답합니다.
> 멀티 스레딩 환경에서 session이 공유되는 형태, 예를들자면 SQLAlchemy Session이 함수의 파라미터로 넘겨지는 형태에서 멀티스레딩으로 처리되는 코드가 있다고 했을때… 여러 스레드에서 동일한 session을 공유하는 형태가 되고 이때 트랜잭션 혹은 session의 commit, flush 등 동작에서 각 스레드에 영향을 주게 된다. (의도치 않은 동작 혹은 오류가 있을수 있다)
>
> 그렇기 때문에 thread-local을 사용하는 scoped_session을 사용해서 멀티스레딩 환경에서의 thread-safe를 보장한다.
>
> 만약 Session을 사용할 경우 멀티 스레딩 환경에서 Session을 하나의 스레드에서만 사용한다거나…혹은 제어 할 수 있는 로직을 짠다면 상관이 없지만 이런 부분들을 직접 제어하기 어렵다면 scoped_session을 사용하는게 좋다.
> 
> https://jay-ji.tistory.com/114
