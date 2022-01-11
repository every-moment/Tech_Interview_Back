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
