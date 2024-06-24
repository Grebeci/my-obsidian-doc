#### read file 

##### 1.逐行读取文件

方法1 

```bash
while read line
do
    echo $line
done < filename 
```

方法2

```bash
cat filename | while read line
do
    etho $line
done
```

方法3

```bash
for line in `cat filename`
do
    echo $line
done
```

for 循环 遇到空格，换行就会循环一次

#### 文本操作

1. 删除匹配到的行（sed）

	```bash
	sed -i "/JAVA_HOME/d" /etc/profile.d/my_env.sh
	# 删除 JAVA_HOME 环境变量
	```

2. 向文件里追加字符串

```bash
# 添加JAVA_HOME 环境变量
cat >>  $ENV_FILE << EOF
#JAVA_HOME
export JAVA_HOME=/opt/module/jdk1.8.0_212
export PATH=\$PATH:\$JAVA_HOME/bin
EOF
```


3. XML文件添加配置项

```bash
function add_site() {
if [ ! -e $1 ]; then
touch $1
cat > $1 << EOF
<?xml version="1.0"?>
<?xml-stylesheet type="text/xsl" href="configuration.xsl"?>
<configuration>
</configuration>
EOF
fi
  sed -i "/<name>$2<\/name>/d" $1
  sed -i "/<\/configuration>/i<property><name>$2<\/name><value>$3<\/value><\/property>" $1
}
```

功能 在configuration 标签下添加  `<property><name>$1</name><value>$2</value></property>`

```xml
<?xml version="1.0"?>
<?xml-stylesheet type="text/xsl" href="configuration.xsl"?>
<configuration>
    <property><name>$1</name><value>$2</value></property>
</configuration>
```


3. 通过文本过滤出替换范围，然后替换

```
sed -i '/MySQL 8\.0 Community Server/,/enabled=1/{s/enabled=1/enabled=0/}' mysql-community.repo

# 过滤出  "MySQL 8.0 Community Server"  "enabled=1" 之间的文本， 把 "enabled=1" 替换为 enabled=0
# 这是贪婪匹配
```


#### etc

1. 变量判空赋值

```bash
[ -z "$JAVA_HOME" ] && JAVA_HOME=/opt/module/jdk1.8.0_212
rm -rf $JAVA_HOME
```

	

### 文件

1. 文件不存在就创建

```bash
[ -f $file_name ] || touch $file_name
```


