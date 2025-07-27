

%% # 1. 요구사항 확인 %%


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






















