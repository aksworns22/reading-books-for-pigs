역할, 책임, 협력

협력 → 메세지를 주고받는 객체들 사이의 상호작용

책임 → 객체가 다른 객체와 협력하기 위해 수행하는 행동

역할 → 대체 가능한 책임의 집합

**책임**이 객체지향의 품질을 결정함.

객체 지향 설계란 올바른 객체에 올바른 책임을 할당하면서 낮은 결합도와 높은 응집도를 가진 구조를 창조하는 활동이다. 이에는 두 가지 관점이 있다. 

1. 객체 지향 설계의 핵심은 책임이라는 것
2. 책임을 할당하는 작업이 응집도와 결합도 같은 설계 품질과 연관.

훌륭한 설계란 합리적인 비용 안에서 변경을 수용할 수 있는 구조를 만드는 것

결합도와 응집도를 합리적으로 유지할 수 있는 원칙은, 객체의 상태가 아니라 객체의 행동에 초점을 맞추는 것.

### 01 데이터 중심의 영화 예매 시스템

두 가지 방법으로 시스템을 객체로 분할 가능.

1. 상태를 분할의 중심축으로 삼는 방법
2. 책임을 분할의 중심축으로 삼는 방법

(상태 = 데이터)

데이터 중점의 관점 

- 객체는 자신이 포함한 데이터를 조작하는데 필요한 오퍼레이션 정의
- 객체의 상태에 초점
- 객체를 독립된 데이터 덩어리

책임 중심의 관점

- 다른 객체가 요청할 수 있는 오퍼레이션을 위해 필요한 상태 보관
- 객체의 행동에 초점
- 객체를 협력하는 공동체의 일원

결론은, 데이터가 아닌 책임에 초점. 이유는 변경.

객체의 상태, 데이터 → 구현. 구현은 변하기 쉽다. 인터페이스의 변경을 초래.

책임 → 인터페이스. 캡슐화를 통해 구현 변경에 대한 파장이 외부로 퍼지는 것을 방지

**데이터 준비**

`Movie` 데이터 준비

```java
public class Movie {
		private String title;
		private Duration runningTime;
		private Money fee;
		private List<DiscountCondition> discountConditions;
		
		private MovieType movieType;
		private Money discountAmount;
		private double discountPervent;
}

/*
 기존 설계와 차이점은, 할인 조건 목록이 안에 들어가있다. 할인 정책도 포함하고, 할인 금액과
 할인 비율도 Movie에서 직접 정의한다.
*/
```

- 할인 정책은 하나만 지정할 수 있으니 `discountAmount`와 `discountPercent` 중 하나만 사용 가능하다.

할인 정책의 종류를 결정한다.

```java
public enum MovieType {
		AMOUNT_DISCOUNT,
		PERCENT_DISCOUNT,
		NONE_DISCOUNT
}
```

말 그대로 데이터 중심의 접근 방법이다. `Movie` 가 할인 금액을 계산하는데 필요한 데이터는 금액 할인은 할인 금액, 비율 할인은 할인 비율이 필요하다. 

데이터 중심 설계는 객체가 포함해야하는 데이터에 집중한다. 객체의 책임을 결정하기 전 포함해야할 데이터가 무엇인지 생각하는건 데이터 중심의 설계에 매몰되어있는 것이다.

캡슐화를 준비해야하므로, 접근자, 수정자를 추가한다.

```java
public class Movie {
		private String title;
		public MovieType getMovieType() {
				return movieType;
		}
		public void setMovieType(MovieType movietype) {
				this.movieType = movieType;
		}
		// getter, setter 작성
}
```

캡슐화도 완성했다. 할인 조건을 구현해보자.

```java
public enum DiscountConditionType {
		SEQUENCE,  // 순번 조건
		PERIOD     // 기간 조건
}
```

할인 조건을 구현하는 `DiscountCondition`은 할인 조건의 타입을 저장할 인스턴스 변수인 type을 포함한다. 요일, 시작시간, 종료시간을 포함한다.

```java
public class DiscountCondition {
		private DiscountConditionType type;
		
		private int sequence;
		
		private DayOfWeek dayOfWeek;
		private LocalTime startTime;
		private LocalTime endTime;
}
```

getter, setter을 추가해준다.

```java
public class DiscountCondition{
		public DiscountConditionType getType() {
				return type;
		}
		public void setType(DiscountConditionType type) {
				this.type = type;
		}
		
		// getter, setter 작성
}
```

`Screening` 클래스 구현

```java
public class Screening { 
		private Movie movie;
		private int sequence;
		private LocalDateTime whenScreened;
		
		public Movie getMovie() {
				return movie;
		}
		public void setMovie(Movie movie) {
				this.movie = movie;
		}
		// getter, setter 작성
}
```

`Reservate`클래스 추가

```java
public class Reservation {
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
		
		//getter, setter 작성
}
```

`Customer`은 고객의 정보를 보관하는 클래스이다.

```java
public class Customer {
		private String name;
		private String id;
		
		public Customer(String name, String id) {
				this.id = id;
				this.name = name;
		}
}
```

!image.png

**영화를 예매하자**

`ReservationAgency`는 데이터를 조합해 영화 예매 절차를 구현하는 클래스다.

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
	        }
	        else {
	            discountable = condition.getSequence() == screening.getSequence();
	        }
	        
	        if(discountable) break;
	    }
	    
	    Money fee;
	    if(discountable) {
	        Money discountable = Money.ZERO;
	        switch(movie.getMovieType()) {
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
	        fee = movie.getFee().minus(discountAmount);
	    }
        else {
            fee = movie.getFee();
        }
        
        return new Reservation(customer, screening, fee, audienceCount);
	}
}
```

`reservation`메서드

1. `DiscountCondition`에 대해 루프를 돌면서 할인 가능 여부 확인
2. `discountable` 변수의 값을 체크하고 적절한 할인 정책에 따라 요금 계산

### 02 설계 트레이드 오프

캡슐화, 응집도, 결합도의 의미.

**캡슐화**

외부에서 알 필요가 없는 부분을 감춤으로써 대상을 단순화 하는 추상화.

변경 가능성이 높은 부분은 객체의 안으로 숨기고, 외부에는 상대적으로 안정적인 부분만 개방해 변경의 여파를 통제 가능함. 

구현 → 변경될 가능성 높음

인터페이스 → 상대적으로 안정적인 부분

즉, 캡슐화란 변경 가능성이 높은 부분을 객체 내부로 숨기는 추상화 기법

**응집도와 결합도**

응집도

: 모듈이 포함된 내부 요소들이 연관돼 있는 정도

객체 또는 클래스에 얼마나 관련 높은 책임들을 할당했는지.

결합도

: 의존성이 정도. 다른 모듈에 대해 얼마나 많은 지식을 갖고 있는지 나타내는 척도

객체 또는 클래스가 협력에 필요한 적절한 수준의 관계만 유지하고 있는지.

변경의 관점에서의 응집도와 결합도

- 응집도 → 변경이 발생 할 때 모듈 내부에서 발생하는 변경의 정도
    - 하나의 변경을 위해 모듈 전체가 함께 변경되면 응집도가 높음.
    - 하나의 변경에 대해 모듈 하나만 바뀌면 응집도가 높음
    - 높을 수록 변경의 대상과 범위가 명확해져 코드를 변경하기 쉬움.
- 결합도 → 한 모듈이 변경되기 위해서 다른 모듈의 변경을 요구하는 정도
    - 하나의 모듈을 수정할 때 얼마나 많은 모듈을 수정해야하는지.
    - 내부 구현을 변경했을 때 다른 모듈에 영향이 미치는 경우
    - 높을 수록 변경해야할 모듈 수가 늘어나 변경이 어려움
    - 높아도 상관없는 경우 → 변경될 확률이 매우 적은 안정적인 모듈에 의존하는건 문제 X
        - ex. Java의 ArrayList, String
        - 직접 작성한 코드는 불안정하며, 변경될 가능성 높음.

### 03 데이터 중심의 영화 예매 시스템의 문제점

1. 캡슐화 위반
2. 높은 결합도
3. 낮은 응집도

**캡슐화 위반**

`Movie`는 메서드를 통해서만 객체 내부 상태에 접근 할 수 있다. 이는 캡슐화를 잘 지킨 것처럼 보이지만… `getFee`라는 이름은 Fee 속송이 있는 것을 암시적으로 보여준다.

```java
public class Movie {
		private Money fee;
		
		public Money getFee() {
				return fee;
		}
		// setter
}
```

이는 내부의 데이터 자체에 초점을 뒀기 때문에 생긴다. 객체에게 중요한 것은 책임이다.

접근자와 수정자에 과도하게 의존하는 것을 추측에 의한 설계전략이라 한다.

**높은 결합도**

`ReservationAgency`

```java
public class ReservationAgency {
		public Reservation reserve(Screening screening, Customer customer, int audienceCount) {
		...
				Money fee;
				
				if(discountable) {
						...
						fee = movie.getFee().minus(discountAmount).times(audienceCount);
				}
				else {
						fee = movie.getFee();
				}		
		...
		}
}
```

`ReservationAgency`는 예매 요금을 계산하기 위해 `Movie`의 `getFee`를 호출하고, 이 결과를 `fee`에 저장한다. `fee`의 타입을 변경하면, 메서드 변환 타입도 수정해야하고, ReservationAgency 구현도 변경된 타입에 맞게 구현해야한다.

`ReservationAgency`는 `Reservation`, `Screening`, `movie`, `DiscountCondition`등 너무 많은 클래스에 의존한다. 이는 변경 한번에 시스템 전체를 엎어하는 것을 보여준다.

**낮은 응집도**

아래와 같은 수정이 발생하면 `ReservationAgency` 는 변경되어야 한다.

- 할인 정책 추가
- 할인 정책별로 할인 요금 계산 방법 변경
- 할인 조건 추가
- 할인 조건별로 할인 여부 판단 방법 변경
- 예매 요금 계산 방법 변경

문제점

- 변경의 이유가 서로 다른 코드를 하나에 모아놔서 변경과 아무런 관련 없는 코드들이 영향받음.
- 하나의 요구사항 변경을 반영하기 위해 동시에 여러모듈 수정.

### 04 자율적인 객체를 향해

**캡슐화를 지켜라**

객체는 스스로 상태를 책임져야하며 외부에서 인터페이스에 정의된 메서드를 통해서만 상태에 접근 가능해야함.

예시로 Rectangle 클래스 생성.

```java
class Rectangle {
    private int left;
    private int top;
    private int right;
    private int bottom;
    
    public Rectangle(int left, int top, int right, int bottom) {
        this.left = left;
        this.top = top;
        this.right = right;
        this.bottom = bottom;
    }
    
    public int getLeft() { return left; }
    public void setLeft(int left) { this.left = left; }
    
}
```

이 사각형의 높이와 너비를 증가시키는 코드가 필요하면, 외부의 어떤 클래스에 어떤 메서드로 들어간다 가정.

- 코드 중복 발생 확률 높음
- 변경에 취약함.

결론적으론, 캡슐화를 강화해 `Rectangle`내부에 높이와 너비를 증가시키는 코드를 넣는다.

**스스로 자신의 데이터를 책임지는 객체**

“이 객체가 어떤 데이터를 포함해야하는가?”는 아래 두 질문으로 분리해야한다.

1. 이 객체가 어떤 데이터를 포함해야하는가?
2. 이 객체가 데이터에 대해 수행해야하는 오퍼레이션은 무엇인가?

`ReservationAgency`로 새어나간 데이터에 대한 책임을 진짜 책임을 가진 객체로 옮기기. (`DiscountCondition`)

첫번째 질문은, 이미 선언해놓았다. 

```java
public class DiscountCondition {
		private DiscountConditionType type;
		private int sequence;
		private DayOfWeek dayOfWeek;
		private LocalTime startTime;
		private LocalTime endTime;
}
```

두번째, 오퍼레이션이 무엇인가?

→ 순번 조건일 경우, `sequence` 사용해 할인 여부 결정

→ 기가나 조건일 경우, 날짜 데이터를 사용해 할인 여부 결정

```java
public class DiscountCondition {
    public boolean isDiscountable(DayOfWeek dayOfWeek, LocalTIme time) {
        if(type != DiscountConditionType.PERIOD) {
            throw new IllegalArgumentException();
        }
        
        return this.dayOfWeek.equals(dayOfWeek) &&
                this.startTime.compareTo(time) <= 0 &&
                this.endTime.compareTo(time) >= 0;
    }

    public boolean isDiscountable(int sequence) {
        if(type != DiscountConditionType.SEQUENCE) {
            throw new IllegalArgumentException();
        }
        return this.sequence == sequence
    }
}
```

`Movie` 설정

```java
public class Movie {
		private String title;
		private Duration runningTime;
		private Money fee;
		private List<DiscountCondition> discountConditions;
		
		private MovieType movieType;
		private Money discountAmount;
		private double discountPercent;
}
```

- 어떤 데이터를 포함해야할까?
- 데이터를 처리하기 위해 어떤 오퍼레이션이 필요할까?
    - 영화 요금을 계산하는 오퍼레이션
    - 할인 여부를 판단하는 오퍼레이션

```java
public class Movie {
		public MovieType getMovieType() { return movieType; }
		public Money calculateAmountDiscountedFee() {
				if(movieType != MovieType.AMOUNT_DISCOUNT) {
						throw new IllegalArgumentException();
				}
				
				return fee.minus(discountAmount);
		}
		
		public Money calculatePercentDiscountedFee() {
				if(movieType != MovieType.PERCENT_DISCOUNT) {
						throw new IllegalArgumentException();
				}
				
				return fee.minus(fee.times(discountPercent));
		}
		
		public Money calculateNoneDiscountedFee() {
				if(movieType != MovieType.NONE_DISCOUNT) {
						throw new IllegalArgumentException();
				}
				
				return fee;
		}
		// 영화 요금 계산 오퍼레이션
		
		public boolean isDiscountable(LocalDateTime whenScreened, int sequence) {
				for(DiscountCondition condition: discountConditions) {
						if(condition.getType() == DiscountConditionType.PERIOD) {
								if(condition.isDiscountable(whenScreened.getDayOfWeek(),
								whenScreened.toLocalTime())) {
								    return true;
								}
								else {
										if(condition.isDiscountable(sequence)) {
												return true;
										}
								}
						}
						return false;
				}
		}
		// 할인 여부 판단 오퍼레이션
}
```

`Screening`

```java
public class Screening {
		private Movie movie;
		private int sequence;
		private LocalDateTime whenScreened;
		
		public Screening (Movie movie, int sequence, LocalDateTime whenScreened) {
				this.movie = movie;
				this.sequence = sequence;
				this.whenScreened = whenScreened;
		}
		
		public Money calculateFee(int audienceCount) {
				switch(movie.getMovieType()) {
						case AMOUNT_DISCOUNT:
							if(movie.isDiscountable(whenScreened, sequence)) {
									return movie.calculateAmountDiscountedFee().times(audienceCount);
							}
							break;
						case PERCENT_DISCOUNT:
							if(movie.isDiscountable(whenScreened, sequence)) {
									return movie.calculatePercentDiscountedFee().times(audienceCount);
							}
						case NONE_DISCOUNT:
							return movie.calculateNoneDiscountedFee().times(audienceCount);
				}
				return movie.calculateNoneDiscountedFee().times(audienceCount);
		}
}
```

- 할인정책을 지원할 경우 `isDiscountable`을 호출해 할인이 가능한지 여부 판단후 요금 계산
- 할인 불가능시, `calculateNoneDiscountFee` 로 영화 값 계산

`ReservationAgency` → `calculateFee` 호출해 예매 요금 계산 및 계산된 요금 이용해 `Reservation` 생성

```java
public class ReservationAgency {
		public Reservation reserve(Screening screening, Customer customer, int audienceCount) {
				Money fee = screening.calculateFee(audienceCount);
				return new Reservation(customer, screening, fee, audienceCount);
		}
}
```

!image.png

이전에 비해서 결합도 측면에서 개선되었다. 한결 캡슐화를 지키고 있다.

### 05 하지만 여전히 부족하다.

캡슐화의 관점에서 더 향상된건 맞지만, 두 번째 역시도 데이터 중심의 설계이다. 첫 번째에서 발생한 문제 역시 두번째에서도 발생하고 있다.

**캡슐화 위반**

수정된 객체들은 자기 자신의 데이터를 스스로 처리한다.

하지만, `DiscountCondition`에 구현된 `isDiscountable`을 보면

- `DayOfWeek` 타입의 요일 정보와 `LocalTime`타입의 시간 정보를 파라미터를 받으므로, 이 타입들의 시간 정보가 인스턴스로 포함되어있다는 사실을 외부에 노출.
- `isDiscountable(int sequence)` 역시 마찬가지로, int 타입의 순번을 노출하고 있다.

`DiscountCondition`을 변경해야한다면 `isDiscountable` 두개의 파라미터를 수정하고 해당 메서드를 사용하는 모든 클라이언트도 수정해야한다. 내부 구현의 변경이 외부로 퍼져나가는 파급효과는 캡슐화가 부족하다는 뜻이다.

`Movie`또한 마찬가지다.

- `Movie` 자체가 아닌, 할인 정책의 종류다.
- `calculateAmountDiscountedFee`, `calculatePercentDiscountFee`, `calculateNoneDiscountedFee` 라는 세 개의 메서드는 금액 할인 정책, 비율 할인 정책, 미적용을 드러내고 있다.

**높은 결합도**

`DiscountCondition`이 외부로 노출되어 `Movie`와 `DiscountCondition` 의 결합도는 높다.

```java
public class Movie {
		public boolean isDiscountable(LocalDateTime whenScreened, int sequence) {
				for(DiscountCondition condition: discountConditions) {
						if(condition.getType() == DiscountConditionType.PERIOD) {
								if(condition.isDiscountable(whenScreened.getDayOfWeek(),whenScreened.toLocalTime())) {
										return true;
								}
								else {
										if(condition.isDiscountable(sequence)) {
												return true;
										}
								}
								return false;
						}
				}
		}
}
```

- `DiscountCondition`의 기간 할인 조건 명칭이 PEROID에서 다른값으로 변경되면 `Movie`를 수정해야함.
- `DiscountCondition`의 종류가 추가되거나 삭제되면 `Movie`안의 if-else문을 수정해야함.
- `DiscountCondition`의 만족 여부를 판단하는 데 필요한 정보가 변경되면 `Movie`의 `isDiscountable` 메서드로 전달된 파라미터를 변경해야함.이로 인해 `Movie` 의 `isDiscountable`도 변경되고, 이 메서드에 의존하는 `Screening`에 대한 변경도 초래.

모든 문제의 원인은 캡슐화를 지키지 않아서.

**낮은 응집도**

위의 세번째 글을 보면, 할인 조건의 종류를 변경하기 위해서 `DiscountCondition` , `Movie`, `Screening`을 함께 수정해야한다. 하나의 변경을 위해 여러 곳을 수정하는 것은 응집도가 낮다는 것이다.

이또한 캡슐화를 지키지 않아서이다.

데이터 중심의 설계가 가지는 문제점으로 인해 발생하는 것이다.

### 06 데이터 중심 설계의 문제점

1. 본질적으로 너무 이른 시기에 데이터에 관해 결정하도록 강요
2. 협력이라는 문맥을 고려하지 않고 객체를 고립시킨채 오퍼레이션을 결정함.

**데이터 중심 설계는 객체의 행동보다는 상태에 초점을 맞춘다.**

- “이 객체가 포함해야하는 데이터가 무엇인가?” → 너무 이른 시기에 데이터에 관해 결정하도록 강요해, 내부 구현에 초점을 맞추게 된다.
- 데이터 중심은 절차지향적일수 밖에 없고, 접근자와 수정자를 남발해 public과 별 다를 바 없이 만든다.
- 인터페이스는 구현을 캡슐화 하는데 실패하고, 코드는 변경에 취약해진다.

즉, 객체의 내부 구현이 객체의 인터페이스를 어지럽히고, 객체의 응집도와 결합도에 나쁜 영향을 미쳐 변경에 취약한 코드를 만든다.

**데이터 중심 설계는 객체를 고립시킨 채 오퍼레이션을 정의하도록 한다.**

객체지향은 협력하는 객체들의 공동체를 만드는데에 있다.

- 객체의 내부가 어떤 상태를 가지고 상태를 관리하는지는 부가적 문제이다.
- 객체가 외부, 다른 객체와 협력하는 방법이 중요하다.

데이터 중심의 초점은 외부가 아닌 내부이다. 이미 구현이 결정된 상태에서 다른 객체와 협력을 고민하기 때문에 이미 구현된 객체의 인터페이스를 억지로 끼워맞추게 되는 것이다.

두번째 설계가 변경에 유연하지 못했던 이유가 이것이다. 인터페이스에 구현이 노출되어 협력이 구현에 종속되고, 그에 따라 객체 내부의 구현이 변경되었을 때 협력하는 객체 모두가 영향을 받게 되는 것이다.
