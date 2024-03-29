## 数据库原理与应用 第四十九讲 数据库安全概述

- 作者：**赵明心**
- 日期：**2019年8月23日**

---

## 五、数据库安全概述

数据库系统数据被破坏的原因可以总结成几个方面：

- 系统崩溃
- 由并发访问导致的数据一致性问题
- 人为损坏（刻意的或无意的，非法用户闯入数据库系统进行数据篡改、在数据库系统中不小心输入错误命令导致数据丢失等等）
- 数据输入过程错误，导致输入进去的数据本身不正确，出现了数据不一致。（没有遵循保持一致性原则，也即违背了ACID准则）

第四种错误举个例子，还是银行转账，100元从账户A转账到账户B，按照事务方式，A账户减少100之后B账户需要增加100，但是如果编程的时候出现错误，使得A减少100，B增加50，这就是编程导致的数据录入错误，从而影响了数据一致性。

在前面两点，可以被DBMS的并发控制和恢复机制来恢复。第三个问题是人为导致的数据库破坏，属于数据库安全问题。第四类问题属于数据库的完整性约束需要解决的问题。用户需要定义一些基于语义的完整性约束规则，从而阻止不符合语义约束的数据进入数据库。这是本章重点讲解的内容。

### 五（1） 保护数据不被非法访问

- 视图和查询重写
- 访问控制
  - 普通用户
  - 有资源特权的用户
  - DBA
- 用户标识（Identification）与识别（Authentication ）
  - 密码
  - 特殊物品，例如钥匙、IC卡等等
  - 个人特征：例如指纹、签名等等
- 授权
  - GRANT CONNECT TO JOHN IDENTIFIED BY xyzabc;（创建新用户JHON，并且授权基本访问权限，并赋予初始密码xyzabc）
  - GRANT SELECT ON TABLE S TO U1 WITH GRANT OPTION;（为数据库库中的用户U1以SELECT授权，则U1用户可以对S表进行SELECT，而其他插删改语句不可以做，最后的with grant option就是U1用户可以将查询权限转授给其他用户）
- 角色
- 数据加密
- Audit trail（审计追踪）
  - AUDIT SELECT, INSERT, DELETE, UPDATE ON emp WHENEVER SUCCESSFUL;

第一种方法，可以约束教务员只对某个视图进行访问，教务员无法得知基表的样子，保护不该访问的数据。对于某个具体用户来说，如果不希望其看到学生的基本信息，例如成绩等隐私信息。这个就可以定义一个视图，只对外显示学生基本信息。视图在一定程度上可以起到保护作用。除此之外还有查询修改的方法，比如有些不允许定义视图的数据库，当有教务员需要查询年龄大于20岁的学生时，如果只允许该教务员查看计算机系学生的情况，系统会自动添加计算机系的筛选条件，也可以保护数据库的数据。

第二种方法是对用户进行分类，第一类是普通用户，权限最低，未经授权的部分什么都不能做。第二类用户是具有资源特权用户，这类用户除了具备第一类用户的所有权利之外，还可以创建表、创建视图、创建簇集。这类用户具有创建数据对象的权限，并且对其他访问这些数据对象的用户授予权限。第三类用户相当于操作系统的管理员，可以对整个数据库进行重组、分配和创建新的用户，这个用户DBA非常重要，它直接影响到信息系统正常、稳定、高效的运行，DBA的作用在讲解下一章数据库设计的时候还要详细讲解。

第三种方式用户识别与验证，最简单的是使用密码对用户进行验证，通过口令识别用户，这样系统就认为是合法用户，系统可以允许访问。但是口令可能会被泄露或者被破解，一般要求口令不能太短或者添加一些特殊字符，而且很多系统会建议定期修改口令。除了密码识别以外，还可以通过一些物品识别，例如磁卡等，但是这些物品会有遗失风险。所以还有使用生物特征识别的方法，借助生物学信息，例如指纹、虹膜等。指纹可以复制，虹膜更难复制。我们现在的系统大部分都是通过密码识别的。

在用户识别基础上，控制合法用户的权限就需要授权机制，DBA等资源创建者在创建了数据对象之后可以授权其他访问用户。

