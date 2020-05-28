



### Junit 单元测试

```
* 测试分类：
  1. 黑盒测试：不需要写代码，给输入值，看程序是否能够输出期望的值。
  2. 白盒测试：需要写代码的。关注程序具体的执行流程。

* Junit使用：白盒测试
  * 步骤：
    1. 定义一个测试类(测试用例)
       * 建议：
         * 测试类名：被测试的类名Test		CalculatorTest
         * 包名：xxx.xxx.xx.test		cn.itcast.test

    2. 定义测试方法：可以独立运行
       * 建议：
         * 方法名：test测试的方法名		testAdd()  
         * 返回值：void
         * 参数列表：空参

    3. 给方法加@Test
    4. 导入junit依赖环境

  * 判定结果：
    * 红色：失败
    * 绿色：成功
    * 一般我们会使用断言操作来处理结果
      * Assert.assertEquals(期望的结果,运算的结果);

  * 补充：
    * @Before:
      * 修饰的方法会在测试方法之前被自动执行
    * @After:
      * 修饰的方法会在测试方法执行之后自动被执行

```



### 反射：框架设计的灵魂

	* 框架：半成品软件。可以在框架的基础上进行软件开发，简化编码
	* 反射：将类的各个组成部分封装为其他对象，这就是反射机制
		* 好处：
			1. 可以在程序运行过程中，操作这些对象。
			2. 可以解耦，提高程序的可扩展性。


	* 获取Class对象的方式：
		1. Class.forName("全类名")：将字节码文件加载进内存，返回Class对象
			* 多用于配置文件，将类名定义在配置文件中。读取文件，加载类
		2. 类名.class：通过类名的属性class获取
			* 多用于参数的传递
		3. 对象.getClass()：getClass()方法在Object类中定义着。
			* 多用于对象的获取字节码的方式
	
		* 结论：
			同一个字节码文件(*.class)在一次程序运行过程中，只会被加载一次，不论通过哪一种方式获取的Class对象都是同一个。


	* Class对象功能：
		* 获取功能：
			1. 获取成员变量们
				* Field[] getFields() ：获取所有public修饰的成员变量
				* Field getField(String name)   获取指定名称的 public修饰的成员变量
	
				* Field[] getDeclaredFields()  获取所有的成员变量，不考虑修饰符
				* Field getDeclaredField(String name)  
			2. 获取构造方法们
				* Constructor<?>[] getConstructors()  
				* Constructor<T> getConstructor(类<?>... parameterTypes)  
	
				* Constructor<T> getDeclaredConstructor(类<?>... parameterTypes)  
				* Constructor<?>[] getDeclaredConstructors()  
			3. 获取成员方法们：
				* Method[] getMethods()  
				* Method getMethod(String name, 类<?>... parameterTypes)  
	
				* Method[] getDeclaredMethods()  
				* Method getDeclaredMethod(String name, 类<?>... parameterTypes)  
	
			4. 获取全类名	
				* String getName()  


	* Field：成员变量
		* 操作：
			1. 设置值
				* void set(Object obj, Object value)  
			2. 获取值
				* get(Object obj) 
	
			3. 忽略访问权限修饰符的安全检查
				* setAccessible(true):暴力反射



	* Constructor:构造方法
		* 创建对象：
			* T newInstance(Object... initargs)  
	
			* 如果使用空参数构造方法创建对象，操作可以简化：Class对象的newInstance方法


	* Method：方法对象
		* 执行方法：
			* Object invoke(Object obj, Object... args)  
	
		* 获取方法名称：
			* String getName:获取方法名


	* 案例：
		* 需求：写一个"框架"，不能改变该类的任何代码的前提下，可以帮我们创建任意类的对象，并且执行其中任意方法
			* 实现：
				1. 配置文件
				2. 反射
			* 步骤：
				1. 将需要创建的对象的全类名和需要执行的方法定义在配置文件中
				2. 在程序中加载读取配置文件
				3. 使用反射技术来加载类文件进内存
				4. 创建对象
				5. 执行方法

### 注解：

	* 概念：说明程序的。给计算机看的
	* 注释：用文字描述程序的。给程序员看的
	
	* 定义：注解（Annotation），也叫元数据。一种代码级别的说明。它是JDK1.5及以后版本引入的一个特性，与类、接口、枚举是在同一个层次。它可以声明在包、类、字段、方法、局部变量、方法参数等的前面，用来对这些元素进行说明，注释。
	* 概念描述：
		* JDK1.5之后的新特性
		* 说明程序的
		* 使用注解：@注解名称


​	

	* 作用分类：
		①编写文档：通过代码里标识的注解生成文档【生成文档doc文档】
		②代码分析：通过代码里标识的注解对代码进行分析【使用反射】
		③编译检查：通过代码里标识的注解让编译器能够实现基本的编译检查【Override】


	* JDK中预定义的一些注解
		* @Override	：检测被该注解标注的方法是否是继承自父类(接口)的
		* @Deprecated：该注解标注的内容，表示已过时
		* @SuppressWarnings：压制警告
			* 一般传递参数all  @SuppressWarnings("all")
	
	* 自定义注解
		* 格式：
			元注解
			public @interface 注解名称{
				属性列表;
			}
	
		* 本质：注解本质上就是一个接口，该接口默认继承Annotation接口
			* public interface MyAnno extends java.lang.annotation.Annotation {}
	
		* 属性：接口中的抽象方法
			* 要求：
				1. 属性的返回值类型有下列取值
					* 基本数据类型
					* String
					* 枚举
					* 注解
					* 以上类型的数组
	
				2. 定义了属性，在使用时需要给属性赋值
					1. 如果定义属性时，使用default关键字给属性默认初始化值，则使用注解时，可以不进行属性的赋值。
					2. 如果只有一个属性需要赋值，并且属性的名称是value，则value可以省略，直接定义值即可。
					3. 数组赋值时，值使用{}包裹。如果数组中只有一个值，则{}可以省略
		
		* 元注解：用于描述注解的注解
			* @Target：描述注解能够作用的位置
				* ElementType取值：
					* TYPE：可以作用于类上
					* METHOD：可以作用于方法上
					* FIELD：可以作用于成员变量上
			* @Retention：描述注解被保留的阶段
				* @Retention(RetentionPolicy.RUNTIME)：当前被描述的注解，会保留到class字节码文件中，并被JVM读取到
			* @Documented：描述注解是否被抽取到api文档中
			* @Inherited：描述注解是否被子类继承


	* 在程序使用(解析)注解：获取注解中定义的属性值
		1. 获取注解定义的位置的对象  （Class，Method,Field）
		2. 获取指定的注解
			* getAnnotation(Class)
			//其实就是在内存中生成了一个该注解接口的子类实现对象
	
		            public class ProImpl implements Pro{
		                public String className(){
		                    return "cn.itcast.annotation.Demo1";
		                }
		                public String methodName(){
		                    return "show";
		                }
		            }
		3. 调用注解中的抽象方法获取配置的属性值


	* 案例：简单的测试框架
	* 小结：
		1. 以后大多数时候，我们会使用注解，而不是自定义注解
		2. 注解给谁用？
			1. 编译器
			2. 给解析程序用
		3. 注解不是程序的一部分，可以理解为注解就是一个标签


## JDBC：

	1. 概念：Java DataBase Connectivity  Java 数据库连接， Java语言操作数据库
		* JDBC本质：其实是官方（sun公司）定义的一套操作所有关系型数据库的规则，即接口。各个数据库厂商去实现这套接口，提供数据库驱动jar包。我们可以使用这套接口（JDBC）编程，真正执行的代码是驱动jar包中的实现类。
	
	2. 快速入门：
		* 步骤：
			1. 导入驱动jar包 mysql-connector-java-5.1.37-bin.jar
				1.复制mysql-connector-java-5.1.37-bin.jar到项目的libs目录下
				2.右键-->Add As Library
			2. 注册驱动
			3. 获取数据库连接对象 Connection
			4. 定义sql
			5. 获取执行sql语句的对象 Statement
			6. 执行sql，接受返回结果
			7. 处理结果
			8. 释放资源

* 代码实现：

```
public class JdbcDemo01 {
    public static void main(String[] args) throws Exception {
        //1. 导入jar包
        //2. 注册驱动
        Class.forName("com.mysql.jdbc.Driver");
        //3. 获取数据库连接对象
        Connection conn = DriverManager.getConnection(
                "jdbc:mysql://localhost:3306/dbname?serverTimezone=GMT&useUnicode=true&characterEncoding=utf-8&useSSL=false",
                "usrname",
                "password"
        );
        //4. 定义sql语句
        String sql = "update titles set price = 88 where isbn = 9787121072984";
        //5. 获取执行sql的对象 Statement
        Statement statement = conn.createStatement();
        //6. 执行sql, 返回的结果是影响的行数
        int count = statement.executeUpdate(sql);
        //7. 处理结果
        System.out.println(count);
        //8. 释放资源
        statement.close();
        conn.close();
    }
}
```

	3. 详解各个对象：
		1. DriverManager：驱动管理对象
			* 功能：
				1. 注册驱动：告诉程序该使用哪一个数据库驱动jar
					static void registerDriver(Driver driver) :注册与给定的驱动程序 DriverManager 。 
					写代码使用：  Class.forName("com.mysql.jdbc.Driver");
					通过查看源码发现：在com.mysql.jdbc.Driver类中存在静态代码块
					 static {
					        try {
					            java.sql.DriverManager.registerDriver(new Driver());
					        } catch (SQLException E) {
					            throw new RuntimeException("Can't register driver!");
					        }
						}
	
					注意：mysql5之后的驱动jar包可以省略注册驱动的步骤。
				2. 获取数据库连接：
					* 方法：static Connection getConnection(String url, String user, String password) 
					* 参数：
						* url：指定连接的路径
							* 语法：jdbc:mysql://ip地址(域名):端口号/数据库名称
							* 例子：jdbc:mysql://localhost:3306/db3
							* 细节：如果连接的是本机mysql服务器，并且mysql服务默认端口是3306，则url可以简写为：jdbc:mysql:///数据库名称
						* user：用户名
						* password：密码 
		2. Connection：数据库连接对象
			1. 功能：
				1. 获取执行sql 的对象
					* Statement createStatement()
					* PreparedStatement prepareStatement(String sql)  
				2. 管理事务：
					* 开启事务：setAutoCommit(boolean autoCommit) ：调用该方法设置参数为false，即开启事务
					* 提交事务：commit() 
					* 回滚事务：rollback() 
		3. Statement：执行sql的对象
			1. 执行sql
				1. boolean execute(String sql) ：可以执行任意的sql 了解 
				2. int executeUpdate(String sql) ：执行DML（insert、update、delete）语句、DDL(create，alter、drop)语句
					* 返回值：影响的行数，可以通过这个影响的行数判断DML语句是否执行成功 返回值>0的则执行成功，反之，则失败。
				3. ResultSet executeQuery(String sql)  ：执行DQL（select)语句
			2. 练习：
				1. account表 添加一条记录
				2. account表 修改记录
				3. account表 删除一条记录
	
				代码：
					Statement stmt = null;
			        Connection conn = null;
			        try {
			            //1. 注册驱动
			            Class.forName("com.mysql.jdbc.Driver");
			            //2. 定义sql
			            String sql = "insert into account values(null,'王五',3000)";
			            //3.获取Connection对象
			            conn = DriverManager.getConnection("jdbc:mysql:///db3", "root", "root");
			            //4.获取执行sql的对象 Statement
			            stmt = conn.createStatement();
			            //5.执行sql
			            int count = stmt.executeUpdate(sql);//影响的行数
			            //6.处理结果
			            System.out.println(count);
			            if(count > 0){
			                System.out.println("添加成功！");
			            }else{
			                System.out.println("添加失败！");
			            }
			
			        } catch (ClassNotFoundException e) {
			            e.printStackTrace();
			        } catch (SQLException e) {
			            e.printStackTrace();
			        }finally {
			            //stmt.close();
			            //7. 释放资源
			            //避免空指针异常
			            if(stmt != null){
			                try {
			                    stmt.close();
			                } catch (SQLException e) {
			                    e.printStackTrace();
			                }
			            }
			
			            if(conn != null){
			                try {
			                    conn.close();
			                } catch (SQLException e) {
			                    e.printStackTrace();
			                }
			            }
			        }
				
		4. ResultSet：结果集对象,封装查询结果
			* boolean next(): 游标向下移动一行，判断当前行是否是最后一行末尾(是否有数据)，如果是，则返回false，如果不是则返回true
			* getXxx(参数):获取数据
				* Xxx：代表数据类型   如： int getInt() ,	String getString()
				* 参数：
					1. int：代表列的编号,从1开始   如： getString(1)
					2. String：代表列名称。 如： getDouble("balance")
			
			* 注意：
				* 使用步骤：
					1. 游标向下移动一行
					2. 判断是否有数据
					3. 获取数据
	
				   //循环判断游标是否是最后一行末尾。
		            while(rs.next()){
		                //获取数据
		                //6.2 获取数据
		                int id = rs.getInt(1);
		                String name = rs.getString("name");
		                double balance = rs.getDouble(3);
		
		                System.out.println(id + "---" + name + "---" + balance);
		            }
	
			* 练习：
				* 定义一个方法，查询emp表的数据将其封装为对象，然后装载集合，返回。
					1. 定义Emp类
					2. 定义方法 public List<Emp> findAll(){}
					3. 实现方法 select * from emp;
						
		5. PreparedStatement：执行sql的对象
			1. SQL注入问题：在拼接sql时，有一些sql的特殊关键字参与字符串的拼接。会造成安全性问题
				1. 输入用户随便，输入密码：a' or 'a' = 'a
				2. sql：select * from user where username = 'fhdsjkf' and password = 'a' or 'a' = 'a' 
	
			2. 解决sql注入问题：使用PreparedStatement对象来解决
			3. 预编译的SQL：参数使用?作为占位符
			4. 步骤：
				1. 导入驱动jar包 mysql-connector-java-5.1.37-bin.jar
				2. 注册驱动
				3. 获取数据库连接对象 Connection
				4. 定义sql
					* 注意：sql的参数使用？作为占位符。 如：select * from user where username = ? and password = ?;
				5. 获取执行sql语句的对象 PreparedStatement  Connection.prepareStatement(String sql) 
				6. 给？赋值：
					* 方法： setXxx(参数1,参数2)
						* 参数1：？的位置编号 从1 开始
						* 参数2：？的值
				7. 执行sql，接受返回结果，不需要传递sql语句
				8. 处理结果
				9. 释放资源
	
			5. 注意：后期都会使用PreparedStatement来完成增删改查的所有操作
				1. 可以防止SQL注入
				2. 效率更高

## 抽取JDBC工具类 ： JDBCUtils

	* 目的：简化书写
	* 分析：
		1. 注册驱动也抽取
		2. 抽取一个方法获取连接对象
			* 需求：不想传递参数（麻烦），还得保证工具类的通用性。
			* 解决：配置文件
				jdbc.properties
					url=
					user=
					password=


		3. 抽取一个方法释放资源
	
	* 代码实现：
		public class JDBCUtils {
	    private static String url;
	    private static String user;
	    private static String password;
	    private static String driver;
	    /**
	     * 文件的读取，只需要读取一次即可拿到这些值。使用静态代码块
	     */
	    static{
	        //读取资源文件，获取值。
	
	        try {
	            //1. 创建Properties集合类。
	            Properties pro = new Properties();
	
	            //获取src路径下的文件的方式--->ClassLoader 类加载器
	            ClassLoader classLoader = JDBCUtils.class.getClassLoader();
	            URL res  = classLoader.getResource("jdbc.properties");
	            String path = res.getPath();
	            System.out.println(path);///D:/IdeaProjects/itcast/out/production/day04_jdbc/jdbc.properties
	            //2. 加载文件
	           // pro.load(new FileReader("D:\\IdeaProjects\\itcast\\day04_jdbc\\src\\jdbc.properties"));
	            pro.load(new FileReader(path));
	
	            //3. 获取数据，赋值
	            url = pro.getProperty("url");
	            user = pro.getProperty("user");
	            password = pro.getProperty("password");
	            driver = pro.getProperty("driver");
	            //4. 注册驱动
	            Class.forName(driver);
	        } catch (IOException e) {
	            e.printStackTrace();
	        } catch (ClassNotFoundException e) {
	            e.printStackTrace();
	        }
	    }


​	
​	    /**
​	     * 获取连接
​	     * @return 连接对象
​	     */
​	    public static Connection getConnection() throws SQLException {
​	

	        return DriverManager.getConnection(url, user, password);
	    }
	
	    /**
	     * 释放资源
	     * @param stmt
	     * @param conn
	     */
	    public static void close(Statement stmt,Connection conn){
	        if( stmt != null){
	            try {
	                stmt.close();
	            } catch (SQLException e) {
	                e.printStackTrace();
	            }
	        }
	
	        if( conn != null){
	            try {
	                conn.close();
	            } catch (SQLException e) {
	                e.printStackTrace();
	            }
	        }
	    }


​	
​	    /**
​	     * 释放资源
​	     * @param stmt
​	     * @param conn
​	     */
​	    public static void close(ResultSet rs,Statement stmt, Connection conn){
​	        if( rs != null){
​	            try {
​	                rs.close();
​	            } catch (SQLException e) {
​	                e.printStackTrace();
​	            }
​	        }
​	

	        if( stmt != null){
	            try {
	                stmt.close();
	            } catch (SQLException e) {
	                e.printStackTrace();
	            }
	        }
	
	        if( conn != null){
	            try {
	                conn.close();
	            } catch (SQLException e) {
	                e.printStackTrace();
	            }
	        }
	    }
	
	}
	
	* 练习：
		* 需求：
			1. 通过键盘录入用户名和密码
			2. 判断用户是否登录成功
				* select * from user where username = "" and password = "";
				* 如果这个sql有查询结果，则成功，反之，则失败
	
		* 步骤：
			1. 创建数据库表 user
				CREATE TABLE USER(
					id INT PRIMARY KEY AUTO_INCREMENT,
					username VARCHAR(32),
					PASSWORD VARCHAR(32)
				
				);
	
				INSERT INTO USER VALUES(NULL,'zhangsan','123');
				INSERT INTO USER VALUES(NULL,'lisi','234');
	
			2. 代码实现：
				public class JDBCDemo9 {
	
				    public static void main(String[] args) {
				        //1.键盘录入，接受用户名和密码
				        Scanner sc = new Scanner(System.in);
				        System.out.println("请输入用户名：");
				        String username = sc.nextLine();
				        System.out.println("请输入密码：");
				        String password = sc.nextLine();
				        //2.调用方法
				        boolean flag = new JDBCDemo9().login(username, password);
				        //3.判断结果，输出不同语句
				        if(flag){
				            //登录成功
				            System.out.println("登录成功！");
				        }else{
				            System.out.println("用户名或密码错误！");
				        }


​				
​				    }


​				
​				
​				    /**
​				     * 登录方法
​				     */
​				    public boolean login(String username ,String password){
​				        if(username == null || password == null){
​				            return false;
​				        }
​				        //连接数据库判断是否登录成功
​				        Connection conn = null;
​				        Statement stmt =  null;
​				        ResultSet rs = null;
​				        //1.获取连接
​				        try {
​				            conn =  JDBCUtils.getConnection();
​				            //2.定义sql
​				            String sql = "select * from user where username = '"+username+"' and password = '"+password+"' ";
​				            //3.获取执行sql的对象
​				            stmt = conn.createStatement();
​				            //4.执行查询
​				            rs = stmt.executeQuery(sql);
​				            //5.判断
​				           /* if(rs.next()){//如果有下一行，则返回true
​				                return true;
​				            }else{
​				                return false;
​				            }*/
​				           return rs.next();//如果有下一行，则返回true
​				

				        } catch (SQLException e) {
				            e.printStackTrace();
				        }finally {
				            JDBCUtils.close(rs,stmt,conn);
				        }


​				
​				        return false;
​				    }
​				}


## JDBC控制事务：

	1. 事务：一个包含多个步骤的业务操作。如果这个业务操作被事务管理，则这多个步骤要么同时成功，要么同时失败。
	2. 操作：
		1. 开启事务
		2. 提交事务
		3. 回滚事务
	3. 使用Connection对象来管理事务
		* 开启事务：setAutoCommit(boolean autoCommit) ：调用该方法设置参数为false，即开启事务
			* 在执行sql之前开启事务
		* 提交事务：commit() 
			* 当所有sql都执行完提交事务
		* 回滚事务：rollback() 
			* 在catch中回滚事务
	
	4. 代码：
		public class JDBCDemo10 {
	
		    public static void main(String[] args) {
		        Connection conn = null;
		        PreparedStatement pstmt1 = null;
		        PreparedStatement pstmt2 = null;
		
		        try {
		            //1.获取连接
		            conn = JDBCUtils.getConnection();
		            //开启事务
		            conn.setAutoCommit(false);
		
		            //2.定义sql
		            //2.1 张三 - 500
		            String sql1 = "update account set balance = balance - ? where id = ?";
		            //2.2 李四 + 500
		            String sql2 = "update account set balance = balance + ? where id = ?";
		            //3.获取执行sql对象
		            pstmt1 = conn.prepareStatement(sql1);
		            pstmt2 = conn.prepareStatement(sql2);
		            //4. 设置参数
		            pstmt1.setDouble(1,500);
		            pstmt1.setInt(2,1);
		
		            pstmt2.setDouble(1,500);
		            pstmt2.setInt(2,2);
		            //5.执行sql
		            pstmt1.executeUpdate();
		            // 手动制造异常
		            int i = 3/0;
		
		            pstmt2.executeUpdate();
		            //提交事务
		            conn.commit();
		        } catch (Exception e) {
		            //事务回滚
		            try {
		                if(conn != null) {
		                    conn.rollback();
		                }
		            } catch (SQLException e1) {
		                e1.printStackTrace();
		            }
		            e.printStackTrace();
		        }finally {
		            JDBCUtils.close(pstmt1,conn);
		            JDBCUtils.close(pstmt2,null);
		        }
		   }
	}



## Servlet：

	1. 概念
	2. 步骤
	3. 执行原理
	4. 生命周期
	5. Servlet3.0 注解配置
	6. Servlet的体系结构	
		Servlet -- 接口
			|
		GenericServlet -- 抽象类
			|
		HttpServlet  -- 抽象类
	
		* GenericServlet：将Servlet接口中其他的方法做了默认空实现，只将service()方法作为抽象
			* 将来定义Servlet类时，可以继承GenericServlet，实现service()方法即可
	
		* HttpServlet：对http协议的一种封装，简化操作
			1. 定义类继承HttpServlet
			2. 复写doGet/doPost方法
	
	7. Servlet相关配置
		1. urlpartten:Servlet访问路径
			1. 一个Servlet可以定义多个访问路径 ： @WebServlet({"/d4","/dd4","/ddd4"})
			2. 路径定义规则：
				1. /xxx：路径匹配
				2. /xxx/xxx:多层路径，目录结构
				3. *.do：扩展名匹配



Web.xml 配置

```
    <servlet>
        <servlet-name>demo01</servlet-name>
        <servlet-class>com.qiuyeyijian.web.servlet.ServletDemo01</servlet-class>
    </servlet>

    <servlet-mapping>
        <servlet-name>demo01</servlet-name>
        <url-pattern>/demo01</url-pattern>
    </servlet-mapping>
```



## HTTP：

	* 概念：Hyper Text Transfer Protocol 超文本传输协议
		* 传输协议：定义了，客户端和服务器端通信时，发送数据的格式
		* 特点：
			1. 基于TCP/IP的高级协议
			2. 默认端口号:80
			3. 基于请求/响应模型的:一次请求对应一次响应
			4. 无状态的：每次请求之间相互独立，不能交互数据
	
		* 历史版本：
			* 1.0：每一次请求响应都会建立新的连接
			* 1.1：复用连接
	
	* 请求消息数据格式
		1. 请求行
			请求方式 请求url 请求协议/版本
			GET /login.html	HTTP/1.1
	
			* 请求方式：
				* HTTP协议有7中请求方式，常用的有2种
					* GET：
						1. 请求参数在请求行中，在url后。
						2. 请求的url长度有限制的
						3. 不太安全
					* POST：
						1. 请求参数在请求体中
						2. 请求的url长度没有限制的
						3. 相对安全
		2. 请求头：客户端浏览器告诉服务器一些信息
			请求头名称: 请求头值
			* 常见的请求头：
				1. User-Agent：浏览器告诉服务器，我访问你使用的浏览器版本信息
					* 可以在服务器端获取该头的信息，解决浏览器的兼容性问题
	
				2. Referer：http://localhost/login.html
					* 告诉服务器，我(当前请求)从哪里来？
						* 作用：
							1. 防盗链：
							2. 统计工作：
		3. 请求空行
			空行，就是用于分割POST请求的请求头，和请求体的。
		4. 请求体(正文)：
			* 封装POST请求消息的请求参数的
	
		* 字符串格式：
			POST /login.html	HTTP/1.1
			Host: localhost
			User-Agent: Mozilla/5.0 (Windows NT 6.1; Win64; x64; rv:60.0) Gecko/20100101 Firefox/60.0
			Accept: text/html,application/xhtml+xml,application/xml;q=0.9,*/*;q=0.8
			Accept-Language: zh-CN,zh;q=0.8,zh-TW;q=0.7,zh-HK;q=0.5,en-US;q=0.3,en;q=0.2
			Accept-Encoding: gzip, deflate
			Referer: http://localhost/login.html
			Connection: keep-alive
			Upgrade-Insecure-Requests: 1
			
			username=zhangsan	


	* 响应消息数据格式




## Request：

	1. request对象和response对象的原理
		1. request和response对象是由服务器创建的。我们来使用它们
		2. request对象是来获取请求消息，response对象是来设置响应消息
	
	2. request对象继承体系结构：	
		ServletRequest		--	接口
			|	继承
		HttpServletRequest	-- 接口
			|	实现
		org.apache.catalina.connector.RequestFacade 类(tomcat)
	
	3. request功能：
		1. 获取请求消息数据
			1. 获取请求行数据
				* GET /day14/demo1?name=zhangsan HTTP/1.1
				* 方法：
					1. 获取请求方式 ：GET
						* String getMethod()  
					2. (*)获取虚拟目录：/day14
						* String getContextPath()
					3. 获取Servlet路径: /demo1
						* String getServletPath()
					4. 获取get方式请求参数：name=zhangsan
						* String getQueryString()
					5. (*)获取请求URI：/day14/demo1
						* String getRequestURI():		/day14/demo1
						* StringBuffer getRequestURL()  :http://localhost/day14/demo1
	
						* URL:统一资源定位符 ： http://localhost/day14/demo1	中华人民共和国
						* URI：统一资源标识符 : /day14/demo1					共和国
					
					6. 获取协议及版本：HTTP/1.1
						* String getProtocol()
	
					7. 获取客户机的IP地址：
						* String getRemoteAddr()
					
			2. 获取请求头数据
				* 方法：
					* (*)String getHeader(String name):通过请求头的名称获取请求头的值
					* Enumeration<String> getHeaderNames():获取所有的请求头名称
				
			3. 获取请求体数据:
				* 请求体：只有POST请求方式，才有请求体，在请求体中封装了POST请求的请求参数
				* 步骤：
					1. 获取流对象
						*  BufferedReader getReader()：获取字符输入流，只能操作字符数据
						*  ServletInputStream getInputStream()：获取字节输入流，可以操作所有类型数据
							* 在文件上传知识点后讲解
	
					2. 再从流对象中拿数据


​				

		2. 其他功能：
			1. 获取请求参数通用方式：不论get还是post请求方式都可以使用下列方法来获取请求参数
				1. String getParameter(String name):根据参数名称获取参数值    username=zs&password=123
				2. String[] getParameterValues(String name):根据参数名称获取参数值的数组  hobby=xx&hobby=game
				3. Enumeration<String> getParameterNames():获取所有请求的参数名称
				4. Map<String,String[]> getParameterMap():获取所有参数的map集合
	
				* 中文乱码问题：
					* get方式：tomcat 8 已经将get方式乱码问题解决了
					* post方式：会乱码
						* 解决：在获取参数前，设置request的编码request.setCharacterEncoding("utf-8");


​					

			2. 请求转发：一种在服务器内部的资源跳转方式
				1. 步骤：
					1. 通过request对象获取请求转发器对象：RequestDispatcher getRequestDispatcher(String path)
					2. 使用RequestDispatcher对象来进行转发：forward(ServletRequest request, ServletResponse response) 
	
				2. 特点：
					1. 浏览器地址栏路径不发生变化
					2. 只能转发到当前服务器内部资源中。
					3. 转发是一次请求


			3. 共享数据：
				* 域对象：一个有作用范围的对象，可以在范围内共享数据
				* request域：代表一次请求的范围，一般用于请求转发的多个资源中共享数据
				* 方法：
					1. void setAttribute(String name,Object obj):存储数据
					2. Object getAttitude(String name):通过键获取值
					3. void removeAttribute(String name):通过键移除键值对
	
			4. 获取ServletContext：
				* ServletContext getServletContext()




## 案例：用户登录

	* 用户登录案例需求：
		1.编写login.html登录页面
			username & password 两个输入框
		2.使用Druid数据库连接池技术,操作mysql，day14数据库中user表
		3.使用JdbcTemplate技术封装JDBC
		4.登录成功跳转到SuccessServlet展示：登录成功！用户名,欢迎您
		5.登录失败跳转到FailServlet展示：登录失败，用户名或密码错误


	* 分析
	
	* 开发步骤
		1. 创建项目，导入html页面，配置文件，jar包
		2. 创建数据库环境
			CREATE DATABASE day14;
			USE day14;
			CREATE TABLE USER(
			
				id INT PRIMARY KEY AUTO_INCREMENT,
				username VARCHAR(32) UNIQUE NOT NULL,
				PASSWORD VARCHAR(32) NOT NULL
			);
	
		3. 创建包cn.itcast.domain,创建类User
			package cn.itcast.domain;
			/**
			 * 用户的实体类
			 */
			public class User {
			
			    private int id;
			    private String username;
			    private String password;


​			

			    public int getId() {
			        return id;
			    }
			
			    public void setId(int id) {
			        this.id = id;
			    }
			
			    public String getUsername() {
			        return username;
			    }
			
			    public void setUsername(String username) {
			        this.username = username;
			    }
			
			    public String getPassword() {
			        return password;
			    }
			
			    public void setPassword(String password) {
			        this.password = password;
			    }
			
			    @Override
			    public String toString() {
			        return "User{" +
			                "id=" + id +
			                ", username='" + username + '\'' +
			                ", password='" + password + '\'' +
			                '}';
			    }
			}
		4. 创建包cn.itcast.util,编写工具类JDBCUtils
			package cn.itcast.util;
	
			import com.alibaba.druid.pool.DruidDataSourceFactory;
			
			import javax.sql.DataSource;
			import javax.xml.crypto.Data;
			import java.io.IOException;
			import java.io.InputStream;
			import java.sql.Connection;
			import java.sql.SQLException;
			import java.util.Properties;
			
			/**
			 * JDBC工具类 使用Durid连接池
			 */
			public class JDBCUtils {
			
			    private static DataSource ds ;
			
			    static {
			
			        try {
			            //1.加载配置文件
			            Properties pro = new Properties();
			            //使用ClassLoader加载配置文件，获取字节输入流
			            InputStream is = JDBCUtils.class.getClassLoader().getResourceAsStream("druid.properties");
			            pro.load(is);
			
			            //2.初始化连接池对象
			            ds = DruidDataSourceFactory.createDataSource(pro);
			
			        } catch (IOException e) {
			            e.printStackTrace();
			        } catch (Exception e) {
			            e.printStackTrace();
			        }
			    }
			
			    /**
			     * 获取连接池对象
			     */
			    public static DataSource getDataSource(){
			        return ds;
			    }


​			

			    /**
			     * 获取连接Connection对象
			     */
			    public static Connection getConnection() throws SQLException {
			        return  ds.getConnection();
			    }
			}
		5. 创建包cn.itcast.dao,创建类UserDao,提供login方法
			
			package cn.itcast.dao;
	
			import cn.itcast.domain.User;
			import cn.itcast.util.JDBCUtils;
			import org.springframework.dao.DataAccessException;
			import org.springframework.jdbc.core.BeanPropertyRowMapper;
			import org.springframework.jdbc.core.JdbcTemplate;
			
			/**
			 * 操作数据库中User表的类
			 */
			public class UserDao {
			
			    //声明JDBCTemplate对象共用
			    private JdbcTemplate template = new JdbcTemplate(JDBCUtils.getDataSource());
			
			    /**
			     * 登录方法
			     * @param loginUser 只有用户名和密码
			     * @return user包含用户全部数据,没有查询到，返回null
			     */
			    public User login(User loginUser){
			        try {
			            //1.编写sql
			            String sql = "select * from user where username = ? and password = ?";
			            //2.调用query方法
			            User user = template.queryForObject(sql,
			                    new BeanPropertyRowMapper<User>(User.class),
			                    loginUser.getUsername(), loginUser.getPassword());


​			

			            return user;
			        } catch (DataAccessException e) {
			            e.printStackTrace();//记录日志
			            return null;
			        }
			    }
			}
		
		6. 编写cn.itcast.web.servlet.LoginServlet类
			package cn.itcast.web.servlet;
	
			import cn.itcast.dao.UserDao;
			import cn.itcast.domain.User;
			
			import javax.servlet.ServletException;
			import javax.servlet.annotation.WebServlet;
			import javax.servlet.http.HttpServlet;
			import javax.servlet.http.HttpServletRequest;
			import javax.servlet.http.HttpServletResponse;
			import java.io.IOException;


​			

			@WebServlet("/loginServlet")
			public class LoginServlet extends HttpServlet {


​			

			    @Override
			    protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
			        //1.设置编码
			        req.setCharacterEncoding("utf-8");
			        //2.获取请求参数
			        String username = req.getParameter("username");
			        String password = req.getParameter("password");
			        //3.封装user对象
			        User loginUser = new User();
			        loginUser.setUsername(username);
			        loginUser.setPassword(password);
			
			        //4.调用UserDao的login方法
			        UserDao dao = new UserDao();
			        User user = dao.login(loginUser);
			
			        //5.判断user
			        if(user == null){
			            //登录失败
			            req.getRequestDispatcher("/failServlet").forward(req,resp);
			        }else{
			            //登录成功
			            //存储数据
			            req.setAttribute("user",user);
			            //转发
			            req.getRequestDispatcher("/successServlet").forward(req,resp);
			        }
			
			    }
			
			    @Override
			    protected void doPost(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
			        this.doGet(req,resp);
			    }
			}
	
		7. 编写FailServlet和SuccessServlet类
			@WebServlet("/successServlet")
			public class SuccessServlet extends HttpServlet {
			    protected void doPost(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
			        //获取request域中共享的user对象
			        User user = (User) request.getAttribute("user");
			
			        if(user != null){
			            //给页面写一句话
			
			            //设置编码
			            response.setContentType("text/html;charset=utf-8");
			            //输出
			            response.getWriter().write("登录成功！"+user.getUsername()+",欢迎您");
			        }


​			

			    }		


			@WebServlet("/failServlet")
			public class FailServlet extends HttpServlet {
			    protected void doPost(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
			        //给页面写一句话
			
			        //设置编码
			        response.setContentType("text/html;charset=utf-8");
			        //输出
			        response.getWriter().write("登录失败，用户名或密码错误");
			
			    }
			
			    protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
			        this.doPost(request,response);
			    }
			}



		8. login.html中form表单的action路径的写法
			* 虚拟目录+Servlet的资源路径
	
		9. BeanUtils工具类，简化数据封装
			* 用于封装JavaBean的
			1. JavaBean：标准的Java类
				1. 要求：
					1. 类必须被public修饰
					2. 必须提供空参的构造器
					3. 成员变量必须使用private修饰
					4. 提供公共setter和getter方法
				2. 功能：封装数据


			2. 概念：
				成员变量：
				属性：setter和getter方法截取后的产物
					例如：getUsername() --> Username--> username


			3. 方法：
				1. setProperty()
				2. getProperty()
				3. populate(Object obj , Map map):将map集合的键值对信息，封装到对应的JavaBean对象中



# 今日内容

	1. HTTP协议：响应消息
	2. Response对象
	3. ServletContext对象

## HTTP协议：

	1. 请求消息：客户端发送给服务器端的数据
		* 数据格式：
			1. 请求行
			2. 请求头
			3. 请求空行
			4. 请求体
	2. 响应消息：服务器端发送给客户端的数据
		* 数据格式：
			1. 响应行
				1. 组成：协议/版本 响应状态码 状态码描述
				2. 响应状态码：服务器告诉客户端浏览器本次请求和响应的一个状态。
					1. 状态码都是3位数字 
					2. 分类：
						1. 1xx：服务器就收客户端消息，但没有接受完成，等待一段时间后，发送1xx多状态码
						2. 2xx：成功。代表：200
						3. 3xx：重定向。代表：302(重定向)，304(访问缓存)
						4. 4xx：客户端错误。
							* 代表：
								* 404（请求路径没有对应的资源） 
								* 405：请求方式没有对应的doXxx方法
						5. 5xx：服务器端错误。代表：500(服务器内部出现异常)


​					

			2. 响应头：
				1. 格式：头名称： 值
				2. 常见的响应头：
					1. Content-Type：服务器告诉客户端本次响应体数据格式以及编码格式
					2. Content-disposition：服务器告诉客户端以什么格式打开响应体数据
						* 值：
							* in-line:默认值,在当前页面内打开
							* attachment;filename=xxx：以附件形式打开响应体。文件下载
			3. 响应空行
			4. 响应体:传输的数据


		* 响应字符串格式
			HTTP/1.1 200 OK
			Content-Type: text/html;charset=UTF-8
			Content-Length: 101
			Date: Wed, 06 Jun 2018 07:08:42 GMT
	
			<html>
			  <head>
			    <title>$Title$</title>
			  </head>
			  <body>
			  hello , response
			  </body>
			</html>



## Response对象

	* 功能：设置响应消息
		1. 设置响应行
			1. 格式：HTTP/1.1 200 ok
			2. 设置状态码：setStatus(int sc) 
		2. 设置响应头：setHeader(String name, String value) 
			
		3. 设置响应体：
			* 使用步骤：
				1. 获取输出流
					* 字符输出流：PrintWriter getWriter()
	
					* 字节输出流：ServletOutputStream getOutputStream()
	
				2. 使用输出流，将数据输出到客户端浏览器


	* 案例：
		1. 完成重定向
			* 重定向：资源跳转的方式
			* 代码实现：
				//1. 设置状态码为302
		        response.setStatus(302);
		        //2.设置响应头location
		        response.setHeader("location","/day15/responseDemo2");


		        //简单的重定向方法
		        response.sendRedirect("/day15/responseDemo2");
	
			* 重定向的特点:redirect
				1. 地址栏发生变化
				2. 重定向可以访问其他站点(服务器)的资源
				3. 重定向是两次请求。不能使用request对象来共享数据
			* 转发的特点：forward
				1. 转发地址栏路径不变
				2. 转发只能访问当前服务器下的资源
				3. 转发是一次请求，可以使用request对象来共享数据
			
			* forward 和  redirect 区别
				
			* 路径写法：
				1. 路径分类
					1. 相对路径：通过相对路径不可以确定唯一资源
						* 如：./index.html
						* 不以/开头，以.开头路径
	
						* 规则：找到当前资源和目标资源之间的相对位置关系
							* ./：当前目录
							* ../:后退一级目录
					2. 绝对路径：通过绝对路径可以确定唯一资源
						* 如：http://localhost/day15/responseDemo2		/day15/responseDemo2
						* 以/开头的路径
	
						* 规则：判断定义的路径是给谁用的？判断请求将来从哪儿发出
							* 给客户端浏览器使用：需要加虚拟目录(项目的访问路径)
								* 建议虚拟目录动态获取：request.getContextPath()
								* <a> , <form> 重定向...
							* 给服务器使用：不需要加虚拟目录
								* 转发路径


​						
​						

		2. 服务器输出字符数据到浏览器
			* 步骤：
				1. 获取字符输出流
				2. 输出数据
	
			* 注意：
				* 乱码问题：
					1. PrintWriter pw = response.getWriter();获取的流的默认编码是ISO-8859-1
					2. 设置该流的默认编码
					3. 告诉浏览器响应体使用的编码
	
					//简单的形式，设置编码，是在获取流之前设置
	    			response.setContentType("text/html;charset=utf-8");
		3. 服务器输出字节数据到浏览器
			* 步骤：
				1. 获取字节输出流
				2. 输出数据
	
		4. 验证码
			1. 本质：图片
			2. 目的：防止恶意表单注册



## ServletContext对象：

	1. 概念：代表整个web应用，可以和程序的容器(服务器)来通信
	2. 获取：
		1. 通过request对象获取
			request.getServletContext();
		2. 通过HttpServlet获取
			this.getServletContext();
	3. 功能：
		1. 获取MIME类型：
			* MIME类型:在互联网通信过程中定义的一种文件数据类型
				* 格式： 大类型/小类型   text/html		image/jpeg
	
			* 获取：String getMimeType(String file)  
		2. 域对象：共享数据
			1. setAttribute(String name,Object value)
			2. getAttribute(String name)
			3. removeAttribute(String name)
	
			* ServletContext对象范围：所有用户所有请求的数据
		3. 获取文件的真实(服务器)路径
			1. 方法：String getRealPath(String path)  
				 String b = context.getRealPath("/b.txt");//web目录下资源访问
		         System.out.println(b);
		
		        String c = context.getRealPath("/WEB-INF/c.txt");//WEB-INF目录下的资源访问
		        System.out.println(c);
		
		        String a = context.getRealPath("/WEB-INF/classes/a.txt");//src目录下的资源访问
		        System.out.println(a);



## 案例：

	* 文件下载需求：
		1. 页面显示超链接
		2. 点击超链接后弹出下载提示框
		3. 完成图片文件下载


	* 分析：
		1. 超链接指向的资源如果能够被浏览器解析，则在浏览器中展示，如果不能解析，则弹出下载提示框。不满足需求
		2. 任何资源都必须弹出下载提示框
		3. 使用响应头设置资源的打开方式：
			* content-disposition:attachment;filename=xxx


	* 步骤：
		1. 定义页面，编辑超链接href属性，指向Servlet，传递资源名称filename
		2. 定义Servlet
			1. 获取文件名称
			2. 使用字节输入流加载文件进内存
			3. 指定response的响应头： content-disposition:attachment;filename=xxx
			4. 将数据写出到response输出流


	* 问题：
		* 中文文件问题
			* 解决思路：
				1. 获取客户端使用的浏览器版本信息
				2. 根据不同的版本信息，设置filename的编码方式不同