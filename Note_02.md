## 数据库原理与应用	第二讲 引言（二）

- 作者：__赵明心__
- 日期：__2019年7月28日__

---

## 一、什么是数据库？

- 一个大规模的、集成的数据集合
- 建模真实世界中的应用单位
	- 实体（例如：学生、课程）
	- 关系（例如：选择）
- 一个数据库管理系统（DBMS）是一个被设计用来存储和管理数据的软件包

数据库可以被看成面向一个企业或应用单位的相关应用开发的数据的集合。现实世界中的企业用计算机管理和描述需要进行建模，数据库就是一种建模的结果，并以数据的形式存储下来，数据库中两类最重要的东西就是实体和关系。实体是现实世界中的实体，例如人、财、物等有形物以及课程等等无形物。客观实体之间存在大量的联系，这些联系也要存储在数据库当中，需要对联系进行描述。

第二章重点讲述的数据模型就是在讲解如何建模实体的关系。

## 二、文件与数据库系统的关系

我们在学习操作系统的时候已经学习过文件系统，我们在给一个企业进行数据处理的时候为什么不用文件来做呢，而是使用数据库系统？

- 操作系统中的文件就是字符流，不存在结构
- 应用必须在主存储和二级存储之间暂存大数据集
- 对于不同的查询有特定的代码
- 必须在并发访问的情况下保护数据的一致性
- 崩溃恢复
- 安全性和访问控制


操作系统中的文件操作就是创建文件、打开文件、读取、写入、调整文件的读写指针等等。文件系统并没有提供很强的数据管理功能，如果使用文件来进行数据管理的话，文件系统提供的管理功能非常初级。以上的这些需求如果使用文件系统来实现的话就需要编写大量的代码，而在DBMS中不需要管，只需要向DBMS来查询、使用就可以。DBMS还会接管并发管理以及数据恢复的问题以确保数据的正确性。同时，有些敏感数据在文件系统中只有一些简单的权限控制，但这种权限控制的粒度是很粗的，在DBMS中可以进行更加精细的控制，这就是在文件系统和DBMS中进行对比。

早期数据管理确实是在文件系统中进行的，随着数据变得复杂、数据关系变得复杂，越来越多的数据管理是由数据库管理系统来完成的。

要注意的是，数据库系统就是建立在操作系统的文件系统之上的，数据库来实现复杂的数据管理。

## 三、为什么使用DBMS？

- 数据独立且有效访问
- 减少应用开发时间
- 数据完整性和安全性
- 统一数据管理
- 并发访问、崩溃恢复

为什么使用数据库就很好理解了。数据库可以减少应用开发时间，大量对文件的基础和共性操作由DBMS接管，比直接基于文件系统来做要快捷的多。

## 四、为什么学习和研究数据库？

信息社会的核心就是计算机，用计算机进行信息的管理，这时就涉及到大量的数据管理问题，这里有很多数据管理的问题，除了我们熟悉的企事业单位中的管理问题、选课管理还有很多其他的数据管理需求。

- 从计算到信息
	- 低端层面：网络上存在大量混乱的数据，这些数据需要进行有效的管理，任其泛滥就是垃圾，需要管理
	- 高端层面：各种科研项目都会有大量的数据产出，例如核能、高能物理等仪器产生海量数据
	
- 数据集在多样性和规模上急剧膨胀
	- 数字图书馆、交互式视频、人类基因库、EOS项目...
	- 对数据库管理系统的需求

- 数据库管理系统的实现包含了计算机技术发展的绝大多数成果


DBMS可以作为学习计算机的一种范例，计算机科学中的很多技术都可以应用在DBMS中。DBMS需要编译，首先就是对SQL进行编译。

## 五、数据、数据模型、数据模式
	
### 5.1 数据是描述现实世界的符号，是信息的存在形式
	
- 整型数、人的姓名、桌子的形状等等都可以用符号性描述在计算机里表示出来，例如用长宽高来描述一张方形桌子。
	
### 5.2 数据模型是描述数据的一组概念和定义
	
- 数据模型可以理解为数据结构，就是用来描述现实世界的一种方法。不同的数据模型就是用了不同的方法来描述现实世界。
	
### 5.3 数据模式使用给定数据模型对特定数据进行描述产生的结果
	
	
数据模式可以看作是某种编程语言，而编程的结果就是使用编程语言编写的结果。