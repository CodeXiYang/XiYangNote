# DQL查询语句

## 1. DQL查询语句

**DQL（Data Query Language 数据查询语言）**

- 查询数据库数据，如SELECT语句
- 简单的单表查询或多表的复杂查询和嵌套查询
- 是数据库语言中最核心，最重要的语句
- 使用频率最高的语句

## 2. 基础查询

### 2.1 *通配符查询

**语法:** `SELECT * FROM 表名;`

`*` 用于查询表的所有列

```sql
-- 查询所有学生
SELECT * FROM student;
-- 查询所有成绩
SELECT * FROM result;
```

### 2.2 指定字段查询

**语法:** `SELECT 字段1,[字段2,字段3]... FROM 表名;`

```sql
-- 查询所有学生表的姓名
SELECT studentname FROM student;
-- 查询所有学生表的姓名和性别
SELECT studentname,sex FROM student;
```

### 2.3 AS起别名

由于查询出来的结果集的列是数据库中的列,不利于查看,可以通过`AS`	取一个别名;

as不仅仅可以给字段取别名,也可以给表取别名

**语法:** `SELECT 字段名 [AS] '别名' FROM 表名 AS 表别名;`

```sql
-- 查询所有学生表的姓名和性别
SELECT studentname AS '姓名',sex AS '性别' FROM student;
-- AS可以省略不写
SELECT studentname '姓名',sex '性别' FROM student;
-- 查询所有学生表的姓名和性别,给表取别名s
SELECT studentname AS '姓名',sex AS '性别' FROM student AS s;
```

**注意点:**

可以搭配mysql中的一些函数使用,例如concat

```sql
-- 函数concat(a,b) 用于拼接a和b
SELECT CONCAT("姓名: ",studentname) AS '姓名列' FROM student AS s;
```

### 2.4 DISTINCT去重

去掉SELECT查询返回的记录结果中重复的记录 ( 返回所有列的值都相同 ) , 只返回一条

**语法:** `SELECT DISTINCT [* || 字段] FROM 表名;`

```sql
-- # 查看哪些同学参加了考试(学号) 去除重复项 
SELECT * FROM result; 
-- 查看考试成绩 
SELECT studentno FROM result; 
-- 查看哪些同学参加了考试 
SELECT DISTINCT studentno FROM result; 
-- 了解:DISTINCT 去除重复项 , (默认是ALL)
```

### 2.5 使用表达式的列

<span style="color:red">数据库中的表达式 : 一般由文本值 , 列值 , NULL , 函数和操作符等组成</span>

**语法:** `SELECT 表达式 FROM 表名;`

**应用场景 :** 

1. SELECT语句返回结果列中使用

2. SELECT语句中的ORDER BY , HAVING等子句中使用

3. DML语句中的 where 条件语句中使用表达式

   ```sql
   -- 查看mysql系统版本
   SELECT version();
   -- 查询自己自增步长
   SELECT @@auto_increment_increment;
   -- 计算结果
   SELECT 100*3-1 AS 计算结果;
   -- 学员考试成绩集体提分一分查看
   SELECT studentno,StudentResult+1 AS '提分后' FROM result;
   ```

**注意:** 

避免SQL返回结果中包含 ' . ' , ' * ' 和括号等干扰开发语言程序. 

## 3. where条件语句

**where作用:** 用于检索数据表中 符合条件 的记录

搜索条件可由一个或多个逻辑表达式组成 , 结果一般为真或假.

where可以搭配不同类型的操作符一起使用

### 3.1 逻辑操作符

| 操作符名称 | 语法                | 描述                               |
| ---------- | ------------------- | ---------------------------------- |
| AND 或 &&  | `a AND b 或 a && b` | 逻辑与，同时为真结果才为真         |
| OR 或 \|\| | `a OR b 或 a`       | 逻辑或，只要一个为真，则结果为真   |
| NOT 或 ！  | `NOT a 或 ！a`      | 逻辑非，若操作数为假，则结果为真！ |

```sql
-- 满足条件的查询(where) 
SELECT Studentno,StudentResult FROM result; 
-- 查询考试成绩在95-100之间的 
SELECT Studentno,StudentResult FROM result WHERE StudentResult>=95 AND StudentResult<=100; 
-- AND也可以写成 && 
SELECT Studentno,StudentResult FROM result WHERE StudentResult>=95 && StudentResult<=100; 
-- 模糊查询(对应的词:精确查询)
SELECT Studentno,StudentResult FROM result WHERE StudentResult BETWEEN 95 AND 100; 
-- 除了1001号同学,要其他同学的成绩 
SELECT studentno,studentresult FROM result WHERE studentno!=1001; 
-- 使用NOT 
SELECT studentno,studentresult FROM result WHERE NOT studentno=1001;
```



### 3.2  模糊查询

<span style="color:red">模糊查询的本质其实就是比较操作符</span>

| 操作符名称  | 语法                        | 描述                                        |
| ----------- | --------------------------- | ------------------------------------------- |
| IS NULL     | `a IS NULL`                 | 若操作符为NULL，则结果为真                  |
| IS NOT NULL | `a IS NOT NULL`             | 若操作符不为NULL，则结果为真                |
| BETWEEN     | `a BETWEEN b AND c`         | 若 a 范围在 b 与 c 之间，则结果为真         |
| LIKE        | `a LIKE b`                  | SQL 模式匹配，若a匹配b，则结果为真          |
| IN          | `a IN (a1，a2，a3，......)` | 若 a 等于 a1,a2..... 中的某一个，则结果为真 |



**注意点:**

1. 数值数据类型的记录之间才能进行算术运算 ;
2. 相同数据类型的数据之间才能进行比较 ;

**LIKE查询:** `_` `%`

```sql
-- 模糊查询 between and \ like \ in \ null 
-- ============================================= 
-- LIKE 
-- ============================================= 
-- 查询姓刘的同学的学号及姓名 
-- like结合使用的通配符 : % (代表0到任意个字符) _ (一个字符) 
SELECT studentno,studentname FROM student WHERE studentname LIKE '刘%'; 
-- 查询姓刘的同学,后面只有一个字的 
SELECT studentno,studentname FROM student WHERE studentname LIKE '刘_'; 
-- 查询姓刘的同学,后面只有两个字的 
SELECT studentno,studentname FROM student WHERE studentname LIKE '刘__'; 
-- 查询姓名中含有 嘉 字的
SELECT studentno,studentname FROM student WHERE studentname LIKE '%嘉%'; 
-- 查询姓名中含有特殊字符的需要使用转义符号 '\' 
-- 自定义转义符关键字: ESCAPE ':' 
```

**IN查询:** 什么什么里面

```sql
-- ============================================= 
-- IN 
-- ============================================= 
-- 查询学号为1000,1001,1002的学生姓名 
SELECT studentno,studentname FROM student WHERE studentno IN (1000,1001,1002); 
-- 查询地址在北京,南京,河南洛阳的学生 
SELECT studentno,studentname,address FROM student WHERE address IN ('北京','南京','河南洛阳'); 
```

**NULL查询:** 是否为空

```sql
-- ============================================= 
-- NULL 空 
-- ============================================= 
-- 查询出生日期没有填写的同学 
-- 不能直接写=NULL , 这是代表错误的 , 用 is null 
SELECT studentname FROM student WHERE BornDate IS NULL; 
-- 查询出生日期填写的同学 
SELECT studentname FROM student WHERE BornDate IS NOT NULL; 
-- 查询没有写家庭住址的同学(空字符串不等于null) 
SELECT studentname FROM student WHERE Address='' OR Address IS NULL;
```



## 4. 连接查询

**连接查询的关键字是join,连接查询分为 内连接,左连接,右连接**

### 4.1 三种连接对比

| 操作符名称 | 描述                                                  |
| ---------- | ----------------------------------------------------- |
| INNER JOIN | 如果表中有至少一个匹配，则返回行 **内连接**           |
| LEFT JOIN  | 即使右表中没有匹配，也从左表中返回所有的行 **左连接** |
| RIGHT JOIN | 即使左表中没有匹配，也从右表中返回所有的行 **右连接** |

![image-20201028170657351](assets/image-20201028170657351.png)

```sql
/*
连接查询如需要多张数据表的数据进行查询,则可通过连接运算符实现多个查询 
内连接 inner join 查询两个表中的结果集中的交集 
外连接 outer join 
	左外连接 left join (以左表作为基准,右边表来一一匹配,匹配不上的,返回左表的记录,右表以NULL填充) 
	右外连接 right join (以右表作为基准,左边表来一一匹配,匹配不上的,返回右表的记录,左表以NULL填充) 
等值连接和非等值连接 
自连接 
*/
-- 查询参加了考试的同学信息(学号,学生姓名,科目编号,分数) 
SELECT * FROM student; -- 同学信息
SELECT * FROM result; -- 参加考试说明有成绩的同学
/*
思路: 
	(1):分析需求,确定查询的列来源于两个类,student result,连接查询 
	(2):确定使用哪种连接查询?(内连接) 
*/
SELECT s.studentno,studentname,subjectno,StudentResult FROM student s INNER JOIN result r ON r.studentno = s.studentno
-- 右连接(也可实现) 
SELECT s.studentno,studentname,subjectno,StudentResult FROM student s RIGHT JOIN result r ON r.studentno = s.studentno 
-- 等值连接 
SELECT s.studentno,studentname,subjectno,StudentResult FROM student s , result r WHERE r.studentno = s.studentno 
-- 左连接 (查询了所有同学,不考试的也会查出来) 
SELECT s.studentno,studentname,subjectno,StudentResult FROM student s LEFT JOIN result r ON r.studentno = s.studentno 
-- 查一下缺考的同学(左连接应用场景) 
SELECT s.studentno,studentname,subjectno,StudentResult FROM student s LEFT JOIN result r ON r.studentno = s.studentno WHERE StudentResult IS NULL 
-- 思考题:查询参加了考试的同学信息(学号,学生姓名,科目名,分数) 
SELECT s.studentno,studentname,subjectname,StudentResult FROM student s INNER JOIN result r ON r.studentno = s.studentno INNER JOIN `subject` sub ON sub.subjectno = r.subjectno
```

**面试点:** where和on的区别

当我们使用连接操作，关联两张或多张表来返回记录时，数据库都会生成一张临时表，最后将这张临时表返回给用户。以 LEFT JOIN 为例：在使用 LEFT JOIN 时，ON 和 WHERE 过滤条件的区别如下：

- <span style="color:red">ON 条件是在生成临时表时使用的条件</span>，它不管 ON 中的条件是否为真，都会返回左边表中的记录；
- WHERE 条件是在临时表已经生成后，对<span style="color:red">临时表进行的过滤条件</span>。因为此时已经没有 LEFT JOIN 的含义（必须返回左侧表的记录）了，所以如果 WHERE 条件不为真的记录就会被过滤掉。

参考博客: https://blog.csdn.net/liitdar/article/details/80817957



### 4.2 自连接

自己的表和自己的表连接,其核心就是: 一张表拆成两张一样的表即可

```sql
-- 需求:从一个包含栏目ID , 栏目名称和父栏目ID的表中 查询父栏目名称和其他子栏目名称 
-- 编写SQL语句,将栏目的父子关系呈现出来 (父栏目名称,子栏目名称) 
-- 核心思想:把一张表看成两张一模一样的表,然后将这两张表连接查询(自连接) 
SELECT a.categoryName AS '父栏目',b.categoryName AS '子栏目' FROM category AS a,category AS b WHERE a.`categoryid`=b.`pid` 

-- 思考题:查询参加了考试的同学信息(学号,学生姓名,科目名,分数) 
SELECT s.studentno,studentname,subjectname,StudentResult FROM student s INNER JOIN result r ON r.studentno = s.studentno INNER JOIN `subject` sub ON sub.subjectno = r.subjectno 

-- 查询学员及所属的年级(学号,学生姓名,年级名) 
SELECT studentno AS 学号,studentname AS 学生姓名,gradename AS 年级名称 FROM student s INNER JOIN grade g ON s.`GradeId` = g.`GradeID`

-- 查询科目及所属的年级(科目名称,年级名称) 
SELECT subjectname AS 科目名称,gradename AS 年级名称 FROM SUBJECT sub INNER JOIN grade g ON sub.gradeid = g.gradeid 

-- 查询 数据库结构-1 的所有考试结果(学号 学生姓名 科目名称 成绩) 
SELECT s.studentno,studentname,subjectname,StudentResult FROM student s INNER JOIN result r ON r.studentno = s.studentno INNER JOIN `subject` sub ON r.subjectno = sub.subjectno WHERE subjectname='数据库结构-1'
```



### 4.3 扩展七种JOIN理论

![image-20201028170425077](assets/image-20201028170425077.png)

## 排序和分页



## 子查询



## SELECT完整语法



## 附录: sql参考文件

```sql
/*
 Navicat Premium Data Transfer

 Source Server         : mysql5.7
 Source Server Type    : MySQL
 Source Server Version : 50719
 Source Host           : localhost:3306
 Source Schema         : school

 Target Server Type    : MySQL
 Target Server Version : 50719
 File Encoding         : 65001

 Date: 28/10/2020 17:39:33
*/

SET NAMES utf8mb4;
SET FOREIGN_KEY_CHECKS = 0;

-- ----------------------------
-- Table structure for category
-- ----------------------------
DROP TABLE IF EXISTS `category`;
CREATE TABLE `category`  (
  `categoryid` int(10) UNSIGNED NOT NULL AUTO_INCREMENT COMMENT '主题id',
  `pid` int(10) NOT NULL COMMENT '父id',
  `categoryName` varchar(50) CHARACTER SET utf8 COLLATE utf8_general_ci NOT NULL COMMENT '主题名字',
  PRIMARY KEY (`categoryid`) USING BTREE
) ENGINE = InnoDB AUTO_INCREMENT = 9 CHARACTER SET = utf8 COLLATE = utf8_general_ci ROW_FORMAT = Dynamic;

-- ----------------------------
-- Records of category
-- ----------------------------
INSERT INTO `category` VALUES (2, 1, '信息技术');
INSERT INTO `category` VALUES (3, 1, '软件开发');
INSERT INTO `category` VALUES (4, 3, '数据库');
INSERT INTO `category` VALUES (5, 1, '美术设计');
INSERT INTO `category` VALUES (6, 3, 'web开发');
INSERT INTO `category` VALUES (7, 5, 'ps技术');
INSERT INTO `category` VALUES (8, 2, '办公信息');

-- ----------------------------
-- Table structure for grade
-- ----------------------------
DROP TABLE IF EXISTS `grade`;
CREATE TABLE `grade`  (
  `gradeid` int(11) NOT NULL AUTO_INCREMENT COMMENT '年级编号',
  `gradename` varchar(50) CHARACTER SET utf8 COLLATE utf8_general_ci NOT NULL COMMENT '年级名称',
  PRIMARY KEY (`gradeid`) USING BTREE
) ENGINE = InnoDB AUTO_INCREMENT = 6 CHARACTER SET = utf8 COLLATE = utf8_general_ci ROW_FORMAT = Dynamic;

-- ----------------------------
-- Records of grade
-- ----------------------------
INSERT INTO `grade` VALUES (1, '大一');
INSERT INTO `grade` VALUES (2, '大二');
INSERT INTO `grade` VALUES (3, '大三');
INSERT INTO `grade` VALUES (4, '大四');
INSERT INTO `grade` VALUES (5, '预科班');

-- ----------------------------
-- Table structure for result
-- ----------------------------
DROP TABLE IF EXISTS `result`;
CREATE TABLE `result`  (
  `studentno` int(4) NOT NULL COMMENT '学号',
  `subjectno` int(4) NOT NULL COMMENT '课程编号',
  `examdate` datetime(0) NOT NULL COMMENT '考试日期',
  `studentresult` int(4) NOT NULL COMMENT '考试成绩',
  INDEX `subjectno`(`subjectno`) USING BTREE
) ENGINE = InnoDB CHARACTER SET = utf8 COLLATE = utf8_general_ci ROW_FORMAT = Dynamic;

-- ----------------------------
-- Records of result
-- ----------------------------
INSERT INTO `result` VALUES (1000, 1, '2013-11-11 16:00:00', 85);
INSERT INTO `result` VALUES (1000, 2, '2013-11-12 16:00:00', 70);
INSERT INTO `result` VALUES (1000, 3, '2013-11-11 09:00:00', 68);
INSERT INTO `result` VALUES (1000, 4, '2013-11-13 16:00:00', 98);
INSERT INTO `result` VALUES (1000, 5, '2013-11-14 16:00:00', 58);

-- ----------------------------
-- Table structure for student
-- ----------------------------
DROP TABLE IF EXISTS `student`;
CREATE TABLE `student`  (
  `studentno` int(4) NOT NULL COMMENT '学号',
  `loginpwd` varchar(20) CHARACTER SET utf8 COLLATE utf8_general_ci NULL DEFAULT NULL COMMENT '登录密码',
  `studentname` varchar(20) CHARACTER SET utf8 COLLATE utf8_general_ci NULL DEFAULT NULL COMMENT '学生姓名',
  `sex` tinyint(1) NULL DEFAULT NULL COMMENT '性别，0或1',
  `gradeid` int(11) NULL DEFAULT NULL COMMENT '年级编号',
  `phone` varchar(50) CHARACTER SET utf8 COLLATE utf8_general_ci NOT NULL COMMENT '联系电话，允许为空',
  `address` varchar(255) CHARACTER SET utf8 COLLATE utf8_general_ci NOT NULL COMMENT '地址，允许为空',
  `borndate` datetime(0) NULL DEFAULT NULL COMMENT '出生时间',
  `email` varchar(50) CHARACTER SET utf8 COLLATE utf8_general_ci NOT NULL COMMENT '邮箱账号允许为空',
  `identitycard` varchar(18) CHARACTER SET utf8 COLLATE utf8_general_ci NULL DEFAULT NULL COMMENT '身份证号',
  PRIMARY KEY (`studentno`) USING BTREE,
  UNIQUE INDEX `identitycard`(`identitycard`) USING BTREE,
  INDEX `email`(`email`) USING BTREE
) ENGINE = MyISAM CHARACTER SET = utf8 COLLATE = utf8_general_ci ROW_FORMAT = Dynamic;

-- ----------------------------
-- Records of student
-- ----------------------------
INSERT INTO `student` VALUES (1000, '123456', '张伟', 0, 2, '13800001234', '北京朝阳', '1980-01-01 00:00:00', 'text123@qq.com', '123456198001011234');
INSERT INTO `student` VALUES (1001, '123456', '赵强', 1, 3, '13800002222', '广东深圳', '1990-01-01 00:00:00', 'text111@qq.com', '123456199001011233');
INSERT INTO `student` VALUES (1002, '123456', '刘邦', 1, 1, '13132323232', '重庆观音桥', '2020-10-15 16:30:46', 'text@163com', '123456199001011235');
INSERT INTO `student` VALUES (1003, '123456', '刘备', 0, 4, '13232423546', '', '2020-10-16 16:32:11', 'text@110.cm', '123456199001011236');
INSERT INTO `student` VALUES (1004, '123456', '刘一手', 1, 3, '13232145345', '南京', '2020-10-15 16:34:01', 'text@1224qq.com', '123456199001011232');
INSERT INTO `student` VALUES (1005, '123456', '刘嘉玲', NULL, 5, '12123423525', '上海', '2020-10-22 16:38:01', 'text#123qq.com', '123456199001032321');

-- ----------------------------
-- Table structure for subject
-- ----------------------------
DROP TABLE IF EXISTS `subject`;
CREATE TABLE `subject`  (
  `subjectno` int(11) NOT NULL AUTO_INCREMENT COMMENT '课程编号',
  `subjectname` varchar(50) CHARACTER SET utf8 COLLATE utf8_general_ci NULL DEFAULT NULL COMMENT '课程名称',
  `classhour` int(4) NULL DEFAULT NULL COMMENT '学时',
  `gradeid` int(4) NULL DEFAULT NULL COMMENT '年级编号',
  PRIMARY KEY (`subjectno`) USING BTREE
) ENGINE = InnoDB AUTO_INCREMENT = 19 CHARACTER SET = utf8 COLLATE = utf8_general_ci ROW_FORMAT = Dynamic;

-- ----------------------------
-- Records of subject
-- ----------------------------
INSERT INTO `subject` VALUES (1, '高等数学-1', 110, 1);
INSERT INTO `subject` VALUES (2, '高等数学-2', 110, 2);
INSERT INTO `subject` VALUES (3, '高等数学-3', 100, 3);
INSERT INTO `subject` VALUES (4, '高等数学-4', 130, 4);
INSERT INTO `subject` VALUES (5, 'C语言-1', 110, 1);
INSERT INTO `subject` VALUES (6, 'C语言-2', 110, 2);
INSERT INTO `subject` VALUES (7, 'C语言-3', 100, 3);
INSERT INTO `subject` VALUES (8, 'C语言-4', 130, 4);
INSERT INTO `subject` VALUES (9, 'Java程序设计-1', 110, 1);
INSERT INTO `subject` VALUES (10, 'Java程序设计-2', 110, 2);
INSERT INTO `subject` VALUES (11, 'Java程序设计-3', 100, 3);
INSERT INTO `subject` VALUES (12, 'Java程序设计-4', 130, 4);
INSERT INTO `subject` VALUES (13, '数据库结构-1', 110, 1);
INSERT INTO `subject` VALUES (14, '数据库结构-2', 110, 2);
INSERT INTO `subject` VALUES (15, '数据库结构-3', 100, 3);
INSERT INTO `subject` VALUES (16, '数据库结构-4', 130, 4);
INSERT INTO `subject` VALUES (17, 'C#基础', 130, 1);

SET FOREIGN_KEY_CHECKS = 1;
```





