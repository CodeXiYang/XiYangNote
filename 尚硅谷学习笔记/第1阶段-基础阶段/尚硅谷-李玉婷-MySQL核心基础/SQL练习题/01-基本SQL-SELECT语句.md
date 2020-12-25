# 案例-基本SQL-select语句

1. 下面的语句是否可以执行成功

  ```sql
  select last_name , job_id , salary as sal
  from employees; -- 答案: 可以执行
  ```

  

2. 下面的语句是否可以执行成功 

  ```sql
  select * from employees; -- 答案: 可以执行
  ```

  

3. 找出下面语句中的错误

  ```sql
  select employee_id , last_name，
  salary * 12 “ANNUAL SALARY”
  from employees;
  -- 答案: 符号错误 ,注意中英文状态的符号就可以了
  ```

  

4. 显示表 departments 的结构，并查询其中的全部数据

   ```sql
   -- 答案:
   DESC departments; -- 显示表的结构
   SELECT * FROM departments; -- 查询全部数据
   ```

   

5. 显示出表 employees 中的全部 job_id（不能重复）

   ```sql
   -- 答案:
   SELECT DISTINCT job_id FROM employees;
   ```

   

6. 显示出表 employees 的全部列，各个列之间用逗号连接，列头显示成 OUT_PUT

   ```sql
   -- 答案
   SELECT CONCAT(last_name,",",first_name,",",job_id) AS "OUT PUT" FROM employees;
   ```

   

