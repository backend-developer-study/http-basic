# 스프링의 강의 리뷰📽
> LoadMap Part : 모든 개발자를 위한 HTTP 웹 기본 지식  
> Section : 01~04   
> CreateDate : 2022.07.04  
> UpdateDate :

<br></br>

## 목차
### 인터넷 네트워크
- [인터넷 통신](#internet)
- [IP(Internet Protocol)](#IP)
- [TCP, UDP](#TCPUDP)
- [PORT](#PORT)
- [DNS](#DNS)
### URI와 웹 브라우저 요청 흐름
- [URI](#URO)
- [웹 브라우저 요청 흐름](#webBrowserRequestFlow)
### HTTP
- [HTTP](#HTTP)
- [클라이언트 서버 구조](#clientServerStructure)
- [비 연결성(connectionless)](#connectionless)
- [HTTP 메시지](#HTTPMessage)
### HTTP 메서드
- [HTTP API](#HTTPAPI)
- [HTTP Method 종류](#methodType)
- [HTTP 메서드의 속성](#MethodProperty)

<br></br>
<br></br>

# 인터넷 네트워크
## 인터넷 통신<a name="internet"></a>
 - 우리가 느끼는 연결은 다이렉트로 연결되어 있어보이지만,
 - 클라이언트와 서버는 복잡한 인터넷망으로 연결되어 있다.
 - 복잡한 연결에 대하여[링크](https://github.com/Gloom-shin/TIL/blob/64d0ed09ff0438c0bd97f94ea46727d9a411c70b/%EC%9D%B8%ED%84%B0%EB%84%B7/%EC%9D%B8%ED%84%B0%EB%84%B7%EB%8F%99%EC%9E%91%EC%9B%90%EB%A6%AC.md)

<br></br>

## IP(인터넷 프로토콜) <a name="IP"></a>
 - 복잡한 인터넷 사이에서, 정확히 서버와 클라이언트를 가리키기위해 주소가 필요

### IP 역할
 - 지정한 IP주소에 데이터 전달
 - `패킷`(Packet)이라는 통신 단위로 데이터 전달
    - pack(포장하다)과 bucket(바구니)의 합친말로, 대표적인 예시로 택배의 행선지를 표시하는 꼬리표를 붙이는 것을 말한다.
    - 출발지 IP, 목적지 IP 등의 정보가 들어간다.
 - 그래서 `클라이언트 출발 IP` = `서버 도착 IP` , `클라이언트 도착 IP` = `서버 출발 IP`

<p align="center"><img src="https://user-images.githubusercontent.com/104331549/177039515-7ea379e5-7fdd-4d5e-b234-4f3ee731d16a.png" width="60%"></p>

### IP 프로토콜 한계
 - 비연결성 : 받을 대상이 없거나, 서비스 불능 상태여도 전송(집에 주인이 없어도 택배 배송)
 - 비신뢰성 : 패킷이 순서대로 안오면?  , 중간에 패킷이 손실되면?
 - 프로그램 구분 : 노래프로그램과 영상프로그램의 정보가 같이 오면?

<br></br>

## TCP, UDP<a name="TCPUDP"></a>
### 인터넷 프로토콜 스택의 4계층 
<p align="center"><img src="https://user-images.githubusercontent.com/104331549/177039700-a45d785d-2028-46eb-be5a-f64a49859dd4.png" width="40%"></p>

- 위와 같이 TCP/IP 계층은 총 4계층으로 이뤄져있다.
- OSI(국제표준기구) 7계층에서 보다 간략화한 계층이라고 한다.
- 서로 밀접한 관계가 있는 계층들을 묶어서, 보다 실무적이고 효율성있게 4계층으로 나눈 것 이다.

<p align="center"><img src="https://user-images.githubusercontent.com/104331549/177039852-d2154d70-5ac1-4593-989a-01dcc1ea5330.png" width="60%"></p>

### IP 패킷정보 담는 과정
 - 4계층이 IP 패킷정보를 담는 과정은 아래와 같다. 
<p align="center"><img src="https://user-images.githubusercontent.com/104331549/177039743-cc241ce0-9380-453d-8867-6147eac74360.png" width="60%"></p>

 - SOCKET 라이브러리를 통해 전달되며,
 - TCP 정보를 생성하고 메시지 데이터를 포함하며,
 - IP 패킷을 생성하고, TCP 데이터를 포함한다. 
 - IP 패킷정보를 포함하고, 이더넷 프레임 까지 쓰위고 전송하게 된다.
    - [이더넷 프레임](http://www.ktword.co.kr/test/view/view.php?m_temp1=2965)

### TCP 특징 
 - 전송 제어 프로토콜
 - 연결지향 TCP 3 way handshake (가상 연결)
 - 데이터 전달 보증
 - 순서 보장
 - 신뢰할 수 있는 프로토콜
 - 현재는 대부분 TCP 사용 
 

### UDP 특징
 - 사용자 데이터그램 프로토콜
 - 하얀 도화지에 비유(기능이 거의 없음)
 - TCP 3 way handshake X ,데이터 전달 보증 X ,순서 보장 X
 - 단순하고 빠름
 - IP와 거의 같다.(PORT,체크섬 정도만 추가)

> HTTP 3 버전은 UDP를 기반으로 하는 QUIC프로토콜을 사용한다.  
> 그외에도, UDP가 사용되는 분야는 인터넷 전화, 온라인 게임, 멜티미디어 스트리밍이 있다 .

<br></br>

## PORT <a name="PORT"></a>
 - 한번에 여러작업을 하게 되는 경우, `PORT`로 구분한다.
 - TCP 세그먼트 영역에서, 출발지 `PORT`정보와, 목적지 `PORT` 정보가 포함되어 있다.
   - 같은 IP 내에서 프로세스를 구분할 수 있다. 
 - 소켓(Socket)에 포함된다. 
### 특징 
 - 0 ~ 65535 할당 가능
 - 0 ~ 1023: 잘 알려진 포트, 사용하지 않는 것이 좋음
   - FTP - 20, 21
   - TELNET - 23
   - HTTP - 80
   - HTTPS - 443

<br></br>

## DNS <a name="DNS"></a>
 - 도메인 네임 시스템
 - 도메인 명을 IP로 주소로 변환 
   - 흔히 `전화번호부`로 비유
 - 자세히 들어가면 DNS 시스템은 크게 3가지로 나눠지는 데
   - Root DNS 
   - TLD(Top-Level-Domain) 최상위도메인
   - Local DNS
 
### 장점
 - 이름(도메인명)만 알고 있어도 연결가능 
 - 한번도 연결된적이 없어도 연결가능

<br></br>
<br></br>

# URI와 웹 브라우저 요청 흐름
## URI <a name="URI"></a>
 - Uniform Resource Identifier
 - URL(locator) 과 URN(name)로 분류할 수 있다. 
   - [차이점](https://github.com/Gloom-shin/TIL/blob/64d0ed09ff0438c0bd97f94ea46727d9a411c70b/%EC%9D%B8%ED%84%B0%EB%84%B7/URL%EA%B3%BCURI.md)
 - URN은 사용 잘 안한다고 한다.
   - 최근 실습에, 인메모리 DB h2에 연결할 때, JDBC URL이  `jdbc:h2:mem:test` URN이다.

### URI뜻
- Uniform: 리소스 식별하는 통일된 방식
- Resource: 자원, URI로 식별할 수 있는 모든 것(제한 없음)
- Identifier: 다른 항목과 구분하는데 필요한 정보

### URL, URN 뜻 
- URL - Locator: 리소스가 있는 위치를 지정
- URN - Name: 리소스에 이름을 부여
- 위치는 변할 수 있지만, 이름은 변하지 않는다.

### URI의 구조

`scheme:[//[user[:password]@]host[:port]][/path][?query][#fragment]`

(ex)
`file:///C:/Users/shin/Desktop/`
`https://www.google.com:443/search?q=JavaScript`
|part|설명|예시|
|--|--|--|
|scheme|사용할 프로토콜을 뜻하며, `통신프로토콜`이라함|`filel://`  `https://`|
|user, password(선택)|접근하기위한 사용자의 이름과 비밀번호| 거의 사용하지 않음|
|hosts|웹 페이지, 이미지, 동영상 등의 파일이 위치한 웹서버, 도메인 또는 IP|`127.0.0.1`   `www.google.com`|
|port|웹 서버에 접속하기 위한 통로|`:80`  `:443`  `:3000`|
|url-path|웹 서버의 루트 디렉토리로부터 웹페이지, 이미지, 동영상 등의 파일이 위치까지의 경로|`/Users/shin/Desktop/`  `/search`|
|query|웹 서버에 전달하는 추가 질문|`q=JavaScript`|

<br></br>

## 웹 브라우저 요청 흐름 <a name="webBrowserRequestFlow"></a>

### 내가 하는 일
 - 접속하고자 하는 서버 URL 입력
### 웹브라우저(크롬)이 하는 일
 - HTTP 요청 메시지 생성 
 - 전송하기위해, SOCKET 라이브러리를 통해, IP와 PORT 추가 
 - 데이터 전달
### OS(window)가 하는 일
 - TCP/IP패킷 성성 및 포함(전송제어, 순서, 검증정보)
 - HTTP 메시지 포함
### 네트워크 인터페이스가 하는일
 - 이더넷 프레임 씌우기
 - 인터넷으로 전송

> 응답메시지가 만들어 지는 경우도 위와 동일한 메커니즘이며, 응답을 디코딩(해석)하는 작업은 반대로 생각하면 된다.

<br></br>
<br></br>

# HTTP <a name="HTTP"></a>
- HTTP 메시지에 모든 것을 전송
- HTML, TEXT 
- IMAGE, 음성, 영상, 파일
- JSOM, XML(API)
> 거의 모든 형태의 데이터 전송 가능  
> 서버 끼리 데이터를 주고 받을 때도 대부분 HTTP 사용

### HTTP 역사 
 - HTTP/0.9 1991년: GET 메서드만 지원, HTTP 헤더X
 - HTTP/1.0 1996년: 메서드, 헤더 추가
 - HTTP/1.1 1997년: 가장 많이 사용, 우리에게 가장 중요한 버전
   - RFC2068 (1997) -> RFC2616 (1999) -> RFC7230~7235 (2014)
 - HTTP/2 2015년: 성능 개선
 - HTTP/3 진행중: TCP 대신에 UDP 사용, 성능 개선

### 기반 프로토콜
 - TCP: HTTP/1.1, HTTP/2
 - UDP: HTTP/3 [참고링크](https://evan-moon.github.io/2019/10/08/what-is-http3/)
 - 현재 HTTP/1.1 주로 사용  
   - HTTP/2, HTTP/3 도 점점 증가

### HTTP 특징
 - 클라이언트 <--> 서버 구조
 - 무상태 프로토콜(Stateless), 비연결성
 - HTTP 메시지
 - 단순함, 확장가능

<br></br>

## 클라이언트 서버 구조 <a name="clientServerStructure"></a>
 - Request(요청) Response(응답) 구조
 - 클라이언트는 서버에 요청을 보내고, 응답을 대기 
 - 서버가 요청에 대한 결과를 만들어서 응답

### 무상태 프로토콜(Stateless)
 - 서버가 클라이언트의 상태를 보존하지 않는다.
 - 장점 : 서버 확장성이 높음(scale out)
 - 단점 : 클라이언트가 추가 데이터 전송

### 상태유지 
 - 서버가 클라이언트 이전 상태 보존하는 것 

### Stateful, Stateless 차이
#### Stateful(상태유지)
 
```markdown
 - 고객 : 이 **노트북** 얼마인가요?
 - 점원 : 100만원 입니다. **(노트북 상태 유지)**

 - 고객 : **2개** 구매하겠습니다.
 - 점원 : 200만원입니다. 신용카드, 현금중에 어떤 걸로 구매 하시겠어요? **(노트북, 2개 상태유지)**

 - 고객 : **신용카드**로 구매하겠습니다.
 - 점원 : 200만원 결제 완료 되었습니다.**(노트북, 2개, 신용카드 상태유지)**
```
#### Stateless(무상태)

```markdown
 - 고객 : 이 **노트북** 얼마인가요?
 - 점원**A** : 100만원 입니다.

 - 고객 : **노트북 2개** 구매하겠습니다.
 - 점원**B** : 노트북 2개 200만원입니다. 신용카드, 현금중에 어떤 걸로 구매 하시겠어요? 

 - 고객 : **노트북 2개를 신용카드**로 구매하겠습니다.
 - 점원**C** : 200만원 결제 완료 되었습니다.
```
 - 상태유지의 경우 다른 점원으로 바뀌면 안된다.
   - 서버가 정보를 가지고 있는 형태이다.  
   - 서버가 장애가 나면, 처음부터 다시 해야된다. 
 - 무상태의 경우 중간에 점원이 바뀌어도 문제가 나타나지않는다. 
   - 갑자기 클라이언트 요청이 증가해도 서버를 대거 투입할 수 있다. 
   - 응답 서버를 쉽게 증설할 수 있다. (확장성이 좋음)
   - 클라이언트가 (많은)정보를 다 가지고 있어야한다.
   
> 로그인한 사용자가 로그인했다는 상태를 서버에 유지해야되기에, 상태유지랑 무상태를 같이 혼용해서 사용한다. 

<br></br>

## 비연결성 <a name="connectionless"></a>
> 연결을 유지하는 모델은, 업무가 없을 때에도 서버 자원을 계속 소모해야된다.

 - HTTP는 기본이 연결을 유지하지 않는 모델
 - 요청과 응답이 끝나면 연결을 끊어버려 불필요한 자원낭비를 없애 최소한의 자원을 유지한다.(서버 자원 효율적 사용)
 - 일정시간 동안 수천명이 서비스를 사용해도 실제 서버에서 동시에 처리하는 요청은 수십개 이하로 매우 작음

### 한계점 
 - 매번 연결을 새로 맺어야함. 
   - 즉, 수많은 자원(이미지, css등)을 함께 다운로드하는 작업이 반복됨

> 지속연결(Persistent Connections)로 문제 해결   
> 연결하는 방법은 여러가지가 있지만, 대표적으로 몇초간 유지하는 방법이 있다.


## HTTP 메시지 <a name="HTTPMessage"></a>

<p align="center"><img src="https://user-images.githubusercontent.com/104331549/177070891-b882b878-77ab-4b24-ba6f-e580720e87fa.png" width="60%"></p>

- HTTP 메시지는 기본적인 틀이 있으며, `요청 메시지`와 `읍답 메시지` 구조가 다르다. 

### 시작라인
#### 요청메시지 
 - HTTP 메서드 : GET, POST, PUT, DELETE
 - 요청 대상 : 절대경로 + 쿼리문
 - HTTP Version

#### 응답메시지
 - HTTP 버전 
 - HTTP 상태코드 : 요청 성공/실패를 나타냄
   - 200: 성공
   - 400 : 클라이언트 요청 오류
   - 500 : 서버 내부 오류
 - 이유 문구 : 사람이 이해할 수 있는 짧은 문구 

<br></br>
### HTTP 헤더
 - header-field = field-name ":" OWS field-value OWS (OWS:띄어쓰기 허용)
 - field-name은 대소문자 구문 없음
 - 현재(2022년 기준) 웹브라우저에서 [HTTP 헤더 확인하는 법](https://github.com/Gloom-shin/TIL/blob/64d0ed09ff0438c0bd97f94ea46727d9a411c70b/%EC%9D%B8%ED%84%B0%EB%84%B7/HTTP%20Messages.md)

#### 용도 
  - 모든 부가정보를 담고있다. 
    - 메시지 바디의 내용, 메시지 바디크기, 압축, 인증, 요청 클라이언트 정보, 서버 애플리케이션 정보, 캐시 관리 정보
  - 표준 헤더가 너무 많음

  <br></br>

### HTTP 바디 
 - 실제 전송할 데이터 
 - HTML 문서, 이미지, 영상, JSON등 byte로 표현할 수 있는 모든 데이터 전송 가능

<br></br>
<br></br>

# HTTP 메서드
## HTTP API<a name ="HTTPAPI"></a>
 - 회원 정보 관리 API에 필요한 기능은 아래와 같다. 
   - 회원 목록 조회 (/read-member-list)
   - 회원 조회(/read-member-by-id)
   - 회원 등록(/create-member)
   - 회원 수정(/update-membe)
   - 회원 삭제/update-membe)
 - 위 기능을 하기위해 URI가 5개가 필요하다.

> 이것은 좋은 URI 설계일까?   
> 가장 중요한 것은 **리소스 식별**

- 회원이라는 개념 자체가 리소스다.
  - 회원 목록 조회 /members
  - 회원 조회 /members/{id}
  - 회원 등록 /members/{id}
  - 회원 수정 /members/{id}
  - 회원 삭제 /members/{id}
- 이렇게 `리소스`와 리소스를 대상으로 하는 `행위`로 분리
   - 리소스 : 회원
   - 행위 ㅣ 조회, 등록, 삭제, 변경

<br></br>
<br></br>

## HTTP 메서드 종류<a name ="methodType"></a>
### 주요 메서드
 - GET: 리소스 조회
 - POST: 요청 데이터 처리, 주로 등록에 사용
 - PUT: 리소스를 대체, 해당 리소스가 없으면 생성
 - PATCH: 리소스 부분 변경
 - DELETE: 리소스 삭제

### 기타 메서드 
- HEAD: GET과 동일하지만 메시지 부분을 제외하고, 상태 줄과 헤더만 반환
- OPTIONS: 대상 리소스에 대한 통신 가능 옵션(메서드)을 설명(주로 CORS에서 사용)
- CONNECT: 대상 자원으로 식별되는 서버에 대한 터널을 설정
- TRACE: 대상 리소스에 대한 경로를 따라 메시지 루프백 테스트를 수행

<br></br>

### GET 
- 리소스 조회
- 서버에 전달하고 싶은 데이터는 query(쿼리 파라미터, 쿼리 스트링)를 통해서 전달
- 메시지 바디를 사용해서 데이터를 전달할 수 있지만, 지원하지 않는 곳이 많아서 권장하지 않는다.

### POST
- 요청 데이터 처리
- 메시지 바디를 통해 서버로 요청 데이터 전달
- 서버는 요청 데이터를 처리
- 메시지 바디를 통해 들어온 데이터를 처리하는 모든 기능을 수행한다.
- 주로 전달된 데이터로 **신규 리소스 등록**, 프로세스 처리에 사용
    - 회원가입, 주문, 게시판 글쓰기, 댓글 달기, 신규 주문 생성
    - 한 문서 끝에 내용 추가하기
  
### PUT
- 리소스를 대체
  - 리소스가 있으면 대체
  - 리소스가 없으면 생성
- 중요! 클라이언트가 리소스를 식별
  - POST와 차이점
    - POST는 없는 걸 새로만드는 느낌, PUT은 덮어씌우기(기존 리소스를 지우고, 생성한다.) 
    - POST는 리소스의 위치를 모르고 요청하지만, PUT은 리소스 위치를 알고 URI 지정
- 수정 용도로 사용하지않는다.

### PATCH 
 - 리소스 부분 변경
 - 회원 정보 수정
### DELETE
 - 리소스 제거


## HTTP 메서드의 속성<a name ="MethodProperty"></a>
- 안전(Safe Methods)
- 멱등(Idempotent Methods)
- 캐시가능(Cacheable Methods)

<p align="center"><img src="https://user-images.githubusercontent.com/104331549/177080619-d5f93b8d-87bd-44cd-bda9-88a45be9de80.png" width="60%"></p>

- [출처 위키백과](https://ko.wikipedia.org/wiki/HTTP)

### 안전 
 - 호출해도 리소스를 변경하지 않는다. 
 - Q: 그래도 계속 호출해서, 로그 같은게 쌓여서 장애가 발생하면요?
 - A: 안전은 해당 리소스만 고려한다. 그런 부분까지 고려하지 않는다

### 멱등 
 - 한번 요청하든 두번 요청하든 **100번 같은 요청**을 보내든 결과가 똑같다. 
 - 멱등 메서드 : GET, PUT
 - 멱등이 아닌 메서드 : POST(중복에러), DELETE;
#### 활용 
 - 자동 복구 메커니즘
 - 서버가 정상응답을 못 주었을 때, 다시 같은 요청을 해도 되는가?를 판단할 수 있다.


> 안전이든 멱등이든,수천번 요청으로 로그가 쌓이게 된다던가, 외부요인으로 중간에 리소스가 변경되는 예외상황 까지는 고려하지 않는다.

### 캐시가능 
 - 캐시란? 값을 미리 복사해 놓은 임시 장소를 가리킨다.
   - [쿠키와 캐시의 차이점](https://www.quora.com/What-is-the-difference-between-cookies-and-cache)  
 - 응답 결과 리소스를 캐시해서 사용해도 되는가? 
 - GET, HEAD, POST, PATCH 캐시가능
   - 실제로는 `GET`, `HEAD` 정도만 캐시로 사용
   - POST, PATCH는 `본문 내용`까지 캐시 키로 고려해야 하는데, 구현이 쉽지 않음

