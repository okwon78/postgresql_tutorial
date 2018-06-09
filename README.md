# Postgresql Tutorial

## 1. Create, Delete Database
### DB 생성

CREATE DATABASE sample_db;

### DB 삭제

DROP DATABASE sample_db;

## 2. Create and Delete Table
### Table 생성

CREATE TABLE company (
	id INT PRIMARY key NOT NULL,
	name text NOT NULL,
	age INT NOT NULL,
	address VARCHAR (255),
	salary REAL
);

CREATE TABLE department (
    id INT PRIMARY key NOT NULL,
    dept CHAR(50) NOT NULL,
    emp_id INT NOT NULL
);

CREATE TABLE student (
    id INT PRIMARY key NOT NULL,
    name text NOT NULL
);

### Table 삭제

DROP TABLE student;

## 3. Inserting Data into Tables

### 데이터 입력

INSERT INTO company (id, name, age, address, salary)
VALUES (1, 'Kwon', 20, 'Anyang', 4000);

INSERT INTO company (id, name, age, address, salary)
VALUES 
(3, 'Joon', 25, 'Daejeon', 5000), 
(4, 'Hyeok', 10, 'US', 500)

### 데이터 가져오기

SELECT * FROM company;

SELECT id, name FROM company;

## 4. WHERE Clause

### Comparison Operators

SELECT * FROM company WHERE age = 22;

SELECT * FROM company WHERE age != 22;

SELECT * FROM company WHERE age <> 22;  같지 않음 !=와 같은 의미

SELECT * FROM company WHERE age > 22;

SELECT * FROM company WHERE age >= 22;

SELECT * FROM company WHERE age < 22;

SELECT * FROM company WHERE age <= 22;

### Logical Operations

SELECT * FROM company
WHERE age > 20 AND age <25;

SELECT * FROM company
WHERE age > 23 AND salary > 4000;

SELECT * FROM company 
WHERE salary IS null;

SELECT * from company
WHERE name like 'C%';  (C로 시작되는 문자)

SELECT * from company
WHERE name like '%n'; (n으로 끝나는 문자)

SELECT * from company
WHERE name like '%a%'; (a가 들어간 모든 문자)

SELECT * from company
WHERE age in (20, 10, 9); (age가 20, 10, 9인 값)

SELECT * FROM company
WHERE age between 10 and 20; (age가 10부터 20인 값)

## 5. Comments and Expressions

### Comments

-- This is comment line

/*

This allows comment out

multiple lines

*/

### Expressions

SELECT name as employee_name FROM company;

SELECT count(*) FROM company; (필드 갯수를 반환)

SELECT max(salary) FROM company;

SELECT min(age) FROM company;

SELECT sum(salary) FROM company;

SELECT avg(salary) FROM company;

SELECT current_timestamp; (현재 타임존 시간 반환)



## 6. CHECK Constraint

CREATE TABLE products (
	product_num INT,
	name TEXT,
	price numeric CHECK (price > 0)
);

- CHECK 조건 맞지 않으면 동작 안함. ex) INSERT INTO products VALUES (1, 'AA', -10);  

CREATE TABLE products (
	product_num INT,
	name TEXT,
	price numeric CHECK (price > 0),
	discounted_price numeric CHECK (discounted_price > 0),
	CHECK (price > discounted_price)
);

- CHECK를 별도 라인으로 설정할 수 있음

## 7. UNIQUE AND NOT NULL Constraint

CREATE TABLE products (
	product_num INT NOT NULL,
	name TEXT NOT NULL,
	price numeric NOT NULL CHECK (price > 0)
);

- NULL 값은 UNIQUE에 해당되지 않는다. 중복된 NULL 값은 들어 갈 수 있다.

CREATE TABLE products (
	product_num INT UNIQUE,
	name TEXT NOT NULL,
	price numeric NOT NULL CHECK (price > 0)
);

- Product_num, name 조합이 고유 해야 한다. 하나라도 다른 값이면 입력 된다.

CREATE TABLE products (
	product_num INT,
	name TEXT NOT NULL,
	price numeric,
	UNIQUE(product_num, name)
);

## 8. PRIMARY KEY AND FOREIGN KEY

### PRIMARY KEY

CREATE TABLE products (
	product_no INT PRIMARY KEY,
	name TEXT,
	price numeric
);

- product_no, name을 같이 PK로 사용함

CREATE TABLE products (
	product_no INT,
	name TEXT,
	price numeric,
	PRIMARY KEY(product_no, name)
);

### FOREIGN KEY

- products 테이블의 product_no를 FK로 같는다.

CREATE TABLE orders (
	order_id INT PRIMARY KEY,
	product_no INT REFERENCES products(product_no),
	quantity INT CHECK (quantity > 0)
);

CREATE TABLE orders (
	order_id INT PRIMARY KEY,
	address TEXT NOT NULL
);

- 두 개의 테이블을 FK로 같는 경우

CREATE TABLE order_items (
	product_no INT REFERENCES products(product_no),
	order_id INT REFERENCES orders(order_id),
	quantity INT CHECK (quantity > 0),
	PRIMARY KEY(product_no, order_id)
);

## 9. ON DELETE RESTRICT AND ON DELETE CASCADE



- FK의 기본 동작은 ON DELETE RESTRICT 기능을 포함하기 때문에 필요는 없다.
- ON DELETE CASCADE 는 부모 table의 항목이 삭제되면 해당 항목을 참조하는 자식 항목도 같이 삭제된다.

CREATE TABLE order_items (
	product_no INT REFERENCES products(product_no) ON DELETE RESTRICT,
	order_id INT REFERENCES orders(order_id) ON DELETE CASCADE,
	quantity INT CHECK (quantity > 0),
	PRIMARY KEY(product_no, order_id)
);

## 10. ALTER TABLE

### 컬럼 추가 삭제

CREATE TABLE company (
	id INT NOT NULL,
	name text NOT NULL,
	age INT NOT NULL,
	address VARCHAR (255),
	salary REAL
);

- gender 컬럼 추가

ALTER TABLE company ADD gender char(1);

- gender 컬럼 삭제. 데이터가 있으면 같이 삭제 된다.

ALTER TABLE company DROP COLUMN gender;

### PK 추가 삭제

- pri 이름으로 PK 추가

ALTER TABLE company ADD CONSTRAINT pri PRIMARY KEY(id);

- pri 이름인 PK 삭제

ALTER TABLE company DROP CONSTRAINT pri;



## 11. UPDATE AND DELETE IN TABLE

- id값이 6인 필드의 age 값을 8로 변경

UPDATE company SET age=8
WHERE id=6;

- id값이 6인 필드 삭제 

DELETE FROM company WHERE id=6;

## 12. LIMIT And OFFSET Operators

- 3 개만 가지고 옴

SELECT * FROM company LIMIT 3;

- 앞의 3개 다음 3개를 가지고 옴

SELECT * FROM company LIMIT 3 OFFSET 3;

## 13. GROUP BY And HAVING Clause

- 고유한 age를 추출해서 표시

SELECT age FROM company
GROUP BY age;

- Age 별로 모아서 COUNT(*)로 갯수 표시

SELECT age, COUNT(*) FROM company
GROUP BY age;

- HAVING은 GROUP BY를 같이 사용해야 한다.
- HAVING은 그룹으로 묶인 항목에 대한 조건절 이다.
- group by로 모인 salary값의 최대 값이 2000보다 넘은 age

SELECT age, COUNT(*) FROM company
GROUP BY age HAVING MAX(salary) > 2000;

- group by로 모인 salary 값의 합의 2000보다 넘는 age 

SELECT age, COUNT(*) FROM company
GROUP BY age HAVING MAX(salary) > 2000;

SELECT age, COUNT(*) FROM company
GROUP BY age HAVING COUNT(*) > 1;

- HAVING 대신 다음과 같이 쓸 수 도 있다.

SELECT * FROM (
	SELECT age, COUNT(*) as emp_count FROM company
	GROUP BY age
) as gru 
WHERE emp_count > 1;

## 14. ORDER BY Clause

- ASC가 디폴드 값이라 적어주지 않아도 된다.
- ASC의 반대는 DESC 다.

SELECT * FROM company ORDER BY age ASC;

- age를 ASC로 정렬하고 같은 age안에서 DESC로 정렬

SELECT * FROM company ORDER BY age ASC, salary DESC;

## 15. Data Types
### ENUM data type
- sad, happy, ok 순서로 인덱스 값을 같는다.

CREATE TYPE mood as ENUM('sad', 'happy', 'ok');

CREATE TABLE person (
	name TEXT,
	current_mood mood
);

- ENUM 인덱스에 의해서 정렬 된다.

SELECT * FROM person ORDER BY current_mood;

SELECT min(current_mood) FROM person;

### JSON data type
CREATE TABLE jsondata (
	id INT,
	doc JSON
);

- SQL에서 JSON 문법 확인을 해준다.

INSERT INTO jsondata
VALUES (1, '{
            "name": "Kwon",
            "age": 20,
            "address": 	{
                        "first": "F",
                        "Second": "S"
                        },
            "post": 1101
		}'
	 );

- json에서 해당 항목을 가지고 온다.

SELECT doc->>'address' as addr FROM jsondata;

- 2차 접근

SELECT doc->'address'->>'first'  FROM jsondata;

- WHERE 조건절 추가

SELECT doc->'address'->>'first'  FROM jsondata WHERE id=1;

### Arrays Data type

#### Creating and Inserting Arrays

CREATE TABLE sal_emp (
	name TEXT,
	pay INT[],
	schedule TEXT[][]
);

INSERT INTO sal_emp
VALUES ('Kwon', 
		'{1000, 1000, 1000, 1000}',
		'{{"meeting", "lunch"}, {"training","presentation"}}'
	   );

- 인덱스 값은 1부터 시작한다.

SELECT pay[1] FROM sal_emp

SELECT name, pay[1] FROM sal_emp WHERE pay[1] <> pay[2]

SELECT array_dims(schedule) FROM sal_emp WHERE name='Kwon';

#### Updating Arrays

UPDATE sal_emp SET pay='{1, 2, 3, 4, 5}' where name='Kwon';

UPDATE sal_emp SET pay[1]=8000 where name='Kwon';

- 단일 값 append

UPDATE sal_emp SET pay = array_append(pay, 1234)

- Array 결합

UPDATE sal_emp SET pay = array_cat(pay, ARRAY[1234, 4321])

- 배열에서 값이 1234인 부분 전부 삭제 

UPDATE sal_emp SET pay = array_remove(pay, 1234);

#### Searching Arrays

SELECT * FROM sal_emp WHERE pay[1]=1 and pay[2]=2;

SELECT * FROM sal_emp WHERE 4321=ANY(pay);

SELECT * FROM sal_emp WHERE 4321=ALL(pay);

## 16. Aggregate Functions

### Basic

SELECT count(*) FROM company;

SELECT max(age) FROM company;

SELECT avg(age) FROM company;

### Advanced

SELECT array_agg(name) FROM company;

SELECT json_agg(name) FROM company;

- Key-value pair 형태의 json

SELECT json_object_agg(name, age) FROM company;

- 표준 편차, 분산

SELECT stddev(age) FROM company;

SELECT variance(age) FROM company;



## 17.  LIKE in PostgreSQL

- K글자로 시작되는 단어

SELECT * FROM company WHERE name like 'K%'

- 아무 문자나 하나 + h + 나머지 글자로 된 단어

SELECT * FROM company WHERE name like '_h%'

- 아무 글자 + o + 아무 글자

SELECT * FROM company WHERE name like '%o%'



## 18.  Join

### Cross Join

- 두 테이블의 컬럼 값에 모든 조합으로 JOIN 수행
- department 4개 company 8개 : 총 32개 로우 생성 됨

SELECT emp_id, name, dept FROM company CROSS JOIN department

### Inner Join

- 기준 컬럼의 값을 기준으로 JOIN 수행

SELECT name, age, dept FROM company INNER JOIN department ON company.id=department.emp_id

- 다음과 같이 작성할 수도 있다

SELECT name, age, dept FROM company c, department d

WHERE c.id=d.emp_id

### Left and Right Outer Joins

- Outer join은 Inner join 수행 후 수행된다.

- company 테이블을 기준으로 department에 해당되는 값이 있으면 채우고 없으면 NULL.

SELECT name, age, dept FROM company left outer join department
ON company.id=department.emp_id

- department 테이블을 기준으로 company에 해당되는 값이 있으면 채우고 없으면 NULL

SELECT name, age, dept FROM company right outer join department
ON company.id=department.emp_id

### Full Outer Join

- Left와 Right outer Join의 합집합
- 해당되는 로우를 전부 포함 시키고 빈 값은 NULL

SELECT name, age, dept FROM company FULL OUTER JOIN department
ON company.id=department.emp_id

## 19. Alias 

SELECT c.name, c.age, d.dept FROM company c, department d
WHERE c.id = d.emp_id

## 20. Views

- 사용 예로는 사용자 정보 중 일부만 공개해야 할 경우, 공개 가능한 컬럼만을 모아서 별도의 View를 생성해서 제공할 수 있다.
- 임의의 테이블이기 때문에 insert, update, delete 등을 수행할 수 없다.
- 원래 테이블이 변경되면 즉시 반영 된다.

CREATE VIEW company_view as SELECT name, address FROM company;

- Join을 사용해서 만들 수도 있다.

CREATE VIEW company_view AS 
SELECT name, address, dept FROM company, department WHERE company.id=department.emp_id;

## 21. UNION and UNION ALL

- 두 쿼리를 중복 없이 합쳐 준다. 결국 FULL OUTER JOIN과 동일하다.

SELECT name, address, dept FROM company LEFT OUTER JOIN department
ON company.id=department.emp_id
UNION
SELECT name, address, dept FROM company RIGHT OUTER JOIN department
ON company.id=department.emp_id

- 중복된 것도 같이 표시해 준다. 결국 INNER JOIN 부분이 중복되서 표시 된다.

SELECT name, address, dept FROM company LEFT OUTER JOIN department
ON company.id=department.emp_id
UNION ALL
SELECT name, address, dept FROM company RIGHT OUTER JOIN department
ON company.id=department.emp_id

## 22. Truncate Table

- DROP TABLE 사용 시, 스키마가 삭제되지만 TRUNCATE 사용 시, 데이터만 삭제된다.

TRUNCATE TABLE sal_emp;



## 23. Subqueries

- salaray 3000 이 넘는 age 조건들의 항목을 쿼리
- (주의) 3000 이상의 salary를 포함한 age가 이하에도 동일하게 있다면 포함 된다.

SELECT * FROM company where age in (
	SELECT age FROM company WHERE salary > 3000
)

UPDATE company SET salary=salary*1.5 where age in (
	SELECT age FROM company WHERE age > 10
);



## 24. INDEXES

- explain을 사용해서 성능을 살펴볼 수 있다.
EXPLAIN SELECT * FROM company WHERE name = 'Kwon';

CREATE INDEX name_idx ON company(name)

