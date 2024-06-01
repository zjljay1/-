<a name="zfsiR"></a>
## 什么是UML
UML是**Unified Model Language**的缩写，中文是**统一建模语言**，是由一整套图表组成的标准化建模语言。<br />你可能会问：这明明是一种图形，为什么说是语言呢?伟大的汉字还不是从图形(象形文字)开始的吗?语言是包括文字和图形的!其实有很多内容文字是无法表达的，你见过建筑设计图纸吗?里面还不是很多图形，光用文字能表达清楚建筑设计吗?在建筑界，有一套标准来描述设计，同样道理，在软件开发界，我们也需要一套标准来帮助我们做好软件开发的工作。UML就是其中的一种标准，注意这可不是唯一标准，只是UML是大家比较推崇的一种标准而已，说不定以后有一个更好的标准可能会取代她呢!UML并不是强制性标准，没有法律规定你在软件开发中一定要用UML，不能用其它的，我们的目标是善用包括UML在内的各种标准，来提高我们软件开发的水平。<br />UML由1.0版发展到1.1、1.2、...，到现在的2.0、2.x，本书将会以2.x版本为基础开展讨论。网络上、书籍、还有各种UML工具软件，各自基于的UML版本可能会不一样，大家在学习过程中可能会有一些困惑，不过没关系，本文章在某些关键地方会描述1.x与2.x的差异。 
<a name="xwrTM"></a>
## 为什么要用UML？
通过使用UML使得在软件开发之前， 对整个软件设计有更好的可读性，可理解性，从而降低开发风险。同时，也能方便各个开发人员之间的交流。<br /> UML提供了极富表达能力的建模语言，可以让软件开发过程中的不同人员分别得到自己感兴趣的信息。
:::info
Page-Jones 在《Fundamental Object-Oriented Design in UML》 一书中总结了UML的主要目的，如下：

1. 为用户提供现成的、有表现力的可视化建模语言，以便他们开发和交换有意义的模型。
2. 为核心概念提供可扩展性 (Extensibility) 和特殊化 (Specialization) 机制。
3. 独立于特定的编程语言和开发过程。
4. 为了解建模语言提供一个正式的基础。
5. 鼓励面向对象工具市场的发展。
6. 支持更高层次的开发概念，如协作，框架，模式和组件。
7. 整合最佳的工作方法 (Best Practices)。
:::
<a name="yxgSZ"></a>
## UML图有哪些

- UML图分为结构图和行为图。
- 结构图分为类图、轮廓图、组件图、组合结构图、对象图、部署图、包图。
- 行为图又分活动图、用例图、状态机图和交互图。
- 交互图又分为序列图、时序图、通讯图、交互概览图。

![](https://cdn.nlark.com/yuque/0/2023/png/28163149/1698325809653-071183ac-3668-4932-8bdb-26e011162d99.png#averageHue=%23fcfcfc&clientId=u26c66cba-d84e-4&from=paste&id=ue6642d34&originHeight=1096&originWidth=1728&originalType=url&ratio=1&rotation=0&showTitle=false&status=done&style=none&taskId=ubf1ea3ac-5453-498d-9949-c344bcf97eb&title=)
<a name="npTFj"></a>
## UML图概览
![](https://cdn.nlark.com/yuque/0/2023/png/28163149/1698325845860-e474e921-107a-464a-bba4-3e2c766d0a87.png#averageHue=%23f4f3c3&clientId=u26c66cba-d84e-4&from=paste&id=ud117adfa&originHeight=846&originWidth=2000&originalType=url&ratio=1&rotation=0&showTitle=false&status=done&style=none&taskId=uc38e15d8-5ab1-4275-be8b-dcaedd485fe&title=)
<a name="zZ7w6"></a>
## 什么是类图

- 【概念】 类图是一切面向对象方法的核心建模工具。类图描述了系统中对象的类型以及它们之间存在的各种静态关系。
- 【目的】用来表示类、接口以及它们之间的静态结构和关系。

在类图中，常见的有以下几种关系。
<a name="o85KW"></a>
#### 泛化（Generalization）

- 【泛化关系】是一种继承关系，表示子类继承父类的所有特征和行为。
- 【箭头指向】带三角箭头的实线，箭头指向父类。

![](https://cdn.nlark.com/yuque/0/2023/png/28163149/1698325910991-16d117fa-4271-42de-b997-0a0df0ef388d.png#averageHue=%23eaead5&clientId=u26c66cba-d84e-4&from=paste&id=ud1bb70d2&originHeight=222&originWidth=672&originalType=url&ratio=1&rotation=0&showTitle=false&status=done&style=none&taskId=uf2358e5a-e3e4-4e85-926e-7a982be6d49&title=)<br /> 
<a name="HKqno"></a>
#### 实现（Realization）

- 【实现关系】是一种类与接口的关系，表示类是接口所有特征和行为的实现。
- 【箭头指向】带三角箭头的虚线，箭头指向接口。

<br /> ![](https://cdn.nlark.com/yuque/0/2023/png/28163149/1698325936707-2807a9cd-f845-4369-9173-123aebf625b5.png#averageHue=%23e4e4cd&clientId=u26c66cba-d84e-4&from=paste&id=u42bf61f7&originHeight=176&originWidth=650&originalType=url&ratio=1&rotation=0&showTitle=false&status=done&style=none&taskId=ua650ccb8-02da-4c00-a469-c49be632f12&title=)
<a name="ZOaqV"></a>
#### 关联（Association）

- 【关联关系】是一种拥有关系，它使得一个类知道另一个类的属性和方法。
- 【代码体现】成员变量
- 【箭头指向】带普通箭头的实线，指向被拥有者。双向的关联可以有两个箭头，或者没有箭头。单向的关联有一个箭头。![](https://cdn.nlark.com/yuque/0/2023/png/28163149/1698326439973-e94ac96f-d9f3-47a1-86c1-53c38ad37eba.png#averageHue=%23e6e6ce&clientId=u26c66cba-d84e-4&from=paste&id=ua56b3a2e&originHeight=176&originWidth=720&originalType=url&ratio=1&rotation=0&showTitle=false&status=done&style=none&taskId=u78296ee5-693a-46ab-bc2b-5145c6668a8&title=)自己买的车，想什么时候开就开。但是车是车，人是人，没有整体与部分的关系。
<a name="XcScH"></a>
#### 聚合（Aggregation）

- 【聚合关系】是一种整体与部分的关系。且部分可以离开整体而单独存在。聚合关系是关联关系的一种，是强的关联关系；关联和聚合在语法上无法区分，必须考察具体的逻辑关系。
- 【代码体现】成员变量
- 【箭头指向】带空心菱形的实线，空心菱形指向整体。![](https://cdn.nlark.com/yuque/0/2023/png/28163149/1698326474715-bf280224-4114-4718-bf73-4407aeab23ed.png#averageHue=%23e1e1c9&clientId=u26c66cba-d84e-4&from=paste&id=ub07ed403&originHeight=166&originWidth=710&originalType=url&ratio=1&rotation=0&showTitle=false&status=done&style=none&taskId=uf6525fe5-a7b9-47f9-8d10-81f582ce4cc&title=)电脑有键盘才能输入信息，电脑是整体，键盘是部分，键盘也可以离开电脑，单纯的拿去敲。所以是聚合。
<a name="QojCP"></a>
#### 组合（Composition）

- 【组合关系】是一种整体与部分的关系。但部分不能离开整体而单独存在，组合关系是关联关系的一种，是比聚合关系还要强的关系。
- 【代码体现】成员变量
- 【箭头指向】带实心菱形和普通箭头的实线，实心菱形指向整体。

<br /> ![](https://cdn.nlark.com/yuque/0/2023/png/28163149/1698326554510-af54a843-06a3-4035-99ca-6b960c3c442b.png#averageHue=%23e3e3ce&clientId=u26c66cba-d84e-4&from=paste&id=uacb7b844&originHeight=170&originWidth=560&originalType=url&ratio=1&rotation=0&showTitle=false&status=done&style=none&taskId=ubbde36ac-acb9-4abe-8402-3ba22adde3f&title=)<br />鸟是整体，翅膀是部分。鸟死了，翅膀也就不能飞了。所以是组合。我们再看一下，下面的一组经典的聚合组合关系的例子。<br />![](https://cdn.nlark.com/yuque/0/2023/png/28163149/1698326636278-b8c6d9c1-13fc-4255-88ca-f0f29cd43cab.png#averageHue=%23e9e9e9&clientId=u26c66cba-d84e-4&from=paste&id=ufc6b1b4f&originHeight=294&originWidth=502&originalType=url&ratio=1&rotation=0&showTitle=false&status=done&style=none&taskId=u8523bc89-33d5-4fb3-94dc-59f46989728&title=)<br />一个公司拥有多个部门，公司和部门之间是组合关系，公司破产了，部门就不复存在了。部门和员工是聚合关系，部门被裁掉，员工就换下家了。
<a name="Sat0q"></a>
#### 依赖（Dependency）

- 【依赖关系】是一种使用关系，即一个类的实现需要另一个类的协助。
- 【箭头指向】带普通箭头的虚线，普通箭头指向被使用者。![](https://cdn.nlark.com/yuque/0/2023/png/28163149/1698326663655-800570d1-6bb2-4fc9-8e70-211ea0821a2b.png#averageHue=%23eaead1&clientId=u26c66cba-d84e-4&from=paste&id=ShpAS&originHeight=190&originWidth=856&originalType=url&ratio=1&rotation=0&showTitle=false&status=done&style=none&taskId=ub5871dd4-3e15-4a9f-b8a3-154c6a9ab65&title=)
- 老司机只管开车，车是谁的不重要，给什么车开什么车。
<a name="WO0MJ"></a>
## 什么是组件图？

- 【概念】描绘了系统中组件提供的、需要的接口、端口等，以及它们之间的关系。
- 【目的】用来展示各个组件之间的依赖关系。![](https://cdn.nlark.com/yuque/0/2023/png/28163149/1698326695318-7324175b-c0a3-4eb8-b5f1-1fef123bad2d.png#averageHue=%23f7f7f7&clientId=u26c66cba-d84e-4&from=paste&id=u7a314f11&originHeight=626&originWidth=1298&originalType=url&ratio=1&rotation=0&showTitle=false&status=done&style=none&taskId=u0d114521-228e-4542-8153-f13e3ef8304&title=)订单系统组件依赖于客户资源库和库存系统组件。中间的虚线箭头表示依赖关系。另外两个符号，表示组件连接器，一个提供接口，一个需要接口。
<a name="JW0wE"></a>
## 什么是[部署图](https://so.csdn.net/so/search?q=%E9%83%A8%E7%BD%B2%E5%9B%BE&spm=1001.2101.3001.7020)？

- 【概念】描述了系统内部的软件如何分布在不同的节点上。
- 【目的】用来表示软件和硬件的映射关系。![](https://cdn.nlark.com/yuque/0/2023/png/28163149/1698326770774-94b306f7-9352-40fe-8855-8e275923797b.png#averageHue=%23f8f8f8&clientId=u26c66cba-d84e-4&from=paste&id=ub6321735&originHeight=652&originWidth=1092&originalType=url&ratio=1&rotation=0&showTitle=false&status=done&style=none&taskId=u8cb8fc6b-8df4-4892-b4b8-1baec75e48a&title=)图中简单的表示，不同机器上面部署的不同软件。
<a name="odiZU"></a>
## 什么是对象图？

- 【概念】对象图是类图的一个实例，是系统在某个时间点的详细状态的快照。
- 【目的】用来表示两个或者多个对象之间在某一时刻之间的关系。

![](https://cdn.nlark.com/yuque/0/2023/png/28163149/1698326784466-38c3b700-562c-4008-b570-901611655bad.png#averageHue=%23f3f2e4&clientId=u26c66cba-d84e-4&from=paste&id=u2f1d2af9&originHeight=552&originWidth=992&originalType=url&ratio=1&rotation=0&showTitle=false&status=done&style=none&taskId=u4b59e2e1-0e87-4687-96eb-b80574c0ff0&title=)<br /> 图中就是描述的，某时间点bat这个公司有一个研发部，一个销售部，两个部门只有一个人iisheng。
<a name="IOXWx"></a>
## 什么是包图？

- 【概念】描绘了系统在包层面上的结构设计。
- 【目的】用来表示包和包之间的依赖关系。
- ![](https://cdn.nlark.com/yuque/0/2023/png/28163149/1698326827098-645ffc9d-3dc0-4cc0-b882-03d4ddef4f30.png#averageHue=%23f5f5f5&clientId=u26c66cba-d84e-4&from=paste&id=uFbKY&originHeight=856&originWidth=992&originalType=url&ratio=1&rotation=0&showTitle=false&status=done&style=none&taskId=u32bb6de1-9cc2-4ffc-b8a9-dba53ecb418&title=)
- 《Use》关系表示使用依赖，Web Shopping依赖Payment
- 《Merge》关系表示合并，Web Shopping合并了Shopping Cart就拥有了Shopping Cart的功能
- 《Access》关系表示私有引入，比如代码中的指定包名类名
- 《Import》关系表示公共引入，比如Java中的import之后，就可以直接使用import包中的类了。
<a name="ZhvJj"></a>
## 什么是组合结构图？

- 【概念】描述了一个"组合结构"的内部结构，以及他们之间的关系。这个"组合结构"可以是系统的一部分，或者一个整体。
- 【目的】用来表示系统中逻辑上的"组合结构"。<br />![](https://cdn.nlark.com/yuque/0/2023/png/28163149/1698326870997-4e1b16c6-1a47-4579-ba87-fbd5aec941a0.png#averageHue=%23fcfcca&clientId=u26c66cba-d84e-4&from=paste&id=uf3310f85&originHeight=600&originWidth=952&originalType=url&ratio=1&rotation=0&showTitle=false&status=done&style=none&taskId=udde7ce19-b647-497f-a551-1a63e0bfa6c&title=)图中描述了Car是由车轴连接着的两个前面轮子、两个后面轮子，和引擎组合的。
<a name="V6ZHg"></a>
## 什么是轮廓图？

- 【概念】轮廓图提供了一种通用的扩展机制，用于为特定域和平台定制UML模型。
- 【目的】用于在特定领域中构建UML模型。

![](https://cdn.nlark.com/yuque/0/2023/png/28163149/1698326902049-3e8cdf23-db13-494c-871d-b07b5ed76e04.png#averageHue=%23f0f0e1&clientId=u26c66cba-d84e-4&from=paste&id=u736f33aa&originHeight=552&originWidth=988&originalType=url&ratio=1&rotation=0&showTitle=false&status=done&style=none&taskId=ua21681fe-ab34-4c7c-af7e-5a39d03bac9&title=)<br />  图中我们定义了一个简易的EJB的概要图。Bean是从Component扩展来的。Entity Bean和Session Bean继承了Bean。EJB拥有Remote和Home接口，和JAR包。
<a name="QVbOb"></a>
## 什么是用例图？

- 【概念】用例图是指由参与者、用例，边界以及它们之间的关系构成的用于描述系统功能的视图。
- 【目的】用来描述整个系统的功能。

![](https://cdn.nlark.com/yuque/0/2023/png/28163149/1698326919484-b20c35bd-e3dc-4586-886b-1a2ead40986d.png#averageHue=%23f7f7f7&clientId=u26c66cba-d84e-4&from=paste&id=u6564eb47&originHeight=718&originWidth=1168&originalType=url&ratio=1&rotation=0&showTitle=false&status=done&style=none&taskId=u913754fc-7b40-4859-971a-989a55ab815&title=)用例图中包含以下三种关系：

- 包含关系使用符号《include》，想要查看订单列表，前提是需要先登录。
- 扩展关系使用符号《extend》，基于查询订单列表的功能，可以增加一个导出数据的功能
- 泛化关系，子用例继承父用例所有结构、行为和关系。
<a name="MPnk1"></a>
## 什么是活动图？

- 【概念】描述了具体业务用例的实现流程。
- 【目的】用来表示用例实现的工作流程。

![](https://cdn.nlark.com/yuque/0/2023/png/28163149/1698327005678-7f2a0676-506b-4168-9f9f-8d7f5f36cf75.png#averageHue=%23eeeeee&clientId=u26c66cba-d84e-4&from=paste&id=u21288abe&originHeight=204&originWidth=808&originalType=url&ratio=1&rotation=0&showTitle=false&status=done&style=none&taskId=ua2691fa1-4881-4249-a31b-d77b85cc347&title=)图中简单描述了，从开始到登录到查看订单列表，或者登录失败直接结束。
<a name="xiVed"></a>
## 什么是状态机图？

- 【概念】状态机图对一个单独对象的行为建模，指明对象在它的整个生命周期里，响应不同事件时，执行相关事件的顺序。
- 【目的】用来表示指定对象，在整个生命周期，响应不同事件的不同状态

<br /> ![](https://cdn.nlark.com/yuque/0/2023/png/28163149/1698327608926-55fbfd52-391a-45e5-8561-0c1788402cf4.png#averageHue=%23f2f2f2&clientId=u26c66cba-d84e-4&from=paste&id=ua9d21fdc&originHeight=340&originWidth=736&originalType=url&ratio=1&rotation=0&showTitle=false&status=done&style=none&taskId=u67acc9e1-80a3-4be1-94f1-9829b7d7107&title=)图中描述了，门在其生命周期内所经历的状态。
<a name="DDDbu"></a>
## 什么是序列图？

- 【概念】序列图根据时间序列展示对象如何进行协作。它展示了在用例的特定场景中，对象如何与其他对象交互。
- 【目的】通过描述对象之间发送消息的时间顺序显示多个对象之间的动态协作。![](https://cdn.nlark.com/yuque/0/2023/png/28163149/1698327632885-4d391df3-1091-46d9-aed2-ec1f2f81f9cb.png#averageHue=%23f4f4f4&clientId=u26c66cba-d84e-4&from=paste&id=u34636e37&originHeight=1150&originWidth=1366&originalType=url&ratio=1&rotation=0&showTitle=false&status=done&style=none&taskId=u098a42cf-5caf-46e8-89d2-fae2d7ac51f&title=) 图中展示的是支付宝条码支付场景的序列图。其中，loop是循环，alt是选择，序列图的其他关系这里就不介绍了。
<a name="tP0uJ"></a>
## 什么是通讯图？

- 【概念】描述了收发消息的对象的组织关系，强调对象之间的合作关系而不是时间顺序。
- 【目的】用来显示不同对象的关系。![](https://cdn.nlark.com/yuque/0/2023/png/28163149/1698327759924-d8eeee45-b4d2-40b4-9e36-becc39a10927.png#averageHue=%23f6f6f6&clientId=u26c66cba-d84e-4&from=paste&id=u6fa68fdf&originHeight=732&originWidth=1118&originalType=url&ratio=1&rotation=0&showTitle=false&status=done&style=none&taskId=ud4eb82bb-86a0-4418-8035-34497626348&title=) 图中展示了一个线上书店的通讯图，方框和小人表示生命线，不同生命线之间可以传递消息，消息前面的数字可以表达序列顺序。
<a name="isVCb"></a>
## 什么是交互概览图？

- 【概念】交互概览图与活动图类似，但是它的节点是交互图。
- 【目的】提供了控制流的概述。
- ![](https://cdn.nlark.com/yuque/0/2023/png/28163149/1698327784861-bb3ac5db-bc33-4d82-9adf-39499130b5cc.png#averageHue=%23f8f8f8&clientId=u26c66cba-d84e-4&from=paste&id=ubec7ff3e&originHeight=922&originWidth=1430&originalType=url&ratio=1&rotation=0&showTitle=false&status=done&style=none&taskId=u07c217f5-4f08-4d8e-b9f7-7580be142d8&title=)图中表示一个调度系统的交互概览图，跟活动图很像。其中sd的框代表具体的交互流程，ref框代表使用交互。
<a name="Wpsis"></a>
## 什么是时序图？

- 【概念】时序图被用来显示随时间变化，一个或多个元素的值或状态的更改。也显示时控事件之间的交互和管理它们的时间和期限约束。
- 【目的】用来表示元素状态或者值随时间的变化而变化的视图。![](https://cdn.nlark.com/yuque/0/2023/png/28163149/1698327804532-a67065ff-257b-48fc-824d-53569d2ca4f1.png#averageHue=%23fbfcc4&clientId=u26c66cba-d84e-4&from=paste&id=u3c53c999&originHeight=940&originWidth=1826&originalType=url&ratio=1&rotation=0&showTitle=false&status=done&style=none&taskId=uaf643a04-af95-4085-9d40-489518375f8&title=)图中展示了老年痴呆病人随着时间的变化病情的变化。
<a name="UMjHM"></a>
### 总结
学习UML，我们没必要纠结比如像聚合关系是带箭头还是不带箭头，这样的问题。更重要的是UML图所给我们带来的画图思想，让我们画UML图或者其他图能让其他人更好的理解我们的设计思想。


