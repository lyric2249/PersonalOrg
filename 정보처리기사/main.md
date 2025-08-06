

%% # 1. 요구사항 확인 %%


|   |   |
|---|---|
 
|SYN 플러딩, dos|TCP 프로토콜 구조적 문제. 동시 가용 사용자 수를 패킷만 보내 점유하여 서버를 사용 불가. ACK 패킷은 안보내고, 새로운 연결 요청을 하여, 서버는 자원 소비하여 고갈.|
|UDP 플러딩, dos|대량의 패킷을 보내 임의의 포트 번호로 전송하여 응답 메시지 ICMP 메시지 (ICMP DESTINATION UNREACHABLE) 을 생성하게 하여 지속해서 자원을 고갈시킴. ICMP 패킷은 변조되어 공격자에게 전달되지 않아 대기.|
|스머프, dos|출발지 주소를 공격 대상의 ip로 설정하여, 네트워크 전체에게 icmp echo 패킷을 직접 브로드캐스팅하여 마비시키는 공격. 바운스 사이트라고 불리는 제3의 사이트를 이용해 공격.|
|Ping of death, dos|icmp 패킷 (ping) 을 정상적인 크기보다 아주 크게 만들어 전송하면 다수의 ip 단편화가 발생. 수신 측에서는 단편화된 패킷을 처리 (재조합) 하는 과정에서 많은 부하가 발생하거나, 재조합 버퍼의 오버플로우가 발생하여 정상적인 서비스를 하지 못하도록 하는 공격기법.|
|랜드 어택, dos|출발지 ip와 목적이 ip 를 같은 패킷 주소로 만들어 보내서, 수신자가 자기 자신에게 응답을 보내게 하여 시스템 가용성 침해|
|티어 드롭, dos, 오류제어로직|ip 패킷의 재조합 과정에서 잘못된 프래그먼트 오프셋 정보로 인해, 수신 시스템에 문제를 발생시킴. 공격자는 ip 프래그먼트 오프셋 값을 서로 중첩되도록 조작하여 전송하고, 이를 수신한 시스템이 재조합하는 과정에서 오류발생, 시스템 마비|
|봉크, dos, 오류제어로직|패킷을 분할하여 보낼 때 처음 배킷을 1번으로 보낸 후, 다음 패킷도 순서번호를 모두 1로 보냄. 똑같은 번호로 전송되어 오류를 일으킴.|
|보잉크, dos, 오류제어로직|패킷을 분할하여 보낼 때 처음 배킷을 1번으로 보낸 후, 다음 패킷은 100, 그다음은 200, 20번째는 100 등 패킷 시퀀스 번호를 비정상적인 상태로 보내서 부하.|
|http get 플러딩, app 공격|cache control attack 공격. 과도한 get 메시지를 이용하여 웹 서버의 과부하 유발. Gttp 캐시 옵션을 조작하여 캐싱 서버가 아닌 웹 서버가 직접 처리하도록 유저, 웹 서버 자원을 소진시키는 서비스 거부 공격|


# 1.1. 소프트웨어 개발 방법론

## 1.1.1. 소프트웨어 생명주기 모델
* SDLC

### 1.1.2. 모델 프로세스

1. 요구사항 분석
2. 설계
3. 구현
4. 테스트
5. 유지보수


### 1.1.3. 모델 종류
1. waterfall
2. prototyping
3. spiral
4. iteration
	1. 병행


### 1.1.1. 소프트웨어 개발 방법론
* SDM

#### 종류
1. structured development
2. information engineering development
3. object-oriented Development
4. component based Development (CBD)
5. Agile Development
	1. waterfall 과 대비
	2. 모바일 특화
	3. 적시성 / 잦은 배포
6. product line Development
	1. 임베디드 소프트웨어 특화


#### 애자일 유형

1. XP
	1. 5가치
	2. 12원리
2. SCRUM
	1. 주요 용어
3. LEAN
	1. JIT
	2. KANBAN


### 1.3. 객체 지향 분석 방법론



#### 1.3.2. 구성요소

1. class
2. object
3. method
	1. 클래스에서 생성된 object 의 활용법
	2. 객체가 메시지를 받아 실행해야할, 객체의 구체적인 연산
	3. 전통 시스템의 함수 / 프로지셔에 대응
4. message
	1. object 간 상호작용을 하기 위한 수단
5. instance
6. property


#### 1.3.3. 객체지향 기법

1. Encapsulation
	1. 경계를 만들고, 필요한 인터페이스만을 공개
	2. Information Hiding 과 관계가 깊음
2. Inheritance
3. Polymorphism
	1. overloading
		1. 메서드의 유형 / 개수를 다르게 하여, 같은 이름의 메서드를 여러개
		2. `void fn(int a)`, `void fn(int a, int b)`
	2. overriding
4. Abstraction
	1. 추상 클래스 개념과 연관
	2. 과정 추상화 / 자료 추상화 / 제어 추상화
5. Information Hiding
	1. 접근 제어자
	2. 접근용 메서드 제공
6. Relationship
	1. Association
		1. is-member-of
	2. Aggregation
		1. is part of, part-whole
	3. Classification
		1. is-instance-of
	4. Generalization
		1. is-a
	5. Specialization
		1. is-a



#### 1.3.4. 객체지향 설계원칙

1. SRP
	1. single responsibility Priciple
2. OCP
3. LSP
4. ISP
5. DIP


#### 1.3.6. 객체지향 분석 (OOA)

사용자의 요구사항을 분석하여, 요구된 문제와 곤련된 모든 클래스, OBJECT, PROPERTY, 연산, 관계를 정의하여 모델링



1. OOSE
	1. 유스케이스에 의한 접근 방법
2. OMT
	1. 그래픽 표기법
		1. 객체 모델링 / 정보 모델링
		2. 동적 모델링
		3. 기능 모델링
3. OOD
	1. 다이어그램
4. CODE-YOURDON
	1. E-R 다이어그램
5. Wirfs-Brock
	1. 분석 / 설계 간의 구분이 없음음




## ㄴㅇㅁㄹ

프로젝트 관리
소프트웨어 생명 주기


계획 관리
품질 관리
범위 관리

프로젝트 관리 3대 요소 (3P)
people


비용산정 모형
하향식
* 전문가 감정 기법
* 델파이 기법
상향식
* LoC
* Man Month
* COCOMO
* Putnam
* FP

비용산정 모형 종류

1. LoC
	1. 예측치 $\dfrac{o+4m+p}{6}$
2. Man Month (M/M)
	1. $Man Month = \dfrac{LOC}{프로그래머 월간 생산성}$
	2. $프로젝트 기간 = \dfrac{Man Month}{프로젝트 인력}$
3. COCOMO
	1. ORGANIC (기본형/단순형, ~5만 라인 KDSI)
	2. semi-detached (중간형, 5만~30만 KDSI)
	3. embedded (30만 KDSI~)
4. Putnam
	1. 생명주기 예측 모형
	2. Reyleigh-Nordon 곡선
5. Funtion Point
	1. $FP = (총 기능점수) * [0.65 + (0.1*총 영향도)]$


일정 관리 모델

* 주 공정법(CPM)
* PERT
* CCPM
* Gantt Chart

위험 관리

회피 / 전가 / 완화 / 수용







# 2. 현행 시스템 분석 - 현행 시스템 파악

## 현행 시스템 파악 개념

### 파악 절차

1. 구성 / 기능 / 인터페이스 파악
2. 아키텍처 / 소트프웨어 구성 파악
3. 하드웨어 / 네트워크 구성 파악 

### SA

1. 소프트웨어 아키텍처
2. 소프트웨어 아키텍처 프레임워크
3. 소프트웨어 아키텍처 4+1 뷰
	1. 유스케이스 뷰
	2. 논리 뷰
	3. 프로세스 뷰
	4. 구현 뷰
	5. 배포 퓨
4. 소프트웨어 아키텍처 패턴
	1. Layered
	2. Client-Server
	3. Pipe-Filter
	4. Broker
	5. MVC
		1. 대화형 어플리케이션
	6. Master-Slave
		1. 실시간 시스템
5. 비용 평가모델
	1. SAAM
	2. ATAM
		1. 아키텍처 품질 속성
	3. CBAM
	4. ADR
	5. ARID


###  디자인 패턴

1. CSB
2. C
	1. Builder
	2. Prototype
	3. Factory Method
	4. Abstract Factory (Kit)
	5. Singleton
3. S
	1. Bridge
	2. Decorator
	3. Facade
	4. Flyweight
	5. Proxy
	6. Composite
	7. Adapter
4. B
	1. Mediator
	2. Interpreter
	3. Iterator
	4. Template Method
	5. Observer
	6. State
	7. Visitor
	8. Command
	9. Strategy
	10. Memento
		1. Undo
	11. Chain of Responsibility
		1. 하드코딩


## 요구사항

### 요구공학

### 분류
1. 기능적 요구사항
2. 비기능적 요구사항

* 요구사항 개발 단계 (CMM Level 3 프로세스)
	1. Elicitation
		1. Interview / Brainstroming / Delphi / 롤플레잉 / Workshop / survery / prototype
	2. Analysis
		1. DFD / Data Dictionary / UML
	3. Specification
		1. 비정형
		2. 정형: Z-schema, Petri NEts, 상태 차트
		* 요구사항 명세서
	4. Validation
		1. Peer Review / Walk Through / Inspection



* 요구사항 관리 단계 (CMM Level 2 프로세스)
	* 협상
	* 기준선 설정
		* 형상 관리
	* 변경 관리
		* 형상 통제 위원회회
	* 확인 및 검증







