实验一：分析SQL执行计划，执行SQL语句的优化指导

查询1：

SELECT d.department_name，count(e.job_id)as "部门总人数"，
avg(e.salary)as "平均工资"
from hr.departments d，hr.employees e
where d.department_id = e.department_id
and d.department_name in ('IT'，'Sales')
GROUP BY department_name;

运行结果：
![查询1](https://github.com/WangHanWei19971211/Oracle/blob/master/test1/01.png)
所用时间：
![时间1](https://github.com/WangHanWei19971211/Oracle/blob/master/test1/1_time.png)

查询2：
SELECT d.department_name，count(e.job_id)as "部门总人数"，
avg(e.salary)as "平均工资"
FROM hr.departments d，hr.employees e
WHERE d.department_id = e.department_id
GROUP BY department_name
HAVING d.department_name in ('IT'，'Sales');

运行结果：
![查询2](https://github.com/WangHanWei19971211/Oracle/blob/master/test1/02.png)
所用时间：
![时间2](https://github.com/WangHanWei19971211/Oracle/blob/master/test1/2_time.png)
