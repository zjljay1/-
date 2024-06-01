
[SpringBoot 多模块项目构建（父/子模块） - 掘金](https://juejin.cn/post/7102790032767287303#heading-7)
<a name="AWRl8"></a>
## 1. 多模块项目
使用 SpringBoot 开发 Web 项目，如果项目整体不太复杂，无需使用微服务架构，为了开发的便利性可以采用 Maven 的多模块项目结构。<br />SpringBoot 的多模块项目就是基于 Maven 管理、对项目按照功能或者层级结构进行拆分，降低项目耦合性，抽取公共模块，实现一处开发多处引用，提高代码复用率和开发效率，更利于项目后期的维护和升级。
<a name="vLNxm"></a>
## 2. 项目结构初始化
<a name="ZwQLb"></a>
### 2.1 创建父工程
创建多模块项目时首先要创建最上层父工程，并用来统一管理子模块，创建方法流程为：

1. 使用 IDEA 编译器，选择文件 -> New -> 新建项目，选择 Spring Initializr，点击下一步，
2. 设置合适的 Group 名称和 Artifact 名称（两者最终代表包路径），选择合适的 Java 版本并点击下一步，
3. 选择合适的起步依赖，也可以不选择后续使用 maven 坐标手动添加，点击下一步，
4. 为项目设置合适的名称，并选择项目在本地生成的根目录，点击完成
5. 初始化生成 SpringBoot 项目

得到父工程项目后，由于父工程只用来统一管理多模块，并不会进行代码编写，因此会将父工程目录结构中无用的文件和文件夹删除

- 删除 .mvn 和 src 文件夹
- 删除 mvnw 和 mvnw.cmd 文件
- 留下 pom.xml 作为父级依赖设置，进行统一依赖管理
<a name="PB44n"></a>
### 2.2 创建子模块
父工程创建成功后，可以在此基础上新建子模块，在父项目上点击鼠标右键，选择新建 -> 模块 -> 新建模块，选择 Spring Initializr，之后便和创建 SpringBoot 项目一致，定义组织名称、选择起步依赖、定义项目名称，最后完成创建得到一个子模块项目。<br />在创建子模块项目时有几点需要注意:

- 子模块创建时 Group 、 Artifact 和 Name 不要与父工程重复，否则关联时会出现冲突
- 子模块的模块名会在目录结构中展示，并使用模块名进行父子关联
- 自定义子模块名称时，对应的的**内容根目录**和**模块文件位置**要跟随变化，如果设置成了父工程目录，则会覆盖父工程文件导致结构错误！

完成子模块的创建后，在项目中就会出现如下的目录结构<br />![](https://cdn.nlark.com/yuque/0/2023/webp/28163149/1682995702509-9f56b165-cb26-416b-9fad-cbc2ad3f9696.webp#averageHue=%233d4246&clientId=uecceed0b-d50d-4&from=paste&id=u0c907985&originHeight=435&originWidth=299&originalType=url&ratio=1.375&rotation=0&showTitle=false&status=done&style=none&taskId=u2e9ada24-27b6-4838-8dbd-435c75e6208&title=)
<a name="HG01r"></a>
### 2.3 关联父子模块
到目前为止，整个项目结构上满足了父工程、子模块，要想真正实现父子模块关联还需要进一步设置。<br />**父工程配置**

- 父工程的 pom.xml 中，设置 pom，代表打包类型为 pom
- 父工程的 pom.xml 中，使用 标签定义子模块进行关联 
- **注意**：父工程的 pom.xml 中的 标签中引用了的 springboot ，这是 SpringBoot 项目默认需要引用的顶级工程，不可以删除，如果删除会导致项目运行失败！

**子模块配置**

- 子模块的 pom.xml 中， 标签内容修改为父工程的 groupId、artifacId 等信息
- 子模块的 pom.xml 中，使用 jar 设置打包为 jar 格式（可不加，在创建模块时会有选择默认打包类型）
<a name="KSZhn"></a>
## 3. 项目依赖的统一管理
<a name="EzUDb"></a>
### 3.1 默认引入依赖
**起步依赖**<br />在创建项目模块时，如果不选择任何初始依赖，那么 SpringBoot 项目只会默认加入 spring-boot-starter 和 spring-boot-starter-test 两个依赖信息
```xml
<modules>
    <module>child_module_name</module>
</modules>
```

```xml
<dependencies>
    <dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter</artifactId>
    </dependency>
    
    <dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-test</artifactId>
    <scope>test</scope>
    </dependency>
</dependencies>
```
**引入依赖**<br />对于 Web 项目，还需要增加 web 依赖信息，可以在父工程的 pom.xml 文件中的 标签中添加如下依赖信息<br />添加完成后，则所有子模块都会继承该依赖信息自动生效，无需额外引入。
<a name="f2ehr"></a>
### 3.2 统一管理依赖
既然是父-子工程的结构，可以将项目中用到的依赖统一在父工程项目中进行管理，统一依赖和版本号信息。

1. pom.xml 文件中，可以使用 标签自定义相关依赖的版本号

2 然后可以使用 标签统一声明用到的依赖信息，其中的版本号使用 标签定义变量

1. 每个子模块需要使用依赖是只需要在 标签中加入依赖 groupId 和 artifactId 信息即可，版本号会自动使用父工程声明的版本

配置完成之后，如果有升级依赖 jar 版本的需求，则只需要在父工程中更新对应依赖的版本信息即可。
<a name="NNXp8"></a>
## 4. 项目运行
多模块的 SpringBoot 项目构建完成并配置完成后，且引入了 web 项目必须的依赖，就可以验证项目是否可以正常运行。

- 验证运行：由于父工程没有代码启动类，因此只需要进入到子模块中，找到对应的启动类运行，如果运行成功，说明子模块运行依赖等配置成功；

![](https://cdn.nlark.com/yuque/0/2023/webp/28163149/1682995702366-2982c6c7-c2aa-44f2-86f1-42e9c490b8ca.webp#averageHue=%23413f3a&clientId=uecceed0b-d50d-4&from=paste&id=u03bee86d&originHeight=114&originWidth=1736&originalType=url&ratio=1.375&rotation=0&showTitle=false&status=done&style=none&taskId=u3d348407-9798-46a5-9a46-8cc60bc7060&title=)

- 验证打包：打包流程使用 maven 工具管理，只需要在 IDEA 右侧打开 maven 工具栏，在父工程下的 Lifecycle 声明周期中，执行 clean、package、install 等阶段，控制台输出执行成功日志说明打包完成。

![](https://cdn.nlark.com/yuque/0/2023/webp/28163149/1682995702358-b0c51e5d-459f-47e3-a68a-fece0277774a.webp#averageHue=%232e2e2e&clientId=uecceed0b-d50d-4&from=paste&id=u0b139f6e&originHeight=259&originWidth=851&originalType=url&ratio=1.375&rotation=0&showTitle=false&status=done&style=none&taskId=u6d76464a-bd0a-4c5d-a900-29ff5e5f43c&title=)<br />如果需要新增子模块，则按照相同的方式初始化模块，关联父工程，并在父工程中增加 标签对应即可。

