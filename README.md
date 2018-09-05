# GreenDao

  greenDAO 是一个将对象映射到 SQLite 数据库中的轻量且快速的 ORM 解决方案。
  

## 一、功能介绍

   性能最大化，可能是  Android  平台上最快的  ORM  框架

   易于使用的 API

   最小的内存开销

   依赖体积小

   支持数据库加密

   强大的社区支持


## 二、使用

### 配置到 Android 项目

在  build.gradle(Module:app) 中添加下面代码：

```
buildscript { 
      repositories {
             mavenCentral()
      }
      dependencies {
              classpath 'org.greenrobot:greendao-gradle-plugin:3.2.1' 
       }
 }

 apply plugin: 'org.greenrobot.greendao'

 dependencies {
         compile 'org.greenrobot:greendao:3.2.0'
 }

```

在 build.gradle(Module:app) 中添加：

```
greendao {   
        schemaVersion 1//数据库版本号    
        daoPackage 'com.com.sky.downloader.greendao'//设置DaoMaster、DaoSession、Dao包名    
        targetGenDir 'src/main/java'//设置DaoMaster、DaoSession、Dao目录   
        //targetGenDirTest：设置生成单元测试目录    
       //generateTests：设置自动生成单元测试用例
}

```

###  实体类 User

 

```
  @Entity
  public class User {   
        @Id(autoincrement = true)   
        private Long id;   
        private String name;   
        private int age;
}

```

相关注解说明：

  实体 @Entity 注解

  schema：告知  GreenDao  当前实体属于哪个  schema
  active：标记一个实体处于活跃状态，活动实体有更新、删除和刷新方法
  nameInDb：在数据库中使用的别名，默认使用的是实体的类名
  indexes：定义索引，可以跨越多个列
  createInDb：标记创建数据库表

基础属性注解

  @Id：主键  Long  型，可以通过 @Id(autoincrement = true) 设置自增长
  @Property：设置一个非默认关系映射所对应的列名，默认是使用字段名，例如：@Property(nameInDb = "name")
  @NotNull：设置数据库表当前列不能为空
  @Transient：添加此标记后不会生成数据库表的列

索引注解

  @Index：使用 @Index 作为一个属性来创建一个索引，通过 name 设置索引别名，也可以通过 unique 给索引添加约束
  @Unique：向数据库添加了一个唯一的约束

关系注解

  @ToOne：定义与另一个实体（一个实体对象）的关系
  @ToMany：定义与多个实体对象的关系

当我们编写好实体类并添加自己需要的注解之后，点击 Make Project 或者 Make Module 'app'，就会项目的build目录下或者自己设定的目录下看到生成的三个类文件：

  DaoMaster
  DaoSession
  UserDao

后面的数据库操作需要借助这三个类来进行，同时在我们的实体类中自动生成了各个属性的  get、set 方法。



###  初始化 GreenDao

一般建议在 Application 中初始化数据库
 
![enter description here](./images/1536131731546.png)
 
 DevOpenHelper 有两个重载方法：

DevOpenHelper(Context context,String name)
DevOpenHelper(Context context,String name,CursorFactory factory)

context  上下文这个不用多说，name数据库的名字，cursorFactory 游标工厂，一般不用，传入null或者使用两个参数的方法即可。我们对外提供一个 getDaoSession() 的方法供外部使用。


(1)  增

  注意：Long 型 id，如果传入 null，则 GreenDao 会默认设置自增长的值。

  insert(User entity)：插入一条记录
 
   ![enter description here](./images/1536131900898.png)
 

(2) 删

  deleteBykey(Long key) ：根据主键删除一条记录。
  delete(User entity) ：根据实体类删除一条记录，一般结合查询方法，查询出一条记录之后删除。
  deleteAll()： 删除所有记录。
 
  ![enter description here](./images/1536132240954.png)

(3) 改

  update(User entity)：更新一条记录

  ![enter description here](./images/1536132285776.png)

(4) 查
 
  loadAll()：查询所有记录
  load(Long key)：根据主键查询一条记录
  queryBuilder().list()：返回：List
  queryBuilder().where(UserDao.Properties.Name.eq("")).list()：返回：List
  queryRaw(String where,String selectionArg)：返回：List
