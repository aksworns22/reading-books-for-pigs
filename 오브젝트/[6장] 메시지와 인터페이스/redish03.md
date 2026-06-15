객체 지향이 클래스가 중요하다고 말하지만, 중요한 도구일 뿐이다. 이 도구에 지나치게 집착하면 유연하지 못하게 설계를 짠다. 객체 지향에서 중요한 건, 말 그대로 “객체를 지향하는 것”이다.

애플리케이션은 클래스로 구성되지만, 메세지로 정의됨을 기억하라. 

### 01 협력과 메시지

**클라이언트-서버 모델**

메시지는 객체 사이의 협력을 가능하게 하는 매개체다. 객체가 다른 객체에 접근할 방법은 메시지를 전송하는 것이다. 메시지를 매개로 하는 요청과 응답의 조합이 두 객체 사이의 협력을 구성하게 된다.

클라이언트-서버(Client-Server) 모델은 두 객체 사이의 협력 관계를 설명한다. 메시지를 전송하는 객체를 클라이언트, 수신하는 객체를 서버라고 부른다. 

!image.png

`Screening`은 *가격을 계산하라* 라는 메시지를 보내고, `Movie`는 이를 수신해 가격을 계산하는 서비스를 제공해 응답한다.

!download.png

`Movie`는 최종 요금을 계산하기 위해서는 할인 요금이 필요하지만 정보가 부족하다. `Movie`는 할인 요금을 계산하라 라는 메세지를 `DiscountPolicy`의 인스턴스에 전송해 할인 요금을 반환 받는다. `Movie` 는 여기서 클라이언트 역할이다.

`Movie`와 같이, 서버와 클라이언트 역할을 동시에 수행하는 것이 일반적이다.

요점은, 객체가 독립적으로 수행할 수 있는 것보다 더 큰 책임을 수행하기 위해서는 다른 객체와 협력해야 한다는 것이다. 이 협력을 가능하게 하는 것이 메시지다.

**메시지와 메시지 전송**

메시지→객체들이 협력하기 위해 사용할 수 있는 유일한 의사소통 수단

메시지 전송 또는 패싱(message sending, passing) → 한 객체가 다른 객체에게 도움을 요청하는 것

메시지 전송자(message sender) → 메시지를 전송하는 객체

메시지 수신자(message receiver) → 메시지 수신 객체 

메시지는 오퍼레이션 명(operation name)과 인자(argument)로 구성되며, 메시지 전송은 여기에 메시지 수신자를 추가한 것이다.

**메시지와 메서드**

메시지를 수신했을 때 실제로 어떤 코드가 실행되는지는 수신자의 실제 타입이 무엇인가에 달려있다.

```java
condition.isSatisfiedBy(screening)
```

수신자인 `condition`은 `DiscountCondition` 이라는 인터페이스 타입으로 정의되어있지만, 실제로 실행되는 코드는 실체화한 클래스의 종류에 따라 달라진다. `PeriodCondition`의 인스턴스라면 그에 맞는 `isSatisfiedBy`가, `SequenceCondition`의 인스턴스면 그에 맞는 메서드가 실행될 것이다.

이처럼 메시지를 수신했을 때 실제로 실행되는 함수 또는 프로시저를 메서드라고 한다. 코드상에서는 같아 보여도, 실제로 실행되는 메서드는 달라질 수 있는 것이다. 전통적인 개발에서는 코드의 의미가 컴파일과 실행 시점이 같지만, 객체 지향에서의 관점에선 다를 수 있다.

메시지와 메서드의 구분은 메시지 전송자와 수신자가 느슨하게 결합하게 해준다. 전송자는 어떤 메세지를 전송해야 하는지 알면 되고, 수신자는 어떤 클래스의 인스턴스인지, 어떤 방식으로 했는지는 몰라도 된다.

**퍼블릭 인터페이스와 오퍼레이션**

퍼블릭 인터페이스 → 객체가 의사소통을 위해 외부에 공개하는 메시지의 집합

오퍼레이션 → 퍼블릭 인터페이스에 포함된 메시지. 수행 가능한 어떤 행동에 대한 추상화로, 내부 구현 코드는 제외하고 단순히 메세지와 관련된 시그니처를 말하는 경우가 대부분.

앞의 예로 든 `DiscountCondition` 인터페이스에 정의된 `isSatisfiedBy`가 오퍼레이션이다.

메서드 → 실제로 실행되는 코드

인터페이스와 메시지의 관점에서 보면 ‘메서드 호출’ 보다는 ‘오퍼레이션 호출’이 더 적절할 것이다.

!메시지, 오퍼레이션, 메서드 사이의 관계

메시지, 오퍼레이션, 메서드 사이의 관계

**시그니처**

오퍼레이션(또는 메서드)의 이름과 파라미터 목록을 합쳐 시그니처(Signature)라고 부른다. 오퍼레이션은 실행 코드 없이 시그니처만 정의한 것이다. 메서드는 시그니처에 구현을 더한 것이다.

하나의 오퍼레이션에 하나의 메서드만 존재하면 단순해진다. 이런 경우 두 개를 구분 할 필요 없지만, 다형성을 위해서는 하나의 오퍼레이션에 다양한 메서드를 구현해야한다.

중요한 것은 객체가 수신할 수 있는 메시지가 퍼블릭 인터페이스와 안에 포함될 오퍼레이션을 결정한다는 것이다.

### 02 인터페이스와 설계 품질

“좋은 인터페이스”

1. 최소한의 인터페이스 → 꼭 필요한 오퍼레이션만 포함
2. 추상적인 인터페이스 → 어떻게 수행하는지가 아니라, 무엇을 하는지를 표현함.

이를 만들기 위해서는 책임 주도 설계를 따르는 것이다. 또한 아래의 기법과 원칙을 따르는 것이다.

퍼블릭 인터페이스의 품질에 영향을 미치는 원칙과 기법

1. 디미터 법칙
2. 묻지 말고 시켜라
3. 의도를 드러내는 인터페이스
4. 명령-쿼리 분리

**디미터 법칙**

4장에서 들고온 절차적 방식의 예매 시스템 코드 중 할인 가능 여부를 체크하는 코드다.

```java
public class ReservationAgency {
    public Reservation reserve(Screening screening, Customer customer, int audienceCount) {
        Movie movie = screening.getMovie();

        boolean discountable = false;
        for(DiscountCondition condition: mobie.getDiscountCondition()) {
            if(condition.getType() == DiscountConditionType.PERIOD) {
                discountable = screening.getWhenScreened().getDayOfWeek().equals(condition.getDayOfWeek()) &&
                    condition.getStartTime().compareTo(screening.getWhenScreened().toLocalTime()) <= 0 &&
                    condition.getEndTime().compareTo(screening.getWhenScreened().toLocalTime()) >=0;
            } else {
                discountable = condition.getSequence == screening.getSequence();
            }

            if(discountable) {
                break;
            }
        }
        // ...
    }
}
```

위 코드의 가장 큰 단점은 `ReservationAgency`와 `Screening`의 결합도가 크다는 것이다. 원인은 `ReservationAgency`가 `Screening`뿐만 아니라 `movie`와 `DiscountCondition`에도 직접 접근하기 때문이다. 이는 각 클래스가 다 조금이라도 바뀌면 `ReservationAgency`는 크게 흔들리는 결과를 맞는다.

디미터 법칙(Law of Demeter)

- 객체의 구조에 대한 결합으로 인해 발생하는 설계 문제를 해결하기 위해 생긴 원칙
- 객체 내부 구조에 강하게 결합되지 않도록 협력 경로를 제한하기.
- “낯선 자에게 말하지 않기”, “인접한 이웃하고만 말하라”와 같은 말로도 표현됨.
- 자바, C#과 같이 도트(`.`)를 이용해 메세지를 전송을 표현하는 언어에서는 “오직 하나의 도트만 사용하라”와 같이 불린다.

디미터 법칙 → 클래스가 특정 조건을 만족하는 대상에게만 메시지를 전송. 

모든 클래스 C에 구현된 모든 메서드 M 에 대해서 M이 메시지를 전송할 수 있는 모든 객체는 아래와 같은 클래스의 인스턴스여야 한다. (M에 의해 생성된 객체나 M이 호출하는 메서드에 의해 생성된 객체, 전역변수로 선언된 객체는 모두 M의 인자로 간주한다.)

- M의 인자로 전달된 클래스 (C자체 포함)
- C의 인스턴스 변수의 클래스

좀 어려울 수 있는데 아래 조건을 만족하는 인스턴스에만 메세지를 전송하도록 하는거라고 이해하면 된다.

- `this`객체
- 메서드의 매개변수
- `this`의 속성
- `this`의 속성인 컬렉션의 요소
- 메서드 내에서 생성된 지역 객체

4장에서 결합도 문제를 해결하기 위해 수정한 `ReservationAgency` 의 최종 코드를 보자

```java
public class ReservationAgency {
	public Reservation reserve(Screening screening, Customer customer, int audienceCount) {
		Money fee = screening.calculateFee(audienceCount);
		return new Reservation(customer, screening, fee, audienceCount);
	}
}
```

`ReservationAgency` 는 메서드의 인자로 전달된 `Screening` 인스턴스에게만 메시지를 전송한다. `ReservationAgency`가 `Screening`의 내부 구조에 결함돼 있지 않아 `Screening`의 구현을 변경할 때 `ReservationAgency` 를 변경할 필요 없다.

디미터 법칙을 따르는 코드는 메시지 수신자의 내부 구조가 전송자에게 노출되지 않으며, 메시지 전송자는 수신자의 내부 구현에 결합되지 않는다. 따라서 클라이언트와 서버 사이에 낮은 결합도를 유지할 수 있다.

```java
screening.getMovie().getDiscountConditions();
```

전송자가 수신자의 내부 구조에 대해 묻고 반환받은 요소에 대해 연쇄적으로 메세지를 전송한다. 이를 기차 충돌(train wreck)이라고 부른다. 이는 내부 구현이 외부로 노출 됐을 때 나타나는 전형적 형태로 전송자는 수신자의 내부 정보를 알게 된다. 따라서 캡슐화는 무너지고, 전송자가 수신자의 내부 구현에 강력하게 결합된다.

```java
screening.calculateFee(audienceCount); 
```

이는 객체가 자기 자신을 책임지는 자율적인 존재여야 한다는 사실을 강조한다. 정보를 처리하는데 필요한 책임을 정보로 알고 있는 객체에게 할당해 응집도가 높은 객체가 만들어진다.

그러나…무비판적으로 디미터 법칙을 수용하면 퍼블릭 인터페이스 관점에서 객체의 응집도가 낮아질 수도 있다. 나중의 “원칙의 함정”을 읽자.

**묻지 말고 시켜라**

`ReservationAgency`는 `Screening` 의 내부 `Movie`에 접근하는 대신 직접 요금을 계산하도록 했다. 디미터 법칙은 훌륭한 메세지는 객체의 상태에 관해 묻지 않고 원하는 것을 시켜야함을 강조한다. 묻지 말고 시켜라는 이런 스타일을 장려한다.

구현 로직은 메세지 수신자가 담당해야할 책임이다. 객체의 외부에서 해당 객체의 상태를 기반으로 결정내리는 건 캡슐화 위반이다.

> 절차적인 코드는 정보를 얻은 후에 결정한다. 객체지향 코드는 객체에게 그것을 하도록 시킨다.
> 

이를 따르면 객체의 정보를 이용하는 행동을 객체 외부가 아닌 내부에 위치시키므로, 자연스럽게 정보와 행동을 동일한 클래스 안에 두게 된다. 메세지를 결정하다 보면 자연스럽게 정보 전문가에게 책임을 할당하게 되고 높은 응집도를 가진 클래스를 가진 확률이 높아진다.

그러나 객체에게 시킨다고 끝이 아니다. 어떻게 작업을 수행하는지 노출시키면 안되고, 객체가 무엇을 하는지를 인터페이스는 서술해야한다.

**의도를 드러내는 인터페이스**

메서드 이름 짓는 방법 두 가지 (켄트 벡)

1. 메서드가 작업을 어떻게 수행하는지 나타내도록 짓기
    1. `isSatisfiedByPeriod(Screening screening)`, `isSatisfiedBySequence(Screening screening)`
    2. 이런 스타일은 메서드에 대해 제대로 커뮤니케이션 하지 못한다. 둘 다 할인 조건을 판단하지만 이름이 달라 내부 구현을 정확히 이해하지 못하면 두 메서드가 같은 작업을 하는걸 모른다.
    3. 메서드 수준에서 캡슐화를 위반한다. 클라이언트로 하여금 협력하는 객체의 종류를 알도록 강요한다.
2. ‘어떻게’가 아닌, ‘무엇’을 하는지 드러내기
    1. 위의 두 메서드를 각각으로 보는게 아닌, `isSatisfiedBy()`로 만들기. 
    2. 변경된 메서드 명은 서로가 동일한 목적을 갖는다는 것을 메서드 이름을 통해 명확히 표현한다. 
    3. 자바와 같은 정적 언어에서 단순히 메서드 이름이 같다고 동일한 메세지를 처리하는 것은 아니다. 두 메서드를 가진 객체를 동일한 타입으로 간주 할 수 있도록 동일한 타입 계층으로 묶어야 한다. `DiscountCondition`인터페이스를 정의하고 `isSatisfiedBy`메서드를 정의하자.

```java
public interface DiscountCondition {
	boolean isSatisfiedBy(Screening screening)
}

public class PeriodCondition implements DiscountCondition {
	public boolean isSatisfiedBy() ...
}

...
```

이렇게 무엇을 하느냐에 따라 메서드 이름을 짓는 것을 의도를 드러내는 선택자(Intention Revealing Selector)라고 한다. 아래는 그 방법이다.

1. 매우 다른 두 번째 구현을 상상한다.
2. 해당 메서드에 동일한 이름을 붙인다 상상하기

→ 그 순간에 할 수 있는 한 가장 추상적인 이름을 메서드에 붙이게 될 것이다.

의도를 드러내는 선택자를 인터페이스 레벨로 확장한 것을 의도를 드러내는 인터페이스(Intention Revealing Interface)라고 한다. 이는 구현과 관련된 모든 정보를 캡슐화하고 객체의 퍼블릭 인터페이스에는 협력과 관련된 의도만 표현하는 것이다.

객체에게 묻지 말고 시키되, 구현 방법이 아닌 클라이언트의 의도를 드러내야 한다.

**함께 모으기**

디미터 법칙, 묻지 말고 시켜라 스타일, 의도를 드러내는 인터페이스를 이해할 수 있는 방법 중 하나는 원칙을 위반하는 코드를 살펴보는 것이다.

디미터 법칙을 위반하는 티켓 판매 도메인

`Theater`의 `enter`은 디미터 법칙을 위반하는 모습을 보여준다.

```java
public class Theater {
    private TicketSeller ticketSeller;

    public Theater(TicketSeller ticketSeller) {
        this.ticketSeller = ticketSeller;
    }

    public void enter(Audience audience) {
        if(audience.getBag().hasInvitation()) {
            Ticket ticket = ticketSeller.getTicketOffice().getTicket();
            audience.getBag().setTicket(ticket);
        } else {
            Ticket ticket = ticketSeller.getTicketOffice().getTicket();
            audience.getBag().minusAmount(ticket.getFee());
            ticketSeller.getTicketOffice().plusAmount(ticket.getFee());
            audience.getBag().setTicket(ticket);
        }
    }
}
```

- `Theater`가 `audience`뿐만 아니라 내부의 `Bag`에도 메세지를 전송한다. 결과적으로 `Theater`가 `Audience`의 퍼블릭 인터페이스 뿐만 아니라 내부 구조에도 결합한다.
- 근본적으로 디미터 법칙 위반 → 인터페이스와 구현의 분리 원칙 위반
- 사용하기 어려움. `Audience`의 퍼블릭 인터페이스 뿐만 아니라 내부 구조들도 다 알고 있어야 하기 때문.

묻지 말고 시켜라

`Theater`는 `TicketSeller`과 `Audience`의 내부 구조에 관해 묻지 말고 원하는 작업을 시켜야 한다. 즉, `TicketSeller`과 `Audience`는 묻지 말고 시켜라를 따르는 퍼블릭 인터페이스를 가져야 한다. `Theater`가 `TicketSeller`에게 자신이 원하는 일을 시키도록 수정하는것이다. 그 내용은 `Audience`가 `Ticket`을 가지도록 하는 것이다. `setTicket`을 추가하고, `enter`로직을 옮기자.

```java
public class TicketSeller {
    public void setTicket(Audience audience) {
        if(audience.getBag().hasInvitation()) {
            Ticket ticket = ticketOffice.getTicket();
            audience.getBag().setTicket(ticket);
        } else {
            Ticket ticket = ticketOffice.getTicket();
            audience.getBag().minusAmount(ticket.getFee());
            ticketOffice.plusAmount(ticket.getFee());
            audience.getBag().setTicket(ticket);
        }
    }
}
```

`Theater`은 자신의 속성으로 포함하고 있는 `TicketSeller`인스턴스 에게만 메세지를 전송하게 됐다.

```java
public class Theater {
	public void enter(Audience audience) {
		ticketSeller.setTicket(audience);
	}
}
```

`TicketSeller`가 원하는 것은 `Audience`가 `Ticket` 을 보유하도록 만드는 것이다. `Audience`에 `setTicket`을 추가하고 스스로 티켓을 가지도록 하자

```java
public class Audience {
	public Long setTicket(Ticket ticket) {
		if(bag.hasInvitation()) {
			bag.setTicket(ticket);
			return 0L;
		} else {
			bag.setTicket(ticket);
			bag.minusAmount(ticket.getFee());
			return ticket.getFee();
		}
	}
}
```

- `TicketSeller`는 속성으로 포함한 `TicketOffice`의 인스턴스와 인자로 전달된 `Audience`에게만 메세지를 전송한다.
- `TicketSeller`역시 디미터 법칙을 준수한다.

`Audience`는 여전히 디미터 법칙을 위반한다.

`Audience`의 `setTicket`메서드 구현을 `Bag`으로 옮기자.

```java
public class Bag {
	public Long setTicket(Ticket ticket) {
		if(hasInvitation()) {
			this.ticket = ticket;
			return 0L;
		} else {
			this.ticket = ticket;
			minusAmount(ticket.getFee());
			return ticket.getFee();
		}
	}
	
	private boolean hasInvitation() {
		return invitation != null;
	}
	
	private void minusAmount(Long amount) {
		this.amount -= amount;
	}
}
```

`Audience`의 `setTicket`이 `Bag`의 `setTicket`을 호출하도록 하지 말고 묻지말고 시켜라 스타일을 따르고 디미터 법칙을 준수하는 `Audience`를 얻을 수 있다.

```java
public class Audience {
	public Long setTicket(Ticket ticket) {
		return bag.setTicket(ticket)
	}
}
```

인터페이스에 의도를 드러내자

현재 인터페이스는 클라이언트의 의도를 명확하게 드러내지 못한다.

`TicketSeller`의 `setTicket`은 클라이언트의 의도를 명확히 드러내는가? `Audience`의 `setTicket`은? `Bag` 의 `setTicket`은?

직접 개발한 사람은 의도가 다른 걸 알고 있지만, 퍼블릭 인터페이스를 해석하고 사용하는 사람은 혼동이 오기 쉽다.각각의 입장에서 `setTicket`이름은 의도가 명확하지 않다. 

- `Theater` → `Audience`에게 티켓을 판매하는 것. 따라서 `setTicket` 보다는 `sellTo` 가 명확하다.
- `TicketSeller` → `Audience`가 티켓을 사도록 만드는 것이 목적, `buy`가 명확하다.
- `Audience` → `Bag`이 티켓을 보관하도록 하는 것이 목적. `hold`가 더 명확하다.

각 수정 사항을 퍼블릭 인터페이스를 통해 명확히 드러내자

```java
public class TicketSeller {
	public void sellTo(Audience audience) {...}
}

public class Audience {
	public Long buy(Ticket ticket) {...}
}

public class Bag {
	public Long hold(Ticket ticket) {...}
}
```

이 예제는 오퍼레이션의 이름을 짓는 법에 관한 지침을 제공한다. `sellTo`, `buy`, `hold`는 무엇을 원하는지 명확하게 표현한다. `setTicket`은 그렇지 않다.

결합도가 낮으면서도 의도를 명확히 드러내는 간결한 협력을 원한다. 디미터 법칙과 묻지 말고 시켜라 스타일, 의도를 드러내는 인터페이스가 우리를 도울 것이다. 

> 정보 은닉 말고도 “묻지 말고 시켜라”스타일에는 좀 더 미묘한 이점이 있다. 이 스타일은 객체 간 상호작용을 getter의 체인 속에 암시적으로 두지 않고 좀 더 명시적으로 만들고 이름을 가지도록 강요한다.
> 

### 03 원칙의 함정

디미터 법칙, 묻지말고 시켜라 스타일은 퍼블릭 인터페이스를 깔끔 유연하게 만드는 설계 원칙이지만, 절대적이진 않다. 법칙에는 예외는 없지만 원칙에는 예외가 많다.

설계는 트레이드 오프의 산물이다. 설계를 적절하게 트레이드 오프 할 줄 알아야한다.

원칙이 상황과 부적합 하다 판단되면 과감하게 원칙을 무시하라. 원칙을 아는 것보다 원칙이 언제 유용하고 유용하지 않은지 판단하는게 중요하다.

**디미터 법칙은 하나의 도트(`.`)를 강제하는 규칙이 아니다.**

“오직 하나의 도트만을 사용하라” 라는 말로 디미터 법칙을 소개했지만, 무조건은 아니다.

`IntStream`을 사용한 코드를 한번 보자.

```java
IntStream.of(1, 15, 20, 3, 9).filter(x -> x > 10).distinct().count();
```

이는 디미터 법칙을 위반한 것처럼 보이지만, 아니다. `of`, `filter`, `distinct`메서드는 모두 `IntStream`이라는 동일한 클래스의 인스턴스를 반환한다.

따라서 이 코드는 디미터 법칙 위반이 아니다. 디미터 법칙은 결합도와 관련된 것이다. `IntStream` 의 내부 구조가 밖으로 노출 된게 아닌 다른 `IntStream`으로 치환 되는 것일 뿐이고 객체를 둘러싸고 있는 캡슐은 그대로 유지된다.

과연 여러 개의 도트를 사용한 코드가 객체 내부 구조를 노출하는지, 스스로 한번 생각해보는게 좋다.

**결합도와 응집도의 충돌**

`Theater`의 `enter`을 보자.

```java
public void enter(Audience audience) {
    if(audience.getBag().hasInvitation()) {
        Ticket ticket = ticketSeller.getTicketOffice().getTicket();
        audience.getBag().setTicket(ticket);
    } else {
        Ticket ticket = ticketSeller.getTicketOffice().getTicket();
        audience.getBag().minusAmount(ticket.getFee());
        ticketSeller.getTicketOffice().plusAmount(ticket.getFee());
        audience.getBag().setTicket(ticket);
    }
}
```

`Theater`은 `Audience` 내부에 결합된 `Bag`에 질문 후 반환 결과로 `Bag`을 변경해 `Theater` 가 `Audience`의 구현에 강하게 결합한다. `Audience`에게 위임 메서드를 추가하자.

```java
public class Audience {
	public Long buy(Ticket ticket) {
		if(bag.hasInvitation()) {
			bag.setTicket(ticket);
			return 0L;
		} else {
			bag.setTicket(ticket);
			bag.minusAmount(ticket.getFee());
			return ticket.getFee();
		}
	}
}
```

`Audience`는 상태와 함께 조작을 하기 때문에 응집도가 높아졌다.

하지만 묻지 말고 시켜라와 디미터 법칙을 준수하는게 항상 긍정적 결과로 이어지는게 아니다. 모든 상황에서 맹목적으로 위임 메서드를 추가하면 퍼블릭 인터페이스 안에 어울리지 않는 오퍼레이션이 공존한다. 결과적으로 객체는 상관없는 책임들을 한번에 떠안게 되어 결과적으로 응집도가 낮아진다.

영화 예매 시스템의 `PeriodConditon` 을 보자. `isSatisfiedBy`는 `screening`에게 질의해 상영 시작 시간을 시용해 할인 여부를 결정한다. 얼핏 보면 캡슐화 위반 처럼 보인다.

```java
public class PeriodCondition implements DiscountCondition {
	public boolean isSatisfiedBy() {
		return screening.getStartTime().getDayOfWeek().equals(dayOfWeek) &&
			startTime.compareTo(screening.getStartTime().toLocalTime()) <= 0 &&
			endTime.compareTo(screening.getStartTime().toLocalTime()) >= 0;
	}
}
```

따라서 판단 로직을 `Screening` 의 `isDiscountable` 로 옮기고 `PeroidCondition`이 메서드를 호출하도록 변경하면 묻지 말고 시켜라 스타일을 준수하는 퍼블릭 인터페이스를 얻을 수 있다고 생각할 수 있다.

```java
public class Screening {
	public boolean isDiscountable(DayOfWeek dayOfWeek, LocalTime startTime, LocalTime endTime) {
		return whenScreened.getDayOfWeek().equals(dayOfWeek) &&
			startTime.compareTo(whenScreened.toLocalTime()) <= 0 &&
			endTime.compareTo(whenScreened.toLocalTime()) >= 0; 
	}
}

public class PeriodCondition implements DiscountCondition {
	public boolean isSatisfiedBy() {
		return screening.getStartTime().getDayOfWeek().equals(dayOfWeek) &&
			startTime.compareTo(screening.getStartTime().toLocalTime()) <= 0 &&
			endTime.compareTo(screening.getStartTime().toLocalTime()) >= 0;
	}
}
```

하지만 이렇게 하면 `Screening`이 기간에 따른 할인 조건을 판단하는 책임을 안게 된다. 이는 `Screening`이 담당하는 본질적 책임은 아니다. 본질적 책임은 영화를 예매하는 것이다. 반면 `PeriodCondition`입장에서는 할인 조건을 판단하는 책임이 본질적이다.

그리고 `PeriodCondition`의 인스턴스 변수를 인자로 받았기 때문에 인스턴스 목록이 바뀌면 영향을 받는다. 결합도가 높다. 따라서 `Screening`의 캡슐화를 향상시키는 것보다 `Screening`의 응집도를 높이고 `Screening` 과 `PeriodCondition`의 결합도를 낮추는게 전체적인 관점에서 좋다.

가끔은 묻는 것 말고 다른 방법이 존재하지 않는 경우도 있다. 컬렉션에 포함된 객체들을 처리하는 유일한 방법은 객체에게 물어보는 것이다.

```java
for(Movie each: movies) {
	total+= each.getFee();
}
```

물으려는 객체가 진짜 데이터인 경우도 있다. 로버트 마틴(<클린 코드>)은 디미터 법칙의 위반 여부는 묻는 대상이 객체인지 자료 구조 인지에 달려 있다고 한다. 객체는 내부 구조를 숨겨야 해 디미터 법칙을 따르는게 좋지만, 자료구조라면 당연히 내부를 노출해야 하므로 디미터 법칙을 적용할 필요가 없다.

원칙이 적절한 상황과 부적절한 상황을 판단하는 안목을 기르자.

### 04 명령-쿼리 분리 원칙

명령-쿼리 원칙 (Command-Query Separation)

이 원칙은 퍼블릭 인터페이스에 오퍼레이션을 정의할 때 참고할 수 있는 지침을 제공한다.

- 루틴(routine)→ 어떤 절차를 묶어 호출 가능하도록 이름을 부여한 기능 모듈
    - 프로시저(procedure) → 정해진 절차에 따라 내부의 상태를 변경하는 루틴의 종류
    - 함수(function) → 어떤 절차에 따라 필요한 값을 계산해서 반환하는 루틴의 한 종류
    - 프로시저는 부수효과를 발생시킬 수 있지만, 값을 반환하지 못한다.
    - 함수는 값을 반환할 수 있지만 부수효과를 발생시키지 못한다.

명령(Command)와 쿼리(Query)는 객체의 인터페이스 측면에서 프로시저와 함수를 부르는 또 다른 이름이다. 객체 상태를 수정하는 오퍼레이션이 명령이고, 관련된 정보를 반환하는게 쿼리이다. 어떤 오퍼레이션도 명령인 동시에 쿼리여서는 안된다. 두 가지를 분리하려면 다음 두 규칙을 준수해라.

- 객체의 상태를 변경하는 명령은 반환값을 가질 수 없다.
- 객체의 정보를 반환하는 쿼리는 상태값을 변경할 수 없다.

= “질문이 답변을 수정해서는 안된다.”

버드란트 마이어(<Object-Oriented Software Construction>)는, 객체는 블랙 박스이며, 객체의 인터페이스는 객체의 관찰 가능한 상태를 보기 위한 일련의 디스플레이와 상태를 변경하기 위해 누를 수 있는 버튼의 집합이다. 마틴 파울러는 명령-쿼리 분리 원칙에 따라 작성된 객체 인터페이스를 명령-쿼리 인터페이스라고 부른다.

**반복 일정의 명령과 쿼리 분리하기**

개발팀은 반복되는 이벤트를 관리할 수 있는 SW를 개발하려고 했으나, 출시 직전 출시 일정을 미룰 치명적 버그가 발견됐다. 

도메인 이해

“이벤트(event)” → 특정 일자에 실제로 발생하는 사건

“반복 일정(recurring schedule)” → 일주일 단위로 돌아오는 특정 시간 간격에 발생하는 사건 전체

이벤트 구현

```java
public class Event {
	private String subject;
	private LocalDateTime from;
	private Duration duration;
	
	public Event(String subject, LocalDateTime from, Duration duration) { 
		this.subject = subject;
		...		
	}	
}
```

반복 일정 구현 

→ 주 단위 반복 일정 정의 클래스.

```java
public class RecurringSchedule {
    private String subject;
    private DayOfWeek dayOfWeek;
    private LocalTime from;
    private Duration duration;

    public RecurringSchedule (String subject, DayOfWeek dayOfWeek,
                             LocalTime from, Duration duration) {
        this.subject = subject;
        this.dayOfWeek = dayOfWeek;
        this.from = from;
        this.duration = duration;
    }

    public DayOfWeek getDayOfWeek() {
        return dayOfWeek;
    }
    public LocalTime getFrom() {
        return from;
    }
    public Duration getDuration() {
        return duration;
    }
}
```

개발팀을 당혹시킨 버그에 대해 알아보자.

`Event`클래스는 현재 이벤트가 `RecurringSchedule`이 정의한 반복 일정 조건을 만족하는지 검사하는 `isSatisfied`메서드를 제공한다. 이는 `RecurringSchedule`의 인스턴스를 받아 해당 이벤트가 일정 조건을 만족하면 `true`, 만족 못하면 `false`를 반환한다.

`isSatisfied`메서드. 

- 매주 수요일 10시 반 부터 30분 진행하는 회의 `RecurringEvent` 인스턴스 생성
- 2019년 5월 8일 10시 반 부터 30분 진행하는 동안 진행되는 회의 `Event`인스턴스 생성

```java
RecurringSchedule schedule = new RecurringSchedule("회의", DayOfWeek.WEDNESDAY,
                                                          LocalTime.of(10, 30), Duration.ofMinutes(30));
Event meeting = new Event("회의", LocalDateTime.of(2019, 5, 9, 10, 30), Duration.ofMinutes(30));
    
assert meeting.isSatisfied(schedule) == true;
```

이 코드는 버그가 숨겨져 있다.

5/9일은 목요일이라서, true가 아닌, false를 반환하게 된다. 다시 한 번 `isSatisfied`를 호출하면 `true`를 반환한다. 동일한 `Event`와 `RecurringSchedule`을 이용했는데, 왜 결과는 다를까?

```java
public class Event {
    public boolean isSatisfied(RecurringSchedule schedule) {
        if(from.getDayOfWeek() != schedule.getDayOfWeek() || 
          !from.toLocalTime().equals(schedule.getFrom()) ||
          !duration.equals(schedule.getDuration())) {
            reschedule(schedule);
            return false;
          }

        return true;
    }
}
```

이벤트가 같은지 판단하고 있는데, `reschedule`메서드를 통해서 `RecurringSchedule` 인스턴스를 수정하고 있다. 객체의 상태를 수정하는 것이다.

```java
private void reschedule(RecurringSchedule schedule) {
    from = LocalDateTime.of(from.toLocalTime().plusDays(daysDistance(schedule))),
        schedule.getFrom();
    duration = schedule.getDuration();
}

private long daysDistance(RecurringSchedule schedule) {
    return schedule.getDayOfWeek().getValue() - from.getDayOfWeek().getValue();
}
```

`reschedule`은 `Event`의 일정을 전달된 `RecurringEvent`에 맞게 변경한다.

버그가 찾기 어려웠던 이유는 명령과 쿼리를 동시에 수행하고 있기 때문이다.

- `isSatisfied`는 `Event`가  `RecurringSchedule` 의 조건에 부합 판단 후 부합하면 true, 아니면 false이다. 따라서 `isSatisfied` 는 개념적으로 쿼리다.
- `isSatisfied`는 `Event`가  `RecurringSchedule` 의 조건에 부합 판단 후 부합하지 않으면 `Event`의 상태를 조건에 부합하도록 변경한다. 실제로는 부수효과를 가지는 명령이다.

명령과 쿼리가 섞이면 실행 결과 예측이 어렵다.이를 명확하게 구분하자

```java
public boolean isSatisfied(RecurringSchedule schedule) {
    if(from.getDayOfWeek() != schedule.getDayOfWeek() || 
      !from.toLocalTime().equals(schedule.getFrom()) ||
      !duration.equals(schedule.getDuration())) {
        return false;
      }

    return true;
}

public void reschedule(RecurringSchedule schedule) {
    from = LocalDateTime.of(from.toLocalTime().plusDays(daysDistance(schedule))),
        schedule.getFrom();
    duration = schedule.getDuration();
}
```

- 부수효과가 사라져 순수한 쿼리가 되었다.

반환 값을 돌려주는 메서드는 쿼리이므로 부수 효과가 없어, 몇 번을 호출해도 다른 부분에 영향이 없다. `reschedule`은 `public`으로 바뀌었다. `Event` 가 `RecurringSchedule` 의 조건을 만족하지 못하면 `reschedule` 메서드를 호출할지 결정한다.

```java
if(!event.isSatisfied(schedule)) {
	event.reschedule(schedule);
}
```

수정 전 보다 더 복잡해진 것 같지만, 명령과 쿼리를 분리해 얻는 이점이 더 크다. 부수효과를 가지는 대신 값을 반환하지 않는 명령, 부수 효과가 없는 대신 값을 반환하는 쿼리를 분리하자.

**명령-쿼리 분리와 참조 투명성**

쿼리 → 상태를 변경하지 않아 반복 호출해도 상관없음.

명령과 쿼리를 분리해 명령형 언어의 틀 안에서 **참조 투명성(referential transparency)** 의 장점을 제한적으로 누릴 수 있다. 참조 투명성은 어떤 표현식 `e` 가 있을 때 `e`의 값으로 `e`가 나타나는 모든 위치를 교체하더라도 결과가 달라지지 않는 특성을 말한다.

- 버그가 적다.
- 디버깅이 용이하다.
- 쿼리 순서에 따라 실행 결과가 변하지 않는 코드를 작성할 수 있다.
- 식의 순서를 변경해도 결과가 달라지지 않는다.

수학은 참조 투명성을 엄격히 지키는 유명한 체계이다.

```java
f(1) + f(1) = 6
f(1) * 2 = 6
f(1) - 1 = 5
```

`f(1)`을 3으로 바꿔도 값은 바뀌지 않을 것이다. 이게 바로 참조 투명성이다.

`f(1)`의 값을 항상 3이라고 말할 수 있는 이유는 `f(1)` 의 값이 변하지 않기 때문이다. 이처럼 어떤 값이 변하지 않는 성질을 불변성(immutability)라고 한다.

수학에서 함수는 어떤 값도 변경하지 않기 때문에 부수 효과가 없고, 불변의 세상에서는 모든 로직이 참조 투명성을 만족한다. 불변성은 부수 효과 발생을 방지하고, 참조 투명성을 만족한다.

참조 투명성을 만족하는 식은 두 가지 장점이 있다.

- 모든 함수를 이미 알고 있는 하나의 결과값으로 대체할 수 있어 식을 쉽게 계산 가능하다.
- 모든 곳에서 함수의 결괏값이 동일해 식의 순서를 변경해도 결과가 달라지지 않는다.

객체 지향 패러다임은 객체 상태 변경이라는 부수 효과를 기반으로 해 참조 투명성은 예외에 가깝다. 그러나 명령-쿼리 원칙을 사용하면 부수 효과를 가지지 않는 쿼리를 명백하게 분리해 제한적으로 참조 투명성의 혜택을 누릴 수 있게 해준다.

**책임에 초점을 맞춰라**

디미터 법칙, 묻지 말고 시켜라, 의도를 드러내는 인터페이스 설계하는 방법→ 메세지를 먼저 선택하고 그 후 메세지를 처리할 객체를 선택하는 것

명령, 쿼리 분리하고 계약에 의한 설계 개념을 통해 객체의 협력 방식을 명시적으로 드러내는 방법 → 객체 구현 이전 객체 사이의 협력에 초점을 맞추고 협력 방식을 단순 유연하게 만들기

모든 방식의 중앙에는 객체가 수행할 책임이 있다.

메세지를 먼저 선택하는게 각각의 원칙들에 미치는 긍정적 영향은 아래와 같다.

- 디미터 법칙 : 객체 사이의 구조적 결합도를 낮출 수 있다.
- 묻지 말고 시켜라 : 협력을 구조화 한다.
- 의도를 드러내는 인터페이스 : 전송하는 클라이언트의 관점에서 메세지의 이름을 정해, 클라이언트가 무엇을 원하는지 의도가 분명하게 드러난다.
- 명령-쿼리 원칙 : 객체가 단순히 어떤 일을 해야 하는지 뿐만 아니라 협력 속 객체의 상태를 예측하고 이해하기 쉽게 만들기 위한 방법에 대해 고민한다. 예측 가능한 협력을 만들기 위해 명령과 쿼리를 분리한다.

훌륭한 메세지를 얻기 위한 출발점은 책임 주도 설계 원칙 따르기. 메세지가 객체를 결정하게 하라.

지금까지 원칙들은 구현과 부수 효과를 캡슐화 하고, 높은 응집도와 낮은 결합도를 가진 인터페이스를 만들 수 있는 지침을 제공하지만, 실제로 실행 시점에 필요한 구체적 제약이나 조건을 명확히 하지는 못한다. 협력을 위해 두 객체가 보장해야하는 실행 시점의 제약을 인터페이스에 명시할 수 있는 방법이 존재하지 않는 다는 것이다.

이런 문제를 해결하기 위해 계약에 의한 설계 (Design By Contract)개념을 버트란드 마이어가 제시했다. 협력을 위해 클라이언트와 서버가 준수해야 할 제약을 코드 상에 명시적으로 표현하고 강제할 수 있는 방법이다.

나중의 부록 1장을 보자.
