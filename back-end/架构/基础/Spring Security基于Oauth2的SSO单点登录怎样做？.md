# [Spring Security基于Oauth2的SSO单点登录怎样做？](https://my.oschina.net/zlt2000/blog/3184956)



## 一、说明

单点登录顾名思义就是在多个应用系统中，只需要登录一次，就可以访问其他相互信任的应用系统，免除多次登录的烦恼。本文主要介绍 `同域` 和 `跨域` 两种不同场景单点登录的实现原理，并使用 `Spring Security` 来实现一个最简单的**跨域** `SSO客户端` 。

 

## 二、原理说明

单点登录主流都是基于共享 `cookie` 来实现的，下面分别介绍 `同域` 和 `跨域` 下的两种场景具体怎样实现共享 `cookie` 的

### 2.1. 同域单点登录

**适用场景**：都是企业自己的系统，所有系统都使用同一个一级域名通过不同的二级域名来区分。

**举个例子**：公司有一个一级域名为 `zlt.com` ，我们有三个系统分别是：**门户系统(sso.zlt.com)**、**应用1(app1.zlt.com)\**和\**应用2(app2.zlt.com)**，需要实现系统之间的单点登录，实现架构如下：

![file](https://oscimg.oschina.net/oscnet/up-24b4f9ce83bb3f6c237ab47da1abe67b304.png)

**核心原理：**

1. **门户系统**设置 `Cookie` 的 `domain` 为一级域名也就是 `zlt.com`，这样就可以共享门户的 `Cookie` 给所有的使用该域名(`xxx.zlt.com`)的系统
2. 使用 `Spring Session` 等技术让所有系统共享 `Session`
3. 这样只要**门户系统**登录之后无论跳转**应用1**或者**应用2**，都能通过门户 `Cookie` 中的 `sessionId` 读取到 `Session` 中的登录信息实现**单点登录**

 

### 2.2. 跨域单点登录

单点登录之间的系统域名不一样，例如第三方系统。由于域名不一样不能共享 `Cookie` 了，这样就需要通过一个单独的授权服务(UAA)来做统一登录，并基于共享UAA的 `Cookie` 来实现单点登录。

**举个例子**：有两个系统分别是：**应用1(webApp.com)\**和\**应用2(zlt.com)\**需要实现单点登录，另外有一个\**UAA授权中心(sso.com)**，实现架构如下：

![file](https://oscimg.oschina.net/oscnet/up-ad31df8d9df57baa63719b987a59bad628e.png)

**核心原理：**

1. 访问**系统1**判断未登录，则跳转到**UAA系统**请求授权
2. 在**UAA系统**域名 `sso.com` 下的登录地址中输入用户名/密码完成登录
3. 登录成功后**UAA系统**把登录信息保存到 `Session` 中，并在浏览器写入域为 `sso.com` 的 `Cookie`
4. 访问**系统2**判断未登录，则跳转到**UAA系统**请求授权
5. 由于是跳转到**UAA系统**的域名 `sso.com` 下，所以能通过浏览器中UAA的 `Cookie` 读取到 `Session` 中之前的登录信息完成**单点登录**

 

### 2.3. 基于Oauth2的跨域单点登录流程

![mark](https://oscimg.oschina.net/oscnet/up-9e68f678aa355a65f00e1012b2760760266.png)

> 关于Oauth2的授权码模式这里就不做介绍了，自行找资料了解

 

## 三、Spring Security实现

**Oauth2单点登录**除了需要**授权中心**完成统一登录/授权逻辑之外

> 基于 `Spring Security` 实现的**UUA统一授权中心**可以参考：https://gitee.com/zlt2000/microservices-platform/tree/master/zlt-uaa

各个系统本身(sso客户端)也需要实现以下逻辑：

1. 拦截请求判断登录状态
2. 与 `UAA授权中心` 通过 `Oauth2授权码模式` 交互完成登录/单点登录
3. 保存用户登录信息

以上逻辑只需使用一个 `@EnableOAuth2Sso` 注解即可实现

![mark](https://oscimg.oschina.net/oscnet/up-823d73cc0ea7caaf22ad36b39ddf86d12bf.png)

SpringBoot配置如下：

![mark](https://oscimg.oschina.net/oscnet/up-cdc84630328a4075b6b04d72014ea63ffa6.png)

下图是访问 `sso客户端` 时 `@EnableOAuth2Sso` 注解与 `UAA授权中心` 通过 `Oauth2授权码模式` 交互完成单点登录的步骤

![mark](https://oscimg.oschina.net/oscnet/up-bd8c656291bc1705f0aa07d93c712112440.png)

> 请结合上面单点时序图中**单点登录系统2**的1~5步

**PS**：如果系统用的不是 `Spring Security` 怎么办？理解原理自行实现