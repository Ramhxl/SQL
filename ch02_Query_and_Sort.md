### code and summarys
```sql
USE Datawhale;

-- --------挑选
SELECT product_name, product_type
	FROM product
	WHERE product_type = '衣服';
    
SELECT *
	FROM product;

SELECT DISTINCT product_type
	FROM product;
    
SELECT product_name, product_type, sale_price
	FROM product
	WHERE NOT sale_price >= 1000;
    
SELECT product_name, product_type, regist_date
	FROM product
	WHERE product_type = '办公用品'
	AND ( regist_date = '2009-09-11'
		OR regist_date = '2009-11-11');
        
-- --------------------------第一部分练习------------------------------
-- 编写一条SQL语句，从 product(商品) 表中选取出“登记日期(regist_date)在2009年4月28日之后”的商品，
-- 查询结果要包含 product name 和 regist_date 两列。
SELECT product_name, regist_date
	FROM product
    WHERE regist_date > '2009-04-28';
     
-- 从 product 表中取出“销售单价（sale_price）比进货单价（purchase_price）高出500日元以上”的商品
SELECT product_name, sale_price, purchase_price
	FROM product
    WHERE sale_price >= 500 + purchase_price;
    
SELECT product_name, sale_price, purchase_price
	FROM product
    WHERE NOT sale_price - purchase_price < 500;

-- --------聚合
-- 计算全部数据的行数（包含NULL）
SELECT COUNT(*)
  FROM product;
-- 计算NULL以外数据的行数
SELECT COUNT(purchase_price)
  FROM product;
-- 计算销售单价和进货单价的合计值
SELECT SUM(sale_price), SUM(purchase_price) 
  FROM product;
-- 计算销售单价和进货单价的平均值
SELECT AVG(sale_price), AVG(purchase_price)
  FROM product;
-- MAX和MIN也可用于非数值型数据
SELECT MAX(regist_date), MIN(regist_date)
  FROM product;

-- 计算去除重复数据后的数据行数
SELECT COUNT(DISTINCT product_type)
 FROM product;
-- 是否使用DISTINCT时的动作差异（SUM函数）
SELECT SUM(sale_price), SUM(DISTINCT sale_price)
 FROM product;
 
-- ------- GROUP BY 
 -- 按照商品种类统计数据行数
SELECT product_type, COUNT(*)
  FROM product
  WHERE (product_type = '衣服' OR product_type = '办公用品')
 GROUP BY product_type;
 -- 不含GROUP BY
SELECT product_type, COUNT(*)
  FROM product;

SELECT purchase_price, COUNT(*)
  FROM product
 WHERE product_type = '衣服'
 GROUP BY purchase_price;
 
-- ------- HAVING
SELECT product_type, COUNT(*)
  FROM product
 GROUP BY product_type
HAVING COUNT(*) = 2;
-- 错误形式（因为product_name不包含在GROUP BY聚合键中）
SELECT product_type, COUNT(*)
  FROM product
 GROUP BY product_type
HAVING product_name = '圆珠笔';

-- -------- 排序
-- 降序排列
SELECT product_id, product_name, sale_price, purchase_price
  FROM product
 ORDER BY sale_price DESC;
-- 多个排序键
SELECT product_id, product_name, sale_price, purchase_price
  FROM product
 ORDER BY sale_price, product_id;
-- 当用于排序的列名中含有NULL时，NULL会在开头或末尾进行汇总。
SELECT product_id, product_name, sale_price, purchase_price
  FROM product
 ORDER BY purchase_price;
 
-- ---------------------------第二部分练习---------------------------------
-- product name无法SUM

-- 请编写一条SELECT语句，求出销售单价（ sale_price 列）合计值大于进货单价（ purchase_price 列）合计值1.5倍的商品种类
SELECT product_type, SUM(sale_price) AS sum, SUM(purchase_price) AS sum
  FROM product
  GROUP BY  product_type
  HAVING SUM(sale_price) > 1.5 * SUM(purchase_price);
  ```
  ### 多重order by
  
注意在多重order by的时候，如图，优先级为regist_date > purchase_price，那么相应的code也需要根据优先级顺序由先到后编写

![image](https://github.com/Ramhxl/SQL/blob/main/images/ch02_order_by.png)

```sql
-- 思考 ORDER BY 子句的内容
SELECT *
  FROM product
  ORDER BY regist_date IS NOT NULL, regist_date DESC, 
			purchase_price IS NOT NULL, purchase_price ASC;
-- 多个字段时，优先级按先后顺序而定
```

