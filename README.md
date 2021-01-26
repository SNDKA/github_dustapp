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

주소를 입력하고 AsyncTask를 이용하여 본격적으로 XML파싱을 시작합니다.

![AsyncTask](https://user-images.githubusercontent.com/78009291/105836939-4d265380-6011-11eb-9e36-4001b06510a9.PNG)
![xml 데이터 파싱 1](https://user-images.githubusercontent.com/78009291/105836943-4dbeea00-6011-11eb-84fa-a1802f39c916.PNG)

그리고 데이터를 외부라이브러리 프로그래스바를 이용해서 수치를 나타냅니다.
먼저 buile.gradle에 implementation 'com.dinuscxj:circleprogressbar:1.3.0'라이브러리를 추가하고,
레이아웃에서 프로그래스바를 만들어줍니다. 그 후에, 데이터를 프로그래스바에 넣어 출력해주고 파싱을 마무리 짓습니다.
![xml 데이터 파싱 2](https://user-images.githubusercontent.com/78009291/105838081-f4f05100-6012-11eb-8bf1-36281836957b.PNG)
![xml 데이터 파싱 3](https://user-images.githubusercontent.com/78009291/105838650-b7d88e80-6013-11eb-8fa0-0aaa16e75b3a.PNG)

프로그래스바도 마무리 짓습니다.

![프로그래스바](https://user-images.githubusercontent.com/78009291/105838560-95467580-6013-11eb-96fa-642f7f2f3cc5.PNG)


-----
#### 4. 실행화면

코딩이 다 끝나고 나면 최종화면은 아래와 같이 나올 것 입니다.
<img src = "https://user-images.githubusercontent.com/78009291/105839344-ae9bf180-6014-11eb-84e9-56cde0af3712.jpg" height = "500" width = "300">

그리고 코드를 작성하실 때 다른 지역의 측정소를 기준으로 하고 싶다면 URL을 해당 측정소의 URL로 변경해주시면 됩니다.
측정소 이름도 stationName 코드를 통해서 받아오고 싶었지만, 코드들에 문제는 없는데 받아오질 못해서
텍스트로 측정소 이름을 지정했습니다.
