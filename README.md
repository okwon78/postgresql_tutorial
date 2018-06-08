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

## 15. Numeric Data Types

