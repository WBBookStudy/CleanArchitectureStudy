# 25. 계층과 경계
시스템이 세 가지 컴포넌트(UI, 업무 규칙, 데이터베이스)로만 구성된다고 생각할 수 있으나, 실제로 대규모 시스템에서는 그 이상이 될수도 있다.

## 움퍼스 사냥 게임
 - GO EAST, SHOOT WEST와 같은 단순한 명령어를 사용한다.
 - 플레이어는 명령어를 입력하며, 컴퓨터는 플레이어가 보고 듣고 경험한 것들로 응답한다.
 - 텍스트 기반 UI는 유지하되, 게임 규칙과 UI를 분리해서 제품을 여러 시장에서 다양한 언어로 발매한다고 가정해보자.

> 소스 코드 의존성을 적절히 관리하면 어떤 언어를 UI가 사용하더라도 게임 규칙을 재사용 할 수 있다.

![001](https://user-images.githubusercontent.com/50142323/148904603-03abfcaf-a636-40a7-8c39-1a61959118fb.png) 

> 게임상태를 저장하는 컴포넌트가 있더라도 마찬가지로 의존성 관리가 되어 고수준인 GameRules를 의존하는 형태이다.

![002](https://user-images.githubusercontent.com/50142323/148904957-aa29f902-4664-4a57-b9e3-ea5b3f985c91.png)

## 클린 아키텍처?
UI에서 언어가 유일한 변경의 축은 아니다. 예를 들면 텍스트를 주고받는 메커니즘을 다양하게 만들고 싶을수도 있다. 
따라서 아래 그림과 같이 해당 변경의 축에 의해 정의되는 아키텍처 경계를 살펴볼 수 있다
![003](https://user-images.githubusercontent.com/50142323/148905256-bce454f3-0e26-4c87-a4c4-6e72a6f378be.png)
 - 점선으로 된 테두리는 API를 정의하는 추상 컴포넌트를 가리키며, 해당 API는 추상 컴포넌트 위나 아래의 컴포넌트가 구현한다.
 - GameRules는 GameRules가 정의하고 Language가 구현하는 API를 통해 Language와 통신한다.
 - API는 사용하는 쪽에 정의되고 소속된다. English, SMS, CloudData와 같은 변형들은 추상 API 컴포넌트가 정의하는 다형적 인터페이스를 통해 제공되고, 실제로 서비스하는 구체 컴포넌트가 이를 구현한다.

> 변형들을 모두 제거하고 순전히 API 컴포넌트만 집중하면 아래와 같은 다이어그램이 된다.

![004](https://user-images.githubusercontent.com/50142323/148905793-4c0e9ef9-8e37-45cc-aeab-8b6f18fd30b8.png)
 - 정보의 흐름 : 사용자 입력 -> TextDelivery -> Language(GameRules에 적합한 명령어로 번역) -> GameRules -> 우측 하단의 DataStorage로 데이터 이동 -> GameRules 출력 -> Language(적절한 언어로 번역) -> TextDelivery -> 사용자 output
 - 정보가 흐르는 방향을 보면 데이터 흐름을 두 개의 흐름으로 효과적으로 분리한다.
 - 왼쪽의 흐름은 사용자와의 통신에 관여하며, 오른족의 흐름은 데이터 영속성에 관여한다.
 - 두 흐름이 GameRules에서 만나, GameRules는 두 흐름이 모두 거치게 되는 데이터에 대한 최종적인 처리기가 된다.

## 흐름 횡단하기
 - 네트워크상에서 여러 사람이 함께 플레이한다고 가정하고 네트워크를 통한 여러 사람과의 플레이를 위해 네트워크 컴포넌트를 추가해보자.
 - 시스템이 복잡해질수록 컴포넌트 구조는 더 많은 흐름으로 분리될 것이다.
![005](https://user-images.githubusercontent.com/50142323/148906623-95b8d54c-dc2c-4bf2-8bdb-165ec0576dc1.png)

## 흐름 분리하기
 - 모든 흐름이 상단의 단일 컴포넌트에서 만나는 것은 아니다.
 - 만약 GameRules 컴포넌트가 플레이어의 이동 관련된 정책(Move Management)과 그보다 고수준 정책인 플레이어 정책(Player Management)이 있다면 GameRules 컴포넌트를 아래와 같이 분리할 수도 있다.
![006](https://user-images.githubusercontent.com/50142323/148907752-c065c532-b9d7-48f0-a5c3-3fb2856dff97.png)


