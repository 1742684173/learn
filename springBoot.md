# 1.给maven配置

给maven的settings.xml配置文件的profile标签添加

```
<profile>
	<id>jdk-1.8</id>
	<activation>
		<activeByDefault>true</activeByDefault>
		<jdk>1.8</jdk>
	</activation>
	<properties>
		<maven.compiler.source>1.8</maven.compiler.source>
		<maven.compiler.target>1.8</maven.compiler.target>
		<maven.compiler.compilerVersion>1.8</maven.compiler.compilerVersion>
	</properties>
</profile>
```



# 2.yml

## 2.1.值的写法

字面量 : 普通的值(数字，字符串，布尔)

 注意：

​	双引号(“”)：不会转义字符串里面的特殊字符，特殊字符会作为本身想表示的意思

​			name: "zhangsan \n lisi" 输出 zhangsan 换行 lisi

​	单引号('')：会转义特殊字符，特殊字符最终只是一个普通的字符串数据

​			name: ‘zhangsan \n lisi’ 输出 zhangsan \n lisi

### 2.1.1.需要在pom.xml导入

```

<!--   导入配置文件处理器，配置文件进行绑定     -->
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-configuration-processor</artifactId>
            <optional>true</optional>
        </dependency>
```

### 2.1.2.application.yml

```
person:
  last-name: 张三
  age: ${random.int}
  boss: false
  birth: 2020/02/02
  map: {k1:v1,k2:v2}
  list: # [1,2]
    - 1
    - 2
  dog: {}
```

### 2.1.3.Person.java

```
/**
 * 将配置文件中配置的每一个属性的值，映射到这个组件中
 * @ConfigurationProperties：告诉SpringBoot将本类中的所有属性和配置文件中相关的配置进行绑定
 *  prefix = "person"：配置文件中所有属性进行一一映射
 *
 * @Component: 只有这个组件是容器中的组件，才提供@ConfigurationPropertiesa功能
 */
@Component
@ConfigurationProperties(prefix = "person")
public class Person {
    private String lastName;
    private Integer age;
    private Boolean boss;
    private Date birth;

    private Map<String,Object> map;
    private List<Object> list;
    private Dog dog;
    
    //get set....
}
```

### 2.1.4.单元测试

```
@RunWith(SpringRunner.class)
@SpringBootTest
public class Test {

    @Autowired
    Person person;

    @org.junit.Test
    public void testYml(){
        System.out.println(person);
    }
}
```



# 3.配置文件注入

## 3.1.@ConfigurationProperties和@Value区别

|                | @ConfigurationProperties | @Value     |
| -------------- | ------------------------ | ---------- |
| 功能           | 批量注入配置文件中的属性 | 一个个指定 |
| 松散绑定       | 支持(驼峰法，_， -)      | 不支持     |
| SpEL           | 不支持                   | 支持       |
| JSR303数据较验 | 支持                     | 不支持     |
| 复杂类型封装   | 支持                     | 不支持     |

配置文件yml和properties他们都能获取到值；

如果说，我们只是在某个业务逻辑中需要获取一下配置文件中的某项值，使用@Value

如果说，我们专门编写了一个javaBean来和配置文件进行映射，就使用@ConfigurationProperties



SpEL：

```
    @Value("张三")
    private String lastName;
    @Value("#{11*2}")
    private Integer age;
    private Boolean boss;
    @Value("${person.birth}")
    private Date birth;
```



## 3.2.@PropertySource 和 @ImportSource

@PropertySource :    加载指定的yml或properties配置文件

```
@PropertySource(value = {"classpath:person.properties"})
@Component
@ConfigurationProperties(prefix = "person")
@Data
public class Person {
    private String lastName;
    private Integer age;
    private Boolean boss;
```



@ImportSource：一般情况下我们自定义的xml配置文件，默认是情况下的bean是不会加载到spring容器中，需要用@ImportSource注解将这个配置文件加载进来

```
@SpringBootApplication
@ImportResource(locations = {"classpath:bean.xml"})
public class MyApplication {
    public static void main(String[] args){
        SpringApplication.run(MyApplication.class);
    }
}
```



区别：

- @PropertySource 加载指定的yml或properties用于给javabean注入值，@ImportSource用于引入.xml类型的配置文件

- @PropertySource一般用在javabean的类名上，@ImportSource一般用于启动类上



## 3.3.Profile

Profile是Spring对不同环境提供不同配置功能的支持，可以通过激活、指定参数等方式快速切换环境

1、多profile文件形式

​	-格式：application-{profile}.properties:

- ​		application-dev.properties、application=prod.properties

2、多profile文档块格式

3、激活方式：

​	-命令行：java -jar 项目.jar --spring.profile.active=dev

​	-配置文件：spring.profiles.active=dev

​	-jvm参数：-Dspring.profiles.active=dev



## 3.4.配置文件加载位置

spring boot启动会扫描以下位置的application.properties或者application.yml文件作为spring boot的默认配置文件

- file:./config/
- file:./
- classpath:/config/
- classpath:/
- 以上是按照优先级从高到低的顺序，所有位置的文件都会被加载，高优先能让配置会覆盖低优先级配置的内容
- 也可以通过配置spring.config.location来改变默认配置



## 3.5.外部配置加载顺序

SpringBoot 也可以人以下位置加载配置：优先级从高到低；高优先级的会覆盖低优先级的配置，所有的配置会形成互补配置

1.命令行参数

多个配置用空格分开：--配置项=值

java -jar  项目.jar --server.port=8087 --server.context-path=/abc

2.来自java:comp/env的JNDI属性

3.java系统属性(System.getProperties())

4.操作环境变量

5.RandomValuePropertySource配置的random.*属性值



**由jar包外向jar包内进行寻找**

**优先加载带profile**

6.jar包外部的application-{profile}.properties或appliction.yml(带spring.profile)配置文件

7.jar包内部的application-{profile}.properties或appliction.yml(带spring.profile)配置文件



**再来加载不带profile**

8.jar包外部的application.properties或appliction.yml(不带spring.profile)配置文件

9.jar包内部的application.properties或appliction.yml不带spring.profile)配置文件



10.@Configuration注解类上的@PropertySource

11.通过SpringApplicationn.SetDefaultProperties指定的默认属性



# 4.自动配置原理

原理：

1. springBoot启动的时候加载主配置类，开启了自动配置功能 @EnableAutoConfiguration

2. @EnableAutoConfiguration的作用

   - 利用EnableAutoConfigurationImportSelector组容器中导入一些组件

   - 实现了selectImports()方法，用来筛选被@Import的Configuration类(减去exclude)

   - List<String> configurations = getCandidateConfigurations(annotationMetadata,attributes)获取候选配置

     ```
     SpringFactoriesLoader.loadFactoryNames()
     扫描所有jar包类路径下 META-INF/spring.factories
     把扫描到的这些文件的内容包装成properties对象
     从properties中获取到EnableAutoConfiguration.class类(类名)对应的值，然后把他们添加到容器中
     ```

     

## 4.1.分析HttpEncodingAutoConfiguration

4.2.通用模式

- -- xxxAutoConfiguration:自动配置类

- xxxProperties: 属性配置类

- yml/properties文件中能配置的什就来源于[属性配置类]

## 4.3.几个重要注解

- @Bean

- @Conditional

## 4.4. debug查看详细的自动配置报告 



## 4.5.写一个自动导入包

![202009140001](.\images\202009140001.jpg)



# 100000.遇见的错误

## 9999.properties中方乱码

file -> settins -> file encodings -> 选中properties ->把编码改成utf-8 -> 并勾选 native-to-asciiconversion



## 10000.Could not autowire. No beans of 'Person' type found.

解决方法：

![image-20200909103727047](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20200909103727047.png)