## 数据库原理与应用 第四十八讲 死锁检测与预防

- 作者：**赵明心**
- 日期：**2019年8月23日**

---

#### **死锁避免**

这是第二类防的方法，防止系统中的循环等待。
- 在事务的一开始就申请所有的锁
- 按照特定的顺序申请锁
- 一旦冲突就放弃

前两种是操作系统中常用的方法，第一个是在程序运行前就申请所有需要的锁，这样已经获取了所有需要的资源，之后就不会出现循环等待。第二种方法是资源排序，我们可以对PC的资源进行排序，例如要用到扫描仪和打印机，扫描仪作为第一号资源，打印机作为第二号资源，这样可以保证运行的程序在竞争资源的时候不会发生循环等待，这两种方法在数据库系统中并不适用。

例如在数据库系统中，我们还没有讨论封锁粒度的问题（研究生阶段会讨论加锁的粒度，之前讨论的数据对象加锁，没有说是表、元组还是哪个属性）。我们假定对表进行加锁，在一个数据库系统中，用户创建的表可能有几千个，事务在运行的时候，可能需要访问很多表，这个时候锁住所有资源不太现实。第二种对资源排序在数据库系统中也不现实，即便是以单粒度仅仅对表加锁的话，也是不现实的。再到元组一级加锁就更难了，很难对所有元组进行排序。

第三种方法是一旦出现冲突就放弃，不等待，只要任何两个事务出现了冲突不等待直接放弃，这种方式其实也是理论可行但不实用。

在数据库系统中，真正使用的是最后一种，它给每个运行的事务安排一个时间戳，这个时间戳$TS$作用有两个，一个作用是作为$TID$，即事务的编号，用于区别不同事务。第二个作用是用时间戳比较两个事务的年龄。时间戳就是一个事务的生日，越小的事务说明运行的越早。当某个事务$TA$需要对某个数据对象申请锁的时候，如果锁已经被$TB$占有，那么此时$TA$有两种选择。
- wait-die：等待死亡法，只要$TA$年龄比$TB$年龄大就会等待，否则$TA$放弃，并自动滚回重新运行并以原来的时间戳运行，这种方法能不能避免死锁？按照等待死亡的方法，系统中只会出现年老等待年轻的事务，年轻的事务会等待更加年轻的事务，这样系统中的等待关系一定是单向的，不可能出现循环等待。这样就不可能出现死锁。这样解决了死锁问题，那么会不会出现活锁呢？如果一个事务不停重复运行的时候，年轻事务会不会反复被KILL掉，一直进入等待状态。我们注意到比$TA$年轻的事务总是有限多个的，$TA$如果是年轻的，那么$TA$在abort几次之后，$TA$总会变成最老的事务，这样就不会出现一直放弃的情况，从而避免了活锁。
- wound-wait:击伤等待法。击伤等待法是反过来，当$TA$发现自己是年轻的时候才等待，否则就去抢占$TB$的资源，使得$TB$被KILL，$TB$滚回，之后重新运行。这种方式使得年轻事务等待年老的事务，年老事务等待更年老的事务。还是单方向的不会出现循环等待，而对于活锁来说也是一样的，年轻的事务只要一直等待下去总会成为年老的事务，以至于自己不会被更年长的事务KILL掉，避免了活锁。

总结一下就是，在上面讲的两种策略“等待死亡法”和“击伤等待法”中，等待关系都是单方向的，不会出现死锁，被滚回事务会自动重新运行且不改变时间戳，因而也不会出现活锁。除此之外，封锁法只是保证可串行化的一类方法，还有时间戳方法等等，封锁法是最常用的方法，其他方法需要在研究生课程中学习。

并发控制就讲这些，在研究生课程中还会讲解多粒度封锁、隐含锁问题、幽灵现象等等。这门课程只需要了解并发控制的基本原理和解决锁问题的基本机制。
