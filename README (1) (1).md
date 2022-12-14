# 😆 10. 상속과 코드 재사용



## 10.상속과 코드 재사용

### 위키

<details>

<summary>결합도</summary>

다른 대상에 대해 알고 있는 지식의 양

</details>

> 객체지향 프로그래밍 장점

*   전통적인 패러다임에서 코드를 재사용하는 방법은

    코드를 복사한 후 수정하는 것
* 객체지향에서 코드를 재사용하기 위해 → 새로운 코드를 추가
* 객체지향에서 클래스를 재사용하는 전통적인 방법은 새로운 클래스를 추가

🔥 이번 주제는 상속

* 재사용 관점에서 : 상속 클래스 안에서 정의된 인스턴스 변수와 메서드를 자동으로 새로운 클래스에 추가하는 구현 기법

> 객체지향에서 상속 외에도 코드를 효과적으로 재사용할 수 잇는 방법이 한 가지 더 있음

🔥 새로운 클래스의 인스턴스 안에 기존 클래스의 인스턴스를 포함시키는 방법 : 합성

> 코드를 재사용하려는 강력한 동기 → 중복된 코드를 제거하는 욕망



<table data-view="cards"><thead><tr><th></th><th></th><th></th><th data-type="content-ref"></th><th data-hidden data-card-target data-type="content-ref"></th><th data-hidden data-card-cover data-type="files"></th></tr></thead><tbody><tr><td><h2>My Velog</h2></td><td><mark style="color:yellow;"><code>복순이</code></mark></td><td><a href="https://velog.io/@ww8007">https://velog.io/@ww8007</a></td><td><a href="https://velog.io/@ww8007">https://velog.io/@ww8007</a></td><td><a href="README (1) (1).md">README (1) (1).md</a></td><td><a href=".gitbook/assets/KakaoTalk_Photo_2022-11-20-03-21-36.jpeg">KakaoTalk_Photo_2022-11-20-03-21-36.jpeg</a></td></tr></tbody></table>

## 1. 상속과 중복 코드

> 중복코드 → 의심을 자아냄

### DRY 원칙 (Don’t Repeat Yourself)

> 중복 코드 : 변경을 방해함

* 중복 코드를 제거해야 하는 가장 큰 이유

🔥 프로그램의 본질 - 비즈니스와 관련된 지식을 코드로 변환하는 것 - 이 지식은 항상 변함 - 지식을 표현하는 코드 역시 변경

> 중복 코드가 가지는 가장 큰 문제는 코드를 수정하는 데 필요한 노력 → 몇 배로 증가

*   모든 중복 코드를 개별적으로 테스트해서 → 동일한 결과를 내놓는지 확인해야함

    중복 코드는 수정과 테스트에 드는 비용을 증가시킬뿐만 아니라

    시스템과 → 공황상태

❓ 중복 코드는 어떻게 판단하나요?

> 중복 여부를 판단하는 기준 : 변경을 할 때 다른 코드가 수정되어야 하나?

> 모든 중복 코드를 개별적으로 테스트해서 동일한 결과를 내놓는지 확인해야만 함

* 중복 코드는 수정과 테스트에 드는 비용을 증가시킬뿐만 아니라 → 시스템과 나의 공황상태로 몰아 넣음
* 신뢰할 수 있고 수정하기 쉬운 소프트웨어를 만드는 효과적인 방법 중 하나 → 중복을 제거

✅ Don’t Repeat Yourself → DRY

* 모든 지식은 시스템 내에서 단일하고, 애매하지 않고, 정말로 믿을 만한 표현 양식을 가져야 한다.
* 코드 안에서 중복이 존재해서는 안된다

###

### 중복과 변경

#### 중복 코드 살펴보기

> 요구사항은 항상 변한다.

* 구현 시간을 절약한 대가로 지불해야 하는 비용은 예상보다 큼
* 중복 코드가 존재하는 것은 → 언제 터질지 모르는 시한폭탄을 안고 있는 것과 같음
* 언젠가 코드를 변경해야 할 때 폭탄의 뇌관이 당겨질지 모름

#### 중복 코드 수정하기

> 새로운 중복 코드를 추가하는 과정 : 코드의 일관성을 무너트리는 위험

* 중복 코드가 늘어날 수록 애플리케이션의 변경에 취약
* 버그가 발생할 가능성이 높아진다는 것
* 중복 코드의 양이 많아질수록 버그의 수는 증가
* 코드를 변경하는 속도는 느려짐

#### 타입 코드 사용하기

> 타입 코드를 사용하는 클래스는 낮은 응집도와 높은 결합도의 문제가 도사림

> 객체지향 프로그래밍 언어는 타입 코드를 사용하지 않고도 중복 코드를 관리할 수 있는 효과적인 방법

* 객체지향 프로그래밍을 대표하는 기법

###

### 상속을 이용해서 중복 코드 제거하기

> 이미 존재하는 클래스와 유사한 클래스가 필요하다면 코드를 복사하지 말고

상속을 이용해서 → 코드를 재사용하는 것

> 실제 코드는 이상과 다름

> 결합도 : 하나의 모듈이 다른 모듈에 대해 얼마나 많은 지식을 갖고 있는지를 나타내는 정도로 정의

상속을 이용해 코드를 재사용하기 위해서는 부모 클래스의 개발자가 세웠던 가정이나

추론 과정을 정확하게 이해하는 것

✅ 상속은 결합도를 높인다. 그리고 상속이 초래하는 부모 클래스와 자식 클래스 사이의 강한 결합이 이 코드를 수정하기 어렵게 만든다.

###

### 강하게 결합된 Phone과 NightlyDiscountPhone

> 부모 클래스와 자식 클래스 사이의 결합이 왜 문제일까?

* 자식 클래스를 만든 이유 : 코드를 재사용 하고, 중복 코드를 재사용 하기 위함임
* 세금을 부과하는 로직을 추가하기 위해 유사한 코드를 추가해야 했음
  *   코드 중복을 제거하기 위해 상속을 사용했어도 → 새로운 로직을 추가하기 위해서

      중복 코드를 만들어 내야함

✅ 상속을 위한 경고1

> 자식 클래스 메서드 안에서 super 참조를 이용해 부모 클래스의 메서드를 직접 호출할 경우 두 클래스는 강하게 결합된다. super 호출을 제거할 수 있는 방법을 찾아서 결합도를 제거하라

> 자식 클래스가 부모 클래스의 변경에 취약해지는 현상을 가리켜 취약한 기반 클래스 문제라고 부름 취약한 기반 클래스 문제는 코드 재사용을 목적으로 상속을 사용할 때 발생하는 가장 대표적인 문제임

## 2. 취약한 기반 클래스 문제

> 상속 : 자식 클래스와 부모 클래스의 결합도를 높임

* 강한 결합도로 인해 : 자식 클래스는 부모 클래스의 불필요한 세부사항에 엮이게 됨
* 부모 클래스 작은 변경 → 자식 클래스는 이에 대한 영향을 크게 받을 수 잇음

> 취약한 기반 클래스 문제

*   상속 → 전체 시스템의 결합도가 높아짐

    *   자식 클래스를 점진적으로 추가해서

        장점 : 기능을 확장하는데는 용이

        단점 : 높은 결합도로 인해 부모 클래스를 점진적 개선이 어려움

    > 최악 : 모든 자식 클래스 동시에 테스트

✅ 객체를 사용하는 이유

> 구현과 관련된 세부사항을 퍼블릭 인터페이스 뒤로 캡슐화가 가능함

* 변경에 의한 파급효과를 제어할 수 있기 때문에 가치가 있음
* 객체는 변경될지도 모르는 불안정한 요소 → 캡슐화
* 파급효과를 거정하지 않고도 자유롭게 내부를 변경할 수 있음

✅ 객체지향의 기반은 캡슐화를 통한 변경의 통제

* 상속 : 재사용을 위해 캡슐화의 장점을 희석 시키고 구현에 대한 결합도를 높임
  * 객체지향이 가진 강력함을 반감
* 예제를 통해 상속이 가지는 문제점을 구체적

### 불필요한 인터페이스 상속 문제

> java.util.Properties와 java.util.Stack

❄️ Vector를 재사용하기 위해 → Stack을 Vector의 자식 클래스로 구현

> Stack이 Vector

* 자식 클래스로 구현

> Stack이 Vector를 상속받기 때문에

* Stack의 퍼블릭 인터페이스에 Vector의 퍼블릭 인터페이스가 합쳐짐
* Stack에게 상속된 Vector의 퍼블릭 인터페이스를 이용하면
* 임의의 위치에서 요소를 추가하거나 삭제가 가능함
  * 맨 마지막 위치에서만 요소를 추가하거나 제거할 수 있도록 하는
  * Stack의 규칙을 쉽게 위반 가능함

❓ 단순하게 add 메서드를 안쓰면 되는거 아닌가요?

> 인터페이스 설계는 제대론 쓰기엔 쉽게
>
> 엉터리로 쓰기에는 어렵게

✅ 상속을 위한 경고 2 상속받은 부모 클래스의 메서드가 자식 클래스의 내부 구조에 대한 규칙을 깨트릴 수 있다.

### 메서드 오버라이딩의 오작용 문제

✅ 상속을 위한 경고 3 자식 클래스가 부모 클래스의 메서드를 오버라이딩할 경우 부모 클래스가 자신의 메서드를 사용하는 방법에 자식 클래스가 결합될 수 있다.

> 클래스가 상속되기를 원한다면 상속을 위해 클래스를 설계하고 문서화해야 하며 그렇지 않은 경우 → 상속을 금지시켜야 함

> 상속이 초래하는 문제점을 보완하면서 코드의 재사용의 장점을 극대화도 가능함

* 문서화

❓ 근데 내부 구현을 문서화 객체지향의 핵심이 구현을 캡슐화 → 이게 맞는 행동인가?

> 잘된 API 문서는 메서드가 무슨 일(what)을 하는지를 기술해야 하고 어떻게 하는지(how)를 설명해서는 안된다는 통념을 어기는 것이 아닐까?

* 상속 → 캡슐화를 위반함으로써 초래되는 불행
* 서브클래스가 안전하도록 → 클래스 문서화 → 상세 구현 내역을 캡슐화

> 설계 : 트레이드오프 활동이라는 사실을 기억

* 상속 : 코드 재사용을 위해 캡슐화를 희생
* 완벽한 캡슐화 : 코드 재사용을 포기하거나 상속 이외의 다른 방법을 사용

### 부모 클래스와 자식 클래스의 동시 수정 문제

> 음악 목록을 추가할 수 있는 플레이리스트

```jsx
public class Song {
	private String singer;
	private String title;

	public Song(String sinter, String title) {
		this.singer = sinter;
		this.title = title;
	}

	public String getSinger() {
		return singer;
	}

	public String getTitle() {
		return title;
	}
}
```

```java
public class Playlist {
	private List<Song> tracks = new ArrayList<>();
	
	public void append(Song song) {
		getTracks().add(song);
	}
}
```

> 요구사항이 변경돼서 PlayList에서 노래의 목록뿐만 아닌
>
> 노래의 제목도 함께 관리해야 한다고 가정
>
> 노래 추가 → 가수의 이름의 키로 추가

> 자식 클래스가 부모 클래스의 메서드를 오버라이딩하거나 불필요한
>
> 인터페이스를 상속받지 않았음에도 부모 클래스 수정 → 자식 클래스를 함께 수정
>
> 상속을 사용하면 → 자식 클래스가 부모 클래스의 구현에 강하게 결합

❓ 결합도 다른 대상에 대해 알고 있는 지식의 양

> 상속 : 부모 클래스의 구현을 재사용 한다는 기본 전제

* 자식 클래스 : 부모 클래스 내부에 대해 속속들이 알도록 강요함
* 코드 재사용을 위한 상속 → 수정 사항이 불가피함

✅ 상속을 위한 경고 4 클래스를 상속하면 결합도로 인해 자식 클래스와 부모 클래스의 구현을 영원히 변경하지 않거나, 자식 클래스와 부모 클래스를 동시에 변경하거나 둘 중 하나를 선택할 수밖에 없다.

## 3. Phone 다시 살펴보기

### 추상화에 의존하자

> 부모 클래스와 자식 클래스 → 모두 추상화에 의존하도록

✅ 중복 코드를 제거하기 위해 상속을 도입함

1.  두 메서드가 유사하게 보인다면 차이점을 메서드로 추출하라

    메서드 추출을 통해 두 메서드를 동일한 형태로 보이도록 만들 수 있음
2.  부모 클래스의 코드를 하위로 내리지 말고 → 자식 클래스의 코드를 상위로 올려라

    구체적인 메서드를 자식 클래스로 내리는 것보다 자식 클래스의 추상적인 메서드를

    부모 클래스로 올리는 것이 재사용성과 응집도 측면에서 뛰어난 결과

### 차이를 메서드로 추출하라

> 변하는 것으로부터 변하지 않는 것을 분리하라

> 변하는 부분을 찾고 이를 캡슐화하라

✅ 비슷 하지만 다른 점 찾기

* [ ] for 문 안에 구현된 요금 계산 로직이 다름

```java
private Money calculateCallFee(Call call) {
	return amount.times(call.getDuration().getSeconds() / second.getSecons());
}
```

```java
private Money calculateCallFee(Call call) {
	if (call.getFrom().getHour() >= LATE_NIGHT_HOUR) {
		return nightlyAmount.times(call.getDuration().getSeconds() / seconds.getSeconds();
	} else {
		return regularAmount.times(call.getDuration().getSeconds() / seconds.getSeconds();
	}
}
```

> 두 클래스의 메서드는 완전히 동일해졌고
>
> 추출한 calculateCallFee 메서드 안에 서로 다른 부분을 격리해 두었음

* [ ] 같은 코드를 부모 클래스로 올리자!

### 중복 코드를 부모 클래스로 올려라

> 부모 클래스를 추가하자

* 목표 : 모든 클래스들이 추상화에 의존하도록 만드는 것임
* 이 클래스는 추상 클래스로 구현하는 것이 적합
  *   새로운 부모 클래스의 이름 : AbstractPhone으로 하고

      Phone, NightlyDiscountPhone이 AbstractPhone을 상속받도록 수정

```java
public abstract class AbstractPhone {}

public class Phone extends AbstractPhone { ... }

public class NightlyDiscountPhone extends AbstractPhone { ... }
```

> 자식 클래스들 사이의 공통점을 부모 클래스 → 실제 코드를 기반으로 상속 계층

* 우리의 설계 : 추상화에 의존

❓ 위로 올리기의 장점은 무엇인가요?

> 문제는 쉽게 찾을 수 있고 쉽게 고칠 수 있음

*   추상화하지 않고 → 빼먹은 코드가 있더라도

    하위 클래스가 해당 행동을 필요할 때가 오면 → 이 문제는 바로 눈에 띔
*   모든 하위 클래스가 이 행동을 할 수 있께 만들려면

    중복 코드 양산, 위로 올리기

> 구체적인 구현을 아래로 내리는 방식을 사용하면

*   현재 클래스를 구체 클래스 → 추상 클래스로 변경하려는 순간

    작은 실수 한 번으로도 구체적인 행동을 상위 클래스에 남겨 놓음

### 추상화가 핵심이다

> 공통 코드를 이동 → 각 클래스는 서로 다른 변경의 이유를 가짐

* [x] 클래스가 각각 하나의 변경의 이유만을 가지도록
  * 단일 책임 원칙 준수 → 응집도가 높음

> 구체적인 구현에 의존 하지 않고 → 추상화에 의존

❓ 사실 부모 클래스 역시 내부에 구현된 추상 메서드를 호출하기 때문에 추상화에 의존한다고 말할 수 있음 의존성 역전 원칙도 준수

> 상위 수준의 정책을 구현하는 세부적인 구현 로직이 추상화에 의존

❓ 상위 계층이 코드를 진화시키는 데 걸림돌이 된다면 1. 추상화를 찾아내고 2. 상속 계층 안의 클래스들이 그 추상화에 의존하도록

> 차이점을 메서드로 추출하고 공통적인 부분 : 부모 클래스로 이동

### 의도를 드러내는 이름 선택하기

> 내용을 명시적으로 전달하지 못함

> 클래스라는 도구

* 메서드뿐만 아닌 인스턴스 변수도 함께 포함
*   클래스 사이의 상속 : 자식 클래스가 부모 클래스가 구현한 행동뿐만이 아니라

    인스턴스 변수에 대해서도 결합되게 만듬

> 인스턴스 변수의 목록이 변하지 않는 상황에서

* 객체의 행동만 변경된다면 상속 계층에 속한 각 클래스들을 독립적으로 진화 가능

❓ 인스턴스 변수가 추가되면요?

> 자식 클래스 → 부모 클래스에 추가된 변수 → 자식 클래스 초기화에 영향을 미침

* 책임을 아무리 잘 분리해도 → 인스턴스 변수의 추가는 상속 계층 전반에 걸친 변경을 유발함

❄️ 이는 React Props 전달에 연결하는 점과 동일하다고 바라봐야 하나?⚠️ 객체 생성 로직을 막기보다는 핵심 로직의 중복을 막아라

> 핵심 로직은 한 곳에 모아두고 조심스럽게 캡슐화
>
> 공통적인 핵심 로직 : 최대한 추상화

## 4. 차이에 의한 프로그래밍

> 상속 : 기존 코드와 다른 부분만 추가 → 차이에 의한 프로그래밍

*   상속을 이용 → 이미 존재하는 클래스의 코드를 쉽게 재사용

    애플리케이션의 점진적인 정의(increamental definition)

> 차이에 의한 프로그래밍의 목표

* [ ] 중복 코드를 제거
* [ ] 코드를 재사용

> 이는 동일한 행동을 가리키는 서로 다른 언어

⚠️ 중복을 제거하기 위해서는 1. 코드를 재사용 가능한 단위로 분해하고 2. 재구성❓ 단순하게 문자를 타이핑 하는 수고만 줄이는 것인가요?

> 재사용 가능한 코드 : 심각한 버그가 존재하지 않는 코드

*   따라서 코드를 재사용하면 코드의 품질은 유사하면서도

    코드를 작성하는 노력과 테스트는 줄일 수 있음

❄️ 상속은 양날의 검이다. 상속의 오용과 남용은 애플리케이션을 이해하고 확장하기 어렵게 만든다. 정말로 필요한 경우에만 상속을 이용하다.

> 합성 : 상속의 단점을 피하면서도 코드를 재사용하는 방법
