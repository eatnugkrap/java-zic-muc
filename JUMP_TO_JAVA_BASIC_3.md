### 점프 투 자바 ] - 기본 3. 그외 + OOP

- 자바에는 call by reference가 없다. 문제는 그... address를 복사해서 call by value 처럼 쓰기 떄문에 오해할만한 케이스가 발생함. [이분](https://deveric.tistory.com/92)이 정리를 굉장히 잘 해두셨다.
- 성격이 비슷한 자바 클래스를 모아둔걸 패키지라고 한다. 그냥 디렉토리임
- 예외처리. 자바에서는 catch에 예상되는 에러의 종류를 알려주고 해당하는 에러가 발생할때만 처리가 가능한 것 같다. 아마도? 모든 예외가 Exception을 extends 하는 것 같으니.... 그걸로 잡으면 다 잡힐지도?

#### 쓰레드

자바는 동작하고 있는 프로세스 내부에서 병렬처리를 위한 쓰레드 관리를 지원한다.

Thread 클래스를 상속받은 클래스가 start 메소드를 호출하면 run 메소드가 별개의 쓰레드로 다른 쓰레드들과 병렬적으로 처리되는 듯 하다.

```java
public class Sample extends Thread {
    int seq;

    public Sample(int seq) {
        this.seq = seq;
    }

    public void run() {
        System.out.println(this.seq + " thread start.");  // 쓰레드 시작
        try {
            Thread.sleep(1000);  // 1초 대기한다.
        } catch (Exception e) {
        }
        System.out.println(this.seq + " thread end.");  // 쓰레드 종료
    }

    public static void main(String[] args) {
        for (int i = 0; i < 10; i++) {  // 총 10개의 쓰레드를 생성하여 실행한다.
            Thread t = new Sample(i);
            t.start();
        }
        System.out.println("main end.");  // main 메소드 종료
    }
}
/// 결과

// 0 thread start.
// 4 thread start.
// 6 thread start.
// 2 thread start.
// main end.
// 3 thread start.
// 7 thread start.
// 8 thread start.
// 1 thread start.
// 9 thread start.
// 5 thread start.
// 0 thread end.
// 4 thread end.
// 2 thread end.
// 6 thread end.
// 7 thread end.
// 3 thread end.
// 8 thread end.
// 9 thread end.
// 1 thread end.
// 5 thread end.
```

위 예시에서는 메인메소드가 각 쓰레드의 작업이 끝나기 전에 메인 메소드가 먼저 끝나버린다. 이런 일을 막기 위해 Thread 클래스의 join 메소드를 사용할 수 있다. join 메소드는 해당 쓰레드의 작업이 끝나기까지 기다려준다.

```java
import java.util.ArrayList;

public class Sample extends Thread {
    int seq;
    public Sample(int seq) {
        this.seq = seq;
    }

    public void run() {
        System.out.println(this.seq+" thread start.");
        try {
            Thread.sleep(1000);
        }catch(Exception e) {
        }
        System.out.println(this.seq+" thread end.");
    }

    public static void main(String[] args) {
        ArrayList<Thread> threads = new ArrayList<>();
        for(int i=0; i<10; i++) {
            Thread t = new Sample(i);
            t.start();
            threads.add(t);
        }

        for(int i=0; i<threads.size(); i++) {
            Thread t = threads.get(i);
            try {
                t.join(); // t 쓰레드가 종료할 때까지 기다린다.
            }catch(Exception e) {
            }
        }
        System.out.println("main end.");
    }
}

///

// 0 thread start.
// 5 thread start.
// 2 thread start.
// 6 thread start.
// 9 thread start.
// 1 thread start.
// 7 thread start.
// 3 thread start.
// 8 thread start.
// 4 thread start.
// 0 thread end.
// 5 thread end.
// 2 thread end.
// 9 thread end.
// 6 thread end.
// 1 thread end.
// 7 thread end.
// 4 thread end.
// 8 thread end.
// 3 thread end.
// main end.
```

그리고 자바에서는 다중상속을 지원하지 않기 때문에 클래스가 Thread 를 상속받아 버리면 다른 클래스를 상속받지 못한다. 그렇기 때문에 Runnable 인터페이스를 구현한 클래스를 Thread 클래스 생성자에 인자로 넘겨주는 형태로 구현하기도 한다.

```java
import java.util.ArrayList;

public class Sample implements Runnable {
    int seq;
    public Sample(int seq) {
        this.seq = seq;
    }

    public void run() {
        System.out.println(this.seq+" thread start.");
        try {
            Thread.sleep(1000);
        }catch(Exception e) {
        }
        System.out.println(this.seq+" thread end.");
    }

    public static void main(String[] args) {
        ArrayList<Thread> threads = new ArrayList<>();
        for(int i=0; i<10; i++) {
            Thread t = new Thread(new Sample(i)); // Runnalbe 구현체를 인자로 넘겨줌
            t.start();
            threads.add(t);
        }

        for(int i=0; i<threads.size(); i++) {
            Thread t = threads.get(i);
            try {
                t.join();
            }catch(Exception e) {
            }
        }
        System.out.println("main end.");
    }
}
```


#### 함수형 프로그래밍

람다와 스트림을 이용한 함수형 프로그래밍이 가능하다는데 예제들이 너무 어질어질하다. 자바에는 콜백이 없나? 근데 이건 강의 듣는데 방해가 될 것 같지는 않다. 자바 찍먹은 일단 여기까지 하고 강의들으러가야지 야호~