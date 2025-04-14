- Exercise Link:https://www.nowcoder.com/discuss/739230254375452672?sourceSSR=users

# Sol
- Use `if` to select male or female employees
- average them, use `coalesce` to fill in the Nulls
  
```mysql
WITH dep AS(
    SELECT staff_id,department,staff_gender
    FROM staff_tb
)
SELECT 
    b.department,
    round(avg(a.normal_salary-a.dock_salary),2) AS average_actual_salary,
    coalesce(round(avg(IF(b.staff_gender='male',a.normal_salary-a.dock_salary,NULL)),2),0.00) AS average_actual_salary_male,--此处不能先补后填充Null值，不然mean value可能不准确
    coalesce(round(avg(IF(b.staff_gender='female',a.normal_salary-a.dock_salary,NULL)),2),0.00) AS average_actual_salary_female
FROM salary_tb AS a
LEFT JOIN dep AS b
ON a.staff_id=b.staff_id
GROUP BY b.department
ORDER BY round(avg(a.normal_salary-a.dock_salary),2) DESC;
```
