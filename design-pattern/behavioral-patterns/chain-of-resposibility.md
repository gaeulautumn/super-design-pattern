## 책임 연쇄(CHAIN OF RESPONSIBILITY)
- 메세지를 보내는 객체와 이를 받아 처리하는 객체들 간 결합도를 없애기 위한 패턴

## 예시
![image](https://user-images.githubusercontent.com/21086676/103279927-69cf6b80-4a12-11eb-9829-72a894c6fecd.png)
- 프린트 창이 뜨고 `정말 프린트하시겠습니까?` 확인버튼을 누르는 상황
    - 확인 버튼을 눌른 상황에서는 당장 프린트를 할 수 없다.
    - 확인 버튼 > dialog에 확인이벤트 전달 > 다이얼로그 확인 결과를 응용프로그램에 전달 > 응용프로그램에서 프린트 출력
    - 프린트 객체는 응용프로그램이 알고 있을 것이기 때문에, 확인 버튼 입장에선 프린트 객체에 접근할 수 없다.

![image](https://user-images.githubusercontent.com/21086676/103280221-32ad8a00-4a13-11eb-909c-661c7e4d5ff7.png)
- 이 패턴의 아이디어는 메세지 송신 측과 수신 측을 분리하는 것
    - 요청은 실제 이 요청이 처리될 때까지 전달
    - 객체가 요청을 받으면 자신이 처리하거나 다음 후보에 전달.

## 구성
![image](https://user-images.githubusercontent.com/21086676/103280317-730d0800-4a13-11eb-9907-3eafd1cefa01.png)
- 연결고리의 모든 객체들은 동일 인터페이스를 가진다 `HelpHandler`
- 자신이 제공할 도움말이 없다면, 다른 객체의 인터페이스를 호출하도록 구현한다.

## 장단점
- 객체간 결합도 적어짐
    - 다른 객체가 어떻게 요청을 처리하는 지 몰라도 된다.
    - 객체 입장에선 들어온 메세지를 처리할 수 있으면 처리하면 되고, 처리할 수 없다면 다른 객체로 전달만 해주면 되므로 객체간의 상호의존도가 사라진다.
- 요청이 처리된다는 보장이 없음
    - 실제 요청을 처리할 객체에서 메세지의 요청이 누락될 경우, 예상치 못한 이슈가 생길 수 있음.

## 예제코드(짧게)
```java
abstract class Logger {
    protected int mask;
    protected Logger next;

    public Logger setNext(Logger log) {
        next = log;
        return log;
    }

    public void message(String msg, int priority) {
        if (priority <= mask) {
            writeMessage(msg);
        }
        if (next != null) {
            next.message(msg, priority);
        }
    }

    abstract protected void writeMessage(String msg);
}

class StdoutLogger extends Logger {
    public StdoutLogger(int mask) {
        this.mask = mask;
    }

    protected void writeMessage(String msg) {
        System.out.println("Writing to stdout: " + msg);
    }
}


class EmailLogger extends Logger {
    public EmailLogger(int mask) {
        this.mask = mask;
    }

    protected void writeMessage(String msg) {
        System.out.println("Sending via email: " + msg);
    }
}

class StderrLogger extends Logger {
    public StderrLogger(int mask) {
        this.mask = mask;
    }

    protected void writeMessage(String msg) {
        System.err.println("Sending to stderr: " + msg);
    }
}

public class ChainOfResponsibilityExample {
    public static void main(String[] args) {
        Logger logger, logger1;
        logger1 = logger = new StdoutLogger(Logger.DEBUG);
        logger1 = logger1.setNext(new EmailLogger(Logger.NOTICE));
        logger1 = logger1.setNext(new StderrLogger(Logger.ERR));
       

        // Handled by StdoutLogger
        logger.message("Entering function y.", Logger.DEBUG);

        // Handled by StdoutLogger and EmailLogger
        logger.message("Step1 completed.", Logger.NOTICE);

        // Handled by all three loggers
        logger.message("An error has occurred.", Logger.ERR);
    }
}
```

