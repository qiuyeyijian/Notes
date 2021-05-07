## 一、SpringBoot入门



### 主程序类，主入口类



**@SpringBootConfiguration**

SpringBoot的配置类，标注在某个类上，表示这是一个SpringBoot的配置类

@EnableAutoConfiguration

==开启自动配置功能==

 

**@AutoConfigurationPackage**

自动配置包

==将主配置类(@SpringBootApplication标注的类)的所在包及下面所有子包里面的所有组件扫描到Spring容器==



**@ConfigurationProperties**

告诉SprigngBoot 将本类中的所有属性和配置文件中相关配置进行绑定

prefix = " ": 配置文件中那个下面的所有属性进行映射





### 使用Spring Initializer 快速创建SpringBoot项目



默认生成的SpringBoot项目：

* 主程序已经生成好了，我们只需要写我们自己的业务逻辑
* resource文件夹的目录结构
  * static： 保存所有的静态资源：js css images;
  * templates: 保存所有的模板页面；（SpringBoot默认jar包使用嵌入式Tomcat，默认不支持jsp页面）；可以使用模板引擎（Freemarker、thymeleaf);
  * application.properties: Spring Boot应用的配置文件；可以修改一些SpringBoot的默认配置，比如端口号等



### YAML 语法

一个例子

```yaml
server:
  port: 8081
```



#### 1. 基本语法

> `key: value` 表示一对键值对，注意key冒号后面一定要有空格

* 以空格缩进来控制层级关系；左对齐的一列数据，都是统一个层级的。

* 缩进时不允许使用Tab键，只允许使用空格

* 缩进的空格数目不重要，只要相同层级的元素左对齐即可

* 大小写敏感

#### 2. YAML支持的三种数据结构

* 对象：键值对的集合
* 数组：一组按次序排列的值（List、set）
* 字面量：单个的，不可再分的值

#### 3. 值得写法

**字面量**

k: v

字符串默认不用加上单引号或者双引号；

“”：双引号：不会转义字符串里面的特殊字符；特殊字符会作为本身想表示的意思。

‘’：单引号：会转义特殊字符，特殊符号最终只是一个普通的字符串数据

**对象：**

对象还是k: v的方式

```yaml
friends:
	lastName: zhangsan
	age: 20
```

行内写法

```yaml
friends: {lastName: zhangsan,age: 10}
```

**数组：**

用减号（-）表示数组中的一个元素, 后面要加空格

```yaml
pets:
 - cat
 - dog
 - dog
```



### 配置文件注入

配置文件

```yaml
person:
  lastName: zhangsan
  age: 18
  boss: false
  birth: 2018/12/12
  maps: {k1: v1,k2: v2}
  lists:
    - lisi
    - zhao
  dog:
    name: xiao
    age: 2
```

javaBean

```yaml

```



  



