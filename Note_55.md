## 数据库原理与应用 第五十五讲 数据库设计

- 作者：**赵明心**
- 日期：**2019年8月28日**

---

## 六 数据库设计

数据库设计在大四课程会专门讲授，尤其是在关系模式规范化理论方面，定理、算法、证明上讲的很多。关系模式形式化理论里有很多定理、证明，但是学习理论化的东西需要明白对于数据库设计有什么指导意义，这门课不着重于讲解定理证明，而是讲解其关系模型规范化理论对于实际的指导作用，讲解一些结论性的东西，结论性的原理、原则的使用。

想要设计一个结构比较好的数据库，需要充分理解应用的需求，理解应用需求需要很好地理清楚数据之间的关系。数据不是孤立存在的，存在很多内在关系，我们统称为数据依赖关系。

- 一些属性之间是存在相互依赖关系的，数据间的相互依赖关系就是数据依赖，这里面最重要的是函数依赖，简称FD（function dependency）。一个属性或者一组属性的值可以决定另外一组属性的值，这样的属性就叫存在**函数依赖关系**，例如在学生信息表里面，学号属性可以决定其他各个属性的值，其他属性值**函数依赖于**学号。这是数据库系统中最常见的依赖关系。
- 多值依赖（multi valued dependency）：简单来说，存在一张表格$R$，表明某个教师在不同学校兼课信息的表。表格中有教师的姓名、教师教授的课程的名称，在实际应用中，如果需要为某个学校开发这样的表格，通过需求分析，发现存在这样一种事实 —— 某个老师如果兼课，无论在哪个学校上课都只上物理课和化学课。或者反过来，某个老师上物理课的时候是在一中和三中，上化学课也在一中和三中，如果有这样的关系，我们就说在属性之间存在多值依赖，$N\rightarrow \rightarrow C$，教师一旦确定，他所教授的几门课程就确定了。或者反过来，只要确定了姓名，就可以确定他所兼职的学校的一组值。这样的话，函数依赖其实就是多值依赖的一个特例。函数依赖是确定属性的一个值，多值依赖在实际应用中是存在的，但是不多。从数据库设计的角度来讲，它作为数据依赖的一种应该进行研究，但实际设计的时候基本不需要考虑。
- 连接依赖（join dependency）：关系的属性之间存在无损连接分解的关系。最典型的是供应商、零件、工程之间的关系。例如一张表格是由供应商编号、零件编号、工程编号组成。$SPJ(S^{\circ},P^{\circ},J^{\circ})$，这种表通过投影操作投影到三组属性上分别是$SPJ[S^{\circ},P^{\circ}],SPJ[P^{\circ},J^{\circ}],SPJ[S^{\circ},J^{\circ}]$，分解成三个子表之后再使用连接操作连接起来，如果能够完完全全还原第一张表的话，元组数一个不多、一个不少，我们就称这个表的分解是一个无损连接分解。可以自行实验，自己设计一个表格，然后随便填写一些数据，把属性投影拆分成三个小表格，看是否能还原，会发现在多数情况下无法还原，很多时候会缺少元素，少数时候会多一些元素。这中间有很高的要求，在书上判断表格是否可以进行无损拆分有一个算法。
  - > 如果能实现无损连接，我们就称三个属性$S,P,J$之间存在连接依赖，实际应用中，很少存在连接依赖。一个具体应用例子的无损连接分解，例如我们需要建设一个工程，需要采购桌椅，假如在建设项目中，我们需要讲台，第二件事情是和南京某个公司A有招投标关系，第三件事情是A公司生产讲台。那么这样就构成了连接依赖关系。也即，我们需要某些产品，我们与A公司有招投标关系，我们需要的该产品A公司生产。不过这种情况在实际应用中出现的也很少。实际数据库设计的时候重点考虑的仍然是函数依赖，不会重点考虑连接依赖。