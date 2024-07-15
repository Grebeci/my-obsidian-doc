## 1. SQL方言

#### 1.1 Alter Table

```sql
alter table table_name option_statement 
```



###### 1) add column

```SQL
ALTER TABLE table_name ADD [COLUMN] column_name column_definition [FIRST|AFTER existing_column];
ALTER TABLE table_name ADD [INDEX|KEY] index_name (column_list);

column_definition :data_type [NOT NULL | NULL] [DEFAULT default_value]
   				[AUTO_INCREMENT] [UNIQUE [KEY] | [PRIMARY] KEY]  
   				[COMMENT 'string']  
```



###### 2) add CONSTRAINT 

```sql
ALTER TABLE 表名 
ADD CONSTRAINT 外键约束名 FOREIGN KEY (外键字段名) REFERENCES 外表表名 (主键字段名) 
[ON DELETE {CASCADE | SET NULL | NO ACTION | RESTRICT}] [ON UPDATE {CASCADE | SET NULL | NO ACTION | RESTRICT}];
```



###### 3) add index

```sql
ALTER TABLE table_name ADD [INDEX|KEY] index_name (column_list);
```



## 2. 函数

#### 2.1 Strings



[MySQL :: MySQL 8.0 Reference Manual :: 12.8 String Functions and Operators](https://dev.mysql.com/doc/refman/8.0/en/string-functions.html)

##### 某个字符出现在字符串的次数

```sql
   CHAR_LENGTH('AcBAc')-LENGTH(REPLACE('AcBAc','A',''));
```

##### 子串相关

- SUBSTRING(s, start, length)  或者
	- SUBSTRING(s, start, length) 
	- MID(s, n, len)
- LEFT(s, n)  , RIGHT(s, n)
- LOCATE(s1, s)  ， POSITION(s1 IN s)
- REPLACE(s, s1, s2)
- REVERSE(s)





