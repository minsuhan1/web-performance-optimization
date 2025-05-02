## 웹 개요
- 웹은 **하이퍼텍스트를 바탕으로 관련 있는 문서들끼리 연결한 집합체**로서, 인터넷에서 제공하는 수많은 서비스들 중 하나
- 웹의 세 가지 대표 요소
	1. `URL`: 웹 자원이 인터넷상의 어느 곳에 위치하는지 나타내는 경로
		<img width="656" alt="image" src="https://github.com/user-attachments/assets/4d2a63ad-b52e-4863-9501-ec4abae5fa9c" />
	2. `프로토콜`: 웹의 자원에 접근하는 네트워크 방식
		- 웹은 대부분 HTTP
			- Header: 클라이언트(브라우저)와 서버 사이에 필요한 정보
			- Body(Paylod): HTML이나 이미지 등의 실제 데이터
	3. `HTML`: 웹페이지에 실제로 보여줄 컨텐츠를 정의하고 태그(<>) 기반으로 웹의 목적에 맞는 다양한 기능 수행
## 웹 성능이 중요한 이유
- **웹 성능**(web performance)이란 **웹 로딩 시간**(web loading time)을 의미
	- 브라우저 주소창에 도메인 주소를 입력하여 해당 사이트에 접속하고 웹페이지가 로딩되어 컨텐츠가 표시되기까지의 시간
- 웹 로딩 속도의 지연은 서비스 사용자의 이탈률 상승과 재접속률 저하로 이어짐
	- 구글 조사에 따르면 페이지가 3초 안에 로딩되지 않으면 53%의 사용자가 떠남
- 기술 발전에 따라 인터넷 전송 속도가 빨라졌지만 그만큼 페이지가 로딩되기까지 기대하는 시간 역시 3초 이내로 줄어듦
	- 컨텐츠의 종류가 다양할수록(이미지, 광고 등) 로딩 시간이 느려지고 이것을 극복해야 함
	- 느린 인터넷 환경에 대비 필요
- 이러한 이유들로 웹 성능을 향상시키는 **웹 성능 최적화**(Web Performance Optimization)가 필요
	- 웹 성능에 영향을 주는 요소와 측정할 수 있는 도구를 선택해야 함
## 웹 성능에 영향을 주는 요소
- 사용자 환경: 거주 지역의 네트워크 장비, 디바이스, 브라우저 환경
- 공급자 환경: DNS 응답 속도, 웹 서버 응답 속도, BE 처리 속도, FE 최적화 여부 등
- 전달 환경: 웹 서버가 위치한 데이터 센터, 서버망(유선/모바일) 등
## 웹 성능 측정 방법
- **크롬 개발자 도구**
	- 전체 HTTP 요청 및 응답 수, 컨텐츠 파일 크기, DOMContentLoaded 시간, Load 시간, 로딩 완료(Finish) 시간, 각 컨텐츠별 로딩 시간(waterfall) 등
   
		<img width="644" alt="image" src="https://github.com/user-attachments/assets/8565194c-c261-43c2-b643-ba3142ba6045" />
- **WebPageTest(WPT)**
	- 세계 여러 위치의 여러 사용자 환경에서 웹 사이트 로딩 속도를 테스트할 수 있는 환경 제공
		- 사용 가능 옵션
			- URL, 테스트 위치, 브라우저 종류
			- 네트워크 종류, 테스트 횟수, 실제 로딩 화면 캡쳐 옵션 등
			- 자바스크립트 실행여부, 인증 정보, 요청 헤더 추가 등
			<img width="599" alt="image" src="https://github.com/user-attachments/assets/9284b882-f40d-41bf-960c-641567b4b589" />
		- 결과 제공 항목
			- First Byte Time, First Contentful Paint, Largest Contentful Paint 등
		  <img width="591" alt="image" src="https://github.com/user-attachments/assets/72412736-0eab-4f35-a746-3c0e02c2bd8e" />
- **Google PageSpeed Insights**
	- 구글이 만든 웹 성능 지표(First Contentful Paint, DOM Content Loaded 등)를 바탕으로 웹 성능 측정 및 평가해줌
		- **FCP**(First Contentful Paint): 웹페이지가 시각적 응답을 보이기까지 걸린 시간
		- **DCP**(Dom Content Loaded): 브라우저가 HTML 문서를 로딩하고 해석하는 데 걸린 시간
		<img width="639" alt="image" src="https://github.com/user-attachments/assets/a1b31f7b-dd25-4c54-ac29-b0bd38fc6370" />
	- PC, 모바일 환경 테스트 지원
	- [Mod_pagespeed](https://www.modpagespeed.com/)라는 오픈소스 모듈을 웹서버에 추가하면 최적화된 페이지를 클라이언트에 제공
- 크롬 개발자 도구의 **Lighthouse**
	- 웹사이트의 성능 항목 5개를 100점 만점 기준으로 나타냄
	- FCP, TTI, LCP 등
	<img width="667" alt="image" src="https://github.com/user-attachments/assets/63f7e2b8-8f06-4774-bfc6-445137bee12b" />
## 웹 성능 지표
- 웹사이트들은 프론트엔드에서 페이지 로딩 시간 중 대부분을 차지함
	- 웹 성능 지표가 웹서버가 아닌 사용자(브라우저) 관점에서 원하는 컨텐츠를 전달받았는지를 기준으로 하기 때문
	- 또한, 웹서버가 컨텐츠를 생산하는 시간보다 클라이언트에서 컨텐츠를 가져다 렌더링하는 데 시간이 더 소요됨
- **브라우저 렌더링 시의 성능 지표**
	- **FCP**(First Contentful Paint): 첫 번째 텍스트 또는 이미지 표시에 걸린 시간(웹페이지가 시각적 응답을 보이기까지 걸린 시간)
	- **LCP**(Largest Contentful Paint): 가장 큰 텍스트 또는 이미지가 표시된 시간
	- **TTI**(Time to Interactive): 사용자가 페이지가 상호작용할 수 있게 된 시간
	- **TBT**(Total Blocking Time): FCP와 TTI 사이의 시간의 합
	- **CLS**(Cumulative Layout Shift): 표시 영역 안에 보이는 요소들이 얼마나 이동하는지에 대한 정보
## 웹 성능 예산
- 웹 성능에 영향을 미치는 요소들의 한계값
- 성능 예산을 개발팀의 성능 관련 목표로 만듦
	- 페이지의 파일 크기
	- 페이지 로딩 시간
	- 페이지의 이미지 파일 수
- **성능 예산의 지표들**
	- **정량 기반 지표**
		- 이미지, 스크립트, 폰트 등 웹페이지 제작에 필요한 요소들에 대한 한계값
			- 이미지 파일 최대 크기
			- 최대 폰트 파일 개수
			- JS 파일 크기 
			- 외부 스크립트 개수
		- 단순히 파일 크기와 개수를 줄인다고 성능이 좋아진다는 보장은 없음
		- 해당 요소들을 페이지 렌더링 시에 어떤 순서로 호출하는지, 페이지 레이아웃이 어떻게 설계되었는지에 따라 변수가 있음
	- **시간 기반 지표**
		- 사용자가 느끼는 웹 성능에 대한 목표치
			- DOMContentLoaded, Load 시간
			- FCP, TTI
	- **규칙 기반 지표**
		- 공신력 있는 웹사이트 성능 측정 도구들(PageSpeed, Lighthouse, WebPageTest 등)이 제공하는 표준 점수 활용
		- 또는 사내 자체 웹 성능 테스트 시스템 활용
	
