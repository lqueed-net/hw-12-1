# Домашнее задание к занятию "`Базы данных`" - `Осипов Геннадий`


### Задание 1

`
Опишите не менее семи таблиц, из которых состоит база данных. Определите:
какие данные хранятся в этих таблицах,
какой тип данных у столбцов в этих таблицах, если данные хранятся в PostgreSQL.
`

`При необходимости прикрепитe сюда скриншоты
![Название скриншота 1](ссылка на скриншот 1)`


---

### Задание 2

`Ниже приведен SQL код (DDL) для создания таблиц в PostgreSQL. Таблицы и связи протестированы на локальной БД.`

```
-- 1. Тип подразделения (Отдел, Группа, Департамент)
CREATE TABLE department_type (
    department_type_id SERIAL PRIMARY KEY,
    type_name VARCHAR(50) NOT NULL UNIQUE
);

-- 2. Структурное подразделение
CREATE TABLE structural_unit (
    structural_unit_id SERIAL PRIMARY KEY,
    unit_name VARCHAR(255) NOT NULL UNIQUE,
    department_type_id INT NOT NULL,
    FOREIGN KEY (department_type_id) REFERENCES department_type(department_type_id) ON DELETE RESTRICT
);

-- 3. Должность
CREATE TABLE position (
    position_id SERIAL PRIMARY KEY,
    position_name VARCHAR(255) NOT NULL UNIQUE
);

-- 4. Адрес филиала
CREATE TABLE branch_address (
    branch_address_id SERIAL PRIMARY KEY,
    full_address TEXT NOT NULL UNIQUE
);

-- 5. Проект
CREATE TABLE project (
    project_id SERIAL PRIMARY KEY,
    project_name VARCHAR(255) NOT NULL UNIQUE
);

-- 6. Сотрудник (основная таблица)
CREATE TABLE employee (
    employee_id SERIAL PRIMARY KEY,
    full_name VARCHAR(255) NOT NULL,
    salary NUMERIC(10, 2) NOT NULL CHECK (salary >= 0),
    position_id INT NOT NULL,
    department_type_id INT NOT NULL,
    structural_unit_id INT NOT NULL,
    hire_date DATE NOT NULL,
    branch_address_id INT NOT NULL,
    FOREIGN KEY (position_id) REFERENCES position(position_id) ON DELETE RESTRICT,
    FOREIGN KEY (department_type_id) REFERENCES department_type(department_type_id) ON DELETE RESTRICT,
    FOREIGN KEY (structural_unit_id) REFERENCES structural_unit(structural_unit_id) ON DELETE RESTRICT,
    FOREIGN KEY (branch_address_id) REFERENCES branch_address(branch_address_id) ON DELETE RESTRICT
);

-- 7. Связь сотрудников и проектов (many-to-many)
CREATE TABLE employee_project (
    employee_project_id SERIAL PRIMARY KEY,
    employee_id INT NOT NULL,
    project_id INT NOT NULL,
    FOREIGN KEY (employee_id) REFERENCES employee(employee_id) ON DELETE CASCADE,
    FOREIGN KEY (project_id) REFERENCES project(project_id) ON DELETE CASCADE
);
```
