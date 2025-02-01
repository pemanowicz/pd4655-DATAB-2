# pd4655-DATAB-2

**Schema (MySQL v5.7)**

    CREATE TABLE patients (
        patient_id INT PRIMARY KEY,
        name VARCHAR(255) NOT NULL,
        age INT NOT NULL,
        gender CHAR(1) NOT NULL
    );
    

---

**Query #1**

    INSERT INTO patients (patient_id, name, age, gender)
    VALUES 
        (1, 'John Doe', 30, 'M'),
        (2, 'Jane Smith', 28, 'F'),
        (3, 'Emily Davis', 35, 'F'),
        (4, 'James Brown', 40, 'M');

There are no results to be displayed.

---
**Query #2**

    
    
    CREATE TABLE tests (
        test_id INT PRIMARY KEY,
        patient_id INT,
        test_name VARCHAR(255) NOT NULL,
        result DECIMAL(6,2) NOT NULL,
        test_date DATE NOT NULL,
        FOREIGN KEY (patient_id) REFERENCES patients(patient_id)
    );

There are no results to be displayed.

---
**Query #3**

    
    
    INSERT INTO tests (test_id, patient_id, test_name, result, test_date)
    VALUES 
        (1, 1, 'Blood Sugar', 85.50, '2023-10-01'),
        (2, 2, 'Cholesterol', 190.00, '2023-09-20'),
        (3, 3, 'Blood Sugar', 102.00, '2023-08-15'),
        (4, 4, 'Vitamin D', 30.00, '2023-07-25'),
        (5, 1, 'Cholesterol', 210.00, '2023-06-30'),
        (6, 2, 'Blood Sugar', 95.00, '2023-05-15');

There are no results to be displayed.

---
**Query #4**

    
    
    SELECT p.name, t.result, t.test_date 
    FROM patients p
    INNER JOIN tests t ON p.patient_id = t.patient_id
    WHERE t.test_name = 'Blood Sugar'
    ORDER BY p.name;

| name        | result | test_date  |
| ----------- | ------ | ---------- |
| Emily Davis | 102.00 | 2023-08-15 |
| Jane Smith  | 95.00  | 2023-05-15 |
| John Doe    | 85.50  | 2023-10-01 |

---
**Query #5**

    
    
    SELECT AVG(result) AS avg_blood_sugar
    FROM tests
    WHERE test_name = 'Blood Sugar';

| avg_blood_sugar |
| --------------- |
| 94.166667       |

---
**Query #6**

    
    
    SELECT p.name, t.test_date
    FROM patients p
    INNER JOIN tests t ON p.patient_id = t.patient_id
    WHERE t.test_name = 'Cholesterol' 
    AND t.result = (SELECT MAX(result) FROM tests WHERE test_name = 'Cholesterol');

| name     | test_date  |
| -------- | ---------- |
| John Doe | 2023-06-30 |

---
**Query #7**

    
    
    SELECT p.name, COUNT(t.test_id) AS total_tests
    FROM patients p
    INNER JOIN tests t ON p.patient_id = t.patient_id
    GROUP BY p.patient_id;

| name        | total_tests |
| ----------- | ----------- |
| John Doe    | 2           |
| Jane Smith  | 2           |
| Emily Davis | 1           |
| James Brown | 1           |

---
**Query #8**

    
    
    SELECT p.name, t.test_name,
        CASE 
            WHEN t.test_name = 'Blood Sugar' AND t.result < 100 THEN 'Normalny'
            WHEN t.test_name = 'Blood Sugar' AND t.result >= 100 THEN 'Wysoki'
            WHEN t.test_name = 'Cholesterol' AND t.result < 200 THEN 'Normalny'
            WHEN t.test_name = 'Cholesterol' AND t.result >= 200 THEN 'Wysoki'
        END AS health_status
    FROM patients p
    INNER JOIN tests t ON p.patient_id = t.patient_id;

| name        | test_name   | health_status |
| ----------- | ----------- | ------------- |
| John Doe    | Blood Sugar | Normalny      |
| Jane Smith  | Cholesterol | Normalny      |
| Emily Davis | Blood Sugar | Wysoki        |
| James Brown | Vitamin D   |               |
| John Doe    | Cholesterol | Wysoki        |
| Jane Smith  | Blood Sugar | Normalny      |

---
**Query #9**

    
    
    SELECT p.name
    FROM patients p
    INNER JOIN tests t ON p.patient_id = t.patient_id
    WHERE t.test_name IN ('Blood Sugar', 'Vitamin D')
    GROUP BY p.patient_id
    HAVING COUNT(DISTINCT t.test_name) = 2;

There are no results to be displayed.

---

[View on DB Fiddle](https://www.db-fiddle.com/)
