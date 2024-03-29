## 数据库原理与应用 第四十讲 连接操作的实现算法

- 作者：**赵明心**
- 日期：**2019年8月20日**

---

### **4.4.2 操作优化（续）**

可以在系统中申请两个缓冲区，两个缓冲区分别存储内循环关系和外循环关系的物理块，使用外循环关系的元组进行比较的时候遍历的是内存中的内循环关系的元组。取进内存的元组仍然是两重循环两两比较。

这样虽然算法逻辑没有改进，但是减少了IO次数，每次扫描的时候是扫描物理块，对于外循环关系来说，读取磁盘的次数减少了。每次读取一个物理块，减少了读取硬盘上物理块的次数。为什么不把外循环关系全都取到内存当中呢？

多余内存可能被DBMS申请用来进行提高执行效率，通过内存缓冲区的使用，大大提高效率。如果机器的内存足够大，之前是只把内存缓冲区给了外循环关系，那么是否可以把多余内存给内循环关系？多余缓冲区给内循环关系有帮助吗？

#### **归并扫描**

参与连接的两个关系，事先做好了外排序。参与连接的关系$R$和$S$，事先做好外排序，之后连接的时候，只需要打开各自的关系，比较连接属性值，关系$R$的属性值大，关系$S$的属性值小，可以直接判断指针的移动方向和移动哪个指针。正是执行了外排序，所以可以直接使用归并的方式来进行连接。注意归并扫描其实也是读取的物理块。

事先没有做好外排序的话，就需要考虑排序的代价。

#### **使用B+树索引**

在实际的关系型数据库中，实现两个关系连接使用最多的方式。因为要求两个关系做好外排序并不一定能实现，如果两个关系更新非常频繁，那就总是需要维护外排序，代价会比较大。在关系型数据库中，最常见的是存储成堆文件或者簇集并建立B+树索引。如果我们连接两个关系，并发现在关系$S$的属性上存在B+树索引，这个时候可以借助嵌套循环的思想，使得没有索引的关系$R$作为外循环关系，有索引的关系属性作为内循环关系。

之前是借助扫描的方式寻找匹配元组，但是现在内循环关系的连接属性上有索引，这个时候就不需要进行扫描，而可以直接查询索引，查询索引可以直接找到地址，得到地址之后就可以直接取出元组完成连接。B+树索引的方式就是在嵌套循环的基础之上寻找匹配元组，避免了对内循环关系进行顺序扫描。

不过这里也会遇到一个跟物理层一样的问题，在关系$S$上的某个属性上有索引，可以利用索引来匹配元组，但是如果在某个属性上，属性值的重复数量达到了关系的所有元组的20%以上的话，实际用索引是不合算的。因为跟属性值匹配的个数很多的话，意味着能匹配上的元组散布在很多物理块里，意味着匹配过程需要取重复物理块，用索引反倒不合适。

#### **HASH join**

因为一般的自然连接所作的是在两个关系的公共属性上，按照公共属性的值进行连接，那么两个关系公共属性的值域是一样的，这个时候可以在公共属性值域上面定义一个哈希函数，把关系$R$和关系$S$散列到一个哈希文件当中，这样将来再进行匹配的时候，两个关系的属性只要相等就可以散列到同一个桶当中，所以如果两个关系的某个属性并不会频繁更新的话，就可以使用散列的方式进行处理。这样就不需要进行嵌套循环和归并扫描，所有具有相同属性的匹配元组都会出现在一个桶当中。其基本思路就是这样。

### 4.4 总结

关系型数据库就是在查询优化上取得了突破取代了层次和网状模型。