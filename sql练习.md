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

[SQL3 查找当前薪水详情以及部门编号dept_no](https://www.nowcoder.com/practice/c63c5b54d86e4c6d880e4834bfd70c3b?tpId=82&rp=1&ru=%2Fexam%2Foj&qru=%2Fexam%2Foj&sourceUrl=%2Fexam%2Foj%3Ftab%3DSQL%25E7%25AF%2587%26topicId%3D82%26page%3D1&difficulty=&judgeStatus=&tags=&title=&gioEnter=menu)
```sql
select d.emp_no, s.salary, s.from_date, s.to_date, d.dept_no 
from dept_manager as d 
left join salaries as s
on d.emp_no = s.emp_no
where d.to_date = '9999-01-01' and s.to_date = '9999-01-01'
order by s.emp_no;
```

- 注意领导表里面可能还会存在**曾经的领导**，所以还需要限制to_date条件。

[SQL 4 查找所有已经分配部门的员工的last_name和first_name以及dept_no](https://www.nowcoder.com/practice/6d35b1cd593545ab985a68cd86f28671?tpId=82&rp=1&ru=%2Fexam%2Foj&qru=%2Fexam%2Foj&sourceUrl=%2Fexam%2Foj%3Ftab%3DSQL%25E7%25AF%2587%26topicId%3D82%26page%3D1&difficulty=&judgeStatus=&tags=&title=&gioEnter=menu)
```sql
select e.last_name, e.first_name, d.dept_no 
from dept_emp as d
left join employees as e
on d.emp_no = e.emp_no;
```
- 因为查询的是已经分配部门的员工，所以以**部门表作为左表**进行连接。

[SQL5 查找所有员工的last_name和first_name以及对应部门编号dept_no](https://www.nowcoder.com/practice/dbfafafb2ee2482aa390645abd4463bf?tpId=82&rp=1&ru=%2Fexam%2Foj&qru=%2Fexam%2Foj&sourceUrl=%2Fexam%2Foj%3Ftab%3DSQL%25E7%25AF%2587%26topicId%3D82%26page%3D1&difficulty=&judgeStatus=&tags=&title=&gioEnter=menu)
```sql
select e.last_name, e.first_name, d.dept_no
from employees as e 
left join dept_emp as d
on e.emp_no = d.emp_no;
```

- 与上题的区别在于**没有分配部门的员工也要输出来**，所以以员工表为左表连接。

[SQL7 查找薪水记录超过15次的员工号emp_no以及其对应的记录次数t](https://www.nowcoder.com/practice/6d4a4cff1d58495182f536c548fee1ae?tpId=82&rp=1&ru=%2Fexam%2Foj&qru=%2Fexam%2Foj&sourceUrl=%2Fexam%2Foj%3Ftab%3DSQL%25E7%25AF%2587%26topicId%3D82&difficulty=&judgeStatus=&tags=&title=&gioEnter=menu)
```Java
select emp_no, count(*) as t 
from salaries
group by emp_no
having t > 15;
```