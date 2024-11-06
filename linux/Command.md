- [ ] [Windows Terminal installation | Microsoft Learn](https://learn.microsoft.com/en-us/windows/terminal/install)  自定义的配置和 terminal chat

- [ ] [Warp: Your terminal, reimagined](https://www.warp.dev/) ： 体验+ 习惯


## Overview

#### 1. 命令行参数语法

Linux 的命令行语法结构语法结构主要是选项和参数的处理方式。这些命令大多数遵循 `POSIX`（Portable Operating System Interface）标准，但是也有引入 GNU 工具和 移植的 BSD 系统工具（`ps` 命令）以及一些第三方工具的自定义语法结构。

##### 1.1 `POSIX`

> 关于 POSIX 参数格式描述约定【文档】： [Utility Argument Syntax ]((https://pubs.opengroup.org/onlinepubs/9699919799.2018edition/)) （ 如何描述一个命令的用法，也就是man手册或者help手册描述命令语法的格式）。

**Syntax**
```
Command Option Option_argument Operand
```

POSIX 风格的参数，也称为命令行参数风格，主要有以下几种形式：

1. **短选项（Short Options）**：
   - 由一个短划线 `-` 和一个字符组成，如 `-a` 或 `-b`。
   - 短选项可以堆叠。例如，`-abc` 相当于 `-a -b -c`。
   - 短选项可以接受参数，参数既可以紧跟在选项字符后面，也可以用空格分隔。例如，`-o value` 或 `-ovalue`。

2. **长选项（Long Options）**：
   - 由两个短划线 `--` 后跟一个单词或单词组合表示，例如 `--help` 或 `--version`。
   - 长选项的参数通常用等号连接，或者用空格分隔。例如，`--output=value` 或 `--output value`。

3. **参数（Operands）**：
   - 指非选项的参数，不以短划线开头。在命令行中，参数通常跟在所有选项之后。例如，`command -a -b operand1 operand2`。

4. **选项参数（Option Arguments）**：
   - 一些命令支持选项后跟其参数，无论是短选项还是长选项。参数可以直接跟在短选项后面，也可以通过空格或等号与长选项分隔。
   - 例如，在命令 `command -a arg1 --option=value` 中，`arg1` 是 `-a` 的参数，`value` 是 `--option` 的参数。

5. **结束选项标记（End-of-Options Marker）**：
   - 特殊的 `--` 标记用来明确指示选项列表的结束。在这个标记之后的任何内容都被视为参数，而不是选项，即使它们以短划线开头。
   - 例如，在命令 `command -a -- -b` 中，`-b` 被视为参数而非选项。

##### 1.2 `BSD`

Linux 系统中存在 BSD 风格的命令，BSD 风格的命令参数在 Linux 中主要是通过移植过来的 BSD 版本的工具体现的，例如在某些 Linux 发行版中，`ps`、`tar` 和 `netstat` 等命令可能提供 BSD 风格的选项。

#### 2.  Command Info/doc

即时的、快速得到这个命令的信息。

**命令的帮助**

1. **man（手册）命令**：
   - 使用方法：`man [命令]`
   - 这是最常用的获取命令帮助的方法。例如，要查看 `ls` 命令的帮助信息，可以使用 `man ls`。
2. **--help 参数**：
   - 使用方法：`[命令] --help`
   - 大多数 Linux 命令都支持 `--help` 参数，用于显示命令的基本用法和选项。例如，`ls --help` 会显示 `ls` 命令的帮助信息。
3. **info 命令**：
   - 使用方法：`info [命令]`
   - `info` 提供比 `man` 更详细的帮助信息，有的命令只在 `info` 中有详细文档。例如，`info ls` 会显示关于 `ls` 命令的详细信息。
4. **whatis/apropos 命令**：
   - 使用方法：`apropos [关键字]` 或 `whatis [命令]`
   - `whatis` 用于搜索一个命令的简短描述。
   - `apropos` 用于搜索手册页面的描述部分，找到与特定关键字相关的所有命令。
5. **tldr（Too Long; Didn't Read）**：
   - 需要先安装 `tldr` 客户端。
   - `tldr` 提供了简洁的命令用法实例，是快速了解命令用法的好方法。例如，`tldr ls` 会显示 `ls` 命令的简化用法。



**Shell 自动提示**

[oh-my-zsh Github Wiki](https://github.com/ohmyzsh/ohmyzsh/wiki)



**终端（AI）提示**

- Copilot ： [GitHub Copilot（CLI 版） - GitHub 文档](https://docs.github.com/zh/copilot/github-copilot-in-the-cli)
- Warp：The terminal for the 21st century.
#### 3.  Command 类别

`which` :  

某个命令的可执行文件位置。`which` 命令用于在环境变量 `$PATH` 指定的目录中查找并显示某个程序的完整路径。

`alias` 

```	 shell
# 查询定义过的别名
alias

# 定义别名
alias ll='ls -l'
```

需要开始别名扩展

```
# Expand aliases defined in the shell 
shopt -s expand_aliases
```

`type`

判断命令是`alias`，`function`，`Shell Builtins`，`可执行文件`（`$PATH` 指向的目录）

#### 4. 工具

##### Windows Terminal  +  Bash 

##### item2 + zsh 

##  操作文件与目录

#### 1. `ls`

**【英文缩写】：** list 

**【功能描述】：** List information about the FILEs (the current directory by default).

```
ls [OPTION]... [FILE]...
OPTION:
  -a, --all                  do not ignore entries starting with .
  -d, --directory            list directories themselves, not their contents
  -h, --human-readable       with -l and -s, print sizes like 1K 234M 2G etc.
      --si                   likewise, but use powers of 1000 not 1024
  -H, --dereference-command-line
  -l                         use a long listing format
  -R, --recursive            list subdirectories recursivel
```

```shell
ls -l
total 102436
drwxr-x---      2   privoxy     adm   4096   Dec 10 00:00   privoxy
# Permissions Links  Owner     Group  Size     Modified      Name
```

`ls -l` 命令在 Linux 和类 Unix 系统中以长格式列出目录内容，显示的每一项包含了文件或目录的详细信息。以下是这些信息的解释：

1. **文件类型和权限**：这部分共有 10 个字符。第一个字符表示文件类型（例如，`-` 表示普通文件，`d` 表示目录，`i`  表示软连接）。接下来的九个字符分为三组，每组三个字符，分别代表文件所有者的权限、文件所属组的权限和其他用户的权限。每组权限包括读（`r`）、写（`w`）和执行（`x`）权限。

2. **链接数**：显示了文件的硬链接数量。对于目录，这个数字表示该目录中的子目录数量（包括 `.` 和 `..`）。

3. **所有者**：文件的所有者用户名。

4. **组**：文件所属的用户组。

5. **大小**：文件的大小，默认单位是字节。对于目录，显示的是目录本身的大小，而不是其中内容的总大小。

6. **修改时间**：文件最后一次被修改的时间。默认格式通常是“月 日 时:分”或“月 日 年”。

7. **文件名**：文件或目录的名称。如果文件是符号链接，还会显示链接到的目标文件名（例如 `link -> target`）。

示例输出看起来可能像这样：

```
-rw-r--r--  1 user group  4096 Jan  1 12:34 example.txt
drwxr-xr-x  2 user group  4096 Jan  1 12:34 my_directory
lrwxrwxrwx  1 user group    11 Jan  1 12:34 link -> target
```

- 第一行表示 `example.txt` 是一个普通文件，所有者有读写权限，组成员和其他用户只有读权限。
- 第二行表示 `my_directory` 是一个目录，所有者有读写执行权限，组成员和其他用户有读和执行权限。
- 第三行表示 `link` 是一个符号链接，指向名为 `target` 的文件或目录。

**【Snippet】：**  

Q :  列出文件夹的二级所有目录

A：`ls` 虽然有 `-R`   的选项，但无法控制深度。这个要求可以用 find 来实现。

```shell
find . -maxdepth 2 -type d -exec ls {} \;
```

```shell
tree -L 2 [your-directory]
```

```shell
for dir in [your-directory]/*; do
    if [ -d "$dir" ]; then
        echo "$dir:"
        ls "$dir"
    fi
done
```


#### 2. `cd` 

命令格式

```
cd DIR
选项： 
```

**【英文缩写】：** Change  directory.

**【功能描述】：** 改变当前路径

绝对路径和相对路径： 主要看相对于谁，相对于 **根路径** 就是绝对路径，相对于 **当前进程路径**  就是相对路径。

**【Snippet】：** 

Q： 拿到当前脚本的实际执行路径。

C：多脚本项目中，要使用绝对路径调用。

> 原因是：当使用相对路径在主脚本中调用其他脚本时，如果主脚本不是从其所在目录执行，可能导致路径错误，进而无法成功调用其他脚本。

A：这个最关键的是拿到当前脚本的执行路径，进而拿到 Project Home 路径。

拿到当前脚本的路径的方式：

```shell
# 这个脚本无论在哪个目录下被执行，都会自动切换到其实际所在的目录。
# 它处理了符号链接，确保即使在复杂的执行环境中也能准确找到脚本的位置。
SCRIPT_PATH="$(dirname "$(readlink -f "$0")")"

# 这种方式也可以，但通过符号链接调用脚本时，不能保证正确的行为。必要情况下检查 SCRIPT_PATH 是否为空
SCRIPT_PATH=$(cd $(dirname "$0") && pwd)
```

#### 3. pwd

**【英文缩写】：** Print Working Directory

大多数情况下，和 `echo $PWD` 命令的行为类似。

`cd` 命令切换目录后，shell 会自动更新 `PWD` 变量。

#### 4. mkdir

```
Usage: mkdir [OPTION]... DIRECTORY...
Create the DIRECTORY(ies), if they do not already exist.
  -p, --parents     no error if existing, make parent directories as needed
```

- `mkdir `  是创建 **空目录**，如果文件存在将会报错，且命令返回值是 非零。 
- `- p` 指示 `mkdir` 忽略已存在的目录，并且只在需要时（即目录尚未存在时）创建新目录。这个选项会避免已存在的目录报错和创建多级目录。 

**【英文缩写】：** make directory(ies). 

#### 5. rmdir

```
Usage: rmdir [OPTION]... DIRECTORY...
Remove the DIRECTORY(ies), if they are empty.

-p, --parents  
```

`-p`  选项只能递归删除 **空目录**。不会空目录报错，这和 `mkdir -p` 的行为不同。  

**【英文缩写】：** remove [empty] directory(ies).

**【功能描述】：** 删除空目录。

#### 6. touch

```shell
Usage: touch [OPTION]... FILE...
Update the access and modification times of each FILE to the current time.

A FILE argument that does not exist is created empty, unless -c or -h
is supplied.
```

更新文件的 修改时间， 如果文件不存在，就创建一个空文件。

**【功能描述】：** 创建空文件或者修改文件时间。

#### 7. stat

```shell
Usage: stat [OPTION]... FILE...
Display file or file system status.
```


#### 8. cat

```shell
Usage: cat [OPTION]... [FILE]...
Concatenate FILE(s) to standard output.

With no FILE, or when FILE is -, read standard input.

  -A, --show-all           equivalent to -vET 显示文件中的非打印字符、tab、换行符
  -n, --number             number all output lines
Examples:
  cat f - g  Output f's contents, then standard input, then g's contents.
  cat        Copy standard input to standard output.
```

命令 ： `cat f - g` ，在 `cat` 命令的上下文中，`-` 用作文件名时代表标准输入。

在 Unix 和 Linux 命令行中，`-` 符号通常用作特殊的占位符，代表标准输入（stdin）或标准输出（stdout），具体取决于它在命令中的使用方式。这个约定允许命令行工具灵活地处理来自不同来源的数据流。

1. **作为标准输入**：当用在需要文件名的命令中时，`-` 代表标准输入。命令将从键盘输入（或管道连接的前一个命令的输出）读取数据，直到遇到文件结束符（EOF），如 Unix/Linux 系统中的 Ctrl+D。
2. **结合管道使用**：在管道（|）操作中，`-` 可以用来接收前一个命令的输出作为输入。

例如：

```
# 将标准输入的内容和文件内容一并显示
echo '######' |cat - test.log
```

【英文缩写】：Concatenate

【snippet】：

Here Doc

```
[command] <<[-] ['DELIMITER' | DELIMITER]
    HERE-DOCUMENT
  DELIMITER
```

#### 9. more

【功能描述】： 分屏展示。

读取整个文件进行显示，这可能导致大文件会延迟。

只能前进，不能后退。

#### 10. less

【英文原意】：opposite of more

【功能描述】：比 more 功能更强大，支持双向导航，搜索文本，跳转指定行，支持 Home / End。

性能上：`less` 一次只读取需要显示的部分，对于查看大文件更加高效。

#### 11. head

```shell
Usage: head [OPTION]... [FILE]...
Print the first 10 lines of each FILE to standard output.
With more than one FILE, precede each with a header giving the file name.

With no FILE, or when FILE is -, read standard input.

  -n, --lines=[-]NUM       print the first NUM lines instead of the first 10;
                             with the leading '-', print all but the last
                             NUM lines of each file
```

【功能描述】：显示前N行，默认前10行。



#### 12. tail

```shell
Print the last 10 lines of each FILE to standard output.
With more than one FILE, precede each with a header giving the file name.

With no FILE, or when FILE is -, read standard input.
   -f output appended data as the file grows;
   -n, --lines=NUM       output the last NUM lines, instead of the last 10;
```

【功能描述】：查看后10行文件，`-f`  监听文件的新增内容。



#### 13. ln

```
link source_file target_file
-s, --symbolic              # 对源文件建立符号链接，而非硬链接
```



#### 14. rm

```
Usage: rm [OPTION]... [FILE]...
Remove (unlink) the FILE(s).
 -f, --force           ignore nonexistent files and arguments, never prompt
 -r, -R, --recursive   remove directories and their contents recursiv
 
 To remove a file whose name starts with a '-', for example '-foo',
use one of these commands:
  rm -- -foo

  rm ./-foo
```


【功能描述】：删除文件或目录

```
# 删除文件一定要检查 var_path 是否为空，否则有删除错误目录的风险
rm -rf ${var_path}/path_to_file
```
- 当删除软连接，且软连接指向目录，则 `rm -rf lk_data` 和 `rm -rf lk_data/` 的含义不同。
- 



#### 15.  mv

```
Usage: mv [OPTION] Source DEST
Rename SOURCE to DEST, or move SOURCE(s) to DIRECTORY.
```

【功能描述】： 重命名或者移动文件

- `Source ` : 源文件列表，DEST 只能是一个文件或者目录。
- 当目标文件或目录不存在，相当于对文件目录重命名。
- 目标文件存在，则 **覆盖**, 如果目录存在，会成为目标目录的子目录。
- DEST 如果是目录，建议强制以 `/` 结尾。 

#### 16. cp

```
Usage: cp [OPTION]...  SOURCE DEST
Copy SOURCE to DEST, or multiple SOURCE(s) to DIRECTORY.

   -R, -r, --recursive          copy directories recursively
   -l  --link                   创建硬链接
   -s  --symblic-link           创建软链接
```

【功能描述】： 复制文件

- `Source ` : 源文件列表，DEST 只能是一个文件或者目录。
- 不能复制文件到不存在的 **目录**。
- 复制文件到目录，如果指定文件存在，会询问是否覆盖。
- 目录到目录，则  源目录后是否以 `/` 结尾会影响起其行为。

#### 注 

```
1. mv, rm, mkdir,cp 对路径是否 以 / 结尾语义敏感，需要仔细注意
2. mv，cp 对目录，文件路径是否存在行为有差别，实践中，是先判断文件存在，选择删除或者创建，而不是依赖具体命令的行为。具体细节注意即可。
```



#### 18. date

```shell
date [OPTION]... [+FORMAT]
-d, --date=STRING          解析字符串并按照指定格式输出，字符串不能是'now'。
```

date 的用法极多且灵活，列举idom记住即可

```
# 格式化输出：
date +"%Y-%m-%d"
date +"%Y-%m-%d %H:%M:%S"
date +%s

# 格式化日期
date -d "20200101" +"%Y-%m-%d"
date --date='@2147483647' +"%Y-%m-%d %H:%M:%S"
date -d "2020-01-01 12:12:12" +"%s"

# 时间加减
date -d "+1 day" +"%Y-%m-%d"            # 当前时间后一天
date -d "2020-01-01 +1 day" +"%Y-%m-%d" # 指定时间过一天
date -d "20200101 +1 day" +"%Y-%m-%d"   # 指定时间过一天
```



#### 17. chmod

```
Usage: chmod [OPTION]... MODE[,MODE]... FILE...
or:  chmod [OPTION]... --reference=RFILE FILE...

mode: [uhoa]*(+-=)*[rwx]
or [+-=][0-7]
```

【英文原意】：Change file mode bits.

【功能描述】： 修改文件的权限模式。

常用权限：

644 ： 文件权限，所有者读写，所属组读。

755： 目录权限/可执行文件权限，所有者读写之行，所出组读，执行权限。

777：最大权限。



#### 18. chown、chgrp

```shell

Usage: chown [OPTION]... [OWNER][:[GROUP]] FILE...
  or:  chown [OPTION]... --reference=RFILE FILE...
  
  -R, --recursive        operate on files and directories recursively

Usage: chgrp [OPTION]... GROUP FILE...
  or:  chgrp [OPTION]... --reference=RFILE FILE...
Change the group of each FILE to GROUP.
```

【英文原意】： Change file owner and/or group.

【功能描述】：修改文件和目录的所有者和所属组。

普通用户不能修改文件的所有者。哪怕自己是这个文件的所有者也不行。



#### 19.  find

```
find [path...] [expression]
default path is the current directory; default expression is -print
expression may consist of: operators, options, tests, and actions:

opeartor : 
   expr1 x expr2 , x: -a , -and , -o , -or, ','
					!expr , not expr
option: 
   -maxdepth LEVELS：限制搜索的最大深度。
	 -mindepth LEVELS：限制搜索的最小深度。
test:
	-name PATTERN：按名称模式搜索文件。
	-iname PATTERN：与 -name 类似，但忽略大小写。
	-type [bcdpflsD]：按文件类型搜索（例如 f 代表普通文件）。
	-size [+|-]N[bcwkMG]：按文件大小搜索。
	-perm MODE：按权限模式搜索。 （ [+|-]mode  这种语法受版本影响，不要记忆
	-user NAME：按文件所有者搜索。
	-group NAME：按组搜索。
	-mtime N、-atime N、-ctime N：按修改时间、访问时间或更改状态时间搜索。
	-newer FILE：找出比指定文件更新的文件。
action：
	-print：打印匹配的文件名。
	-exec COMMAND {} \;：对找到的每个文件执行 COMMAND。 
	-delete：删除找到的文件。
	-ls：类似于 ls -dils 的格式列出文件。
```



- `-name pattern`， 按照文件名搜索，其中 pattern 是通配符模式：（且是完全匹配）

  - `*`：匹配任意数量的字符（包括零个字符）。
  - `?`：匹配任意单个字符。
  - `[...]`：匹配方括号内的任意单个字符。范围可以用连字符指定，如 `[a-z]`。

- `-size n[cwbkmg]

  > - `b'  for 512-byte blocks (this is the default if no suffix is used)
  >
  > - `c'  for bytes
  >
  > - `w'  for two-byte words
  > - `k'  for kibibytes (KiB, units of 1024 bytes)
  > - M'  for mebibytes (MiB, units of 1024 * 1024 = 1048576 bytes)
  >
  > -  `G'  for gibibytes (GiB, units of 1024 * 1024 * 1024 = 1073741824 bytes)

- 按时间搜索，可以有 `[+/-] N` 的语法，要小心它们的语义。

#### 20 echo

```
echo [-n] [string ...]
```

### 21 tar

在linux 中可以识别的压缩和格式有十几种，有 .zip、.gz    .bz2 .tar . tar.gz. .tar.bz2

严格根据**扩展名**使用正确的命令，

广义的压缩分为两步，压缩和打包。

linux 下大多数是 tar.gz， 使用的命令为

解压：

```shell
tar -zxvf 压缩包 -C dest 
```

打包：

```shell
tar -zcvf 压缩包 source
```


```
用法: tar [选项...] [FILE]...  
​  
  -x, --extract, --get       从归档中解出文件  
  -z, --gzip, --gunzip, --ungzip   通过 gzip 过滤归档  
  -Z, --compress, --uncompress   通过 compress 过滤归档  
  -f, --file=ARCHIVE        使用指定的存档文件  
  -C, --directory=DIR        输出到指定目录
```



## 组合命令

#### 管道



#### xargs



#### grep

```shell
Usage: grep [OPTION]... PATTERNS [FILE]...

Pattern selection and interpretation:
    -v, --invert-match        select non-matching lines 反向查找
 
Output control:
     -n, --line-number         print line number with output lines 输出行号
```

【命令描述】：从文本流中提取和匹配特定模式的字符串行

【snippet】：

查找指定的进程名字。

```shell
ps -ef | grep -v grep | grep "process_name"
```





关机命令

shutdown 、 reboot









