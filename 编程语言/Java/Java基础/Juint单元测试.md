# 单元测试

## Junit基本操作

定义一个测试类，命名规则

* 测试包名：`xxx.xx.test`，例如`com.qiuyeyijian.test`
* 测试类名：`被测试的类名Test`，例如`CalculatorTest`

定义测试方法，给方法加上注解`@Test`

> * 方法名：`test测试的方法名`，例如`testAdd()`，`testSum()`
> * 返回值：void
> * 参数列表：空参

判定结果：

* 一般我们会使用断言操作来处理结果：`Assert.assertEquals(期望的结果,运算的结果);`

> 其他注解：
>
> * @Before：每个测试方法执行前都会执行一次
>
> * @After: 每个测试方法执行后都会执行一次
> * @BeforeClass：在所有测试方法执行前，先执行。但是所修饰的方法必须是静态的
> * @AfterClass：在所有测试方法执行后，最后执行。但是所修饰的方法必须是静态的

```java
public class Calculator {
    public int add(int a, int b) {
        return a + b;
    }
    public int div(int a , int b) {
        return a / b;
    }
}
```

```java
public class CalculatorTest {
    Calculator c = new Calculator();

    @Test
   public void testAdd() {
        assertEquals(6, c.add(3, 3));
        System.out.println("testAdd");
    }

    @Test
    public void testDiv() {
        assertEquals(2, c.div(6, 3));
        System.out.println("testDiv");
    }

    @Before
    public void before() {
        System.out.println("每个测试方法执行前都会执行我");
    }

    @After
    public void after() {
        System.out.println("每个测试方法执行后都会执行我");
    }

    @BeforeClass
    public static void beforeAll() {
        System.out.println("\n在所有测试方法执行前，最先执行\n");
    }

    @AfterClass
    public static void afterAll() {
        System.out.println("\n在所有测试方法执行后，最后执行\n");
    }
}
```

