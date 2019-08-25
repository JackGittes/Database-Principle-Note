## 数据库原理与应用 第五十二讲 数据库的更新和引用完整性

- 作者：**赵明心**
- 日期：**2019年8月25日**

---

以上的第一个约束，实际上是要求$r_2$在属性$\alpha$上的投影需要属于关系$r_1$在$K1$的投影。因此插入操作必须保证插入元组引用的属性$\alpha$必须在$r_1$关系的$K1$中。同样地，在删除的时候，也需要满足一些类似的关系，删除$r_1$中的元组的时候必须查找在对应的$r_2$关系中是不是存在着对被删除元组属性的引用。

当发现被删除元组在$r_2$中存在引用的时候有可能采取两种操作，一种是拒绝用户删除，一种是级联删除，级联删除不仅要删除$r_1$中的元组，还要删除对应的$r_2$中的元组。在缺省情况下，大部分系统都是按照报错处理，如果需要指定级联删除，就需要显式告知系统。

在ACCESS中，有一个“**关系**”功能，可以查看几张表之间的引用关系，在连线关系上进行双击的时候，就可以允许用户进行定义引用关系是一对一还是一对多，同时允许用户设置级联删除和级联更新。如果不进行设置级联删除和级联更新，系统在遇到引用完整性问题的时候就会报错处理。思考一下存在多重的级联关系，很复杂情况下会出现什么样的情况？

更新操作也是类似的，当我们对$r_2$中的$t_2$更新的时候，更新操作会修改外键$\alpha$，类似插入操作的检测就会执行。用来判断是不是更新后违反引用完整性。同样地，当我们对$r_1$关系中的元组$t_1$进行更新的时候也需要检查引用的$r_2$是否也需要修改。也存在级联更新问题。

### **5.1.3 数据完整性定义**

- 完整性约束由程序指定
  - 编写应用程序的时候程序负责检查完整性约束，这样做就没有利用DBMS的功能
- 使用DBMS提供的断言
  - 定义一个断言说明语句，并且由DBMS进行自动检查
    - ASSERT balanceCons ON account: balance>=0;
    - 以上语句检查在account表中balance需要大于0，并且命名这个断言为balanceCons。
- 创建表的时候，利用CHECK子句定义完整性约束。

#### 通用约束
