# OCP (Open Closed Principle)

## 개방 폐쇄 원칙이란?

- 자신의 확장에는 열려 있지만 변경에는 닫혀 있도록 설계하는 원칙이다.
- 확장과 폐쇄는 무엇일까?
    - 확장
        - 기존의 요구사항을 유지하면서 추가적인 요구사항 로직을 구현할 수 있다.
    - 폐쇄
        - 확장이 발생 했을 때, 상위 레벨에 변경사항에 대한 영향이 미치지 않는 것을 의미한다.

## 요구사항

- 현재 PG사는 하나만 지원한다.
- 카카오페이 결제를 추가할 예정이며 이후 다른 PG사도 추가 가능하다.

## 기존 결제 시스템

![Untitled](https://s3.us-west-2.amazonaws.com/secure.notion-static.com/a02fdb69-8d91-4f4c-a869-12a5de404f0f/Untitled.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=AKIAT73L2G45EIPT3X45%2F20220130%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20220130T114810Z&X-Amz-Expires=86400&X-Amz-Signature=7e5fe382f6bc5248c217c101b0191ec04e12114469995d77ac466c30f0771b4a&X-Amz-SignedHeaders=host&response-content-disposition=filename%20%3D%22Untitled.png%22&x-id=GetObject)

```java
// PaymentController
@Slf4j
@RestController
@RequestMapping(value = "/ocp")
@RequiredArgsConstructor
public class PaymentController {

    private final PaymentService paymentService;

    @GetMapping("/legacy")
    public ResponseEntity<String> legacy(@RequestBody PaymentResponse paymentResponse) {
        return ResponseEntity.ok(paymentService.payment(paymentResponse));
    }
}

// PaymentService
@Service
public class PaymentService {

    public String payment(PaymentResponse paymentResponse) {
        return String.format("%s 님, %s 원 결제 완료되었습니다.", paymentResponse.getBuyerName(), paymentResponse.getTotalPrice());
    }
}
```

- 현재 `PaymentController` 는 `PaymetService` 에 강한 의존성을 가지고 있다.
- 만약 카카오페이 결제 서비스를 추가하려고 한다면 컨트롤러 레이어나 서비스 레이어에서 결제 수단에 따른 분기처리를 하는 코드를 추가해줘야 한다.
    - Case 1
        - 카카오페이 결제 전용 핸들러 메소드 추가 구현
        - 서비스에서 카카오페이 결제 메소드 추가 구현
    - Case 2
        - 카카오페이 결제 메소드 추가 구현
        - 컨트롤러에서 결제 수단에 따른 결제 메소드 분기 처리

## 카카오페이 결제 서비스 추가한다면?

Case 2 상황을 구현한 간단한 예시입니다.

![Untitled](https://s3.us-west-2.amazonaws.com/secure.notion-static.com/a7942654-751c-419a-9cfe-98e2777dda5d/Untitled.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=AKIAT73L2G45EIPT3X45%2F20220130%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20220130T114826Z&X-Amz-Expires=86400&X-Amz-Signature=609f93be09a89d3e4c0bec54e58c34de0472dc8880c5a2ea1f75379be87a3b60&X-Amz-SignedHeaders=host&response-content-disposition=filename%20%3D%22Untitled.png%22&x-id=GetObject)

```java
// PaymentController
@Slf4j
@RestController
@RequestMapping(value = "/ocp")
@RequiredArgsConstructor
public class PaymentController {

    private final PaymentService paymentService;

    @GetMapping("/legacy")
    public ResponseEntity<String> legacy(@RequestBody PaymentResponse paymentResponse) {
        switch (paymentResponse.getPaymentType()) {
            case DEFAULT:
                return ResponseEntity.ok(paymentService.payment(paymentResponse));
            case KAKAO_PAY:
                return ResponseEntity.ok(paymentService.kakaoPayment(paymentResponse));
            default:
                return ResponseEntity.status(HttpStatus.NOT_ACCEPTABLE).body(paymentResponse.getPaymentType() + "은(는) 지원하지 않는 결제수단입니다.");
        }
    }
}

// PaymentService
@Service
public class PaymentService {

    public String payment(PaymentResponse paymentResponse) {
        return String.format("%s 님, %s 원 결제 완료되었습니다.", paymentResponse.getBuyerName(), paymentResponse.getTotalPrice());
    }

    public String kakaoPayment(PaymentResponse paymentResponse) {
        return String.format("%s 님, %s 로 %s 원 결제 완료되었습니다.", paymentResponse.getBuyerName(), paymentResponse.getPaymentType(), paymentResponse.getTotalPrice());
    }
}
```

카카오페이 결제 수단을 위의 코드처럼 추가할 경우, `PaymentController` 에서 `PaymentService` 의 결제 메소드를 호출하기 위해서 인자로 받은 `PaymentResponse` 의 결제 수단에 따라 분기처리를 수행해야 한다.
이는 결제 수단이 추가될 때 마다 if문을 통해서 어떤 메소드를 호출할지 컨트롤러 레이어에서 추가적인 수정이 필요하게 만든다.

즉, 이는 하위 객체에서 변경이 일어날 때 마다 상위 객체에서 하위 객체의 변경에 큰 영향을 미치게 된다. 때문에 유지보수가 어려줘진다는 단점이 있고, 현재는 `PaymentService` 하나만 있지만 여러 객체를 의존하게 되는 상황이 생기면 변경에 대한 의존 객체를 계속 만들어줘야 한다.

## OCP 를 준수한 결제 시스템

Proxy 객체를 이용해 변경에는 닫혀있고, 확장에는 열려있도록 구현해보려고 한다.

![Untitled](https://s3.us-west-2.amazonaws.com/secure.notion-static.com/e37589a6-f20d-4ddf-8d1f-ee78b3064d34/Untitled.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=AKIAT73L2G45EIPT3X45%2F20220130%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20220130T114837Z&X-Amz-Expires=86400&X-Amz-Signature=6d9a66a7a806605d1023c94e63b257fbfab480acf3fb9d1bcb5f2209672b4df1&X-Amz-SignedHeaders=host&response-content-disposition=filename%20%3D%22Untitled.png%22&x-id=GetObject)

```java
// PaymentController
@Slf4j
@RestController
@RequestMapping(value = "/ocp")
@RequiredArgsConstructor
public class PaymentController {

    private final LegacyPaymentService legacyPaymentService;
    private final PaymentService paymentService;

    @GetMapping("/refactor")
    public ResponseEntity<String> refactor(@RequestBody PaymentResponse paymentResponse) {
        return ResponseEntity.ok(paymentService.payment(paymentResponse));
    }
}

// PaymentService
public interface PaymentService {
    String payment(PaymentResponse paymentResponse);
}

// PaymentProxy
@Primary
@Component
@RequiredArgsConstructor
public class PaymentProxy implements PaymentService {

    private final DefaultPaymentService defaultPaymentService;
    private final KakaoPayPaymentService kakaoPayPaymentService;

    public String payment(PaymentResponse paymentResponse) {
        switch (paymentResponse.getPaymentType()) {
            case DEFAULT:
                return defaultPaymentService.payment(paymentResponse);
            case KAKAO_PAY:
                return kakaoPayPaymentService.payment(paymentResponse);
            default:
                return paymentResponse.getPaymentType() + "은(는) 지원하지 않는 결제수단입니다.";
        }
    }
}

// DefaultPaymentService
@Service
public class DefaultPaymentService implements PaymentService {

    public String payment(PaymentResponse paymentResponse) {
        return String.format("%s 님, %s 원 결제 완료되었습니다.", paymentResponse.getBuyerName(), paymentResponse.getTotalPrice());
    }
}

// KakaoPaymentService
@Service
public class KakaoPayPaymentService implements PaymentService {

    public String payment(PaymentResponse paymentResponse) {
        return String.format("%s 님, %s 로 %s 원 결제 완료되었습니다.", paymentResponse.getBuyerName(), paymentResponse.getPaymentType(), paymentResponse.getTotalPrice());
    }
}
```

위의 코드를 확인해보면 `KakaoPaymentService` 가 추가되었지만 상위 객체인 `PaymentController` 에서는 결제 수단 추가에 대한 영향은 받지 않는다는 것을 알 수 있다.

이를 통해 의존 관계를 인터페이스와 프록시 객체로 역전시켜 확장에는 유연하지만 변경에는 닫혀있도록 구현할 수 있다. 때문에 이러한 OCP 원칙은 클래스 간 강한 결합 관계를 느슨한 관계로 만들 수 있다.