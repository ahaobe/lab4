# 实验目的
-  Spring和AOP编程
# 实验任务
- 构建一个安全验证的切面。要求：
a. 对于所有的Alumni表的查询操作，验证用户已经登录；如果用户没有登录，先导航到登录页面；
b. 对于所有的Alumni表的更新（更新和删除）操作，在Read权限的基础上验证用户具有Update的权限。如果没有，该操作取消，并导航到错误页面。
c. 对于所有的Alumni表的汇总和下载操作，验证用户具有Aggregate权限；如果没有，该操作取消，并导航到错误页面。
# 环境设置、工具
- java jdk （1.6或者以上）
- IDEA 或者 eclipse 等开发环境
- MySQL 数据库
# 具体过程
### 1. 创建数据库表

![数据库表](https://i.loli.net/2019/06/21/5d0c87a39e99421985.png
)

![admin表](https://i.loli.net/2019/06/21/5d0c87a2e5ab466972.png)

![resource表](https://i.loli.net/2019/06/21/5d0c87a2e77da69453.png
)

![right表](https://i.loli.net/2019/06/21/5d0c87a3081e596086.png
)

可以看到，管理员对应的rightId可以对应多个resourceId（读、写、下载）

### 2. 创建对应实体类

Admin实体类举例

![Entity举例](https://i.loli.net/2019/06/21/5d0c8958ac61425671.png
)

### 3. 创建实体类对应repository

因为逻辑相对简单，所以选用JPA ORM。

AdminRepository举例

![Repository举例](https://i.loli.net/2019/06/21/5d0c88f6b301254686.png)

### 4. 创建注解

![Find注解举例](https://i.loli.net/2019/06/21/5d0c8ac19a93432982.png
)

同理，创建@Update、@Aggregate注解

### 5. 创建切面

![Update切面](https://i.loli.net/2019/06/21/5d0c8b820178961827.png
)

若已登录则不做操作，若无则跳转到login页

![Find切面](https://i.loli.net/2019/06/21/5d0c8b825f26678768.png
)

若有更新权限（resourceId=2）则不做操作，若无则跳转到error页

![Aggregate切面](https://i.loli.net/2019/06/21/5d0c8be6c94fd62296.png
)

若有下载权限（resourceId=3）则不做操作，若无则跳转到error页

### 6. 创建controller

![LoginController](https://i.loli.net/2019/06/21/5d0c89e200bd380649.png
)
![AlumniController](https://i.loli.net/2019/06/21/5d0c8a4827e5529051.png
)
![AlumniController](https://i.loli.net/2019/06/21/5d0c8c9be82e222234.png)

在查询方法前加@Find注解；

在更新方法前加@Update注解；

在下载方法前加@Aggregate注解。
