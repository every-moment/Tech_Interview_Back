# 🐼 Java

## static 을 왜 쓰는가 ? 성능에 향상이 있는가 ? 

우선 특징부터 알아보자. <br>

static 변수는 객체를 생성해서 참조할 필요가 없다. 클래스 참조를 한다. <br>
static 불록은 클라스가 최초 로딩될 때 수행되므로 생성자 실행과 상관없이 수행. <br>
하나의 JVM, WAS 에서는 같은 주소에 존재하며 GC 대상이 되지 않음.<br>
만약 여러 스레드가 같이 접근하면 데이터가 꼬일 수 있음 <br>

자주 쓰는 절대 변하지 않는 값은 final 을 붙이고 static을 붙이자. <br>
- 이유는 명확하다. final을 붙이지 않는다면 여러 스레드가 접근했을 때 데이터의 무결성(정확함)이 깨진다. <br>
- final을 붙임으로써 한번만 할당이 되며 그다음에는 변화시키면안된다. <br>

GC의 대상이 아니기에 Array 나 Map등의 Collection을 사용하면 OutOfMemory 메모리 릭이 나올 수 있다. <br>

## POJO란 무엇인가요?

우선 POJO란 객체가 아니라 클래스다.

클래스를 확장하거나 인터페이스를 구현하지 않는다. <br>
클래스 변경이 가능하다. <br>
매개변수를 허용하지 않는 public 생성자를 갖는다. (기본생성자) <br>
private 변수의 값을 저장하고 조회하는데 public 메소드를 사용한다. <br>

POJO의 하나의 예이다. <br>

```java 
public POJO {
 private int value = 0;
 
 public POJO() {
   // 기본 생성자
 }
 
 public getValue() { return value; };
 public setValue(final int value) { this.value = value; };
}
```

## Input, Output Stream

한번쯤은 다시 정리해야했다. 우선 간단하게만 결론을 내면 <br>
읽는 것은 **in**,  어디에 기록하는 것은 **out** 이라고 생각하면 된다. 상대적인 것이라고 생각하면 되려나...😗😗 <br>

예를 들자면 System.in 같은 경우에는 키보드에 입력하는 것이고 System.out은 콘솔에 출력하는 것이다.  <br>
이렇게 in, out 이 있으면 중간에 하나의 파이프같은 것이 있을 텐데 그것을 **Stream** 이라고 한다. <br>
( *단방향이다. 즉슨, 입력과 출력을 하려면 두 개의 Stream이 필요로 한다는 것 ) <br>

이 개념이 키보드 뿐만 아니라 네트워크 통신, 메모리 등 모든 부분에서 쓰인다. <br>

음.. 이게 매일 헷갈리는 이유는 상대적인 부분이여서 그렇다.<br>
입력을 할때 당연히 쓰이는 스트림은 입력 스트림인데 '우리가 키보드를 쓰니까 쓴 것이 나간다!' 라는 직감이 오는데 그게 아니라 이 입력을 받는 것에 초점을 맞춰야한다. <br>
즉슨, 우리가 키보드에 입력을 하면 받는 주체는 컴퓨터고 컴퓨터는 이 입력받은 것을 입력스트림을 통해 받는 것 !<br>

이러한 입력, 출력에 대한 스트림 종류가 많은 데 입력은 말그대로 그 주체에서 읽는 거니까 Reader 를 쓰고 출력은 내뱉고 써야하느까 Writer 을 쓴다. <br>
![](https://images.velog.io/images/minyul/post/a7cbfdfc-24ee-4161-8bc7-3e04e7d1f409/image.png) <br>

## GC

OutOfMemoryError 일 때, 

1. GC verbose 를 켠다 -verbose:gc
2. verbose GC 출력을 살펴 Java Heap footprint를 살핀다. (Young OLD 과 Gen 의 비율 등)
3. verbose GC 출력이 JConsole 과 같은 툴을 사용하여 Java Heap 이 시간이 경과하면서 점점 사용률이 올라가는지 살핀다.
4. Young Gen 에서 다수의 짧은 주기의 객체들이 생성되는 지 살핀다. Java Heap 영역의 필요한 만큼보다 작을 때 이러한 현상이 일어난다.
5. 메모리 누수나 Old Gen footprint 를 살릴 필요가 있을 때, -XX:+HeapDumpOnOutOfMemoryError JVM 옵션 추가한 뒤 프로그램을 실행하여,
OOM 발생 후 Heap Dump 를 얻을 수 있는데 Memory Analyzer 나 JHat 과 같은 툴을 이용하는 분석하는데 용이할 것이다.
