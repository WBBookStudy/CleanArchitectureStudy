# 08. OCP: 개방-폐쇄 원칙
소프트웨어 개체는 확장에는 열려 있어야 하고, 변경에는 닫혀 있어야 한다.

## 사고 실험
![KakaoTalk_Photo_2021-11-28-19-32-01](https://user-images.githubusercontent.com/60125719/143764302-f5cabfcc-1653-49b8-9da3-56b0298e7c69.jpeg)
> 보고서 생성이 두 개의 책임으로 분리된다.  
이처럼 책임을 분리했다면, 두 책임 중 하나에서 변경이 발생하더라도 다른 하나는 변경되지 않도록 소스 코드 의존성도 조직화해야한다.  또한 새로 조직화한 구조에서는 행위가 확장될 때 변경이 발생하지 않음을 보장해야 한다.

![KakaoTalk_Photo_2021-11-28-19-35-25](https://user-images.githubusercontent.com/60125719/143764370-c5842438-73a4-4e50-a87c-7277fb55ca34.jpeg)
> 이런 목적을 달성하려면 처리과정을 클래스 단위로 분할하고, 이들 클래스를 컴포넌트 단위로 구분해야한다.  
### 모든 의존성이 소스 코드 의존성을 나타낸다.
FinancialDataMapper는 구현 관계를 통해 FinancialDataGateway를 알고 있지만 FinancialDataGateway는 FinancialDataMapper에 대해 아무것도 알지 못한다.  
### 이중선은 화살표와 오직 한 방향으로만 교차한다.
![KakaoTalk_Photo_2021-11-28-19-40-38](https://user-images.githubusercontent.com/60125719/143764490-c63d9902-c598-4495-90f8-453d552af1d7.jpeg)
이 예제의 경우 Presenter에서 발생한 변경으로부터 Controller를 보호하고자 한다. 그리고 View에서 발생한 변경으로부터 Presenter를 보호하고자 한다. Interactor는 다른 모든 것에서 발생한 변경으로부터 보호하고자 한다.  
Interactor는 OCP를 가장 잘 준수할 수 있는 곳에 위치한다. Database, Controller, Presenter, View에서 발생한 어떤 변경도 Interactor에 영향을 주지 않는다.  
-> Interactor가 업무 규칙을 포함하기 때문! 가장 중요한 문제는 Interactor가 담당한다

### 아키텍트는 기능이 어떻게, 왜, 언제 발생하는지에 따라서 기능을 분리하고 분리한 기능을 컴포넌트의 계층구조로 조직화한다. 컴포넌트 계층구조를 이와같이 조직화하면 저수준 컴포넌트에서 발생한 변경으로부터 고수준 컴포넌트를 보호할 수 있다.

## 방향성 제어
FinancialDataGateway 인터페이스는 FinancialReportGenerator와 FinancialDataMapper 사이에 위치하는데, 이는 의존성을 역전시키기 위해서다. FinacialDataGateway 인터페이스가 없었다면, 의존성이 Interactor 컴포넌트에서 Database컴포넌트로 바로 향하게 된다.

## 정보 은닉
FinancialReportRequester 인터페이스는 FinancialReportController가 Interactor 내부에 대해 너무 많이 알지 못하도록 막기 위해서 존재한다. 만약 이 인터페이스가 없었다면, Controller는 FinancialEntities에 대해 추이 종속성(transitive dependency)을 가지게 된다. 추이 종속성을 가지게 되면, 소프트웨어 엔티티는 '자신이 직접 사용하지 않는 요소에는 절대로 의존해서는 안 된다'를 위반하게 된다.  

## 결론
OCP의 목표는 시스셑 을 확장하기 쉬운 동시에 변경으로 인해 시스템이 너무 많은 영향을 받지 않도록 하는 데 있다. 이러한 목표를 달성하려면 시스템을 컴포넌트 단위로 분리하고, 저수준 컴포넌트에서 발생한 변경으로부터 고수준 컴포넌트를 보호할 수 있는 형태의 의존성 계층구조가 만들어지도록 해야 한다.















































