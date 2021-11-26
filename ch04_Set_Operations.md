Codes and answers:
```sql
USE datawhale;
-- -------- 练习题 ------------------------------
SELECT  product_id, product_name, product_type, sale_price, purchase_price
  FROM PRODUCT 
 WHERE sale_price<800
 
 UNION
 
SELECT  product_id, product_name, product_type, sale_price, purchase_price
  FROM PRODUCT 
 WHERE sale_price>1.5*purchase_price;
 
 -- 保留重复行
SELECT product_id, product_name
  FROM Product
 UNION ALL
SELECT product_id, product_name
  FROM product;
  
SELECT * 
  FROM product 
 WHERE sale_price < 1000
 UNION ALL
SELECT * 
  FROM product 
 WHERE sale_price < 1.5 * purchase_price;
 
 -- -------- 时间日期类型和字符串,数值以及缺失值均能兼容. -------------
SELECT SYSDATE(), SYSDATE(), SYSDATE()
UNION
SELECT 'chars', 123,  null;

-- 使用 IN 子句的实现方法 写 EXCEPT 运算
SELECT * 
  FROM Product
 WHERE product_id NOT IN (SELECT product_id 
                            FROM Product2);
 
 SELECT * 
  FROM product
 WHERE sale_price > 2000 
   AND product_id NOT IN (SELECT product_id 
                            FROM Product 
                           WHERE sale_price<1.3*purchase_price);

-- 使用 NOT IN 实现两个表的差集 ---------------------------
SELECT * 
  FROM Product
 WHERE product_id NOT IN (SELECT product_id FROM Product2)
UNION
SELECT * 
  FROM Product2
 WHERE product_id NOT IN (SELECT product_id FROM Product);

-- -------- 内连结 ---------------------------------
SELECT SP.shop_id
       ,SP.shop_name
       ,SP.product_id
       ,P.product_name
       ,P.product_type
       ,P.sale_price
       ,SP.quantity
  FROM ShopProduct AS SP
 INNER JOIN Product AS P
    ON SP.product_id = P.product_id;
    
SELECT  SP.shop_id
       ,SP.shop_name
       ,SP.product_id
       ,P.product_name
       ,P.product_type
       ,P.sale_price
       ,SP.quantity
  FROM ShopProduct AS SP
 INNER JOIN Product AS P
    ON SP.product_id = P.product_id
 WHERE SP.shop_name = '东京'
   AND P.product_type = '衣服' ;
```
