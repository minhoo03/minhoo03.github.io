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
이때, URL의 스키마(예: http:// 또는 https://)를 확인하여 요청에 사용할 프로토콜을 결정한다.

- - -

#### HSTS 정책 조회
URL의 스키마가 HTTP인 경우, 브라우저는 해당 도메인에 대해 HSTS(HTTP Strict Transport Security) 정책이 적용되어 있는지 확인한다.
HSTS 정책이 적용된 도메인이라면, 브라우저는 HTTP 요청을 HTTPS로 강제 변환한다.
이 과정은 브라우저 내부에 저장된 HSTS 캐시 또는 브라우저가 내장한 HSTS Preload 목록을 통해 이루어진다.

- - -

#### 이후 흐름
HSTS 정책이 확인되면, 브라우저는 프로토콜에 따라 DNS 조회와 같은 네트워크 요청 과정을 진행한다.
HTTPS가 적용된 경우, TLS 핸드셰이크 과정을 통해 보안 연결을 설정한 뒤, 데이터를 서버와 송수신한다.

<!-- // 만약 HTTP로 요청이 왔다면 HTTP 응답 헤더에 "Strict Transport Security"라는 필드를 포함하여 응답. -->


<!-- 암호화 관련 -->

### URL을 IP로 변환 (DNS):

브라우저는 DNS 서버에 요청을 보내 URL의 도메인 이름을 숫자로 된 IP 주소로 변환한다.
IP 주소는 컴퓨터가 통신하기 위해 사용하는 실제 주소다.
주로 장치의 "위치"를 나타내며, 네트워크 간 데이터 전달에 사용된다.

##### ex. "192.168.0.1" 같은 IP 주소는 논리적이고 계층적인 주소다.

<!-- 반환되는 값 -->

### 라우터를 통해 서버 게이트웨이로 이동:

IP 주소를 기반으로 인터넷을 통해 요청이 올바른 서버로 전달된다.
네트워크 라우터가 데이터를 중계하며, 요청은 서버의 게이트웨이에 도착한다.

### IP 주소를 MAC 주소로 변환:

네트워크 내에서 통신하기 위해 ARP(Address Resolution Protocol)를 사용하여,
IP 주소를 물리적인 MAC 주소로 변환한다.

IP 주소는 전송 경로를 설정하기 위한 논리적 주소다. 그러나 실제 데이터 전송은 물리적 주소인 MAC 주소를 통해 이루어져야 한다.

IP 패킷이 최종 목적지에 도달하려면 LAN(Local Area Network) 또는 이더넷 네트워크 내에서 MAC 주소가 필요하다.
ARP(Address Resolution Protocol)는 네트워크 계층(IP 주소)와 데이터 링크 계층(MAC 주소)을 연결해 주는 역할을 한다.

### 대상 서버와 TCP 소켓 연결:

서버와의 데이터를 주고받기 위해 TCP 소켓 연결을 설정한다.
HTTPS라면 암호화된 연결을 설정하기 위해 TLS 핸드셰이킹이 추가로 이루어진다.

<!-- 핸드셰이킹 과정 -->

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

서버로부터 받은 HTML, CSS, JavaScript 파일을 브라우저의 렌더링 엔진이 처리한다.

이 과정에서:
*    HTML: DOM(Document Object Model)으로 변환.
*    CSS: CSSOM(CSS Object Model)으로 변환.
*    JavaScript: 브라우저의 JavaScript 엔진에서 실행.

DOM과 CSSOM을 결합하여 렌더 트리를 생성하고, 이를 화면에 그려준다(레이아웃, 페인팅).

<!-- 렌더링 과정 자세히 -->


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
JavaScript 코드를 실행하고, 웹 애플리케이션의 동적인 기능을 처리한다.

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
