## ****15장 JUnit 들여다보기****

### JUnit 프레임워크

- 자바 프로그래밍 언어용 **유닛 테스트 프레임워크**
- JUnit은 자바 프레임워크 중에서 가장 유명하다

보이스카우트 규칙에 따른 개선 

### 조건문 캡슐화

- 의도를 명확하게 표현하기 위해 조건문을 캡슐화

```java
// Before
if (expected == null || actual == null || areStringsEqual()) {
  return Assert.format(message, expected, actual);
}

// After
if (shouldNotCompact()) {
  return Assert.format(message, expected, actual);
}

private boolean shouldNotCompact() {
  return expected == null || actual == null || areStringsEqual();
}
```

### 숨겨진 시간적인 결합

- 잘못된 순서로 호출하면 밤샘 디버깅을 하게됨
- 함수를 합쳐서 호출 순서 변경

결론

- 보이스카우트 규칙을 지킴
- 모듈이 처음보다 깨끗해짐
- **세상에 개선이 불필요한 모듈은 없음**

## ****16장 SerialDate 리팩터링****

- 날짜를 표현하는 자바 클래스

### 첫째, 돌려보자

- SerialDateTests 는 단위 테스트 케이스 몇개를 포함
- 테스트 커버리지가 대략 50% 정도이다.
- **클래스를 철저히 이해하고 리팩터링하려면 높은 테스트 커버리지가 필요하다.**

### 둘째, 고쳐보자

- **주석 분류**
    - 불필요한 주석 삭제
- **import 문 간결화**
    - import 문 java.util.* 형식으로 줄이기

- 서술적인 이름 사용
    - ex) createInstance → makeDate
- 상수 열거형 보다 Enum 사용
    - MonthConstants 를 상속하고 있는데 static final 상수 모음에 불과하다.
    - MonthConstants 는 Enum 으로 변경하여 사용하는것이 마땅하다.

### 접근 제한자(지정자) 변경

- 내부에서만 사용하고 외부로 공개할 필요가 없는 경우 public -> private 으로 변경

### 불필요한 final 키워드 제거

- 인수와 변수 선언에서 final 키워드 제거
- 실질적인 가치는 없으면서 코드만 복잡하게 만든다고 판단

### 중첩 if 문 통합

- if 문이 중첩되어 나올 경우 ||, && 등으로 하나로 만든다.

### 테스트 케이스에서만 호출 한다면 제거

- 일련의 리팩터링 작업은 나름대로 우아했지만실제로 이 메서드를 사용하는 코드는 방금 수정한 테스트 케이스가 유일했다.
- 그래서 저자는 테스트 케이스를 삭제했다.

### 물리적 의존성과 논리적 의존성

- 물리적 의존성은 없지만 논리적 의존성이 존재하는 경우도 있다.
- 뭔가가 구현에 논리적으로 의존한다변 물리적으로도 의존해야 마땅하다.

## 결론

- 다시 한 번 우리는 보이스카우트 규칙을 따랐다.
- 체크아웃한 코드보다 좀 더 깨끗한 코드를 체크인하게 되었다.
- 시간은 걸렸지만 가치있는 작업이었다.
- 테스트 커버리지가 증가했으며, 버그 몇 개를 고쳤으며, 코드 크기가 줄었고, 코드가 명확해졌다.
- 다음 사람은 우리보다 코드를 좀 더 쉽게 이해하리라.
- 그래서 우리 보다 코드를 좀 더 쉽게 개선하리라.

## ****17장 냄새와 휴리스틱****

코드에서 나는 '나쁜 냄새'들의 목록

## 주석

- 부적절한 정보
- 쓸모 없는 주석
- 중복된 주석 정리
- 주석 처리된 코드

## 환경

- 여러 단계 빌드
- 단위 테스트 시 여러 명령어 사용

## 함수

- 너무 많은 인수
    - 인수의 개수는 없는 것이 가장 좋고 적을수록 좋다.
- 출력 인수
- 플래그 인수 (두 가지 역할 하기 때문에 사용 지양)
- 죽은 함수
    - 아무도 호출하지 않는 함수는 삭제

## 일반

- 한 소스 파일에 여러 언어 사용
- 당연한 동작을 구현하지 않는다
    - 놀람 최소화 원칙 (https://en.wikipedia.org/wiki/Principle_of_least_astonishment)
- 경계를 올바로 처리하지 않는다
    - 경계 조건 테스트
- 안전 절차 무시
- 중복
    - 코드에서 중복을 발견할 때마다 추상화할 기회로 간주하라.
    - 여러 모듈에서 일련의 switch/case, if/else문으로 똑같은 조건을 확인하는 중복은 다형성으로 대체해야 한다.
    - 알고리즘이 유사하나 코드가 서로 다른 중복일 경우 TEMPLATE METHOD 패턴이나 [STRATEGY 패턴](https://en.wikipedia.org/wiki/Strategy_pattern)으로 중복을 제거
- 추상화 수준이 올바르지 못하다

- 기초 클래스가 파생 클래스에 의존
    - 기초 클래스는 파생 클래스를 몰라야 한다.
- 과도한 정보
- 실행되지 않는 코드
- 수직 분리
    - 변수와 함수는 사용되는 위치에 가깝게 정의한다
- 일관성 부족
- 기능 욕심
- 모호한 의도
- 부적절한 static 함수
    - 스턴스 함수로 정의하는 것이 바람직하다.
- 서술적 변수
- 이름과 기능이 일치
- 알고리즘 이해
    
    **구현이 끝났다고 선언하기 전에 함수가 돌아가는 방식을 충분히 이해했는지 확인해야 한다.**
    
- 논리적 의존성은 물리적으로 드러내라
    - 의존하는 모듈이 상대 모듈에 대해 뭔가를 가정하면 안된다. 의존하는 모든 정보를 명시적으로 요청하는 것이 좋다.
    - 메서드를 추가함으로써 논리적으로만 의존하던 것을 물리적으로도 의존하도록 보여줄 수 있다.
- if/else 혹은 switch/case문보다 다형성을 활용