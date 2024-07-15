# shell 

1.  一切（变量值）都是字符串

2.  对 空格（` `） 和 逗号 `;` 敏感， 且使用空格分隔参数，用逗号分隔命令


### Run Script

1.

​    sh test.sh

2. 

	```bash
	 #!bin/bash
	```
	
	chmod +x test.sh
	
	./test.sh
	
	/${absolutePath}/test.sh

3. 命令优先级

	显式调用脚本（绝对路径 / 相对路径 执行的路径
	
	别名（alias
	
	bash 内部命令
	
	PATH 的目录顺序查找



### 配置文件

`/etc/profile `

`/etc/profile.d/*.sh`

`~/.bash_profile ` 

`~/.bashrc `

`/etc/bashrc`

```
/etc/profile 		-> 		 ~/bash_profile          ->         ~/.bashrc		->		/etc/bashrc      -> 命令提示符

   		|																					|
   		|                                                                                   |
  	    V																					|
  /etc/profile.d/*.sh              <-                   <-                 <-               |
  
   
```

新增环境变量

​	对所有用户生效： /etc/profile  或者 /etc/profile/my.sh

​	对某个用户有效： ~/bashrc  ~/bash_profile
















