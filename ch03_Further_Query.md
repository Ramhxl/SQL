### SAFE UPDATE MODE

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

### 
