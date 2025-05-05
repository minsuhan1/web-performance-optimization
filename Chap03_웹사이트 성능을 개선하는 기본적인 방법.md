# Chapter 3. 웹사이트 성능을 개선하는 기본적인 방법

## HTTP 요청 수 줄이기

1. **스크립트 파일 병합**
    - 기능 단위로 모듈화된 여러 파일들을 하나로 합치기
        - 합쳐진 파일 크기가 너무 크면 화면에 로딩이 오래 걸릴 수 있으므로 적절한 크기 유지
    - 파일을 병합하면 브라우저에 캐시될 확률도 좀 더 높아짐
2. **인라인 이미지**
    - `<img src=` 또는 CSS `background-image: url()` 등에 이미지 파일의 해시 정보를 삽입
        
        ```html
        <img src="data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAAUAAAAFCAYAAACNbyblAAAAHElEQVQI12P4
          //8/w38GIAXDIBKE0DHxgljNBAAO9TXL0Y4OHwAAAABJRU5ErkJggg==" alt="Red dot" />
        ```
        
    - 이미지 파일을 따로 호출하여 해당 크기만큼 파일을 받아오는 것보다 로딩 시간이 단축됨
    - 다만 HTML 파일이 캐시되지 않는 이상 이미지 파일이 별도로 브라우저에 캐시되지 않음
3. **CSS 스프라이트([CSS sprite](https://velog.io/@untiring_dev/HTMLCSS-Day31.-%EC%9D%B4%EB%AF%B8%EC%A7%80-%EC%8A%A4%ED%94%84%EB%9D%BC%EC%9D%B4%ED%8A%B8-%EA%B8%B0%EB%B2%95))**
    
    ![image.png](https://minsuhan.notion.site/image/attachment%3A985f9732-8b13-4f43-8dc9-4642bc9ae2ab%3Aimage.png?table=block&id=1ea0c39f-5ceb-8067-93b1-e40184cfc851&spaceId=c82232a7-d43c-402c-9d26-0630584e59c1&width=1170&userId=&cache=v2)
    
    - 여러 개의 이미지를 하나의 이미지 파일로 결합해 필요한 이미지가 위치하는 픽셀 좌표 정보를 사용하는 방식
        - 아이콘이나 버튼 등 작은 이미지에 유용
        - 각각의 이미지를 일일이 호출하는 것에 비해 HTTP 요청 한 번이면 되기 때문에 이미지 로드 속도가 향상됨
    - 다만, [MDN](https://developer.mozilla.org/ko/docs/Web/CSS/CSS_images/Implementing_image_sprites_in_CSS)에서는 HTTP/2를 사용하는 경우 오히려 작은 요청을 여러 번 날리는 게 네트워크 사용량에 더 좋을 수도 있다고 함
    - 예제 코드
        
        ```css
        .sprite_face {
        background: url(‘imgs/sprites_image.png’);
        background-position: 0 -64px;
        width: 50px;
        height: 50px;
        float:left;
        }
        .sprite_rss {
        background: url(‘imgs/sprites_image.png’);
        background-position: -57px -64px;
        width: 50px;
        height: 50px;
        margin-left:10px;
        float:left;
        }
        .sprite_linked {
        background: url(‘imgs/sprites_image.png’);
        background-position: -114px -64px;
        width: 50px;
        height: 50px;
        margin-left:10px;
        float:left;
        }
        .sprite_pint {
        background: url(‘imgs/sprites_image.png’);
        background-position:-171px -64px;
        width: 50px;
        height: 50px;
        float:left;
        margin-left:10px;
        }
        ```
        

## 컨텐츠 파일 크기 줄이기

1. **스크립트 파일 압축**
    - 웹서버와 클라이언트가 서로 지원하는 스크립트 압축 방식(HTTP 헤더 표기)을 사용해서 파일을 압축하여 내려주면, 클라이언트가 압축 해제하여 컨텐츠를 사용
    - `Accept-Encoding`: 클라이언트가 웹 서버에 요청 시 자신이 지원하는 압축 알고리즘을 나열
    - `Content-Encoding`: 웹 서버가 Accept-Encoding에 나열된 압축 알고리즘 중 자신이 지원하는 압축 알고리즘 하나를 선택해 클라이언트에게 알려줌
    
    ![image.png](https://minsuhan.notion.site/image/attachment%3A1fc24991-98e2-433a-b7fc-246783bcc213%3Aimage.png?table=block&id=1ea0c39f-5ceb-80b8-8973-f28b9e7f845d&spaceId=c82232a7-d43c-402c-9d26-0630584e59c1&width=1360&userId=&cache=v2)
    
2. **스크립트 파일 최소화**
    - HTML, JS, CSS 등의 파일에 포함된 주석, 공백, 개행문자 등 실제 로직에 영향을 주지 않는 부분을 제거하여 파일 크기를 줄임
        - [Minify](https://www.minifier.org/)
    - 최소화된 파일로는 추후 로직 변경이나 디버깅에 어려움이 있을 수 있으므로 개발 서버와 운영 서버를 분리
        - 개발 서버에서 스크립트 원본 파일로 개발하고
        - 로직 테스트와 검증 완료 후 스크립트를 최소화 및 압축하여 운영 서버로 이관
3. **이미지 파일 압축**
    - 이미지 파일의 메타 데이터(meta data)는 실제 눈에 보이지 않으므로 불필요한 부분을 제거 가능
    - 별다른 특징이 없는 배경 비율이 높은 이미지의 경우 압축률이 높음
4. **브라우저 선호 이미지 포맷 사용**
    - `WebP`: 구글이 웹사이트 트래픽 감소와 로딩 속도 단축을 목적으로 개발한 이미지 압축 형식
        - 손실 압축이지만 화질 저하를 최소화, JPEG 대비 평균 50% 크기 감소
    - `JPEG XR`: 마이크로소프트가 개발한 이미지 압축 형식
        - 손실, 비손실 압축 모두 지원, JPEG 대비 평균 30% 크기 감소
    - 웹서버에서 동일한 이미지를 각 형식으로 두 장씩 준비해서 크롬이면 WebP, IE면 JPEG XR 포맷으로 내려줄 수 있음
5. **큰 파일을 작게 나누어 전송**
    - 부분 요청 응답 방식
        - 큰 파일에서 필요한 부분만 요청하고 전달받아 다운로드 속도를 빠르게 하고 사용하지 않는 구간의 컨텐츠를 낭비하지 않게 함
        - 웹 서버에서 특정 부분 파일 전달을 지원해야 사용 가능
            - 웹 서버 응답 헤더를 통해 부분 파일 전달 가능 여부 확인
                
                ```bash
                // 웹 서버의 부분 응답 지원 여부 확인
                curl -I http://www.example.com/bigfile.jpg
                
                HTTP/1.1 200 OK
                (중략)
                Accept-Ranges: bytes  // 바이트 단위로 파일의 부분 지원 기능을 수락
                Content-Length: 50000000  // 해당 파일의 전체 크기가 50MB임을 클라이언트에게 알림
                ```
                
            - 클라이언트에서 파일의 특정 부분만 요청
                
                ```bash
                // 파일의 특정 부분 요청 (파일의 처음부터 1023바이트까지)
                curl -v http://www.example.com/bigfile.jpg -H "Range: bytes=0-1023"
                
                HTTP/1.1 206 Partial Content
                Content-Range: bytes 0-1023/50000000  // 파일의 전체 범위 중 처음부터 1023바이트까지만 전달
                Content-Length: 1024  // 현재 전달한 부분 파일의 전체 용량
                (생략)
                (해당 파일의 부분 컨텐츠)
                ```
                
                동영상 플레이어의 progress bar는 Content-Range, Content-Length 값을 이용해 구현함
                

## 캐시 최적화

- **캐시**: 자주 사용되는 컨텐츠나 데이터를 임의의 저장소에 복제해두고 재사용하는 방식
- **인터넷 캐시**
    - 프록시 서버(proxy server)
        - 서버와 클라이언트 사이에서 컨텐츠 중계 기능 수행
            - 클라이언트가 처음 요청한 컨텐츠를 origin 서버에 대신 요청하여 클라이언트에 전달하고 이를 스스로 저장
            - 이후 동일한 컨텐츠를 요청했을 때 origin 서버에 접속할 필요 없이 자체 저장한 컨텐츠 제공
        - 장점
            - 클라이언트와 웹 서버 간 물리적 거리에 의한 응답 속도 지연 개선
            - 원본 서버로 몰릴 수 있는 인터넷 트래픽을 프록시 서버로 분산하여 원본 서버 자원 절약
- **브라우저 캐시**
    - 웹 컨텐츠 중 브라우저의 메모리나 디스크에 일부를 저장하여 이후 동일한 컨텐츠에 대해 인터넷 상 요청을 하지 않음
    - 특정 컨텐츠의 브라우저 캐시 사용 여부는 웹 서버가 `Cache-Control` 응답 헤더로 결정
        
        ```
        // 캐시 생존 시간(TTL, Time To Live) 설정
        Cache-Control: max-age=3600  // 캐시 시간은 초(sec) 단위
        
        // 브라우저가 캐시해선 안 되는 컨텐츠
        Cache-Control: no-store
        
        // 캐시 컨텐츠를 보여주기 이전에, 원본 서버의 컨텐츠 변경 여부를 조사하는 요청을 보내도록 강제
        Cache-Control: no-cache
        
        // 캐시는 사용하기 이전에 기존 리소스의 상태를 반드시 확인해야 하며 만료된 리소스는 사용되어서는 안됨
        Cache-Control: must-revalidate
        
        // 캐시가 절대 불가능함
        Cache-Control: no-cache, no-store, must-revalidate
        
        // 정적 컨텐츠와 같이 명확히 캐시 가능한 컨텐츠임을 알림
        Cache-Control: public, max-age=30000000
        
        // 캐시 만료 시간을 특정 날짜와 시간으로 설정
        Expires: Mon, 30 Nov 2020 07:00:00 GMT
        ```
        

## CDN 사용하기

- **CDN(Content Delivery Network)**
    
    ![image.png](https://minsuhan.notion.site/image/attachment%3Aee496d3a-bf84-4843-bfda-5071de2c2142%3Aimage.png?table=block&id=1ea0c39f-5ceb-80db-bbc4-ec7188ee6863&spaceId=c82232a7-d43c-402c-9d26-0630584e59c1&width=1360&userId=&cache=v2)
    
    - 전 세계에 분산된 캐시 서버 그룹
    - 사용자와 가까운 서버에서 콘텐츠를 제공하여 웹사이트 로드 시간을 단축하고, 원본 서버의 부담을 줄여 안정성을 높이는 네트워크 방식
    - 장점
        - 원거리에 있는 컨텐츠를 전달받는 과정에서의 네트워크 지연과 패킷 손실 감소
        - 가까운 캐시 서버(에지 서버)에 캐시된 컨텐츠를 전달받으므로 RTT(Round Trip Time) 감소
        - 원본 서버 부하 감소
        - 컨텐츠가 ICP(Internet Cache Protocol)을 이용하여 에지 서버와 주변 에지 서버 사이에 서버 전파를 할 수 있어 캐시 컨텐츠의 재사용률이 높음
