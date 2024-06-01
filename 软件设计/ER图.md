:::info
**前置知识**<br />概述：数据模型的基本概念<br />模型就是对现实世界特征的模拟和抽象，数据模型是对现实世界数据特征的抽象。对于具体的模型人们并不陌生，如航模飞机、地图和建筑设计沙盘等都是具体的模型。最常用的数据模型分为概念数据模型和基本数据模型。
:::
<a name="sUWJL"></a>
### 概念数据模型
概念数据模型也称为信息模型，是按用户的观点对数据和信息建模，是现实世界到信息世界的第一层抽象，强调其语义表达功能，易于用户理解，是用户和数据库设计人员交流的语言，主要用于数据库设计。这类模型中最著名的是**实体联系模型，简称E-R模型。**
<a name="wQAhS"></a>
### 基本数据模型
基本数据模型是按计算机系统的观点对数据建模，是现实世界数据特征的抽象，用于DBMS的实现，不同的数据模型具有不同的数据结构形式，目前最常用的数据结构模型有**层次 模型(Hierarchical Model)、网状模型(Network Model)、关系模型(Relational Model)和面向 对象数据模型(Objct Oriented Model)**。其中，**层次模型和网状模型统称为非关系模型**。非关系模型的数据库系统在20世纪70年代非常流行，在数据库系统产品中占据了主导地位。
<a name="DyLht"></a>
### E-R模型（什么是E-R图）
    概念模型是对信息世界的建模。所以概念模型能够方便、准确地表示信息世界中的常用概念。概念模型有很多种表示方法，其中最常用的是 P.P.S.Chen 于 1976 年提出的 实体-联系 方法（Entity Relationship Approach）。该方法用E-R图来描述现实世界的概念模型，称为实体-联系模型（Entity-Relationship Model，E-R模型）。<br />    E-R模型是软件工程设计中的一个重要方法，在数据库设计中，常用E-R模型来描述现实世界到信息世界的问题。因为它接近于人的思维方式，容易理解并且于计算机无关，所以用户容易接受，是用户和数据库设计人员交流的语言。但是， E-R模型只能说明实体间的语义联系，还不能进一步地详细说明数据结构。在解决实际应用问题时，通常应该先设计一个E-R模型，然后再将其转换成计算机能接受地数据模型。<br />E-R图中的主要构件：<br />![](https://cdn.nlark.com/yuque/0/2023/png/28163149/1698328194165-0a43e953-aed1-43c4-b596-2eb6e98cd627.png#averageHue=%23e4e4e4&clientId=ucfd600ab-242c-4&from=paste&id=ubfe08b08&originHeight=295&originWidth=756&originalType=url&ratio=1&rotation=0&showTitle=false&status=done&style=none&taskId=u3bc99968-1998-4001-8613-161a4ce6642&title=)
:::info
在E-R图中，实体集作为主码（或主键）的一部分属性名下面加 **下画线 **标明。另外，在实体集于联系的线段上标注联系的类型。
:::
![](https://cdn.nlark.com/yuque/0/2023/png/28163149/1698328227175-d1fc7c0b-d9b7-46c8-ab2b-01f2a29897f3.png#averageHue=%23292d34&clientId=ucfd600ab-242c-4&from=paste&id=uae3f0614&originHeight=229&originWidth=887&originalType=url&ratio=1&rotation=0&showTitle=false&status=done&style=none&taskId=u4fc7be41-d6b7-4dbf-b8a6-8a05bdea2c2&title=)实体（Entity） 在E-R图中实体用矩形表示，通常在矩形框内写明实体名。实体是现实世界中可以区别于其他对象的“事件”或“物体”。例如，企业中每个人都是一个实体。每个实体由一组特性（属性）来表示，其中的某一部分属性可以唯一标识实体，如职工号。实体集是具有相同属性的实体集合，例如，学校所有教师具有相同属性，因此教师的集合可以定义为一个实体集，学生具有相同的属性，因此学生的集合可以定义为另一个实体集。<br />联系（Relationship） 在E-R模型中，联系用菱形表示。通常可在菱形框内写明联系名，并用无向边分别与有关实体连接起来，同时在无向边旁标注上联系的类型(1:1、1:* 或 *：* )。实体的联系分为实体内部的联系和实体与实体之间的联系。实体内部的联系反映数据在同一记录内部各字段间的联系。
<a name="OSTuM"></a>
#### 两个不同实体之间的联系
两个实体之间的联系可分为3类:一对联系记为1：1，一对多联系记为1：*(或1:n)，多对多联系记为  *：*(m:n)。

-  1:1。如果对于实体集A中的每一个实体， 实体集B中至多有一个实体与之对应；反之亦然，则称A与B具有一对一联系。
-  1:*。如果对于实体集A中的每一个实体， 实体集B中有n个实体(n≥0)与 之对应；反之， 对于实体集B中的每一个实体，实体集A中至多只有一个实体与之对应，则称A与B具有一对多联系。
- *：*。如果对于实体集A中的每一个实体， 实体集B中有n个实体(n≥0)与 之对应；反之，对于实体集B中的每一个实体，实体集A 中也有m个实体(m≥0)与之对应，则称A与B具有多对多联系。

![](https://cdn.nlark.com/yuque/0/2023/png/28163149/1698328272070-a606e6e9-dcaa-4a70-b8e3-2e8631c16c4c.png#averageHue=%23f1f1f1&clientId=ucfd600ab-242c-4&from=paste&id=u9dac5bd9&originHeight=200&originWidth=313&originalType=url&ratio=1&rotation=0&showTitle=false&status=done&style=none&taskId=u77fc99b6-5f79-4c7e-b359-d3088a648bc&title=)
<a name="ys6Wr"></a>
#### **两个以上不同实体集之间的联系**
两个以上不同实体集之间存在 1：1：1、1：1：*、1：*：* 和 *：*：*的联系。
<a name="b9UQ3"></a>
#### ![](https://cdn.nlark.com/yuque/0/2023/png/28163149/1698328303808-d3cb23ea-56d6-4eea-a65d-e7e714120029.png#averageHue=%23f9f9f9&clientId=ucfd600ab-242c-4&from=paste&id=u42fc2ebe&originHeight=425&originWidth=1334&originalType=url&ratio=1&rotation=0&showTitle=false&status=done&style=none&taskId=u356cfbc1-88ac-4ad2-9d34-f63b1245f28&title=)<br /> 同一实体集内的二元联系
同一实体集内的各实体之间也存在 1：1、1：*和*：*的联系。<br />![](https://cdn.nlark.com/yuque/0/2023/png/28163149/1698328327949-c7fa6723-35b7-4399-8684-0764943ce62b.png#averageHue=%23f4f4f4&clientId=ucfd600ab-242c-4&from=paste&id=u721a77b1&originHeight=364&originWidth=612&originalType=url&ratio=1&rotation=0&showTitle=false&status=done&style=none&taskId=u3779dbf2-838e-4bad-b331-3be21a3f64f&title=)
<a name="j0Mip"></a>
### 属性（attribute）
 属性是实体某方面的特性。例如，职工实体集具有职工号、姓名、年龄、参加工作时间和通信地址等属性。每个属性都有其取值范围，如职工号为0001-9999的4位整型数，姓名为10位的字符串，年龄的取值范围为18-60等。在同一实体集中，每个实体的属性及其域是相同的，但可能取不同的值。E-R模型中的属性有如下分类。
<a name="Xa5xg"></a>
#### 简单属性和复合属性。
简单属性是原子的、不可再分的，复合属性可以细分为更小的部分(即划分为别的属性)。有时用户希望访问整个属性，有时希望访问属性的某个成分，那么在模式设计时可采用复合属性。例如，职工实体集的通信地址可以进一步分为邮编、省、市、街道。若不特别声明，通常指的是简单属性。
<a name="zguVR"></a>
#### 单值属性和多值属性。
前面所举的例子中，定义的属性对于一个特定的实体都只有单独的一个值。例如，对于一个特定的职工，只对应一个职工号、职工姓名，这样的属性叫作单值属性。但是，在某些特定情况下，一个属性可能对应一组值。例如，职工可能有0个、1个或多个亲属，那么职工的亲属的姓名可能有多个数目，这样的属性称为多值属性。
<a name="PS4xH"></a>
#### NULL属性。
当实体在某个属性上没有值或属性值未知时，使用NULL值。表示无意义或不知道。
<a name="rcPXD"></a>
#### 派生属性。
派生属性可以从其他属性得来。例如，职工实体集中有“参加工作时间”和“工作年限”属性，那么“工作年限”的值可以由当前时间和参加工作时间得到。这里，“工作年限”就是一个派生属性。年龄可从身份证号中得出，因此年龄也属于派生属性。
<a name="ZZdOf"></a>
### 扩充的 E-R 模型
    尽管基本的 E-R 模型是对大多数数据库特征建模，但数据库某些情况下的特殊语义，仅用基本的 E-R 模型无法表达清楚。所以引入了扩充的 E-R 模型，包括：弱实体、特殊化、概括和聚集等概念。<br />**弱实体 :即一个实体的存在必须以另一个实体为背景，将这类实体称为弱实体。在扩展的 E-R 图中，弱实体用双边矩形表示。**<br />![](https://cdn.nlark.com/yuque/0/2023/png/28163149/1698328452699-3f45c160-f033-4616-ad0a-695d022d39c4.png#averageHue=%23f3f3f3&clientId=ucfd600ab-242c-4&from=paste&id=L1iKy&originHeight=281&originWidth=1420&originalType=url&ratio=1&rotation=0&showTitle=false&status=done&style=none&taskId=ud0465fa8-c891-42a7-8827-a80cb8b3a09&title=)<br />**特殊化：某些实体在一方面具有一些共性，另一方面还具有各自的特殊性。**<br />![](https://cdn.nlark.com/yuque/0/2023/png/28163149/1698328496050-b7ac6e39-eeb2-4ee9-8d84-66167e4b1d62.png#averageHue=%232a3034&clientId=ucfd600ab-242c-4&from=paste&id=u8f58e252&originHeight=419&originWidth=616&originalType=url&ratio=1&rotation=0&showTitle=false&status=done&style=none&taskId=u06cd76fd-65a1-46ff-b044-126212be47b&title=)<br />这样，一个实体集可以按照某些特征区分为几个子实体。设有实体集E，如果 S 是 E 的某些真子集的集合，则称 S 是 E 的一个特殊化，E 是 S 的超类，S 是 E 的子类。图中，学生是超类，专科生、本科生和研究生是学生的子类。

- 如果，所有子集 S1、S2、....、Sn的并集等于 E，则称 S 是 E 的全特殊化，否则是 E 的部分特殊化。
- 如果，Si 与 Sj 的交集为空集，i ≠ j，则 S 是不相交特殊化，否则是重叠特殊化。
<a name="Z3O5B"></a>
####  扩充 E-R 图中的主要构件：
![](https://cdn.nlark.com/yuque/0/2023/png/28163149/1698328542264-76c045cc-b30a-4fe9-9a12-b54c4e29d2e4.png#averageHue=%23e7e7e7&clientId=ucfd600ab-242c-4&from=paste&id=uc25d2a7b&originHeight=543&originWidth=1457&originalType=url&ratio=1&rotation=0&showTitle=false&status=done&style=none&taskId=u63c20495-9322-499b-b0cb-6c01ea4088e&title=)<br /> 
