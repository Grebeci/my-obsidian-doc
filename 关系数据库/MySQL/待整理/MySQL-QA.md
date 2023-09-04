### exsits 语法和 in语法的选用？

exists ， not exists 是经常用在相关子查询上，用来代替 in，not in 提升性能

-   exists /in : in 是把外表和子表作hash 连接，而exists是对外表作loop循环，每次loop循环再对子表进行查询。
    
    如果子表很大，外表很小，而且连接键有索引，exist是可以更快的。
    
-   not exists / not in : 使用not in 会内外表都全表扫描；使用not exists还能用上内表（子表）的索引。
    
-   复杂度 in : O(M * (N+1)/2) ， not in O(MN), EXIST O(M * 内表查询时间) ， not EXIST O(M * 内表查询时间)
    
-   in/not in 走的内存，如果内存装的下子表，用这个更快。exist / exist 对子表走B+树的查询，但是是基于磁盘的IO，如果B表很大，是占便宜的。
    

[MySQL:EXIST还是in?哪个性能好？为什么？ - 知乎 (zhihu.com)](https://zhuanlan.zhihu.com/p/110734978)


描述SQL语法

[EBNF（Extended Backus–Naur form）](https://link.zhihu.com/?target=https%3A//en.wikipedia.org/wiki/Extended_Backus%25E2%2580%2593Naur_form)

![[Pasted image 20230418041130.png]]