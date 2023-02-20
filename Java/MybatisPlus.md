# 这是什么
简化dao包的开发， 不用些简单的sql语句，以及使用java代码编写sql语句
简化开发，提高效率
# 配置
spring boot依赖
```xml
<dependency>  
    <groupId>org.springframework.boot</groupId>  
    <artifactId>spring-boot-starter</artifactId>  
</dependency>
```
mysql jia包依赖
```xml
<dependency>  
    <groupId>mysql</groupId>  
    <artifactId>mysql-connector-java</artifactId>  
</dependency>
```
mp依赖
```xml
<dependency>  
    <groupId>com.baomidou</groupId>  
    <artifactId>mybatis-plus-boot-starter</artifactId>  
    <version>3.4.2</version>  
</dependency>
```
alibaba数据源
```xml
<dependency>  
    <groupId>com.alibaba</groupId>  
    <artifactId>druid</artifactId>  
    <version>1.2.4</version>  
</dependency>
```
yaml数据库连接

```xml
spring:  
  datasource:  
    driver-class-name: com.mysql.cj.jdbc.Driver  
    username: root  
    password: root  
    url: jdbc:mysql://localhost:3306/ys?serverTimezone=GMT  
    type: com.alibaba.druid.pool.DruidDataSource
```
# 显示mp生成的sql语句
配置文件中添加如下配置：
```xml
mybatis-plus:  
  configuration:  
    log-impl: org.apache.ibatis.logging.stdout.StdOutImpl
```
#  mp提供的基础增删改查
使当前的接口继承BaseMapper<>
就可以使用它提供的一些简单的增删改查
```java
public interface UserMapper extends BaseMapper<Users> {  
}
```
# 各层注解
@Repository注解在数据层Mapper
@

# 增删改查
## insert
```java
 userMapper.insert(user);
```
## delete
```java
//按照多条件格式删除，使用map键值对存放（"字段名":"条件值"）
Map<String,Object> map = new HashMap<>();  
map.put("id","6");  
map.put("name","值");  
map.put("password","值");  
userMapper.deleteByMap(map);
```
```sql
DELETE FROM users WHERE password = ?   AND name = ?   AND id = ?
```

```java

//按照单条件删除
userMapper.deleteById(6);
//批量删除
List<Long> list = Arrays.asList(6L,7L,8L);  
userMapper.deleteBatchIds(list);
```
```sql
DELETE FROM users WHERE userid IN ( ? , ? , ? )
```
## update

```java
//按照id删除==>若未设置值就不作修改
Users user  =  new Users();  
user.setUserid(5L);  
user.setNickname("宋超阳");  
userMapper.updateById(user);\
```
```sql
UPDATE users SET nickname=? WHERE userid=?
```
## select
### selectById
```java
userMapper.selectById(1);
```
```sql
SELECT userid,username,password,nickname,sex FROM users WHERE userid=?
```
### selectBatchIds
按照id查询多个用户
```java
List<Long> list = Arrays.asList(1l,2l,3l);  
userMapper.selectBatchIds(list);
```
```sql
SELECT userid,username,password,nickname,sex FROM users WHERE userid IN ( ? , ? , ? )
```

### selectByMap
多条件查询,返回值用List接收
```java
Map<String,Object> map = new HashMap<>();  
map.put("sex","f");  
map.put("nickname","管理员01");  
userMapper.selectByMap(map);
```
```sql

SELECT userid,username,password,nickname,sex FROM users WHERE sex = ? AND nickname = ?
```

### selectByList 
使用条件构建器 #条件构造器
1. 定义：在mybatis-plus中提了构造条件的类Wrapper,它可以根据自己的意图定义我们需要的条件。Wrapper是一个抽象类，一般情况下我们用它的子类QueryWrapper来实现自定义条件查询.
2. 生成=>**QueryWrapper wrapper = new QueryWrapper<>();**
3. 调用构造器中的方法实现按条件查询
| 方法名                   | 说明                                | 用法实例                               | sql                            |
| ------------------------ | ----------------------------------- | -------------------------------------- | ------------------------------ |
| eq(R column, Object val) | 等于=                               | eq.put("id","3")                       | id=3                           |
| ne                       | 不等于<>                            | ne.put("id","3")                       | id<> 3                         |
| gt                       | 大于>                               | gt("user_age","18")                    | user_age>18                    |
| ge                       | 大于等于                            | ge(("user_age","18"))                  | user_age>=18                   |
| lt                       | 小于                                | lt(("user_age","18"))                  | user_age<18                    |
| le                       | 小于等于                            | le(("user_age","18"))                  | user_aeg<=18                   |
| between                  | between  值1 and 值2                | between("user_age","18","25")          | user_age between 18 and 25     |
| like                     | like '%值%'                         | like("user_name","可乐")               | like '%可乐%'                  |
| notLike                  | NOT LIKE ' %值%'                    | notLike("user_name","可乐")            | not like '%可乐%'              |
| likeLeft                 | Like '%值'                          | likeLeft("user_name","可乐")           | like '%可乐'                   |
| likeRight                | Like '值%'                          | likeRight("user_name","可乐")          | like '可%'                     |
| isNull                   | 字段 is NULL                        | isNull("user_name")                    | user_name is NULL              |
| groupBy                  | 分组：GROUP BY 字段, …              | groupBy("id","user_age")               | GROUP BY id,user_age           |
| orderByAsc               | 排序【升序】：ORDER BY 字段, … ASC  | orderByAsc(“id”,“user_age”)            | ORDER BY id ASC,user_age ASC   |
| orderByDesc              | 排序【降序】：ORDER BY 字段, … DESC | orderByDesc(“id”,“user_age”)           | ORDER BY id DESC,user_age DESC |
| or()                     | 拼接OR                              | eq(“id”,1).or().eq(“user_age”,25)      | id=1 or user_age=25            |
| and(Consumerconsumer)    | AND嵌套                             | and(i->i.eq(“id”,1).ne(“user_age”,18)) | id = 1 AND user_age <> 25      |
|                          |                                     |                                        |                                |
|                          |                                     |                                        |                                |

---

# 常用注解
## @TableName #tableName
1. @TableNam("数据库表名")-->在实体类上添加-->对应数据库的表名
2. 可以进行一些设置在每个实体类前加统一的前缀，全局配置
```xml
global-config:  
  db-config:  
    table-prefix: 前缀  t_xxx
```
## @TableId #tableId
1. 用途：将属性所对应的字段设置为主键
2. 属性
	1. value（默认）：用于指定数据库中的主键字段
	2. type ：主键生成策略（默认雪花算法(ASSIGN_ID)）|| 自动递增（AUTO）

```java
	@TableId(value = "userid" ,type= IdType.AUTO)
```
3. 设置全局的主键生成策略
```xml
	id-type: auto
```


## @TableField #tableField
指定属性对应的字段名
```java
@TableField（"数据库的字段名"）
```
指定某列不被查询
```java
@TableField（"数据库的字段名",select=false）
```
## @TableLogic #tableLogic
**逻辑删除**：在数据库中和实体类中添加一个字段表示逻辑删除，在实体类中的相应属性的上加@TableLogic标记为判断逻辑删除的字段
				默认标记删除为1，未删除为0
				SQL中delete语句自动转换为update语句
```java
@TableLogic  
private Integer isDelete;
```

# wapper进阶(对上面表格的应用

## 多条件查询
```java
	QueryWrapper<Users> queryWrapper = new QueryWrapper<Users>();  
	//新建一个对象，放置查询条件
	queryWrapper.like("nickname","管")//昵称中带有管字的  
	        .between("username","10001","10005")//username在10001和10005之间的  
	        .isNotNull("username");  //username不为空的
	List<Users> users = userMapper.selectList(queryWrapper);  
	users.forEach(System.out::print);
```
```sql
	==>  Preparing: SELECT userid,username,password,nickname,sex,isdelete FROM users WHERE isdelete=0 AND (nickname LIKE ? AND username BETWEEN ? AND ? AND username IS NOT NULL)
	==> Parameters: %管%(String), 10001(String), 10005(String)
	<==    Columns: userid, username, password, nickname, sex, isdelete
	<==        Row: 2, 10001, 123sa, 管理员01, f, 0
	<==        Row: 3, 10002, 123sa, 管理员01, m, 0
	<==      Total: 2
```
## 组装排序条件
升序降序排序
```java
	QueryWrapper<Users> queryWrapper = new QueryWrapper<Users>();  
	queryWrapper.orderByDesc("username")//降序排序  
	        .orderByAsc("isdelete");//升序排列  
	List<Users> users = userMapper.selectList(queryWrapper);  
	users.forEach(System.out::print);
```
```sql
	==>  Preparing: SELECT userid,username,password,nickname,sex,isdelete FROM users WHERE isdelete=0 ORDER BY username DESC,isdelete ASC
```
## 组装删除功能
 将username列为空的标记为已删除
 ```java
	QueryWrapper<Users> queryWrapper = new QueryWrapper<Users>();  
	queryWrapper.isNull("username");  
	System.out.println(userMapper.delete(queryWrapper));
```
```sql
	==>  Preparing: UPDATE users SET isdelete=1 WHERE isdelete=0 AND (username IS NULL)
	==> Parameters: 
	<==    Updates: 6
```
## 多条件修改
```java
//修改username大于10003且nickname中含有管字，或者isdelete字段为空的性别
	QueryWrapper<Users> queryWrapper = new QueryWrapper<Users>();  
	queryWrapper.gt("username",10003)  
	            .like("nickname","管")  
	            .or()  
	            .isNull("isdelete");  
	Users users = new Users();  
	users.setSex("修改后的性别");  
	userMapper.update(users,queryWrapper);
```
```sql
	==>  Preparing: UPDATE users SET sex=? WHERE isdelete=0 AND (username > ? AND nickname LIKE ? OR isdelete IS NULL)
	==> Parameters: 修改后的性别(String), 10003(Integer), %管%(String)
	<==    Updates: 1
```
## 条件优先级
### lambda表达式
在 mybatisplus中lambda**优先执行**
Lambda表达式函数只有参数列表，和方法体
（参数列表）->{ 方法体 }
->: Lambda运算符，箭头符号，或者goes to
* 小括号内的参数类型可以省略
* 只有一个参数小括号可以省略
* 方法体只有一条语句的时候大括号可以省略！只有一条return语句 return也可以省略

### 多条件设置
```java
	//将username大于10004且小于10009的女士的nickname为空的设置为 小宋的老婆  
	QueryWrapper<Users> queryWrapper = new QueryWrapper<Users>();  
	queryWrapper.eq("sex","f")  
	        .and(i->i.between("username","10004","10009"));  
	Users users = new Users();  
	users.setNickname("小宋的老婆！！！");  
	userMapper.update(users,queryWrapper);
```

```sql
	==>  Preparing: UPDATE users SET nickname=? WHERE isdelete=0 AND (sex = ? AND (username BETWEEN ? AND ?))
	==> Parameters: 小宋的老婆！！！(String), f(String), 10004(String), 10009(String)
```

##  组装SELECT语句
1. 只查询某些字段
```java
	QueryWrapper<Users> queryWrapper = new QueryWrapper<Users>();  
	queryWrapper.select("username","sex","nickname");  
	List<Users> users =  userMapper.selectList(queryWrapper);  
	users.forEach(System.out::print);
```

```sql
	==>  Preparing: SELECT username,sex,nickname FROM users WHERE isdelete=0
```

## 嵌套子查询
```java
	//查询username小于10007的,先查uid再查数据 只查询三个字段  
	QueryWrapper<Users> queryWrapper = new QueryWrapper<Users>();  
	queryWrapper.inSql("username","select username from users")  
	        .select("username","sex","nickname");  
	List<Users> users = userMapper.selectList(queryWrapper);  
	users.forEach(System.out::print);
```
```sql
	SELECT username,sex,nickname FROM users WHERE isdelete=0 AND (username IN (select username from users))
```

## 使用updateWrapper进行多条件查询修改
```java
	//在username中小于10003或者password为空中nickname中包含老婆， 修改nickname为~~，password修改为~~
	UpdateWrapper<Users> updateWrapper = new UpdateWrapper<>();  
	updateWrapper.like("nickname","老婆")  
	        .and(i->i.gt("username","10003")).or().isNull("password");  
	updateWrapper.set("nickname","小宋的媳妇！！！").set("password","不告诉你！");  
	int update = userMapper.update(null, updateWrapper);  
	System.out.printf("影响的行数："+update);`
```
```sql
	==>  Preparing: UPDATE users SET nickname=?,password=? WHERE isdelete=0 AND (nickname LIKE ? AND (username > ?) OR password IS NULL)
	==> Parameters: 小宋的媳妇！！！(String), 不告诉你！(String), %老婆%(String), 10003(String)
```
# SQL的组装条件
 **StringUtils.isBlank(username)** --mp中提供的一个判断字符串的工类，判断是否为空，null ，空格 ===>true
 
```java
	String username = null;  
	String sex = "f";  
	System.out.println(StringUtils.isBlank(username));  
	QueryWrapper<Users> usersQueryWrapper = new QueryWrapper<>();  
	usersQueryWrapper.like(!StringUtils.isBlank(username),"username",username)  
	                 .eq(!StringUtils.isBlank(sex),"sex","f");  
	System.out.println(userMapper.selectList(usersQueryWrapper));
```
```sql
	==>  Preparing: SELECT userid,username,password,nickname,sex,isdelete FROM users WHERE isdelete=0 AND (sex = ?)
	==> Parameters: f(String)
	由于username不符合条件，未被放入语句中
```
# LambdaQueryWrapper 和 LambdaUpdateWrapper

 
1. 我们是写的字符串配置，比如 QueryWrapper.eq(“id”,1);这里的id是数据库表的列名，很有可能我们会写错，但是通过lambda 的方式，**LambdaQueryWrapper.eq(User::getId,1)**，这样就不会有写错的可能了。
2. 经常配和@TableField #tableField 使用
3. 构造方式
```java
//两种方式        
LambdaQueryWrapper queryLambda = new QueryWrapper().lambda();//弃用
LambdaQueryWrapper lambdaQueryWrapper = new LambdaQueryWrapper<>();

//两种方式
LambdaUpdateWrapper updateLambda = new UpdateWrapper().lambda();//弃用
LambdaUpdateWrapper lambdaUpdateWrapper = new LambdaUpdateWrapper();

//一些简单的示例

/**
 * LambdaQueryWrapper
 * SQL实例：SELECT id,user_name,user_age FROM user WHERE (id = ? AND user_age <> ?)
 */
@Test
public void testLambdaQueryWrapper(){
    LambdaQueryWrapper<User> queryLambda = new LambdaQueryWrapper<>();
    queryLambda.eq(User::getId,"1").ne(User::getUserAge,25);
    List<User> users = userMapper.selectList(queryLambda);
    System.out.println(users);
 
}
 
/**
 * LambdaQueryWrapper
 * SQL实例：UPDATE user SET user_name=? WHERE (user_name = ?)
 */
@Test
public void testLambdaUpdateWrapper(){
    User user = new User();
    user.setUserName("LambdaUpdateWrapper");
    LambdaUpdateWrapper<User> userLambdaUpdateWrapper = new LambdaUpdateWrapper<>();
    userLambdaUpdateWrapper.eq(User::getUserName,"IT可乐");
    userMapper.update(user,userLambdaUpdateWrapper);
}```
# 两个插件
## 分页插件
### 配置
1. 新建分页插件的配置类
```java
	@Configuration  
	@MapperScan("com.mp.mapper")  
	public class MpConfig {  
	    @Bean  
	    public MybatisPlusInterceptor mybatisPlusInterceptor(){  
	        MybatisPlusInterceptor mybatisPlusInterceptor = new MybatisPlusInterceptor();  
	        mybatisPlusInterceptor.addInnerInterceptor(new PaginationInnerInterceptor(DbType.MYSQL));  
	        return mybatisPlusInterceptor;  
	    }  
	}
```
2. 使用方法
```java
	void Page(){  
	    Page<Users> page = new Page<>(1,4);  
	    userMapper.selectPage(page,null);  
	}
```
```sql
==>  Preparing: SELECT COUNT(*) FROM users WHERE isdelete = 0
==> Parameters: 
<==    Columns: COUNT(*)
<==        Row: 11
<==      Total: 1
==>  Preparing: SELECT userid,username,password,nickname,sex,isdelete FROM users WHERE isdelete=0 LIMIT ?
==> Parameters: 4(Long)
<==    Columns: userid, username, password, nickname, sex, isdelete
<==        Row: 1,  666888, admin, 管理员01, 修改后的性别, 0
<==        Row: 2, 10001, 123sa, 管理员01, f, 0
<==        Row: 3, 10002, 123sa, 管理员01, m, 0
<==        Row: 4, 10003, 123sa, 汤姆, m, 0
<==      Total: 4
```

3. 自定义分页
```java
	//模拟接收环境  
	Integer nowPage = 1;  
	Integer mesNum = 2;  
	Integer age = 27;  
	//实现  
	Page<Users> page = new Page<>(nowPage,mesNum);  
	LambdaQueryWrapper<Users> lambdaQueryWrapper = new LambdaQueryWrapper<>();  
	lambdaQueryWrapper.lt(age != null,Users::getAge,age);  
	System.out.println(userMapper.selectPage(page, lambdaQueryWrapper));
```
```sql
	==>  Preparing: SELECT COUNT(*) FROM users WHERE isdelete = 0 AND (age < ?)
	==> Parameters: 27(Integer)
	<==    Columns: COUNT(*)
	<==        Row: 6
	<==      Total: 1
	==>  Preparing: SELECT userid,username,password,age,nickname,sex,isdelete FROM users WHERE isdelete=0 AND (age < ?) LIMIT ?
	==> Parameters: 27(Integer), 2(Long)
	<==    Columns: userid, username, password, age, nickname, sex, isdelete
	<==        Row: 2, 10001, 123sa, 12, 管理员01, f, 0
	<==        Row: 4, 10003, 123sa, 23, 汤姆, m, 0
	<==      Total: 2
```
4. page 中常用的属性
	* 
## 代码生成器1
1. 添加对应依赖
```xml
	<dependency>  
	    <groupId>com.baomidou</groupId>  
	    <artifactId>mybatis-plus-generator</artifactId>  
	    <version>3.5.3</version>  
	</dependency>  
	<dependency>  
	    <groupId>org.freemarker</groupId>  
	    <artifactId>freemarker</artifactId>  
	</dependency>
```
## 代码生成器2
![[SpringBoot/assests/Pasted image 20221110113730.png]]
![[SpringBoot/assests/Pasted image 20221110113354.png]]

![[SpringBoot/assests/Pasted image 20221110113451.png]]

# 乐观锁悲观锁
## 乐观锁 
1. 实现：通过添加字段version 版本号实现 ,在pojo类version字段上加注解 
```java
@Version  
private Long version;```
2. 在分页插件里添加一行代码
```java
	public class MpConfig {  
	    @Bean  
	    public MybatisPlusInterceptor mybatisPlusInterceptor(){  
	        MybatisPlusInterceptor mybatisPlusInterceptor = new MybatisPlusInterceptor();  
	        //添加分页插件  
	        mybatisPlusInterceptor.addInnerInterceptor(new PaginationInnerInterceptor(DbType.MYSQL));  
	        //添加乐观锁  timisticLockerInnerInterceptor());  
	        return mybatisPlusInterceptor;  
	    }
```
# 枚举
1. 在枚举类中 在属性前加入注解 @EmunValue #EmunValue 就是要插入数据库的那个属性前面
2. 在配置文件application.yaml中加入对应的配置：mp3.5.2后无需配置
```xml
type-enums-package: 枚举包的位置
```