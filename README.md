
# 会话有效期


在 Keycloak 中，"SSO Session Idle" 和 "SSO Session Max" 是用于配置单点登录（SSO）会话的两个参数。这两个参数影响用户在系统中的会话过期和最大有效时间。


1. **SSO Session Idle（单点登录会话空闲时间）：**


	* **定义：** 表示用户在系统中没有活动的时间阈值。如果用户在这段时间内没有与系统进行交互，那么他们的单点登录会话就被视为处于空闲状态。
	* **实际意义：** 当用户登录后，在一段时间内没有进行任何操作，系统会认为用户不再活跃，这时候可以选择在一定的时间后自动注销用户，以提高安全性。例如，设置为 30 分钟，如果用户在 30 分钟内没有任何操作，他们的会话将被认为是空闲的，可以选择要求用户重新验证身份。
2. **SSO Session Max（单点登录会话最大时间）：**


	* **定义：** 表示用户单点登录会话的最大有效时间。即使用户一直在系统中活跃，当达到此时间阈值后，他们的会话也会被强制注销。
	* **实际意义：** 即使用户一直在系统中活跃，为了安全和资源管理的目的，可以设置一个最大时间，超过这个时间后，用户将被强制重新登录，以确保他们的身份验证状态是最新的。例如，设置为 8 小时，用户的会话将在 8 小时后过期，需要重新登录。


这两个参数通常与安全性和用户体验的平衡相关。设置得太短可能导致用户频繁需要重新登录，而设置得太长可能增加安全风险。具体的设置应该根据你的应用程序的安全需求和用户行为来确定。


# Client Session有效期


"Client Session Idle" 和 "Client Session Max" 是 Keycloak 中用于配置客户端会话的两个参数。这两个参数影响与客户端相关的会话过期和最大有效时间。


1. **Client Session Idle（客户端会话空闲时间）：**


	* **定义：** 表示用户与特定客户端之间没有活动的时间阈值。如果用户在这段时间内没有与特定客户端进行交互，那么与该客户端关联的会话就被视为处于空闲状态。
	* **实际意义：** 当用户与特定客户端建立了会话后，在一段时间内没有与该客户端进行任何操作，系统会认为用户与该客户端的会话是空闲的。这可以用于自动注销与客户端的会话，以提高安全性。
2. **Client Session Max（客户端会话最大时间）：**


	* **定义：** 表示用户与特定客户端的会话的最大有效时间。即使用户一直在与特定客户端进行交互，当达到此时间阈值后，与该客户端关联的会话也会被强制注销。
	* **实际意义：** 即使用户与特定客户端一直在活跃，为了安全和资源管理的目的，可以设置一个最大时间，超过这个时间后，与该客户端关联的会话将被强制结束，用户可能需要重新进行身份验证。


这两个参数通常与客户端相关的会话管理和安全性相关。设置得太短可能导致用户频繁需要重新进行身份验证，而设置得太长可能增加安全风险。具体的设置应该根据你的应用程序的安全需求和用户行为来确定。


# 会话有效期谁小用谁


在 Keycloak 中，会话的有效期确实是由这些参数中的最小值决定的。换句话说，如果设置了 "SSO Session Idle"、"SSO Session Max"、"Client Session Idle" 和 "Client Session Max"，则会话将在这些参数中的最小值所定义的时间段内过期。


这种行为是为了确保会话的有效期在所有相关配置中都被严格限制，以提供更加精确的控制。这也意味着无论是整体的单点登录会话有效期还是与特定客户端关联的会话有效期，都将受到最小值的限制。


# 空闲会话



> 空闲时间这块，keycloak14\.0\.0有些bug，这块我进行了源码调整，当session max和session idle不同时，用户在session idle时间内不操作，用户会话也会超时。


在Keycloak中，有四个与空闲会话（Idle Session）相关的设置参数。它们的作用和关系如下：


1. Offline Session Idle（领域设置）：这是Keycloak的领域设置中的一个参数，用于定义空闲会话的超时时间。当用户在一段时间内没有与Keycloak进行任何交互时，会话会被视为处于空闲状态。默认情况下，该参数设置为30天。
2. Offline Session Max Limited（领域设置）：这是Keycloak的领域设置中的一个参数，用于定义空闲会话的最大持续时间。该参数定义了会话的最长持续时间，从会话开始到会话终止的时间段，以秒为单位。超过这个时间段后，会话将被标记为过期，并将被销毁。默认情况下，该参数也设置为30天。
3. Offline Session Max（领域设置）：这是Keycloak的客户端设置中的一个参数，用于定义特定客户端的空闲会话的最大持续时间。每个客户端可以具有自己的空闲会话最大持续时间，以使具有不同需求的客户端具有不同的会话时间。默认情况下，它继承自领域的 "Offline Session Max Limited" 值。
4. Client Offline Session Idle（客户端设置）：这是Keycloak的客户端设置中的一个参数，用于定义特定客户端的空闲会话的超时时间。每个客户端可以具有自己的空闲会话超时时间，以使具有不同需求的客户端具有不同的会话时间。默认情况下，它继承自领域的 "Offline Session Idle" 值。


这些参数共同用于控制空闲会话的超时和最大持续时间。"Offline Session Idle" 和 "Client Offline Session Idle" 确定了用户在没有与Keycloak进行任何交互时，会话被认为是空闲的时间。而 "Offline Session Max Limited" 和 "Offline Session Max" 确定了会话可以持续的最长时间。如果会话超过这个时间段，它将被销毁。


请注意，领域设置中的参数适用于整个领域（realm），而客户端设置中的参数适用于特定的客户端。通过为每个客户端设置不同的值，您可以针对不同的客户端应用程序调整会话时间。


# 关于会话空闲时间的bug修改


## 验证token逻辑，解析session idle和session max的逻辑


* org.keycloak.services.managers.AuthenticationManager.isSessionValid
* SessionTimeoutHelper.IDLE\_TIMEOUT\_WINDOW\_SECONDS是个时间戳口，它是120秒，为了解决集群环境下的时间差问题



```
  public static boolean isSessionValid(RealmModel realm, UserSessionModel userSession) {
  if (userSession == null) {
    logger.debug("No user session");
    return false;
  }
  int currentTime = Time.currentTime();

  // Additional time window is added for the case when session was updated in different DC 
  // and the update to current DC was postponed
  int maxIdle = userSession.isRememberMe() && realm.getSsoSessionIdleTimeoutRememberMe() > 0 ?
      realm.getSsoSessionIdleTimeoutRememberMe() : realm.getSsoSessionIdleTimeout();
  int maxLifespan = userSession.isRememberMe() && realm.getSsoSessionMaxLifespanRememberMe() > 0 ?
      realm.getSsoSessionMaxLifespanRememberMe() : realm.getSsoSessionMaxLifespan();

  boolean sessionIdleOk =
      maxIdle > currentTime - userSession.getLastSessionRefresh() - SessionTimeoutHelper.IDLE_TIMEOUT_WINDOW_SECONDS;
  boolean sessionMaxOk = maxLifespan > currentTime - userSession.getStarted();
  return sessionIdleOk && sessionMaxOk;
}

```

## session idle和session max的逻辑生效，如果修改refresh\_token生成时的校验逻辑


* 将verifyRefreshToken方法参数中的checkExpiration改成false



```
public RefreshResult refreshAccessToken(KeycloakSession session, UriInfo uriInfo, ClientConnection connection,
                                          RealmModel realm, ClientModel authorizedClient,
                                          String encodedRefreshToken, EventBuilder event, HttpHeaders headers,
                                          HttpRequest request) throws OAuthErrorException {

    // TODO: 完善实现了在线校验时，将参数checkExpiration从true改为false,session idle和session max的功能，
    // 但session idle到期后会话会提前注销，不会等session max到期
    RefreshToken refreshToken =
        verifyRefreshToken(session, realm, authorizedClient, request, encodedRefreshToken, false);

    event.user(refreshToken.getSubject()).session(refreshToken.getSessionState())
        .detail(Details.REFRESH_TOKEN_ID, refreshToken.getId())
        .detail(Details.REFRESH_TOKEN_TYPE, refreshToken.getType());

```

## session idle(空闲过期时间）和session max（最大过期时间）不相等时，产生的问题


1. session idle会作为刷新token的过期时间
2. 当这个时间到达后，不能再刷新token了，但是，session还是在线的
3. 是否需要在到达这个时间后，将会话删除？
4. 如果真要删除的话，可能产生的问题就是session max的时间还没到，但是session已经被删除了，这样就会导致session max的时间不准确了
5. 但如果session idle到达，并且token没有成功刷新，这说明用户空闲了，这时session是可以删除的，与4不矛盾
6. 解决方法
\*\[x] 在session idle到达后，将session删除，应该就解决问题了
\*\[x] 或者在生成code之前，判断它的session idle是否到期，如果到期，将会话删除，不能生成code


## keycloak关于删除过期会话的调度


* org.keycloak.services.scheduled.ScheduledTaskRunner \# 每5执行一次
	+ org.keycloak.services.scheduled.ClearExpiredUserSessionsTask
		- org.keycloak.models.map.authSession.removeExpired
		- 需要添加空间过期时间的判断，如果到期，就删除会话
		- 这块需要看如何手动清除，因为默认的，`infinispan中的session有自己的过期时间`，按着过期时间自动清除的
		- 咱们相当于，如何让它按着咱们的时间（这个时间经过了session idle的时间）来清除的
* ~~通过上面的分析，直接从infinispan中获取过期的session，`并删除不太可能`，人家通知初始化的过期时间自行维护的，而且这种过期时间，是session
max，而咱们的过期时间是可变的，它可能是一个session idle，也可能是session max，这与用户是否在idle时间内是否有操作有关~~


* 这种定时器，可能是为了mysql中存储的离线token用的，可查看offline\_user\_session相关的内容
* 我们如果在生成code时，添加一个判断，判断这个session idle是否到期，如果到期，就删除会话，不能生成code


## 生成code时，添加session idle的判断


* **如果不添加这个判断**，将会出现的问题是，当session idle和session max设置不同时，当session
idle到期后，用户的会话不会被删除，导致刷新token和申请code码换token，两块产生的token逻辑不一样，最终效果就是，code可以换回来token，但token在校验时是
`session not active`这样的错误。
* org.keycloak.protocol.AuthorizationEndpointBase.createAuthenticationSession()方法
![](./assets/%E5%8F%98%E6%9B%B4%E7%AC%94%E8%AE%B0-1704788815160.png)


![](https://img2024.cnblogs.com/blog/118538/202410/118538-20241023095237164-1446253875.png)


# 登录超时


* 如果用户在登录页，长时间停留（达到了后台配置的“登录超时”时间），不进行登录提交操作，会出现`登录超时，请重新开始登录`的提示，这时，页面自动刷新，用户重新提交登录请求即可。
* 登录超时配置：领域设置\-\-\-tokens选项，如图
![](https://img2024.cnblogs.com/blog/118538/202410/118538-20241023084753395-63344988.png)


对于一种开源框架和产品的学习，我们主要还是实践中去总结它，只有在实践中才能发现它的问题及那些“不为你知”的功能。


 本博客参考[milou加速器](https://xinminxuehui.org)。转载请注明出处！
