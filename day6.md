项目一
```sql
SELECT department.`Name` as Department,
employee.`Name` as Employee,
employee.Salary as Salary
FROM employee,department
WHERE employee.Salary in (SELECT MAX(employee.Salary)
FROM employee GROUP BY employee.DepartmentId) AND employee.DepartmentId=department.Id
ORDER BY  Salary DESC;
```
项目二

```sql
(SELECT id+1 as id,student FROM seat WHERE id %2 =1 and NOT id=(SELECT MAX(id) FROM seat)
UNION
SELECT id-1 as id,student FROM seat WHERE id%2=0
UNION
SELECT id as id,student FROM seat WHERE id=(SELECT MAX(id) FROM seat))
ORDER BY id;
```

项目三

```sql
SELECT Score,
(SELECT COUNT(DISTINCT Score) 
FROM score WHERE score>=s.Score) as 'rank' 
FROM score as s 
ORDER BY Score DESC;
```
