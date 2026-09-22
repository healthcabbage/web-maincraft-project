# 🎮 Game Server Project

Spring Boot와 JPA, MySQL, Redis, WebSocket을 활용하여 게임 서버의 기본적인 데이터 관리와 실시간 통신 기능을 구현한 프로젝트입니다.

---

## 📚 구현 내용

## Lv 1. Docker로 MySQL과 Redis 설정

Docker를 이용해 게임 서버에서 사용할 **MySQL과 Redis 실행 환경을 구성**했습니다.

* Docker 컨테이너를 이용한 MySQL 실행
* Docker 컨테이너를 이용한 Redis 실행
* Spring Boot에서 MySQL 데이터베이스 연결 설정
* Redis 연결 설정
* 컨테이너 포트 매핑을 통한 로컬 개발 환경 구성
* JPA를 이용한 MySQL 데이터 접근 환경 구성

## Lv 2. SQL을 JPA 인덱스로 표현하기

JPA Entity에 `@Index`를 적용하여 데이터베이스에서 자주 사용되는 조회 조건에 대한 인덱스를 설정했습니다.

* JPA `@Table`과 `@Index`를 이용한 인덱스 정의
* 조회 성능을 고려한 인덱스 설계
* SQL에서 정의하는 인덱스를 JPA Entity에 표현

## Lv 3. 요청 검증과 DTO: 플레이어 등록

플레이어 등록 API를 구현하고 Bean Validation을 이용해 요청 데이터를 검증했습니다.

* 플레이어 등록 요청 DTO 구현
* `@NotBlank`, `@Size` 등을 이용한 입력값 검증
* 중첩 객체에 대한 `@Valid` 적용
* Entity와 DTO를 분리하여 요청/응답 처리

## Lv 4. 월드 생성

게임에서 사용할 월드를 생성하고 관리하는 기능을 구현했습니다.

* 월드 생성 API 구현
* 월드 정보 저장
* 생성된 월드의 ID 및 기본 정보 반환
* JPA Repository를 이용한 데이터 관리

## Lv 5. 채팅 저장과 내역 조회

월드에서 발생한 채팅 메시지를 저장하고 조회하는 기능을 구현했습니다.

* 채팅 메시지 Entity 및 Repository 구현
* 월드와 채팅 메시지의 관계 설정
* 채팅 메시지 저장
* 월드별 채팅 내역 조회
* Service 계층을 통한 비즈니스 로직 분리

## Lv 6. 최근 채팅 조회 API 구현

월드의 최근 채팅 메시지를 조회할 수 있는 API를 구현했습니다.

* 최근 채팅 메시지 조회 API 구현
* 최신 메시지 기준 정렬
* JPA Repository의 쿼리 메서드 활용
* 조회 결과를 DTO로 변환하여 응답

## Lv 7. WebSocket 연결과 사용자 식별

게임 클라이언트와 실시간으로 통신하기 위해 WebSocket 연결 구조를 구현했습니다.

* WebSocket Handler 구성
* 월드 ID와 닉네임을 이용한 사용자 식별
* WebSocket 세션 관리에 필요한 사용자 정보 처리
* WebSocket 메시지 처리 구조 구성

## Lv 8. HandshakeInterceptor 등록

WebSocket 연결 과정에서 닉네임과 월드 정보를 검증하고 세션 속성에 저장하도록 `HandshakeInterceptor`를 등록했습니다.

* `NicknameHandshakeInterceptor` 구현
* 닉네임을 이용한 Player 조회
* World ID를 이용한 World 조회
* WebSocket Session Attributes에 사용자 및 월드 정보 저장
* WebSocket 설정에 Interceptor 등록

## Lv 9. 월드별 WebSocket 세션 관리

월드별로 접속 중인 사용자의 WebSocket 세션을 관리하는 `WorldSessionRegistry`를 구현했습니다.

* 월드별 WebSocket 세션 관리
* 닉네임을 Key로 사용하는 세션 관리
* 동일 닉네임의 중복 접속 처리
* 세션 등록 및 제거
* 연결된 세션 조회

## Lv 10. Redis 접속 상태 관리

Redis Sorted Set을 이용하여 사용자의 접속 상태와 만료 시간을 관리했습니다.

* 월드별 Presence Key 관리
* Redis Sorted Set을 이용한 접속 상태 저장
* Connection ID를 Member로 저장
* 만료 시간을 Score로 저장
* 접속 종료 시 Presence 제거
* TTL을 이용한 Presence 데이터 자동 만료

## Lv 11. 메시지 라우팅과 Ping/Pong

WebSocket 메시지의 종류에 따라 적절한 Handler로 전달하는 메시지 라우팅 구조와 Ping/Pong 기능을 구현했습니다.

* `MessageRouter`를 이용한 메시지 타입별 Handler 호출
* Client Ping 요청 처리
* Server Pong 응답
* Ping 수신 시 Redis Presence Heartbeat 갱신
* 연결 상태 유지를 위한 Heartbeat 처리

## Lv 12. 플레이어 이동 요청 처리

WebSocket을 통해 전달된 플레이어의 이동 정보를 검증하고 게임 엔진에 전달하도록 구현했습니다.

* 플레이어 위치 `x`, `y`, `z` 값 처리
* `yaw`, `pitch` 회전 값 처리
* `crouching`, `gliding` 상태 처리
* 숫자 및 실수값 유효성 검증
* `PlayerAction.Move` 생성
* 게임 엔진의 이동 요청 Queue에 등록

## Lv 13. 채팅 요청 처리와 응답 구성

WebSocket으로 전달된 채팅 메시지를 저장하고 클라이언트에게 반환할 응답을 구성했습니다.

* 채팅 내용 검증 및 추출
* 채팅 메시지를 DB에 저장
* 저장된 채팅 정보로 응답 생성
* `ChatResponse` DTO 구현
* 채팅 타입, 발신자, 내용, 시간 정보 반환

## Lv 14. 같은 월드의 참여자에게 채팅 전송

채팅 메시지를 현재 월드에 접속한 모든 사용자에게 전달하는 기능을 구현했습니다.

* `LocalChatSender` 구현
* `WorldBroadcaster`를 이용한 월드 단위 메시지 전송
* 동일 월드의 모든 WebSocket 세션에 채팅 메시지 Broadcast

## Lv 15. 접속자 목록 조회

현재 같은 월드에 접속 중인 사용자 목록을 WebSocket을 통해 조회할 수 있도록 구현했습니다.

* `WorldSessionRegistry`에서 현재 월드의 세션 조회
* 연결이 열려 있는 세션만 필터링
* Session Attributes에서 닉네임 조회
* 접속자 목록 정렬
* 접속자 수(`count`) 계산
* 요청한 사용자에게만 `OnlineUsersResponse` 전송

---

## 🧪 테스트

각 단계에서 제공된 테스트 코드를 이용하여 구현 내용을 검증했습니다.

* JPA / REST API 테스트
* WebSocket Handler 테스트
* WebSocket Session 관리 테스트
* Redis Presence 테스트
* 채팅 저장 및 전송 테스트
* 온라인 사용자 조회 테스트

또한 Postman을 이용하여 실제 WebSocket 연결 및 메시지 송수신을 확인했습니다.

---

### 트러블슈팅

원인
Redis CLI 사용 오류 - 처음 Redis 컨테이너 내부에서 ZRANGE를 직접 입력했을때 shell 명령으로 인식되는 문제.
해결과정
Redis CLI에 접속한 후 Redis 명령어를 실행하고나서 해당 명령을 실행해야지 가능한 것을 인식

원인
Spring Boot를 실행할때, DB 오류가 발생하는 부분
해결과정
DB가 생성되지 않고 실행해서 발생한 문제, DB를 다시 생성하니 문제 해결

---

###회고

인터넷검색과 강의를 참고하면서 제작했지만 여전히 미숙하고 모르는 부분이 생겨난다.
WebSocket과 JPA를 더 복습하며 개념 복습을 더 하는게 좋을거같다.
