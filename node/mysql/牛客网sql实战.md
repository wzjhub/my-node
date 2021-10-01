https://www.nowcoder.com/ta/sql

| 题号  | 题目                                                         | 难度 | 通过率 |
| :---- | :----------------------------------------------------------- | :--: | :----: |
| SQL1  | 查找最晚入职员工的所有信息                                   | 入门 | 45.13% |
| SQL2  | 查找入职员工时间排名倒数第三的员工所有信息                   | 简单 | 41.50% |
| SQL3  | 查找当前薪水详情以及部门编号dept_no                          | 中等 | 27.50% |
| SQL4  | 查找所有已经分配部门的员工的last_name和first_name以及dept_no | 简单 | 43.73% |
| SQL5  | 查找所有员工的last_name和first_name以及对应部门编号dept_no   | 中等 | 43.93% |
| SQL7  | 查找薪水记录超过15次的员工号emp_no以及其对应的记录次数t      | 简单 | 42.55% |
| SQL8  | 找出所有员工当前薪水salary情况                               | 简单 | 51.71% |
| SQL10 | 获取所有非manager的员工emp_no                                | 简单 | 42.07% |
| SQL11 | 获取所有员工当前的manager                                    | 中等 | 35.35% |
| SQL12 | 获取每个部门中当前员工薪水最高的相关信息                     | 困难 | 18.83% |
| SQL15 | 查找employees表emp_no与last_name的员工信息                   | 简单 | 48.45% |
| SQL16 | 统计出当前各个title类型对应的员工当前薪水对应的平均工资      | 中等 | 38.35% |
| SQL17 | 获取当前薪水第二多的员工的emp_no以及其对应的薪水salary       | 简单 | 44.34% |
| SQL18 | 获取当前薪水第二多的员工的emp_no以及其对应的薪水salary       | 较难 | 25.48% |
| SQL19 | 查找所有员工的last_name和first_name以及对应的dept_name       | 中等 | 33.59% |
| SQL21 | 查找在职员工自入职以来的薪水涨幅情况                         | 困难 | 20.99% |
| SQL22 | 统计各个部门的工资记录数                                     | 中等 | 30.47% |
| SQL23 | 对所有员工的薪水按照salary降序进行1-N的排名                  | 较难 | 28.77% |
| SQL24 | 获取所有非manager员工当前的薪水情况                          | 较难 | 28.98% |
| SQL25 | 获取员工其当前的薪水比其manager当前薪水还高的相关信息        | 困难 | 28.09% |

## SQL1  查找最晚入职员工的所有信息

```sql
select * from employees order by hire_date DESC limit 1
```

## SQL2  查找入职员工时间排名倒数第三的员工所有信息

```sql
select * from employees order by hire_date DESC limit 2, 1
```

## SQL3  查找当前薪水详情以及部门编号dept_no

```sql
SELECT s.*, dm.dept_no
FROM salaries s
	INNER JOIN dept_manager dm ON s.emp_no = dm.emp_no
```

## SQL4  查找所有已经分配部门的员工的last_name和first_name以及dept_no

```sql

```



## SQL5  查找所有员工的last_name和first_name以及对应部门编号dept_no

```sql

```



## SQL7  查找薪水记录超过15次的员工号emp_no以及其对应的记录次数t

```sql

```



##  SQL8  找出所有员工当前薪水salary情况

```sql

```



## SQL10  获取所有非manager的员工emp_no

```sql

```



## SQL11  获取所有员工当前的manager

```sql

```



## SQL12  获取每个部门中当前员工薪水最高的相关信息

```sql

```



## SQL15  查找employees表emp_no与last_name的员工信息

```sql

```



## SQL16  统计出当前各个title类型对应的员工当前薪水对应的平均工资

```sql

```



## SQL17  获取当前薪水第二多的员工的emp_no以及其对应的薪水salary

```sql

```



## SQL18  获取当前薪水第二多的员工的emp_no以及其对应的薪水salary

```sql

```



## SQL19  查找所有员工的last_name和first_name以及对应的dept_name

```sql

```



## SQL21  查找在职员工自入职以来的薪水涨幅情况

```sql

```



## SQL22  统计各个部门的工资记录数

```sql

```



##  SQL23  对所有员工的薪水按照salary降序进行1-N的排名

```sql

```



## SQL24  获取所有非manager员工当前的薪水情况

```sql

```



## SQL25  获取员工其当前的薪水比其manager当前薪水还高的相关信息

```sql

```

