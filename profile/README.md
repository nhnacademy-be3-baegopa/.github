# BAEGOPA
![logo_down](https://github.com/nhnacademy-be3-baegopa/.github/assets/70000247/1dc2ddd5-d618-4c43-a786-d12474264a1d)

## 팀 구성

### [@김현준](https://github.com/yoho94)
### [@이소라](https://github.com/sora0303)
### [@김종명](https://github.com/lightbell03)
### [@한제우](https://github.com/hanjaewoo98)

-----

# 홈페이지 시연
https://github.com/nhnacademy-be3-baegopa/.github/assets/127469035/60fe8453-d802-48d5-ba1f-9cc3122278e1

-----

# WBS
https://docs.google.com/spreadsheets/d/1KfMOusPvU6z0kEWEr3EvL9mK_K1yDBH-OVwCcWxwHzs/edit?pli=1#gid=42995231

-----

# ERD
![Image](https://github.com/nhnacademy-be3-baegopa/etc/assets/127469035/b903fe24-6f1e-4000-b094-9535bafe3c68)

---

# 인프라

### 담당자 : 김현준

### 아키텍쳐

![image](https://github.com/nhnacademy-be3-baegopa/.github/assets/50386803/0b3e15ec-1394-47be-b120-49da284e8bcb)


### 각 시스템별 담당자
<details>
    <summary>자세히</summary>

- L4 Switch
    - 담당자 : 한제우
- Front
    - 담당자 : 전원
- Gateway
    - 담당자 : 김현준
- Eureka
    - 담당자 : 한제우
- Store API
    - 담당자 : 전원
- Coupon API
    - 담당자 : 김종명
- Delivery
    - 담당자 : 김현준
- Batch
    - 담당자 : 이소라
- My SQL
    - 담당자 : 이소라
- Mroonga
    - 담당자 : 김종명
- Redis Session
    - 담당자 : 김현준
- Redis Basket
    - 담당자 : 한제우
- Redis Coupon
    - 담당자 : 김종명
- Rebbit MQ
    - 담당자 : 김종명
- Web Soket
    - 담당자 : 김종명
- Github
    - 프로젝트 관리
- Github Action
    - 담당자 : 김종명, 한제우
    - Store, Eureka, Coupon, Batch, Websoket
- Jenkins
    - 담당자 : 김현준
    - Gateway, Auth, Front, Delivery
- Prometheus
    - 담당자 : 김현준
    - Grafana
    - AlertManager
- 파일 관리
    - 담당자 : 이소라
    - Object Storage
    - Image Manager
</details>

### CI/CD

<img width="1613" alt="배고파 CI_CD (1)" src="https://github.com/nhnacademy-be3-baegopa/.github/assets/50386803/adbc285a-4322-4357-9b7a-b59aef318044">

- 젠킨스
    - ![젠킨스](https://github.com/nhnacademy-be3-baegopa/.github/assets/50386803/ab4b5fea-3a92-4ed9-b3c1-255fdb05a5d0)

- 깃 액션
    - ![깃허브](https://github.com/nhnacademy-be3-baegopa/.github/assets/50386803/b9d99fb4-8c86-4c2c-9d58-c634bfe48216)


### Service Discovery

- Spring Cloud Netflix Eureka
   - Server-Side Load-Balancing
- Spring Cloud OpenFeign
   - Client-Side Load-Balancing

### 모니터링

- Prometheus
- Grafana
    - ![image](https://github.com/nhnacademy-be3-baegopa/.github/assets/50386803/a12fd7d8-ae66-4613-b729-7c164ac4f9ff)
    - ![image](https://github.com/nhnacademy-be3-baegopa/.github/assets/50386803/d2a2b804-6e6c-4551-8b53-fcecdf8bba98)

- AlertManager
    - 서버에 이상이 생기면 Dooray Message로 알림 전송
    - ![image](https://github.com/nhnacademy-be3-baegopa/.github/assets/50386803/98b6b9f4-c013-48d6-9dc2-13fc22e26270)

- Log & Crash
    - 로그 수집 및 알림
    - 에러 로그 발생 시 Dooray Message로 알림 전송

### 코드 품질 관리

- SonarQube

---
# 인증/인가

### 담당자 : 김현준

### 사용 기술

- Spring Boot 2.7.13
- Spring Security
    - 프론트 서버
        - 로그인 사용자에게 권한을 부여하여 접근 제어 (사용자, 휴면계정, 시스템 관리자, 매장관리자, 매장 매니저, 매장 알바)
        - UsernamePasswordAuthenticationFilter를 상속받아 JWT 로그인 구현
        - OncePerRequestFilter를 상속받아 로그인 검증 구현
        - csrf 사용 및 ChangeSessionId 정책 사용으로 보안 강화
    - 인증 서버
        - 기존 formLogin에 핸들러, AuthenticationProvider를 추가하여 로그인 구현
- JWT
    - 프론트와 백엔드 서버가 분리되어 있는 환경에서 로그인한 세션의 유저 정보를 백엔드에서 가져오려고 JWT를 선택하게 되었다.
- Spring Cloud Gateway
    - API 호출이 들어오면 필터를 통해 로그인 검증 후 호출
- PAYCO Login 적용

### 설명

- Spring Redis

### 로그인 시퀀스 다이어그램

> 유레카 서버는 생략합니다.
> 

```mermaid
sequenceDiagram

actor c as 고객
participant front as 프론트 서버
participant session as Redis Session
participant gateway as 게이트웨이 서버
participant auth as 인증 서버
participant jwt as Redis JWT
participant store as 스토어 API 서버
participant db as MY SQL

c->>front: 1. ID, PW 입력
front->>gateway: 2. 로그인 요청
gateway->>auth: 3. 로그인 요청
auth->>gateway: 4. ID로 회원 정보 요청
gateway->>store: 5. ID로 회원 정보 요청
store->>db: 6. ID로 회원 정보 요청
alt 아이디 없음
	db->>store: 7. 회원 정보 없음
	store->>gateway: 8. 회원 정보 없음
	gateway->>auth: 9. 회원 정보 없음
	auth->>gateway: 10. 로그인 실패
	gateway->>front: 11. 로그인 실패
	front->>c: 12. 로그인 실패
else 아이디 있음
	db->>store: 7. 회원 정보 응답
	store->>gateway: 8. 회원 정보 응답
	gateway->>auth: 9. 회원 정보 응답
	alt PW 불일치
		auth->>gateway: 10. 로그인 실패
		gateway->>front: 11. 로그인 실패
		front->>c: 12. 로그인 실패
	else PW 일치
		auth->>jwt: 10. 엑세스 토큰으로 접근 가능한 로그인 유저 정보 생성
		auth->>jwt: 11. 리프레시 토큰 생성
		auth->>gateway: 12. JWT 엑세스토큰, 리프레시토큰 응답
		gateway->>front: 13. JWT 엑세스토큰, 리프레시토큰 응답
		front->>session: 14. JWT 엑세스토큰, 리프레시토큰 저장
		front->>c: 15. 로그인 성공, 세션 쿠키 응답
	end
end
```

### 로그인 검증 시퀀스 다이어그램

> 유레카 서버, MY SQL은 생략합니다.
로그인 후 동작 가능한 매장 목록 조회(/market)를 예시로 그렸습니다.
> 

```mermaid
sequenceDiagram

actor c as 고객
participant front as 프론트 서버
participant session as Redis Session
participant gateway as 게이트웨이 서버
participant auth as 인증 서버
participant jwt as Redis JWT
participant store as 스토어 API 서버

c->>front: 매장 목록 요청(/market)

alt Session 없음
	front->>c: 로그인 페이지 리다이렉트
end

front->>session: 엑세스 토큰, 리프레시 토큰 요청
session->>front: 엑세스 토큰, 리프레시 토큰 응답

alt 엑세스, 리프레시 토큰 만료
	front->>c: 로그인 페이지 리다이렉트
end

front->>gateway: 엑세스 or 리프레시 토큰을 헤더에 담아 매장 목록 요청
note over front, gateway: 엑세스는 있고 리프레시가 만료되면 리프레시토큰 재발급 요청한다.

alt 엑세스 토큰 있음
	gateway->>auth: 엑세스 토큰 검증 요청
	auth->>jwt: 블랙리스트 확인(로그아웃 리스트)
	jwt->>auth: 결과 응답
	auth->>gateway: 검증 결과 응답
end

alt 리프레시 토큰 있음
	gateway->>auth: 엑세스 토큰 재발급 요청
	auth->>jwt: 리프레시 토큰 확인
	jwt->>auth: 결과 응답
	auth->>gateway: 엑세스 토큰 재발급 응답
end

alt 리프레시토큰 재발급 요청
	gateway->>auth: 리프레시 토큰 재발급 요청
	auth->>jwt: 리프레시 토큰 저장
	auth->>gateway: 리프레시 토큰 재발급 응답
end

alt 검증 실패
	gateway->>front: 인증 실패
	front->>c: 로그인 페이지 리다이렉트
end

note over gateway: 재발급된 토큰들을 헤더에 담아간다.

gateway->>store: 매장 목록 요청
store->>gateway: 매장 목록 응답
gateway->>front: 매장 목록 응답

alt 재발급 토큰 있음
	note over front: 리프레시 토큰 재발급은 Session Timeout시간을 초기화한다.	
	front->>session: 재발급 토큰 저장
end

front->>c: 매장 목록 응답

```

---

# 마이페이지

### 담당자 : 김현준

- 내 정보
- 주소 관리
- 기념일 관리
- 쿠폰함
- 리뷰 관리
- 주문 내역
- 포인트 내역

---

# 매장

### 담당자 : 김현준, 이소라

- 매장 등록, 수정, 삭제, 조회
    - 매장 등록 시 관련 정보 한 번에 등록 처리 → 템플릿 메소드 패턴 이용 → 요소별 API 호출로 순서로 리팩토링 진행
    - 리뷰 많은 순, 평점순, 거리순 (하버사인 공식), 주문순 정렬 조회
- Validation
    - FrontEnd : javascript, BackEnd : spring validation을 통해 매장 등록 및 수정 시 발생하는 여러 가지 경우 검증 진행
- API 비동기 호출
    - 사업자 진위확인 API, 카카오 지도 API 등 다양한 API를 비동기적으로 호출 처리
- 동기적 생성 및 수정
    - 운영시간, 휴일 생성 및 수정 시 페이지를 리로드하지 않고 javascript template을 활용해서 동기적으로 생성 및 수정 처리
- 사용기술 : Spring Data JPA, Query DSL, Thymeleaf, javascript 등

---

# 메뉴

### 담당자: 김종명

- 메뉴 카테고리, 메뉴, 메뉴옵션, 보조메뉴 조회, 등록, 수정, 삭제
    - 메뉴 삭제 시 soft delete
    - 보조메뉴, 옵션 수정 시 존재하는 값은 수정, 존재하지 않는 값은 삭제해 수정, 삭제를 한 번에 처리
- toast ui editor 사용
    - wiswyg 텍스트를 그대로 저장, 불러오기 가능
- 메뉴 검색
    - 메뉴 이름과 메뉴 설명으로 검색 매장 검색
    - trigger 를 사용해 menu 저장, 수정, 삭제 시 검색 테이블에도 데이터가 자동으로 들어갈 수 있게 적용
    - mroonga 플러그인 사용
        - mroonga 를 이용해 메뉴와 메뉴 설명으로 매장 검색
        - full text search 를 통해 검색
- 메뉴를 장바구니에 담을 때 매장의 영업상태에 따라 담을 수 없게 적용

---

# 쿠폰

### 담당자: 김종명

- 쿠폰 등록, 조회, 수정, 삭제
- RabbitMQ 를 사용
    - 이벤트 쿠폰 발급에 대한 대규모 트래픽 방지
    - 발급시 에러가 발생했을 때 3번의 시도기회를 주고도 예외가 발생하면 dead letter queue를 사용해 에러로그로 에러 처리
- JdbcTemplate 에서 제공하는 bulkUpdate 를 사용
    - 이벤트 쿠폰과 같이 한 번에 제한되며 많은 데이터를 빠르게 저장
- redis 사용
    - 동시성 처리를 위해 redis 에서 제공하는 rpush, lpop 사용
- 발급쿠폰의 식별키로 uuid 사용
    - 데이터베이스 성능을 위해 정렬된 uuid 사용
    - String 인 uuid 를 binary16 으로 변경해 데이터베이스 저장공간 확보
- 쿠폰발급 시 polling 방식을 사용
    - ajax 통신을 사용해 1초(10초 까지)마다 coupon 서버에 확인
    - ![Untitled](https://github.com/nhnacademy-be3-baegopa/.github/assets/70000247/4a28ae2f-57aa-4149-a6d0-830408254ab1)
- 회원가입 쿠폰 발급
    - feignClient 를 사용해 회원가입 쿠폰발급 시 다른 api 서버와 통신해 쿠폰 데이터 저장
    - 회원가입시 쿠폰 발급에 문제가 생겨도 회원가입 트랜잭션에는 문제없도록 처리

# 장바구니 / 주문 / 결제

담당자: 한제우 

- 장바구니, 주문, 결제의 흐름도

![스크린샷 2023-08-16 오후 10 04 49](https://github.com/nhnacademy-be3-baegopa/.github/assets/125388076/2e2c8c58-35cc-4538-a35c-f9381a9189d4)

- 장바구니 동작 화면

![sample](https://github.com/nhnacademy-be3-baegopa/.github/assets/50386803/836ffe4f-d00e-43ca-ba30-efee4b818808)

- 싱글 페이지 어플리케이션
    - 사용성을 확보하고자 단일 페이지에서 장바구니,주문,결제를 모두 처리 가능하도록 구현
    - 동적으로 할인율 및 잔액을 산정하고 페이지 재 렌더링을 최소화하도록 구현
    - 포인트의 사용이나 쿠폰의 사용이 적용되더라도 페이지를 다시 로드하지 않음
 
![스크린샷 2023-08-16 오후 10 09 03](https://github.com/nhnacademy-be3-baegopa/.github/assets/125388076/a9dd9372-f0a0-4006-95b5-acd6408bbf41)
- 장바구니
    - 여러 가게에서 여러 메뉴 동시 담기 가능
    - 포인트 및 쿠폰 실시간 적용
    - 장바구니 페이지 내에서 수량 조절 및 삭제 가능
    - 장바구니 수정 등록 삭제 조회 가능하도록 구현
    
- 통합결제
    - 여러 가게에서 여러 메뉴 동시 주문 가능
    - 단일 결제 이후 부분 취소 및 환불 가능
    - 다중 메뉴 수정 및 동시주문 가능하도록 구현
      
- TOSS API 사용
    - 결제 서비스 및 주문/환불 처리에 카드 모의 시스템을 도입하기 위해 TOSS API 사용
    - 결제 데이터 및 취소 데이터 주고받음      
      
- Redis 사용
    - 빈번한 내용 변경이 이루어지는 장바구니에 알맞게 Redis 적용
    - DB의 부하를 줄이고 빠른 응답을 통한 사용성 확보
    - Phantom Key를 사용해 Redis의 TTL 만료 시 DB 자동저장을 통한 데이터 영구 저장 및 복원
    - DB 조회 최소화를 위해 로그인, 로그아웃, 결제 시에만 DB를 조회하도록 구현

      
- 주문 시 검증
    - Front 서버와 Backend 서버 간  통신 시 주문 내역의 유효성을 검증
    - 결제를 위해 주문서 발행 시 Front 단에서 최소한의 데이터만 전송
    - 이후, 주문서 발행 시 Backend에서 유효성 검증을 통과 후 정리된 데이터를 제공
    - 다양한 검증 로직을 사용 (쿠폰 유효성, 포인트 유효성, 배달거리 유효성, 영업시간, Toss Api, Null check, 판매가능 여부, 주문 존재여부 등)

---

# 주문, 배달 알림

### 담당자: 김종명, 김현준

- websocket 사용
    - 주문 상태, 배달 상태 알림을 위한 웹소켓 서버개발
    - gateway에 붙여서 사용자 인증처리를 gateway에 맡기려했지만 https 와 ws 간 연결이 되지 않는 문제 발생
        - websocket 서버를 위한 서브 도메인을 만들어 인증서를 붙여 wss 통신으로 가능하게 하고 gateway와 분리해 websocket 서버 내부적으로 사용자 인증을 할 수 있게 처리
- Stomp 프로토콜을 사용해 pub/sub 구조로 소켓 연결
    - 주문구독, 배달구독을 나누어 주문, 배달 요청을 구분해 메시지 발행
- RabbitMQ 를 사용
    - 사용자 주문을 받아 대규모 트래픽 방지
    - 3개의 큐를 등록해 대규모 트래픽에 대비해 큐에 주문요청 저장
- redis 사용
    - sub/pub 구조를 사용해 소켓 서버 확장에 대비
 
---

# 매장 관리자 대시보드, 주문 현황

### 담당자 : 김현준

- 매장 직원 권한 별 대시보드 통계 조회
    - 대표, 매니저는 통계 조회 가능, 알바는 불가능
- 주문 현황
    - 웹소켓과 연동하여 사용자의 주문, 취소 시 알림
    - 수락, 배송 시 사용자에게 알림 (Dooray Messanger)

---

## 포인트

### 담당자 : 김현준, 이소라

- 주문, 회원가입, 리뷰 작성 등에 사용되는 포인트 사용, 적립, 조회, 환불 처리
- 사용자의 편의성을 위해 다양한 오버로딩 활용
- 사용기술 : Spring Data JPA, Query DSL

## 리뷰

### 담당자: 이소라

- 리뷰 등록, 수정, 삭제, 조회
- 리뷰 이미지 다중화를 통해 한 리뷰에 여러 이미지를 등록, 수정할 수 있도록 처리
- 해당 주문 및 매장에 관련된 총리뷰를 작성한 후, 주문한 메뉴에 대해서도 메뉴 리뷰를 작성할 수 있도록 처리
    - 해당 총리뷰에서 메뉴 리뷰가 존재한다면 메뉴 리뷰를 수정할 수 있고, 메뉴 리뷰가 존재하지 않는다면 메뉴 리뷰를 작성할 수 있도록 동적으로 처리
- 각각의 메뉴와 메뉴 리뷰를 연동하여 메뉴에서 별점 및 리뷰 확인 가능
- 최신순, 낮은 별점 순, 높은 별점 순, 평균 평점 및 리뷰 수 등 통계기능 제공
- 사용기술 : Spring Data JPA, Query DSL, Thymeleaf, javascript 등

## 파일

### 담당자: 이소라

- 파일 업로드, 다운로드, 수정, 삭제 처리
- 다양한 스토리지 저장
    - 로컬 서버, 오브젝트 스토리지, 이미지 매니저 등 다양한 스토리지에 파일을 저장할 수 있도록 프록시 패턴을 활용
        → 이미지 저장 시 이미지 매니저, 이미지 이외의 확장자 저장 시 오브젝트 스토리지에 저장
- 비동기 방식을 활용한 기능 분리
    - 파일을 등록 form의 post 요청 시 함께 건네지 않고, 기능을 별도로 분리하여 javascript 비동기 방식으로 파일 생성 요청을 보낸 후 생성된 결과값(파일 식별번호)을 가지고 form post 제출함
- 파일 Multiple 기능
    - 파일은 multiple로 생성 및 수정이 가능하며, 파일 수정 시 기존 파일과의 개수를 비교하여 증감분만큼 생성하거나 삭제함
    - 파일 업로드 시 gateway를 거치면 부하가 높아질 수 있으므로 이로 인해 파일 처리의 경우 기존 gateway와 분리하고, auth 기능과 및 resttemplate을 별도로 사용함
- 권한 확인
    - 사업자 등록증 등 보안이 필요한 파일의 경우 업로더 회원 식별번호와 요청 회원 식별번호를 비교 후 다운로드 권한 확인
- 예외 처리
    - 파일 아이디가 null이거나, url은 존재하나 http 요청 후 존재하지 않는 파일임이 확인될 경우 특정 파일 호출
- 이미지 썸네일 조절 
    - 이미지 매니저 사용으로 인해 서버 측에서 이미지 썸네일 조절 가능
- 이미지 호출 속도 개선
    - Redis를 활용한 Spring Cache 적용  
- 사용기술 : Object Storage, Image Manager, Spring Data JPA, SpringCache, FeignClient 등


## Batch

### 담당자: 이소라

- 휴면회원 갱신, 회원등급 갱신, 생일 및 기념일 쿠폰 발급
- 다양한 JDBC API 중 MyBatis 선택
    - JPA(Hibernate), JDBC Templates, MyBatis 중 bulk insert가 가능하고 기본키 전략을 사용하지 않는 Mybatis 선택
    - `MyBatisCursorItemReader` 사용
- 데이터베이스 다중화
    - Batch 서버에서 두개의 데이터베이스(Store 데이터베이스, Coupon 데이터베이스)에 접근해야 하므로, 각각의 `DataSource`, `SqlSessionFactory`, `PlatformTransactionManager`을 설정함
    - `DefaultBatchConfigurer`에서 사용하는 Primary DataSource는 Store 데이터소스
    - 쿠폰 발급 시, TransactionManager만 쿠폰 TransactionManager를 사용
- 배치 예외처리
    - retry 정책 : `ConnectTimeoutException`의 경우에만 최대 3번 재실행
    - skip 정책 : 모든 exception에 대해 모두 skip하고 no rollback 적용 → 결과적으로 문제없는 케이스는 DB에 저장되고 오류로 인해 DB에 저장되지 않는 데이터는 로그 및 두레이 알림으로 기록된다.
    - 배치 코드 재실행해도 문제없도록 작성
- 사용기술 : Spring Batch, MyBatis, Dooray API 등

---

# 배송 시스템

### 담당자 : 김현준

- 요청이 들어오면 new Thread를 만들어 처리
    - 요청시 콜백 ID, 콜백 URL를 받고 동일한 콜백 ID로는 진행이 불가능 하도록 처리
    - 배송 진행 중에도 취소가 가능하도록 Thread.interrupt 사용
- 배송이 절차대로 진행될 때마다 콜백
    - 랜덤한 시간을 갖고 계속 상태가 바뀐다.
    - 수락 > 배송 중 > 배달완료 상태 값을 콜백

---

# 로그

### 담당자: 김종명

- log & crash 사용
    - 각 api 서버에 대해 error 로그를 수집하고 클라우드 서버에서 모니터링 환경구축

---

# 데이터베이스 관리(DBA)

### 담당자: 이소라
- ERD Cloud 및 MySql 데이터 관리 및 업데이트, 깃허브 업로드
- 버전별 ddl 작성 및 업데이트, code table 및 dummy data script 등 관리

---

# 테스트
### Sonar Qube
- test coverage
### Store API
<img width="1305" alt="store_api_test_coverage" src="https://github.com/nhnacademy-be3-baegopa/.github/assets/70000247/0e405af8-3cda-4d38-b1be-92b191d00a05">

### Coupon API
<img width="1324" alt="coupon_api_test_coverage" src="https://github.com/nhnacademy-be3-baegopa/.github/assets/70000247/ec6c9ba5-3521-40e8-9f07-eaebaba4a0bb">


### 부하테스트

- 서버 스펙
NHN Cloud t2.c1m1 인스턴스
    - CPU : 1 Core
    - RAM : 1GB
    - SSD : 20GB


- nGrinder
![image](https://github.com/nhnacademy-be3-baegopa/.github/assets/50386803/3f7ccab3-851a-4844-87d8-6082e2d38d87)

![image](https://github.com/nhnacademy-be3-baegopa/.github/assets/50386803/62536065-77e0-49bb-b116-4e4ec5ba4279)

![image](https://github.com/nhnacademy-be3-baegopa/.github/assets/50386803/7284118a-47a5-441d-b1d7-1e0e96e5f5ba)

Vuser(동시접속자 수) 60명 기준으로 TPS는 증가하지 않고 점점 응답시간(첫번째 바이트 평균 도달 시간)이 길어집니다.

Vuser가 300명이 넘어가면 서버가 죽습니다. (프론트1, 프론트2)

자동 복구되는 시간은 대략 30 ~ 40분 입니다.

최대 접속자 수 : 60

---

# Skill set

### Language
![Java](https://img.shields.io/badge/Java-E34F26?style=flat&logo=Java&logoColor=white)

### Framework
![Spring](https://img.shields.io/badge/spring-6DB33F?style=flat&logo=spring&logoColor=white)
![Spring Boot](https://img.shields.io/badge/spring%20boot-6DB33F?style=flat&logo=springboot&logoColor=white)
![Spring Security](https://img.shields.io/badge/spring%20security-6DB33F?style=flat&logo=springsecurity&logoColor=white)
![Spring Websocket](https://img.shields.io/badge/spring%20websocket-6DB33F?style=flat&logo=spring&logoColor=white)
![Spring RestDocs](https://img.shields.io/badge/spring%20RESTdocs-6DB33F?style=flat&logo=spring&logoColor=white)
</br>
![Spring Cloud](https://img.shields.io/badge/spring%20cloud-3693F3?style=flat&logo=googlecloud&logoColor=white)
![Spring Gateway](https://img.shields.io/badge/spring%20gateway-3693F3?style=flat&logo=googlecloud&logoColor=white)
![Spring Eureka](https://img.shields.io/badge/spring%20eureka-3693F3?style=flat&logo=googlecloud&logoColor=white)
</br>
![SpringDataJPA](https://img.shields.io/badge/Spring%20Data%20JPA-6DB33F?style=flat&logo=Spring&logoColor=white)
![MyBatis](https://img.shields.io/badge/MyBatis-ED1C24?style=flat&logo=bitdefender&logoColor=white)
![Spring Batch](https://img.shields.io/badge/spring%20batch-6DB33F?style=flat&logo=spring&logoColor=white)
![QueryDSL](http://img.shields.io/badge/QueryDSL-4479A1?style=flat-square&logo=Hibernate&logoColor=white)

### Database
![MySQL](http://img.shields.io/badge/MySQL-4479A1?style=flat&logo=MySQL&logoColor=white)
![Redis](https://img.shields.io/badge/Redis-DC382D?style=flat&logo=Redis&logoColor=white)

### Build Tool
![ApacheMaven](https://img.shields.io/badge/Maven-C71A36?style=flat&logo=ApacheMaven&logoColor=white)

### CI/CD
![Jenkins](http://img.shields.io/badge/Jenkins-D24939?style=flat&logo=Jenkins&logoColor=white)
![Github Action](https://img.shields.io/badge/Github%20Action-2088FF?style=flat&logo=githubactions&logoColor=white)

### DevOps
![NHN Cloud](https://img.shields.io/badge/-NHN%20Cloud-blue?style=flat&logo=iCloud&logoColor=white)
![Docker](https://img.shields.io/badge/Docker-2496ED?style=flat&logo=docker&logoColor=white)
![Grafana](https://img.shields.io/badge/Grafana-F46800?style=flat&logo=Grafana&logoColor=white)
![Prometheus](https://img.shields.io/badge/prometheus-E6522C?style=flat&logo=prometheus&logoColor=white)
![SonarQube](https://img.shields.io/badge/SonarQube-4E98CD?style=flat&logo=SonarQube&logoColor=white)

### ETC
![Rabbit MQ](https://img.shields.io/badge/rabbitmq-FF6600?style=flat&logo=rabbitmq&logoColor=white)
![JWT](https://img.shields.io/badge/JWT-000000?style=flat&logo=jsonwebtokens&logoColor=white)
![OAuth2](https://img.shields.io/badge/OAuth2-EB5424?style=flat&logo=auth0&logoColor=white)
![Junit5](https://img.shields.io/badge/Junit5-25A162?style=flat&logo=Junit5&logoColor=white)
![Git](https://img.shields.io/badge/Git-F05032?style=flat&logo=Git&logoColor=white)
![GitHub](https://img.shields.io/badge/GitHub-181717?style=flat&logo=GitHub&logoColor=white)

### Front
![Thymeleaf](https://img.shields.io/badge/Thymeleaf-005F0F?style=flat&logo=Thymeleaf&logoColor=white)
![HTML5](https://img.shields.io/badge/HTML5-E34F26?style=flat&logo=html5&logoColor=white)
![CSS3](https://img.shields.io/badge/CSS3-1572B6?style=flat&logo=CSS3&logoColor=white)
![JavaScript](https://img.shields.io/badge/JavaScript-F7DF1E?style=flat&logo=JavaScript&logoColor=white)
