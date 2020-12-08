这边说JWT首先要先说一个有状态服务和无状态服务（对应saas第4章 第5章)

无状态服务不会记录服务状态 比如JWT 的token 还有cookie

有状态服务会进行服务状态的一个记录如seesion 还有我们后面会提到的 比如Shiro+redis组成的一个会话管理

先说下token token就是客户登录后 生成一个token返回给前端 前端下次可利用token通过http请求头进行校验和验证

有状态服务是通过登录产生seesionid  步骤和token差不多 唯一区别就是seesionid会储存到服务器中 这边可以用redis来做数据库



先说第一个无状态服务 JWT

@test

token的创建需要先引入pom 然后创建test 生成一个token 很简单

用一个构造模式 Jwts.builder().setid()方法 。。。。。

接下来就是token的解析  

先获取token生成的秘钥 然后进行Jwts.parser()进行秘钥转意 转成一个字符串 返回一个claims方法  然后就可以打印token里面的数据了 

到此 生存token和解析token就完成了 很简单

下面是自定义claims

token在生成的时候是可以设置过期时间的 需要写一个有关时间的util类

这边有一个涉及签发util类 关于签发token 需要从笔记上查看



可以在Jwts.bulider()设置的时候进行自定义claim 上面显示Json 可以指定角色 也可以指定权限 

所以 一般来说 会自己手写一个生成token（签发token）的util和解析token的一个util直接拿来用就可以了 主要 记得写yaml

![image-20200530120921341](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20200530120921341.png)

解析请求

首先写一个返回Json的一个实体类

然后再写解析代码 token是通过http请求头进行解析的 所以需要获取token的key key存在了authorizationn里面 通过key获取到token 然后引用工具类进行解析 用claims获取到id 用id进行查询到user的所有数据 返回给之前写好的Json里面



JWT如何用 也可以用来认证和授权 当它是通过拦截器实现 签发用户可以用来当做权限 说实话 写起来还是有点麻烦的 这边也很少用JWT来实现权限 所以 需要再看笔记吧

下一章 讲Shiro