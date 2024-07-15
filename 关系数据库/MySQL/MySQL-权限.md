## 用户与权限管理

### 1. 用户管理

MySQL用户可以分为**普通用户** 和 `root用户`， root用户是超级管理员，普通用户只拥有被授予的权限。

对用户的操作核心就是操作 `mysql.user` 表， mysql8 推荐用标准SQL操作，避免直接 upsert 表的操作。

user表的主键是 (host,user) 的联合主键，host取值是 `%` 表示不限制host，是 `localhost` 表示只能本地登录。 

##### 1.1 查询用户

```mysql
 select host,user from mysql.user;
```

##### 1.2 创建用户

```
CREATE USER 用户名 [IDENTIFIED BY '密码'][,用户名 [IDENTIFIED BY '密码']]
```

需要有**创建用户的权限** 才可以执行。

##### 1.3 修改用户

直接update user表，推荐删除&新建替代这个操作。

```mysql
UPDATE mysql.user SET USER='li4' WHERE USER='wang5'; 
FLUSH PRIVILEGES;
```

##### 1.4 删除用户

```mysql
DROP USER user[,user]…;

-- 例如
DROP USER 'jeffrey'@'localhost';
-- 如果省略主机名称，默认是 % 
```

##### 1.5 设定用户密码

```mysql
ALTER USER user [IDENTIFIED BY '新密码'] [,user[IDENTIFIED BY '新密码']]…;
```

- 当前用户可以用 user() 函数指定

```mysql
SET PASSWORD FOR 'username'@'hostname'='new_password';
```

- 对于当前用户 `SET PASSWORD='new_password';`



### 2.权限管理

MySQL权限列表

```mysql
show privileges
```

[Privileges Provided by MySQL](https://dev.mysql.com/doc/refman/8.0/en/privileges-provided.html)



授权的原则： 最小权限原则，限制登录主机ip，复杂密码，清理用户



#### 2.1 授权

给用户授权的方式有 2 种，分别是通过把 角色赋予用户给用户授权 和 直接给用户授权 。

```mysql
GRANT 权限1,权限2,…权限n ON 数据库名称.表名称 TO 用户名@用户地址 [WITH GRANT OPTION] [IDENTIFIED BY ‘密码口令’];
```

- 该权限如果发现没有该用户，则会直接新建一个用户。
- WITH GRANT OPTION 表示是否授予grant权限

授予通过网络方式登录的joe用户 ，对所有库所有表的全部权限，密码设为123。注意这里唯独不包括grant的权限

```
GRANT ALL PRIVILEGES ON *.* TO joe@'%' IDENTIFIED BY '123';
```

该权限如果发现没有该用户，则会直接新建一个用户。

#### 2.2 查看权限

- 查看当前用户权限

```mysql
SHOW GRANTS; 
# 或 
SHOW GRANTS FOR CURRENT_USER; 
# 或 
SHOW GRANTS FOR CURRENT_USER();
```

- 查看某用户的全局权限

```mysql
SHOW GRANTS FOR 'user'@'主机地址';
```

#### 2.3 收回权限

**注意：在将用户账户从user表删除之前，应该收回相应用户的所有权限。**

- 收回权限命令

```mysql
REVOKE 权限1,权限2,…权限n ON 数据库名称.表名称 FROM 用户名@用户地址;
```

- 举例

```mysql
#收回全库全表的所有权限 
REVOKE ALL PRIVILEGES ON *.* FROM joe@'%'; 
#收回mysql库下的所有表的插删改查权限 
REVOKE SELECT,INSERT,UPDATE,DELETE ON mysql.* FROM joe@localhost;
```

- 注意：`须用户重新登录后才能生效` 

### **3.** **角色管理**

##### **3.1** **创建角色**

```mysql
CREATE ROLE 'role_name'[@'host_name'] [,'role_name'[@'host_name']]...
```

角色名称的命名规则和用户名类似。如果`host_name省略，默认为%`，`role_name不可省略`，不可为空。

##### **3.2** **给角色赋予权限**

```mysql
GRANT privileges ON table_name TO 'role_name'[@'host_name'];
```

上述语句中privileges代表权限的名称，多个权限以逗号隔开。可使用SHOW语句查询权限名称

```mysql
SHOW PRIVILEGES\G
```

##### **3.3** **查看角色的权限**

```mysql
SHOW GRANTS FOR 'role_name';
```

只要你创建了一个角色，系统就会自动给你一个“`USAGE`”权限，意思是`连接登录数据库的权限`。

##### **3.4** **回收角色的权限**

```mysql
REVOKE privileges ON tablename FROM 'rolename';
```

##### **3.5** **删除角色**

```mysql
DROP ROLE role [,role2]...
```

注意，`如果你删除了角色，那么用户也就失去了通过这个角色所获得的所有权限`。

##### **3.6** **给用户赋予角色**

角色创建并授权后，要赋给用户并处于`激活状态`才能发挥作用。

```mysql
GRANT role [,role2,...] TO user [,user2,...];
```

查询当前已激活的角色

```mysql
SELECT CURRENT_ROLE();
```

##### **3.7** **激活角色**

**方式1：使用set default role 命令激活角色**

```mysql
SET DEFAULT ROLE ALL TO 'kangshifu'@'localhost';
```

**方式2：将activate_all_roles_on_login设置为ON**

```mysql
SET GLOBAL activate_all_roles_on_login=ON;
```

这条 SQL 语句的意思是，对`所有角色永久激活`。

##### **3.8** **撤销用户的角色**

```mysql
REVOKE role FROM user;
```

##### **3.9** **设置强制角色(mandatory role)**

方式1：服务启动前设置

```ini
[mysqld] 
mandatory_roles='role1,role2@localhost,r3@%.atguigu.com'
```

方式2：运行时设置

```mysql
SET PERSIST mandatory_roles = 'role1,role2@localhost,r3@%.example.com'; #系统重启后仍然有效
SET GLOBAL mandatory_roles = 'role1,role2@localhost,r3@%.example.com'; #系统重启后失效
```
