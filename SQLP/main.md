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


---
## 3.2. 실행 계획 수행

### 3.2.2. 기본 실행 계획 항목 분석

![[Pasted image 20250727143828.png]]

1. ID
	1. 조인되는 테이블끼리는 똑같은 ID가 표시됨
2. SELECT_TYPE
	1. SIMPLE
	2. PRIMARY
	3. SUBQUERY
	4. DERIVED
	5. UNION
	6. UNION RESULT
	7. DEPENDENT SUBQUERY
	8. DEPENDENT UNION
	9. UNCACHEABLE SUBQUERY
	10. MATERIALIZED
3. TABLE
4. PARTITIONS
5. TYPE
	1. system
	2. const
	3. eq_ref
	4. ref
	5. ref_or_null
	6. range
	7. fulltext
	8. index_merge
	9. index
	10. ALL
6. possible_keys
7. key
8. key_len
9. ref
10. rows
11. filtered
12. extra
	1. Distinct
	2. Using where
	3. Using temporary
	4. Using index
	5. Using filesort
	6. Using join buffer
	7. Using union / Using intersect / Using sort_union
	8. Using index condition
	9. Using index condition (BKA)
	10. Using index for group-by
	11. Not exists




---
### 3.2.3. 좋고 나쁨을 판단하는 기준


- 2. SIMPLE_TYPE
	- 좋음: SIMPLE, PRIMARY, DERIVED
	- 나쁨: DEPENDENT ~, UNCACHEABLE ~
- 5. TYPE
	- 좋음: SYSTEM, CONST, EQ_REF
	- 나쁨: INDEX, ALL
- 12. EXTRA
	- 좋음: USING INDEX
	- 나쁨: USING FILESORT, USING TEMPORARY

---
### 3.2.4. 확장된 실행 계획 수행

1. Mysql
	1. EXPLAIN FORMAT = TRADITIONAL
	2. EXPLAIN FORMAT = TREE
	3. EXPLAIN FORMAT = JSON
	4. EXPLAIN ANALYZE
2. MariaDB
	1. EXPLAIN PARTITIONS
	2. EXPLAIN EXTENDED
	3. ANALYZE



---
### 3.3. PROFILING









---
# 4. SQL 튜닝

---
## 4.1. SQL 튜닝 준비

### 4.2.1. 기본키 변형
1. SQL 실행결과 & 현황 파악
	1. 결과 및 소요시간 확인
	2. 조인/서브쿼리 구조
	3. 동등/범위 조건
2. 체크요소
	1. 가시적
		1. 테이블의 데이터 건수
		2. SELECT 절 컬럼 분석
		3. 조건절 컬럼 분석
		4. 그루핑/정렬 컬럼
	2. 비가시적
		1. 실행계획
		2. 인덱스 현황
		3. 데이터 변경 추이
		4. 업무적 특징
3. 튜닝 방향 판단 & 개선/적용


---
## 4.2. 튜닝 케이스

### 4.2.1. 기본키 변형 → 온존

```SQL
-- 튜닝 전
EXPLAIN
SELECT *
FROM 사원
WHERE SUBSTRING(사원번호,1,4) = 1100
AND LENGTH(사원번호) = 5;

-- 튜닝 후
EXPLAIN
SELECT *
FROM 사원
WHERE 사원번호 BETWEEN 11000 AND 11009;
```


### 4.2.2. 미사용 함수 포함

```SQL
+----------+---------------+------+-----+---------+-------+
| Field    | Type          | Null | Key | Default | Extra |
+----------+---------------+------+-----+---------+-------+
| 사원번호 | int           | NO   | PRI | NULL    |       |
| 생년월일 | date          | NO   |     | NULL    |       |
| 이름     | varchar(14)   | NO   |     | NULL    |       |
| 성       | varchar(16)   | NO   |     | NULL    |       |
| 성별     | enum('M','F') | NO   | MUL | NULL    |       |
| 입사일자 | date          | NO   | MUL | NULL    |       |
+----------+---------------+------+-----+---------+-------+

SELECT IFNULL(성별,'NO DATA') AS 성별, COUNT(1) 건수
FROM 사원
GROUP BY IFNULL(성별,'NO DATA');

SELECT 성별, COUNT(1) 건수
FROM 사원
GROUP BY 성별;
```


















