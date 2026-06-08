**4장 요약**

데이터 중심의 접근법을 취할 경우 행동보다 데이터를 먼저 결정하고 협력이라는 문맥을 벗어남.

1. 고립된 객체의 상태에 초점을 맞추기 때문에 캡슐화를 위반하기 쉬움
2. 요소들 사이의 결합도가 높아짐
3. 코드 변경하기 어려워짐.

위 문제를 해결하기 위해서는 데이터가 아닌, 책임에 초점.

책임에 초점해 설계할 때 직면하는 어려움

1. 어떤 객체에게 어떤 책임을 할당할지.
    1. 책임 할당은 일종의 트레이드 오프이다.

이번장에서는 GRASP 패턴으로, 책임 할당의 어려움을 해결하기위한 답이 될 것이다

### 01 책임 주도 설계를 향해

책임 중심 설계의 원칙

1. 데이터보다 행동을 먼저 결정
2. 협력이라는 문맥 안에서 책임 결정

**데이터보다 행동 먼저 결정**

객체에게 중요한 건 데이터가 아닌, 외부에 제공하는 행동. 행동 = 객체의 책임. 데이터에서 행동으로 초점을 맞춰야 한다. 

객체를 설계하기 위한 질문의 순서를 바꾸자. 

1. 이 객체가 수행 해야하는 책임은 무엇인가
2. 이 책임을 수행하는데 필요한 데이터는 무엇인가

**협력이란 문맥안에서 책임 결정하기**

책임의 품질 → 협력에 적합한 정도로 결정. 

협력에 적합하다면 그 책임은 좋은 것.

- 메세지를 전송하는 클라이언트의 의도에 적합한 책임을 할당해야한다.
- 적합한 책임을 수확하기 위해서는 메세지를 결정한 후에 객체를 선택해야한다. 객체가 메시지를 선택하는 것이 아니고, 메세지가 객체를 선택해야한다.

> 객체를 가지고 있기 때문에 메시지를 보내는 것이 아니다. 메시지를 전송하기 때문에 객체를 갖게 된 것이다. - Metz
> 
- 메시지가 클라이언트의 의도를 표현함.
- 객체를 결정하기 전 객체가 수신 할 메세지를 먼저 결정함.
- 메세지를 수신하기로 결정된 객체는 메세지를 처리할 책임을 할당 받게 된다.

객체에게 적절한 책임을 할당하기 위해선 협력을 고려해야한다. 협력이라는 문맥에서 적절한 책임이란 클라이언트 관점에서 책임을 의미한다. 올바른 객체지향 설계는 클라이언트가 전송할 메세지를 결정 후에 비로소 객체의 데이터에 관해 고민한다.

이는 책임 주도 설계 방법의 핵심과 거의 동일하다.

**책임 주도 설계**

책임 주도 설계의 흐름

- 시스템이 사용자에게 제공해야 하는 기능인 시스템 책임을 파악한다.
- 시스템 책임을 더 작은 책임으로 분할한다.
- 분할된 책임을 수행할 수 있는 적절한 객체 또는 역할을 찾아 책임을 할당
- 객체가 책임을 수행하는 도중 다른 객체의 도움이 필요한 경우 이를 책임질 적절한 객체 또는 역할을 찾는다.
- 해당 객체 또는 역할에게 책임을 할당해 두 객체가 협력하도록 한다.

### 02 책임 할당을 위한 GRASP 패턴

GRASP 패턴(General Responsibility Assignment Software Pattern, 일반적 책임 할당을 위한 SW 패턴)

**도메인 개념에서 출발하기**

설계를 시작하기 전 도메인에 대한 개략적 모습을 그리는게 유용함. 도메인 안에 많은 개념이 존재하고, 이 도메인 개념을 책임 할당의 대상으로 사용하면 코드에 도메인의 모습을 투영하기가 좀 더 수월해짐.

영화 예매 시스템을 구성하는 도메인 개념과 개념 사이의 관계를 대략적으로 표현하자.

- 하나의 영화는 여러 번 상영 가능
- 하나의 상영은 여러 번 예약 될 수 있다.
- 영화는 다수의 할인 조건을 가지고, 할인 조건에는 순번 조건, 기간 조건이 있다.
- 할인 조건은 순번 조건과 기간 조건으로 분류
- 영화는 금액이나 비율에 따라 할인될 수 잇지만 동시에 두 가지 할인 정책을 적용할 수 없다.

시작하는 단계에서는 개념들의 의미와 관계가 정확, 완벽할 필요 없다. 단지 출발점이 필요할 뿐. 중요한 것은 설계를 시작하는 것이지, 도메인 개념들을 완벽하게 정리하는 것이 아니고, 빠르게 설계와 구현을 하자.

**정보 전문가에게 책임을 할당하라**

첫 단계 - 애플리케이션이 제공해야하는 기능을 애플리케이션의 책임으로 생각하기

- 애플리케이션에 대해 전송된 메세지로 간주, 이 메세지를 책임질 첫 번째 객체를 선택하는 것으로 시작.
- 애플리케이션 → 영화를 예매할 책임이 있다.
- 위의 책임을 수행하는데 필요한 메세지
    - 메세지를 전송할 객체는 무엇을 원하는가?
        - 객체가 원하는 것은 “예매하라”라는 것, 예매하라는 메시지.
    - 메세지를 수신할 객체는 누구인가?
        - 적합한 객체 선택.
        - 답하기 위해서는 객체가 상태와 행동을 통합한 캡슐화의 단위임을 잊지말자.
        - 객체의 책임과 이를 수행하는데 필요한 상태는 동일한 객체 안에 있어야 한다. 책임을 수행할 정보를 알고 있는 객체에게 책임을 할당하기 → 정보 전문가(Information Expert) 패턴이라고 GRASP에서는 부른다.

정보 전문가(Information Expert) 패턴

- 객체가 자신이 소유하고 있는 정보와 관련된 작업을 수행한다는 일반적인 직관을 표현함.
- 정보 ≠ 데이터. 그 정보를 알고 있다고 해서 저장 할 필요는 없다.
    - 다른 객체를 알거나, 필요한 정보를 계산해서 제공 가능.
    - 어떤 방식이든 정보 전문가가 데이터를 반드시 저장하고 있을 필요 X

‘예매하라’ 라는 메세지를 처리하기 위해서는 ‘상영’이라는 도메인이 적합할 것이다.

영화 정보, 상영 시간, 상영 순번과 같이 정보를 많이 안다.

‘예매하라’를 수신하고, `Screening`이 해야할 작업의 흐름 (외부 인터페이스 X, 내부 구현)

- `Screening`이 책임을 수행하는데 필요한 작업 구상
- 스스로 처리 할 수 없는 작업이 무엇일까? 구상
    - 스스로 처리할 수 없다면 외부에 도움 요청. 이는 새로운 메세지가 되고, 새로운 객체의 책임으로 할당된다.
- 예매 가격은 한 편의 가격 x 예매 인원수 이므로, 한 편의 가격을 알아야 한다.
- 가격 계산에 필요한 정보를 모르기 때문에 외부 객체에 요청해 ‘가격을 계산하라’라는 메시지가 전송 될 것이다.

!`Screening`은 예매하라를 수신하고, 가격을 계산하라라는 메시지가 전송될 것이다.

`Screening`은 예매하라를 수신하고, 가격을 계산하라라는 메시지가 전송될 것이다.

‘가격을 계산하라’ 라는 메세지를 처리하기 위해 필요한 정보를 알고 있는 전문가는 `Movie`.

따라서, Movie는 영화 가격을 계산할 책임을 지게 된다.

- 계산 할 때는 할인 가능한지 판단한다.
- 할인 정책에 따라 할인 요금을 제외한 금액을 계산
- 영화가 스스로 처리할 수 없는 일 → “할인 가능 판단”
    - 할인 여부를 판단하라 메세지를 전송해 외부의 도움을 요청해야한다.

!image.png

그렇다면 할인 여부를 판단할 객체는? 할인 조건(`DiscountCondition`)이다.

- 할인 조건은 할인 여부를 판단하는데 필요한 모든 조건을 알고 있다.
- 외부 도움 필요 없이 가능하므로 외부에 메시지를 전송하지 않는다.

정리

INFORMATION EXPERT 패턴은 객체에게 책임을 할당 할 때 가장 기본이 되는 책임 할당 원칙이다. 이 패턴을 따르는 것만으로도 자율성이 높은 객체들로 구성된 협력 공동체를 구축할 가능성이 높다.

**높은 응집도와 낮은 결합도**

설계는 동일한 기능의 무수히 많은 설계가 가능하다. 이 설계 중 하나를 선택 해야할 때가 올 것이다. 올바른 책임 할당을 위해 INFORMATION EXPERT 외의 다른 패턴도 적용할 필요가 있다.

`Movie`가 `DiscountCondition`에 할인 여부를 판단하라 여부를 전송하는 대신, `Screening`이 직접 `DiscountCondition` 에 할인 여부를 판단하라 메세지를 받고 반환 받은 할인 여부를 `Movie`에 전달하는 방식은?

!image.png

기능적인 측면에서는 별로 차이 없으나, 응집도와 결합도 측면에서 위와 차이가 있다.

이전 패턴과 현재 패턴 중에서, 이전 패턴을 선택한 이유는, 높은 응집도와 낮은 결합도를 얻을 수 있는 설계가 있다면 그 설계를 선택하는게 좋기 때문.

이를 LOW COUPLING (낮은 결합도) 패턴과 HIGH COHESION (높은 응집도) 라고 한다.

LOW COUPLING (낮은 결합도)

- `Movie`와 `DiscountCondition`은 이미 결합되어있어 협력하게 되면 설계 전체적으로 결합도를 추가하지 않고 협력 완성 가능
- 그러나 두 번째의 경우 `Screening`과 `DiscountCondition`사이에 새로운 결합이 추가된다.

HIGH COHESION (높은 응집도)

- `Screening`과 `DiscountCondition`이 협력하면 `Screening`도 함께 변경되어야 한다.
- `Movie`의 책임은 영화 요금을 계산하는 것이고, 요금을 계산하는데 필요한 조건을 판단하기 위해 `Movie`와 `DiscountCondition`이 결합하는 것은 응집도에 해 X

**창조자에게 객체 생성 책임을 할당하라**

이 서비스의 최종 결과물은 `Reservation` 인스턴스를 생성하는 것 = 어떤 객체에게는 `Reservation`인스턴스를 생성할 책임을 할당해야한다.

CREATER(창조자)

아래 조건을 최대한 많이 만족하는 객체(생성하는 클래스 B)에 객체 생성(생성해야 할 A 클래스) 책임을 할당해라.

- B가 A를 포함하거나 협조한다.
- B가 A를 기록한다.
- B가 A를 긴밀하게 사용한다.
- B가 A를 초기화 하는데 필요한 데이터를 가지고 있다. (이 경우엔 B는 A에 대한 정보전문가이다.)

CREATER의 의도는 어떤 방식으로든 생성되는 객체와 연결되거나 관련될 필요가 있는 객체에 해당 객체의 생성 책임을 맡기는 것이다. 생성될 객체에 대해 잘 알고 있어야 하거나 그 객체를 사용해야 하는 객체는 어떤 방식으로든 생성될 객체와 연결될 것이다. 즉, 두 객체는 서로 결합된다.

CREATER은 이미 존재하는 객체 사이의 관계를 이용하기 때문에 설계가 낮은 결합도를 유지할 수 있다.

`Reservation`을 잘 알고 긴밀하게 사용하며 초기화에 필요한 데이터를 가지고 있는 객체는 `Screening`이다. 따라서, `Screening` 을 `Reservation`의 CREATER로 선택하는 것이 좋다.

!image.png

### 03 구현을 통한 검증

```java
public class Screening {
  public Reservation reserve(Customer customer, int audienceCount) {
  }
  // Screening의 관점에서 예약하라 메세지를 처리할 메서드.
}
```

메세지를 생각했으므로`Screening` 의 변수들을 생각해야한다.

```java
public class Screening {
  private Movie movie;
  private int sequence;
  private LocalDateTime whenScreened;
  
  public Reservation reserve(Customer customer, int audienceCount) {
    return new Reservation(customer, this, calculateFee(audienceCount), audienceCount);
  }
  
  private Money calculateFee(int audienceCount) {
    return movie.calculateMovieFee(this).times(audienceCount);
  }
}
```

`Screening`이 `Movie`의 내부 구현에 대한 어떤 지식도 없이 전송할 메세지를 정했다. 수신자의 `Movie`를 생각한 게 아닌, 송신자의 `Screening`의 의도를 표현했다. `Movie` 의 구현을 고려하지 않고 필요한 메세지를 결정하면 깔끔하게 캡슐화가 가능하다.

`Movie` 및 영화 종류 `MovieType`구현 

```java
public class Movie {
  private String title;
  private Duration runningTime;
  private Money fee;
  private List<DiscountCondition> discountConditions;
  
  private MovieType movieType;
  private Money discountAmount;
  private double discountPercent;
  
  public Money calculateMovieFee(Screening screening) {
    if(isDiscountable(screening)) {
      return fee.minus(calculateDiscountAmount());
    }
    
    return fee;
  } // 
  
  private boolean isDiscountable(Screening screening) {
    return discountConditions.stream()
        .anyMatch(condition -> condition.isSatisfiedBy(screening));
  }
  /*
  discountCondition의 원소를 순회하고, isSatisfiedBy 메세지를 전송해 
  할인 여부를 판단한다. 할인 조건 만족하는 DiscountCondition 인스턴스
  가 존재한다면 요금 계산을 하는 calculateDiscountAmount를 호출한다.
  */
  
  private Money calculateDiscountAmount() {
    switch(movieType) {
      case AMOUNT_DISCOUNT:
        return calculateAmountDiscountAmount();
      case PERCENT_DISCOUNT:
        return calculatePercentDiscountAmount();
      case NONE_DISCOUNT:
        return calculateNoneDiscountAmount();
    }
    
    throw new IllegalStateException();
  } // 실제 할인 요금을 movieType에 따라 적절한 메서드를 호출한다.
  
  private Money calculateAmountDiscountAmount() {
    return discountAmount;
  }
  private Money calculatePercentDiscountAmount() {
    return fee.times(discountPercent);
  }
  private Money calculateNoneDiscountAmount() {
    return Money.ZERO;
  }
}
```

`DiscountCondition` 은 기간 조건과 순번 조건을 위한 메서드를 제공한다.

```java
public class DiscountCondition {
  private DiscountConditionType type;
  private int sequence;
  private DayOfWeek dayOfWeek;
  private LocalTime startTime;
  private LocalTime endTime;
  
  public boolean isSatisfiedBy(Screening screening) {
    if(type == DiscountConditionType.PERIOD) {
      return isSatisfiedByPeriod(screening);
    }
    
    return isSatisfiedBySequence(screening);
  }
  
  private boolean isSatisfiedByPeriod(Screening screening) {
    return dayOfWeek.equals(screening.getWhenScreened().getDayOfWeek()) &&
      startTime.compareTo(screening.getWhenScreened().toLocalTime()) <= 0 &&
      endTime.compareTo(screening.getWhenScreened().toLocalTime()) >= 0;
  }
  
  private boolean isSatisfiedBySequence(Screening screening) {
    return sequence == screening.getSequence();
  }
}

public enum DiscountConditionType {
  SEQUENCE,
  PERIOD
} // 할인 조건의 종류를 나열하는 열거형
```

`DiscountCondition`은 할인 조건 판단을 위해 `Screening`의 상영 시간과 순번을 알아야 한다. 두 정보를 제공하는 메서드를 `Screening`에 추가한다. (getter 추가)

불편한 몇가지가 있다.

**DiscountCondition 개선하기**

- `DiscountCondition`은 변경에 취약한 클래스이다.
1. 새로운 할인 추가
2. 순번 조건을 판단하는 로직 변경
3. 기간 조건을 판단하는 로직이 변경될 경우

하나 이상의 변경 이유를 가지기 때문에 응집도가 낮다. 낮은 응집도가 초래하는 문제를 해결하기 위해서는 변경에 이유에 따라 클래스를 분리한다.

클래스의 응집도 판단하기

1. 클래스가 하나 이상의 이유로 변경되어야 한다면 응집도가 낮다.
2. 클래스의 인스턴스를 초기화 하는 시점에 경우에 따라 다른 속성들을 초기화하고 있다면 응집도가 낮다. 초기화되는 그룹을 기준으로 클래스를 분리하자.
3. 메서드 그룹이 속성 그룹을 사용하는지 여부로 나뉘면 응집도가 낮다. 이 그룹을 기준으로 클래스를 분리하자.

`DiscountCondition`에는 세가지 모두가 들어가있다.

**타입 분리하기**

`DiscountCondition`에는 순번 조건과 기간 조건 두 개의 독립적 타입이 하나의 클래스 안에 공존하고 있다. 두 타입을 `SequenceCondition`과 `PeriodCondition`두 가지로 나뉜다.

```java
public class PeriodCondition {
    private DayOfWeek dayOfWeek;
    private LocalTime startTime;
    private LocalTime endTime;

    public PeriodCondition(DayOfWeek dayOfWeek, LocalTime startTime, LocalTime endTime) {
        this.dayOfWeek = dayOfWeek;
        this.startTime = startTime;
        this.endTime = endTime;
    }

    public boolean isSatisfiedBy(Screening screening) {
        return dayOfWeek.equals(screening.getWhenScreened().getDayOfWeek()) && 
            startTime.compareTo(screening.getWhenScreened().toLocalTime()) <= 0 &&
            endTime.compareTo(screening.getWhenScreened().toLocalTime() >= 0);
    }
}

public class SequenceCondition() {
    private int sequence;

    public SequenceCondition(int sequence) {
        this.sequence = sequence;
    }

    public boolean isSatisfiedBy(Screening screening) {
        return sequence == screening.getSequence()
    }
}
```

클래스를 분리하면 앞의 문제가 모두 해결된다. 

- `SequenceCondition`과 `PeriodCondition`은 자신의 모든 인스턴스를 초기화 할 수 있다.
- `sequence`만 사용하는 메서드는 `SequenceCondition`으로, `dayOfWeek`, `endTime` … 사용하는 메서드는 `PeriodCondition`으로 이동했다.결과적으로 클래스의 응집도가 향상됐다.

하지만 새로운 문제가 발생.

`Movie`는 `SequenceCondition`과 `PeriodCondition` 두 개의 클래스의 인스턴스 모두가 협력할 수 있어야 한다.

첫 번째 해결 방법은 각각의 목록을 따로 유지하는 것.

```java
public class Movie {
  private String title;
  private Duration runningTime;
  private Money fee;
  private List<DiscountCondition> discountConditions;
  private List<PeriodCondition> periodConditions;
  private List<SequenceCondition> sequenceConditions;
  
  private MovieType movieType;
  private Money discountAmount;
  private double discountPercent;
  
  public Money calculateMovieFee(Screening screening) {
    if(isDiscountable(screening)) {
      return fee.minus(calculateDiscountAmount());
    }
    
    return fee;
  }
  
  private boolean isDiscountable(Screening screening) {
    return checkPeriodConditions(screening) || checkSequenceConditions(screening);
  }
  
  private boolean checkPeriodConditions() {
	  return periodConditions.stream()
						  .anyMatch(condition -> condition.isSatisfiedBy(screening));
  }
  
  private boolean checkSequenceConditions(Screening screening) {
	  return sequenceConditions.stream()
						  .anyMatch(condition -> condition.isSatisfiedBy(screening));
  }
} 
```

위와 같은 방법은, `PeriodConditions` 와 `SequenceConditions` 둘 모두 결합을 하게 된다.

또한, 수정 후에 새로운 할인 조건을 추가하기가 더 어려워졌다. 새로운 할인 조건을 추가하려면 List를 Movie에 인스턴스 변수로 추가해야하고, 호출하도록 `isDiscountable`도 수정해야한다.

두번째 해결 방법은 다형성으로 분리하기다.

**다형성을 통해 분리하기**

두 클래스가 서로 다르다는 사실은 `Movie`입장에서는 중요하지 않다. 할인 가능 여부만 반환해주면 되고, 어떤 종류인지는 중요하지 않다.

역할

`Movie`의 입장에서 `SequenceConditions`과 `PeriodConditions`는 동일한 책임, 동일한 역할을 수행한다. 각각의 역할에 개념을 적용하면 `Movie`는 구체적인 클래스는 알지 못하고 역할에 대해서만 결합되도록 의존성을 제한할 수 있다.

할인 조건의 경우엔 `SequenceConditions`과 `PeriodConditions` 클래스가 구현을 공유할 필요는 없고, `DiscountConditions`라는 인터페이스를 이용하면 된다.

```java
public interface DiscountCondition {
    boolean isSatisfiedBy(Screening screening);
}

public class PeriodCondition implements DiscountConditions {
 ...   
}

public class SequenceCondition implements DiscountConditions {
 ...   
}
```

`Movie`가 전송한 메세지를 수신한 객체의 구체적인 클래스가 무엇인가에 따라 적절한 메서드가 실행된다. `Movie` 와 `DiscountableCondition` 사이의 협력은 다형적으로 바뀐다.

```java
public class Movie {
    private List<DiscountConditions> discountConditions;
    public Money calculateMovieFee(Screening screening) {
        if(isDiscountable(screening)) {
            return fee.minus(calculateDiscountAmount());
        }
        return fee
    }
    
    private boolean isDiscountable(Screening screening) {
        return discountConditions.stream().anyMatch(condition->condition.isSatisfiedBy(screening));
    }
}
```

위를 통해서 알 수 있듯이, 객체의 암시적인 타입에 따라 행동을 분기하면 암시적인 타입을 명시적인 클래스로 정의하고 행동을 나눠 응집도 문제를 해결할 수 있다.

GRASP에서는 이를 POLYMORPHISM(다형성) 패턴이라고 부른다

POLYMORPHISM(다형성)

객체의 타입을 검사해 타입에 따라 여러 대안들을 수행하는 조건적인 논리를 사용하지 말라고 경고한다. 대신 다형성을 이용해 새로운 변화를 다루기 쉽게 확장하라고 한다.

if~else, switch~case 등의 조건 논리는 수정하기 어렵고 변경에 취약하게 만든다.

**변경으로부터 보호하기**

새로운 할인 조건을 추가하는 경우를 생각하자. 

`DiscountCondition`는 `Movie`로부터 `PeriodCondition`과 `SequenceCondition`의 존재를 감춘다. 새로운 `DiscountCondition`이 추가되어도 `Movie`는 수정할 필요가 없다.

이처럼 변경을 캡슐화 하도록 책임을 할당하는 것을 PROTECTED VARIATIONS(변경 보호)패턴이라고 한다.

이는 설계에서 변하는 것이 무엇인지 고려하고 변하는 개념을 캡슐화 하라는 객체지향의 본질을 따른다.

**Movie 클래스 개선하기**

`Movie`또한 `DiscountCondition`이랑 비슷하다. 금액 할인 정책 영화와 비율 할인 정책이라는 두가지 타입을 하나의 클래스 안에 구현하고 있다. 응집도가 낮은 것이다.

똑같이 POLYMORPHISM(다형성)패턴을 따르면 된다. 또한 변경 보호 패턴으로 인터페이스 뒤로 숨겨 캡슐화 할 수 있다. 새로 추가해보자.

```java
public abstract class Movie {
    private String title;
    private Duration runningTime;
    private Money fee;
    private List<DiscountCondition> discountConditions;

    public Movie(String title, Duration runningTime, Money fee,
                DiscountCondition ... discountConditions) {
        this.title = title;
        this.runningTime = runningTime;
        this.fee = fee;
        this.discountConditions = discountConditions;
    }

    public Money calculateMovieFee(Screening screening) {
        if(isDiscountable(screening)) {
            return fee.minus(calculateDiscountAmount());
        }
        return fee;
    }

    private boolean isDiscountable(Screening screening) {
        return discountConditions.steam().anyMatch(condition -> condition.isSatisfiedBy(screening));
    }

    abstract protected Money calculateDiscountAmount();
}
```

`discountAmount`, `discountPercent`와 이 인스턴스 변수들을 사용하는 메서드들이 삭제되었다. 이것들을 `Movie`역할을 수행하는 적절한 자식 클래스로 옮긴다.

금액 할인 정책과 관련된 인스턴스와 메서드를 `AmountDiscountMovie`로 옮기자. `Movie`를 상속받아 구현을 재사용한다.

```java
public class AmountDiscountMovie extends Movie {
    private Money discountAmount;

    public AmountDiscountMovie(String title, Duration runningTime,
                              Money fee, Money discountAmount, DiscountCondition ... discountConditions) {
        super(title, runningTime, fee, discountConditions);
        this.discountAmount = discountAmount;
    }

    @Override
    protected Money calculateDiscountAmount() {
        return discountAmount;
    }
}
```

비율 할인 정책 구현하기

```java
public class PercentDiscountMovie extends Movie {
    private double percent;

    public PercentDiscountMovie(String title, Duration runningTime, 
                        Money fee, double percent, DiscountCondition ... discountConditions){
        super(title, runningTime, fee, discountConditions);
        this.percent = percent;
    }

    @Override
    protected Money calculateDiscountAmount() {
        return getFee().times(percent);
    }
}
```

할인요금 계산을 위해 영화의 기본 금액이 필요하다. `Movie`에서 `getFee`를 추가하자. 가시성을 `protected`로 사용한다.

```java
public abstract class Movie {
    protected Money getFee() {
        return fee;
    }
}
```

`NoneDiscountMovie`클래스 사용하기.

```java
public class NoneDiscountMovie extends Movie {
    public NoneDiscountMovie(String title, Duration runningTime, Money fee) {
        super(title, runningTime, fee);
    }

    @Override
    protected Money calculateDiscountAmount() {
        return Money.ZERO;
    }
}
```

!image.png

**변경과 유연성**

개발자로서 변경에 대비할 수 있는 방법 2가지

1. 코드를 이해하고 수정하기 쉽도록 최대한 단순하게 설계
2. 코드를 수정하지 않고도 변경을 수용할 수 있도록 코드를 더 유연하게 만들기

대부분 전자가 더 좋은 방법이지만, 유사한 변경이 반복적으로 발생한다면 유연성을 추가하는 두번째가 더 좋다.

새로운 할인 정책이 추가 될 때마다 인스턴스 생성, 상태 복사, 식별자 관리코드 추가하는 일은 번거로움과 오류가 발생하기도 쉽다. 이 경우 코드의 복잡성이 높아지더라도 할인 정책의 변경을 쉽게 수용할 수 있게 코드를 유연하게 만드는게 더 좋은 방법이다.

해결 방법은 상속 대신 **합성**을 이용하는 것이다. `Movie`의 상속 계층 안에 구현된 할인 정책을 독립적인 `DiscountPolicy`로 분리한 후 `Movie`에 합성시키면 유연한 설계가 된다. 이것이 2장에서 봤던 영화 예매 시스템의 전체 구조다.

금액 할인 정책이 적용된 영화를 비율 할인 정책으로 바꾸는 일은 `Movie`에 연결된 `DiscountPolicy`의 인스턴스를 교체하는 단순한 작업으로 바뀐다.

```java
Movie movie = new Movie("타이타닉", Duration.ofMinutes(120), Money.wons(10000), new AmountDiscountPolicy(...));
movie.changeDiscountPolicy(new PercentDiscountPolicy(...));
```

새로운 클래스를 추가하고 클래스의 인스턴스를 `Movie`의 `changeDiscountPolicy`메서드에 전달한다.

유연성의 정도에 따라 결합도를 조절할 수 있는 능력은 객체지향 개발자가 갖춰야 하는 중요한 기술 중 하나다.

책임을 할당하고 유연성을 기반으로 설계를 리팩터링 해나가면서 2장의 코드 구조에 도달했다. 

### 04 책임 주도 설계의 대안

책임 주도 설계는 노력과 시간이 필요하다. 책임 관점에서 사고하기 위해서는 충분한 경험과 학습이 필요하다. 

저자의 개인적 추천은 최대한 빠르게 목적한 기능을 수행하는 코드를 작성하는 것이다. 아무것도 없는 상태에서 책임과 협력에 관해 고민하기 보다 일단 실행하는 코드를 얻고 코드상에서 명확하게 드러나는 책임들을 올바른 위치로 이동시키는 것이다.

주의할 점은 코드를 수정한 후 겉으로 드러나는 동작은 바뀌면 안된다. 캡슐화를 향상시키고, 응집도를 높이고, 결합도를 낮춰야 하지만 동작은 그대로 유지해야한다. 이해하기 쉽고 수정하기 쉬운 SW로 개선하기 위해 겉으로 보이는 동작은 바꾸지 않고 내부를 변경하는 것을 **리팩토링(Refactoring)** 이라고 부른다.

리팩터링의 장점

**메서드 응집도**

영화 예매를 처리하는 모든 절차는 `ReservationAgency`에 집중되어있다. 따라서 `ReservationAgency`에 포함된 로직들을 적절한 객체의 책임으로 분배하면 책임 주도 설계와 거의 유사한 결과를 얻을 수 있다.

```java
public class ReservationAgency {
    public Reservation reserve(Screening screening, Customer customer, int audienceCount) {
        Movie movie = screening.getMovie();

        boolean discountable = false;
        for(DiscountCondition condition: movie.getDiscountConditions()) {
            if(condition.getType() == DiscountConditionType.PERIOD) {
                discountable = screening.getWhenScreened().getDayOfWeek().equals(condition.getDayOfWeek()) &&
                        condition.getStartTime().compareTo(screening.getWhenScreened().toLocalTime()) <= 0 &&
                        condition.getEndTime().compareTo(screening.getWhenScreened().toLocalTime()) >= 0;
            } else {
                discountable = condition.getSequence() == screening.getSequence();
            }

            if(discountable) {
                break;
            }
        }

        Money fee;
        if(discountable) {
            Money discountAmount = Money.ZERO;
            switch (movie.getMovie()) {
                case AMOUNT_DISCOUNT:
                    discountAmount = movie.getDiscountAmount();
                    break;
                case PERCENT_DISCOUNT:
                    discountAmount = movie.getFee().times(movie.getDiscountPercent());
                    break;
                case NONE_DISCOUNT:
                    discountAmount = Money.ZERO;
                    break;
            }

            fee = movie.getFee().minus(discountAmount).times(audienceCount);
        } else {
            fee = movie.getFee().times(audienceCount);
        }
        return new Reservation(customer, screening, fee, audienceCount);
    }
}

```

`reserve`메서드는 길이가 길고 이해가 어렵다. 긴 메서드는 다양한 측면에서 코드의 유지 보수에 부정적인 영향을 미친다. 

- 어떤 일을 수행하는지 한눈에 파악이 어려워 코드를 전체적으로 이해하는데 시간이 오래 걸린다.
- 하나의 메서드 안에 너무 많은 작업을 처리해 변경이 필요할 때 수정해야 할 부분을 찾기 어렵다.
- 메서드 내부의 일부 로직만 수정해도 메서드의 나머지 부분에서 버그가 발생할 확률이 높다.
- 로직의 일부만 재사용이 불가능하다.
- 코드를 재사용하는 유일한 방법은 원하는 코드를 복사해 붙여넣는 것이므로 코드 중복을 초래하기 쉽다.

→ 긴 메서드는 응집도가 낮아 이해하기 어렵고 재사용이 어려우며 변경하기 어렵다. 이를 몬스터 메서드(Monster Method)라고 한다.

이런 메서드엔 주석이 길게 들어가는데, 주석을 추가하는 대신 각 메서드를 작게 분해해 응집도를 높이자.

메서드 응집도 또한 변경과 관련이 깊다. 응집도 높은 메서드는 변경되는 이유가 하나여야 한다. 클래스가 작고 목적이 명확한 메서드로 이뤄진다면 변경을 처리하기 위해 어떤 메서드를 수정해야할지 쉽게 판단 할 수있다. 메서드의 크기가 적고 목적이 분명해 재사용도 쉽다. 작은 메서드는 주석과 비슷해보이기 때문에 코드 이해도 쉽다.

`ReservationAgency`를 응집도 높은 메서드로 분해한 결과다.

```java
public class ReservationAgency {
    public Reservation reserve(Screening screening, Customer customer, int audienceCount) {
        boolean discountable = false;
        Money fee = calculateFee(screening, discountable, audienceCount);
        return createReservation(screening, customer, audienceCount, fee);
    }

    private boolean checkDiscountable(Screening screening) {
        return screening.getMovie().getDiscountConditions().stream()
            .anyMatch(condition -> isDiscountable(condition, screening));
    }

    private boolean checkDiscountable(Screening screening) {
        return screening.getMovie().getDiscountConditions().stream()
            .anyMatch(condition -> isDiscountable(condition, screening));
    }

    private boolean isDiscountable(DiscountCondition condition, Screening screening) {
        if(condition.getType() == DiscountConditionType.PERIOD) {
            return isSatisfiedByPeriod(condition, screening);
        }

        return isSatisfiedBySequence(condition, screening);
    }

    private boolean isSatisfiedByPeriod(DiscountCondition condition, Screening screening) {
        return condition.getWhenScreened().getDayOfWeek().equals(condition.getDayOfWeek()) &&
            condition.getStartTime().compareTo(screening.getWhenScreened().toLocalTime()) <= 0 &&
            condition.getEndTime().compareTo(screening.getWhenScreened().toLocalTime()) >= 0;
    }

    private Money calculateFee(Screening screening, boolean discountable, int audienceCount) {
        if(discountable) {
            return screening.getMobie().getFee()
                .minus(calculateDiscountedFee(screening.getMovie()))
                .times(audienceCount);
        }
        return screening.getMovie().getFee().times(audienceCount);
    }

    private Money calculateDiscountedFee(Movie movie) {
        switch (movie.getMovieType()) {
            case AMOUNT_DISCOUNT:
                return calculateAmountDiscountedFee(movie);
            case PERCENT_DISCOUNT:
                return calculatePercentDiscountedFee(movie);
            case NONE_DISCOUNT:
                return calculateNoneDiscountFee(movie);
        }

        throw new IllegalArgumentExcpetion()
    }

    private Money calculateAmountDiscountedFee(Movie movie) {
        return movie.getDiscountAmount();
    }

    private Money calculatePercentDiscountedFee(Movie movie) {
        return movie.getFee().times(movie.getDiscountPercent());
    }

    private money calculateNoneDiscountFee(Movie movie) {
        return Money.ZERO;
    }

    private Reservation createReservation(Screening screening, Customer customer,
                                         inst audienceCount, Money fee) {
        return new Reservation(customer, screening, fee, audienceCount);
    }
} 
```

`ReservationAgency`는 하나의 작업만 수행하고, 하나의 변경 이유만 가지는 작고 명확하고 응집도 높은 메서드로 구성되어있다. 클래스의 길이는 더 길어졌지만, 더 명확해졌다.

`reserve`는 수정 전은 목적을 알기 어려웠지만, 수정 후 메서드가 어떤 일을 하는지 한눈에 알아보기 쉬워졌다.

수정 후는 전체적인 흐름 이해하기 쉽고, 변경하기도 쉬우며 명확하다.

`ReservationAgency`의 응집도는 여전히 낮아 변경의 이유가 다른 각 메서드를 적절한 위치로 분배해야한다.

**객체를 자율적으로 만들자**

어떤 메서드를 어떤 클래스로 이동시켜야 할지는 객체가 자율적인 존재여야 한다는 사실을 떠올리면 쉽다. 메서드가 사용하는 데이터를 저장하고 있는 클래스로 메서드를 이동시키자.

어떤 데이터를 사용하는지를 가장 쉽게 알 수 있는 방법은 메서드 안에서 어떤 클래스의 접근자 메서드를 사용하는지 파악하는 것이다.

`ReservationAgency` 의 `isSatisfiedBySequence`나 `isSatistiedByPeriod` 는`DiscountCondition`에 속한 데이터를 주로 이용하므로 두 메서드를 `DiscountCondition`으로 옮기자.

`DiscountCondition`의 `isDiscountable`은 외부에서 호출가능해야 하므로 `private`에서 `public`으로 바꾼다. `isDiscountable`은 원래는 인자를 받아 계산했지만, `DiscountCondition`의 일부가 되어서 인자로 전달 받을 필요가 없다.

`DiscountCondition`내부에서만 인스턴스 변수에 접근하므로, 모든 접근자를 제거할 수 잇다. 이를 통해 `DiscountCondition` 를 캡슐화 할 수 있다. `ReservationAgency`는 할인 여부를 판단하기 위해 `DiscountCondition`의 `isDiscountable`을 호출하도록 변경된다.

```java
public class ReservationAgency {
	private boolean checkDiscountable(Screening screening) {
		return screening.getMovie().getDiscountConditions().scream()
				    .anyMatch(condition -> condition.isDiscountable(screening));
	}
}
```

위 코드는 책임 주도 설계 방법을 적용해 구현한 `DiscountCondition`의 초기모습과 유사해졌다. 여기에 POLYMORPHISM과 PROTECTED VARIATIONS를 적용하면 최종 설계와 유사한 모습이 될 것이다.

여기서의 요지는 처음부터 책임 주도 설계를 따르기보다 동작하는 코드를 작성하고 리팩터링하는 것이 더 훌륭한 결과를 낳을 수 있다는 것이다. 훌륭한 객체지향 원칙을 따르기 위해 노력하면 책임 주도 설계 방법을 따르지 않아도 유연하고 깔끔한 코드를 얻을 수 있다.
