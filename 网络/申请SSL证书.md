## acme.sh 申请SSL

-------

- acme.sh 已经被ZeroSSL收购，默认CA为 **ZeroSSL**，可以指定其他CA， [详见官方doc ](https://github.com/acmesh-official/acme.sh/wiki/Change-default-CA-to-ZeroSSL)。

- 确认本地windows是否已经安装指定的CA证书，方法：

	运行 certmgr.msc ，一般在  **第三方证书颁发机构** 或 **受信任的根证书颁发机构** 能查到。

	注： 发现，ZeroSSL CA证书是在 win10还没有的

	如果没有证书，就需要手动下载 ca.cer 证书到本地双击安装。

- acme.sh 需要一个任意的邮箱地址。

- curl 默认寻找证书位置：

	```
	*  CAfile: /etc/ssl/certs/ca-certificates.crt
	*  CApath: /etc/ssl/certs
	```



acme.sh 申请流程

安装 => 获取DNS APIkey => 申请（命令）  => 安装

参考：

[acme.sh申请免费证书 | cc's blog (cc2048.com)](https://cc2048.com/archives/bd4e69fb.html)

## Squid 代理

- 目前我的代理脚本还是无法 代理 bing的网站

参考

[Squid代理服务器搭建 | cc's blog (cc2048.com)](https://cc2048.com/archives/c4788f70.html)


个人 API-token

```
export DOMAIN="grebeci.top"
export CF_Key="64d8d1015e5d7d446131ea51e7054f0570846"
export CF_Email="grebeci_@outlook.com"
export LOCALNET="$(curl -s http://httpbin.org/ip | jq -r '.origin')"
export CF_TOKEN_DNS="adUcnUBfrh1y0c0bATGCRCr-xDJdbwnVuKOOpGAt"
export ZONE_ID="25b7ae690060fd9919d1dda7d914487e"
export VULTR_API_KEY="UYI47ZRNUUCNACVL2OBFCOCUFCLWLXTDTZGA"




# export REGION_ID=("sea" "lax" "atl" "cdg")
export REGION_ID="atl"


CF_TOKEN_DNS="p1Gqle_TCHGhFjdavic4l9dYhnAonDjb0WBeF_tj"
```



