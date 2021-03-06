# 12. 컴포넌트
컴포넌트 == 배포단위.  
컴포넌트가 마지막에 어떤 형태로 배포되든, 잘 설계된 컴포넌트라면 반드시 독립적으로 배포 가능한, 따라서 독립적으로 개발 가능한 능력을 갖춰야한다.

## 컴포넌트의 간략한 역사
```
			*200 
			TLS
START,		CLA
			TAD BUFR
			...
			...
			...
			...
```
> PDP-9 프로그램. 프로그램 시작부에 있는 200 명령어는 메모리 주서 200에 로드할 코드를 생성하라고 컴파일에게 알려준다.

![KakaoTalk_Photo_2021-11-30-17-00-25](https://user-images.githubusercontent.com/60125719/144008257-57d774fc-2ad0-4609-8078-3127c444acd2.jpeg)
> 외부 라이브러리를 사용하고싶다면? 먼저 컴파일러로 컴파일 한 후(시간 절약을 위해) 특정 메모리의 주소값에 올려놓는다.

### 하지만 애플리케이션이 점점 커져 기존 메모리 할당공간을 초과한다면?
![KakaoTalk_Photo_2021-11-30-17-04-31](https://user-images.githubusercontent.com/60125719/144008733-c6f30398-3bcf-4096-bc0d-8de92aa297b5.jpeg)
> 이렇게 했다고 한다.. 해결방법이 필요했음

## 재배치성
해결책은 재배치가 가능한 바이너리였다.  
지능적인 로더를 사용해서 메모리에 재배치할 수 있는 형태의 바이너리를 생성하도록 컴파일러를 수정하자.  
이때 로더는 재배치 코드가 자리할 위치 정보를 전달받고, 재배치 코드에는 로드한 데이터에서 어느 부분을 수정해야 정해진 주소에 로드할 수 있는지를 알려주는 플래그가 추가됐다.  
대개 이러한 플래그는 바리너리에서 참조하는 메모리의 시작주소였다.  
또한 컴파일러는 재배치 가능한 바이너리 안의 함수 이름을 메타데이터 형태로 생성하도록 수정했다. 만약 프로그램이 라이브러리 함수를 호출한다면 컴파일러는 라이브러리 함수 이름을 외부참조(external reference)로 생성했다.  
반면 라이브러리 함수를 정의하는 프로그램이라면 컴파일러는 해당 이름을 외부정의(external definition)로 생성했다.  
-> Linking loader

## 링커
시간이 지날수록 프로그램은 커져갔고, 링킹로더는 너무 느렸다.  
마침내 로드와 링크가 두 단계로 분리되었다.  
링커라는 별도의 애플리케이션으로 이 작업을 처리하도록 만들었다. 링커는 링크가 완료된 재배치 코드를 만들어 주었고, 그 덕분에 로더의 로딩 과정이 빨라졌다.  
소스 모듈은 .c 파일에서 .o 파일로 컴파일된 후, 링커로 전달되어 빠르게 로드될 수 있는 형태의 실행파일로 만들어졌다.  



















































