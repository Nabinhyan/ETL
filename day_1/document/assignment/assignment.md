
Entity Identification
1.	punch_time  (dimension table)
2.	shift  (dimension table)
3.	role  (dimension table)
4.	department  (dimension table)
5.	bi_week  (dimension table)
6.	employee  (fact table)

Logical Design
Entity	Attributes	description
punch_time	punch_id (PK)
punch_on_time
punch_out_time
punch_date
punch_day
pay_code
cover_status	auto generated primary key
time type data
time type data
date type data
shows the weekdays 
shows the paycode of employee
shows if employee need to cover (BOOLEAN)
shift	shift_id (PK)
shift_start_time
shift_type	auto generated primary key
punch_on_time
type of shift based on punch_on_time
role	role_id (PK)
role_name	auto generated primary key
shows the name of role of employee
department	department_id (PK)
department_name	auto generated primary key
shows the department name 
bi_week	bi_week_id (PK)
start_date
to_date	auto generated primary key
shows the start date for starting biweekly analysis
shows to which date do the biweekly analysis is
employee	punch_id (FK)
shift_id (FK)
department_id (FK)
bi_week_id (FK)
employee_id 
work_hour
salary	valid id from punch_time table
valid id from shift table
valid id from department table
valid id from bi_week table
shows the employee id of the employee
shows the working hour of the employee
shows the salary of the employee












	








 
Physical model
















Diagram link
 
Physical implementation
CREATE SCHEMA IF NOT EXISTS etl;

CREATE TABLE IF NOT EXISTS etl.punch_time(
    punch_id SERIAL PRIMARY KEY,
    punch_on_time TIME,
    punch_out_time TIME,
    punch_date DATE,
    punch_day VARCHAR(10),
    pay_code VARCHAR(12) NOT NULL,
    cover_status BOOLEAN NOT NULL
);

CREATE TABLE IF NOT EXISTS etl.department(
    department_id SMALLSERIAL PRIMARY KEY,
    department_name VARCHAR(70) NOT NULL
);

CREATE TABLE IF NOT EXISTS etl.shift(
    shift_id SMALLSERIAL PRIMARY KEY,
    shift_start_time TIME,
    shift_type VARCHAR(9)
);

CREATE TABLE IF NOT EXISTS etl.role(
    role_id SMALLSERIAL PRIMARY KEY,
    role_nane VARCHAR(70)
);

CREATE TABLE IF NOT EXISTS etl.bi_week(
    bi_week_id SERIAL PRIMARY KEY,
    start_date DATE,
    to_date DATE
);

CREATE TABLE IF NOT EXISTS etl.employee(
    punch_id INT,
    shift_id SMALLINT,
    role_id SMALLINT,
    department_id SMALLINT,
    bi_week_id INT,
    employee_id INT,
    work_hour FLOAT,
    salary NUMERIC(10, 2),
    FOREIGN KEY (punch_id) REFERENCES etl.punch_time(punch_id),
    FOREIGN KEY (shift_id) REFERENCES etl.shift(shift_id),
    FOREIGN KEY (role_id) REFERENCES etl.role(role_id),
    FOREIGN KEY (department_id) REFERENCES etl.department(department_id),
    FOREIGN KEY (bi_week_id) REFERENCES etl.bi_week(bi_week_id)
);
