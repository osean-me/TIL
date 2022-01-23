# SRP (Single Responsibility Principle)

## 단일 책임 원칙이란?

- 클래스는 하나의 역할만을 책임져야 한다는 원칙이다.
  - 하나의 클래스가 여러 개의 책임을 가지는 것은 결합도가 높다는 의미를 가지는데, 이는 해당 클래스를 여러 역할로 의존하는 클래스들이 많아 의존하는 클래스의 요구가 변경할 때마다 변경해줘야 해 유지보수가 어려워진다.
- 클래스를 변경하는 이유는 단 한 개여야 한다.
- 클래스를 변경하게 하는 주체가 누구인가?

단일 책임 원칙에 대해서 이해하기 위해서 카드사 별 결제 시스템을 다음의 요구사항을 바탕으로 구현해보고 공부하는 것이 이번 시간의 목표이다.

*(참고 - 스프링 입문을 위한 자바 객체 지향의 원리와 이해, [cheese10yun 님의 레퍼지토리](https://github.com/cheese10yun/spring-SOLID/blob/master/docs/SRP.md))*

## 요구사항

- 기존 카드 결제 시스템은 국내 결제만 지원한다.
- 현재 등록된 카드사 가맹점은 비씨카드와 하나카드가 존재한다.
  - 카드사는 지속적으로 추가될 예정이다.
- 비씨카드사에서 해외 결제 서비스를 추가하려고 한다.
  - 단, 하나카드사는 해외 결제 서비스를 지원하지 않는다.
  - 해외 결제 가능 여부와 관계없이 카드사는 추가 가능하다.

## 기존 카드 결제 시스템

![old-local-payment](https://s3.us-west-2.amazonaws.com/secure.notion-static.com/527ec1e2-780d-47e7-96e9-42666c0a20d8/Untitled.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=AKIAT73L2G45EIPT3X45%2F20220123%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20220123T143158Z&X-Amz-Expires=86400&X-Amz-Signature=6dc2df126c4a2b6dff00767ae8fe5dcc8721419f6836508cf7d4aece8b1ce0cd&X-Amz-SignedHeaders=host&response-content-disposition=filename%20%3D%22Untitled.png%22&x-id=GetObject)

```java
// PaymentService
public interface PaymentService {
    String payByLocal();
}

// BC카드사의 PaymentService 구현 클레스
@Service
public class BcPaymentService implements PaymentService {
    @Override
    public String payByLocal() {
				...
        return "BC카드 :: 국내 가맹점 결제 승인";
    }
}

// 하나카드사의 PaymentService 구현 클래스
@Service
public class HanaPaymentService implements PaymentService {
    @Override
    public String payByLocal() {
				...
        return "하나카드 :: 국내 가맹점 결제 승인";
    }
}
```

- 결제를 처리하는 클래스를 `PaymentService` 인터페이스로 추상화하고 각 카드사의 `PaymentService` 에서 `payByLocal()` 메소드를 구현하고 있다.

## PaymentService 에 해외 결제 메소드를 추가한다면?

![old-local-and-global-payment](https://s3.us-west-2.amazonaws.com/secure.notion-static.com/46c5b702-66c2-4b0f-93d8-b153cc03d80c/Untitled.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=AKIAT73L2G45EIPT3X45%2F20220123%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20220123T143307Z&X-Amz-Expires=86400&X-Amz-Signature=890f99cedcb0f68dafba22312bc6275ce15f266915fb99a1466459e10ef73e05&X-Amz-SignedHeaders=host&response-content-disposition=filename%20%3D%22Untitled.png%22&x-id=GetObject)

```java
// 해외 결제 메소드를 추가한 PaymentService
public interface PaymentService {
    String payByLocal();

    String payByGlobal();
}

// BC카드사의 PaymentService 구현 클레스
@Service
public class OldBcPaymentService implements OldPaymentService {
    @Override
    public String payByLocal() {
				...
        return "BC카드 :: 국내 가맹점 결제 승인";
    }

    @Override
    public String payByGlobal() {
				...
        return "BC카드 :: 해외 가맹점 결제 승인";
    }
}

// 하나카드사의 PaymentService 구현 클래스
@Service
public class OldHanaPaymentService implements OldPaymentService {
    @Override
    public String payByLocal() {
				...
        return "하나카드 :: 국내 가맹점 결제 승인";
    }

    @Override
    public String payByGlobal() {
        // 하나카드사는 해외 결제를 지원하지 않는다.
				// 과연 하나카드사의 PaymentService 구현체는 해외 결제 메소드를 구현해야할까?
				...?
        return "하나카드 :: 해외 가맹점은 결제 서비스를 지원하지 않습니다.";
    }
}
```

- `PaymentService` 인터페이스에 해외 결제 메소드를 추가하게 되면 위의 코드처럼 각 카드사 별 `PaymentService` 구현체에서 해당 메소드를 구현해줘야 한다.
- 하지만 요구사항을 보면 하나카드사는 해외 결제 서비스를 지원하지 않는데, `PaymentService` 를 구현하고 있다고 해서 꼭 해외 결제 메소드를 구현해야할까?
- 만약 빈 메소드로 구현하고, 행위자가 해외 결제를 시도한다면 결제 처리 로직 중 하나카드사일 경우를 대비한 분기처리 코드를 작성해야 한다.

## 단일 책임 원칙을 준수해야 하는 이유

위의 예시처럼 하나의 클래스에서 여러 역할을 책임져야 하는 경우, 구현하는 클래스에서 필요하지 않은 역할까지 구현해줘야 한다는 번거로움이 존재한다.

또한, 행위자는 하나의 메소드에 의존하고 있는데, 의존하고 있는 메소드가 구현체에 따라 수행 로직이 달라지고 담당하는 구현체가 해당 메소드를 구현하고 있냐 없느냐에 따라 분기처리를 해줘야 하는데 이 경우의 수가 많으면 많을 수록 코드가 복잡해진다.

즉, 단일 책임 원칙은 하나의 행위자에 대한 하나의 책임을 의미하며, 위의 예시처럼 국내 결제, 해외 결제 같이 행위자가 의존하는 역할이 나눠져 있을 경우 각 역할을 책임지는 클래스를 나눠줘야 한다.

(행위자가 의존하는 역할이 나뉘는 경우는 두 개의 행위자로 나뉘어졌다고 생각할 수 있다.)

## SRP 를 준수한 결제 시스템

![srp-payment](https://s3.us-west-2.amazonaws.com/secure.notion-static.com/2f987fe9-5458-46a0-bb2a-cb5ae37b5803/Untitled.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=AKIAT73L2G45EIPT3X45%2F20220123%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20220123T143353Z&X-Amz-Expires=86400&X-Amz-Signature=3f43dc32596cb44dbb7cbd64b7511a4d5f7e10ba43369d14d2ef644fcedc3f62&X-Amz-SignedHeaders=host&response-content-disposition=filename%20%3D%22Untitled.png%22&x-id=GetObject)

```java
// 국내 결제만 책임지는 PaymentService
public interface LocalPaymentService {
    String payByLocal();
}

// 해외 결제만 책임지는 PaymentService
public interface GlobalPaymentService {
    String payByGlobal();
}

// 비씨카드사의 국내 결제 PaymentService
@Service
public class BcLocalPaymentService implements LocalPaymentService {
    @Override
    public String payByLocal() {
        return "BC카드 :: 국내 가맹점 결제 승인 (SRP)";
    }
}

// 비씨카드사의 해외 결제 PaymentService
@Service
public class BcGlobalPaymentService implements GlobalPaymentService {
    @Override
    public String payByGlobal() {
        return "BC카드 :: 해외 가맹점 결제 승인 (SRP)";
    }
}

// 하나카드사의 결제 PaymentService
@Service
public class HanaPaymentService implements LocalPaymentService {
    @Override
    public String payByLocal() {
        return "하나카드 :: 국내 가맹점 결제 승인 (SRP)";
    }
}
```