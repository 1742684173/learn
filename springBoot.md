---

---

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

- ​		application-dev.properties、application-prod.properties

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

```
@Configuration //表示这是一个配置类，以前编写的一配置文件一样，也可以给容器中添加组件

@EnableConfigurationProperties({HttpEncodingProperties.class}) //启用指定类的ConfigurationProperties的功能；将配置文件中对应的值和HttpEncodingProperties绑定起来

@ConditionalOnWebApplication

@ConditionalOnClass({CharacterEncodingFilter.class}) //判断当项目中有没有这个类CharacterEncodingFilter ：springMVC中进行乱码解决的过滤器

@ConditionalOnProperty(
    prefix = "spring.http.encoding",
    value = {"enabled"},
    matchIfMissing = true
) //判断配置文件中是否存在某个配置，spring.http.encoding.enabled;
public class HttpEncodingAutoConfiguration {
	//已经和SpringBoot的配置文件映射了
	private final HttpEncodingProperties properties;

	//只有一个有参构造器的情况下，参数的值就会从容器中拿
    public HttpEncodingAutoConfiguration(HttpEncodingProperties properties) {
        this.properties = properties;
    }
    
    @Bean //给容器中添加一个组件，这个组件的某些值需要从properties中获取
    @ConditionalOnMissingBean({CharacterEncodingFilter.class})
    public CharacterEncodingFilter characterEncodingFilter() {
        CharacterEncodingFilter filter = new OrderedCharacterEncodingFilter();
        filter.setEncoding(this.properties.getCharset().name());
        filter.setForceRequestEncoding(this.properties.shouldForce(Type.REQUEST));
        filter.setForceResponseEncoding(this.properties.shouldForce(Type.RESPONSE));
        return filter;
    }

```



```
# 我们配置的属性都是来源于这个功能的properties类
spring.http.encoding.enabled=true
spring.http.encoding.charset=UTF-8
spring.http.encoding.force=true
```



- -- xxxAutoConfiguration:类都是容器中的一个组件，都加入到容器中；用他们来做自动配置

  都是一个

- xxxProperties: 属性配置类

- yml/properties文件中能配置的什就来源于[属性配置类]

总结：

1. SpringBoot启动会加载大量的自动配置类
2. 我们看我们需要的功能有没有SpringBoot默认的自动配置类
3. 我们再来看这个自动配置类中到底配置了哪些组件：(只要我们要用的组件有，我们就不需要再来配置了)
4. 给容器中自动配置类添加组件的时候，会从properties类中获取某些属性，我们就可以在配置中指定这些属性的值

## 4.3.几个重要注解

- @Bean

- @Conditional

## 4.4. debug查看详细的自动配置报告 

可以通过启用debug=true属性来让控制台打印自动配置报告，这样我们就可以很方便的知道哪些自动配置类生效



```
Positive matches: //自动配置类是生效的
-----------------

   CodecsAutoConfiguration matched:
      - @ConditionalOnClass found required class 'org.springframework.http.codec.CodecConfigurer' (OnClassCondition)

   CodecsAutoConfiguration.JacksonCodecConfiguration matched:
      - @ConditionalOnClass found required class 'com.fasterxml.jackson.data
      
Negative matches: //自动配置类是没有生效的
-----------------

   ActiveMQAutoConfiguration:
      Did not match:
         - @ConditionalOnClass did not find required class 'javax.jms.ConnectionFactory' (OnClassCondition)

   AopAutoConfiguration:
```



## 4.5.写一个自动导入包

![202009140001](.\images\202009140001.jpg)

```
public class Hello {
    private String msg;

    public String sayHello(){
        return  "hello " + this.msg;
    }

    public String getMsg() {
        return msg;
    }

    public void setMsg(String msg) {
        this.msg = msg;
    }
}
```

```
@Configuration
//为带有@ConfigurationProperties注解的Bean提供有效的支持
//这个注解可以提供一种方便的方式来将带有@ConfigurationProperties注解的类注入到为spring容器在的Bean
@EnableConfigurationProperties(HelloProperties.class)//开启属性注入，通过@Autowared注入
@ConditionalOnClass(Hello.class)
@ConditionalOnProperty(prefix = "hello",value="enabled",matchIfMissing = true)
public class HelloAutoConfiguration {

    @Autowired
    private HelloProperties helloProperties;

    @Bean
    @ConditionalOnMissingBean(Hello.class)//容器中如果没有Hello这个类那么自动适配这个Hello
    public Hello hello(){
        Hello hello = new Hello();
        hello.setMsg(helloProperties.getMsg());
        return hello;
    }
}
```

```
@ConfigurationProperties(prefix = "hello")
public class HelloProperties {
    private static final String MSG = "world";

    private String msg = MSG;

    public String getMsg() {
        return msg;
    }

    public void setMsg(String msg) {
        this.msg = msg;
    }
}
```

spring.factories

```
org.springframework.boot.autoconfigure.EnableAutoConfiguration
com.aloogn.test.HelloAutoConfiguration
```

使用**mvn package**将项目打包，使用**mvn install:install-file**命令将打包文件上传到本地maven仓库

如：

```
mvn install:install-file -Dfile C:\Users\Administrator\Desktop\spring-boot-starter-hello-1.0-SNAPSHOT.jar -DgroupId=com.aloogn.test -DartifactId=spring-boot-starter-hello -Dversion=1.0.0   -Dpackaging=jar
```



# 5.日志框架

https://www.cnblogs.com/xieyupeng/p/9665254.html



![202009180001](.//images/202009180001.jpg)

SpringBoot：底层是Spring框架，Spring框架默认是用JCL; SpringBoot选用SLF4j和logback

## 5.1、SLF4j使用

### 5.1.1.如何在系统中使用SLF4j

以后开发的时候，日志记录方法的使用，不应该直接使用日志的实现类，而是调用日志抽象层里面的方法

```
public class Test {
    public static void main(String[] args){
        Logger logger = LoggerFactory.getLogger(Test.class);
        logger.info("hello wolrd");
    }
}
```

如何让系统中所有的日志都统一到slf4j:

1.将系统中其它日志框架先排除出去；

2.用中间包来替换原有的日志框架；

3.我们导入slf4j其它的实现；



## 5.1.2.日志依赖关系

![202009190001](.\images\202009190001.jpg)

SpringBoot使用它来做日志功能

```
 <!--  日志功能    -->
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-logging</artifactId>
        </dependency>
```



 1）、SpringBoot底层也是使用slf4j+logback的方式进行日志记录

 2）、SpringBoot也把其他的日志都替换成了slf4j；

 3）、中间替换包？

```
public abstract class LogFactory {;
    static LogFactory logFactory = new SLF4JLogFactory();
```

4）、如果我们要引入其它框架？一定要把这个框架的默认日志依赖移除掉

​		Spring框架用的是commons-logging:

```
	<dependency>
            <groupId>org.springframework</groupId>
            <artifactId>spring-core</artifactId>
            <exclusions>
                <exclusion>
                    <groupId>commons-logging</groupId>
                    <artifactId>commons-logging</artifactId>
                </exclusion>
            </exclusions>
        </dependency>
```

```
//日志的级别；
        //由低到高   trace<debug<info<warn<error
        //可以调整输出的日志级别；日志就只会在这个级别以以后的高级别生效
        logger.trace("这是trace日志...");
        logger.debug("这是debug日志...");
        //SpringBoot默认给我们使用的是info级别的，没有指定级别的就用SpringBoot默认规定的级别；root级别
        logger.info("这是info日志...");
        logger.warn("这是warn日志...");
        logger.error("这是error日志...");
```

```
日志输出格式：
        %d表示日期时间，
        %thread表示线程名，
        %-5level：级别从左显示5个字符宽度
        %logger{50} 表示logger名字最长50个字符，否则按照句点分割。 
        %msg：日志消息，
        %n是换行符
    -->
    %d{yyyy-MM-dd HH:mm:ss.SSS} [%thread] %-5level %logger{50} - %msg%n
```

```
logging.level.com.aloogn=trace
​
​
#logging.path=
# 不指定路径在当前项目下生成springboot.log日志
# 可以指定完整的路径；
#logging.file=G:/springboot.log
​
# 在当前磁盘的根路径下创建spring文件夹和里面的log文件夹；使用 spring.log 作为默认文件
logging.path=/spring/log
​
#  在控制台输出的日志的格式
logging.pattern.console=%d{yyyy-MM-dd} [%thread] %-5level %logger{50} - %msg%n
# 指定文件中日志输出的格式
logging.pattern.file=%d{yyyy-MM-dd} === [%thread] === %-5level === %logger{50} ==== %msg%n
```



# 6.静态资源加载

## 6.1.分析WebMvcAutoConfiguration

https://www.pianshen.com/article/42271132832/

```
# 静态加载路径,默认"classpath:/META-INF/resources/", "classpath:/resources/", "classpath:/static/", "classpath:/public/"

spring.resources.static-locations=classpath:/templates/
```

## 6.2.SpringMvc默认配置EnableWebMvcConfiguration

EnableWebMvcConfiguration是SpringMvc的自动配置



# 7.国际化







# 100000.遇见的错误



## 9999.properties中方乱码

file -> settins -> file encodings -> 选中properties ->把编码改成utf-8 -> 并勾选 native-to-asciiconversion



## 10000.Could not autowire. No beans of 'Person' type found.

解决方法：

![image-20200909103727047](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20200909103727047.png)