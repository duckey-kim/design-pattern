## 1. Abstract Factory Pattern?

추상 팩토리 패턴은 관련된 객체들의 집합을 생성하기 위한 인터페이스를 제공하는 디자인 패턴입니다. 팩토리 메서드를 조합해서 어떤 연관성이 있는 객체들을 생성하는 팩토리를 만든다고 생각한다.

## 2. 왜 추상 팩토리 패턴을 사용할까?
관련된 객체들을 일관되게 생성: 특정 그룹의 객체들을 함께 생성할 때 유용합니다.

### 추상 팩토리 패턴을 사용하면
관련된 객체들을 생성하는 Abstract Factory를 선언한우 관련성에 따라서 객체들을 반환하는 팩토리를 구현한다.
관련된 객체들을 한번에 생성할 수 있어서 유용하고 객체들 생성을 한 곳에서 하기때문에 유지및 확장에 유리하다.

## 3. 코드 예제
```java
interface Notification {
    void send(String message);
}

// Email
class EmailNotification implements Notification {
    @Override
    public void send(String message) {
        System.out.println("이메일 알림: " + message);
    }
}

// SMS
class SMSNotification implements Notification {
    @Override
    public void send(String message) {
        System.out.println("문자 알림: " + message);
    }
}



// Sender 인터페이스 
interface Sender {
    void validate();
}

// 이메일 발송자
class EmailSender implements Sender {
    @Override
    public void validate() {
        System.out.println("이메일 발송자 유효성 검증");
    }
}

// SMS 발송자
class SMSSender implements Sender {
    @Override
    public void validate() {
        System.out.println("SMS 발송자 유효성 검증");
    }
}

// 추상 팩토리 (AbstractFactory)
interface NotificationFactory {
    Notification createNotification();
    Sender createSender();
}

// 이메일 관련 객체 생성 팩토리 (ConcreteFactory1)
class EmailFactory implements NotificationFactory {
    @Override
    public Notification createNotification() {
        return new EmailNotification();
    }

    @Override
    public Sender createSender() {
        return new EmailSender();
    }
}

// SMS 관련 객체 생성 팩토리 (ConcreteFactory2)
class SMSFactory implements NotificationFactory {
    @Override
    public Notification createNotification() {
        return new SMSNotification();
    }

    @Override
    public Sender createSender() {
        return new SMSSender();
    }
}

public class Client {
    public static void main(String[] args) {
        // 이메일 팩토리로 관련 객체 생성
        NotificationFactory emailFactory = new EmailFactory();
        Notification emailNotification = emailFactory.createNotification();
        Sender emailSender = emailFactory.createSender();

        emailSender.validate();
        emailNotification.send("회원가입 완료");

        // SMS 팩토리로 관련 객체 생성
        NotificationFactory smsFactory = new SMSFactory();
        Notification smsNotification = smsFactory.createNotification();
        Sender smsSender = smsFactory.createSender();

        smsSender.validate();
        smsNotification.send("결제 요청");
    }
}
```

### 느낀점  
객체들을 생성하는 팩토리를 추상화 해놓았다. 그래서 연관성 있는 객체들을 생성할 수 있는 팩토리들을 구현하여 객체들을 생성한다.

### 궁금한 점 
현재코드는 추상 팩토리를 구현할 때 객체들을 생성할 때 new 를 사용하여 객체를 생성하였는데 이걸 factory 메서드를 통해서 하면 좀더 객체 생성에 대한 유지보수가 좋아질거 같은데.... 이러면 코드는 좀더 많아질거 같다.

## 내가 생각하는 팩토리 메서드 패턴의 장점
생성 로직 분리  
관련성 있는 객체들을 생성할 수 있다.