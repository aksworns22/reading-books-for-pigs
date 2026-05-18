p.67~p.102

### 01 영화 예매 시스템

**요구사항**

본격적인 요구사항에 앞서, 용어를 정리하자.

- ‘영화’는 영화의 기본정보이다. 제목, 상영시간, 가격 정보가 들어가있다.
- ‘상영’은 실제로 관객이 영화를 관람하는 사건이다. 상영 일자, 시간, 순번을 가리키기 위해 사용된다.

사용자가 실제로 예매하는 대상은 영화가 아닌, ‘상영’이다. 표현은 영화를 예매한다 하지만, 실제로는 특정 시간의 특정 영화를 예매하는 것이다.

할인액을 결정하는 두 가지 규칙

1. 할인 조건 (discount condition)
    1. 가격의 할인 여부 결정
    2. ‘순서 조건’과 ‘기간 조건’으로 나뉨
        1. 순서 조건 (sequence condition) : 상영 순번을 이용해 할인 여부 결정 
            
            ex. 순번이 10인 경우, 매일 10번째로 상영되는 영화를 예매한 사람에게 할인제공
            
        2. 기간 조건 (period condition) : 영화 시작시간을 이용해 할인 여부 결정
            
            ex. 요일, 시작시간, 종료시간으로 구성되며 영화 시작시간이 해당기간에 포함되면 할인제공
            
            월요일, 10-13시라면 월요일 10시-13시에 시작하는 모든 영화는 할인 가능
            
    3. 다수의 할인 조건을 함께 지정 가능. 섞기 또한 가능.
2. 할인 정책 (discount policy)
    1. 할인 요금 결정 
    2. 금액 할인 정책(amount discount policy)와 비율 할인 정책 (percent discount policy)
        1. 금액 할인 정책 : 예매 요금에서 일정 금액 할인
        2. 비율 할인 정책 : 정가에서 일정 비율의 요금 할인
    3. 영화별로 하나의 할인 정책만 할당 가능. (지정하지 않는 것도 가능.) 

예시

| 영화 | 할인 정책 | 할인 조건 |
| --- | --- | --- |
| 아바타
가격 : 10000원 | 금액 할인 정책
(800원 할인) | 1. 순번 조건 : 조조 상영
2. 순번 조건 : 10회 상영
3. 기간 조건 : 월 10~12시 사이
4. 기간 조건 : 목 18~21시 사이 |
| 타이타닉
가격 : 11000원 | 비율 할인 정책
(10% 할인) | 1. 기간 조건 : 화 14~17시 사이
2. 순번 조건 : 2회 상영
3. 기간 조건 : 목 10~14시 사이 |
| 스타워즈
가격 : 10000원 | 없음 | 없음 |

할인 적용 프로세스

1. 사용자의 예매 정보가 할인 조건 중 하나라도 만족 하는지 확인
2. 할인 조건 만족 → 할인 정책에 맞는 할인 요금 계산
3. 불만족 & 조건 X → 요금 할인 X

위 티켓은 1인기준이다. 2인이면 2인만큼 할인.

예매 완료 하면 예매 정보를 생성한다. 제목, 상영 정보, 인원, 정가, 결제금액이 나온다.

### 02 객체지향 프로그래밍을 향해

**협력, 객체, 클래스**

우리는 보통 어떤 클래스를 만들지 생각하고, 메서드, 속성을 생각한다. 하지만 객체 지향은, 클래스가 아닌 객체 자체에 초점을 둬야한다. 이를 위해 2가지에 집중해야한다.

1. 어떤 클래스가 필요한지 전에, 어떤 객체들이 필요한지 고민.
    1. 클래스는 공통적인 상태와 행동을 공유하는 객체를 추상화 한것. → 어떤 객체들이 어떤 상태와 행동을 가지는지 먼저 생각해야한다.
2. 객체를 독립적 존재가 아니라 기능을 구현하기 위해 협력하는 공동체의 일원으로 봐야함.
    1. 객체를 협력하는 공동체의 일원으로 보면 설계가 유연해지고 확장가능하게 한다.
    2. 객체의 윤곽이 잡히면 공통된 특성과 상태를 가진 객체를 타입으로 분류, 이 타입을 클래스로 구현

**도메인의 구조를 따르는 프로그램 구조**

도메인(Domain)

: 문제를 해결하기 위해 사용자가 프로그램을 사용하는 분야. 

![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/b0078df0-8e0f-42b1-9c37-cd13fc42e65c/7573d080-96ff-44ad-8181-b6381cb7bad6/image.png)

영화 예매 도메인을 구성하는 개념과 관계를 표현한 것. 영화는 여러 번 상영 될 수있고, 상영은 여러번 예매될 수 있다. 할인 정책은 선택 안하거나 오직 하나만, 할인 조건은 하나 이상이 존재한다.

![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/b0078df0-8e0f-42b1-9c37-cd13fc42e65c/749e571d-8f70-495f-93f2-9b955c58afd4/image.png)

영화→ `Movie`, 상영 → `Screening`, 할인 정책 → `DiscountPolicy` … 등으로 이해하면 된다.

**클래스 구현**

```java
public class Screening {
    private Movie movie;
    private int sequence;
    private LocalDateTime whenScreened;
    
    public Screening(Movie movie, int sequence, LocalDateTime whenScreened) {
        this.movie = movie;
        this.sequence = sequence;
        this.whenScreened = whenScreened;
    }
    
    public LocalDateTime getStartTime() {
        return whenScreened;
    }
    
    public boolean isSequence(int sequence) {
        return this.sequence == sequence;
    }
    
    public Money getMovieFee() {
        return movie.getFee();
    }
}

// '상영' 구현. 이는 영화, 순번, 상영시작시간을 포함하고, isSequence는 순번의 일치여부를 
// 검사하고, getMovieFee는 기본 요금을 반환한다.
```

내부와 외부 어떤 것을 감추고 열어야 할지 고민하는 게 중요하다. 이는 경계의 명확성이 객체의 자율성을 보장하기 때문이다. 또한, 프로그래머에게 구현의 자유를 제공한다.

- **자율적인 객체**

객체에 대해 두 가지 사실을 알아야 한다.

1. 상태(state)와 행동(behavior)을 함께 가진 복합적 존재이다.
2. 객체가 스스로 판단하고 행동하는 자율적인 존재이다.

- 캡슐화 → 데이터와 기능을 객체 내부로 함께 묶는 것
- 접근 제어 (Access Control) → 객체를 자율적으로 만들기 위해서 접근 수정자 (Access Modifier) 외부에서의 접근을 통제하는 메커니즘

캡슐화와 접근 제어는 객체를 두 부분으로 나눈다.

1. 퍼블릭 인터페이스(public interface) : 외부에서 접근 가능한 부분
2. 구현(implementation) : 접근 불가능하고 내부에서만 접근 가능한 부분

**인터페이스와 구현의 분리**는 중요한 원칙이다.

일반적으로는 속성은 `private`, 외부에 제공 해야하는 메서드만 `public` 으로 공개한다.

- **프로그래머의 자유**

프로그래머의 역할 구분

1. 클래스 작성자(class creater)
    1. 새로운 데이터 타입을 프로그램에 추가
    2. 목표 : 클라이언트 프로그래머에게 필요한 부분만 공개하고 나머지는 숨기기 → **구현 은닉**
2. 클라이언트 프로그래머(client programmer)
    1. 클래스 작성자가 추가한 데이터 타입을 사용
    2. 목표 : 필요한 클래스를 엮어 애플리케이션을 안정적으로 구축

구현 은닉

- 접근 제어는 구현 은닉을 할 수 있게 한다.
- 클래스 작성자 → 인터페이스를 바꾸지 않는 한 외부에 미치는 영향 걱정 X
- 클라이언트 프로그래머 → 내부 상관 없이 인터페이스만 알고 있으면 됌

따라서, 인터페이스와 구현을 깔끔하게 분리하기 위해 노력.

**협력하는 객체들의 공동체**

`Screening` → `reserve`를 통해 예매, `Reservation` 인스턴스 반환, `customer` → 예매자 정보, `audienceCount` → 인원수, `calculateFee` → 요금 계산

```java
class Screening {
    public Reservatioin reserve(Customer customer, int audienceCount) {
        return new Reservation(customer, this, calculateFee(audienceCount), audienceCount)
    }
    private Money calculateFee(int audienceCount) {
        return movie.calculateMovieFee(this).times(audienceCount);
    }
}
```

```java
class Money {
    public static final Money ZERO = Money.wons(0);
    
    private final BigDecimal amount;
    
    public static Money wons(long amount) {
        return new Money(BigDecimal.valueOf(amount));
    }
    
    public static Money wons(double amount) {
        return new Money(BigDecimal.valueOf(amount));
    }
    
    Money(BigDecimal amount) {
        this.amount = amount;
    }
    
    public Money plus(Money amount) {
        return new Money(this.amount.add(amount, amount));
    }
    
    public Money minus(Money amount) {
        return new Money(this.amount.subtract(amount, amount));
    }
    
    public Money times(double percent) {
        return new Money(this.amount.multiply(percent));
    }
    
    public boolean isLessThan(Money other) {
        return amount.compareTo(other.amount) < 0;
    }
    
    public boolean isGreaterThanOrEqual(Money other) {
        return amount.compareTo(other.amount) >= 0;
    }
}

```

`Money` →금액과 관련된 다양한 계산 구현

```java
class Reservation() {
    private Customer customer;
    private Screening screening;
    private Money fee;
    private int audienceCount;
    
    public Reservation(Customer customer, Screening screening, Money fee, int audienceCount) {
        this.customer = customer;
        this.screening = screening;
        this.fee = fee;
        this.audienceCount = audienceCount;
    }
}

// 고객, 상영 정보, 예매 요금, 인원수를 속성으로 포함
```

**협력** → 인스턴스들은 서로의 메서드를 호출하며 상호작용 하는 것

**협력에 관한 짧은 이야기**

요청(request) → 다른 객체의 인터페이스에 공개된 행동을 수행하도록 하는 것

응답(respond) → 요청 받은 객체는 요청을 처리 후 응답.

객체끼리 상호작용 하는 것은 메세지를 전송(send a message)하는 것 뿐이다. 반대로도 메세지를 수신했다고 한다.

메세지를 처리하기 위한 자신만의 방법을 **메서드(method)**라고 함

### 03 할인 요금 구하기

**할인 요금 계산을 위한 협력 시작하기**

`Movie` → 제목, 상영 시간, 기본 요금, 할인 정책 가짐.

```java
class Movie {
    private String title;
    private Duration runningTime;
    private Money fee;
    private DiscountPolicy discountPolicy;
    
    public Movie(String title, Duration runningTime, Money fee, DiscountPolicy discountPolicy) {
        this.title = title;
        this.runningTime = runningTime;
        this.fee = fee;
        this.discountPolicy = discountPolicy;
    }
    
    public Money getFee() {
        return fee;
    }
    
    public Money calculateMovieFee(Screening screening) {
        return fee.minus(discountPolicy.calculateDiscountAmount(screening));
    }
}
```

- `calculateMovieFee` → `discountPolicy`에 `calculateDiscountAmount` 메시지를 전송해 할인 요금을 받는다.

어떤 할인 정책을 사용할 지에 대한 코드는 없음. 단지 `discountPolicy`에게 메세지를 전달할 뿐.

1. 상속 (Inheritance)
2. 다형성 

이 속에는 추상화 원리가 숨어있다.

**할인 정책과 할인 조건**

할인 정책 → 금액 & 비율 → AmountDiscountPolicy, PercentDiscountPolicy

두 클래스는 대부분이 유사하고, 계산 방식만 다르다.

부모 클래스인 `DiscountPolicy` 안에 중복 코드를 두고 `AmountDiscountPolicy`와 `PercentDiscountPolicy`가 상속 받도록 한다. 실제로는 `DiscountPolicy` 인스턴스가 없어서, 추상 클래스로 구현함.

```java
public abstract class DiscountPolicy {
    private List<DiscountCondition> conditions = new ArrayList<>();
    
    public DiscountPolicy(DiscountCondition ... conditions) {
        this.conditions = Arrays.asList(conditions)
    }
    
    public Mondy calculateDiscountAmount(Screening screening) {
        for(DiscountCondition each: conditions) {
            if(each.isSatisfiedBy(screening)) {
                return getDiscountAmount(screening);
            }
        }
        
        return Money.ZERO;
    }
    
    abstract protected Money getDiscountAmount(Screening Screening);
    // 추상메서드로, 할인 요금을 계산한다.
}
```

`DiscountPolicy`는 할인 여부, 계산에 필요한 흐름은 제공하지만 실제로 요금을 계산하는 부분은 추상 메서드인 `getDiscountAmount`에 위임한다. 실제로는 `DiscountPolicy`를 상속받은 자식 클래스에서 오버라이딩 한 메서드가 실행 될 것이다.

위와 같은 디자인 패턴을 TEMPLATE METHOD 패턴이라고 한다.

```java
public interface DiscountCondition{
		boolean isSatisfiedBy(Screening screening);
}

// 인터페이스를 통해 선언.
// isSatisfied -> 인자로 전달된 screening이 할인 가능한지 
```

DiscountCondition은 순번 조건(`SequenceCondition`)과 기간 조건(`PeriodCondition`)이 있다.

```java
public class SequenceCondition implements DiscountCondition {
    private int sequence;
    
    public SequenceCondition(int sequence) {
        this.sequence = sequence;
    }
    
    public boolean isSatisfiedBy(Screening screening) {
        return screening.isSequence(sequence);
    }
    
    // 인자로 전달된 screening의 상영 순번과 일치하는지 판단해 return
}
```

```java
public class PeriodCondition implements DiscountCondition {
    private DayOfWeek dayOfWeek;
    private LocalTime startTime;
    private LocalTime endTime;
    
    public PeriodCondition(DayOfWeek dayOfWeek, LocalTime startTime, LocalTime endTime) {
        this.dayOfWeek = dayOfWeek;
        this.startTime = startTime;
        this.endTime = endTime;
    }
    
    public boolean isSatisfiedBy(Screening screening) {
        return screening.getStartTime().getDayOfWeek().equals(dayOfWeek) &&
            startTime.compareTo(screening.getStartTime().toLocalTime()) <= 0 &&
            endTime.compareTo(screening.getStartTime().toLocalTime()) >= 0;
    }
    
    // 인자로 전달된 screening의 상영 요일이 dayOfWeek과 같고, 상영 시작 시간이 startTime과 endTime 있는지 판단해 return
}
```

할인 정책 구현

```java
public class AmountDiscountPolicy extends DiscountPolicy {
    private Money discountAmount;
    
    public AmountDiscountPolicy(Money discountAmount, DiscountCondition ... conditions) {
        super(conditions);
        this.discountAmount = discountAmount;
    }
    
    @Override
    protected Money getDiscountAmount(Screening screening) {
        return discountAmount;
    }
}
```

```java
public class PercentDiscountPolicy extends DiscountPolicy {
    private double percent;
    
    public PercentDiscountPolicy(double percent, DiscountCondition ... conditions) {
        super(conditions);
        this.percnet = percent;
    }
    
    @Override
    protected Money getDiscountAmount(Screening screening) {
        return screening.getMovieFee().times(percent);
    }
}
```

영화 가격 계산에 참여하는 모든 클래스 나타냄.

![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/b0078df0-8e0f-42b1-9c37-cd13fc42e65c/c8a1532c-b29a-45fc-86f8-c8c4d64ed7d1/image.png)

**할인 정책 구성하기**

할인 정책 → 단 하나

할인 조건 → 여러 개 적용

`Movie`와 `DiscountPolicy`의 생성자는 이런 제약을 강제한다. 

- `Movie`의 생성자는 오직 하나의 `DiscountPolicy`인스턴스만 받을 수 있다.

```java
class Movie {    
    public Movie(String title, Duration runningTime, Money fee, DiscountPolicy discountPolicy) {
        ...
        this.discountPolicy = discountPolicy;
    }
		...
}
```

- `DiscountPolicy`의 경우에는 여러 개의 `DiscountCondition` 인스턴스를 허용

```java
public abstract class DiscountPolicy {
		...
    public DiscountPolicy(DiscountCondition ... conditions) {
        this.conditions = Arrays.asList(conditions)
    }
    ...
} 
```

위와 같이 생성자의 파라미터 목록을 이용해 초기화에 필요한 정보를 강제하면 올바른 상태를 가진 객체의 생성을 보장할 수 있다.

### 04 상속과 다형성

**컴파일 시간 의존성과 실행 시간 의존성**

`Movie`는 `DiscountPolicy` 와 연결되어있으며, `Amount`와 `Policy`는 추상 클래스 `DiscountPolicy`를 상속 받는다. 이처럼 어떤 클래스가 다른 클래스에 접근 할 수 있는 경로를 가지거나 해당 클래스의 객체의 메서드를 호출 할 수 있는 경우 두 클래스 사이에 의존성이 있다 할 수 있다.

![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/b0078df0-8e0f-42b1-9c37-cd13fc42e65c/b6c52ba0-a404-40f9-82dd-c8abfaefc17f/image.png)

문제는 영화 요금을 계산하기 위해 `DiscountPolicy`가 아닌, `AmountDiscountPolicy`와 `Percent~` 가 필요하다는 것이다. 따라서, `Movie`의 인스턴스는 `Amount~`나 `Percent~` 에 의존해야하는데, 아무것도 의존하지 않음.

그렇다면 `Movie`인스턴스가 코드 작성 시점에 `Amount~` 와 `Percent`의 인스턴스와 실행 시점에 협력 가능한 이유는?

만약 영화 요금을 계산하기 위해 금액 할인 정책을 쓴다면 `Movie` 의 인스턴스를 생성할 때 인자로 `Amount~` 의 인스턴스를 전달하면 된다.

```java
Movie avatar = new Movie("아바타",
	Duration.ofMinutes(120),
	Money.wons(10000),
	new AmountDiscountPolicy(Money.wons(800), ...));
```

실행 시, Movie 인스턴스는 `AmountDiscountPolicy`에 의존하게 될 것이다.

만약 비율할인 정책을 적용하고 싶다면, `Percent~` 를 전달하면 된다.

```java
Movie avatar = new Movie("아바타",
	Duration.ofMinutes(120),
	Money.wons(10000),
	new PercentDiscountPolicy(0.1, ...));
```

요지는,

코드의 의존성과 실행 시점의 의존성이 서로 다를 수 있다는 것이다. 즉, 클래스 사이의 의존성과 객체 사이의 의존성은 동일하지 않다.

그러나 코드의 의존성과 실행 시점의 의존성이 다를 수록 코드를 이해하기 어려워진다. 코드를 이해하기 위해서 다양한 부분을 찾고 이해해야하기 때문이다. 

반면 코드의 의존성과 실행 시점의 의존성이 다르면 코드는 더 유연해지고 확장가능해진다. 의존성의 양면성은 설계가 트레이드 오프라는 사실을 보여준다.

설계가 유연할 수록 코드를 이해하고 디버깅 하기는 어려워진다. 반대로도 성립한다. 따라서 우리가 고민해야 할 것들이다.

**차이에 의한 프로그래밍**

클래스를 추가하고 싶은데 기존의 어떤 클래스와 매우 유사해서 재사용에 조금만 더 기능을 추가하고 싶을 때 상속을 사용한다.

- 기존의 모든 속성과 행동을 새 클래스에 포함 가능
- 쉽고 빠르게 추가 가능
- 부모 클래스의 구현은 공유하면서도 행동이 다른 자식 클래스를 쉽게 추가 가능

이처럼 부모클래스와 다른 부분만 추가해 새로운 클래스를 쉽고 빠르게 만드는 방법 → 차이에 의한 프로그래밍(programming by difference)

**상속과 인터페이스**

상속이 가치 있는 이유는 부모가 제공하는 모든 인터페이스를 자식이 물려받을 수 있기 때문이다. 그러나 대부분은 상속의 목적은 메서드나 인스턴스 변수를 재사용 하는 것이라 생각한다.

인터페이스 → 객체가 이해할 수 있는 메시지의 목록을 정의함. 상속을 통해 자식은 자신의 인터페이스에 부모의 인터페이스를 포함할수 있음.

```java
class Movie {
		...
    public Money calculateMovieFee(Screening screening) {
        return fee.minus(discountPolicy.calculateDiscountAmount(screening));
    }
}
```

`Movie`는 인터페이스에 정의된 `calculateDiscountAmount`를 전송한다. `DiscountPolicy`를 상속받는 `Amount~`와 `Percent` 도 메서드가 포함되어있다. `Movie`는 어떤 클래스의 인스턴스인지가 중요한게 아니고, `calculateDiscountAmoun`와 소통한다는게 중요하다. 

자식 클래스는 상속을 통해 부모의 인터페이스를 제공받아 부모 대신 사용 될 수 있다. `Movie`의 생성자에서 인자 타입이 `DiscountPolicy`임에도 `Amount~`와 `Percent~` 를 사용할 수 있는 이유다.

이처럼 자식이 부모를 대신 하는 것을 업캐스팅(upcasting)이라 한다.

**다형성**

메시지 ↔ 메서드 다른 개념!

`Movie`는 `DiscountPolicy`의 인스턴스에 `calculateDiscountAmount` 메세지를 전송한다. 실행되는 메서드는 `Movie`와 상호작용 하기 위해 연결된 클래스가 무엇이냐에 따라 다름(`Amount` 또는 `Percent`)

`Movie` 는 동일한 메세지를 전송하지만, 실제로 어떤 메서드가 실행될 것인지는 메세지를 수신하는 객체의 클래스가 무엇이냐에 따라 달라진다. 이를 다형성이라 한다.

다형성

→ 동일한 메세지를 수신 했을 때, 객체의 타입에 따라 다르게 응답할 수 있는 능력

→ 따라서, 다형적 협력에 참여하는 객체는 모두같은 메세지를 이해할 수 있어야 한다.

- 다형성은 컴파일 시간 의존성과 실행시간 의존성이 다를 수 있다는 사실을 기반으로 한다. 코드 자체는 `DiscountPolicy`에 의존한 기존 코드 처럼.
- 다형성 구현 방법
    - 지연 바인딩 or 동적 바인딩 (lazy binding or dynamic binding)
        - 메세지에 응답하기 위해 실행될 메서드를 컴파일 시점이 아닌 실행시점에 결정함.
    - 초기 바인딩 or 정적 바인딩 (early binding or static binding)
        - 전통적인 함수 호출 처럼 컴파일 시점에 실행될 함수나 프로시저를 결정하는 것

객체지향이 컴파일 시점의 의존성과 런타임 의존성을 분리하고 하나의 메세지를 선택적으로 서로 다른 메서드에 연결할 수 있는 이유가 동적 바인딩을 사용하기 때문.

**인터페이스와 다형성**

종종 구현은 필요 없고 순수하게 인터페이스만 공유하고 싶을 때가 있다. C#, Java에서는 `interface`를 제공한다. C++에서는 Abstract Base Class(ABC) 제공.

추상 클래스를 이용해 다형성을 구현한 할인 정책과 달리, 

할인 조건은 구현을 공유할 필요가 없어 인터페이스를 이용해 타입 계층을 구현했다. 

위와 같은 경우 역시 동일한 인터페이스를 공유하며 `DiscountCondition`을 대신해 사용 될 수 있다. 동일하게 업캐스팅이 가능하며, 협력은 다형적이다.

### 05 추상화와 유연성

**추상화의 힘**

할인 정책은 구체적인 금액 & 비율 할인 정책을 포괄하는 추상적 개념이다. 할인 정책 역시 더 구체적인 순번 & 기간 조건을 포괄하는 추상적 개념이다. 

이와 같은 추상화를 사용할 경우 장점

- 추상화의 계층만 따로 떼어놓고 살펴보면 요구사항의 정책을 높은 수준에서 서술 가능하다는 것
    - 세부적인 내용을 무시하고 상위 정책을 쉽고 간단하게 표현 가능
    - 기본적인 애플리케이션 협력 흐름을 기술한다는 것이다.
    - 이는 디자인 패턴과 프레임워크 모두 추상화를 이용해 정책을 정의하는 객체지향의 매커니즘을 따르기 때문이다.
- 추상화를 이용하면 설계가 더 유연해진 다는 것

**유연한 설계**

위의 장점 두번째이다.

우리는 아직 스타워즈의 경우인 할인 정책이 적용되지 않은 경우이다.

```java
public class Movie {
		public Money calculateMovieFee(Screening screening) {
				if(discountPolicy == null) {
						return fee;
				}
		}
}
```

이 방식은 할인 정책이 없는 경우를 예외로 취급해 지금까지 일관성 있던 협력을 무너트린다.

할인 금액이 0원이라는 사실을 결정하는 책임이 `DiscountPolicy`가 아닌, `Movie` 가 결정하기 때문.

`NoneDiscountPolicy`를 추가해 책임을 그대로 `DiscountPolicy` 에 유지한다.

```java
public class NoneDiscountPolicy extends DiscountPolicy {
		@Override
		protected Money getDiscountAmount(Screening screening) {
				return Money.ZERO;
		}
}
```

중요한 것은 `Movie`, `DiscountPolicy` 변경없이 했다는 점이다. 새로운 클래스 추가를 통해서 애플리케이션의 기능을 추가했다.

이는 구체적인 상황에 묶이는 것을 방지한다.이를 컨텍스트 독립성이라고 부른다.

**추상 클래스와 인터페이스 트레이드오프**

`NoneDiscountPolicy` → `getDiscountAmount`가 어떤 값을 반환해도 상관이 없다. 부모클래스에서 할인 조건이 없을 때 위 메서드를 호출하지 않는다. 이는 부모클래스와 자식을 개념적으로 결합한다.

`DiscountPolicy`를 인터페이스로 변경

```jsx
public interface DiscountPolicy {
		Money calculateDiscountAmount(Screening screening);
}
```

원래의 `DiscountPolicy`를 `DefaultDiscountPolicy` 로 변경

```jsx
public abstract class DefaultDiscountPolicy implements DiscountPolicy {
    ...
}
```

`NoneDiscountPolicy`가 `DiscountPolicy` 인터페이스를 구현하도록 변경하면 개념적 혼란과 결합을 제거 가능하다.

```jsx
public class NoneDiscountPolicy implements DiscountPolicy {
		@Override
		public Money calculateDiscountAmount(Screening screening){
				return Money.ZERO;
		}
}
```

바뀐 설계 (인터페이스 이용해 구현한 DiscountPolicy 계층)

![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/b0078df0-8e0f-42b1-9c37-cd13fc42e65c/a84b8316-c007-4f3f-ad41-f27bd55f76cb/image.png)

인터페이스 사용이 이상적일 순 있으나, `NoneDiscountPolicy`를 위해 인터페이스를 추가하는게 과하다고 생각할 수도 있다. (앞으로는 위의 설계가 아니고 기존 설계로 나아감.)

**코드 재사용**

상속은 항상 좋은게 아니다.

합성(composition)

- 다른 객체의 인스턴스를 자신의 인스턴스 변수로 포함해 재사용 하는 것
- `Movie`가 `DiscountPolicy` 의 코드를 재사용 하는게 합성이다.

**상속**

두 가지 관점에서 안 좋은 영향 미침.

- 상속이 캡슐화를 위반하는 것
    - 상속을 쓰기 위해선 부모 클래스의 내부 구조를 잘 알고 있어야 한다.
    - 부모 클래스의 구현
- 설계를 유연하지 못하게 만든다.
    - 부모-자식 관계를 컴파일 시점에 결정한다. 따라서 실행 시점에 객체의 종류를 변경하는것이 불가능하다.

**합성**

`Movie` →요금 계산 위해 `DiscountPolicy` 코드를 재사용. 인터페이스를 통해 약하게 결합함.

상속 → 부모의 코드와 자식의 코드를 컴파일 시점에 강하게 결합함.

합성은 상속이 가지는 두가지 문제 모두를 해결한다.

- 인터페이스에 정의된 메세지를 통해서만 가능하기 때문에 구현을 효과적으로 캡슐화 가능했다.
- 의존하는 인스턴스를 교체하는 것이 비교적 쉽기 때문에 유연한 설계가 가능.

따라서, 코드 재사용을 위해서는 상속 보다는 합성이 좋다.

그렇지만 상속을 사용하지 말라는 것은 아님. 대부분의 설계에선 같이 사용한다. 원래 설계를 보면, `Movie`- `DiscountPolicy`는 합성이고, `DiscountPolicy` - `AmountDiscountPolicy`, `Percent` 는 상속으로 이뤄진다. 코드를 재사용하면 합성이 맞지만, 다형성을 위해 인터페이스를 재사용하면 상속과 합성을 같이 사용해야한다.
