# 😆 1. 객체 설계



## 1. 객체 설계

💡 이론이 먼저일까? 실무가 먼저일까?

> 대부분의 사람들은 이론이 먼저 정립 → 그 뒤를 따라 발전

* 글래스는 그 반대라고 주장

글래스에 따르면 어떤 분야를 막론하고 이론을 정립할 수 없는 초기에는

실무가 먼저 급속한 발전을 이룸

> 실무가 어느 정도 발전하고 난 다음에야 비로소 실무의 실용성을 입증할 수 있는
>
> 이론이 ⇒ 그 모습을 갖춰가고
>
> 해당 분야가 충분히 성숙해지는 시점 → 이론이 실무를 추월

### 실무가 이론보다 앞서 있는 것

> 소프트웨어 설계

> 소프트웨어 유지보수

70년대에 들어서야 최초의 이론이 세상에 모습을 드러냄

### 1. 티켓 판매 어플리케이션 구현하기

> 연극이나 음악회를 공연할 수 있는 작은 소극장 경영을 상상

* 작은 이벤트를 기획
* 추첨을 통해 선정된 관람객에게 공연을 무료로 관람할 수 있는 초대장 발송

> 염두 사항

* 이벤트 당첨 고객과 그렇지 못한 관람객 → 다른 방식으로 입장
* 이벤트 당첨 → 초대장을 티켓으로 교환 후 입장
* 이벤트 당첨 X → 티켓을 구매해야만 입장 가능

> 이벤트 당첨자에게 발송되는 초대장을 구현하는 것으로 시작

* 초대장이라는 개념을 구현한 Invitation은 공연을 관람할 수 있는
* when을 인스턴스 변수로 포함하는 간단한 클래스

```java
public class Invitation {
	private LocalDateTime when;
}
```

> 공연을 관람하기 원하는 모든 사람들은 → 티켓을 소지하고 있어야 함

> Ticket 클래스를 추가

```java
public class Ticket {
	private Long fee;

	public Long getFee() {
		return fee;
	}
}
```

* 이벤트 당첨자는 티켓으로 교환할 초대장을 가지고 있을 것임
* 이벤트에 당첨되지 않은 관람객 → 현금을 보유
* 관람객이 가지고 올 수 있는 소지품
  1. 초대장
  2. 현금
  3. 티켓
* 관람객은 소지품을 보관할 용도로 → 가방
* 관람객이 소지품을 보관할 → Bag 클래스

> Bag 클래스

* 초대장(invitation)
* 티켓(ticket)
* 현금(amount)

→ 인스턴스 변수로 포함

> 초대장의 보유 여부를 판단하는 hasInvitation → 메서드

> 티켓의 소유 여부를 판단하는 hasInvitation → 메서드

> 현금을 증가시키거나 감소시키는 plusAmount

> minusAmount

> 초대장을 티켓 교환하는 setTicket 메서드

```java
public class Bag {
	private Long amount;
	private Inviation invitation;
	private Ticket ticket;

	public boolean hasInvitation() {
		return invitation != null;
	}

	public boolean hasTicket() {
		return ticket != null;
	}

	public void setTicket(Ticket ticket) {
		this.ticket = ticket;
	}

	public void minusAmount(Long Amout) {
		this.amout -= amount;
	}
	
	public void plusAmount(Long amount) {
		this.amount += amount;
	}
}
```

> 이벤트에 당첨된 가방 안에는 현금과 초대장

> 이벤트 당첨 X → 초대장이 들어있지 않음

* Bag 인스턴스의 상태는 현금과 초대장을 함께 보관하거나
* 초대장 없이 현금만 보관한는 두 가지 중 하나일 것

Bag 인스턴스를 생성하는 시점에 이 제약을 강제할 수 있도록 생성자를 추가

```java
public class Bag{
	public Bag(long amount) {
		this(null, amount);
	}

	public Bag(Invitation invitation, long amount) {
		this.invitation = invitation;
		this.amount = amount;
	}
}
```

> 다음은 관람객이라는 개념을 구현하는 Audience 클래스를 만들 차례

관람객은 소지품을 보관하기 위해 가방을 소지 가능함

```java
public class Audience {
	private Bag bag;
	
	public Audience(Bag bag) {
		this.bag = bag;
	}

	public Bag getBag() {
		return bag;
	}
}
```

> 관람객이 소극장에 입장하기 위해서 → 매표소에서 초대장을 티켓으로 교환하거나 구매
>
> 따라서 매표소에는 관람객에게 판매할 티켓과 티켓의 판매 금액이 보관
>
> 매표소를 규현하기 위해 → TicketOffice 클래스를 추가
>
> TicketOffice는 판매하거나 교환해 줄 티켓을 목록(ticket)와
>
> 판매 금액(amount)을 인스턴스 변수로 포함함

* 티켓을 판매하는 getTicket 메서드는 편의를 위해
* tickets 컬렉션에 맨 첫 번째 위치에 저장된 Ticket을 반환하는 것으로 구현
* 또한 판매 금액을 더하거나 빼는 plusAmount, minusAmount도 함께 구현

```java
public class TicketOffice {
	private Long amount;
	private List<Ticket> tickets = new ArrayList<>();
	
	public TicketOffice(Long amount, Ticket ... tickets) {
		this.amount = amount;
		this.tickets.addAll(Arrays.asList(tickets));
	}

	public Ticket getTicket() {
		return tickets.remove(0);
	}

	public void minusAmount(Long amount) {
		this.amount -= amount;
	}

	public void plusAmount(Long amount) {
		this.amount += amount;
	}
}
```

> 판매원은 매표소에 초대장 티켓으로 교환해 주거나 티켓을 판매하는 역할을 수행
>
> 판매원을 구현한 TicketSeller 클래스는 자신이 일하는 매표소(ticketOffice)를 알고 있어야 함

```java
public class TicketSeller {
	private TicketOffice ticketOffice;

	public TicketSeller(TicketOffice ticketOffice) {
		this.ticketOffice = ticketOffice;
	}
	public TicketOffice getTicketOffice() {
		return ticketOffice;
	}
}
```

> 소극장을 구현하는 클래스 → Theater
>
> Theater 클래스가 관람객을 맞일할 수 있도록 enter 메서드 구현

```java
public class Theater {
    private TicketSeller ticketSeller {
        this.ticketSeller = ticketSeller;
    }

    public void enter(Audience audience) {
        if (audience.getBag().hasInvitation()) {
            Ticket ticket = ticketSeller.getTicketOffice().getTicket();
            audience.getBag().setTicket(ticket);
        } else {
          Ticket ticket = ticketSeller.getTicketOffice().getTicket();
          audience.getBag().minusAmount(ticket.getFee());
          ticketSeller.getTicketOffice().plusAmount(ticket.getFee());
          audience.getBag().setTicket().setTicket(ticket);
        }
    }
}
```

1. 소극장은 먼저 관람객의 가방 안에 초대장이 들었는지 확인
2. 만약 초대장이 들어 있따면 → 이벤트에 당첨 → 가방안에 티켓을 넣어줌
3. 가방안에 티켓이 없다면 티켓을 판매
4. 소극장의 관람객의 가방안에서 티켓 금액만큼 금액을 차감한 후 → 매표소 금액 증가
5. 관람객의 가방 안에 티켓 → 관람객의 입장 절차를 끝냄

### 2. 무엇이 문제인가?

> 로버트 마틴은 소프트웨어 모듈이 가져야 하는 세 가지 기능

> 모듈 : 크기와 상관 없이 클래스나 패키지, 라이브러리와 같이 프로그램을 구성하는 임의의 요소

* 모든 소프트웨어 모듈에는 세 가지 목적

1. 실행 중에 제대로 동작
   * 이것은 모든 모듈의 존재 이유
2. 변경을 위해 존재
   * 대부분의 모듈은 생명주기 동안 변경 → 간단한 작업만으로 변경이 가능해야 함
   * 변경하기 어려운 모듈은 제대로 동작하더라도 → 개선
3. 코드를 읽는 사람과 의사소통
   * 모듈은 특별한 훈련 없이도 개발자가 쉽게 이해하고 읽을 수 있어야함
   * 읽는 사람과 소통할 수 없는 모듈은 개선

#### 예상을 빗나가는 코드

> 소극장은 → 관람객의 가방을 열어 → 초대장이 들어있는지 확인
>
> 가방안에 초대장이 들었다면 → 판매원은 매표소에 보관돼 있는 티켓을 → 관람객의 가방 안으로 옮김
>
> 가방 안에 초대장이 들어 있지 않다면 → 가방에서 티켓 금액만큼 현금 꺼내 → 매표소에 적립
>
> 보관돼 있는 티켓을 가방 안으로 옮김

> 관람객과 판매원이 소극장의 통제를 받는 수동적인 존재

* 누군가 허락없이 내 가방을 뒤진다면 → 이를 두고 봄?

> 판매원도 동일한 상황

* 소극장이 허락도 없이 → 티켓과 현금에 접근이 가능함

> 이해 가능한 코드 → 우리의 예상을 크게 벗어나지 않는 코드

#### 또 다른 이유

> 여러 가지 세부적인 내용들을 한번에 기억해야 함

* enter 메서드를 이해하기 위해
  * Aundience가 Bag을 가지고 있고
  * Bag 안에는 현금과 티켓이 들어 있으며
  * TicketSeller가 TicketOffice에서 티켓을 판매하고
  * TicketOffice 안에 돈과 티켓이 보관되어 있음

> 이 모든 사실을 기억이 가능함?

#### 가장 큰 문제

> Audience, TicketSeller를 변경할 경우 → Theater도 함께 변경해야 함

### 변경에 취약한 코드

> 더 큰 문제는 → 변경에 취약하는 점

* 이 코드는 관람객이 현금과 초대장을 보관하기 위해 → 항상 가방을 들고 다니는 점
* 또한 판매원이 매표소 에서만 → 티켓을 판매하고 있다고 가정
* 관람객이 현금이 아닌 → 신용카드를 들고다면?
* 판매원이 매표소 밖에서 티켓을 판매 한다면?

> 이는 객체 사이의 의존성(dependency)에 관한 문제

문제는 의존성이 변경과 관련돼 있다는 점임

의존성은 변경에 대한 영향을 암시함

`의존성이라는 말 속에는 어떤 객체가 변경될 때 → 그 객체에 의존하는 다른 객체도 변경 가능함`

> 그렇다고 해서 객체 사이의 의존성을 완전히 없애는 것이 정답은 아님

객체지향 설계는 서로 의존하면서 협력하는 객체들의 공동체를 구축하는 것

따라서 우리의 목표는 애플리케이션의 기능을 구현하는 데 필요한 최소한의

의존성 만을 유지하고 → 불필요한 의존성을 제거한느 것

> 객체 사이의 의존성이 과한 경우 → `결합도(coupling)가 높다고 함`

> 반대로 객체들이 합리적인 수준으로 의존 → 결합도가 낮다

결합도는 의존성과 관련 → 결합도 역시 변경과 관련

두 객체 사이의 결합도가 높으면 높을수록 → 함께 변경될 확률도 높아짐

변경이 어려워짐

따라서 설계의 목표는 객체 사이의 결합도를 낮춰 변경이 용이한 설계를 만드는 것

### 설계 개선하기

예제 코드는 로버트 마틴이 이야기한 세가지 목적 중 한가지는 만족시키지만

다른 두 조건은 만족시키지 못함

> 변경과 의사소통이라는 문제가 서로 엮여 있다는 점에

코드를 이해하기 어려운 이유는 Theater가 관람객의 가방과 판매원의 매표소에 직접 접근

이것은 관람객과 판매원이 자신의 일을 스스로 처리해야한다.

> 이것은 관람객과 판매원이 자신의 일은 스스로 처리해야 한다는
>
> 직관에서 벗어남

* 의도를 정확하게 의사소통하지 못하기 때문에 소통이 어려워진 것
* Theatter가 관람객의 가방과 배표소에 직접 접근한다
  * Theater가 Audience와 TicketSeller에 결합된다는 것을 의미함
  * 따라서 Audince 변경 사항이 생기면 모든 코드 변경 → 비효율적

> 해결 방법은 간단

* Theater가 Audience, TicketSeller에 관해 너무 세세한 부분까지 알지 못하도록 정보를 차단
  *   관람객이 가방을 가지고 있다는 사실을 매표소에서 티켓을 판매 한다는 사실을

      Theater가 알아야 할 필요가 없음
*   Theater가 원하는 것은 관람객이 소극장에 입장하는 것뿐임

    따라서 관람객이 스스로 가방 안의 현금과 초대장을 처리하고

    판매원이 스스로 매표소의 티켓과 판매 요금을 다루면

    모든 문제를 한 번에 해결할 수 있음

> 관람객과 판매원을 → 자율적인 존재로

### 자율성을 높이자

> 설계를 변경하기 어려운 이유

Theater가 Audience와 TicketSeller뿐만 아니라

Audience 소유의 Bag과 TicketSleer가 근무하는 TicketOffice까지 마음대로 접근 가능

> 해결 방법

Audience와 TicketSeller가 직접 Bag과 TicketOffice를 처리하는 자율적인 존재가 되도록

설계를 변경하는 것

> 첫 번째 단계

Theater의 enter 메서드에서 TicketOffice에 접근하는 모든 코드를

TicketSeller 내부로 숨기는 것

TicketSeller에 sellTo 메서드를 추가하고 Teater에 있던 로직을 이 메서드로 이동

```java
public class Theater {
	private TicketSeller ticketSeller;

	public Theater(TicketSeller ticketSeller) {
		this.ticketSeller = ticketSeller;
	}

	public void enter(Audience audince) {
		if (audience.getBag().hasInvitation() {
			Ticket ticket = ticketSeller.getTikcetOffice().getTicket();
			audience.getBag().setTicket(ticket);
		} else {
			Ticket ticket = ticketSeller.getTicketOffice().getTicket();
			audience.getBag().minusAmount(ticket.getFee());
			ticketSeller.getTicketOffice().plusAmount(ticket.getFee());
			audience.getBag().setTicket(ticket);
		}
	}
}

public class TicketSeller {
	private TicketOffice ticketOffice;

	public TicketSeller(TicketOffice ticketOffice) {
		this.ticketOffice = ticketOffice;
	}

	public void sellTo(Audience audience) {
		if (audience.getBag().hasInvitation() {
			Ticket ticket = ticketSeller.getTikcetOffice().getTicket();
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

> 다음은 sellTo 메서드를 추가한 후의 TicketSeller 클래스를 나타낸 것

```java
public class TicketSeller {
	private TicketOffice ticketOffice;

	public TicketSeller(TicketOffice ticketOffice) {
		this.ticketOffice = ticketOffice;
	}

	public void sellTo(Audience audience) {
		if (audience.getBag().hasInvitation() {
			Ticket ticket = ticketSeller.getTikcetOffice().getTicket();
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

TicketSeller에서 getTicketOffice 메서드가 제거됐다는 사실에 주목

ticketOffice의 가시성이 private이고 접근 가능한 퍼블릭 메서드가 더 이상 존재하지 않기 때문에

외부에서는 ticketOffice에 직접 접근할 수 없음

결과적으로 ticketOffice에 대한 접근은 오직 TicketSeller 안에만 존재하게 됨

따라서 TicketSeller는 ticketOffice에서 티켓을 꺼내거나 판매 요금을 적립하는 일을 스스로

수행할 수밖에 없음

> 이처럼 개념적으로 물리적으로 객체 내부의 세부적인 사항을 감추는 것
>
> 캡슐화(encapsulation)라고 부름

* 캡슐화의 목적은 변경하기 쉬운 객체를 만드는 것
  * 캡슐화를 통해 객체 내부로의 접근을 제한하면
  * 객체와 객체 사이의 결합도를 낮출 수 있기 때문에
  * 설계를 좀 더 쉽게 변경할 수 있게됨

```java
public class Theater {
	private TicketSeller ticketSeller;
	
	public Theater(TicketSeller ticketSeller) {
		this.ticketSeller = ticketSeller;
	}

	public void enter(Audience audience) {
		ticketSelle.sellTo(audience);
	}
}
```

* 수정된 Theater 클래스 어디서도 ticketOffice에 접근하지 않다는 사실에 주목
  * Theater는 ticketOffice가 TicketSeller 내부에 존재한다는 사실을 알지 못함
  * Theater는 단지 ticketSeller가 sellTo 메시지를 이해하고 응답할 수 있다는 사실만
* Theater는 오직 TicketSeller는 인터페이스(interface)에만 의존함
* TicketSeller가 내부에 TicketOffice 인스턴스를 포함하고 있다는 사실은
  * 구현의 영역에 속함
* 객체를 인터페이스의 구현으로 나누고
  * 인터페이스만을 공개하는 것은 객체 사이의 결합도를 낮추고 변경하기 쉬운 코드를 작성
* 쉬운 코드를 작성하기 위해서 따라야 하는 가장 기본적인 설계 원칙

> Theater의 로직을 TicketSeller로 이동시킨 결과

Theater에서 TicketOffice로의 의존성이 제거됐따는 사실을 알 수 있음

TicketOffice와 협력하는 TicketSeller의 내부 구현이 성공적으로 캡슐화

> TicketSeller 다음으로 Audience의 캡슐화를 개선

TicketSeller는 Audience의 getBag 메서드를 호출해서

Audience 내부의 Bag 인스턴스에 직접 접근

Bag 인스턴스에 접근하는 객체가 Theater에서 TicketSeller로 바뀌었을 뿐

Audience는 여전히 자율적인 존재가 아닌 것

> TicketSeller와 동일한 방법으로 Audience의 캡슐화를 개선할 수 있음

Bag에 접근하는 모든 로직을 Audience 내부로 감추기 위해

Audience에 buy 메서드를 추가하고 TicketSeller의 sellTo 메서드에서

getBag 메서드에 접근하는 부분을 buy 메서드로 옮기나

```java
public class Audience {
	private Bag bag;
	
	public Audience(Bag bag) {
		this.bag = bag;
	}

	public Long by(Ticket ticket) {
		if (bag.hasInvitation() {
			bag.setTicket(ticket);
			return OL;
		} else {
			bag.setTicket(ticket);
			bag.minusAmount(ticket.getFee());
			return ticket.getFee();
		}
	}
}
```

* 변경된 코드에서 Audience는 자신의 가방 안에 초대장이 들어있는지를 스스로 확인
* 외부의 3자는 자신의 가방을 열어보도록 허용하지 않음
  * Audience가 Bag을 직접 처리하기 때문에
  * 외부에서는 더 이상 Audience가 Bag을 소유하고 있다는 사실을 알 필요가 없음
* Audience 클래스에서 getBag 메서드를 제거할 수 있고
* 결과적으로 Bag의 존재를 내부로 캡슐화 가능함

> TicketSeller가 Audience의 인터페이스에만 의존하도록 수정

TicketSeller가 buy 메서드를 호출하도록 코드를 변경

```java
public class TicketSeller {
	private TicketOffice ticketOffice;

	public TicketSeller(TicketOffice tikcetOffice {
		this.ticketOffice = ticketOffice;
	}

	public void sellTo(Audience audience) {
		ticketOffice.plusAmount(audience.buy(ticketOffice.getTicket()));
	}
}
```

> 코드를 수정한 결과 TicketSeller와 Audience 사이의 결합도가 낮아짐

또한 내부 구현이 캡슐화됐으므로 Audience의 구현을 수정하더라도 TicketSeller에 영향을 미치지 않음

### 무엇이 개선됐는가?

> 수정된 예제 역시 필요한 기능을 오류 없이 수행함

* 동작을 수행해야 한다는 첫 번째 목적을 만족

💡 그러면 변경 용이성과 의사소통?

> 각각의 소지품을 스스로 관리
>
> → 우리의 예상과 정확한 일치
>
> 코드를 읽는 사람의 의사소통이라는 관점에서 이 코드는 확실히 개선

💡 더 중요한 점

> Audience나 TicketSeller의 내부 구현을 변경하더라도
>
> Theater를 함께 변경할 필요가 없어졌다는 것
>
> Audience가 가방이 아닌 → 작은 지갑?
>
> 내부만 변경하면 됨

### 어떻게 한 것인가?

* 판매자가 티켓을 판매하기 위해 → TicketOffice를 사용하는 부분
  * TicketSeller 내부로 옮기고
* 관람객이 티켓을 구매하기 위해 Bag 사용하는 모든 부분
  * Audience 내부로 옮긴 것
* 다시 말해 자기 자신의 문제를 스스로 해결하도록 코드를 변경한 것
  * 우리는 우리의 직관을 따랐고 그 결과로 코드는 변경이 용이하고 이해 가능하도록
* 수정하기 전의 코드와 수정한 후의 코드를 한 번 비교

> 수정 후

*   Theater는 Audience나 TicketSeller의 내부에 직접 접근하지 않음

    Audience는 Bag 내부의 내용물을 확인하거나, 추가하거나, 제거하는 작업을 스스로 처리하며

    외부의 누군가에게 자신의 가방을 열어보도록 허용하지 않음
* TicketSeller 역시 매표소에 보관된 티켓을 직접 판매하도록 바뀜
  * 수정된 TicketSeller는 다른 누군가가 매표소 안을 마음대로 휘젓지 않도록

> 우리는 개체의 자율성을 높이는 방향으로 설계

### 캡슐화와 응집도

> 핵심은 객체 내부 상태를 캡슐화하고 객체 간에 오직 메시지를 통해서만 상호작용하도록 만드는 것

> Theater는 TicketSeller의 내부에 대해서는 전혀 알지 못함

* 단지 TicketSeller가 sellTo 메시지를 이해하고 응답할 수 있다는 사실만 알고 있음

> 자신이 원하는 응답값을 받을 수 있다는 확신

> 밀접하게 연관된 작업만을 수행하고
>
> 연관성 없는 작업은 다른 객체에게 위임하는 객체

→ 응집도가 높다

* 자신의 데이터를 스스로 처리하는 자율적인 객체를 만들면
  * 결합도를 낮추고 → 응집도를 높일 수 있음

> 객체의 응집도를 높이다

* 객체 스스로 자신의 데이터를 책임져야함
* 자신이 소유하고 있지않은 데이터를 이용해
  * 작업을 처리하는 객체에게 어떻게 연관성 높은 작업들을 할당할 수 있겠는가?
* 객체는 자신의 데이터를 스스로 처리하는 자율적인 존재
  * 그것이 객체의 응집도를 높이는 첫걸음
  * 외부의 간섭을 최대한 배제하고
  * 메시지를 통해서만 협력하는 자율적인 객체들의 공동체
  * 휼륭한 객체 지향 설계

💡 외부의 간섭을 최대한 배제하고 메시지를 통해서만 협력하는 자율적인 객체들의 공동체 휼륭한 객체지향 설계

### 절차지향과 객체 지향

> 절차적 프로그래밍(Procedural Programming)

프로세스와 데이터를 별도의 모듈에 위치시키는 방식

*   모든 처리가 하나의 클래스 안에 위치하고

    다른 클래스들은 단지 데이터의 역할만 수행함

⚠️ 문제점

절차적 프로그래밍 세상에서는 데이터의 변경으로 인한 영향을

지역적으로 고립시키기 어려움

> 내부 구현을 변경시 → 다른 메서드도 함께 변경해야함

> 변경 → 버그 → 두려움 → 코드 변경하기 어렵게

* 절차적 프로그래밍의 세상은 변경하기 어려운 코드를 양산

💡 변경하기 쉬운 설계

> 한 번에 하나의 클래스만 변경할 수 있는 설계

💡 객체지향 프로그래밍

* 데이터와 프로세스가 동일한 모듈 내부에 위치하도록 프로그래밍
  * 메서드 : 프로세스
  * 데이터

> 휼륭한 객체지향 설계의 핵심

* 캡슐화를 이용해 의존성을 적절히 관리
  * 객체 사이의 결합도를 낮추는 것
* 객체지향 > 절자지향 (유연)
* 객체지향 코드는 자신의 문제를 스스로 처리해야 한다는 우리의 예상을 만족
  * 객체 내부의 변경이 객체 외부에 파급되지 않도록
  * 제어할 수 있기 때문에 변경하기 수월함

### 책임의 의동

* 두 방식의 근복적인 차이

`책임의 이동(shift of responsibility)`

> 책임 : 기능을 가리키는 개체지향 세계의 용어로 생각해도 무방

#### 두 방식의 차이점을 가장 쉽게 이해할 수 있는 방법

* 기능을 처리하는 방법을 살펴보는 것
* 절차지향
  * 책임이 Theater에 집중
* 객체지향
  * 책임이 분산되서 이동

> 각 객체는 자신을 스스로 책임짐

> 이런관점에서 객체지향 프로그래밍

* 흔히 데이터와 프로세스를 하나의 단위로 통합해 놓는 방식으로 표현
  * 구현 관점에서만 바라본 지극히 편협한 사실
  * 하지만 입문자에게 실용적인 조언
* 코드에서 데이터와 데이터를 사용하는 프로세스가 → 별도의 객체에 위치
  * 절차적 프로그래밍 방식을 따르고 있을 확률이 높음
* 객체지향 안에는 단순히 데이터와 프로세스를 하나로 묶는 것 이상
  * 사실 객체지향 설계의 핵심은 절절한 객체에 적절한 책임을 할당

> 객체지향 설계의 핵심은

* `객체는 다른 객체와의 협력이라는 문맥 안에서 특정한 책임을 할당하는 것`
* 객체가 어떤 데이터를 가지냐 보다 → 어떤 책임을 할당할 것인가?

> 설계를 어렵게 만드는 것

* 의존성이라는 것을 기억
* 해결 방법은 불필요한 의존성을 제거 → 객체 사이의 결합도를 낮추는 것
* 몰라도 되는 세부사항을 객체 내부로 캡슐화
  * 객체의 자율성을 높이고
  * 응집도 높은 객체들의 공동체를 창조
* 불필요한 세부사항을 캡슐화 하는 자율적인 객체들이 낮은 결합도와 높은 응집도를 가지고 협력하도록
  * 최소한의 의존성만 남기는 것
  * 훌륭한 객체지향 설계

### 더 개선 가능함

> bag과 ticketSeller를 개별적으로 선언

```java
public TicketSeller {
	public void sellTo(Audience audience) {
		ticketOffice.plusAmount(audience.by(ticketOffice.getTicket());
	}
}
```

#### 만족스럽지 못한 결과

> 각각으로 분리하는 것이 가장 옳지는 않음

처음에는 없던 의존성들이 생기면서

이에 대해서 높은 결합도를 가지게 됨

→ 이는 어려운 설계를 의미함

### 그래 거짓말이다!

> 실생활의 관람객과 판매자가 → 스스로 자신의 일을 처리

코드에서 Audience와 TicketSeller 역시 스스로 자신을 책임짐

우리가 세상을 바라보는 직관과도 일치

→ 직관을 따르는 코드는 이해하기가 더 쉬운 경향

💡 Theater, Bag, TicketOffice

* 실세계에서 자율적인 존재가 아님
  * 소극장에 관람객이 입장 → 누군가가 문을 열고 입장을 허가
* 가방에서 돈을 꺼내는 것
  * 관람객이지 → 가방이 아님
* 판매원이 없는데 → 티켓이 자동으로 관람객에게 전달 X

> 현실에서 수동적인 존재가 일단 객체지향의 세계 → 능동적이고 자율적인 존재

레베카 워브스브록 → 능동적이고 자율적인 존재로 소프트웨어 설계

→ 의읜화(anthropomorphism)

💡 객체가 현실 세계의 대상보다 더 많이 안다는 것이 모순적으로 보일 수 있음 결국 인간이라는 에이전트 없이 현실의 전화는 스스로 전화를 걸 수 없음

모든 생물처럼 소프트웨어는 태어나고 → 삶을 영위하고 → 죽음

> 생물처럼 스스로 생각하고 행동하도록 소프트웨어를 설계하는 것
>
> 쉬운 코드를 작성하는 것

> 휼륭한 객체지향 설계
>
> 소프트웨어를 구성하는 모든 객체들이 자율적으로 행동하는 설계
>
> 그 대상이 비록 실세계에서 생명이 없는 수동적인 존재라도
>
> 객체지향의 세계로 넘어오는 순간 → 그들은 생명과 지능을 가진 싱싱한 존재

> 이해하기 쉽고 변경하기 쉬운 코드 작성 → 한 편의 애니메이션

* 애니메이션
  * 코드 안에서
  * 웃고, 떠들고, 화내는 → 가방 객체

### 4. 객체지향 설계

💡 설계란 코드를 배치하는 것이다.

* 설계가 코드를 작성하는 것 보다 높은 차원이 아님
  * 설계는 코드를 작성하는 매 순간 코드를 어떻게 배치할 것인지 결정하는 과정

> 설계는 코드 작성의 일부이며 → 코드를 작성하지 않고서는 검증할 수 없음

💡 좋은 설계란 무엇일까?

1. 우리는 오늘 완성해야 하는 기능을 구현하는 코드를 짜야 하는 동시에
2. 내일 쉽게 변경할 수 있는 코드를 짜야 한다.

> 오늘 요구사항을 온전히 수행하면서 내일의 변경을 매끄럽게 수용할 수 있는 설계

💡 변경을 수용할 수 있는 설계가 중요한 이유

> 오늘의 요구사항과 내일의 요구사항은 다름

개발을 시작하는 시점에 구현에 필요한 모든 요구사항을 수집하는 것은 불가능에 가까움

모든 요구사항을 수집할 수 있다고 가정하더라도

개발이 진행되는 동안 요구사항은 바뀔 수 밖에 없음

> 코드를 변경할 때 버그가 추가될 가능성이 높기 때문

* 코드를 수정하지 앟으면 → 버그는 발생하지 않음
* 버그의 가장 큰 문제점은 → 코드를 수정하려는 의지를 꺾는다는 것

### 객체지향 설계

> 우리가 진정으로 원하는 것 → 변경에 유연하게 대응할 수 있는 코드

객체지향 프로그래밍은 의존성을 효율적으로 통제할 수 있는 다양한 방법을 제공함으로써

요구사항 변경에 좀 더 수월하게 대응할 수 있는 가능성을 높여줌

> 코드 변경이라는 측면에서는 객체지향이 과거의 다른 방법보다 → 안정감을 줌

> 변경 가능한 코드 → 이해하기 쉬운 코드
>
> 어떤 코드를 변경해야 하는데 크 코드를 이해할 수 없다면 어떻겠는가?
>
> 그 코드가 변경에 유연하다고 하더라도
>
> 아마 코드를 수정하겠다는 마음이 선뜻 들지 않음

#### 객체지향 패러다임

> 세상을 바라보는 방식대로 코드를 작성할 수 있게 도움

세상에 존재하는 모든 자율적인 존재처럼

객체 역시 자신의 데이터를 스스로 책임지는 자율적인 존재

> 여러분이 세상에 대해 예상하는 방식대로 객체가 행동하리라는 것을 보장함으로

코드를 쉽게 이해할 수 있음

> 하지만 데이터와 프로세스를 객체라는 덩어리에 밀어넣는것으로 모든것이 이뤄지지 않음

* 객체들 사이의 상호작용 → 객체 사이의 받는 메시지를 통해서 이루어짐

> 휼륭한 객체지향 설계란 협력하는 객체 사이의 의존서을 적절하게 괸리하는 설계

*   세상에 엮인 것이 많은 사람일수록 변하기 어려운 것처럼

    객체가 주변 환경에 강하게 결합될수록 → 변경하기 어려움

> 데이터와 프로세스를 하나의 덩어리로 모으는 것은 휼륭한 객체지향으로 가는 첫걸음
>
> 협력하는 객체들 사이의 의존성을 적절하게 조절 → 변경에 용이한 설계
