## 1. Factory method Pattern?

Factory method Pattern은 **객체 생성을 코드에서 분리**하여, 객체를 직접 생성하지 않도록 한다. 객체 생성을 위한 메서드를 부모 클래스에서 정의하고, 이를 하위 클래스에서 구현하도록 하는 패턴**이다.  
즉, **객체 생성의 책임을 하위 클래스에 위임하여 유연성을 높이는 것이 핵심**이다.


## 2. 왜 팩토리 메서드 패턴을 사용할까?

예를 들어, 다양한 알림을 만든다고 가정해보자  
**팩토리 메서드 패턴을 사용하지 않으면** 다음과 같은 문제가 생긴다.
새로운 타입의 알림이 추가되거나 기존에 있던 알림이 변경이 된다고 하면 객체를 생성하는 모든 곳에서의 코드 수정이 불가피하다.


### 팩토리 메서드 패턴을 사용하면…
알림을 생성하는 method를 갖는 인터페이스 또는 추상클래스를 만든 후 각각의 알림 타입에 따른 Factory 클래스를 구현하여 객체 생성을 Factory 클래스에 일임하면 알림이 변경되거나, 새로운 타입의 알림이 추가되도 Factory 클래스만 수정하거나 추가를 하면 된다.


## 3. 코드 예제

### 1) 팩토리 메서드 패턴을 사용하지 않은 경우

```java
// 알림의 공통 interface
interface Notification {
    void sendNotification(String message);
}

// Email
class EmailNotification implements Notification {
    @Override
    public void sendNotification(String message) {
        System.out.println("이메일 알림: " + message);
    }
}

// SMS
class SMSNotification implements Notification {
    @Override
    public void sendNotification(String message) {
        System.out.println("문자 알림: " + message);
    }
}

// Push 클래스
class PushNotification implements Notification {
    @Override
    public void sendNotification(String message) {
        System.out.println("푸시 알림: " + message);
    }
}

public class Main {
    public static void main(String[] args) {
       Notification emailNotification = new EmailNotification();
       Notification smsNotification = new SMSNotification();
       Notification pushNotification = new PushNotification();

       emailNotification.sendNotification("Hello");
       smsNotification.sendNotification("Hello");
       pushNotification.sendNotification("Hello");
    }
}
```

문제점  

기존의 Notification 인터페이스를 구현한 클래스에 변경이 되면 main 함수의 코드가 수정되어야 한다.



1) 팩토리 메서드 패턴을 사용한 경우

```java
// 알림의 공통 interface
interface Notification {
    void sendNotification(String message);
}

// Email
class EmailNotification implements Notification {
    @Override
    public void sendNotification(String message) {
        System.out.println("이메일 알림: " + message);
    }
}

// SMS
class SMSNotification implements Notification {
    @Override
    public void sendNotification(String message) {
        System.out.println("문자 알림: " + message);
    }
}

// Push 클래스
class PushNotification implements Notification {
    @Override
    public void sendNotification(String message) {
        System.out.println("푸시 알림: " + message);
    }
}

// NotificationFactory 인터페이스 (Creator 역할)
interface NotificationFactory {
    Notification createNotification(); // 공통의 interface Notification을 반환한다.
}

// 각각의 타입별로 Factory class를 구현한다.

// EmailNotification을 반환하는 factory 클래스
class EmailNotificationFactory implements NotificationFactory {
    @Override
    public Notification createNotification() {
        return new EmailNotification();
    }
}

// SMSNotification을 반환하는 factory 클래스
class SMSNotificationFactory implements NotificationFactory {
    @Override
    public Notification createNotification() {
        return new SMSNotification();
    }
}

// PushNotification을 반환하는 factory 클래스
class PushNotificationFactory implements NotificationFactory {
    @Override
    public Notification createNotification() {
        return new PushNotification();
    }
}

public class Main {
    public static void main(String[] args) {
        NotificationFactory emailNotificationFactory = new EmailNotificationFactory();
        Notification emailNotification = emailNotificationFactory.createNotification();
        emailNotification.sendNotification("Hello");
    }
}
```
### 느낀점  
팩토리 메서드를 사용하지 않는다면 만약에 EmailNotification 이 수정된다고 하면 main함수나, 또다른 곳에 직접 객체를 생성한 곳이 있다면 수정해야되는 부분이 많을 거라는게 인식이 된다.  

### 궁금한 점 
새로운 Notification이 추가가 된다면 추가된 Notification을 위한 Factory 클래스를 구현하여야 한다.


## 내가 생각하는 팩토리 메서드 패턴의 장점
1. 생성 로직 분리  
2. Factory class 에서 생성에 대한 책임을 지기 때문에 main 코드는 객체 생성 방법을 몰라도 된다.
