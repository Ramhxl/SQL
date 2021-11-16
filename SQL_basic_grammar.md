### 1. SQL 语句可以分为以下三类.

- **DDL** ：DDL（Data Definition Language，数据定义语言） 用来创建或者删除存储数据用的数据库以及数据库中的表等对象。DDL 包含以下几种指令。

    - CREATE ： 创建数据库和表等对象
    - DROP ： 删除数据库和表等对象
    - ALTER ： 修改数据库和表等对象的结构

- **DML** :DML（Data Manipulation Language，数据操纵语言） 用来查询或者变更表中的记录。DML 包含以下几种指令。

    - SELECT ：查询表中的数据
    - INSERT ：向表中插入新数据
    - UPDATE ：更新表中的数据
    - DELETE ：删除表中的数据

- **DCL** ：DCL（Data Control Language，数据控制语言） 用来确认或者取消对数据库中的数据进行的变更。除此之外，还可以对 RDBMS 的用户是否有权限操作数据库中的对象（数据库表等）进行设定。DCL 包含以下几种指令。

    - COMMIT ： 确认对数据库中的数据进行的变更
    - ROLLBACK ： 取消对数据库中的数据进行的变更
    - GRANT ： 赋予用户操作权限
    - REVOKE ： 取消用户的操作权限
    
### 2. SQL 基本书写规则

* SQL语句要以分号（ ; ）结尾
* SQL 不区分关键字的大小写，但是插入到表中的数据是区分大小写的
* win 系统默认不区分表名及字段名的大小写
* linux / mac 默认严格区分表名及字段名的大小写
            * 本教程已统一调整表名及字段名的为小写，以方便初学者学习使用。
* 常数的书写方式是固定的

'abc', 1234, '26 Jan 2010', '10/01/26', '2010-01-26'......

* 单词需要用半角空格或者换行来分隔

### 3. 命名规则

* 只能使用半角英文字母、数字、下划线（_）作为数据库、表和列的名称
* 名称必须以半角英文字母开头

'''sql
CREATE DATABASE ch01;
USE ch01;

CREATE TABLE product(
	product_id 		CHAR(4) 		NOT NULL,
	product_name 	VARCHAR(100) 	NOT NULL,
	product_type 	VARCHAR(32) 	NOT NULL,
	sale_price 		INTEGER ,
	purchase_price 	INTEGER ,
	regist_date 	DATE ,
	PRIMARY KEY (product_id)
 );
 
-- -- 包含列清单
-- INSERT INTO productins (product_id, product_name, product_type, sale_price, purchase_price, regist_date) VALUES ('0005', '高压锅', '厨房用具', 6800, 5000, '2009-01-15');
-- -- 省略列清单
-- INSERT INTO productins VALUES ('0002', '打孔器', '办公用品', 500, 320, '2009-09-11'),
--                               ('0003', '运动T恤', '衣服', 4000, 2800, NULL),
--                               ('0004', '菜刀', '厨房用具', 3000, 2800, '2009-09-20');  
--                               
-- -- 将商品表中的数据复制到商品复制表中
-- create table productcopy
-- as SELECT product_id, product_name, product_type, sale_price, purchase_price, regist_date
-- FROM productins; 

INSERT INTO product VALUES('0001', 'T恤衫', '衣服', 1000, 500, '2009-09-20');
INSERT INTO product VALUES('0002', '打孔器', '办公用品', 500, 320, '2009-09-11');
INSERT INTO product VALUES('0003', '运动T恤', '衣服', 4000, 2800, NULL);
INSERT INTO product VALUES('0004', '菜刀', '厨房用具', 3000, 2800, '2009-09-20');
INSERT INTO product VALUES('0005', '高压锅', '厨房用具', 6800, 5000, '2009-01-15');
INSERT INTO product VALUES('0006', '叉子', '厨房用具', 500, NULL, '2009-09-20');
INSERT INTO product VALUES('0007', '擦菜板', '厨房用具', 880, 790, '2008-04-28');
INSERT INTO product VALUES('0008', '圆珠笔', '办公用品', 100, NULL, '2009-11-11');
COMMIT;

-- 编写一条 CREATE TABLE 语句，用来创建一个包含表 1-A 中所列各项的表 Addressbook （地址簿），
-- 并为 regist_no （注册编号）列设置主键约束
CREATE TABLE Addressbook (
	regist_no		INTEGER			NOT NULL,
	name			VARCHAR(128) 	NOT NULL,
	address			VARCHAR(256)	NOT NULL,
	tel_no			CHAR(10),
	mail_address 	CHAR(20),
	PRIMARY KEY (regist_no)
);

-- 添加列
-- 列名 ： postal_code
-- 数据类型 ：定长字符串类型（长度为 8）
-- 约束 ：不能为 NULL
ALTER TABLE Addressbook ADD COLUMN postal_code CHAR(8) NOT NULL;

-- 请补充如下 SQL 语句来删除 Addressbook 表
TRUNCATE TABLE Addressbook;
-- 或者 
DROP TABLE Addressbook;

-- 是否可以编写 SQL 语句来恢复删除掉的 Addressbook 表？
-- 答：无法恢复
'''
