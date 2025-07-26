## 2.1. 물리 엔진과 오브젝트 용어
---
### 2.1.1. DB 엔진 용어
1. MySQL 엔진
2. 스토리지 엔진
	1. InnoDB, MyISAM, Memory, SEQUENCE 엔진


---
### 2.1.2. SQL 프로세스 용어

- PPOE
	1. PARSER
		- 구성요소 최소단위로 분리, 트리化
		- 문법 검사
	2. PREPROCESSOR
		1. 구조적인 문제
		2. 테이블, 컬럼이 실존하는지
	3. OPTIMIZER
	4. EXECUTION ENGINE


---

### 2.1.3. DB 오브젝트 용어
1. TABLE
2. ROW
3. COLUMN
4. Primary Key
	- Clustered Index
	- Non-Clustered Index
5. Foreign Key
6. Index
	- Unique Index
		- 기본키와 고유인덱스의 차이점?
	- non-Unique Index
	- 오버헤드를 발생시키는 조건은?
7. VIEW

---
## 2.2. 논리적인 SQL 개념 용어
### 2.2.1. 서브쿼리 위치

1. SCALAR SUBQUERY
	* SELECT, (FROM, WHERE)
2. INLINE VIEW
	* FROM
	* 내부 동작구조는 임시테이블
3. NESTED SUBQUERY
	* SELECT, WHERE
	* IN, EXISTS, NOT IN, NOT EXISTS



---
### 2.2.2. 서브쿼리 - 메인쿼리 관계성

1. non-corrleated subquery
	* 서브쿼리 실행 → 메인쿼리 실행
	* 메인쿼리 결과값을 서브쿼리가 받아쓰는 상황이 아닐 경우
	* 옵티마이저에 따라 view merging = SQL rewirte 작동
2. corrleated subquery
	* 메인쿼리 실행 → 서브쿼리 실행 → 메인쿼리 실행
	* SELECT scalar 서브쿼리, WHERE nested 서브쿼리
	* 옵티마이저에 따라 view merging = SQL rewirte 작동


---

### 2.2.3. 서브쿼리의 반환 결과

1. single-row subquery
	* SELECT 절 서브쿼리
2. multiple-row subquery
	* IN 구문
3. multiple-column subquery
	* `WHERE (이름, 전공코드 IN (SELECT 이름, 전공코드 FROM TBL1))`


---
### 2.2.4. JOIN 연산방식

1. INNER
2. OUTER
	* FULL OUTER: $n^2+2n$
3. CROSS JOIN: $n^2$
	* cartesian product
4. NATURAL JOIN
	* CROSS JOIN 으로 변경되는 조건은?

```SQL
SELECT *
FROM 교수, 학생
	ON 교수.학번 = 학생.학번
```

---
### 2.2.5. JOIN 알고리즘

* JOIN 조건절의 열이 인덱스이도록 + Driving 결과가 적도록
	1. driving table
		* 실행 먼저
	2. driven table
		* 실행 뒤늦게

> 학생 테이블 100 row, 비상연락망 테이블 1000 row
> `SELECT` 에 대응하는 학생 테이블 2 row, ON 에 해당하는 비상연락망 테이블 각각 2 row / 1 row

1. nested loop join
	* driving 데이터 1건 당 driven 반복검색
	* ~~(100+1000) + (100+1000)~~  : 인덱스 없는 케이스
	* (1+2) + (1+1)
		* 비고유 인덱스일 경우 [[랜덤 액세스]]
2. block nested loop join (BNL)
	* JOIN buffer
		* driving 에서 대상을 조인 버퍼에 전부 적재 → driven 을 1번만 full scan
	* 파생: block hash join
		* JOIN buffer 값에 해시값 후, 해시값 사용해서 driven 참조
	* 필연적으로 인덱스에 의한 [[랜덤 액세스]] → 해결책 [[BKA]]
3. batched key access join (BKA)
	* 접근할 데이터를 미리 예상 ([[MRR]]) → 랜덤 액세스 아니라 시퀀셜 액세스
	* JOIN buffer
	* driving → driven index → JOIN buffer 체크 → 대응하는 데이터값들을 driven 에서 추출
4. hash JOIN
	* block nested loop <u>**HASH**</u>
	* 해시값으로 내부 조인을 실행한 결과는 조인 버퍼에 저장되므로, 조인 열의 인덱스를 필수적으로 요구하지 않아도 됩니다.
	* 대용량 데이터의 동등 비교 연산



---
## 2.3. 개념적인 튜닝 용어
### 2.3.1. 기초 용어


1. 오브젝트 스캔 유형
	1. 테이블 풀 스캔 
		1. 인덱스 없을시의 유일한 방식
	2. 인덱스 풀 스캔 
	3. 인덱스 range 스캔
	4. 인덱스 unique 스캔
		1. 기본키 / 고유인덱스로 접근
	5. 인덱스 loose 스캔
	6. 인덱스 ,erge 스캔
2. 디스크 접근 방식
	* 페이지란, 데이터를 검색하는 최소 단위
	1. sequential access
		1. 테이블 풀 스캔
			1. multi-page read
	2. random access
3. 조건 유형
	1. 액세스 조건
	2. 필터 조건
		1. 필터 조건이 없다면, 매우 훌륭한 SQL 문
---
### 2.3.2. 응용 용어

1. 셀렉티비티
2. 카디널리티
3. 힌트
	1. 상황에 따라 옵티마이저는 힌트를 안쓴다.
	2. 언어에 따라 힌트로 오류가 발생할지 / 힌트를 무시하는지가 갈린다.

```SQL
FROM 학생 /*! USE INDEX (학생_IDX01) */
FROM 학생 USE INDEX (학생_IDX01)
```

4. COLLATION
5. HISTOGRAM

