# 🐼 이펙티브자바

## 예외 

예외는 진짜 예외 상황에서만 사용해라. <br>

책을 읽다보니 2가지의 경우가 생기는데 <br>
표준 관용구 vs try catch 블록안에 넣기 <br>
전자 같은 경우에는 for문을 딱 끝까지 돌리는 것과 try catch 같은 경우에는 무한루프로 돌려서 JVM이 그 배열 인덱스를 넘게 된다면 <br>
에러를 내뱉는것. <br>

여기서 내가 몰랐던 부분은 최적화 ( 빠르기 ) 표준 관용구가 훨씬 빠르다는 것 <br>

되게 간단하게 결론만 말하자면, <br>
올바르지않은 값일 때, null - Optional을 써서 제어하자.<br>
예외는 예외상태에서만 쓰자. 쓸데없는 예외를 넣지 말자.<br>

이 아이템을 읽으면서 내가 경험적으로 느끼는 것은 external API에서 값을 불러오고 거기에 다한 최우선의 데이터를 뽑을 때, <br>
Data.get(0) 으로 뽑았고 이 안에 try catch 를 써서 ArrayIndex Excecption을 넣엇다. <br>
하지만 이것이 아닌 Data 에 대한 값을 ObjectUtils.isEmpty 또는 size == 0 의 메소드를 이용하여 <br>
확인하고 JVM 성능 최적화에 맞지 않는 try catch 를 지양해야겠다고 생각하고 있다. <br>


에러 vs 예외 <br>

에러는 시스템에 비정상적인 상황이 생겼을때. 개발자가 미리 예측하여 처리할 수 없는 것 <br>
이부분 신경써줄 필요가 없다. 시스템 레벨인거지.<br>

예외는 개발자가 구현한 로직에서 발생. 즉, 예외는 발생할 상황을 미리 예측하여 처리 가능. <br>
모든 예외클래스는 Throwable 클래스를 상속받고있다. <br>

여기서 이 Throwable 클래는 ERROR 또한 상속받고 있는데 이부분은 시스템적으로 개발자가 할 수 없기에 우선 생각안해도 된다. <br>
이제 Exception 같은 경우에는 Throwable을 모두 상속하고있다. <br>

자, 쉽게 생각하다 여기서 RuntimeException을 제외한 모든 Exception은 CheckedException 이다. <br>
IOException , SQLException 은 CheckedException 이다. 컴파일 단계이며 반드시 예외처리를 해야한다. <br>
즉슨, IOException, SQLException 이 날 것 같은 메소드는 무조건 try catch 를 써줘야한다는거지. <br>
list에 있는 부분에서는 try catch를 할필요가없다는야. <br>

진짜 쉽게 생각하면 컴파일단계에서 발견할수있는건 체크할수있는 예외. 그 밖의 모든건 런타임에러로 보면된다 .<br>
IOException, SQLException과 같은 체크드 인셉션같은 경우 글로벌하게 ControllerAdvice, ExceptionHandler 를 이용하여 <br>
핸들링한다. <br>


                                                                           
