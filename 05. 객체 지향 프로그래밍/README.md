# 5. 객체 지향 프로그래밍
좋은 아키텍처를 만드는 것은 객체 지향(Object-Oriented, OO) 설계 원칙을 이해하고 응용하는데서 시작한다.  
OO의 본질을 설명하기 위해 세가지 주문에 기대는 부류가 있다.
그들은 객체지향의 본질을 설명하기 위해 객체지향은 다음 세 가지 개념을 적절하게 조합하거나, 이 세 가지 요소를 반드시 지원해야 한다고 말한다.  
 - 캡슐화(encapsulation)
 - 상속(inheritance)
 - 다형성(polymorphism)

## 캡슐화?
 - 데이터와 함수를 쉽고 효과적으로 캡슐화하는 방법을 객체지향 언어가 제공한다.
 - 이를 통해 데이터와 함수가 응집력 있게 구성된 집단을 서로 구분 짓는 선을 그을 수 있다.
 - 분선 바깥에서 데이터는 은닉되고, 일부 함수만이 외부에 노출된다. 실제 OO언어에서는 각각 클래스의 private 멤버 데이터와, public 멤버 함수로 표현된다.
> point.h
```C
struct Point;
struct Point* makePoint(double x, double y);
double distance (struct Point *p1, struct Point *p2)
```
> point.c

```C
#include "point.h"
#include <stdlib.h>
#include <math.h>

struct Point {
	double x,y;
};


struct Point *makePoint(double x, double y) {
	struct Point* p = malloc(sizeof(struct Point));
 	p->x = x;
    p->y = y;
    return p;
}

dpible distance(struct Point* p1, struct Point* p2) {
	double dx = p1->x - p2->x;
    double dy = p1->y - p2->y;
    return sqrt(dx*dx+dy*dy);
}
```
위 코드에서 point.h를 사용하는 측에서 struct Point의 멤버에 접근할 방법이 전혀 없다.  
사용자는 makePoint() 함수와 distance() 함수를 호출할 수는 있지만, Point 구조체의 데이터 구조체가 어떻게 구현되어 있는지 알 수 없다.  
이것이 완벽한 캡슐화이다.
이후 C++라는 형태로 OO가 등장했고, C가 제공하던 완벽한 캡슐화가 깨졌다.  
C++ 컴파일러는 기술적인 이유(컴파일러는 클래스의 인스턴스 크기를 알 수 있어야 한다.)로 클래스의 멤버 변수를 해당 클래스의 헤더 파일에 선언할 것을 요구했다.  
따라서 앞의 Point 프로그램은 C++에서 아래와 같이 변경해야 한다.  

> point.h
```C++
class Point {
public:
	Poimt(double x, double y);
    double distance(const Point& p) const;
   
private:
	double x;
    double y;
};
```
> point.cc
```C++
#include "point.h"
#include <math.h>

Point::Point(double x, double y)
: x(x), y(y)
{}

double Point::distance(const Point& p) coonst {
	double dx = x-p.x;
    double dy = y-p.y;
    return sqrt(dx*dx + dy*dy);
}
```
이제 point.h 헤더 파일을 사용하는 측에서는 멤버 변수인 x와 y를 알게 된다.  
물론 컴파일러가 멤버 변수에 접근하는 일은 컴파일러가 막겠지만, 사용자는 멤버 변수가 존재한다는 사실 자체를 알게 된다. 이는 캡슐화가 깨진 것이다.  
언어에 public, private protected 키워드를 도입함으로써 불완전한 캡슐화를 어느 정도 보완하기는 했지만 이는 임시방편일 뿐이다.  
자바와 C#에서는 헤더와 구현체를 분리하는 방식을 모두 버렸고, 이로 인해 캡슐화는 더욱 심하게 훼손되었다.  
따라서 객체지향은 캡슐화에 의존하지 않는다고 볼 수 있고, 많은 객체지향 언어가 캡슐화를 거의 강제하지 않는다.  

## 상속?
상속은 OO 언어가 확실히 제공하고 있다.  
하지만 상속이란 단순히 어떤 변수와 함수를 하나의 유효 범위로 묶어서 재정의하는 일에 불과하다.  
사실상 아래 코드와 같이 언어의 도움 없이 구현할 수 있었다.  
> namedPoint.h
```C
struct NamedPoint;

struct NamedPoint* makeNamedPoint(double x, double y, char* name);
void setName(struct NamedPoint* np, char* name);
char* getName(struct NamedPoint* np);
```

> namedPoint.c
```C
#include "namedPoint.h"
#include <stdlib.h>

struct NamedPoint {
	double x, y;
	char* name;
};

struct NamedPoint* makeNamedPoint(double x, double y, char* name) {
	struct NamedPoint* p = malloc(sizeof(struct NamedPoint));
	p->x = x;
	p->y = y;
	p->name = name;
	return p;
}

void setName(struct NamedPoint* np, char* name) {
	np->name = name;
}

char* getName(struct NamedPoint* np) {
	return np->name;
}
```


> main.c
```C
#include "point.h"
#include "namedPoint.h"
#include <stdio.h>

int main(int ac, char** av) {
	struct NamedPoint* origin = makeNamedPoint(0.0, 0.0, "origin");
	struct NamedPoint* upperRight = makeNamedPoint(1.0, 1.0, "upperRight");
	printf("distance=%f\n", distance((struct Point*) origin, (struct Point*) upperRight));
}
```
main 프로그램을 살펴보면 NamedPoint 데이터 구조가 마치 Point 데이터 구조로부터 파생된 구조인 것처럼 동작한다는 사실을 볼 수 있다.  
이는 NamedPoint에 선언된 두 변수의 순서가 Point와 동일하기 때문이다.  
이는 NamedPoint가 순전히 Point를 포함하는 상위 집합으로, Point에 대응하는 멤버 변수의 순서가 그대로 유지되기 때문이다.  
C++에서는 이 방법을 이용해서 단일 상속을 구현했다.  
이 방법은 상속과 비슷하다고 말하기에는 어폐가 있다.  
 - 실제 상속만큼 편리한 방식은 절대 아니다.
 - 이 방법으로 다중 상속을 구현하기에 어려움이 있다.
또한 main.c에서 NamedPoint 인자를 Point로 타입을 강제로 변환한 점도 확인할 수 있다.  
OO언어에서는 이러한 업캐스팅(upcasting)이 암묵적으로 이뤄진다.  
따라서 OO언어가 완전히 새로운 개념을 만들지는 못했지만, 데이터 구조에 가면을 씌우는 일을 상당히 편리한 방식으로 제공했다고 볼 수는 있다.

## 다형성?
객체 지향 언어가 있기 이전에도 다형성을 표현할 수 있는 언어는 있었다.
 - 함수를 가리키는 포인터를 응용한 것이 다형성이다.
 - 객체지향 언어는 다형성을 새롭게 만들어 제공하진 않지만, 다형성을 좀 더 안전하고 더욱 편리하게 사용할 수 있게 해준다.
 - 포인터를 안전하게 사용하려면 관례를 기억하고 따라야 하는데 객체지향 언어는 이러한 관례를 없애줌으로써, 실수할 위험이 없다.
 - 이러한 이유로 객체지향 언어는 제어흐름을 간접적으로 전환하는 규칙을 부과한다고 결론지을 수 있다.
### 다형성이 가진 힘
 - 플러그인 아키텍처(plugin architecture)는 장치 독립성을 지원하기 위해 만들어졌고 적용됐다.
 - OO의 등장 이전까지 프로그래머들은 이러한 개념을 직접 작성하는 프로그램에서 적용하지 않았는데, 함수를 가리키는 포인터는 위험을 수반하기 때문이었다.
 - 객체지향의 등장으로 언제 어디서든 플러그인 아키텍처를 적용할 수 있게 되었다.
### 의존성 역전
![01](https://user-images.githubusercontent.com/50142323/141290874-6fec1307-6fc7-4c0a-b651-f391318c0ff3.png)
위 전형적힌 호출 트리를 보자.  
main 함수가 고수준 함수를 호출하고, 고수준 함수는 다시 중간 수준 함수를 호출하며, 중간 수준 함수는 다시 저수준 함수를 호출한다.  
이러한 호출 트리에서 소스 코드 의존성의 방향은 반드시 제어흐름을 따르게 된다.  
이러한 제약 조건으로 인해 제어흐름은 시스템의 행위에 따라 결정되며, 소스 코드 의존성은 제어흐름에 따라 결정된다.  
하지만 다형성을 적용하면 얘기가 다르다.  
![02](https://user-images.githubusercontent.com/50142323/141291063-19be7595-06d5-444e-9bae-6ca9ea52eaf1.png)
위 그림에서 HL1 모듈은 ML1 모듈의 F() 함수를 호출한다. 소스코드에서는 HL1 모듈은 인터페이스를 통해 F() 함수를 호출한다.  
이 인터페이스는 런타임에는 존재하지 않는다. HL1은 단순히 ML1 모듈의 함수 F()를 호출할 뿐이다.  
하지만 ML1과 I 인터페이스 사이의 소스코드 의존성(상속 관계)이 제어 흐름과는 반대이다.  
이를 의존성 역전이라 부른다.  
OO 언어가 다형성을 안전하고 편리하게 제공한다는 것은 소스 코드 의존성을 어디에서든 역전시킬 수 있다는 뜻이기도 하다.  
이러한 접근법을 사용한다면 OO 언어로 개발된 시스템을 다루는 소프트웨어 아키텍트는 시스템의 소스 코드 의존성 전부에 대해 방향을 결정할 수 있는 절대적인 권한을 갖는다.  
즉, 소스 코드 의존성이 제어흐름의 방향과 일치되도록 제한되지 않는다.  
![03](https://user-images.githubusercontent.com/50142323/141291368-af33599b-845b-4408-8077-859f0360566a.png)
위 예시 처럼 업무 규칙이 데이터베이스와 사용자 인터페이스에 의존하는 대신에, 시스템의 소스 코드 의존성을 반대로 배치하여 데이터베이스와 UI가 업무 규칙에 의존하게 만들 수 있다.  
즉, UI와 데이터베이스가 업무 규칙의 플러그인이 되고 따라서, 업무 규칙의 소스 코드에서는 UI나 데이터베이스를 호출하지 않는다.  
따라서 업무 규칙을 UI와 데이터베이스와는 독립적으로 배포할 수 있다.  
UI나 데이터베이스에서 발생한 변경사항은 업무 규칙에 일절 영향을 미치지 않는다. 즉 이들 컴포넌트는 독립적으로 배포 가능하다.
 - 배포 독립성 - 컴포넌트의 소스 코드가 변경되면, 해당 코드가 포함된 컴포넌트만 다시 배포하면 된다.
 - 개발 독립성 - 배포 독립성이 있으면,서로 다른 팀에서 각 모듈을 독립적으로 개발할 수 있다.
