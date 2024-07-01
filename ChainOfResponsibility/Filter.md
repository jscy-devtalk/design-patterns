# Filter
## Filter 생성 및 구동
1. 웹 애플리케이션 실행 시 바로 필터 객체가 생성되고 init() 메서드가 호출된다.
2. 요청이 들어올 때마다 doFilter() 가 호출된다. 서블릿이 실행되기 전 필터가 먼저 실행되며 서블릿을 실행한 후 다시 필터로 리턴한다. 
```java
// 다음 필터를 실행한다.
// 만약 다음 필터가 없으면,
// 요청한 서블릿의 service() 메서드를 호출한다.
// service() 메서드 호출이 끝나면 리턴된다.
chain.doFilter(request, response);
```
3. 웹 애플리케이션을 종료할 때 destroy() 가 호출된다.

## 필터의 용도
서블릿을 실행하기 전후에 기능을 추가할 때 사용한다. 

*기능 유형
- 암호 해제/암호화
- 압축 해제/압축
- 인코딩/디코딩
- 로깅(logging)
- 권한 검사


## 필터 구동 실체
Filter에는 GoF의 "Chain of Responsibility" 패턴이 적용되어 있다. 

>Chain of Responsibility 패턴:
>요청을 처리하는 객체들의 연결된 체인을 만들어 각각의 객체가 요청을 처리하거나 다음 객체로 전달하는 패턴. 기존 코드를 손대지 않고 기능을 추가하거나 제거할 수 있는 설계 기법이다

<img src="../../img/Filter.png">

1. 클라이언트 요청이 들어오면 Servlet Container는 FilterChain을 실행한다. 
2. FilterChain은 내부적으로 등록된 필터를 찾아 실행한다. 
3. chain.doFilter를 통해 다음 필터가 없을 때까지 반복한 후 없으면 service() 메서드를 호출한다. 
4. 종료할 때는 service() 부터 실행된 순서 반대로 종료해나간다. 

Chain of Responsibility는 클래스 간 Coupling이 맺지 않아 의존관계가 없고 독립적이기 때문에 유지보수가 쉽다. 