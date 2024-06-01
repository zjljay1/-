<a name="xiF7f"></a>
## Maven快速上手
![](https://cdn.nlark.com/yuque/0/2023/webp/28163149/1691333026853-865848f4-3e37-402a-be3d-5a22f00a87a7.webp#averageHue=%23e8e8e8&clientId=u237dd497-258d-4&from=paste&id=u06845826&originHeight=525&originWidth=927&originalType=url&ratio=1.375&rotation=0&showTitle=false&status=done&style=none&taskId=ub5b7939c-9f02-4be7-b029-707782a5bf3&title=)<br />Maven是专门用于构建、管理Java项目的工具，它为我们提供了标准化的项目结构，如下：
```java
├─ProjectName                              // 项目名称
    │  ├─src                      // 根目录
    │  │   ├─main                  // 主目录
    │  │   │  ├─java             // Java源码目录
    │  │   │  ├─resources      //配置文件目录
    │  │   │  └─webapp                            // Web文件目录
    │  │   ├─test                  // 测试目录
    │  │   │  ├─java             // Java测试代码目录
    │  │   │  └─resources                            // 测试资源目录
    │  └─pom.xml                      // Maven项目核心配置文件

```
同时也提供了一套标准的构建流程：<br />![](https://cdn.nlark.com/yuque/0/2023/webp/28163149/1691333047380-c8ee50e8-93b0-4833-af36-47ef04a11c0d.webp#averageHue=%23fdfcfc&clientId=u237dd497-258d-4&from=paste&id=u7c6ee10b&originHeight=317&originWidth=349&originalType=url&ratio=1.375&rotation=0&showTitle=false&status=done&style=none&taskId=u44577dcd-71fe-489b-a737-99a1666a636&title=)<br /> 从编译，到测试、打包、发布……，涵盖整个项目开发的全流程。<br />并且最重要的一点，它还提供了依赖（Jar包）管理功能，回想大家最初学JavaEE时，想要用到一个外部的工具包，必须先从网上找到对应的Jar文件，接着将其手动丢到项目的lib目录下，当项目需要依赖的外部包达到几十个、每个外部包还依赖其他包时，这个过程无疑很痛苦。<br />而这一切的一切，随着Maven的出现，从此不复存在。
<a name="ny4QS"></a>
### Maven安装
使用Maven前，必须先安装它，这时可以先去到[Maven官网](https://link.juejin.cn/?target=https%3A%2F%2Fmaven.apache.org%2Fdownload.cgi)下载自己所需的版本：<br />![](https://cdn.nlark.com/yuque/0/2023/webp/28163149/1691333154058-31901e86-0f67-40f8-ae15-adcfb1a886e7.webp#averageHue=%23fcfcfc&clientId=u237dd497-258d-4&from=paste&id=u8b25d4d0&originHeight=469&originWidth=1512&originalType=url&ratio=1.375&rotation=0&showTitle=false&status=done&style=none&taskId=u84c70ad7-6dd3-425f-9be4-f4223123cd0&title=)<br /> 下载进行解压后（不过解压的目录最好别带中文，否则后续会碰到一些问题），接着需要配置一下<br />①在系统环境中，新建一个MAVEN_HOME或M2_HOME的环境变量，值写成解压路径。<br />![](https://cdn.nlark.com/yuque/0/2023/webp/28163149/1691333582955-00af2f77-4ef5-4ea8-94df-4075eb09bf0e.webp#averageHue=%23e9e7e0&clientId=u237dd497-258d-4&from=paste&id=u83d92621&originHeight=222&originWidth=597&originalType=url&ratio=1.375&rotation=0&showTitle=false&status=done&style=none&taskId=ue4b1f23a-ef8d-4307-af50-c9f4067beda&title=)<br />②找到Path变量并编辑，在其中新增一行，配置一下bin目录：
```bash
%M2_HOME%\bin
```
![](https://cdn.nlark.com/yuque/0/2023/webp/28163149/1691333617911-c1966243-db8e-4e17-a01e-37c949e8dca4.webp#averageHue=%23e4e2da&clientId=u237dd497-258d-4&from=paste&id=u925cdf18&originHeight=280&originWidth=583&originalType=url&ratio=1.375&rotation=0&showTitle=false&status=done&style=none&taskId=u73bbf6d5-b678-425c-b8f8-82751418575&title=)<br />![](https://cdn.nlark.com/yuque/0/2023/webp/28163149/1691333620001-e88d8d42-e4e1-49ac-ae7b-8d7bea9c7db3.webp#averageHue=%23e1ded6&clientId=u237dd497-258d-4&from=paste&id=ub34a83d1&originHeight=549&originWidth=519&originalType=url&ratio=1.375&rotation=0&showTitle=false&status=done&style=none&taskId=uad952681-a49b-4771-a1ae-b014b1106c8&title=)<br />其实安装许多软件，都要配置这一步，到底是为啥呢？因为任何软件的bin目录，通常会存放一些可执行的脚本/工具，如JDK的bin目录中，就存放着javac、javap、jstack……一系列工具。如果不在Path中配置bin，那想要使用这些工具，只能去到JDK安装目录下的bin目录，然后才能使用。<br />不过当大家在Path中配置了bin之后，这个配置就会对全局生效，任何位置执行javac这类指令，都可以从Path中，找到对应的bin目录位置，然后调用其中提供的工具。<br />③找到Maven解压目录下的conf/settings.xml，然后点击编辑，找到<localRepository>标签，将其挪动到注释区域外，然后配置本地仓库位置：
```bash
<localRepository>自己选择一个空的本地目录（最好别带中文）</localRepository>
```
④由于Apache的官方镜像位于国外，平时拉取依赖比较慢，所以还需配置Maven国内的镜像源，这时在settings.xml文件中，先搜索<mirrors>标签，接着在其中配置阿里云的镜像地址：
```xml
<mirror>
  <id>aliyunmaven</id>
  <mirrorOf>*</mirrorOf>
  <name>阿里云公共仓库</name>
  <url>https://maven.aliyun.com/repository/public</url>
</mirror>
```
到这里，整个Maven安装流程全部结束，最后也可以在终端工具，执行mvn -v命令检测一下。
<a name="uczam"></a>
### ![image.png](https://cdn.nlark.com/yuque/0/2023/png/28163149/1691333304598-e0a5fff3-33dc-491f-8134-2d132b438b78.png#averageHue=%23191919&clientId=u237dd497-258d-4&from=paste&height=92&id=ue4c372a1&originHeight=126&originWidth=793&originalType=binary&ratio=1.375&rotation=0&showTitle=false&size=8846&status=done&style=none&taskId=u815d5b65-44ba-4764-9df3-328434b73c3&title=&width=576.7272727272727)
<a name="Jhv64"></a>
### Maven入门指南
安装好Maven后，接着可以通过IDEA工具来创建Maven项目，不过要记得配置一下本地Maven及仓库位置：![](https://cdn.nlark.com/yuque/0/2023/webp/28163149/1691333338837-757ce6af-097e-4221-b33d-9df1014f2d02.webp#averageHue=%23e1dfd8&clientId=u237dd497-258d-4&from=paste&id=u1d168f65&originHeight=730&originWidth=1190&originalType=url&ratio=1.375&rotation=0&showTitle=false&status=done&style=none&taskId=u8fe4dcb5-6f01-49d5-9f1a-e2d5ec68acc&title=)在这里配置，是对全局生效，后续创建的所有Maven项目，都会使用这组配置。
<a name="oTxGH"></a>
#### IDEA创建Maven项目
接着就可以创建Maven项目，这个过程特别简单，先选择New Project：<br />![](https://cdn.nlark.com/yuque/0/2023/webp/28163149/1691333374281-f29b5455-0368-4a61-a010-0740eec65d4d.webp#averageHue=%23e4e3dd&clientId=u237dd497-258d-4&from=paste&id=u56f8ca40&originHeight=593&originWidth=890&originalType=url&ratio=1.375&rotation=0&showTitle=false&status=done&style=none&taskId=u1f87b9cd-a462-4ef8-84ff-8331f54edac&title=)<br /> 这里选创建Maven项目，接着指定一下JDK，还可以选择是否使用骨架，选好后直接Next下一步：<br />![](https://cdn.nlark.com/yuque/0/2023/webp/28163149/1691333382854-ab75e719-adf7-4a45-aec4-5cf9eebb6533.webp#averageHue=%23f0efef&clientId=u237dd497-258d-4&from=paste&id=u797bf450&originHeight=591&originWidth=885&originalType=url&ratio=1.375&rotation=0&showTitle=false&status=done&style=none&taskId=u345211d1-25ba-4e29-be18-06bb2119c80&title=)<br /> 这里需要写一下GAV坐标，稍微解释一下三个选项的含义：

- GroupID：组织ID，一般写公司的名称缩写；
- ArtifactID：当前Maven工程的项目名字；
- Version：当前Maven工程的版本。

接着点下一步，然后选择一下项目的存储位置，最后点击Finish创建即可：<br />![](https://cdn.nlark.com/yuque/0/2023/webp/28163149/1691333407482-28ee77a7-e920-4f04-9614-a27f38ebd3c5.webp#averageHue=%23ecebea&clientId=u237dd497-258d-4&from=paste&id=u328117e9&originHeight=591&originWidth=890&originalType=url&ratio=1.375&rotation=0&showTitle=false&status=done&style=none&taskId=u01ebb167-8bed-4e9b-9bac-28f4a9984e6&title=)这一步结束后，就得到了一个纯净版的Maven项目，然后可以基于Maven实现依赖管理。
<a name="QTtVl"></a>
#### Maven依赖管理
最简单的依赖管理，总共就只有三步，如下：

- ①在pom.xml中，先写一个<dependencies>标签；
- ②在<dependencies>标签中，使用<dependency>标签来导入依赖；
- ③在<dependency>标签中，通过GAV坐标来导入依赖。

如果你不知道一个依赖的GAV该怎么写，可以去[仓库索引](https://link.juejin.cn/?target=https%3A%2F%2Fmvnrepository.com%2F)中搜索，现在写个坐标来感受一下：
```xml
<dependencies>
  <dependency>
    <groupId>org.springframework</groupId>
    <artifactId>spring-web</artifactId>
    <version>5.1.8.RELEASE</version>
  </dependency>
</dependencies>

```
引入GAV坐标后，依赖不会立马生效，需要手动刷新一下项目：<br />![](https://cdn.nlark.com/yuque/0/2023/webp/28163149/1691333729566-6be81618-8cc2-4ead-8209-17970e1a6381.webp#averageHue=%23f7f7f6&clientId=u237dd497-258d-4&from=paste&id=ue4c8248a&originHeight=701&originWidth=1512&originalType=url&ratio=1.375&rotation=0&showTitle=false&status=done&style=none&taskId=uc2693b9f-e17a-4f64-9821-3cf5f78f5f5&title=)可以借助IDEA自带的Maven项目工具来进行刷新；也可以安装Maven-Helper插件，在项目上右键，然后通过Run Maven里的指令刷新。至此，大家就掌握了Maven的基本使用。
:::warning
PS：如果你本地仓库中有依赖，但忘了GAV坐标怎么写，通过IDEA工具，在pom.xml文件中按下alt+insert快捷键，接着点击Dependency，可以做到可视化快捷导入。
:::
<a name="DYqPN"></a>
#### 依赖管理范围
有时候，有些依赖我们并不希望一直有效，比如典型的JUnit测试包，对于这类jar包而言，最好只针对测试环境有效，而编译环境、运行环境中，因为用不到单元测试，所以有没有办法移除呢？这时可以通过<scope>标签来控制生效范围：例如：
```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-test</artifactId>
    <version>2.1.8.RELEASE</version>
    <scope>test</scope>
</dependency>
```
该标签共有五种取值方式，每种取值对应着一种依赖范围，而不同的依赖范围，生效的环境（classpath）也并不同，如下表所示：

| 依赖范围 | 编译环境 | 测试环境 | 运行环境 |
| --- | --- | --- | --- |
| compile | 生效 | 生效 | 生效 |
| provided | 生效 | 生效 | 不生效 |
| system | 生效 | 生效 | 不生效 |
| runtime | 不生效 | 生效 | 生效 |
| test | 不生效 | 生效 | 不生效 |

项目引入的所有依赖，如果不显式指定依赖范围，默认是compile，意味着所有环境下都生效，而一般的依赖包也无需更改，只有某些特殊的依赖，才需要手动配置一下。如：

- JUnit、spring-test这类包，只在测试环境使用，所以配成test；
- Tomcat内置servlet-api包，为了避免在运行环境冲突，应该配成provided；
- ……

同时，<scope>标签还可以通过自定义的方式来添加其他的scope范围，例如Maven插件中使用的scope值：
```xml
<dependency>
    <groupId>some.group</groupId>
    <artifactId>some-artifact</artifactId>
    <version>1.0</version>
    <scope>plugin</scope>
</dependency>
```
这里的plugin就是自定义的scope，表示该依赖只在Maven插件中生效。<br />最后，<scope>标签还有一类特殊、但很常用的取值范围，即import，但这个放在后面去讲
<a name="cwMOd"></a>
### Maven工作原理剖析
在Maven中，节点会分为工程，仓库两大类，工程是“依赖使用者”，仓库是"依赖提供者"，关系如下：<br />![](https://cdn.nlark.com/yuque/0/2023/webp/28163149/1691344377009-f6464766-49e0-4a9c-80e7-a35d2ca9ed41.webp#averageHue=%23f9f8f6&clientId=u237dd497-258d-4&from=paste&id=u032f7c18&originHeight=370&originWidth=767&originalType=url&ratio=1.375&rotation=0&showTitle=false&status=done&style=none&taskId=u8aa5b3b2-bcd3-45a4-a6f9-da4e83ed972&title=)<br />看着或许有点头大，要讲明白得先弄清里面三种仓库：

- 中央仓库：就是前面配置的镜像源，里面拥有海量的公共jar包资源；
- 远程仓库：也叫私服仓库，主要存储公司内部的jar包资源，这个后续会细说；
- 本地仓库：自己电脑本地的仓库，会在磁盘上存储jar包资源。

大致了解三种仓库的含义后，接着来梳理Maven的工作流程：

1. 项目通过GAV坐标引入依赖，首先会去本地仓库查找JAR包
2. 如果在本地仓库找到了，就直接把依赖载入到当前工程的External Libraries中；
3. 如果没找到，则去读取settings.xml文件，判断是否存在私服配置；
4. 如果有私服配置，根据配置的地址找到远程仓库，接着拉取依赖到本地仓库；
5. 如果远程仓库中没有依赖，根据私服配置去中央仓库拉取，然后放到私服、本地仓库；
6. 从远程或中央仓库中，把依赖下载到本地后，再重复第二步，把依赖载入到项目中。

上述六步便是Maven的完整工作流程，可能许多人没接触过私服，这个会放到后面聊。如果你的项目没配置Maven私服，那么第三步时，会直接从settings.xml读取镜像源配置，直接去到中央仓库拉取依赖

不过这里有个问题，拉取/引入依赖时，Maven是怎么知道要找谁呢？答案是依靠GAV坐标，大家可以去观察一下本地仓库，当你引入一个依赖后，本地仓库中的目录，会跟你的GAV坐标一一对应，如：<br />![](https://cdn.nlark.com/yuque/0/2023/webp/28163149/1691344616414-1f105af1-c6f5-4cbc-afeb-9b76e44c9a64.webp#averageHue=%23f6f3f2&clientId=u237dd497-258d-4&from=paste&id=u40ac0c9b&originHeight=570&originWidth=915&originalType=url&ratio=1.375&rotation=0&showTitle=false&status=done&style=none&taskId=u760fb8ce-3103-4e1f-be6a-038b0d214f3&title=)无论是什么类型的仓库，都会遵循这个原则进行构建，所以，只要你书写了正确的GAV坐标，就一定能够找到所需的依赖，并将其载入到项目中。
<a name="mPNbz"></a>
### Maven生命周期
通过IDEA工具的辅助，能很轻易看见Maven的九种Lifecycle命令，如下：<br />![](https://cdn.nlark.com/yuque/0/2023/webp/28163149/1691344675736-7b09a5f2-6c37-475d-9f7a-2c518b0598d8.webp#averageHue=%23fdfcfc&clientId=u237dd497-258d-4&from=paste&id=uc957acef&originHeight=317&originWidth=349&originalType=url&ratio=1.375&rotation=0&showTitle=false&status=done&style=none&taskId=u76e890c5-412f-4188-906d-87d49017ef1&title=)<br />双击其中任何一个，都会执行相应的Maven构建动作，为啥IDEA能实现这个功能呢？道理很简单，因为IDEA封装了Maven提供的命令，如：点击图中的clean，本质是在当前目录中，执行了mvn clean命令，下面解释一下每个命令的作用：

- **clean **: 清除当前工程编译后生成的文件（即删除target整个目录）
- **validate : **对工程进行基础验证，如工程结构，pom,资源文件等是否正确
- **compile ： **对src/main/java目录下的源码进行编译，（会产生target目录）
- **test ：** 编译并执行src/test/java 目录下的所有测试样例
- **package ： **将当前项目打包。普通项目打JAR包，webapp项目打WAR包
- **verify : **验证工程所有代码。配置进行是否正确，如类中代码的语法检测等；
- **install: **将当前工程打包，然后安装到本地仓库，别人可通过GAV导入；
- **site: **生成项目的概述、源码测试覆盖率、开发者列表等站点文档（需要额外配置）
- **deploy: **将当前工程对应的包，上传到远程仓库，提供给他人使用（私服会用）。

上述便是九个周期阶段命令的释义，而Maven总共划分了三套生命周期：<br />![](https://cdn.nlark.com/yuque/0/2023/webp/28163149/1691345076472-20eca4c2-6992-4716-a0e5-51b8c8b1fa6d.webp#averageHue=%23f7f5f5&clientId=u237dd497-258d-4&from=paste&id=u216ccbbd&originHeight=397&originWidth=719&originalType=url&ratio=1.375&rotation=0&showTitle=false&status=done&style=none&taskId=u16a25d30-4011-4990-a275-d727cdc39e6&title=)<br />主要看default这套，该生命周期涵盖了构建过程中的检测、编译、测试、打包、验证、安装、部署每个阶段。注意一点：**同一生命周期内，执行后面的命令，前面的所有命令会自动执行**！比如现在执行一条命令:
```xml
mvn test
```
test命令位于default这个生命周期内，所以它会先执行validate、compile这两个阶段，然后才会真正执行test阶段。同时，还可以一起执行多个命令，如：
```xml
mvn clean install
```
这两个命令隶属于不同的周期，所以会这样执行：先执行clean周期里的pre-clean、clean，再执行default周期中，validate~install这个闭区间内的所有阶段。<br />从上面不难发现，default是Maven的核心周期，但其实上面并没有给完整，因为官方定义的default一共包含23个小阶段，上面的图只列出了七个核心周期，对详细阶段感兴趣的可以自行了解。

从上面不难发现，default是Maven的核心周期，但其实上面并没有给完整，因为官方定义的default一共包含23个小阶段，上面的图只列出了七个核心周期，对详细阶段感兴趣的可以自行了解

Maven中只定义了三套生命周期，以及每套周期会包含哪些阶段，而每个阶段具体执行的操作，这会交给插件去干，也就是说：**Maven插件会实现生命周期中的每个阶段**，这也是大家为什么看到IDEA的Lifecycle下面，还会有个Plugins的原因：<br />![](https://cdn.nlark.com/yuque/0/2023/webp/28163149/1691345284632-e5ec1ece-5c85-4dcb-bf68-04ae88e8e7da.webp#averageHue=%23ebeae5&clientId=u237dd497-258d-4&from=paste&id=u8839520a&originHeight=331&originWidth=492&originalType=url&ratio=1.375&rotation=0&showTitle=false&status=done&style=none&taskId=u7bd21844-8a70-415a-af3c-256ffd7a44d&title=)<br />当你双击Lifecycle中的某个生命周期阶段，实际会调用Plugins中对应的插件。在Shell窗口执行mvn命令时，亦是如此，因为插件对应的实现包，都会以jar包形式存储在本地仓库里。
:::warning
"你有特殊的需求，也可以在pom.xml的<build>标签中，依靠<plugins>插件来导入"
:::
<a name="kVS8Q"></a>
## Maven进阶操作
上面所说到的一些知识，仅仅只是Maven的基本操作，而它作为Java项目管理占有率最高的工具，还提供了一系列高阶功能，例如属性管理、多模块开发、聚合工程等，不过这里先来说说依赖冲突。
<a name="WvnpK"></a>
### 依赖冲突
依赖冲突是指：**在Maven项目中，当多个依赖包，引入了同一份类库的不同版本时，可能会导致编译错误或运行时异常**。这种情况下，想要解决依赖冲突，可以靠升级/降级某些依赖项的版本，从而让不同依赖引入的同一类库，保持一致的版本号。

另外，还可以通过隐藏依赖、或者排除特定的依赖项来解决问题。但是想搞明白这些，首先得理解Maven中的依赖传递性，一起来看看。
<a name="JZNO6"></a>
#### 依赖的传递性
先来看个例子：<br />![](https://cdn.nlark.com/yuque/0/2023/webp/28163149/1691345424940-fc736f05-e0ea-4e25-a1d9-73acb77685db.webp#averageHue=%23efeee8&clientId=u237dd497-258d-4&from=paste&id=u5dd8dd71&originHeight=415&originWidth=509&originalType=url&ratio=1.375&rotation=0&showTitle=false&status=done&style=none&taskId=ub440307e-73f3-4024-9cfe-23199019a16&title=)<br />目前的工程中，仅导入了一个spring-web依赖，可是从下面的依赖树来看，web还间接依赖于beans、core包，而core包又依赖于jcl包，此时就出现了依赖传递，所谓的依赖传递是指：**当引入的一个包，如果依赖于其他包（类库），当前的工程就必须再把其他包引入进来**。

 这相当于无限套娃，而这类“套娃”引入的包，被称为间接性依赖。与之对应的是直接性依赖，即：**当前工程的pom.xml中，直接通过GAV坐标引入的包**。既然如此，那么一个工程内的依赖包，就必然会出现层级，如：![](https://cdn.nlark.com/yuque/0/2023/webp/28163149/1691345479037-002f0d3e-9aed-4880-965d-df269c663f61.webp#averageHue=%23fcfbfb&clientId=u237dd497-258d-4&from=paste&id=udb73e3ce&originHeight=707&originWidth=1201&originalType=url&ratio=1.375&rotation=0&showTitle=false&status=done&style=none&taskId=ub1c3f382-a85a-43a4-a1a1-d894e9058f9&title=)在这里我们仅引入了一个boot-test坐标，但当打开依赖树时，会发现这一个包，依赖于其他许多包，而它所依赖的包又依赖于其他包……，如此不断套娃，最深套到了五层。而不同的包，根据自己所处的层级不同，会被划分为1、2、3、4……级。
<a name="VOgsa"></a>
#### 自动解决冲突问题
Maven作为Apache旗下的产品，而且还经过这么多个版本迭代，对于依赖冲突问题，难道官方想不到吗？必然想到了，所以在绝对大多数情况下，依赖冲突问题并不需要我们考虑，Maven工具会自动解决，怎么解决的呢？就是基于前面所说的依赖层级，下面来详细说说。<br />**①层级优先原则**，Maven会根据依赖树的层级，来自动剔除相同的包，层级越浅，优先级越高。这是啥意思呢？同样来看个例子：<br />![](https://cdn.nlark.com/yuque/0/2023/webp/28163149/1691345528412-2b54bdb9-c18c-426b-bec7-7fe09affa64b.webp#averageHue=%23fbfaf9&clientId=u237dd497-258d-4&from=paste&id=u386b2bc3&originHeight=537&originWidth=1203&originalType=url&ratio=1.375&rotation=0&showTitle=false&status=done&style=none&taskId=u40b8cb3d-8125-46bd-82a9-6f175a8e742&title=)<br />我们又通过GAV坐标导入了spring-web包，根据前面所说，web依赖于beans、core包，而beans包又依赖于core包，此时注意，这里出现了两个core包，前者的层级为2，后者的层级为3，所以Maven会自动将后者剔除，这点从图中也可明显看出，层级为3的core直接变灰了。<br />**②声明优先原则**，上条原则是基于层级深度，来自动剔除冲突的依赖，那假设同级出现两个相同的依赖怎么办？来看例子：<br />![](https://cdn.nlark.com/yuque/0/2023/webp/28163149/1691345591293-72e0c54f-fe9b-4e09-af8f-073365c98b6e.webp#averageHue=%23eeede6&clientId=u237dd497-258d-4&from=paste&id=u4817b741&originHeight=387&originWidth=1093&originalType=url&ratio=1.375&rotation=0&showTitle=false&status=done&style=none&taskId=u9c77071c-81f4-4afb-b356-0eccac3c2fd&title=)此时用GAV引入了web、jdbc两个包，来看右边的依赖树，web依赖于beans、core包，jdbc也依赖于这两个包，此时相同层级出现了依赖冲突，可从结果上来看，后面jdbc所依赖的两个包被剔除了，能明显看到一句：omitted for duplicate，这又是为啥呢？因为根据声明优先原则，**同层级出现包冲突时，先声明的会覆盖后声明的，为此后者会被剔除**。

**③配置优先原则**，此时问题又又来了，既然相同层级出现同版本的类库，前面的会覆盖后面的，可是当相同层级，出现不同版本的包呢？依旧来看例子：<br />![](https://cdn.nlark.com/yuque/0/2023/webp/28163149/1691345663836-f19c85db-7e8d-4cd1-b98e-3d3e689b5d53.webp#averageHue=%23f9f7f6&clientId=u237dd497-258d-4&from=paste&id=u75abb5e5&originHeight=435&originWidth=1025&originalType=url&ratio=1.375&rotation=0&showTitle=false&status=done&style=none&taskId=uba710922-ee6a-4824-84aa-100c73b2187&title=)此时pom引入了两个web包，前者版本为5.1.8，后者为5.1.2，这两个包的层级都是1，可是看右边的依赖树，此时会发现，5.1.8压根没引进来啊！为啥？这就是配置优先原则，**同级出现不同版本的相同类库时，后配置的会覆盖先配置的。**

所以大家发现了嘛？在很多时候，并不需要我们考虑依赖冲突问题，Maven会依据上述三条原则，帮我们智能化自动剔除冲突的依赖，其他包都会共享留下来的类库，只有当出现无法解决的冲突时，这才需要咱们手动介入。
<a name="xSjkr"></a>
#### 主动排除依赖
所谓的排除依赖，即是指从一个依赖包中，排除掉它依赖的其他包，如果出现了Maven无法自动解决的冲突，就可以基于这种手段进行处理，例如：
```xml
<dependency>
    <groupId>org.springframework</groupId>
    <artifactId>spring-web</artifactId>
    <version>5.1.8.RELEASE</version>
    <exclusions>
        <!-- 排除web包依赖的beans包 -->
        <exclusion>
            <groupId>org.springframework</groupId>
            <artifactId>spring-beans</artifactId>
        </exclusion>
    </exclusions>
</dependency>
```
![](https://cdn.nlark.com/yuque/0/2023/webp/28163149/1691345739958-f33c88bd-b497-4220-9648-7c4df4996f5e.webp#averageHue=%23f2f0eb&clientId=u237dd497-258d-4&from=paste&id=u42a7af39&originHeight=380&originWidth=1022&originalType=url&ratio=1.375&rotation=0&showTitle=false&status=done&style=none&taskId=ufe2eefb0-b669-4b14-9203-a2611010a2e&title=)从图中结果可以明显看出，通过这种方式，可以手动移除包所依赖的其他包。当出现冲突时，通过这种方式将冲突的两个包，移除掉其中一个即可。<br />其实还有种叫做“隐藏依赖”的手段，不过这种手段是用于多工程聚合项目，所以先讲清楚“多模块/工程”项目，接着再讲“隐藏依赖”。
<a name="nfTTM"></a>
### Maven分模块开发
现如今，一个稍具规模的完整项目，通常都要考虑接入多端，如PC、WEB、APP端等，那此时问题来了，每个端之间的逻辑，多少会存在细微差异，如果将所有代码融入在一个Maven工程里，这无疑会显得十分臃肿！为了解决这个问题，Maven推出了分模块开发技术。

所谓的分模块开发，**即是指创建多个Maven工程，组成一个完整项目**。通常会先按某个维度划分出多个模块，接着为每个模块创建一个Maven工程，典型的拆分维度有三个：

- ①接入维度：按不同的接入端，将项目划分为多个模块，如APP、WEB、小程序等；
- ②业务维度：根据业务性质，将项目划分为一个个业务模块，如前台、后台、用户等；
- ③功能维度：共用代码做成基础模块，业务做成一个模块、API做成一个模块……。

当然，通常①、②会和③混合起来用，比如典型的“先根据代码功能拆分，再根据业务维度拆分”。<br />相较于把所有代码揉在一起的“大锅饭”，多模块开发的好处特别明显：

- ①简化项目管理，拆成多个模块/子系统后，每个模块可以独立编译、打包、发布等；
- ②提高代码复用性，不同模块间可以相互引用，可以建立公共模块，减少代码冗余度；
- ③方便团队协作，多人各司其职，负责不同的模块，Git管理时也能减少交叉冲突；
- ④构建管理度更高，更方便做持续集成，可以根据需要灵活配置整个项目的构建流程；
- ……

不过**Maven2.0.9**才开始支持聚合工程，在最初的时期里，想要实现分模块开发，需要手动先建立一个空的Java项目（Empty Project）：<br />![](https://cdn.nlark.com/yuque/0/2023/webp/28163149/1691345878835-1e353eae-aef4-495a-844b-5fa9482383cc.webp#averageHue=%23f9f9f9&clientId=u237dd497-258d-4&from=paste&id=u5342f62a&originHeight=575&originWidth=877&originalType=url&ratio=1.375&rotation=0&showTitle=false&status=done&style=none&taskId=u91e55bcb-22c3-4ced-a568-1e5dcf00575&title=)<br />接着再在其中建立多个Maven Project：<br />![](https://cdn.nlark.com/yuque/0/2023/webp/28163149/1691345886228-13cf302e-3933-4ae9-ad2b-8a08da4dbb17.webp#averageHue=%23dad9d9&clientId=u237dd497-258d-4&from=paste&id=u03206770&originHeight=558&originWidth=1512&originalType=url&ratio=1.375&rotation=0&showTitle=false&status=done&style=none&taskId=u61f94b76-7ec3-4f5a-9995-7316063690e&title=)<br /> 然后再通过mvn install命令，将不同的Maven项目安装到本地仓库，其他工程才能通过GAV坐标引入。<br />这种传统方式特别吃力，尤其是多人开发时，另一个模块的代码更新了，必须手动去更新本地仓库的jar包；而且多个模块之间相互依赖时，构建起来额外的麻烦！正因如此，官方在后面推出了“聚合工程”，下面聊聊这个。
<a name="E4ZWd"></a>
### Maven聚合工程
所谓的聚合工程，即是指：**一个项目允许创建多个子模块，多个子模块组成一个整体，可以统一进行项目的构建**。不过想要弄明白聚合工程，得先清楚“父子工程”的概念：

- 父工程：不具备任何代码、仅有pom.xml的空项目，用来定义公共依赖、插件和配置；
- 子工程：编写具体代码的子项目，可以继承父工程的配置、依赖项，还可以独立拓展。

而Maven聚合工程，就是基于父子工程结构，来将一个完整项目，划分出不同的层次，这种方式可以很好的管理多模块之间的依赖关系，以及构建顺序，大大提高了开发效率、维护性。并且当一个子工程更新时，聚合工程可以保障同步更新其他存在关联的子工程！
<a name="xmIhM"></a>
#### 聚合工程入门指南
理解聚合工程是个什么东东之后，接着来聊聊如何创建聚合工程，首先要创建一个空的Maven项目，作为父工程，这时可以在IDEA创建Maven项目时，把打包方式选成POM，也可以创建一个普通的Maven项目，然后把src目录删掉，再修改一下pom.xml：
```xml
<!-- 写在当前项目GAV坐标下面 -->
<packaging>pom</packaging>
```
这样就得到了一个父工程，接着可以在此基础上，继续创建子工程：<br />![](https://cdn.nlark.com/yuque/0/2023/webp/28163149/1691345994073-41632afe-6912-4dc9-b46a-6787ab7b8472.webp#averageHue=%23dcdcdb&clientId=u237dd497-258d-4&from=paste&id=ued0b7893&originHeight=587&originWidth=1512&originalType=url&ratio=1.375&rotation=0&showTitle=false&status=done&style=none&taskId=u4295e155-1663-4283-a3c8-c9b48b07da2&title=)当点击Next后，大家会发现：<br />![](https://cdn.nlark.com/yuque/0/2023/webp/28163149/1691346011040-0881572e-78ab-4f49-be84-0fa6afbf8371.webp#averageHue=%23f0efee&clientId=u237dd497-258d-4&from=paste&id=ub2f15c53&originHeight=252&originWidth=877&originalType=url&ratio=1.375&rotation=0&showTitle=false&status=done&style=none&taskId=u15bc232f-a460-45c0-a870-07197d30faa&title=)这时无法手动指定G、V了，而是会从父工程中继承，最终效果如下：<br />![](https://cdn.nlark.com/yuque/0/2023/webp/28163149/1691346025140-4dae83bc-8c4a-4885-84ea-0685d1805b5e.webp#averageHue=%23f2f1e5&clientId=u237dd497-258d-4&from=paste&id=u2272102d&originHeight=485&originWidth=1512&originalType=url&ratio=1.375&rotation=0&showTitle=false&status=done&style=none&taskId=u8f161518-ffb0-4a10-b540-c1426ad1813&title=)<br /> 这里我创建了两个子工程，所以父工程的pom.xml中，会用一个<modules>标签，来记录自己名下的子工程列表，而子工程的pom头，也多了一个<parent>标签包裹！大家看这个标签有没有眼熟感？大家可以去看一下SpringBoot项目，每个pom.xml文件的头，都是这样的。

这里提个问题：**子工程下面能不能继续创建子工程**？答案Yes，你可以无限套娃下去，不过我的建议是：一个聚合项目，最多只能有三层，路径太深反而会出现稀奇古怪的问题。
<a name="XiX33"></a>
#### 聚合工程的依赖管理
前面搭建好了聚合工程，接着来看个问题：<br />![](https://cdn.nlark.com/yuque/0/2023/webp/28163149/1691346081218-8820a453-a29c-4991-b43f-7be0d5c5144d.webp#averageHue=%23f3f1e9&clientId=u237dd497-258d-4&from=paste&id=u4c45dd96&originHeight=781&originWidth=1392&originalType=url&ratio=1.375&rotation=0&showTitle=false&status=done&style=none&taskId=ucd8eff3a-0fe6-41fd-bc83-b7ff40fa088&title=)<br /> zhuzi_001、002两个子工程中，各自引入了三个依赖，可观察上图会发现，两者引入的依赖仅有一个不同，其余全部一模一样！所以这时，就出现了“依赖冗余”问题，那有没有好的方式解决呢？答案是有的，前面说过：**公共的依赖、配置、插件等，都可以配置在父工程里**，如下：<br />![](https://cdn.nlark.com/yuque/0/2023/webp/28163149/1691346110250-c3ab108c-10fd-4ba7-b1b5-b9e786e3bbd1.webp#averageHue=%23edeee6&clientId=u237dd497-258d-4&from=paste&id=ud0d34dfb&originHeight=471&originWidth=1512&originalType=url&ratio=1.375&rotation=0&showTitle=false&status=done&style=none&taskId=u095d550c-3b15-4c00-aca5-d7aafd64ddb&title=)当把公共的依赖定义在父工程中，此时观察图中右侧的依赖树，会发现两个子工程都继承了父依赖。<br />不过此时问题又来了！为了防止不同子工程引入不同版本的依赖，最好的做法是在父工程中，统一对依赖的版本进行控制，规定所有子工程都使用同一版本的依赖，怎么做到这点呢？可以使用**<dependencyManagement>**标签来管理，例如：<br />![](https://cdn.nlark.com/yuque/0/2023/webp/28163149/1691346125308-21aa0a75-cfe6-489f-b9b7-6f969d7a6f1d.webp#averageHue=%23efeee6&clientId=u237dd497-258d-4&from=paste&id=u55f50177&originHeight=646&originWidth=1512&originalType=url&ratio=1.375&rotation=0&showTitle=false&status=done&style=none&taskId=u121dba86-b7d5-4318-977e-01ddbe25000&title=)在父工程中，<dependencies>里只定义了一个webmvc依赖，而<dependencyManagement>中定义了druid、test、jdbc三个依赖，这两个标签有何区别呢？

- **<dependencies>**：定义强制性依赖，写在该标签里的依赖项，子工程必须强制继承；
- **<dependencyManagement>**：定义可选性依赖，该标签里的依赖项，子工程可选择使用。

相信这样解释后，大家对于两个标签的区别，就能一清二楚了！同时注意，子工程在使用<dependencyManagement>中已有的依赖项时，不需要写<version>版本号，版本号在父工程中统一管理，这就满足了前面的需求。这样做的好处在于：**以后为项目的技术栈升级版本时，不需要单独修改每个子工程的POM，只需要修改父POM文件即可，大大提高了维护性**！
<a name="js1ix"></a>
#### 聚合工程解决依赖冲突
之前传统的Maven项目会存在依赖冲突问题，那聚合工程中存不存在呢？当然存在，比如001中引入了jdbc、test这两个包，而002中也引入了，这时假设把001工程打包到本地仓库，在002工程中引入时，此时依赖是不是又冲突了？Yes，怎么处理呢？先看例子：<br />![](https://cdn.nlark.com/yuque/0/2023/webp/28163149/1691346279844-508221f3-751e-4201-9b48-a78c93fce583.webp#averageHue=%23e8eae2&clientId=u237dd497-258d-4&from=paste&id=u318d0544&originHeight=525&originWidth=1512&originalType=url&ratio=1.375&rotation=0&showTitle=false&status=done&style=none&taskId=ucdcf144e-30c4-40bc-927a-b888b8fd7cb&title=)<br />在上图中，001引入了aop包，接着通过install操作，把001工程打到了本地仓库。于是，在002工程中，引入了web、zhuzi_001这两个包。根据前面所说的依赖传递原则，002在引入001时，由于001引用了别的包，所以002被迫也引入了其他包。

还是那句话，大多数情况下，Maven会基于那三条原则，自动帮你剔除重复的依赖，如上图右边的依赖树所示，Maven自动剔除了重复依赖。这种结果显然是好现象，可是万一Maven不能自动剔除怎么办？这时就需要用到最开始所说的“隐藏依赖”技术了！

 修改001的pom.xml，如下：
```xml
<dependency>
    <groupId>org.springframework</groupId>
    <artifactId>spring-aop</artifactId>
    <version>5.1.8.RELEASE</version>
    <optional>true</optional>
</dependency>
```
眼尖的小伙应该能发现，此时多了一个<optional>标签，该标签即是“隐藏依赖”的开关：

- true：开启隐藏，当前依赖不会向其他工程传递，只保留给自己用；
- false：默认值，表示当前依赖会保持传递性，其他引入当前工程的项目会间接依赖。

此时重新把001打到本地仓库，再来看看依赖树关系：<br />![](https://cdn.nlark.com/yuque/0/2023/webp/28163149/1691346348026-16e70eba-e85e-4bea-b13c-e0b4a24fbada.webp#averageHue=%23eeefe8&clientId=u237dd497-258d-4&from=paste&id=u6f1cfe89&originHeight=417&originWidth=423&originalType=url&ratio=1.375&rotation=0&showTitle=false&status=done&style=none&taskId=uaae4717d-26f5-4004-9c2d-cd51c759573&title=)<br /> 当开启隐藏后，其他工程引入当前工程时，就不会再间接引入当前工程的隐藏依赖，因此来手动排除聚合工程中的依赖冲突问题。其他许多资料里，讲这块时，多少讲的有点令人迷糊，而相信看到这里，大家就一定理解了Maven依赖管理。
<a name="mukJp"></a>
#### 父工程的依赖传递
来思考一个问题，现在项目需要用到Spring-Cloud-Alibaba的多个依赖项，如Nacos、Sentinel……等，根据前面所说的原则，由于这些依赖项可能会在多个子工程用到，最好的方式是定义在父POM的<dependencyManagement>标签里，可是CloudAlibaba依赖这么多，一个个写未免太繁杂、冗余了吧？<br />面对上述问题时，该如何处理呢？如下：
```xml
<dependency>
  <groupId>com.alibaba.cloud</groupId>
  <artifactId>spring-cloud-alibaba-dependencies</artifactId>
  <version>${spring-cloud-alibaba.version}</version>
  <type>pom</type>
  <scope>import</scope>
</dependency>
```
**<scope>**标签取值为**import**的方式，通常会用在聚合工程的父工程中，不过必须配合**<type>pom</type>**使用，这是啥意思呢？<br />这代表着：**把spring-cloud-alibaba-dependencies的所有子依赖，作为当前项目的可选依赖向下传递**。<br />而当前父工程下的所有子工程，在继承父POM时，也会将这些可选依赖继承过来，这时就可以直接选择使用某些依赖项啦，如：
```xml
<dependency>
    <groupId>com.alibaba.cloud</groupId>
    <artifactId>spring-cloud-starter-alibaba-sentinel</artifactId>
</dependency>

<dependency>
    <groupId>com.alibaba.cloud</groupId>
    <artifactId>spring-cloud-starter-alibaba-nacos-config</artifactId>
</dependency>
```
<a name="xmGf0"></a>
#### 聚合工程的构建
前面说到过，Maven聚合工程可以对所有子工程进行统一构建，这是啥意思呢？如果是传统的分模块项目，需要挨个进行打包、测试、安装……等工作，而聚合工程则不同，来看IDEA提供的Maven辅助工具：<br />![](https://cdn.nlark.com/yuque/0/2023/webp/28163149/1691346603613-6a6ea022-25f7-43db-b361-120b9929ed9a.webp#averageHue=%23f7f9f4&clientId=u237dd497-258d-4&from=paste&id=u8c466e81&originHeight=490&originWidth=311&originalType=url&ratio=1.375&rotation=0&showTitle=false&status=done&style=none&taskId=u5e8fdb4d-1242-4b6f-b9e3-af6388e8b2f&title=)<br />尾巴上带有root标识的工程，意味着这是一个父工程，在我们的案例中，有一个父、两个子，来看IDEA的工具，除开给两个子工程提供了Lifecycle命令外，还给父工程提供了一套Lifecycle命令，这两者的区别在哪儿呢？当你双击父工程的某个Lifecycle命令，它找到父POM的<modules>标签，再根据其中的子工程列表，完成对整个聚合工程的构建工作。

大家可以去试一下，当你双击父工程Lifecycle下的clean，它会把你所有子工程的target目录删除。同理，执行其他命令时也一样，比如install命令，双击后它会把你所有的子工程，打包并安装到本地仓库，不过问题又又又来了！

:::warning
假设这里001引用了002，002又引用了001，两者相互引用，Maven会如何构建啊？到底该先打包001，还是该先打包002？我没去看过Lifecycle插件的源码，不过相信背后的逻辑，应该跟Spring解决依赖循环类似，感兴趣的小伙伴可以自行去研究。不过我这里声明一点：**Maven聚合工程的构建流程，跟<modules>标签里的书写顺序无关，它会自行去推断依赖关系，从而完成整个项目的构建**。
:::

<a name="Cxm6h"></a>
#### 聚合打包跳过测试
当大家要做项目发版时，就需要对整个聚合工程的每个工程打包（jar或war包），此时可以直接双击父工程里的package命令，但test命令在package之前，按照之前聊的生命周期原则，就会先执行test，再进行打包。<br />test阶段，会去找到所有子工程的src/test/java目录，并执行里面的测试用例，如果其中任何一个报错，就无法完成打包工作。而且就算不报错，执行所有测试用例也会特别耗时，这时该怎么办呢？可以选择跳过test阶段，在IDEA工具里的操作如下：<br />![](https://cdn.nlark.com/yuque/0/2023/webp/28163149/1691346746664-d1479d40-d7b3-4d51-a3ed-94b9b233513e.webp#averageHue=%23f2f2ed&clientId=u237dd497-258d-4&from=paste&id=ua7519bb5&originHeight=357&originWidth=338&originalType=url&ratio=1.375&rotation=0&showTitle=false&status=done&style=none&taskId=u74b0c1ca-c9b3-4a2f-b126-958d00ebe95&title=)<br />先选中test命令，接着点击上面的闪电图标，这时test就会画上横线，表示该阶段会跳过。如果你是在用mvn命令，那么打包跳过测试的命令如下：
```xml
mvn package -D skipTests
```
同时大家还可以在pom.xml里，配置插件来精准控制，比如跳过某个测试类不执行，配置规则如下：
```xml
<build>
  <plugins>
    <plugin>
      <artifactId>maven-surefire-plugin</artifactId>
      <version>2.22.1</version>
      <configuration>
        <skipTests>true</skipTests>
        <includes>
          <!-- 指定要执行的测试用例 -->
          <include>**/XXX*Test.java</include>
        </includes>
        <excludes>
          <!-- 执行要跳过的测试用例 -->
          <exclude>**/XXX*Test.java</exclude>
        </excludes>
      </configuration>
    </plugin>
  </plugins>
</build>
```
不过这个功能有点鸡肋，了解即可，通常不需要用到。
<a name="WSRdk"></a>
### Maven属性
回到之前案例的父工程POM中，此时来思考一个问题：<br />![](https://cdn.nlark.com/yuque/0/2023/webp/28163149/1691346833731-c4e38f29-2631-4b3e-9b4a-932ab7490b9e.webp#averageHue=%23eeefeb&clientId=u237dd497-258d-4&from=paste&id=u4cdd5b62&originHeight=788&originWidth=633&originalType=url&ratio=1.375&rotation=0&showTitle=false&status=done&style=none&taskId=u440f8339-d9e3-4da3-adf1-8f56605264c&title=)<br /> 虽然我们通过<dependencyManagement>标签，来控制了子工程中的依赖版本，可目前还有一个小问题：**版本冗余**！比如现在我想把Spring版本从5.1.8升级到5.2.0，虽然不需要去修改子工程的POM文件，可从上图中大家会发现，想升级Spring的版本，还需要修改多处地方！<br />咋办？总不能只升级其中一个依赖的版本吧？可如果全部都改一遍，无疑就太累了……，所以，这里我们可以通过Maven属性来做管理，我们可以在POM的<properties>标签中，自定义属性，如：
```xml
<properties>
    <spring.version>5.2.0.RELEASE</spring.version>
</properties>
```
而在POM的其他位置中，可以通过${}来引用该属性，例如：<br />![](https://cdn.nlark.com/yuque/0/2023/webp/28163149/1691346866706-3b038c9a-0fc5-44ed-be39-c3d73fa38f25.webp#averageHue=%23f8f8f8&clientId=u237dd497-258d-4&from=paste&id=u554282d7&originHeight=764&originWidth=662&originalType=url&ratio=1.375&rotation=0&showTitle=false&status=done&style=none&taskId=u49d7555b-8944-4698-85de-31329dcb621&title=)<br /> 这样做的好处特别明显，现在我想升级Spring版本，只需要修改一处地方即可！<br />除开可以自定义属性外，Maven也会有很多内置属性，大体可分为四类：

| 类型 | 使用方式 |
| --- | --- |
| Maven内置属性 | **${属性名}**，如${**version**} |
| 项目环境属性 | **${setting 属性名}**，如${**settings**.**localRepository**} |
| Java环境变量 | **${xxx.属性名}**，如**${java.class.path}** |
| 系统环境变量 | **${env.属性名}**，如**${env.USERNAME}** |

不过这些用的也不多，同时不需要记，要用的时候，IDEA工具会有提示：
<a name="hoPtX"></a>
### ![](https://cdn.nlark.com/yuque/0/2023/webp/28163149/1691347093963-de909480-810c-4559-a5f6-0e0c190e5bf9.webp#averageHue=%23d3e3ed&clientId=u237dd497-258d-4&from=paste&id=u213c3d2d&originHeight=373&originWidth=580&originalType=url&ratio=1.375&rotation=0&showTitle=false&status=done&style=none&taskId=u051a8362-5e81-4e9f-92e2-4b0a63c6919&title=)<br /> Maven多环境配置
实际工作会分为开发、测试、生产等环境，不同环境的配置信息也略有不同，而大家都知道，我们可以通过spring.profiles.active属性，来动态使用不同环境的配置，而Maven为何又整出一个多环境配置出来呢？想要搞清楚，得先搭建一个SpringBoot版的Maven聚合工程。

首先创建一个只有POM的父工程，但要注意，这里是SpringBoot版聚合项目，需稍微改造：
```xml
<!-- 先把Spring Boot Starter声明为父工程 -->
<parent>
  <groupId>org.springframework.boot</groupId>
  <artifactId>spring-boot-starter-parent</artifactId>
  <version>2.1.5.RELEASE</version>
  <relativePath/>
</parent>

<!-- 当前父工程的GAV坐标 -->
<modelVersion>4.0.0</modelVersion>
<groupId>com.zhuzi</groupId>
<artifactId>maven_zhuzi</artifactId>
<version>1.0-SNAPSHOT</version>
<packaging>pom</packaging>

<!-- 配置JDK版本 -->
<properties>
  <java.version>8</java.version>
</properties>

<dependencies>
  <!-- 引入SpringBootWeb的Starter依赖 -->
  <dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-web</artifactId>
  </dependency>
</dependencies>

<build>
  <plugins>
    <!-- 引入SpringBoot整合Maven的插件 -->
    <plugin>
      <groupId>org.springframework.boot</groupId>
      <artifactId>spring-boot-maven-plugin</artifactId>
    </plugin>
  </plugins>
</build>
```
对比普通聚合工程的父POM来说，SpringBoot版的聚合工程，需要先把spring-boot-starter声明成自己的“爹”，同时需要引入SpringBoot相关的插件，并且我在这里还引入了一个boot-web依赖。<br />接着来创建子工程，在创建时记得选SpringBoot模板来创建，不过创建后记得改造POM：
```xml
<!-- 声明父工程 -->
<parent>
    <artifactId>maven_zhuzi</artifactId>
    <groupId>com.zhuzi</groupId>
    <version>1.0-SNAPSHOT</version>
</parent>
<modelVersion>4.0.0</modelVersion>

<!-- 子工程的描述信息 -->
<artifactId>boot_zhuzi_001</artifactId>
<name>boot_zhuzi_001</name>
<description>Demo project for Spring Boot</description>
```
就只需要这么多，因为SpringBoot的插件、依赖包，在父工程中已经声明了，这里会继承过来。

 接着来做Maven多环境配置，找到父工程的POM进行修改，如下：
```xml
<profiles>
  <!-- 开发环境 -->
  <profile>
    <id>dev</id>
    <properties>
      <profile.active>dev</profile.active>
    </properties>
  </profile>

  <!-- 生产环境 -->
  <profile>
    <id>prod</id>
    <properties>
      <profile.active>prod</profile.active>
    </properties>
    <!-- activeByDefault=true，表示打包时，默认使用这个环境 -->
    <activation>
      <activeByDefault>true</activeByDefault>
    </activation>
  </profile>

  <!-- 测试环境 -->
  <profile>
    <id>test</id>
    <properties>
      <profile.active>test</profile.active>
    </properties>
  </profile>
</profiles>

```
配置完这个后，刷新当前Maven工程，IDEA中就会出现这个：<br />![](https://cdn.nlark.com/yuque/0/2023/webp/28163149/1691347271304-25a015eb-3805-4847-b5f8-befbdae44930.webp#averageHue=%23f5f6f2&clientId=u237dd497-258d-4&from=paste&id=uf6bc4f73&originHeight=233&originWidth=303&originalType=url&ratio=1.375&rotation=0&showTitle=false&status=done&style=none&taskId=ue8595a49-c8d7-455c-ba6b-ffc234eef84&title=)<br /> 默认停留在prod上，这是因为POM中用<activeByDefault>标签指定了，接着去到子工程的application.yml中，完成Spring的多环境配置，如下：
```yaml
# 设置启用的环境
spring:
  profiles:
    active: ${profile.active}

---
# 开发环境
spring:
  profiles: dev
server:
  port: 80
---
# 生产环境
spring:
  profiles: prod
server:
  port: 81
---
# 测试环境
spring:
  profiles: test
server:
  port: 82
---


```
这里可以通过文件来区分不同环境的配置信息，但我这里为了简单，就直接用---进行区分，这组配置大家应该很熟悉，也就是不同的环境中，使用不同的端口号，但唯一不同的是：**以前spring.profiles.active属性会写上固定的值，而现在写的是${profile.active}**，这是为什么呢？

这代表从pom.xml中，读取profile.active属性值的意思，而父POM中配了三组值：dev、prod、test，所以当前子工程的POM，也会继承这组配置，而目前默认勾选在prod上，所以最终spring.profiles.active=prod，不过想要在application.yml读到pom.xml的值，还需在父POM中，加一个依赖和插件：
```xml
<!-- 开启 yml 文件的 ${} 取值支持 -->
<dependency>
  <groupId>org.springframework.boot</groupId>
  <artifactId>spring-boot-configuration-processor</artifactId>
  <version>2.1.5.RELEASE</version>
  <optional>true</optional>
</dependency>

<!-- 添加插件，将项目的资源文件复制到输出目录中 -->
<plugin>
  <groupId>org.apache.maven.plugins</groupId>
  <artifactId>maven-resources-plugin</artifactId>
  <version>3.2.0</version>
  <configuration>
    <encoding>UTF-8</encoding>
    <useDefaultDelimiters>true</useDefaultDelimiters>
  </configuration>
</plugin>
```
最后来尝试启动子工程，操作流程如下：

- ①在Maven工具的Profiles中勾选dev，并刷新当前项目；
- ②接着找到子工程的启动类，并右键选择Run ……启动子项目。

![](https://cdn.nlark.com/yuque/0/2023/webp/28163149/1691347418611-daaa06fb-bcb0-4763-8bd3-9625b37515cc.webp#averageHue=%23efefee&clientId=u237dd497-258d-4&from=paste&id=ubf5cb7c9&originHeight=684&originWidth=1188&originalType=url&ratio=1.375&rotation=0&showTitle=false&status=done&style=none&taskId=u32893930-09f4-475d-ab1d-489bc51f928&title=)<br />先仔细看执行的结果，我来解释一下执行流程：

- ①启动时，pom.xml根据勾选的Profiles，使用相应的dev环境配置；
- ②yml中${profile.active}会读到profile.active=dev，使用dev配置组；
- ③application.yml中的dev配置组，server.port=80，所以最终通过80端口启动。

看完这个流程，大家明白最开始那个问题了吗？Maven为何还整了一个多环境配置？

大家可能有种似懂非懂的感觉，这里来说明一下，先把环境换到微服务项目中，假设有20个微服务，此时项目要上线或测试，所以需要更改配置信息，比如把数据库地址换成测试、线上地址等，而不同环境的配置，相信大家一定用**application-dev.yml**、**application-prod.ym**l……做好了区分。

但就算提前准备了不同环境的配置，可到了切换环境时，还需要挨个服务修改spring.profiles.active这个值，从dev改成prod、test，然后才能使用对应的配置进行打包，可这里有20个微服务啊，难道要手动改20次吗？

而在父POM中配置了Maven多环境后，这时yml会读取pom.xml中的值，来使用不同的配置文件，此时大家就只需要在IDEA工具的Profiles中，把钩子从dev换到test、prod，然后刷新一下Maven，SpringBoot就能动态的切换配置文件，这是不是妙极了？因此，这才是Maven多环境的正确使用姿势！

<a name="KHCif"></a>
## Maven私服搭建
前面叨叨絮絮说了一大堆，最后就来聊聊Maven私服配置，为啥需要私服呢？

大家来设想这么个场景，假设你身在基建团队，主要负责研发各个业务开发组的公用组件，那么当你写完一个组件后，为了能让别的业务开发组用上，难道是先把代码打包，接着用U盘拷出来，给别人送过去嘛？有人说不至于，难道我不会直接发过去啊……

的确，用通讯软件发过去也行，但问题依旧在，假设你的组件升级了，又发一遍吗？所以，为了便于团队协作，搭建一个远程仓库很有必要，写完公用代码后，直接发布到远程仓库，别人需要用到时，直接从远程仓库拉取即可，而你升级组件后，只需要再发布一个新版本即可！<br />那远程仓库该怎么搭建呀？这就得用到Maven私服技术，最常用的就是基于Nexus来搭建。
<a name="QXX34"></a>
### Nexus私服搭建指南
Nexus是Sonatype公司开源的一款私服产品，大家可以先去到[Nexus官网](https://link.juejin.cn?target=https%3A%2F%2Fhelp.sonatype.com%2Frepomanager3%2Fdownload)下载一下安装包，Nexus同样是一款解压即用的工具，不过也要注意：**解压的目录中不能存在中文，否则后面启动不起来！**<br />解压完成后，会看到两个目录：

- nexus-x.x.x-xx：里面会放Nexus启动时所需要的依赖、环境配置；
- sonatype-work：存放Nexus运行时的工作数据，如存储上传的jar包等。

接着可以去到：
```
解压目录/etc/nexus-default.properties
```
这个文件修改默认配置，默认端口号是8081，如果你这个端口已被使用，就可以修改一下，否则通常不需要更改。接着可以去到解压目录的bin文件夹中，打开cmd终端，执行启动命令：
```
nexus.exe /run nexus
```
初次启动的过程会额外的慢，因为它需要初始化环境，创建工作空间、内嵌数据库等，直到看见这句提示：<br />![](https://cdn.nlark.com/yuque/0/2023/webp/28163149/1691347630016-8e207845-967e-4712-814d-4efd48a22dfb.webp#averageHue=%230f0f0f&clientId=u237dd497-258d-4&from=paste&id=u9b082250&originHeight=185&originWidth=543&originalType=url&ratio=1.375&rotation=0&showTitle=false&status=done&style=none&taskId=ucc6612f9-3311-4be8-a7ac-e15c3d9a720&title=)<br /> 此时才算启动成功，Nexus初次启动后，会在sonatype-work目录中生成一个/nexus3/admin.password文件，这里面存放着你的初始密码，默认账号就是admin，在浏览器输入：
```
http://localhost:8081
```
访问Nexus界面，接着可以在网页上通过初始密码登录，登录后就会让你修改密码，改完后就代表Nexus搭建成功（不过要记住，改完密码记得重新登录一次，否则后面的操作会没有权限）。
<a name="qtVfC"></a>
### Nexus私服仓库
![](https://cdn.nlark.com/yuque/0/2023/webp/28163149/1691347664382-ac60f69b-8ac6-433f-84c5-dbbaad961134.webp#averageHue=%23f8f8f8&clientId=u237dd497-258d-4&from=paste&id=ua2a9173b&originHeight=321&originWidth=1380&originalType=url&ratio=1.375&rotation=0&showTitle=false&status=done&style=none&taskId=u9e93b40b-4953-47d4-9af4-d6f02ab72f8&title=)登录成功后，点击Browse会看到一些默认仓库，这里稍微解释一下每个字段的含义。

- Name：仓库的名字；
- Type：仓库的类型；
- Format：仓库的格式；
- Status：仓库的状态；
- URL：仓库的网络地址。

重点来说说仓库的分类，总共有四种类型：

| 类型 | 释义 | 作用 |
| --- | --- | --- |
| hosted | 宿主仓库 | 保存中央仓库中没有的资源，如自研组件 |
| proxy | 代理仓库 | 配置中央仓库，即镜像源，私服中没有时会去这个地址拉取 |
| group | 仓库组 | 用来对宿主、代理仓库分组，将多个仓库组合成一个对外服务 |
| virtual | 虚拟仓库 | 并非真实存在的仓库，类似于MySQL中的视图 |

仓库的关系如下：<br />![](https://cdn.nlark.com/yuque/0/2023/webp/28163149/1691347690776-f46eeed6-086a-42e0-b9be-0d3823bb08c1.webp#averageHue=%23fafaf9&clientId=u237dd497-258d-4&from=paste&id=u1f019273&originHeight=461&originWidth=827&originalType=url&ratio=1.375&rotation=0&showTitle=false&status=done&style=none&taskId=u49cdacf1-2ad2-4152-9ea8-2a9d3fa4ada&title=)<br /> 本地的Maven需要配置私服地址，当项目需要的依赖，在本地仓库没有时，就会去到相应的宿主/远程仓库拉取；如果宿主仓库也没有，就会根据配置的代理仓库地址，去到中央仓库拉取，最后依次返回……。
<a name="tidQR"></a>
### Maven配置私服
Maven想要使用私服，需要先修改settings.xml文件，我的建议是别直接改，先拷贝一份出来，接着来讲讲配置步骤。<br />①修改settings.xml里的镜像源配置，之前配的阿里云镜像不能用了，改成：
```xml
<mirror>
  <id>nexus-zhuzi</id>
  <mirrorOf>*</mirrorOf>
  <url>http://localhost:8081/repository/maven-public/</url>
</mirror>
```
②在私服中修改访问权限，允许匿名用户访问，如下：<br />![](https://cdn.nlark.com/yuque/0/2023/webp/28163149/1691347728238-78a8564b-76a4-461c-9635-e7aad9385e7b.webp#averageHue=%23d7d4d4&clientId=u237dd497-258d-4&from=paste&id=u9564021d&originHeight=598&originWidth=811&originalType=url&ratio=1.375&rotation=0&showTitle=false&status=done&style=none&taskId=ud1c232d1-63dc-438e-924c-fd23c27d681&title=)<br /> ③在Nexus私服中配置一下代理仓库地址，即配置镜像源：<br />![](https://cdn.nlark.com/yuque/0/2023/webp/28163149/1691347734971-461ddb31-5d9a-4a36-a760-74932921dadc.webp#averageHue=%23ebe9e9&clientId=u237dd497-258d-4&from=paste&id=u2f1411cd&originHeight=477&originWidth=1282&originalType=url&ratio=1.375&rotation=0&showTitle=false&status=done&style=none&taskId=ub65e75a9-e41c-40ae-9b6a-1559478dd2e&title=)<br /> 将这个默认的中央仓库地址，改为国内的阿里云镜像：
```
http://maven.aliyun.com/nexus/content/groups/public/
```
改完后记得拖动到最下方，点击Save保存一下即可。

 ④在Maven的settings.xml中，配置私服的账号密码：
```xml
<server>
  <id>zhuzi-release</id>
  <username>admin</username>
  <password>你的私服账号密码</password>
</server>

<server>
  <id>zhuzi-snapshot</id>
  <username>admin</username>
  <password>你的私服账号密码</password>
</server>

```
这两组配置，放到<servers>标签中的任何一处即可，这里可以先这样配置，看不懂没关系。
<a name="FVLId"></a>
### 项目配置私服
前面配置好了本地Maven与私服的映射关系，接着要配置项目和私服的连接，说下流程。<br />①为项目创建对应的私服仓库，如果已有仓库，可以直接复用，创建步骤如下：![](https://cdn.nlark.com/yuque/0/2023/webp/28163149/1691347795772-f51e05fb-4d35-44e4-8c0d-18865fd8bd85.webp#averageHue=%23efeeee&clientId=u237dd497-258d-4&from=paste&id=ucaf29b24&originHeight=908&originWidth=1265&originalType=url&ratio=1.375&rotation=0&showTitle=false&status=done&style=none&taskId=u88aa5ede-6422-4536-88ad-f429553d292&title=)其中唯一值得一提的就是仓库格式，这里有三个可选项：

- Release：稳定版，表示存放可以稳定使用的版本仓库；
- Snapshot：快照版，代表存储开发阶段的版本仓库；
- Mixed：混合版，不区分格式，表示混合存储代码的仓库。

为了规范性，我的建议是Release、Snapshot格式的仓库，各自都创建一个。<br />②在Maven工程的pom.xml文件中，配置对应的私服仓库地址，如下：
```xml
<!-- 配置当前工程，在私服中保存的具体位置 -->
<distributionManagement>
    <repository>
        <!-- 这里对应之前 settings.xml 里配置的server-id -->
        <id>zhuzi-release</id>
        <!-- 这里代表私服仓库的地址，大家只需要把后面的名字换掉即可 -->
        <url>http://localhost:8081/repository/zhuzi-release/</url>
    </repository>
    <snapshotRepository>
        <id>zhuzi-snapshot</id>
        <url>http://localhost:8081/repository/zhuzi-snapshot/</url>
    </snapshotRepository>
</distributionManagement>
```
③将当前项目发布到私服仓库，这里可以执行mvn clean deploy命令，也可以通过IDEA工具完成：<br />![](https://cdn.nlark.com/yuque/0/2023/webp/28163149/1691347820779-7ba0844e-2a7d-44c2-9fb5-350af3af0468.webp#averageHue=%23ebeae2&clientId=u237dd497-258d-4&from=paste&id=u201a3165&originHeight=425&originWidth=879&originalType=url&ratio=1.375&rotation=0&showTitle=false&status=done&style=none&taskId=u61531f66-d851-4ff9-905a-41a04c8d285&title=)<br /> 不过这里有一个细节要注意，由于配置了私服上的两个宿主仓库，一个为稳定仓库，另一个为快照仓库，所以发布时，默认会根据当前项目的<version>版本结尾，来选择上传到相应的仓库，例如上图中的结尾是SNAPSHOT，所以会被发布到快照仓库，如果结尾不是这个后缀时，就会被发布到Release仓库。<br />当发布完成后，大家就可以登录Nexus界面，找到对应的宿主仓库，查看相应的jar包信息啦！不过还有一点要注意：你要发布的包不能带有上级，即不能有parent依赖，否则在其他人在拉取该项目时，会找不到其父项目而构建失败。要解决这个问题，可以先将parent项目打包并上传至远程仓库，然后再发布依赖于该parent项目的子模块。
<a name="Drona"></a>
### Nexus配置仓库组
前面在说仓库类型时，还提到过一个“仓库组”的概念，如果你目前所处的公司，是一个大型企业，不同团队都有着各自的宿主仓库，而你恰恰又需要用到其他团队的组件，这时难道需要在pom.xml中，将远程仓库地址先改为其他团队的地址吗？答案是不需要的，这时可以创建一个仓库组。<br />![](https://cdn.nlark.com/yuque/0/2023/webp/28163149/1691347854854-d29c8603-2887-4f68-9ec7-bc4cba7daab7.webp#averageHue=%23f2f0f0&clientId=u237dd497-258d-4&from=paste&id=u71bd7e71&originHeight=809&originWidth=1052&originalType=url&ratio=1.375&rotation=0&showTitle=false&status=done&style=none&taskId=u098f6729-0f08-47c6-a854-546a74db719&title=)大家可以看到，图中的Members区域代表当前仓库组的成员，而这些成员会按照你排列的顺序，具备不同的优先级，越靠前的优先级越高。创建好仓库组后，接着可以去配置一下仓库组，这里有两种方式。
<a name="DZIFq"></a>
#### 配置单个工程与仓库组的映射
这种方式只需修改pom.xml即可：
```xml
<repositories>
  <repository>
    <id>zhuzi-group</id>
    <!-- 配置仓库组的地址 -->
    <url>http://localhost:8081/repository/zhuzi-group/</url>
    <!-- 允许从中拉取稳定版的依赖 -->
    <releases>
      <enabled>true</enabled>
    </releases>
    <!-- 也允许从中拉取快照版的依赖 -->
    <snapshots>
      <enabled>true</enabled>
    </snapshots>
  </repository>
</repositories>

<pluginRepositories>
  <pluginRepository>
    <id>plugin-group</id>
    <url>http://localhost:8081/repository/zhuzi-group/</url>
    <releases>
      <enabled>true</enabled>
    </releases>
    <snapshots>
      <enabled>true</enabled>
    </snapshots>
  </pluginRepository>
</pluginRepositories>

```
在上述这组配置中，配置了<repositories>、<pluginRepositories>两个标签，分别是啥意思呢？很简单，第一个是普通依赖的仓库组地址，第二个是插件依赖的仓库组地址，前者针对于pom.xml中的<dependency>标签生效，后者针对<plugin>标签生效。

当你通过GAV坐标，引入一个依赖时，如果本地仓库中没找到，则会根据配置的仓库组地址，去到Nexus私服上拉取依赖。不过因为仓库组是由多个仓库组成的，所以拉取时，会根据仓库的优先级，依次搜索相应的依赖，第一个仓库将是最优先搜索的仓库。
<a name="yiHsJ"></a>
#### 配置本地Maven与仓库组的映射
一种配置方式，只针对于单个Maven工程生效，如果你所有的Maven工程，都需要与Nexus私服上的仓库组绑定，这时就可以直接修改settings.xml文件，如下：
```xml
<profile>
	<id>zhuzi-group</id>
	<repositories>
		<repository>
			<id>nexus-maven</id>
			<url>http://localhost:8081/repository/zhuzi-group/</url>
			<releases>
				<enabled>true</enabled>
				<updatePolicy>always</updatePolicy>
			</releases>
			<snapshots>
				<enabled>true</enabled>
				<updatePolicy>always</updatePolicy>
			</snapshots>
		</repository>
	</repositories>
 
	<pluginRepositories>
		<pluginRepository>
			<id>nexus-maven</id>
			<url>http://localhost:8081/repository/zhuzi-group/</url>
			<releases>
				<enabled>true</enabled>
				<updatePolicy>always</updatePolicy>
			</releases>
			<snapshots>
				<enabled>true</enabled>
				<updatePolicy>always</updatePolicy>
			</snapshots>
		</pluginRepository>
	</pluginRepositories>
</profile>
```
这组配置要写在<profiles>标签里面，其他的与前一种方式没太大区别，唯一不同的是多了一个<updatePolicy>标签，该标签的作用是指定仓库镜像的更新策略，可选项如下：

- always：每次需要Maven依赖时，都先尝试从远程仓库下载最新的依赖项；
- daily：每天首次使用某个依赖时，从远程仓库中下载一次依赖项；
- interval:X：每隔X个小时，下载一次远程仓库的依赖，X只能是整数；
- never：仅使用本地仓库中已经存在的依赖项，不尝试从远程仓库中拉取。

Maven工程使用依赖时，首先会从本地仓库中查找所需的依赖项，如果本地仓库没有，则从配置的远程仓库下载这时会根据<updatePolicy>策略来决定是否需要从远程仓库下载依赖。<br />不过上述这样配置后，还无法让配置生效，如果想要生效，还得激活一下上述配置：
```xml
<activeProfiles>
    <!-- 这里写前面配置的ID -->
	<activeProfile>zhuzi-group</activeProfile>
</activeProfiles>
```
不过要记住，无论两种方式内的哪一种，都只允许从私服上拉取依赖，如果你的某个工程，想要打包发布到私服上，还是需要配置3.4阶段的<distributionManagement>标签。
