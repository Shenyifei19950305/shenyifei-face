springsecurity 文件是狂神的 需要去公众号进行查看 

实战拿松哥的那一套吧 毕竟比较熟悉

先说下springsecurity 然后再去看shiro 最后比较下两者 为什么选择后者

其实一般用的最多的就是springsecurity的权限和认证吧 等会会说具体的代码



- WebSecurityConfigurerAdapter：自定义Security策略 适配器模式
- AuthenticationManagerBuilder：自定义认证策略 构造者模式
- @EnableWebSecurity：开启WebSecurity模式

Spring Security的两个主要目标是 “认证” 和 “授权”（访问控制）。其实关于这两个 一搬过滤器和拦截器也是可以实现的 底层代码太过于复制 springsecurity 是基于切面 AOP的基础上进行设计的

首先 继承适配器 一般认证都是http授权比较多 configure（httpSecurity http）

```java
@Override
protected void configure(HttpSecurity http) throws Exception {
   // 定制请求的授权规则
   // 首页所有人可以访问
   http.authorizeRequests().antMatchers("/").permitAll()
       .antMatchers("/level1/**").hasRole("vip1")
       .antMatchers("/level2/**").hasRole("vip2")
       .antMatchers("/level3/**").hasRole("vip3");
   http.formlogin()//表示没有权限的话会自动返回登录页面
   http.logout().logoutSuccessUrl("/");//注销成功跳到首页 
}
```

用authorizerequests进行配置匹配 根据不同的角色分配不同的请求地址

上面是授权  授权要根据认证来进行定规矩的

```java
//定义认证规则
@Override
protected void configure(AuthenticationManagerBuilder auth) throws Exception {
   //在内存中定义，也可以在jdbc中去拿....
   //Spring security 5.0中新增了多种加密方式，也改变了密码的格式。
   //要想我们的项目还能够正常登陆，需要修改一下configure中的代码。我们要将前端传过来的密码进行某种方式加密
   //spring security 官方推荐的是使用bcrypt加密方式。
   
   auth.inMemoryAuthentication().passwordEncoder(new BCryptPasswordEncoder())
          .withUser("kuangshen").password(new BCryptPasswordEncoder().encode("123456")).roles("vip2","vip3")
          .and()
          .withUser("root").password(new BCryptPasswordEncoder().encode("123456")).roles("vip1","vip2","vip3")
          .and()
          .withUser("guest").password(new BCryptPasswordEncoder().encode("123456")).roles("vip1","vip2");
}
```

需要一个密码加密规则  new BCryptPasswordEncoder() 

接下来说的是，比如kuangshen这个用户，它只有 vip2，vip3功能，那么登录则只显示这两个功能，而vip1的功能菜单不显示！这个就是真实的网站情况了！该如何做呢？

我们需要结合thymeleaf中的一些功能

接下来就是添加pom然后就是前端进行修改，项目中用的是vue，所以只要Json传给前端一个enableview，0或者1 0表示不显示  1表示显示，具体要看项目



接下来最后一个就是记住我的按钮

可以直接在springSecury configure里面进行配置 利用cookie进行保存

最后一点就是用自己的登录页 而不是用springsecurity的登录页

```java
http.formLogin()
  .usernameParameter("username")
  .passwordParameter("password")
  .loginPage("/toLogin")
  .loginProcessingUrl("/login"); // 登陆表单提交请求
```

## 完整配置代码

```java
package com.kuang.config;

importorg.springframework.security.config.annotation.authentication.builders.AuthenticationManagerBuilder;
import org.springframework.security.config.annotation.web.builders.HttpSecurity;
importorg.springframework.security.config.annotation.web.configuration.EnableWebSecurity;
importorg.springframework.security.config.annotation.web.configuration.WebSecurityConfigurerAdapter;
import org.springframework.security.crypto.bcrypt.BCryptPasswordEncoder;

@EnableWebSecurity
public class SecurityConfig extends WebSecurityConfigurerAdapter {

   //定制请求的授权规则
   @Override
   protected void configure(HttpSecurity http) throws Exception {

       http.authorizeRequests().antMatchers("/").permitAll()
      .antMatchers("/level1/**").hasRole("vip1")
      .antMatchers("/level2/**").hasRole("vip2")
      .antMatchers("/level3/**").hasRole("vip3");


       //开启自动配置的登录功能：如果没有权限，就会跳转到登录页面！
           // /login 请求来到登录页
           // /login?error 重定向到这里表示登录失败
       http.formLogin()
          .usernameParameter("username")
          .passwordParameter("password")
          .loginPage("/toLogin")
          .loginProcessingUrl("/login"); // 登陆表单提交请求

       //开启自动配置的注销的功能
           // /logout 注销请求
           // .logoutSuccessUrl("/"); 注销成功来到首页

       http.csrf().disable();//关闭csrf功能:跨站请求伪造,默认只能通过post方式提交logout请求
       http.logout().logoutSuccessUrl("/");

       //记住我
       http.rememberMe().rememberMeParameter("remember");
  }

   //定义认证规则
   @Override
   protected void configure(AuthenticationManagerBuilder auth) throws Exception {
       //在内存中定义，也可以在jdbc中去拿....
       //Spring security 5.0中新增了多种加密方式，也改变了密码的格式。
       //要想我们的项目还能够正常登陆，需要修改一下configure中的代码。我们要将前端传过来的密码进行某种方式加密
       //spring security 官方推荐的是使用bcrypt加密方式。

       auth.inMemoryAuthentication().passwordEncoder(newBCryptPasswordEncoder())
              .withUser("kuangshen").password(newBCryptPasswordEncoder().encode("123456")).roles("vip2","vip3")
              .and()
              .withUser("root").password(newBCryptPasswordEncoder().encode("123456")).roles("vip1","vip2","vip3")
              .and()
              .withUser("guest").password(newBCryptPasswordEncoder().encode("123456")).roles("vip1","vip2");
  }
}
```