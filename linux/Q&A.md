- [ ] 如何定位到一个环境变量在哪初始化的，何时初始化

- [x] 使用 Ctrl + Z  stop 进程，如何找到并kill掉

- [ ] 


### Q&A

1. 使用 Ctrl + Z  stop 进程，如何找到并kill掉

	```bash
	ps -e j | grep T
	```

stopped进程的STAT状态为T

2. 进程往已删除的文件写数据，导致磁盘写满

	```
	# 现象
	df -hT # 占用率100%
	du --max-depth=1 ./ -h # 实际没有满
	lsof |grep delete
	```


