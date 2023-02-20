# 这是什么
简化pojo的开发
# 配置
pom添加依赖，简化实体类
```xml
<dependency>  
    <groupId>org.projectlombok</groupId>  
    <artifactId>lombok</artifactId>  
    <version>1.18.18</version>  
</dependency>
```
 ![[SpringBoot/assests/Pasted image 20221107201400.png]] 
```java
@Getter//getter and setter 方法  
@Setter//  
@ToString//输出方法  
@EqualsAndHashCode//对比值  
@NoArgsConstructor//无参构造  
@AllArgsConstructor//全参构造  
@RequiredArgsConstructor//必须参数生成构造函数  
@Data//一键以上所有 
@Builder//将类转变为建造者模式
@Accessors//用法类似上面
@Value//为属性添加finally属性  
@Slf4j//输出日志
@RequiredArgsConstructor//注解在类，为类中需要特殊处理的属性生成构造方法，比如final和被@NonNull注解的属性：
在controller上加上RequiredArgsConstructor注解，mapper前加上final；就可以当个AutoWride用
```
#Getter #Setter #ToString #EqualsAndHashCode #NoArgsConstructor #AllArgsConstructor #RequiredArgsConstructor #Value #Slf4j #RequiredArgsConstructor
