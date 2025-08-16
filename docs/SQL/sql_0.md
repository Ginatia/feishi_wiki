
## 创建表

首先描述表

| Customers | 表名      |
| --------- | ------- |
| ID        | INT     |
| NAME      | VARCHAR |
| AGE       | INT     |
| ADDRESS   | CHAR    |
| SALARY    | DECIMAL |

```sql
CREATE TABLE CUSTOMERS(
    ID INT NOT NULL,
    NAME VARCHAR (20) NOT NULL,
    AGE INT NOT NULL,
    ADDRESS CHAR (25),
    SALARY DECIMAL (18,2),
    PRIMARY KEY (ID)
);
```

- **`NOT NULL`**：该列必须有值，不能是 `NULL`。
    
- **`PRIMARY KEY (ID)`**：指定 `ID` 列为主键，确保唯一性并且不能为空。
    
- **`VARCHAR(n)`**：变长字符串，存储时只占用实际长度 + 1 字节。
    
- **`CHAR(n)`**：固定长度字符串，长度不足会自动填充空格。
    
- **`DECIMAL(p,s)`**：精确数值类型，`p` 是总位数，`s` 是小数位数（这里是 `18,2`，即总长 18 位，小数 2 位）。

## INSERT

```sql
INSERT INTO CUSTOMERS (ID,NAME,AGE,ADDRESS,SALARY)
VALUES (1, 'Ramesh', 32, 'Ahmedabad', 2000.00 );
```

## UPDATE

`UPDATE CUSTOMERS` ： 表示要修改 `CUSTOMERS` 表中的数据。
`SET ADDRESS = 'Pune'` ：指定要更新的列及新值，这里是把 `ADDRESS` 列更新为 `'Pune'`。
`WHERE ID = 6` ： 限定更新条件，只更新 **`ID` 等于 6** 的那一行。

```sql
UPDATE CUSTOMERS SET ADDRESS = 'Pune' WHERE ID = 6;
```

## DELECT

`DELETE FROM CUSTOMERS WHERE ID = 6;`

## SELECT

### P1

找出表中 **CITY** 条目的总数与表中不同 **CITY** 条目的数量之间的差值。

```sql
SELECT 
    COUNT(*) - COUNT(DISTINCT CITY) as ans
FROM STATION;
```

### P2

查询 STATION 中 CITY 名称最短和最长的两个城市，以及它们各自的长度（即名称中的字符数）。
如果最小或最大的城市不止一个，请选择按字母顺序排列的第一个。

```sql
SELECT CITY, CHAR_LENGTH(CITY) AS city_length
FROM STATION
ORDER BY CHAR_LENGTH(CITY) ASC, CITY ASC
LIMIT 1;

SELECT CITY, CHAR_LENGTH(CITY) AS city_length
FROM STATION
ORDER BY CHAR_LENGTH(CITY) DESC, CITY ASC
LIMIT 1;
```


