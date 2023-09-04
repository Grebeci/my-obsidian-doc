Q：**终端的提示符（prompt）**   `[root@Hostname ~]#` 的颜色，怎么整？

P：SSH登录服务器，由于 prompt 没有颜色，难以区分命令和输出。

A: 终端提示符是远程服务器 PS1 环境变量所决定的

- 如果在从远程服务器上动手：

	在 Bash 中，您可以在远程服务器上的 .bashrc 文件中添加以下内容来更改提示符的颜色：

	```shell
	export PS1="\[\e[32m\]\u@\h:\w\$ \[\e[m\]"
	```

- 如果是通过 SSH 命令行参数临时修改

	```shell
	ssh -t user@remote-server "export PS1='\[\e[32m\]\u@\h:\w\$ \[\e[m\]'; bash -l"
	```

Q: **ssh 免密登录**

```shell
ssh-keygen -t rsa
ssh-copy-id user@remote_host
```

Q: **ssh tunel for proxy** 

1. 本地`ssh 隧道`过去

(本地动态端口转发)

```shell
ssh -c aes128-gcm@openssh.com -N -D 1080 -C -o ServerAliveInterval=30 -o TCPKeepAlive=yes username@your_vps_ip -p 22 
```
- 加密可以不选。

或者更简单命令：

```
ssh -N -D 1080 username@your_vps_ip
```

2. 端口映射方式：

前提 ：VPS 提供Socks5 或者http的代理服务
```bash
ssh -N -L 1080:localhost:http_port -o ServerAliveInterval=30 -o TCPKeepAlive=yes username@your_vps_ip
```



