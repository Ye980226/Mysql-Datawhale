项目一

```sql
CREATE VIEW banned_list as SELECT DISTINCT users.Users_Id,users.Role FROM trips,users 
WHERE 
((users.Role='client' AND trips.Client_Id=users.Users_Id) 
OR (users.Role='driver' AND trips.Driver_Id=users.Users_Id)) AND users.Banned='Yes';

CREATE VIEW non_banned_list as (SELECT `Status`,`Request_at` 
FROM trips
WHERE NOT ((Client_Id IN (SELECT Users_Id FROM banned_list) 
AND 'client'=(SELECT `Role` FROM banned_list WHERE `Users_Id`=`Client_Id`)) 
OR (Driver_Id IN (SELECT Users_Id FROM banned_list)
AND 'driver'=(SELECT `Role` FROM banned_list WHERE `Users_Id`=`Driver_Id`))));

SELECT s1.Request_at as `Day`,ROUND(s1.cancelled_rate/s2.all_status,2) as `Cancellation Rate` 
FROM 
(SELECT Request_at,SUM(CASE WHEN `Status`='cancelled_by_driver' THEN 1 WHEN `Status`='cancelled_by_client' THEN 1 ELSE 0 END) as cancelled_rate 
FROM non_banned_list  GROUP BY Request_at) as s1,
(SELECT Request_at,COUNT(`Status`) as all_status FROM non_banned_list GROUP BY Request_at) as s2
WHERE s1.Request_at=s2.Request_at;
```

项目二

```sql
CREATE VIEW dif_part_salary_rank AS SELECT DepartmentId,`Name`,Salary,(SELECT COUNT(Salary) FROM employee WHERE e.Salary<=Salary AND e.DepartmentId=DepartmentId) AS 'Rank'
FROM employee AS e;

SELECT department.`Name` AS Department,
d.`Name`,
d.Salary  
FROM dif_part_salary_rank AS d,department
WHERE department.Id=d.DepartmentId AND d.Rank<=3
```

项目三

```sql
SELECT ROUND(s2.Score,2) as Score,
(SELECT COUNT(s.Score)+1 FROM score as s  WHERE s.Score>s2.Score) as `Rank`
FROM score as s2 ORDER BY Score DESC;
```