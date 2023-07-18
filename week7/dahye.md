..이어서

### G24 : 표준 표기법을 따르라
팀은 업계 표준에 기반한 구현 표준을 따라야 한다.  
모두가 동의한 위치에 넣는다는 사실이 중요하다.  

### G25 : 매직 숫자는 명명된 상수로 교체하라
- 코드에서 숫자를 사용하지 말라는 규칙. 숫자는 명명된 상수 뒤로 숨기라는 의미.  
- 어떤 상수는 이해하기 쉬우므로, 코드 자체가 자명하다면, 상수 뒤로 숨길 필요가 없다.  
- 매직 숫자는 단지 숫자만 의미하지 않는다. 의미가 분명하지 않은 모든 토큰을 의미한다.  
    ex) 777, "John Doe"

### G26 : 정확해라
- 검색 결과 중 첫 번째 결과만 유일한 결과로 간주하는 행동은 순진하다.
- 결정을 코드에서 뭔가를 결정할 때는 결정을 내리는 이유와 예외를 처리할 방법을 분명히 알아야 한다.
- 코드에서 모호성과 부정확은 의견차나 게으름의 결과다.

### G27 : 관례보다 구조를 사용하라.  
- 설계 결정을 강제할 때는 규칙보다 관례를 사용한다.  
- 명명 관례도 좋지만 구조 자체로 강제하면 더 좋다.  
- ex) enum변수가 switch/case 문보다 추상 메서드가 있는 기초 클래스가 좋다.  
switch/case문을 매번 똑같이 구현하게 강제하긴 어렵고 파생 클래스는 추상 메서드를 모두 구현하지 않으면 안됨..

### G28 : 조건을 캡슐화하라
조건문에 대한 이름을 부여한 함수를 만들 것

### G29 : 부정 조건은 피하라

### G30 : 함수는 한 가지만 해야한다.
- 함수를 짜다보면 한 함수 안에 여러 단락을 이어서 일련의 작업을 수행하는 함수는 한 가지만 수행하는 함수가 아니다.
- 한 가지만 수행하는 함수는 좀 더 작은 함수 여럿으로 나눠야 한다.
```java
// 잘못된 함수 - 한 함수 안에 세 가지 임무를 수행하도록 하는 것
public void pay(){
    for( Employee e : employees ){
        if( e.isPayday() ){
            Money pay = e.calculatePay();
            e.deliverPay(pay);
        }
    } 
}
```
```java
// 잘된 함수 - 함수 셋으로 나눠서 세 가지 임무를 수행하도록 하느 ㄴ것.
public void pay(){
    for( Employee e : employees ){
        payIfNecessary(e);
    } 
}

private void payIfNecessary(Employee e){
    if( e.isPayday() ){
        calculateAndDeliverPay(e);
    }
}

private void calculateAndDeliverPay(Employee e){
    Money pay = e.calculatePay();
    e.deliverPay(pay);
}
```

### G31 : 숨겨진 시각적인 결합
시각적인 결합이 필요할 때가 있는데 이떄 결합을 숨기지 말고  
함수 인수를 적절히 배치해서 함수가 호출되는 순서를 명백히 드러내야 한다.
일종의 연결 소자를 새성해서 시간적인 결합을 노출한다.
- ?? 출력인지 안만드는 게 좋다고 그랬는데 이건 어쩔 수 없이 시간적인 결합 노출해야 될 때만 한정되나?
```java
public class MoogDiver {
    Gradient gradient;
    List<SPline> splines;

    public void dive(String reason) {
        Gradient gradient = saturateGradient();
        List<SPline> splines = reticulateSplines(gradeint);
        diveForMoog(splines, reason);
    }
    ...
}
```

### G32 : 일관성을 유지하라
`★` 시스템 전반에 걸쳐 구조가 일관성이 있다면 남들도 일관성을 따르고 보존한다.

### G33 : 경계 조건을 캡슐화하라
경계 조건은 한 곳에서 별도로 처리한다.

### G34 : 함수는 추상화 수준을 한 단계만 내려야 한다.
- 휴리스틱 중 가장 이해가 어렵고 따르기도 어려운 항목..
- 추상화 : 뭉뚱 그려서 다 표현하는 것
- 함수에서 추상화 수준을 분리하면 앞서 드러나지 않았던 새로운 추상화 수준이 드러나는 경우가 빈번하다.

### G35 : 설정 정보는 최상위 단계에 둬라
추상화 최상위 단계에 둬야 할 기본값 상수나 설정 관련 상수를 저차원 함수에 숨겨서는 안된다.  
대신, 고차원 함수에서 저차원 함수를 호출할 때 인수를 넘긴다.  

### G36 : 추이적 탐색을 피한다.
한 모듈은 주변 모듈을 모를 수록 좋고 자신이 직접 사용하는 모듈만 알아야 한다. 
원하는 메서드를 찾느라 객체 그래프를 따라 시스템을 탐색할 필요가 없어야 한다.  

<br>

## 자바

### J1 : 긴 import 목록을 피하고 와일드카드를 사용해라
### J2 : 상수는 상속하지 않는다.   
상수를 상속 계층 맨 위에 숨기는 것은 언어의 범위 규칙을 속이는 행위이다.  
(상수를 인터페이스에 넣은 다음, 그 인터페이스를 상속해서 해당 상수를 사용하는 것)
- ?? 근데 이걸 써도  계속 상속 받고 imple 하고 오면 Employee-Payrollconstatus에 있는 걸 쓰는 건 매 한가지 아님..? 뭐가 달라지나요..?

### J3 : 상수 대 Enum
enum은 이름이 부여된 열거체에 속하기 떄문에  
public satic final int라는 옛날 기교를 사용할 필요가 없다.

<br>

## 이름

### N1 : 서술적인 이름을 사용하라
소프트웨어 가독성의 90%는 이름이 결정한다.  
그러므로 시간을 들여 현명한 이름을 선택하고 유효한 상태로 유지해야한다.

### N2 : 적절한 추상화 수준에서 이름을 선택하라  
구현을 드러내는 이름을 피하고  
작업 대상 클래스나 함수가 위치하는 추상화 수준을 반영하는 이름을 선택해라  

### N3 : 가능하다면 표준 명명법을 사용해라
기존 명명법을 사용하는 이름은 이해하기 더 쉽다.  
프로젝트에 유효한 의미가 담긴 이름을 많이 사용할 수록 독자가 코드를 이해하기 쉬워진다.

### N5 : 명확한 이름
함수/변수의 목적을 명확히 밝히는 이름을 선택한다.  
길다는 단점을 서술성이 충분히 메꾼다.

### N6 : 인코딩을 피하라
이름에 유형/범위 정보를 넣어서는 안된다.   
오늘날 환경은 이름을 조작하지 않고도 모든 정보를 제공한다.  

### N7 : 이름으로 부수 효과를 설명하라
- 함수, 변수, 클래스가 하는 일을 모두 기술하는 이름을 사용한다.
- 특정 객체의 모든 기능에 대해 설명할 수 있는 이름으로 짓기

<br>

## 테스트

### T1 : 불충분한 테스트
테스트 케이스는 잠재적으로 깨질 만한 부분을 모두 테스트 해야한다.

### T2 : 커버리지 도구를 사용해라
커버리지 도구는 테스트가 빠뜨리는 공백을 알려준다.

### T3 : 사소한 테스트를 건너띄지 마라
사소한 테스트가 제공하는 문서적 가치는 구현에 드는 비용을 넘어선다.

### T4 : 무시한 테스트는 모호함을 뜻한다.
- 불분명한 요구사항은 테스트 케이스를 주석으로 처리하거나 테스트 케이스에 @Ignore를 붙여 표현한다.
- 선택 기준은 모호함이 존재하는 테스트 케이스가 컴파일이 가능한지 불가능한지 달려있다.

### T5 : 경계 조건을 테스트 하라

### T6 : 버그 주변은 철저히 테스트 하라
- 버그는 서로 모이는 경향이 있다.
- 왜냐면 그 버그 기준으로 밑에도 짰을거라서

### T7 : 실패 패턴을 살펴라
테스트 케이스가 실패하는 패턴으로 문제를 진단할 수 있따.  
합리적인 순서로 정렬된 꼼꼼한 테스트 케이스는 실패 패턴을 드러낸다.

### T8 : 테스트 커버리지 패턴을 살펴라.
통과하는 테스트가 실행 하거나 하지 않는 코드를 살펴보면 실패하는 테스트 케이스의 실패 원인이 드러난다.

### T9 : 테스트는 빨라야 한다.
느린 테스트 케이스는 실행하지 않게 된다.

## 결론
- 일군의 규칙만 따른다고 깨끗한 코드가 얻어지지 않는다.  
- 휴리스틱 목록의 가치에 기반한 규율과 절제가 핑료하다.