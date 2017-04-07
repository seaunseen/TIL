# html 개요

웹브라우저, 서버, 웹 어플리케이션은 모두 HTTP를 통해서 서로 대화한다.

1장에서는 다음에 대해 이야기 한다.

	- 얼마나 많은 클라가 서버와 통신하는지
	- 리소스(웹 컨텐츠)는 어디서 오는지
	- 웹 트랙잭션 동작 방법
	- HTTP 통신을 위해 사용하는 메시지 형식
	- HTTP 기저 TCP 네트워크 전송
	- 여러 종류의 HTTP 프로토콜
	- 다양한 HTTP 구성요소

## 1.1 HTTP 인터넷 멀티미디어 배달부

하루에 수십억 개의 이미지, 동영상, HTML 파일이 움직인다.

HTTP는 신뢰성 있는 데이터 전송 프로토콜을 사용하기 때문에, 데이터가 지구 반대편에서 오더라도 손상 되지 않는다.


## 1.3 리소스

웹 서버는 웹 리소스를 관리하고 제공한다. 가장 단순한 웹 리소스는 웹 서버 파일 시스템의 정적 파일이다.

정적파일은 `.txt`, `.html`, `.doc`, `.pdf`, `.jpg` 등이 있다.

그러나 리소스는 반드시 정적 파일 이지 않아도 된다. 리소스는 요청에 따라 `콘텐츠를 생산하는 프로그램` 이 될 수도 있다.

누구인지, 어떤 정보 요청 했는지, 시간에 따라 다른 컨텐츠를 생성한다.

또한, 카메라에서 라이브 영상 가져오기, 주식 거래, 부동산 DB 검색, 쇼핑몰에서 선물 구입할 수 있게도 해준다.

요약하자면 어떠한 종류의 컨텐츠도 리소스가 될 수 있다.

지역 공공 도서관의 서가를 탐색하는 웹 게이트웨이도 리소스다.


### 1.3.1 미디어 타입

HTTP는 웹에서 전송되는 객체 각각에 신중하게 **`MIME`** **타입이라는 데이터 포맷 라벨**을 붙인다.

Multipurpose internet mail extensions은 각기 다른 전자메일 시스템 사이에서 메시지가 오갈 때 겪는 문제점을 해결하기 위해 설계 되었다.

MIME은 이메일에서 워낙 잘 동작했기 때문에, HTTP에서도 멀티미디어 컨텐츠를 기술하고 라벨을 붙이기 위해 채택되었다.

웹 서버는 모든 HTTP 객체 데이터에 MIME 타입을 붙인다.

MIME은 `/`으로 구분된 주 타입과 부 타입으로 이루어진 문자열 라벨이다.

	- text/html
	- text/plain
	- image/jpeg


### 1.3.2 URI

웹 서버 리소스는 각자 이름을 가지고 있다. 그래서 클라는 관심 있는 리소스를 지목할 수 있다.

서버 리소스 이름은 통합 자원 식별자 URI로 불린다.


```
	http://www.joes-hardware.com/specials/saw-blade.gif
```

	- `http://` : http프로토콜 사용해라
	- `www.joes-hardware.com` :  www.~~.com 으로 이동해라
	- `/specials/saw-blade.gif` : 라고 불리는 `리소스`를 가져와라.


## 1.4 트랙잭션

클라가 웹서버와 리소스를 주고받기 위한 HTTP 사용 방법

HTTP 트랜잭션은 `요청 명령`과 `응답 결과`로 구성되어 있다.


*요청 명령*
```
GET /specials/saw-blade.gif HTTP/1.0
Host: www.joes-hardware.com
```

*응답 결과*
```
HTTP/1.0 200 OK
Content-type: image/gif
Content-length: 8572
```

HTTP 응답 메시지는 트랜잭션의 결과를 포함한다.


### 1.4.3 웹 페이지는 여러 객체로 이뤄질 수 있다.

웹 서비스는 하나의 작업 수행을 위해 **여러 HTTP 트랜잭션** 을 수행한다.

예를 들어, 시각적으로 풍부한 웹페이지를 가져올 때 대량의 HTTP 트랜잭션을 수행한다.

	1. 페이지 레이아웃을 서술하는 HTML 뼈대를 한 번의 트랜잭션으로 가져온다
	2. 첨부된 이미지
	3. 그래픽 조각
	4. 자바 애플릿

등을 가져오기 위해 추가 HTTP 트랜잭션을 수행한다.

1~4의 리소스들은 **다른 서버에 위치**할 수도 있다.

이와 같이 **웹페이지는 하나의 리소스가 아닌 리소스의 모음**이다.


## 1.6 TCP Connection

HTTP는 **`애플리케이션 계층`** 프로토콜이다.

**`네트워크 계층`** 은 신경쓰지 않는다. 대신 대중적인 TCP/IP에게 맡긴다.

	- 오류 없는 데이터 전송
	- 순서에 맞는 전달 (데이터는 언제나 보낸 순서대로 도착)
	- 조각나지 않는 데이터 스트림(언제든 어떤 크기로도 보낼 수 있다.)

TCP/IP는

	- 패킷 교환 네트워크 프로토콜의 집합이다.
	- 네트워크와 하드웨어 특성을 숨기고, 어떤 종류의 컴퓨터나 네트워크든 서로 신뢰성 있는 의사소통을 한다.


### 1.6.2 접속, IP 주소, 포트 번호

IP 주소와 포트번호를 사용해 클라와 서버 사이에 TCP/IP 커넥션을 생성

TCP는 서버 컴퓨터의 `IP 주소`, 서버에서 실행 중인 프로그램이 사용 중이 `포트 번호`가 필요하다.

URL이란 리소스에 대한 주소라고 언급한 적이 있다.

URL은 그 리소스를 가진 장비에 대한 IP주소를 알려줄 수 있다.

호스트 명은 DNS라 불리는 장치를 통해 쉽게 IP로 변환된다.


## 1.8 웹의 구성요소

프락시
: 클라와 서버 사이에 위치한 HTTP 중개자

캐시
: 많이 찾는 웹페이지를 클라 가까이에 보관하는 HTTP 창고

게이트웨이
: 다른 어플리케이션과 연결된 특별한 웹 서버

터널
: 단순히 HTTP 통신을 전달하기만 하는 특별한 proxy


### 1.8.1 proxy

proxy는 클라와 서버 사이에 위치한다.

클라는 모든 HTTP 요청을 받아 서버에 전달한다.

**proxy는 사용자를 대신해 서버에 접근한다.**

proxy는 주로 `보안`을 위해 사용된다. 또한, 요청과 응답을 필터링 한다.

무엇인가 다운받을 떄, 바이러스 검출. 학생들의 성인 컨텐츠 차단.


### 1.8.2 캐시

웹 캐시와 cache proxy는 자신을 거쳐가는 문서들 중 자주 찾는 것의 사본을 저장해두는, 특별한 종류의 HTTP proxy server다.

**클라는 멀리 떨어진 웹 서버보다 근처의 캐시에서 더 빨리 문서를 다운 받을 수 있다.**

HTTP는 캐시를 효율적으로 동작하게 하고 **캐시된 콘텐츠를 최신으로 유지한다.**



### 1.8.3 게이트웨이

다른 서버들의 중개자다.

HTTP 트래픽을 다른 프로토콜로 변환하기 위해 사용된다.

FTP URI에 대해 HTTP 요청을 받은 뒤, FTP 프로토콜을 이용해 문서를 가져온다.


### 1.8.4 터널

두 커넥션 사이에서 raw data를 열어보지 않고 그대로 전달하는 HTTP application

HTTP 데이터를 하나 이상의 HTTP 연결을 통해 `그대로` 전송해주기 위해 사용한다.

암호화된 SSL 트래픽을 HTTP 커넥션으로 전송하여 웹 트래픽만 허용하는 사내 방화벽을 통과시키는 것

어려우니까 나중에 이해하자..