### UPDATE VIEWS遇到的问题

当执行以下命令更新视图的时候，遇到了Error Code: 1175. You are using safe update mode and you tried to update a table without a WHERE that uses a KEY column. To disable safe mode, toggle the option in Preferences -> SQL Editor and reconnect.
```sql
-- 更新视图
UPDATE productsum
  SET sale_price = '5000'
  WHERE product_type = '办公用品';
```
原因是MySql正处于一种安全的update模式, This means that you can't update or delete records without specifying a key (ex. primary key) in the where clause.

- **Solution1** (NOT SO SAFE)
  - 设置命令改变update模式
```sql
SET SQL_SAFE_UPDATES = 0;
--  update之后记得设置回
SET SQL_SAFE_UPDATES = 1;
```
  
- **Solution2** (有点小麻烦)
  - Go to Edit --> Preferences
  - Click "SQL Editor" tab and uncheck "Safe Updates" check box
  - Query --> Reconnect to Server // logout and then login
  - Now execute your SQL query
  
- **Solution3** (SAFT SOLUTION!!!)
  - 直接modify your query to follow the rule (use primary key in where clause).
  - Just add in the WHERE clause a KEY-value that matches everything like a primary-key comparing to 0, 注意与primary-key相比的内容要符合key的type
```sql
UPDATE productsum
  SET sale_price = '5000'
  WHERE (product_type = '办公用品' AND product_id <> '0'); -- Because product_id is a primary key
```
成功update

![image](https://github.com/Ramhxl/SQL/blob/main/images/ch03_update.png)

### 关联子查询
**SELECT的执行顺序**
FROM 》WHERE》GROUP BY》HAVE》SELECT》ORDER BY
所以在以下代码中，即使"as p1"在"p1.product_type = p2.product type"书写的后面，依然可以成功执行，因为优先编译FROM语句
```sql
CREATE VIEW AvgPriceByType
AS SELECT product_id, product_name, product_type, sale_price, (
			SELECT AVG (sale_price) FROM product AS p2 WHERE p1.product_type = p2.product_type GROUP BY (product_type))AS avg_sale_price
FROM product AS p1; -- p1 的声明在最后
```
另外，WHERE p1.product_type = p2.product_type保证了标量输出，否则如果只有GROUP BY语句的话，输出为三个数字

### code and answers
```sql
USE Datawhale;

-- 在product表的基础上创建一个视图
CREATE VIEW productsum (product_type, cnt_product)
AS
SELECT product_type, COUNT(*)
  FROM product
  GROUP BY product_type ;

-- --------创建新的table ------------------------------
CREATE TABLE shop_product
(shop_id    CHAR(4)       NOT NULL,
 shop_name  VARCHAR(200)  NOT NULL,
 product_id CHAR(4)       NOT NULL,
 quantity   INTEGER       NOT NULL,
 PRIMARY KEY (shop_id, product_id));

INSERT INTO shop_product (shop_id, shop_name, product_id, quantity) VALUES ('000A',	'东京',		'0001',	30);
INSERT INTO shop_product (shop_id, shop_name, product_id, quantity) VALUES ('000A',	'东京',		'0002',	50);
INSERT INTO shop_product (shop_id, shop_name, product_id, quantity) VALUES ('000A',	'东京',		'0003',	15);
INSERT INTO shop_product (shop_id, shop_name, product_id, quantity) VALUES ('000B',	'名古屋',	'0002',	30);
INSERT INTO shop_product (shop_id, shop_name, product_id, quantity) VALUES ('000B',	'名古屋',	'0003',	120);
INSERT INTO shop_product (shop_id, shop_name, product_id, quantity) VALUES ('000B',	'名古屋',	'0004',	20);
INSERT INTO shop_product (shop_id, shop_name, product_id, quantity) VALUES ('000B',	'名古屋',	'0006',	10);
INSERT INTO shop_product (shop_id, shop_name, product_id, quantity) VALUES ('000B',	'名古屋',	'0007',	40);
INSERT INTO shop_product (shop_id, shop_name, product_id, quantity) VALUES ('000C',	'大阪',		'0003',	20);
INSERT INTO shop_product (shop_id, shop_name, product_id, quantity) VALUES ('000C',	'大阪',		'0004',	50);
INSERT INTO shop_product (shop_id, shop_name, product_id, quantity) VALUES ('000C',	'大阪',		'0006',	90);
INSERT INTO shop_product (shop_id, shop_name, product_id, quantity) VALUES ('000C',	'大阪',		'0007',	70);
INSERT INTO shop_product (shop_id, shop_name, product_id, quantity) VALUES ('000D',	'福冈',		'0001',	100);

-- -------- 视图 -------------------------------------------- 
-- 创建新视图
CREATE VIEW view_shop_product(product_type, sale_price, shop_name)
AS
SELECT product_type, sale_price, shop_name
  FROM product, shop_product
  WHERE product.product_id = shop_product.product_id;
  
-- 在该视图上继续查询
SELECT sale_price, shop_name
  FROM view_shop_product
  WHERE product_type = '衣服';

-- 修改视图
ALTER VIEW productSum
AS
SELECT product_type, sale_price, product_id
  FROM Product
  WHERE regist_date > '2009-09-11';
  
-- 更新视图
UPDATE productsum
  SET sale_price = '5000'
  WHERE (product_type = '办公用品' AND product_id <> '0');

SELECT * FROM product;

-- 删除视图
DROP VIEW productSum;

-- --------- 子查询 -------------------------------------- 
SELECT stu_name
FROM (
  SELECT stu_name, COUNT(*) AS stu_cnt
  FROM students_info
  GROUP BY stu_age) 
AS studentSum;

SELECT product_type, cnt_product
FROM (SELECT *
	FROM (SELECT product_type, COUNT(*) AS cnt_product
		FROM product 
		GROUP BY product_type) 
	AS productsum
	WHERE cnt_product = 4) 
AS productsum2;

-- 标量子查询（只返回一个值）

SELECT product_id, product_name, sale_price
FROM product
WHERE sale_price > (SELECT AVG(sale_price) FROM product);
 
SELECT product_id, product_name, sale_price, 
		(SELECT AVG(sale_price) FROM product) AS avg_price
FROM product;

-- 关联子查询（建立查询与子查询之间的关系）

-- 提取各个product type中大于该种类平均销售价格的商品
SELECT product_type, product_name, sale_price 
FROM product AS p1
WHERE sale_price > (SELECT AVG(sale_price) 
					FROM product AS p2 
                    WHERE p1.product_type = p2.product_type
GROUP BY product_type);

-- --------第一部分练习 ---------------------------------------------

CREATE VIEW ViewPractice5_1
AS SELECT product_name, sale_price, regist_date
FROM product
WHERE (sale_price >= 1000 AND regist_date = '2009-09-20');

SELECT * FROM ViewPractice5_1;

INSERT INTO ViewPractice5_1 VALUES (' 刀子 ', 300, '2009-11-02'); 
-- 无法插入，原表改变时不满足NUT NULL的条件

SELECT product_id, product_name, product_type, sale_price, (
SELECT AVG(sale_price) FROM product AS sale_price_all)
FROM product;

CREATE VIEW AvgPriceByType
AS SELECT product_id, product_name, product_type, sale_price, (
			SELECT AVG (sale_price) FROM product AS p2 WHERE p1.product_type = p2.product_type GROUP BY (product_type))AS avg_sale_price
FROM product AS p1; -- p1 的声明在最后

-- -------- 字符串按索引截取 -------------------------------
SELECT SUBSTRING_INDEX('www.mysql.com', '.', -2);
SELECT SUBSTRING_INDEX(SUBSTRING_INDEX('www.mysql.com', '.', 2), '.', -1);

-- -------- CASE WHEN ---------------------------------------
SELECT SUM(CASE WHEN product_type = '衣服' THEN sale_price ELSE 0 END) AS sum_price_clothes,
       SUM(CASE WHEN product_type = '厨房用具' THEN sale_price ELSE 0 END) AS sum_price_kitchen,
       SUM(CASE WHEN product_type = '办公用品' THEN sale_price ELSE 0 END) AS sum_price_office
  FROM product;
  
-- -------- 第二部分练习 ------------------------------------

-- 运算中含有 NULL 时，运算结果是否必然会变为NULL ？
-- 答：是的，因为NULL值只能由is null 或 is not null 来判断
  
SELECT product_name, purchase_price
  FROM product
WHERE purchase_price NOT IN (500, 2800, 5000);

SELECT product_name, purchase_price
  FROM product
WHERE purchase_price NOT IN (500, 2800, 5000, NULL);
-- IN 语句中不能有NULL

SELECT  SUM(CASE WHEN sale_price <= 1000 THEN 1 ELSE 0 END) AS low_price,
		SUM(CASE WHEN (sale_price BETWEEN 1001 AND 3000) THEN 1 ELSE 0 END) AS mid_price,
        SUM(CASE WHEN sale_price > 3000 THEN 1 ELSE 0 END) AS high_price
  FROM product;
```  

