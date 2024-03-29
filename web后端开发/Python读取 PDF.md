`Python` 读取 `PDF`

- `PDF` 的本身的结构怎么描述的。Python 库（ `PyMuPDF` ）怎么描述这个格式的。



#### `PDF` 格式描述



##### 官方描述

`PDF`（Portable Document Format）是一种用于表示文档的文件格式，具有独立于软件、硬件和操作系统的特性。`PDF`文件通常用于保持文件的原始图形外观。

PDF文件的主要结构包括以下几个部分：

1. **对象**：PDF文件是由许多小的对象组成的，这些对象可以是文本、图像、页面描述等。每个对象都有一个唯一的编号。

2. **页面树（Page Tree）**：页面树定义了PDF文档中页面的顺序。它是一个树状结构，其中的叶节点指向单独的页面对象。

3. **内容流**：每个页面都有一个或多个内容流，这些内容流包含了实际绘制在页面上的指令。这些指令可以是文本显示、图形绘制、图像放置等。

4. **资源**：资源是内容流绘制内容所需的额外信息，如字体、图像等。

5. **文件目录**：PDF文件有一个根目录（通常称为目录对象），它包含了指向文件中其他重要对象的引用，如页面树。

6. **交叉引用表**：此表指明了文件中各个对象的位置，使得快速访问成为可能。

7. **文件尾部**：包含一个指向交叉引用表起始位置的指针和文件中对象总数等信息。



####  `PyMuPDF`  对PDF格式的描述和抽象

