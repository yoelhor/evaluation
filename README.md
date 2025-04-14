# Evaluation


## Identity providers and authentication methods

The following table lists the authentication methods and external [identity providers](https://learn.microsoft.com/en-us/entra/external-id/identity-providers.md) for primary authentication and multifactor authentication (MFA) for external users.


|Method  |Sign-in  |Sign-up  |Password reset  |MFA  |
|---------|---------|---------|---------|---------|
| [Email with password](https://learn.microsoft.com/en-us/entra/external-id/customers/concept-authentication-methods-customers#email-and-password-sign-in) | Yes | Yes |  |  |
| [Email one-time passcode](https://learn.microsoft.com/en-us/entra/external-id/customers/concept-authentication-methods-customers#email-with-one-time-passcode-sign-in)| Yes | Yes |Yes  | Yes |
| [SMS-based authentication](https://learn.microsoft.com/en-us/entra/external-id/customers/concept-multifactor-authentication-customers#sms-based-authentication)|  |  |  | Yes |
| [Apple federation](https://learn.microsoft.com/en-us/entra/external-id/customers/how-to-apple-federation-customers) (preview)| Yes |Yes  |  |  |
| [Facebook federation](https://learn.microsoft.com/en-us/entra/external-id/customers/how-to-facebook-federation-customers) (preview)| Yes | Yes |  |  |
| [Google federation](https://learn.microsoft.com/en-us/entra/external-id/customers/how-to-google-federation-customers) (preview)| Yes | Yes |  |  |
| Microsoft peroanal account (via [OpenID Connect federation](https://learn.microsoft.com/en-us/entra/external-id/customers/how-to-custom-oidc-federation-customers) | Yes | Yes |  |  |
| [OpenID Connect federation](https://learn.microsoft.com/en-us/entra/external-id/customers/how-to-custom-oidc-federation-customers)| Yes | Yes |  |  |
| [SAML/WS-Fed](https://learn.microsoft.com/en-us/entra/external-id/direct-federation) to confirm with Bora| Yes| [Yes](https://learn.microsoft.com/en-us/entra/external-id/customers/how-to-saml-ws-federation-self-service-sign-up)||

## Application integration

Microsoft Entra External ID supports a variety of industry-standard protocols and authorization flows. To enable applications to interact with Microsoft Entra external ID, they must be [registered](https://learn.microsoft.com/en-us/entra/identity-platform/quickstart-register-app) in the Microsoft Entra External ID tenant. This registration allows applications to utilize Microsoft Entra's security features and enable provides single sign-on (SSO). This section outlines the features available for your applications.

### OpenID Connect and OAuth2 flows

The following table compares the features available for OAuth 2.0 and OpenID Connect authorization flows in each type of tenant.

- [Authorization code](../../identity-platform/v2-oauth2-auth-code-flow.md)
- [Authorization code with Code Exchange (PKCE)](../../identity-platform/v2-oauth2-auth-code-flow.md)
- [Client credentials](../../identity-platform/v2-oauth2-client-creds-grant-flow.md) using [v2.0 applications](../../identity-platform/reference-app-manifest.md) (preview)
- [Device authorization](../../identity-platform/v2-oauth2-device-code.md)
- [Implicit grant](../../identity-platform/v2-oauth2-implicit-grant-flow.md)
- [OpenID Connect](../../identity-platform/v2-protocols-oidc.md)
- [On-Behalf-Of flow](../../identity-platform/v2-oauth2-on-behalf-of-flow.md)
- Resource Owner Password Credentials (no) - For mobile and single page applications, use [native authentication](concept-native-authentication.md)

#### Certificates & secrets

For confidential applications, like web apps that are capable of storing sensitive data, application registration **credentials** in Microsoft Entra external ID are essential for establishing a secure trust relationship with your application. These credentials verify your application's identity, enabling it to safely obtain tokens and access to web APIs. This section lists the supported application registration credentials you application can use.

- [Certificate](https://learn.microsoft.com/en-us/entra/identity-platform/how-to-add-credentials)
- [Client secrets](https://learn.microsoft.com/en-us/entra/identity-platform/how-to-add-credentials)
- [Federated credentials](https://learn.microsoft.com/en-us/entra/identity-platform/how-to-add-credentials)

#### Authentication library

[Microsoft Authentication Library (MSAL)](https://learn.microsoft.com/en-us/entra/identity-platform/msal-overview) enables application developers to authenticate users and obtain security tokens to access secured web APIs, including our own web APIs or Microsoft Graph. MSAL supports both **OpenID Connect** and **OAuth2** protocols and is compatible with various application architectures and platforms, such as .NET, JavaScript, Java, Python, Android, and iOS, making it versatile and suitable for a wide range of applications.

### SAML relying parties

Microsoft Entra supports (preview) [SAML (Security Assertion Markup Language) relaying party](https://learn.microsoft.com/en-us/entra/external-id/customers/how-to-register-saml-app) applications, enabling seamless integration and secure single sign-on (SSO) for enterprise applications.

### Token customization

The following features are available for both JSON Web Tokens (JWT) and SAML tokens:

- Customize claims issued in security tokens using [Claim mapping](https://learn.microsoft.com/en-us/entra/external-id/customers/how-to-add-attributes-to-token).
- Apply a [transformation](https://learn.microsoft.com/en-us/entra/identity-platform/jwt-claims-customization) to a user attribute issued security tokens.
- Include claims from external systems using [custom claims provider](https://learn.microsoft.com/en-us/entra/identity-platform/custom-claims-provider-overview).
- [Configure groups optional](https://learn.microsoft.com/en-us/entra/identity-platform/optional-claims#configure-groups-optional-claims) claims are (limited to the group object ID).
- Application roles are included in the security token by default.
- You can [specify the lifetime](https://learn.microsoft.com/en-us/entra/identity-platform/configurable-token-lifetimes) of security tokens issued by the Microsoft Entra ID. However you cannot configure refresh and session token lifetimes. 

### Application registration automation

To streamline the application registration process for all types of applications, use Microsoft Graph or PowerShell scripts:

- Application registration - [create](https://learn.microsoft.com/en-us/graph/api/application-post-applications), [list](https://learn.microsoft.com/en-us/graph/api/application-list), [get](https://learn.microsoft.com/en-us/graph/api/application-get), and [delete](https://learn.microsoft.com/en-us/graph/api/application-delete).
- Application registraiton secrets (passwords) - [add](https://learn.microsoft.com/en-us/graph/api/application-addpassword) and [remove](https://learn.microsoft.com/en-us/graph/api/application-removepassword) secrets to an application registration.
- Application registraiton keys - [add](https://learn.microsoft.com/en-us/graph/api/application-addkey) and [remove](https://learn.microsoft.com/en-us/graph/api/application-removekey) keys to an application registration.
 
## Accounts and user profile

The user attributes you collect during sign-up are stored with the user's profile in your directory. You can choose from built-in user attributes or create custom user attributes.

- [Built-in user attributes](https://learn.microsoft.com/en-us/entra/external-id/customers/concept-user-attributes#built-in-user-attributes), such as city, country/region, email address, and so on, are available in Microsoft Entra External ID. You can choose the built-in user attributes you want to collect during sign-up.

- For any additional information you want to collect, you can create [custom user attributes](https://learn.microsoft.com/en-us/entra/external-id/customers/concept-user-attributes#custom-user-attributes).

Several [input controls](https://learn.microsoft.com/en-us/entra/external-id/customers/concept-user-attributes#custom-user-attributes-input-types) can be added to the sign-up page to collect the attributes, including text boxes, numeric text boxes radio buttons, and single select and multi-select check boxes.

### Admin account management

In Microsoft Entra External ID, administrators with the appropriate permissions can access the Microsoft Entra Admin Center. This serves as the primary interface for administrators to manage users and perform various tasks. You can automate account management by using the Microsoft Graph [user](https://learn.microsoft.com/en-us/graph/api/resources/users) endpoint. The following features are available for account management:

- [Create](https://learn.microsoft.com/en-us/entra/external-id/customers/how-to-manage-customer-accounts) an external account
- Manage user profile info
- [Reset a user's password](https://learn.microsoft.com/en-us/entra/external-id/customers/how-to-manage-customer-accounts#reset-a-users-password).
- [Delete](https://learn.microsoft.com/en-us/entra/external-id/customers/how-to-manage-customer-accounts#reset-a-users-password) user's account
- Restore or remove a recently deleted user.
- Disable accounts to prevent the new user from being able to sign in.
- [Assign application roles](https://learn.microsoft.com/en-us/entra/external-id/customers/how-to-use-app-roles-customers#app-roles) to users and groups to control who has access to content and functionality in the application.
- [Add users to security groups](https://learn.microsoft.com/en-us/entra/external-id/customers/how-to-use-app-roles-customers#groups).
- Reset their email or phone multi-factor authentication

### Self-service account management

Microsoft Entra External ID provides a [self-service profile editing](https://learn.microsoft.com/en-us/entra/identity-platform/quickstart-web-app-node-sign-in-edit-profile?toc=%2Fentra%2Fexternal-id%2Ftoc.json&bc=%2Fentra%2Fexternal-id%2Fbreadcrumb%2Ftoc.json&pivots=external), enabling users to update their profile information securely. Profile edit is performed within your applications by calling the Microsoft Graph API /me endpoint using "delegated permissions" with the user tokens.

Application administrators can configure multifactor authentication (MFA) to allow users to edit their profiles securely. This ensures that only authorized users can make changes to their profiles.

### Account restoration (soft delete)

An administrator or an application with appropriate permissions can delete a user account from the directory. After you delete a user, the account remains in a suspended state for 30 days. During that 30-day window, the user account can be restored, along with all its properties.

## Security

### Password protection

[Smart lockout](https://learn.microsoft.com/en-us/entra/identity/authentication/howto-password-smart-lockout) helps lock out bad actors that try to guess your users' passwords or use brute-force methods to get in. 