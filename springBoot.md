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