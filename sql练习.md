# 牛客网
[SQL1 查找最晚入职员工的所有信息](https://www.nowcoder.com/practice/218ae58dfdcd4af195fff264e062138f?tpId=82&tqId=29753&rp=1&ru=/exam/oj&qru=/exam/oj&sourceUrl=%2Fexam%2Foj%3Ftab%3DSQL%25E7%25AF%2587%26topicId%3D82%26page%3D1&difficulty=undefined&judgeStatus=undefined&tags=&title=)

```sql
select * from employees 
    order by hire_date desc 
    limit 1;

/* 使用limit 与 offset关键字  */
select * from employees 
    order by hire_date desc 
    limit 1 offset 0;

/* 使用limit关键字 从第0条记录 向后读取一个，也就是第一条记录 */
select * from employees 
    order by hire_date desc 
    limit 0,1;

/* 使用子查询，最后一天的时间有多个员工信息 */
select * from employees
    where hire_date = (select max(hire_date) from employees);

```

[SQL2 查找入职员工时间排名倒数第三的员工所有信息](https://www.nowcoder.com/practice/ec1ca44c62c14ceb990c3c40def1ec6c?tpId=82&tqId=29754&rp=1&ru=/exam/oj&qru=/exam/oj&sourceUrl=%2Fexam%2Foj%3Ftab%3DSQL%25E7%25AF%2587%26topicId%3D82%26page%3D1&difficulty=undefined&judgeStatus=undefined&tags=&title=)
```sql
select * from employees
where hire_date = 
(select distinct hire_date from employees 
order by hire_date desc
limit 1 offset 2);


select * from employees
where hire_date = 
(select hire_date from employees
group by hire_date 
order by hire_date desc 
limit 1 offset 2);
```
- 考虑到入职时间可能有相同的，所以需要用 **distinct** 或者 **group by** 去重。

[]()
```sql
select d.emp_no, s.salary, s.from_date, s.to_date, d.dept_no 
from dept_manager as d 
left join salaries as s
on d.emp_no = s.emp_no
where d.to_date = '9999-01-01' and s.to_date = '9999-01-01'
order by s.emp_no;
```