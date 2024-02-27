# Hospital Database SQL Project

## Project Overview

This project involves utilizing SQL database queries to manage hospital administration tasks. The project explores an existing hospital database, executing various SQL queries to gain insights into the dataset. The database encompasses information crucial for hospital management, including patient records, staff details, doctor information, and disease data.

*Project Done by:*  
Vishnu Priya Ashok Kumar  

*Course Info:* INFO8075 SQL and Data Analysis    
*Conestoga College, Doon Campus*

## Table of Contents

1. [Introduction](#introduction)
2. [ER Diagram for Hospital Database](#er-diagram-for-hospital-database)
3. [Implementing Various SQL Query Functions](#implementing-various-sql-query-functions)
4. [Goal of the Project](#goal-of-the-project)

## Introduction

In this project, SQL queries are applied to analyze and manage hospital data. The database comprises four main tables:

- **Staffs**: Information about hospital staff, including names, departments, IDs, and salaries.
- **Doctor**: Details of doctors employed by the hospital, including names, IDs, specialized departments, and appointment times.
- **Patient**: Records of patients, including names, IDs, ages, diseases, assigned doctor IDs, and locations.
- **Diseases**: Data regarding various diseases, including names, IDs, and associated departments.

The project aims to demonstrate the application of SQL queries for hospital management tasks, leveraging the datasets provided.

## ER Diagram for Hospital Database

![image](https://github.com/priya-ak/Hospital-Database-SQL-Analysis/assets/67804361/8cde1855-c605-4266-b799-452729a365bb)


## 3. Implementing Various SQL Query Functions

### 3.1 Query to Display Patient's Rank Based on Their Assigned Doctor
```sql
SELECT CONCAT(patient_firstname, ' ', patient_lastname) AS patient_NAME, 
d.doctor_name, d.doctor_department,patient_age,
RANK() OVER (
	   PARTITION BY p.doctor_id ORDER BY patient_age) AS patient_Rank
FROM patient p
INNER JOIN doctor d
ON p.doctor_id = d.doctor_id;

```
![image](https://github.com/priya-ak/Hospital-Database-SQL-Analysis/assets/67804361/77348c1c-50a1-405e-8431-b68ba3e368f6)

### 3.2 Query to Display Sum of Patients By Dividing Their Age Into Bucket of 5 Ranging Between 15 to 90
```sql
SELECT BUCKET, SUM(patient_count) OVER (ORDER BY BUCKET) FROM
(
SELECT WIDTH_BUCKET(patient_age, 15, 90, 5) AS BUCKET,
COUNT(*) AS patient_count
FROM patient
GROUP BY BUCKET
ORDER BY BUCKET ) S;

```
![image](https://github.com/priya-ak/Hospital-Database-SQL-Analysis/assets/67804361/75d3ae6a-832e-4686-93e4-04e6e791e48d)

### 3.3 Query to Display Patient Name and Age and Categorizing Patient into Elderly, Youth, and Teenager
```sql
SELECT Patient_id,Patient_FirstName,Patient_lastName,Patient_Age,
CASE WHEN patient_Age >=50 THEN 'Elderly'
          	WHEN patient_Age between 23 and 48 THEN 'youth'
         	 ELSE'Teenager'
 	 END AS PatientAge_range
FROM Patient

```
![image](https://github.com/priya-ak/Hospital-Database-SQL-Analysis/assets/67804361/59f2998b-a383-4837-bc96-8961633d8cc1)

### 3.4 Display Disease Count Most Common in Lab
```sql
WITH top_diseaselab as (
        SELECT Disease.disease_lab, count(patient.disease_id) as disease_count
FROM Disease, Patient
WHERE Disease.disease_id =patient.disease_id
GROUP BY Disease.disease_lab
)
SELECT Disease.*, top_diseaselab.Disease_count
FROM Disease
JOIN top_diseaselab
ON Disease.disease_lab =top_diseaselab.disease_lab
ORDER BY Disease_count Desc;

```
![image](https://github.com/priya-ak/Hospital-Database-SQL-Analysis/assets/67804361/b2455c7f-6eec-4f6e-8c4f-477e9cf2c54b)

### 3.5 Query to Create a Percentile on Salary Based on Doctor’s ID
```sql
SELECT staff_id, doctor_id, staff_salary,
NTILE (3) OVER (PARTITION BY doctor_id ORDER BY staff_salary) AS percentile
FROM staff;

```
![image](https://github.com/priya-ak/Hospital-Database-SQL-Analysis/assets/67804361/05ccc6ac-9917-4d79-9f9f-54c3d6617fa2)

### 3.6 Query to Identify the Subtotal and Total of Salaries Paid to Doctors According to their ID
```sql
SELECT staff_id, doctor_id, SUM (staff_salary) AS salary_earned 
FROM staff
GROUP BY CUBE (staff_id, doctor_id)
ORDER BY doctor_id, staff_id;
```
![image](https://github.com/priya-ak/Hospital-Database-SQL-Analysis/assets/67804361/8c4416c9-07aa-47fc-ba54-e5be34aeb82b)

### 3.7 Query to Display Doctor’s and Staff Tables Using Join and Subqueries Statements
```sql
SELECT d.doctor_id, d.doctor_name
FROM doctor d
INNER JOIN staff s
ON d.doctor_id = s.doctor_id
WHERE d.doctor_id=ANY(SELECT doctor_id FROM staff)
AND s.staff_salary >=2000;

```
![image](https://github.com/priya-ak/Hospital-Database-SQL-Analysis/assets/67804361/1645b1b7-c4a9-4841-8f30-3583315e7059)

### 3.8 Query to Display Aggregation and Joins of Staff and Patient Table
```sql
SELECT A.doctor_id, B.staff_name,
COUNT (DISTINCT B.staff_id ) AS staff_count,
SUM(B.staff_salary) as Total_salary
FROM patient A
INNER JOIN staff B
ON A.doctor_id=B.doctor_id
GROUP BY A.doctor_id, B.staff_name

```
![image](https://github.com/priya-ak/Hospital-Database-SQL-Analysis/assets/67804361/c6722037-3b9b-4dce-9808-eb5c41641a52)
![image](https://github.com/priya-ak/Hospital-Database-SQL-Analysis/assets/67804361/14066ec0-091f-4f1b-9618-98d4bc349d7f)


### 3.9 Query to Display Doctor’s name, Department Names, Staffs and Departments Using the ‘Left Join’
```sql
SELECT doctor_name, doctor_department, staff_name, staff_department 
FROM doctor   
LEFT JOIN staff  
ON doctor.doctor_id = staff.doctor_id; 

```
![image](https://github.com/priya-ak/Hospital-Database-SQL-Analysis/assets/67804361/a6bd6d82-335e-4df3-ab10-f494cd575247)

### 3.10 Query using ‘Replace’ to Change Doctor Appointment Time
```sql
SELECT doctor_appointment, REPLACE(doctor_appointment, '10AM - 1PM', '10AM - 12PM')
FROM doctor;

```
![image](https://github.com/priya-ak/Hospital-Database-SQL-Analysis/assets/67804361/4b49f1e5-9cd8-43eb-918f-53966c9a3432)

### 3.11 Query to Change Patient Disease Datatype Using The ‘Cast’ Syntax
```sql
SELECT patient_firstname, CAST (patient_disease AS CHAR(30))  
Char_patient_disease FROM patient;
```
![image](https://github.com/priya-ak/Hospital-Database-SQL-Analysis/assets/67804361/a6a13469-c3ce-466d-b487-d5a1c0945181)


### 3.12 Query To Display Patient based on Skin Disease
![image](https://github.com/priya-ak/Hospital-Database-SQL-Analysis/assets/67804361/413de5e5-4364-49a0-b806-baf235f37ef6)


### 3.13 Query to Select from the Doctor Department Specialised in Clinical Immunology
![image](https://github.com/priya-ak/Hospital-Database-SQL-Analysis/assets/67804361/5651da98-5f26-463a-acd5-f590c3d92a6e)


### 3.14 Using the Case When to Identify Different Disease Names and How They Should Be Handled
![image](https://github.com/priya-ak/Hospital-Database-SQL-Analysis/assets/67804361/4b70868d-b8ea-4253-ad5f-5ae8c7f2aa71)


### 3.15 Using the Max Function with Having Clause and Select the Highest Payment
![image](https://github.com/priya-ak/Hospital-Database-SQL-Analysis/assets/67804361/58450fc6-e317-48fd-8272-2d0b488482b6)


## Goal of the Project

The primary objectives of this project are:

1. To analyze patient ranks based on assigned doctors.
2. To summarize patient counts by age groups.
3. To categorize patients into different age categories.
4. To identify common diseases in the lab.
5. To create percentiles of salary for doctors.
6. To calculate salary subtotals and totals for doctors.
7. To display doctor and staff information using joins and subqueries.
8. To perform aggregations and joins on staff and patient tables.
9. To display doctor, department, staff, and departmental information.
10. To modify doctor appointment times.
11. To change patient disease data types.
12. To query and display patients based on specific diseases.
13. To select doctors from specific departments.
14. To handle different disease names appropriately.
15. To identify the highest staff payments.

This project aims to showcase the practical application of SQL queries in hospital management and data analysis tasks.
