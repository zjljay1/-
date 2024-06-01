
[mybatis – MyBatis 3 | 配置](https://mybatis.org/mybatis-3/zh/configuration.html)<br />[mybatis看这一篇就够了，简单全面一发入魂_抠脚的大灰狼的博客-CSDN博客](https://blog.csdn.net/vcj1009784814/article/details/106391982?ops_request_misc=%257B%2522request%255Fid%2522%253A%2522168160700516800192226302%2522%252C%2522scm%2522%253A%252220140713.130102334..%2522%257D&request_id=168160700516800192226302&biz_id=0&utm_medium=distribute.pc_search_result.none-task-blog-2~all~top_positive~default-1-106391982-null-null.142^v83^insert_down38,239^v2^insert_chatgpt&utm_term=mybatis&spm=1018.2226.3001.4187)
<a name="Gix4p"></a>
## 应用场景
<a name="JTlrJ"></a>
### 主键返回
通常我们会将数据库表的主键id设为自增。在插入一条记录时，我们不设置其主键id，而让数据库自动生成该条记录的主键id，那么在插入一条记录后，如何得到数据库自动生成的这条记录的主键id呢？有两种方式

1. 使用useGeneratedKeys和keyProperty属性
```xml
<insert id="insert" parameterType="com.yogurt.po.Student" useGeneratedKeys="true" keyProperty="id">
  INSERT INTO student (name,score,age,gender) VALUES (#{name},#{score},#{age},#{gender});
</insert>
```

2. 使用<selectKey>子标签
```xml
<insert id="insert" parameterType="com.yogurt.po.Student">
        INSERT INTO student (name,score,age,gender) VALUES (#{name},#{score},#{age},#{gender});
        <selectKey keyProperty="id" order="AFTER" resultType="int" >
            SELECT LAST_INSERT_ID();
        </selectKey>
    </insert>
```
如果使用的是mysql这样的支持自增主键的数据库，可以简单的使用第一种方式；对于不支持自增主键的数据库，如oracle，则没有主键返回这一概念，而需要在插入之前先生成一个主键。此时可以用<selectKey>标签，设置其order属性为BEFORE，并在标签体内写上生成主键的SQL语句，这样在插入之前，会先处理<selectKey>，生成主键，再执行真正的插入操作。<br /><selectKey>标签其实就是一条SQL，这条SQL的执行，可以放在主SQL执行之前或之后，并且会将其执行得到的结果封装到入参的Java对象的指定属性上。注意<selectKey>子标签只能用在<insert>和<update>标签中。上面的LAST_INSERT_ID()实际上是MySQL提供的一个函数，可以用来获取最近插入或更新的记录的主键id。
<a name="LKtO5"></a>
### 批量查询
主要是动态SQL标签的使用，注意如果parameterType是List的话，则在标签体内引用这个List，只能用变量名list，如果parameterType是数组，则只能用变量名array

```xml
<select id="batchFind" resultType="student" parameterType="java.util.List">
        SELECT * FROM student
        <where>
            <if test="list != null and list.size() > 0">
                AND id in
                <foreach collection="list" item="id" open="(" separator="," close=")">
                    #{id}
                </foreach>
            </if>
        </where>
</select>

```
**动态SQL**<br />可以根据具体的参数条件，来对SQL语句进行动态拼接。<br />比如在以前的开发中，由于不确定查询参数是否存在，许多人会使用类似于where 1 = 1 来作为前缀，然后后面用AND 拼接要查询的参数，这样，就算要查询的参数为空，也能够正确执行查询，如果不加1 = 1，则如果查询参数为空，SQL语句就会变成SELECT * FROM student where ，SQL不合法。<br />mybatis里的动态标签主要有

- if
```xml
<!-- 示例 -->
<select id="find" resultType="student" parameterType="student">
        SELECT * FROM student WHERE age >= 18
        <if test="name != null and name != ''">
            AND name like '%${name}%'
        </if>
</select>

```

   - 当满足test条件时，才会将<if>标签内的SQL语句拼接上去
- choose<br /> 
```xml
<!-- choose 和 when , otherwise 是配套标签 
类似于java中的switch，只会选中满足条件的一个
-->
<select id="findActiveBlogLike"
     resultType="Blog">
  SELECT * FROM BLOG WHERE state = ‘ACTIVE’
  <choose>
    <when test="title != null">
      AND title like #{title}
    </when>
    <when test="author != null and author.name != null">
      AND author_name like #{author.name}
    </when>
    <otherwise>
      AND featured = 1
    </otherwise>
  </choose>
</select>

```

- trim
   - where
      - <where>标签只会在至少有一个子元素返回了SQL语句时，才会向SQL语句中添加WHERE，并且如果WHERE之后是以AND或OR开头，会自动将其删掉
```xml
<select id="findActiveBlogLike"
     resultType="Blog">
  SELECT * FROM BLOG
  <where>
    <if test="state != null">
         state = #{state}
    </if>
    <if test="title != null">
        AND title like #{title}
    </if>
    <if test="author != null and author.name != null">
        AND author_name like #{author.name}
    </if>
  </where>
</select>
```
<where>标签可以用<trim>标签代替
```xml
<trim prefix="WHERE" prefixOverrides="AND | OR">
  ...
</trim>
```

- set
- 在至少有一个子元素返回了SQL语句时，才会向SQL语句中添加SET，并且如果SET之后是以,开头的话，会自动将其删掉

<set>标签相当于如下的<trim>标签
```xml
<trim prefix="SET" prefixOverrides=",">
   ...
</trim>

```
可以通过<trim>标签更加灵活地对SQL进行定制<br />实际上在mybatis源码，也能看到trim与set,where标签的父子关系<br />![](https://cdn.nlark.com/yuque/0/2023/png/28163149/1681630333286-cd5f7350-d417-4c3b-ba14-d443723de8a1.png#averageHue=%236a8568&clientId=u41973cfe-c2e2-4&from=paste&id=u81281b70&originHeight=127&originWidth=450&originalType=url&ratio=1.100000023841858&rotation=0&showTitle=false&status=done&style=none&taskId=u5b5be133-6ca3-4d51-833f-ef1e7a4c675&title=)

- foreach

用来做迭代拼接的，通常会与SQL语句中的IN查询条件结合使用，注意，到parameterType为List（链表）或者Array（数组），后面在引用时，参数名必须为list或者array。如在foreach标签中，collection属性则为需要迭代的集合，由于入参是个List，所以参数名必须为list
```xml
<select id="batchFind" resultType="student" parameterType="list">
        SELECT * FROM student WHERE id in
        <foreach collection="list" item="item" open="(" separator="," close=")">
          #{item}
        </foreach>
</select>
```
sql<br />可将重复的SQL片段提取出来，然后在需要的地方，使用<include>标签进行引用
```xml
<select id="findUser" parameterType="user" resultType="user">
	SELECT * FROM user
	<include refid="whereClause"/>
</select>

<sql id="whereClause">
     <where>
         <if test="user != null">
         	AND username like '%${user.name}%'
         </if>
     </where>
</sql>

```
bind<br />mybatis的动态SQL都是用OGNL表达式进行解析的，如果需要创建OGNL表达式以外的变量，可以用bind标签
```xml
<select id="selectBlogsLike" resultType="Blog">
  <bind name="pattern" value="'%' + _parameter.getTitle() + '%'" />
  SELECT * FROM BLOG
  WHERE title LIKE #{pattern}
</select>
```
