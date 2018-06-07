# Postgresql tutorial

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