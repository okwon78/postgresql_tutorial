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



