### Schema Objects

数据库模式是数据结构（称为模式对象）的逻辑容器。模式对象的例子包括表和索引。每个用户账户拥有一个模式，模式名称与用户相同。

数据库结构 ： DataBase => Schema => Table    

模式对象（Schema Objects）的类型：

Table，Indexes, Partitions，Views，Sequence，Dimension，Synonyns，PL/SQL subprograms and packages 

模式对象的一些属性

- Schema Object Storage：有些模式用段来存储数据，例如表和索引。有的模式只需要存储元数据，例如视图和序列。
- Schema Object Dependencies ： 模式对象之间有依赖关系，例如视图依赖表，表结构的变更会导致视图失效。


SYS and SYSTEM Schemas

sys ： 最高权限的账户，称为管理账户。
`SYSTEM` schema ： 存储数据库级别的元数据，任何用户不得修改。


### Overview of Tables

表描述一个实体，即必须记录信息的重要事物。例如，雇员就是一个实体。

可以创建的表类型

- head-organized table （堆组织表）
    
    堆组织表不按任何特定顺序存储行。CREATE T ABLE 语句默认创建堆组织表。
    
- index-organized table (索引组织表)
    
    索引组织表根据主键值排列行。
    
- external table
    
    外部表是一种只读表，其元数据存储在数据库中，但数据存储在数据库之外。
    

#### Column

列描述实体的属性。

- 表中可以包含虚拟列，虚拟列式表达式或者函数。
    
    不可见列：不可见列是用户指定的列，其值只有在列名明确指定时才可见。例如 select * 查询不到不可见列。
    

#### Row

一行表示一个对象。

#### Oracle Data Types

##### Character Data Types

VARCHAR2 数据类型存储长度可变的字符字面量。创建带有 VARCHAR2 列的表时，需要指定最大字符串长度。

CHAR 存储固定长度的字符串。创建带有 CHAR 列的表时，该列需要一个字符串长度。默认长度为 1 字节。数据库使用空格将值填充到指定长度。

Oracle 数据库使用非填充比较语义比较 VARCHAR2 值，使用空白填充比较语义比较 CHAR 值。

NCHAR 和 NVARCHAR2 数据类型存储 Unicode 字符数据。

##### Numeric Data Types

NUMBER(p,s) 格式指定定点数， Precision 精度 ：指定总位数， scale刻度 ：刻度指定从小数点到最小有效数字的位数。NUMBER 使用的是十进制精度，没有浮点数精度损失和舍入误差。

##### Datetime Data Types

日期时间数据类型包括 DATE 和 TIMESTAMP。Oracle 数据库为时间戳提供全面的时区支持。

**DATE Data Type** ：

Date存储日期和时间。精度到秒，没有时区的概念。默认是用 formatting-string 显示。

**TIMESTAMP Data Type** ：

Date 的扩展，存储到小数秒。有时区信息的是 `IMESTAMP WITH TIME ZONE` 和 `TIMESTAMP WITH LOCAL TIME ZONE`

##### Rowid Data Types ：

数据库中存储的每一行都有一个地址。Oracle 数据库使用 ROWID 数据类型来存储数据库中每一行的地址 (rowid)。

Oracle 数据库中的每个表都有一个名为 ROWID 的伪列。ROWID 伪列的值是字符串，代表每一行的地址。这些字符串的数据类型为 ROWID。可以通过使用保留字 ROWID 作为列名的 SQL 查询来检索每一行的 rowid。

##### Format Models and Data Types

在 SQL 中，可以使用格式模型作为 TO_CHAR 和 TO_DATE 函数的参数，以格式化要从数据库返回的值，或格式化要存储在数据库中的值。

TO_CHAR(salary, '$99,990.99')

TO_DATE('1998 05 20','YYYY MM DD')

### Integrity Constraints（完整性约束）

完整性约束是一种命名规则，用于限制表中一个或多个列的值。

### Table Storage（表存储）

##### Row Storage （行存）

- rowid 和物理地址想对应，并且数据库内部使用rowid 来构建索引。
    

### Overview of Temporary Tables

- 临时表保存仅在事务或会话期间存在的数据。
    
- 可以创建全局临时表或私有临时表。
    

- 全局临时表也是数据字典中静态定义的持久对象。对于私有临时表，元数据只存在于内存中，但可以驻留在磁盘上的临时表空间中。
    

私有临时表对于动态报表应用程序非常有用。例如，客户资源管理 (CRM) 应用程序可能会无限期地以同一用户身份连接，同时激活多个会话。每个会话都会为每个新事务创建一个名为 ORA$PTT_crm 的私有临时表。应用程序可以在每个会话中使用相同的表名，但要更改定义。数据和定义只对会话可见。表定义会一直存在，直到事务结束或手动删除表。

### Overview of External Tables