## Windows Terminal + Bash

Windows Terminal 

**选择文本**

左键单击并拖动鼠标以创建选区。 双击按单词扩展选区，三次单击按线条扩展选区。

如果按住 Alt 键，将创建块选择（而不是行选择）。 块状选择创建一个矩形区域，该区域不会环绕到行尾。

如果按住 Shift 键，则可以将选区显式扩展到终端上的特定点，而无需单击和拖动。







### 1. 快捷键

**Bash 行操作**

> [行操作 - 《阮一峰 Bash 脚本教程》 - 书栈网 · BookStack](https://www.bookstack.cn/read/bash-tutorial/docs-readline.md)
>
> 在 Bash 环境下，快捷键的配置通常存储在一个叫做 `.inputrc` 的文件中，这个文件位于用户的家目录中。
>
> `.inputrc` 文件用于自定义 readline 库的行为，readline 是 Bash 和其他许多 shell 使用的一个库，用于处理输入的编辑功能，比如快捷键绑定。

- `Ctrl + a`：移到行首
  
- `Ctrl + e`：移到行尾。
  
- `Alt + f`：移动到当前单词的词尾。
  
- `Alt + f`：移动到当前单词的词尾。
  
- `Ctrl + w`：删除光标前面的单词。
  
- `Ctrl + k`：剪切光标位置到行尾的文本。
  
- `Ctrl + u`：剪切光标位置到行首的文本。
  
- `Alt + d`：剪切光标位置到词尾的文本。
  
- `Alt + Backspace`：剪切光标位置到词首的文本。
  
- `Ctrl + y`：在光标位置粘贴文本。



**补全**

- Tab：完成自动补全。
  
- `Alt + $`：变量名补全。
  
- `Alt + Tab`：尝试用`.bash_history`里面以前执行命令，进行补全。



### 2. 工作流

**1. 删除当前命令** 

1.  `Ctrl + e` + `Ctrl + u` 。 

2. 自定义快捷键（bind + readline 库），实现光标任意位置删除整行。

   ```
   bind '"\C-x": kill-whole-line'
   ```

   