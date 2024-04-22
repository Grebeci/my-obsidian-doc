

#### ssh 指定配置项

> 可以在命令行上临时覆盖配置文件中的设置或指定通常不在配置文件中设置的选项。



1. SSH 客户端忽略任何主机密钥的相关检查

   > 通常是提示 yes or error

```shell
ssh -p 22  \
	-o StrictHostKeyChecking=no   \ 
	-o UserKnownHostsFile=/dev/null \
	-o GlobalKnownHostsFile=/dev/null \
	user@remote_host
```



