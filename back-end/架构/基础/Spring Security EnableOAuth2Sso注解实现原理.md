# Spring Security EnableOAuth2Sso注解实现原理



## `@EnableOAuth2Sso`注解的源码

```
@Target(ElementType.TYPE)
@Retention(RetentionPolicy.RUNTIME)
@Documented
@EnableOAuth2Client
@EnableConfigurationProperties(OAuth2SsoProperties.class)
@Import({ OAuth2SsoDefaultConfiguration.class, OAuth2SsoCustomConfiguration.class,
		ResourceServerTokenServicesConfiguration.class })
public @interface EnableOAuth2Sso {

}
```

它解决的问题：

- 拦截未授权请求：如果用户没有登录，引导用户（去认证服务器）做登录认证
- (去认证服务器)获取用户的身份信息

> 对于后端渲染页面视图的Web项目，只需要加上这个注解，即可实现OAuth2接入，无需编写登录页面。如果是前后端分离项目，由于后端不能控制页面跳转，需要前端辅助完成授权码获取，并传递给后端完成后续认证流程。



## `EnableOAuth2Client`的处理

导入了`OAuth2ClientConfiguration` ，这个配置完成以下任务：

- 注册`OAuth2ClientContextFilter`过滤器，这个过滤器主要任务是负责捕获过滤链上的`UserRedirectRequiredException`异常，重定向到正确的`登录/授权`页面。
- 以bean的方式提供`AccessTokenRequest`

核心代码

```
public void doFilter(ServletRequest servletRequest,
		ServletResponse servletResponse, FilterChain chain)
		throws IOException, ServletException {
	HttpServletRequest request = (HttpServletRequest) servletRequest;
	HttpServletResponse response = (HttpServletResponse) servletResponse;
	request.setAttribute(CURRENT_URI, calculateCurrentUri(request));

	try {
		chain.doFilter(servletRequest, servletResponse);
	} catch (IOException ex) {
		throw ex;
	} catch (Exception ex) {
		// Try to extract a SpringSecurityException from the stacktrace
		Throwable[] causeChain = throwableAnalyzer.determineCauseChain(ex);
		UserRedirectRequiredException redirect = (UserRedirectRequiredException) throwableAnalyzer
				.getFirstThrowableOfType(
						UserRedirectRequiredException.class, causeChain);
		if (redirect != null) {
			redirectUser(redirect, request, response);
		} else {
			if (ex instanceof ServletException) {
				throw (ServletException) ex;
			}
			if (ex instanceof RuntimeException) {
				throw (RuntimeException) ex;
			}
			throw new NestedServletException("Unhandled exception", ex);
		}
	}
}
```

## `OAuth2SsoDefaultConfiguration`的处理

> Configuration for OAuth2 Single Sign On (SSO). If the user only has @EnableOAuth2Sso but not on a WebSecurityConfigurerAdapter then one is added with all paths secured.

确保当前应用的安全配置(`WebSecurityConfigurerAdapter`)总是存在。

## `OAuth2SsoCustomConfiguration`的处理

> Configuration for OAuth2 Single Sign On (SSO) when there is an existing WebSecurityConfigurerAdapter provided by the user and annotated with @EnableOAuth2Sso. The user-provided configuration is enhanced by adding an authentication filter and an authentication entry point.

对当前应用的安全配置(`WebSecurityConfigurerAdapter`)进行增强(这也是为`OAuth2SsoDefaultConfiguration`存在的意义)，提供认证过滤器和认证入口（让用户登录的地址）。

实现方式是通过反射对`HttpSecurity`再次进行配置,代码如下

```
private static class SsoSecurityAdapter implements MethodInterceptor {

	private SsoSecurityConfigurer configurer;

	SsoSecurityAdapter(ApplicationContext applicationContext) {
		this.configurer = new SsoSecurityConfigurer(applicationContext);
	}

	@Override
	public Object invoke(MethodInvocation invocation) throws Throwable {
		if (invocation.getMethod().getName().equals("init")) {
			Method method = ReflectionUtils
					.findMethod(WebSecurityConfigurerAdapter.class, "getHttp");
			ReflectionUtils.makeAccessible(method);
			HttpSecurity http = (HttpSecurity) ReflectionUtils.invokeMethod(method,
					invocation.getThis());
			this.configurer.configure(http);
		}
		return invocation.proceed();
	}

}
```

`OAuth2 SSO`的主要实现由`SsoSecurityConfigurer`完成，它提供认证过滤器和认证入口。

### 认证入口

> 简单的说，认证入口就是负责在发生未认证请求时，提供一个登录页面。

安装异常处理器，捕获过滤链上抛出的`Spring Security`相关异常，然后发重定向响应。

```
private void addAuthenticationEntryPoint(HttpSecurity http, OAuth2SsoProperties sso)
		throws Exception {
	...
    // sso.getLoginPath() 就是某个OAuth2 认证服务器的地址
	exceptions.defaultAuthenticationEntryPointFor(
			new LoginUrlAuthenticationEntryPoint(sso.getLoginPath()),
			preferredMatcher);
    ...
}
```

### 认证过滤器（`OAuth2ClientAuthenticationProcessingFilter`）

对请求进行安全拦截的过滤器由`OAuth2ClientAuthenticationProcessingFilter`实现，核心代码如下：

```
// 这个父类方法，来自AbstractAuthenticationProcessingFilter
public void doFilter(ServletRequest req, ServletResponse res, FilterChain chain)
		throws IOException, ServletException {

	HttpServletRequest request = (HttpServletRequest) req;
	HttpServletResponse response = (HttpServletResponse) res;

    // 只有登录请求才会被此过滤器处理
	if (!requiresAuthentication(request, response)) {
		chain.doFilter(request, response);

		return;
	}

	if (logger.isDebugEnabled()) {
		logger.debug("Request is to process authentication");
	}

	Authentication authResult;

	try {
        // 做实际认证的地方，认证成功就会返回Authentication对象，当前用户的身份就确认了
		authResult = attemptAuthentication(request, response);
		if (authResult == null) {
			// return immediately as subclass has indicated that it hasn't completed
			// authentication
			return;
		}
		sessionStrategy.onAuthentication(authResult, request, response);
	}
	catch (InternalAuthenticationServiceException failed) {
		logger.error(
				"An internal error occurred while trying to authenticate the user.",
				failed);
		unsuccessfulAuthentication(request, response, failed);

		return;
	}
	catch (AuthenticationException failed) {
		// Authentication failed
		unsuccessfulAuthentication(request, response, failed);

		return;
	}

	// Authentication success
	if (continueChainBeforeSuccessfulAuthentication) {
		chain.doFilter(request, response);
	}

    // 认证成功后，Authentication 对象就会由SecurityContextHolder保存，供后续过滤链使用
	successfulAuthentication(request, response, chain, authResult);
}
@Override
public Authentication attemptAuthentication(HttpServletRequest request, HttpServletResponse response)
		throws AuthenticationException, IOException, ServletException {

	OAuth2AccessToken accessToken;
	try {
		// 取token，没有token或者已经过期就会往认证服务器发请求拿新的
		accessToken = restTemplate.getAccessToken();
	} catch (OAuth2Exception e) {
		BadCredentialsException bad = new BadCredentialsException("Could not obtain access token", e);
		publish(new OAuth2AuthenticationFailureEvent(bad));
		throw bad;			
	}
	try {
		// 用token换用户信息
		OAuth2Authentication result = tokenServices.loadAuthentication(accessToken.getValue());
		if (authenticationDetailsSource!=null) {
			request.setAttribute(OAuth2AuthenticationDetails.ACCESS_TOKEN_VALUE, accessToken.getValue());
			request.setAttribute(OAuth2AuthenticationDetails.ACCESS_TOKEN_TYPE, accessToken.getTokenType());
			result.setDetails(authenticationDetailsSource.buildDetails(request));
		}
		publish(new AuthenticationSuccessEvent(result));
		return result;
	}
	catch (InvalidTokenException e) {
		BadCredentialsException bad = new BadCredentialsException("Could not obtain user details from token", e);
		publish(new OAuth2AuthenticationFailureEvent(bad));
		throw bad;			
	}

}
```

处理逻辑：

- 尝试执行用户认证（

  ```
  attemptAuthentication
  ```

  ）

  - 获取access token（

    ```
    OAuth2RestTemplate.getAccessToken
    ```

    ）

    - 检查有没有授权码,没有就抛出`UserRedirectRequiredException`，这个异常之后会被过滤链上的异常处理器捕获，见前面**认证入口**的说明。注意，本次请求到这一步就处理完了，浏览器会重定向到认证服务器的授权页面，授权回调地址就是当前地址，所以授权成功后又会走一遍`attemptAuthentication`，不过这一次就有授权码了。
    - 有授权码就用授权码去认证服务器拿access token

  - 用access token换用户信息(`RemoteTokenServices.loadAuthentication`)

- 认证成功，做一些上下文数据保存和事件发布之类的善后工作

> 备注：
>
> 认证服务器是一个外部应用
>
> 授权流程走的是oauth2 授权码模式
>
> 客户端（也就是本应用）并不需要有登录页面，登录动作由过滤器在自动完成

## `ResourceServerTokenServicesConfiguration`的处理

> Configuration for an OAuth2 resource server.

主要用处是将当前应用配置为资源服务器。

# 例子

核心依赖

```
<dependencies>
    <dependency>
        <groupId>org.springframework.security.oauth.boot</groupId>
        <artifactId>spring-security-oauth2-autoconfigure</artifactId>
        <version>2.1.9.RELEASE</version>
    </dependency>
    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-security</artifactId>
    </dependency>
</dependencies>
```

开启注解

```
@EnableOAuth2Sso
@SpringBootApplication
public class DemoOauth2SsoClientApplication {
    public static void main(String[] args) {
        SpringApplication.run(DemoOauth2SsoClientApplication.class, args);
    }
}
```

配置文件

```
# OAuth2 认证服务器地址
oauth2.auth-server.base-url: http://localhost:28080

security:
  oauth2:
    # oauth2 客户端配置信息，需要与认证服务器登记的客户端信息一致
    client:
      client-id: test
      client-secret: test
      user-authorization-uri: ${oauth2.auth-server.base-url}/oauth/authorize
      access-token-uri: ${oauth2.auth-server.base-url}/oauth/token
      scope: server
    resource:
      # /oauth/check_token ： 用于获取登录用户身份信息
      token-info-uri: ${oauth2.auth-server.base-url}/oauth/check_token
```