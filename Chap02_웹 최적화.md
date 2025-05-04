# Chapter 2. 웹 최적화
## 웹 최적화

최고의 웹 성능을 구현하기 위해 최고의 조건을 만드는 노력들

- FE 최적화
    - 브라우저에서 컨텐츠를 다운로드, 로딩, 렌더링하는 속도를 향상
    - 대표 기술
        - 스크립트 최적화
        - 이미지 최적화
        - 브라우저 캐시 사용
        - DNS 조회 최소화, DNS 정보 미리 읽기
        - CSS를 HTML 상단에, 자바스크립트를 HTML 하단에 위치
        - 페이지 미리 읽기
        - 외부 스크립트 조정
- BE 최적화
    - WAS, DB, 로드밸런싱, DNS 서버 등의 시스템들을 튜닝
    - 대표 기술
        - DNS 서버 증설, DNS 정보 캐싱
        - WAS 네트워크 출력/대역폭 증설
        - 서버 성능 증설
        - 프록시 서버 설정하여 웹 컨텐츠 캐싱
        - CDN 사용
        - DB 정규화 및 캐싱
        - 로드밸런싱
        - 웹 애플리케이션 로직 최적화
- 프로토콜 최적화
    - 웹 컨텐츠를 더 빠르게 요청하고 응답하도록 HTTP/HTTPS 프로토콜을 업그레이드

## TCP/IP 프로토콜

- 웹에서는 TCP/IP 프로토콜의 일종인 HTTP를 사용해 컨텐츠를 전달
- TCP는 OSI 7계층 중 4번째인 전송 계층, HTTP는 OSI 7계층 중 7번째인 응용 계층에 속함
- TCP 네트워크의 성능 지표는 대역폭과 지연시간
    - 대역폭: 시간당 네트워크 트래픽 전송량
    - 지연 시간: 클라이언트-서버 간 요청, 전달, 응답까지 걸리는 시간
        - RTT(Round Trip Time): 서버-클라이언트를 모두 왕복하는 데 걸리는 지연 시간
- TCP 혼잡 제어
    - 패킷을 보내는 쪽에서 네트워크가 수용할 수 있는 양만큼의 패킷만 보내도록 약속
    - 호스트가 네트워크 상태를 시시각각 파악하고 전송 속도를 조절
    - 대표 기술들
        - 느린 시작(Slow Start)
            - 혼잡 윈도우(Congestion Window, CWND)의 초기값을 작게 설정하여 초기 패킷을 보내보고, 해당 패킷의 ACK 응답을 받으면 처음 보낸 패킷의 2배 등 곱셈 방식으로 전송 패킷 크기를 늘리는 방법
            - HTTP에서도 사용
        - 빠른 재전송
            - 먼저 도착해야 하는 패킷이 도착하지 않고 다음 패킷이 도착한 경우에도 일단 ACK 패킷을 보냄
            - 송신자는 중복된 ACK 패킷을 통해 패킷 손실을 감지하고 재전송 + CWND 조정
        - 흐름 제어
            - 송신자의 전송 속도를 수신자 버퍼가 따라가지 못하는 현상을 방지
            - 트래픽 수신 속도와 송신 속도를 일치시키는 기술

## HTTP 프로토콜

하이퍼텍스트 컨텐츠를 웹에서 전달하기 위해 만들어진 프로토콜

- `~HTTP/0.9`
    - 클라이언트-서버(CS)의 인터넷 통신 정상화, 가용성, 신뢰성 등에 초점
- `HTTP/1.0`
    - 이때부터 클라이언트-서버 사이 요청과 응답을 빠르게 하는 연구 진행
    - 1.0 버전까지는 CS 환경에서 사용할 수 있는 충분한 기능들을 포함
- **`HTTP/1.1`**
    - 멀티호스트 웹 환경 지원
    - **TCP/IP 연결 재사용**(persistent connection) 기능 추가
        - 지속적 요청을 사용하지 않는 HTTP 요청과 응답 구조에서, 기본적으로 `Connection: keep-alive` 헤더를 포함하지 않아도 모든 요청과 응답에서 HTTP 지속적 연결을 기본으로 지원하도록 변경
            - 클라이언트에서 헤더를 포함하지 않아도 서버의 응답에 포함됨
            - TCP 연결을 끊고 싶을 때만 `Connection: close` 헤더 사용
        - 매 요청-응답마다 TCP 연결을 위한 three-way handshaking을 하지 않아도 되므로 서버 CPU/메모리 자원 절약, 네트워크 혼잡 및 지연 감소
            
            ![image.png](https://minsuhan.notion.site/image/attachment%3Aef59e20a-7e6d-4782-958c-07f0aac36e62%3Aimage.png?table=block&id=1e90c39f-5ceb-8064-a90c-db52924d8a25&spaceId=c82232a7-d43c-402c-9d26-0630584e59c1&width=1250&userId=&cache=v2)
            
        - 하지만 메인 페이지와 같이 많은 클라이언트가 접속하는 페이지에서는 오히려 유지해야 할 TCP 연결이 늘어나서 오히려 서버 자원이 고갈될 수도 있음
    - **파이프라이닝**(pipelining) 지원
        - 먼저 보낸 요청의 응답이 지연되더라도 다음 요청을 병렬적으로 전송
            - 기존에는 먼저 보낸 요청의 응답이 지연되면 다음 요청을 보내지 못했음
            
            ![image.png](https://minsuhan.notion.site/image/attachment%3A9863a27c-23df-45cf-908d-e8b9b016a10b%3Aimage.png?table=block&id=1e90c39f-5ceb-80ac-8cb2-efa807dc09e6&spaceId=c82232a7-d43c-402c-9d26-0630584e59c1&width=1250&userId=&cache=v2)
            
        - HTTP 지속적 연결이 지원되어야 함
- `HTTP/2`
    - 스트림 형태로 다수의 HTTP 요청과 응답을 주고받는 멀티플렉싱(multiplexing) 지원
    - 더 이상 지속적 연결을 고민하지 않아도 됨
    - 이후 장에서 자세히 다룸

## DNS

- DNS(Domain Name System)는 인터넷 호스트명을 클라이언트-서버가 이해할 수 있는 IP 주소로 변환하는 시스템
- DNS 질의와 응답 성능이 웹사이트 로딩에 영향을 줄 수 있음
- **DNS 작동 원리**
    
    ![image.png](https://minsuhan.notion.site/image/attachment%3Aba9db0b8-9ccb-4b5c-85c6-96bf45af4821%3Aimage.png?table=block&id=1e90c39f-5ceb-80f0-b7ba-ea658a73d5b0&spaceId=c82232a7-d43c-402c-9d26-0630584e59c1&width=1360&userId=&cache=v2)
    
    1. 로컬 DNS 서버로 질의
        - 사용자와 인접한 DNS
            - 사용자가 이용 중인 ISP 업체나 DNS 전문 서비스 업체 등이 관리, 또는 사용자가 직접 수동 설정
            - 이전에 질의되어 캐싱되어 있는 도메인이라면 해당 IP 주소를 반환
    2. 루트 DNS 서버로 질의
        - 전체 도메인을 관장
        - ICANN 기관에서 관리
        - 질의 결과가 없는 경우 자신이 가진 .com DNS 서버의 IP 주소 반환
    3. .com DNS 서버로 질의
        - 질의 결과가 없는 경우 자신이 가진 [example.com](http://example.com) DNS 서버의 IP 주소 반환
    4. [example.com](http://example.com) DNS 서버로 질의
        - 자신이 가진 [www.example.com](http://www.example.com) 서버의 IP 주소 반환

## DNS 최적화 방법

- 웹 서비스 운영자는 [example.com](http://example.com) 서버부터 관여해 DNS 전문 업체 서비스를 받거나 분산된 DNS 서버를 직접 운영하는 방식으로 DNS 성능을 향상시킬 수 있음
- 하지만 웹사이트에는 자신뿐만 아니라 다른 웹서비스의 다양한 컨텐츠를 포함하고 있음
    - 오픈소스 모듈, jQuery, 폰트, 외부 CSS 파일, 애널리틱스 스크립트 등
    - 웹사이트에서 사용되는 도메인 리스트를 확인하는 방법
        - [개발자 도구] → [Source]
            
            ![image.png](https://minsuhan.notion.site/image/attachment%3A38814036-e879-4c29-b4df-326e8acbe494%3Aimage.png?table=block&id=1e90c39f-5ceb-802d-918f-f69d4a84e1f5&spaceId=c82232a7-d43c-402c-9d26-0630584e59c1&width=1250&userId=&cache=v2)
            
    - 특정 서비스 업체의 DNS 조회가 느리다면 해당 모듈을 다운로드해 자신의 웹서버에 직접 업로드 후 설치하여 사용하는 방법 고려
- 직접 개발한 내부 서비스에 상위 도메인(top level domain)을 동일하게 도메인을 분할하면 DNS 질의를 줄일 수 있음
    - DNS 질의 시간 단축
    - HTTPS SSL 인증서를 와일드카드 형식으로 하나만 생성해도 모든 도메인에 사용할 수 있어서 인증서 발급 비용이 줄어듦
- [DNS 프리패치](https://developer.mozilla.org/en-US/docs/Web/Performance/Guides/dns-prefetch)(prefetch) 활용
    - 하나의 웹 페이지에 다수의 도메인 호스트명이 있을 때 멀티스레드 방식으로 미리 DNS를 조회하는 기술
    
    ```html
    <link rel="dns-prefetch" href="https://fonts.googleapis.com/" />
    <link rel="dns-prefetch" href="[https://img.googleapis.com/](https://fonts.googleapis.com/)" />
    ```
    

## 브라우저 [Navigation Timing API](https://developer.mozilla.org/ko/docs/Web/API/Performance_API/Navigation_timing)

- HTTP가 빠르게 웹 컨텐츠를 전달해도 이를 사용자에게 제공하는 브라우저가 느리게 동작하면 웹 성능은 느려짐
- 브라우저의 Navigation Timing API로 웹사이트 성능 측정하기
    
    ![image.png](https://minsuhan.notion.site/image/attachment%3Af4001ecc-2a49-4ee9-94a8-9726289e9d66%3Aimage.png?table=block&id=1e90c39f-5ceb-8075-ac20-ef7a4cfaad4b&spaceId=c82232a7-d43c-402c-9d26-0630584e59c1&width=1360&userId=&cache=v2)
    
    - end-to-end latency 정보 제공
        - navigation: 사용자가 어떻게 페이지를 탐색하는지 조사
        - timing: 탐색과 페이지 로드 이벤트에 대한 데이터 조사
    - `window.performance` 객체 속성으로 사용
        - `performance.timing`
            - 페이지 로딩 흐름 순서에 따라 각 단계가 언제 완료되었는지의 정보를 제공
                
                ![image.png](https://minsuhan.notion.site/image/attachment%3A1815adb1-be99-4ee2-bc53-0079dc156683%3Aimage.png?table=block&id=1e90c39f-5ceb-800f-ab4b-e7f03de808db&spaceId=c82232a7-d43c-402c-9d26-0630584e59c1&width=1200&userId=&cache=v2)
                
        - `performance.navigation`
            - 페이지 재전송 속성, 앞뒤 이동 버튼이나 URL이 어떤 페이지 로딩을 발생시키는지 확인하는 속성 저장
                - [window.performance.navigation.type](https://developer.mozilla.org/en-US/docs/Web/API/PerformanceNavigation/type#value): 해당 웹페이지에 어떻게 접속했는지에 관한 정보
                
                ![image.png](https://minsuhan.notion.site/image/attachment%3A6a8f241a-609d-4810-92b7-0c09f3ab6bdf%3Aimage.png?table=block&id=1e90c39f-5ceb-801e-a5c3-db6808785d11&spaceId=c82232a7-d43c-402c-9d26-0630584e59c1&width=1200&userId=&cache=v2)
                
        - 내비게이션 타이밍 API로 다양한 성능 지표 확인하기
            - 각 속성이 나타내는 지표를 참고하여 어떤 부분을 최적화 해야할지 고민
            
            ```jsx
            //Navigation Timing API 개체 생성
            var perfData = window.performance.timing;
            //navigationStart, loadEventEnd 속성을 사용하여 페이지 전체 로드 시간 구하기
            var pageLoadTime = perfData.loadEventEnd - perfData.navigationStart;
            console.log("pageLoadTime: " + pageLoadTime + "ms.");
            //requestStart, responseEnd 속성을 사용하여 HTTP 요청에서 응답까지 걸린 시간 구하기
            var connectTime = perfData.responseEnd - perfData.requestStart;
            console.log("connectTime: " + connectTime + "ms.");
            ```
