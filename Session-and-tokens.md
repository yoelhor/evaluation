# Session and secury tokens

Microsoft Entra external ID authenticates users and provides security tokens, such as access tokens, refresh tokens, and ID tokens. The security tokens are used by the client application to authenticate the user, and enable access to protected resources on a resource server, such as a REST API. This article explains the types of security tokens, session management, single sign-on, and single sign-out in Microsoft Entra external ID.

## Single sign-on session

Single sign-on (SSO) allows users to authenticate once and gain access to multiple applications without needing to re-enter their credentials. The following digram expains the single-sign-on exprince:

[**TBD**] add a diagrm.

1. When a user attempts to access an application, they are redirected to the Microsoft Entra external ID sign-in page.
1. Users enter their credentials, and Microsoft Entra external ID verifies their identity. They can also sign up and create a new account, or sign in using one of the social or corporate identity providers.
1. Upon successful authentication, Microsoft Entra external ID creates a session for the user. This session includes the “session token” that represents the user's authenticated state, enabling seamless access to other applications without requiring re-authentication.

   The session token is stored in the user's browser or device. Using different web browsers, such as Edge and Chrome, will likely result in separate sessions. Also, the session is maintained under a specific URL domain name. We recommend using a [single URL domain name](https://learn.microsoft.com/en-us/entra/external-id/customers/how-to-custom-url-domain) for all of your applications to take advantage of the single sign-on feature.
1. When a user attempts to access another application, they are redirected to the Microsoft Entra External ID sign-in page. The Microsoft Entra External ID validates the session cookie. If the session remains valid, Microsoft Entra External ID issues a new security token for the second application without requiring the user to re-enter their credentials. The seamless single sign-on experience may be disrupted by any of the following features and users will require to sign-in with their credentials.

   - The session token has an expiration time, after which the user must re-authenticate.
   - Conditinal access policies can be applied to enforce security measures, such as [multifactor authentication](https://learn.microsoft.com/en-us/entra/external-id/customers/concept-multifactor-authentication-customers).
   - [**TBC**] Microsoft Entra External ID may revoke the session if there is a change in the user's credentials, such as when an administrator resets the user's password, or due to suspicious activity.
   - Administrators can [revoke](https://learn.microsoft.com/en-us/graph/api/user-revokesigninsessions) sessions if suspicious activity is detected or if the user's credentials are compromised.


## Security tokens

Upon successful sign-in with or without SSO, Microsoft Entra external ID issues security tokens to the application. Security tokens are digital objects used in authentication and authorization processes to access resources. These tokens confirm a user's identity and grant access to resources without transmitting a password or credential for each transaction. The following are the OAuth 2.0 and OpenID Connect tokens:

- **Access token**  - the accesss token is issued by Microsoft Entra external ID as part of an OAuth 2.0 and OpenID Connect flows. It contains information about the user and the resource for which the token is intended. The information can be used to access web APIs and other protected resources. The protected resources, such as REST API validate access tokens to grant access to a client application.
- **Refresh token** - because access tokens are valid for only a short period of time, Microsoft Entra external ID issues a refresh token at the same time the access token is issued. Refresh tokens have a longer lifetime than access tokens. The default lifetime for the refresh tokens is 24 hours for single page apps and 90 days for all other scenarios. 
  
  When the access token expires, the client application uses the refresh token to obtain a new access token and refresh token without requiring the user to sign in again. Refresh tokens should be stored securely.
  
  Upon the expiration of the refresh token's lifetime period, the refresh token will expire and cannot be used. Additionally, refresh tokens may be [revoked](https://learn.microsoft.com/en-us/graph/api/user-revokesigninsessions) by using Graph API at any point prior to their expiration. The client app should handle such revocations gracefully by redirecting the user to an interactive sign-in prompt to reauthenticate and obtain a new token.

- **ID token** - ID tokens are used by client applications to verify that the user has been authenticated. They contain information about the user, such as their name and unique identifier. However, ID tokens do not grant permissions to protected resources such as REST APIs, as they do not contain scopes or permissions specifying the actions the user can perform on the protected resource. 

  The ID tokens are sent to the client application alongside or instead of an access token as part of an OpenID Connect flow.  Like access token, the ID tokens are valid for only a short period of time.

## Application session

Upon successful completion of the sign-in process and receipt of security tokens, the application may initiate its own session. This approach is commonly utilized in browser-based web applications. Once the session is established, the application manages access based on this session. It is possible that the user may not need to return to Microsoft Entra ID as long as the application session remains valid.

Other types of applications, such as mobile applications and single-page applications, typically utilize access tokens and refresh tokens for managing access to their features.

## Resources

- <https://learn.microsoft.com/en-us/entra/identity-platform/security-tokens>
- <https://learn.microsoft.com/en-us/entra/identity/users/users-revoke-access>
