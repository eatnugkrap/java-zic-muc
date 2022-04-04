### 점프 투 자바 ] - 기본 1. 환경 설정과 확인해볼 기본 문법

## 환경

jvm은 자바파일(.java)을 컴파일한 자바 바이너리파일(.class) 파일을 실행할 수 있다. 


jre는 JVM이 자바 프로그램을 동작시킬 때 필요한 라이브러리들을 포함한 자바 실행환경이다. 

jdk는 말그대로 자바로 개발을 하기 위한 킷으로, JRE + 개발을 위한 도구들을 포함한다.

jdk는 어른들의 사정으로... 라이센스가 복잡하다고 하는데, 오라클이 재산권을 가지고 있는 Oracle JDK와 오픈소스인 openjdk가 있다.

## 기본 문법

```java
/* 클래스 블록 */
public class 클래스명 { // public한 클래스이름은 파일이름과 같아야함. 클래스명.java

    /* 메소드 블록 */
    [public|private|protected] [static] (리턴자료형|void) 메소드명1(입력자료형 매개변수, ...) {
        명령문(statement);
        ...
    }

    /* 메소드 블록 */
    [public|private|protected] [static] (리턴자료형|void) 메소드명2(입력자료형 매개변수, ...) {
        명령문(statement);
        ...
    }

    ...
}
```

### 접근제어자

public > protected > default > private 순서로 많은 접근을 허용한다.

#### public

public 접근제어자가 붙은 변수, 메소드는 어떤 클래스에서라도 접근 가능하다.

#### protected

protected 접근 제어자가 붙은 변수, 메소드는 ___동일 패키지의 클래스___ 또는 ___해당 클래스를 상속받은___ 다른 패키지의 클래스에서만 접근이 가능하다.

```java
// house/HousePark.java
package house;  // 패키지가 서로 다르다.

public class HousePark {
    protected String lastname = "park";
}
```
```java
// house/person/EngYongPark.java
package house.person;  // 패키지가 서로 다르다.

import house.HousePark;

public class EungYongPark extends HousePark {  // HousePark을 상속했다.
    public static void main(String[] args) {
        EungYongPark eyp = new EungYongPark();
        System.out.println(eyp.lastname);  // 상속한 클래스의 protected 변수는 접근이 가능하다.
    }
}
```

#### default

별도로 접근제어자를 설정하지 않는 변수나 메소드는 해당 패키지 내부에서만 접근 가능하다.

```java
// house/HouseKim.java
package house;  // 패키지가 동일하다.

public class HouseKim {
    String lastname = "kim";  // lastname은 default 접근제어자로 설정된다.
}
```

```java
// house/HousePark.java
package house;  // 패키지가 동일하다.

public class HousePark {
    String lastname = "park";

    public static void main(String[] args) {
        HouseKim kim = new HouseKim();
        System.out.println(kim.lastname);  // HouseKim 클래스의 lastname 변수를 사용할 수 있다.
    }
}
```

#### private

해당 클래스 내부에서만 접근이 가능하다. 
```java
public class Sample {
    private String secret;
    private String getSecret() {
        return this.secret;
    }
}
```

### static

static 키워드를 사용하면 자바는 메모리 할당을 단 한번만 하게되어서 메모리 사용에 이점이 있다. 값이 변하지 않을 변수라면 static 키워드를 사용하면 된다.

> final과의 차이는? <br >
> final 을 찾아보며 알게된 건데, static 은 사실 값이 변하지 않는것과는 크게 관련 없는 듯. 그냥 클래스에서 접근할 수 있는 변수가 되고 메모리 할당이 한번만 일어난다? 바로 밑 예제만 봐도 ++로 값은 바뀔 수 있음. final 은 최종적으로 값을 한번만 할당할 수 있다는 의미. [여기](https://gobae.tistory.com/3) 설명 잘 해둠

그리고 static 변수는 같은 메모리 주소를 바라보기 때문에 변수의 값이 공유된다. 

```java
class Counter  {
    int count = 0; // 독립적인 값을 갖는 객체변수임
    Counter() {
        this.count++;
        System.out.println(this.count);
    }
}

class StaticCounter {
    static int staticCount = 0;
    StaticCounter() {
        staticCount++; // 객체변수가 아니라서 this 를 안쓰는게 좋다.
        System.out.println(staticCount);
    }
}

public class Sample {
    public static void main(String[] args) {
        Counter c1 = new Counter(); // 1
        Counter c2 = new Counter(); // 1
        StaticCounter c3 = new StaticCounter(); // 1
        StaticCounter c4 = new StaticCounter(); // 2
    }
}
```

메소드 앞에 static 키워드를 사용할 경우 객체 생성 없이 클래스를 통해 직접 호출할 수 있다. 다만 이런 스태틱 메소드의 경우 객체 변수에 접근할 수 없다.

```java
class Counter  {
    static int count = 0;
    Counter() {
        count++;
        System.out.println(count);
    }

    public static int getCount() {
        return count;
    }
}

public class Sample {
    public static void main(String[] args) {
        Counter c1 = new Counter();
        Counter c2 = new Counter();

        System.out.println(Counter.getCount());  // 스태틱 메서드는 클래스를 이용하여 호출
    }
}
```