---
layout: single
title: "브라우저에 대해"
published: true
classes: wide
category: browser
---

# 브라우저란 무엇인가?

브라우저는 사용자가 입력한 URL을 통해 웹 서버에 요청을 보내고, 서버로부터 받은 데이터를 해석하여 화면에 출력하는 소프트웨어다. 웹 페이지 탐색, 애플리케이션 실행, 미디어 콘텐츠 재생 등 다양한 작업을 수행할 수 있도록 돕는다.

- - -

## 브라우저의 서버 요청 플로우

브라우저에서 URL을 입력했을 때, 다음과 같은 흐름으로 동작한다.

<!-- 이미지 -->

![BrowserOperationFlow](../assets/img/browser-operation-flow.png)

### URL 파싱:

사용자가 입력한 URL을 분석하여 스키마(프로토콜), 도메인, 경로 등을 구분한다.  
이 과정에서 HTTP 또는 HTTPS 등 프로토콜의 종류를 확인한다.

```javascript
"https://example.com/path/to/resource?query=123#section";

// result
{
    "protocol": "https",
    "domain": "example.com",
    "path": "/path/to/resource",
    "query": "?query=123",
    "hash": "#section"
}
```

<!-- 구분 예시 -->




### HTTP 확인:
브라우저는 웹 페이지를 다운로드하기 위해 응용 계층의 HTTP 프로토콜을 이용해 데이터를 송신/수신한다.
이때 URL의 스키마(예: http:// 또는 https://)를 확인하여, 요청에 사용할 프로토콜을 결정한다.

- - -

#### HSTS 정책 조회
만약 URL의 스키마가 HTTP라면, 브라우저는 해당 도메인에 HSTS(HTTP Strict Transport Security) 정책이 적용되어 있는지 확인한다.
HSTS 정책이 적용된 도메인이라면, 브라우저는 HTTP 요청을 HTTPS로 강제 변환한다.
이 과정은 브라우저 내부에 저장된 HSTS 캐시 또는 내장된 HSTS Preload 목록을 통해 이루어진다.

###### HSTS(HTTP Strict Transport Security)는 웹사이트가 HTTPS 연결만 허용하도록 강제하는 보안 정책이다.
<!-- // 만약 HTTP로 요청이 왔다면 HTTP 응답 헤더에 "Strict Transport Security"라는 필드를 포함하여 응답. -->

- - -

#### 이후 흐름
HSTS 정책 확인이 끝나면, 브라우저는 결정된 프로토콜에 따라 네트워크 요청을 시작한다.
먼저 URL의 도메인 이름을 IP 주소로 변환하기 위해 DNS 조회가 이루어진다.
HTTPS 프로토콜인 경우, DNS 조회 이후 TLS 핸드셰이크를 통해 보안 연결을 설정한 뒤, 데이터를 송수신한다.

- - -

### URL을 IP로 변환 (DNS):
DNS 조회는 브라우저가 URL의 도메인 이름을 숫자로 된 IP 주소로 변환하는 과정이다.
IP 주소는 네트워크에서 통신할 때 사용하는 실제 주소로, 데이터가 목적지에 도달할 수 있도록 "위치"를 나타낸다.

##### 예: "192.168.0.1" 같은 IP 주소는 계층적이고 논리적인 네트워크 주소다.








<!-- 반환되는 값 -->

### 라우터를 통해 서버 게이트웨이로 이동:

IP 주소를 기반으로 인터넷을 통해 요청이 올바른 서버로 전달된다.
네트워크 라우터가 데이터를 중계하며, 요청은 서버의 게이트웨이에 도착한다.

### IP 주소를 MAC 주소로 변환:

네트워크에서 데이터 전송이 이루어지려면 **논리적 주소(IP 주소)**와 **물리적 주소(MAC 주소)**가 모두 필요하다. IP 주소는 앞서 설명했듯, 네트워크 상에서 장치 간 통신을 위한 주소로, 데이터의 전송 경로를 설정하는 데 사용된다. 하지만 실제로 데이터는 MAC 주소를 통해 전송된다.

**MAC 주소(Media Access Control Address)**는 데이터 링크 계층에서 네트워크 인터페이스에 할당된 고유한 식별자다. 이는 네트워크 내에서 장치들이 서로를 식별하고 통신을 할 수 있도록 돕는다.

IP 주소만으로는 데이터 전송이 불가능하기 때문에, IP 주소를 MAC 주소로 변환하는 과정이 필요하다. 이때 **ARP(Address Resolution Protocol)**를 사용한다. ARP는 네트워크 내에서 IP 주소를 입력으로 받아 해당 IP에 연결된 MAC 주소를 찾아준다.

예를 들어, IP 패킷이 목적지에 도달하려면 로컬 네트워크(LAN)나 이더넷 환경에서 MAC 주소를 기반으로 데이터를 전송해야 한다. ARP는 이를 가능하게 하여 네트워크 내에서 원활한 통신이 이루어지도록 돕는다.

요약하면, IP 주소는 장치 간 논리적 연결을, MAC 주소는 실제 데이터 전송을 담당하며, ARP는 이 둘을 연결하는 중요한 역할을 한다.

###### IP는 도로명 주소, MAC은 위도/경도, 좌표라고 생각하면 좀 더 이해가 쉬울 것이다.


### 대상 서버와 TCP 소켓 연결:

서버와의 데이터를 주고받기 위해 TCP 소켓 연결을 설정한다.
HTTPS라면 암호화된 연결을 설정하기 위해 TLS 핸드셰이킹이 추가로 이루어진다.


<!-- 

[참고] TLS 핸드셰이킹은 TCP 3 Way Handshake와 다른 개념이다.

#### TLS 핸드셰이킹

#### TCP 3 Way Handshake

TCP/IP프로토콜을 이용해서 통신을 하는 응용프로그램이 데이터를 전송하기 전, 정확한 전송을 보장하기 위해 상대방 컴퓨터와 사전에 세션을 수립하는 과정을 의미한다.

1. A 클라이언트는 B 서버에 접속을 요청하는 SYN 패킷을 보낸다. A 클라이언트는 ```SYN``` 을 보내고 ```SYN/ACK``` 응답을 기다리는 ```SYN_SENT``` 상태가 된다.
2. B 서버는 ```SYN```요청을 받고 A 클라이언트에게 요청을 수락한다는 ```ACK``` 와 ```SYN flag``` 가 설정된 패킷을 발송하고, A가 다시 ```ACK```으로 응답하기를 기다린다. B서버는 ```SYN_RECEIVED``` 상태가 된다.
3. A 클라이언트가 B 서버에게 ```ACK```을 보내면 연결이 이루어지고 데이터가 오가게 된다. 이때 B서버 상태는 ```ESTABLISHED``` 이다. 


-->


### HTTP 프로토콜로 요청 및 응답 처리:

브라우저가 HTTP 요청을 보내고, 서버는 이에 대한 응답으로 HTML, CSS, JavaScript 등의 데이터를 반환한다.

<!--
- 송신 장치가 패킷을 생성:
송신 장치는 목적지 IP 주소를 알고 있다.
- MAC 주소 확인 필요:
패킷이 물리적으로 전달되려면 해당 IP 주소에 대응하는 MAC 주소가 필요하다.
- ARP로 MAC 주소 확인:
ARP는 네트워크 상의 모든 장치에 브로드캐스트 메시지를 보내 "누가 IP X를 가지고 있나요?"라고 묻는다.
응답을 통해 해당 IP 주소에 해당하는 MAC 주소를 얻는다.
- 데이터 링크 계층에서 전송:
MAC 주소를 이용해 이더넷 프레임에 패킷을 담아 전송한다.
-->

### 브라우저에서 응답 해석 (렌더링 엔진):

렌더링 엔진에서 사용자가 요청한 HTML 문서와 CSS, JS를 파싱하여 화면에 그린다.

이 과정에서:
*    HTML: DOM(Document Object Model)으로 변환.
*    CSS: CSSOM(CSS Object Model)으로 변환.
*    JavaScript: 브라우저의 JavaScript 엔진에서 실행.

<!--

##### HTML 파싱
엔진은 태그를 파싱하여 DOM(Document Object Model) node로 변환한다. DOM node들이 계층 구조로 구성되어 DOM tree가 구축된다.

##### CSS 파싱
HTML을 파싱하는 동시 CSS를 파싱하여, CSSOM(Cascading Style Sheet Object Model)을 생성한다.

##### attachment (렌더 트리 생성)
앞서 생성된 DOM tree와 CSSOM을 결합하여, 색상 및 면적 등의 시각적 정보를 담은 Render tree를 구축한다. 이 과정에서 화면에 표시되지 않는 DOM node(ex: head tag, display:none 속성의 node)는 제외된다.

##### Reflow(Layout) & Repaint(Redraw) 그리고 Composite

* Reflow는 레이아웃(margin, padding, width, height 등) 변경이 발생할때 트리거 되며, 연관된 노드의 레이아웃을 다시 계산하여 렌더트리를 조정한다.
* Repaint는 가시성과 관련된(color, background-color, visibility 등) 변경이 발생할때 트리거 되며, 전체 노드의 가시성을 다시 확인하고 Layer를 생성한다.

-->


## 브라우저의 기본 구조

브라우저는 다음과 같은 주요 구성 요소로 이루어져 있다.

![BrowserArchitecture](../assets/img/browser-architecture.png)

**1. 사용자 인터페이스 (User Interface):**
사용자가 브라우저와 상호작용하는 화면 요소를 제공한다. 주소창, 북마크, 뒤로/앞으로 가기 버튼 등이 이에 해당한다. (요청한 페이지를 보여주는 창을 제외한 나머지 부분이다.)

**2. 브라우저 엔진 (Browser Engine):**
사용자 인터페이스와 렌더링 엔진 사이에서 동작을 제어하며 (중개), 렌더링과 관련된 명령을 관리한다.

**3. 렌더링 엔진 (Rendering Engine):**
요청한 콘텐츠를 표시한다. HTML, CSS를 해석해 화면에 표시하고, DOM(Document Object Model)과 CSSOM(CSS Object Model)을 생성한다.

**4. 통신 (Networking):**
HTTP/HTTPS 요청을 처리하고 서버와의 데이터를 송수신한다.

**5. UI Backend:**
콤보 박스와 윈도우 창 같은 기본적인 UI를 그림. 플랫폼에서 명시하지 않은 기본적인 인터페이스로서, OS 사용자 인터페이스 체계를 사용.

**6. JavaScript 엔진 (JS 해석기):**
JavaScript 코드를 실행하는 소프트웨어 컴포넌트다. 대표적인 예로 구글 크롬의 V8 엔진, 파이어폭스의 SpiderMonkey, 사파리의 JavaScriptCore (Nitro) 등이 있다. JS 엔진은 웹 브라우저나 Node.js와 같은 런타임 환경에서 사용된다.

<!--
런타임 환경은 컴퓨터가 실행되는 동안 프로세스나 프로그램을 위한 소프트웨어 서비스를 제공하는 가상 머신의 상태

즉, 프로그래밍 언어가 구동되는 환경

JS가 브라우저에서 실행되면 런타임 환경은 브라우저, Node.js에서 실행되면 런타임 환경은 Node.js
-->

**7. 데이터 저장소:**
쿠키, 로컬 스토리지, 세션 스토리지 등 브라우저에 데이터를 저장하는 계층이다.


<!--
<details>
<summary>접기/펼치기 버튼</summary>
<div markdown="1">

|제목|내용|
|--|--|
|1|1|
|2|10|

</div>
</details>
-->
