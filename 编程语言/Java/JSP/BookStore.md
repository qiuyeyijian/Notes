## BookStore



### 知识要点

网上书店项目是前几个实验的综合。前端登录利用了之前的登录页面设计实验和JS前端验证实验。数据库方便利用了之前JSP访问数据库实验。书籍的管理功能，包括书籍的增删改查，利用之前的JavaBean实验，和Servlet应用实验。总的来说，这是一个综合实例，只有理解并掌握了前面的实验要点，才能完成这个实验。

### 实验任务

完成书店用户的登录功能，前端登录页面应该加上验证码，防止机器人暴力穷举用户账号和密码。

完成图书添加功能，一本图书必须包含书名，作者，出版社，封面链接，价格，ISBN编号

完成图书的删除和修改功能，用户可以修改除了id以外的的信息。

完成上传功能，用户可以上传图书的封面，上传的文件可以保存到服务器相应的文件夹内。

用户登录成功后应该显示用户的登录名

### 实验步骤

1. 准备好数据库，设计好相关表文件
2. 准备好第三发jar包，比如Mysql数据库驱动，文件上传
3. 编写AddBook.java、Books.java、DeleteBook.java、Login.java、Upload.java等程序文件文件

AddBook.java是负责添加图书的。该程序首先会设置编码格式，然后接受前端传来的图书参数，接着创建数据库连接，将图书信息保存到数据库中。Books.java负责前端的图书展示。改程序会首先从数据库中拿到所有图书信息，然后以JSON数组的形式返回到前端，最后再由前端页面展示图书信息。DeleteBook.java负责删除图书，如果用户点击前端相应按钮，就会触发删除图书请求。该请求会被改程序处理，从数据库中将相应的图书删除。

