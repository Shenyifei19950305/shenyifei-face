Shiro 

JWT可以做身份验证，授权 ，密码 和会话管理 这边前面三个和springsecurity一样都是认证、授权 最后一个会话管理其实就是seesion的存储

可提供单点登录 sso 还有提供Remember Me 服务

与springSecurity对比

差不多吧 ss是重量级 shiro是轻量级

- Authentication：身份认证/登录，验证用户是不是拥有相应的身份。
- Authorization：授权，即权限验证，验证某个已认证的用户是否拥有某个权限；即判断用户是否能做事情。
- Session Management：会话管理，即用户登录后就是一次会话，在没有退出之前，它的所有信息都在会话
- 中；会话可以是普通JavaSE环境的，也可以是如Web环境的。
- Cryptography：加密，保护数据的安全性，如密码加密存储到数据库，而不是明文存储。
- Web Support：Shiro 的 web 支持的 API 能够轻松地帮助保护 Web 应用程序。
- Caching：缓存，比如用户登录后，其用户信息、拥有的角色/权限不必每次去查，这样可以提高效率。
- Concurrency：Apache Shiro 利用它的并发特性来支持多线程应用程序。
- Testing：测试支持的存在来帮助你编写单元测试和集成测试，并确保你的能够如预期的一样安全。
- "Run As"：一个允许用户假设为另一个用户身份（如果允许）的功能，有时候在管理脚本很有用。北京市昌平区建材城西路金燕龙办公楼一层 电话：400-618-9090
- "Remember Me"：记住我

![image-20200530171046234](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20200530171046234.png)



**Subject**：主体，可以看到主体可以是任何可以与应用交互的“用户”；

**SecurityManager**：相当于SpringMVC中的DispatcherServlet或者Struts2中的FilterDispatcher；是Shiro的心

脏；所有具体的交互都通过SecurityManager进行控制；它管理着所有Subject、且负责进行认证和授权、及会

话、缓存的管理。

**Authenticator**：认证器，负责主体认证的，这是一个扩展点，如果用户觉得Shiro默认的不好，可以自定义实

现；其需要认证策略（Authentication Strategy），即什么情况下算用户认证通过了；

**Authrizer**：授权器，或者访问控制器，用来决定主体是否有权限进行相应的操作；即控制着用户能访问应用中的

哪些功能；

**Realm**：可以有1个或多个Realm，可以认为是安全实体数据源，即用于获取安全实体的；可以是JDBC实现，也可

以是LDAP实现，或者内存实现等等；由用户提供；注意：Shiro不知道你的用户/权限存储在哪及以何种格式存储；

所以我们一般在应用中都需要实现自己的Realm；

**SessionManager**：如果写过Servlet就应该知道Session的概念，Session呢需要有人去管理它的生命周期，这个

组件就是SessionManager；而Shiro并不仅仅可以用在Web环境，也可以用在如普通的JavaSE环境、EJB等环境；

所有呢，Shiro就抽象了一个自己的Session来管理主体与应用之间交互的数据；

**SessionDAO**：DAO大家都用过，数据访问对象，用于会话的CRUD，比如我们想把Session保存到数据库，那么可

以实现自己的SessionDAO，通过如JDBC写到数据库；比如想把Session放到Memcached中，可以实现自己的

Memcached SessionDAO；另外SessionDAO中可以使用Cache进行缓存，以提高性能；

**CacheManager**：缓存控制器，来管理如用户、角色、权限等的缓存的；因为这些数据基本上很少去改变，放到

缓存中后可以提高访问的性能

**Cryptography**：密码模块，Shiro提高了一些常见的加密组件用于如密码加密/解密的。 



认证器可对登录进来的进行认证 验证交给realm 授权器进行授权

会话管理需要通过sessionManager SessionDao 最后才给redis储存

![image-20200530171540811](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20200530171540811.png)

![image-20200530175912843](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20200530175912843.png)

![image-20200530175856517](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20200530175856517.png)



这样就可以完成用户授权了

测试的主要步骤 其实就是写在controller层 主要是获取subject （通过securityfactory获得 最后通过和securitymanager进行绑定)

然后生成token 用subject绑定token进行登录

这边的ini文件都是模拟的 realm可以进行数据认证和授权



realm先起名，然后重写两个方法 一个是授权一个是认证

一般先写下面的认证

![image-20200530180944525](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20200530180944525.png)

再写授权，数据是从token中拿到的 获取token的用户名和密码

进行和数据库验证 业务流程 

![image-20200530181547456](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20200530181547456.png)