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

```sql
select emp_no, count(*) as t 
from salaries
group by emp_no
having t > 15;
```

[SQL8 找出所有员工当前薪水salary情况](nowcoder.com/practice/ae51e6d057c94f6d891735a48d1c2397?tpId=82&rp=1&ru=%2Fexam%2Foj&qru=%2Fexam%2Foj&sourceUrl=%2Fexam%2Foj%3Ftab%3DSQL%25E7%25AF%2587%26topicId%3D82&difficulty=&judgeStatus=&tags=&title=&gioEnter=menu)
```sql
select distinct salary from salaries
order by salary desc;
```

[SQL10 获取所有非manager的员工emp_no](https://www.nowcoder.com/practice/32c53d06443346f4a2f2ca733c19660c?tpId=82&rp=1&ru=%2Fexam%2Foj&qru=%2Fexam%2Foj&sourceUrl=%2Fexam%2Foj%3Ftab%3DSQL%25E7%25AF%2587%26topicId%3D82&difficulty=&judgeStatus=&tags=&title=&gioEnter=menu)

```sql
select emp_no from employees 
where emp_no not in 
(select emp_no from dept_manager);

select e.emp_no 
from employees as e
left join dept_manager as d
on e.emp_no = d.emp_no
where d.dept_no is null;
```

- 可以使用 not in 或者 left join。

[SQL11 获取所有员工当前的manager](https://www.nowcoder.com/practice/e50d92b8673a440ebdf3a517b5b37d62?tpId=82&rp=1&ru=%2Fexam%2Foj&qru=%2Fexam%2Foj&sourceUrl=%2Fexam%2Foj%3Ftab%3DSQL%25E7%25AF%2587%26topicId%3D82&difficulty=&judgeStatus=&tags=&title=&gioEnter=menu)

```sql
select d.emp_no, m.emp_no as manager
from dept_emp as d
left join dept_manager as m
on d.dept_no = m.dept_no
where d.emp_no <> manager and m.to_date = '9999-01-01';
```

- 注意：**以下的 sql 中为了可读性都省略了 to_date, 实际中 to_date 保证了当前员工没有离职，并且是最新的工资数据(因为可能存在升职加薪的情况)应该加上**。

[SQL12 获取每个部门中当前员工薪水最高的相关信息]()
- 错误写法
误区：使用GROUP BY子句后，select 语句中只能出现**group by语句中出现的字段，或者聚合函数，或者常数**，所以这里无法同时出现dept_no,emp_no.
(mysql语法松散，允许出现select语句中出现group by语句未出现的字段，但这样展示没有意义，因为记录并没有对应，默认取第一条，所以可能出现对应错误。
```sql
/*select d.dept_no, d.emp_no, max(s.salary) as maxSalary
from dept_emp as d
join salaries as s
on d.emp_no = s.emp_no
group by d.dept_no
order by d.dept_no
*/
```

```sql
select uni.dept_no, uni.emp_no, uni.salary
from 
(select d.dept_no, d.emp_no, s.salary 
from dept_emp as d
join salaries as s
on d.emp_no = s.emp_no) as uni /*部门编号 员工编号 薪水*/
join 
(select d.dept_no, max(s.salary) as maxSalary
from dept_emp as d
join salaries as s
on d.emp_no = s.emp_no
group by d.dept_no) as ms /*部门编号 最大薪水*/
on uni.dept_no = ms.dept_no and uni.salary = ms.maxSalary 
order by uni.dept_no;
```

[SQL15 查找employees表emp_no与last_name的员工信息](https://www.nowcoder.com/practice/a32669eb1d1740e785f105fa22741d5c?tpId=82&rp=1&ru=%2Fexam%2Foj&qru=%2Fexam%2Foj&sourceUrl=%2Fexam%2Foj%3Ftab%3DSQL%25E7%25AF%2587%26topicId%3D82&difficulty=&judgeStatus=&tags=&title=&gioEnter=menu)
```sql
select * from employees
where emp_no & 1 and last_name <> 'Mary'
order by hire_date desc;
```

- 本题要保留 emp_no 是**奇数**的，可以**使用 & 运算符号**。(或者使用 mod(emp_no, 2) = 1， 但是某些 sql 版本可能没有这个函数)

[SQL16 统计出当前各个title类型对应的员工当前薪水对应的平均工资](https://www.nowcoder.com/practice/c8652e9e5a354b879e2a244200f1eaae?tpId=82&rp=1&ru=%2Fexam%2Foj&qru=%2Fexam%2Foj&sourceUrl=%2Fexam%2Foj%3Ftab%3DSQL%25E7%25AF%2587%26topicId%3D82&difficulty=&judgeStatus=&tags=&title=&gioEnter=menu)

```sql
select t.title, avg(s.salary) 
from titles as t
join salaries as s
on t.emp_no = s.emp_no
group by t.title
order by avg(s.salary);
```

[SQL17 获取当前薪水第二多的员工的emp_no以及其对应的薪水salary](https://www.nowcoder.com/practice/8d2c290cc4e24403b98ca82ce45d04db?tpId=82&rp=1&ru=%2Fexam%2Foj&qru=%2Fexam%2Foj&sourceUrl=%2Fexam%2Foj%3Ftab%3DSQL%25E7%25AF%2587%26topicId%3D82&difficulty=&judgeStatus=&tags=&title=&gioEnter=menu)
```sql
select emp_no, salary from salaries 
where salary = 
(select distinct salary from salaries
 order by salary desc 
limit 1 offset 1 /*从 1 开始，取一个*/) 
order by emp_no;
```

[SQL18 获取当前薪水第二多的员工的emp_no以及其对应的薪水salary](https://www.nowcoder.com/practice/c1472daba75d4635b7f8540b837cc719?tpId=82&rp=1&ru=%2Fexam%2Foj&qru=%2Fexam%2Foj&sourceUrl=%2Fexam%2Foj%3Ftab%3DSQL%25E7%25AF%2587%26topicId%3D82&difficulty=&judgeStatus=&tags=&title=&gioEnter=menu)
```sql
select e.emp_no, s.salary, e.last_name, e.first_name
from employees as e
join salaries as s
on e.emp_no = s.emp_no
where salary = 
(select max(salary) from salaries
where salary < 
(select max(salary) from salaries))
```
- 本题目不允许使用 order by，还需要排名工资第二高的员工信息，所以可以先找最高的 salary，再找第二高的 salary。

[SQL19 查找所有员工的last_name和first_name以及对应的dept_name](https://www.nowcoder.com/practice/5a7975fabe1146329cee4f670c27ad55?tpId=82&rp=1&ru=%2Fexam%2Foj&qru=%2Fexam%2Foj&sourceUrl=%2Fexam%2Foj%3Ftab%3DSQL%25E7%25AF%2587%26topicId%3D82&difficulty=&judgeStatus=&tags=&title=&gioEnter=menu)

```sql
select e.last_name, e.first_name, d.dept_name
from employees as e
left join dept_emp as de
on e.emp_no = de.emp_no
left join departments as d
on de.dept_no = d.dept_no
```
- 三表连接。因为没有分配 dept 的员工也要出现，所以以 employees 作为主表进行左连接。

[SQL21 查找在职员工自入职以来的薪水涨幅情况](https://www.nowcoder.com/practice/fc7344ece7294b9e98401826b94c6ea5?tpId=82&rp=1&ru=%2Fexam%2Foj&qru=%2Fexam%2Foj&sourceUrl=%2Fexam%2Foj%3Ftab%3DSQL%25E7%25AF%2587%26topicId%3D82&difficulty=&judgeStatus=&tags=&title=&gioEnter=menu)

```sql
select pre.emp_no, (now.salary - pre.salary) as growth
from 
/*入职薪水*/
(select e.emp_no, s.salary
from employees as e
join salaries as s
on e.emp_no = s.emp_no 
and e.hire_date = s.from_date) as pre
join 
/*当前薪水*/
(select s.emp_no, s.salary
from salaries as s
where s.to_date = '9999-01-01') as now
on pre.emp_no = now.emp_no
order by growth;
```
- 获取入职薪水和当前薪水相减即可。

[SQL22 统计各个部门的工资记录数](https://www.nowcoder.com/practice/6a62b6c0a7324350a6d9959fa7c21db3?tpId=82&rp=1&ru=%2Fexam%2Foj&qru=%2Fexam%2Foj&sourceUrl=%2Fexam%2Foj%3Ftab%3DSQL%25E7%25AF%2587%26topicId%3D82&difficulty=&judgeStatus=&tags=&title=&gioEnter=menu)

```sql
select d.dept_no, d.dept_name, count(*) as sum
from departments as d
join dept_emp as de
on d.dept_no = de.dept_no
join salaries as s
on de.emp_no = s.emp_no
group by d.dept_no, d.dept_name
order by d.dept_no
```

[SQL23 对所有员工的薪水按照salary降序进行1-N的排名](https://www.nowcoder.com/practice/b9068bfe5df74276bd015b9729eec4bf?tpId=82&rp=1&ru=%2Fexam%2Foj&qru=%2Fexam%2Foj&sourceUrl=%2Fexam%2Foj%3Ftab%3DSQL%25E7%25AF%2587%26topicId%3D82&difficulty=&judgeStatus=&tags=&title=&gioEnter=menu)

```sql
select emp_no, salary, dense_rank() over(order by salary desc) as rank
from salaries
order by rank, emp_no
```
- RANK()
    - 在计算排序时，若**存在相同位次，会跳过之后的位次**。
    - 例如，有3条排在第1位时，排序为：1，1，1，4······

- DENSE_RANK()
    - 这就是题目中所用到的函数，在计算排序时，若**存在相同位次，不会跳过之后的位次**。
    - 例如，有3条排在第1位时，排序为：1，1，1，2······

- ROW_NUMBER()
    - 这个函数赋予**唯一的连续位次**。
    - 例如，有3条排在第1位时，排序为：1，2，3，4······

- 窗口函数用法：
<窗口函数> OVER ( [PARTITION BY <列清单> ]
                                ORDER BY <排序用列清单> ）
*其中[ ]中的内容可以忽略

[SQL24 获取所有非manager员工当前的薪水情况](https://www.nowcoder.com/practice/8fe212a6c71b42de9c15c56ce354bebe?tpId=82&rp=1&ru=%2Fexam%2Foj&qru=%2Fexam%2Foj&sourceUrl=%2Fexam%2Foj%3Ftab%3DSQL%25E7%25AF%2587%26topicId%3D82&difficulty=&judgeStatus=&tags=&title=&gioEnter=menu)

```sql
select de.dept_no, e.emp_no, s.salary
from employees as e
join dept_emp as de
on e.emp_no = de.emp_no
join salaries as s
on s.emp_no = e.emp_no
where e.emp_no not in (select emp_no from dept_manager)

select de.dept_no, e.emp_no, s.salary
from employees as e
join dept_emp as de
on e.emp_no = de.emp_no
join salaries as s
on s.emp_no = e.emp_no
left join dept_manager as dm
on e.emp_no = dm.emp_no
where dm.emp_no is null
```

- 第一种写法是正常的使用 not in 去掉 manager 的 emp_no。第二种可以使用左连接，由于 manager 表只有 manager 的 emp_no, 所以在连接的过程中会有 null 的情况，为 null 的就是所有不是 manager 的员工。

[SQL25 获取员工其当前的薪水比其manager当前薪水还高的相关信息](https://www.nowcoder.com/practice/f858d74a030e48da8e0f69e21be63bef?tpId=82&rp=1&ru=%2Fexam%2Foj&qru=%2Fexam%2Foj&sourceUrl=%2Fexam%2Foj%3Ftab%3DSQL%25E7%25AF%2587%26topicId%3D82&difficulty=&judgeStatus=&tags=&title=&gioEnter=menu)
```sql
select ss.emp_no, m.emp_no as manager_no,
ss.salary as emp_salary, m.salary as manager_salary
from
/*员工 的工资*/
(select s.emp_no, s.salary, de.dept_no
from salaries as s
join dept_emp as de
on s.emp_no = de.emp_no) as ss
join 
/*manager 的工资*/
(select dm.dept_no, dm.emp_no, s.salary
from dept_manager as dm
join salaries as s
on dm.emp_no = s.emp_no) as m
on ss.dept_no = m.dept_no
where emp_salary > manager_salary;
```
- 步骤分开，分别击破





