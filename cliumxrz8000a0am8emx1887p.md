---
title: "Postgres CASE Statement Basics with Examples"
datePublished: Tue Jun 13 2023 18:47:20 GMT+0000 (Coordinated Universal Time)
cuid: cliumxrz8000a0am8emx1887p
slug: postgres-case-statement-basics-with-examples

---

# Introduction

Control Structures can be considered as one of the building blocks of a computer program. They control the execution order of a program and allow you to perform an operation when a specific condition is met. Similarly, In PostgreSQL, there are times when you need to specify a condition and the data that should be returned when the condition is met. PostgreSQL provides CASE statements that go through conditions and return a value when the first condition is met (like an if-then-else statement). When a condition is met, it stops reading and returns the result, this continues for all conditions that are met. If no condition is satisfied it returns the value in the ELSE block

In this article, you will learn about CASE statements in PostgreSQL, best practices for writing CASE statements, their importance, and the use of CASE statements in RedShift

# Case Statements in Postgres

The CASE Statement is very similar to IF-THEN statements in most programming languages. It is SQL’s way of handling if/then logic. The syntax of a CASE statement is given as

```typescript
CASE
    WHEN condition1 THEN result1
    WHEN condition2 THEN result2
    WHEN conditionN THEN resultN
    ELSE result
END;
```

CASE statements in PostgreSQL can be used in two formats

* **Simple CASE statement**: A simple case statement compares a column or an expression with another expression and outputs the result when the condition is satisfied. e.g
    

```typescript
SELECT firstname, lastname,
      CASE firstname
         WHEN 'albert' THEN 'Great Scientist'
         ELSE 'Brilliant Scientist'
      END AS comments
FROM  scientist
```

The query above will return the first name and last name with a column called comments that evaluate to **Great Scientist** when firstname is **albert** and returns **Brilliant Scientist** for other rows

* **Search CASE Statement**: A search case statement evaluates a set of boolean expressions or conditions to produce an output
    

```typescript
SELECT name, age, id,
      CASE   
         WHEN age >= 0  and age <= 14 THEN 'Children'  
         WHEN age >= 15  and age <= 24 THEN 'Youths'  
         WHEN age >= 25  and age <= 64 THEN 'Adult'  
         WHEN age >= 65  and age <= 100 THEN 'Seniors'  
         ELSE 'Senior Men and Women'  
      END as age_bracket
FROM users
```

# Why CASE Statements

CASE Statements are extremely versatile and are used in lots of complex queries. They are useful for organizing and beautifying structured data and also perform checks to prevent divide by zero. They can also be used in GROUP BY, HAVING, and ORDER BY clauses.

Case Statements are also useful for data transformation i.e transforming data from one form to another. e.g. Given a gender column with values **M** for Male and **F** for Female, you can use a CASE statement to transform the value of **M** to Male and **F** to Female for each row.

```typescript
SELECT JobTitle,
        CASE Gender
           WHEN 'M' THEN 'Male'
           WHEN 'F' THEN 'Female'
           ELSE 'Unknown Value'
        END
 FROM  Employee
```

You can also use CASE statements to standardize several values into one. For example. you can assign the same result to more than one value in a column. Using the previous example, it's possible to map several variations to either Male or Female

```typescript
SELECT JobTitle,
        CASE Gender
           WHEN 'M' THEN 'Male'
           WHEN '0' THEN 'Male'
           WHEN 'F' THEN 'Female'
           WHEN '1' THEN 'Female'
           ELSE 'Unknown Value'
        END
 FROM  Employee
```

# The Execution Order of CASE Statements

CASE Statements evaluates its conditions sequentially and stops when a condition is satisfied. It stops after the first match or the first expression that is evaluated to be a match and does not continue with the rest.

```typescript
CASE
    WHEN condition1 THEN result1
    WHEN condition2 THEN result2
    WHEN conditionN THEN resultN
    ELSE result
END;
```

For example, in the above snippet, when the `condition1` is satisfied it evaluates to `result1`. If the `condition2` is not satisfied the value in the `ELSE` block will be used. The `ELSE` block is an optional part and the value, `NULL` will be returned if the `ELSE` block is not specified. It will always stop execution once the first condition is met.

Case Statements also perform **short-circuit evaluation** i.e evaluating each condition and comparing with an expression instead of having to evaluate all conditions before comparing to expression.

# Best Practices for Using CASE Statements

There are several performance tips and benefits to be aware of when using CASE statements in PostgreSQL.

* It's good to always put the evaluations in this CASE syntax in the order that the criteria will be found first. For example
    

```typescript
SELECT name,email,
        CASE grade
           WHEN 'A' THEN 'Excellent'
           WHEN 'B' THEN 'Good'
           WHEN 'C' THEN 'Fair'
           WHEN 'D' THEN 'Pass'
           ELSE 'Fail' 
        END as remarks
 FROM students_results
```

* it's best practice to create CASE statements that don't overlap i.e
    

```typescript
SELECT name, weight,
       CASE 
            WHEN weight > 450 THEN 'over 450'
            WHEN weight > 300 THEN '301-450'
            WHEN weight > 150 THEN '150-300'
            ELSE '150 or under' END AS weight
  FROM players
```

If the weight in a column is `120` for example. The first two conditions will not be satisfied because 120 is not greater than `450` and `300` but you can see that the first two conditions overlap when the weight is less than the value. You can fix this by clearly specifying a range in the condition

```typescript
SELECT name, weight,
       CASE 
            WHEN weight > 450 THEN 'over 450'
            WHEN weight > 300 AND weight <= 450 THEN '301-450'
            WHEN weight > 150 AND weight <=300 THEN '150-300'
            ELSE '150 or under' END AS weight
  FROM players
```

# More Use Cases

* **CASE Statements With** `IN` Clause
    

```typescript
SELECT id, month, country,
    CASE
      WHEN month IN ('December', 'January', 'February') THEN 'Winter'
      WHEN month IN ('March', 'April', 'May') THEN 'Spring'
      WHEN month IN ('June', 'July', 'August') THEN 'Summer'
      WHEN month IN ('September', 'October', 'November') THEN 'Autumn'
    ELSE 'Unknown'
END as seasons
FROM users
ORDER BY id;
```

You can also use CASE Statements with the `IN` clause to check if a condition exists in a list of values

* **CASE Within CASE Statement**
    

```typescript
SELECT id, month, country,
     CASE
           WHEN month IN ('December', 'January', 'February') THEN
                 (CASE WHEN country = 'Nigeria' THEN 'Nigeria A' ELSE 'Nigeria B' END)
           WHEN month IN ('September', 'October', 'November') THEN
                (CASE WHEN country = 'Spain' THEN 'Europe F' ELSE 'Europe M' END)
     ELSE 'Unknown'
END as seasons
FROM users
ORDER BY id;
```

* **Using CASE statements with AGGREGATE Functions**
    

```typescript
SELECT
       SUM(CASE score
             WHEN 'A' THEN 1 
		        ELSE 0 
		     END
        ) "A students",
       SUM(CASE score
             WHEN 'B' THEN 1 
		       ELSE 0 
		     END
        ) "B Students",
       SUM(CASE score
             WHEN 'C' THEN 1 
		       ELSE 0 
		     END
        ) "C Students",
       SUM(CASE score
             WHEN 'D' THEN 1 
		       ELSE 0 
		     END
       ) "D Students",
       SUM(CASE score
             WHEN 'E' THEN 1 
		       ELSE 0 
		     END
       ) "E Students"
FROM scores;
```

# Usage in Redshift

If you understand all the concepts above, Using CASES in Amazon Redshift will be very easy as there is not much difference.

* Amazon Redshift Searched CASE expression.
    

```typescript
SELECT
  CASE
    WHEN score < 70 THEN 'failed'
    WHEN score BETWEEN 70 AND 80 THEN 'passed'
    WHEN score BETWEEN 81 AND 90 THEN 'very good'
    ELSE 'outstanding'
  END AS performance
FROM test_scores;
```

* Amazon Redshift Simple Case Expression
    

```typescript
SELECT
  CASE grade
    WHEN 'A' THEN 'Excellent'
    WHEN 'B' THEN 'Good'
    WHEN 'C' THEN 'Needs Improvement'
    ELSE 'Failed'
  END AS grade_interpretation
FROM grades;
```

# Conclusion

Using CASE Statements can be very useful as they allow you to specify several conditions and the values to be returned when each requirement is satisfied. As seen above, they are also very handy for handling aggregate functions and writing complex PostgreSQL queries.