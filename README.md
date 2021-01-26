미세먼지 측정을 위한 애플리케이션 제작
=================================

개요
---------------
이 프로젝트는 팀 프로젝트로 아두이노에서 미세먼지를 측정하고, 그 데이터를 안드로이드로 넘겨받아 공공데이터포털의 오픈API와 비교해서
측정한 값이 맞는지 보기 위한 프로젝트입니다. 
이 글은 안드로이드 스튜디오로 작업하고, 안드로이드만 집중해서 소개를 드립니다.

------------------------------------------

#### 1. 블루투스 연결
아두이노와 안드로이드간의 통신을 위해서는 블루투스 연결을 필요로 합니다.
하지만 아두이노에는 기본적으로 블루투스 통신을 위한 장치가 설치되어 있지않아 HC-06이라는 모듈을 사용해야 합니다.
그 후, 안드로이드 스튜디오에서 bluetoothspp:1.0.0 라이브러리를 추가하고 manifast에 블루투스, 인터넷 권한을 추가합니다.
![권한](https://user-images.githubusercontent.com/78009291/105829019-2d8a2d80-6007-11eb-8770-6e274ba936a5.PNG)

기본 설정을 완료하고 나면 블루투스 연결을 위한 버튼과 MainActivity 코딩을 시작합니다.
<img src="https://user-images.githubusercontent.com/78009291/105829787-26afea80-6008-11eb-9876-52a3859d0cf0.PNG" width="600" height="600">

그리고 버튼을 누르면 블루투스를 연결할 기기 목록을 출력합니다.
![블루투스 목록](https://user-images.githubusercontent.com/78009291/105830816-4abffb80-6009-11eb-810d-25def6a621e2.PNG)

마지막으로 화면에 연결 가능한 기기의 목록이 출력된 후, 화면에 해당 기기의 이름을 클릭하면 
블루투스 연결 혹은 블루투스 연결 실패시 어플을 종료하는 작업을 합니다.
![블루투스 목록에서 연결](https://user-images.githubusercontent.com/78009291/105831095-a68a8480-6009-11eb-8984-9c5f9a089327.PNG)

---
#### 2. 아두이노에서 데이터를 가져와 화면에 출력하기

블루투스 연결을 완료하면 블루투스 통신을 이용해 아두이노에서 측정한 미세먼지 데이터를 가져와 화면에 출력해야 합니다.
저는 외부 라이브러리를 이용하여 실시간 그래프를 사용하여 화면에 표시 해봤습니다.

먼저 bulid.gradle에 
implementation 'com.mysugr.MPAndroidChart:MPAndroidChart:3.1.0-mysugr-1' 를 추가하고, 레이아웃에서 라인차트를 생성합니다.

그 뒤에 클래스를 하나 생성해주고, 그 클래스에 차트라인에 대한 코딩을 합니다. 저는 chartLine로 생성하였습니다.

![차트라인 1](https://user-images.githubusercontent.com/78009291/105833803-0e8e9a00-600d-11eb-9305-5a2f594b5334.PNG)
![차트라인 2](https://user-images.githubusercontent.com/78009291/105833805-0f273080-600d-11eb-9359-205eda037185.PNG)

그리고 데이터를 받아서 그래프에 그려질 선 들을 생성합니다.

![차트에 들어갈 데이터](https://user-images.githubusercontent.com/78009291/105833808-0fbfc700-600d-11eb-9690-f7db8e9ba8ac.PNG)

![차트에 들어갈 데이터 2](https://user-images.githubusercontent.com/78009291/105833806-0f273080-600d-11eb-99d8-0dbd5e7583f7.PNG)

이렇게 하면 chartLine에 대한 클래스는 끝이납니다.

다시 MainActivity로 넘어와서 아두이노에서 데이터를 가져와서 그 값을 그래프에 그려주는 작업을 시작합니다.
![데이터 가져오기](https://user-images.githubusercontent.com/78009291/105834959-83ae9f00-600e-11eb-8107-3cb78141a3d9.PNG)

여기까지 완료하면 값을 가져와 그래프를 그리는 것 까지 끝났습니다.

-----
#### 3. 공공데이터포털에서 오픈API 데이터 파싱하기
오픈API를 파싱하는 방법은 여러가지 언어로 가능하지만 저는 XML을 이용해서 데이터를 파싱하겠습니다.

우선 manifests 안에 application 영역에 android:usesCleartextTraffic="true"를 추가합니다.
이 코드는 text URL을 허용하라는 코드이며 HTTP 사용시 에러가 나지 않게 합니다.

그리고 가져오고자 하는 데이터가 들어있는 주소를 입력합니다.
![데이터 가져오기](https://user-images.githubusercontent.com/78009291/105836948-4e578080-6011-11eb-875a-a3e3e19d8d9d.PNG)


