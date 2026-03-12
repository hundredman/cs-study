# SQL

## 1. DDL (Data Definition Language)

데이터베이스 **구조를 정의**합니다. 자동으로 COMMIT됩니다.

| 명령어 | 설명 |
|--------|------|
| CREATE | 테이블/뷰/인덱스 생성 |
| ALTER | 구조 변경 (컬럼 추가/수정/삭제) |
| DROP | 테이블 삭제 (구조 + 데이터) |
| TRUNCATE | 데이터만 전체 삭제 (구조 유지) |

```sql
CREATE TABLE Student (
    id   INT PRIMARY KEY,
    name VARCHAR(50) NOT NULL,
    age  INT
);

ALTER TABLE Student ADD email VARCHAR(100);
DROP TABLE Student;
```

---

## 2. DML (Data Manipulation Language)

데이터를 **조작**합니다. COMMIT/ROLLBACK 가능합니다.

### SELECT

```sql
SELECT name, age
FROM   Student
WHERE  age >= 20
ORDER BY age DESC;
```

### INSERT / UPDATE / DELETE

```sql
INSERT INTO Student (id, name, age) VALUES (1, '김철수', 22);

UPDATE Student SET age = 23 WHERE id = 1;

DELETE FROM Student WHERE id = 1;
```

---

## 3. 조인 (JOIN)

두 테이블을 연결하여 데이터를 조회합니다.

| 종류 | 설명 |
|------|------|
| INNER JOIN | 양쪽 모두 일치하는 행만 반환 |
| LEFT JOIN | 왼쪽 테이블 전체 + 오른쪽 일치 행 |
| RIGHT JOIN | 오른쪽 테이블 전체 + 왼쪽 일치 행 |
| FULL OUTER JOIN | 양쪽 모두 반환 (불일치는 NULL) |
| CROSS JOIN | 모든 조합 (카테시안 곱) |

```sql
-- INNER JOIN
SELECT s.name, e.grade
FROM   Student s
INNER JOIN Enrollment e ON s.id = e.student_id;

-- LEFT JOIN (수강 안 한 학생도 포함)
SELECT s.name, e.grade
FROM   Student s
LEFT JOIN Enrollment e ON s.id = e.student_id;
```

---

## 4. 집계 함수 / GROUP BY / HAVING

```sql
SELECT dept, COUNT(*), AVG(salary)
FROM   Employee
GROUP BY dept
HAVING AVG(salary) > 3000;
```

| 함수 | 설명 |
|------|------|
| COUNT() | 행 수 |
| SUM() | 합계 |
| AVG() | 평균 |
| MAX() / MIN() | 최대 / 최솟값 |

> **WHERE vs HAVING**: WHERE는 그룹화 전 필터, HAVING은 그룹화 후 필터

---

## 5. 서브쿼리

쿼리 안에 또 다른 쿼리를 넣습니다.

```sql
-- 평균 급여보다 높은 사원 조회
SELECT name
FROM   Employee
WHERE  salary > (SELECT AVG(salary) FROM Employee);
```

---

## 6. 뷰 (View)

저장된 SELECT 쿼리입니다. 실제 데이터는 저장되지 않습니다.

```sql
CREATE VIEW HighSalary AS
SELECT name, salary FROM Employee WHERE salary > 5000;

SELECT * FROM HighSalary;
```

- **장점**: 보안(특정 컬럼 숨기기), 복잡한 쿼리 단순화
- **단점**: 인덱스 직접 생성 불가, 성능 최적화 한계

---

## 7. 인덱스 (Index)

데이터 검색 속도를 높이는 자료구조입니다 (B-Tree 기반).

```sql
CREATE INDEX idx_name ON Student(name);
```

- **장점**: SELECT 속도 향상
- **단점**: INSERT/UPDATE/DELETE 시 인덱스도 갱신 → 쓰기 성능 저하
- **자동 생성**: PRIMARY KEY, UNIQUE 제약조건

---

## 시험 포인트

- **DDL vs DML**: DDL은 자동 COMMIT, DML은 수동 COMMIT 필요
- **WHERE vs HAVING**: 그룹화 전/후 필터 차이
- **INNER vs LEFT JOIN**: 불일치 행 포함 여부
- **뷰**: 실제 데이터 저장 안 함, 가상 테이블
- **인덱스**: 읽기 빠름, 쓰기 느림
- **TRUNCATE vs DELETE**: TRUNCATE는 ROLLBACK 불가 (DDL)
