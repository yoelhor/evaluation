# Evaluation


## 1. Authentication

Microsoft Entra External ID supports a variety of industry-standard protocols and authorization flows. To enable applications to interact with Microsoft Entra external ID, they must be [registered](https://learn.microsoft.com/en-us/entra/identity-platform/quickstart-register-app) in the Microsoft Entra External ID tenant. This registration allows applications to utilize Microsoft Entra's security features and enable provides single sign-on (SSO). This section outlines the features available for your applications.

### 1.1 OpenID Connect and OAuth2 authorization flows

The following table compares the features available for OAuth 2.0 and OpenID Connect authorization flows in each type of tenant.

- [Authorization code](../../identity-platform/v2-oauth2-auth-code-flow.md)
- [Authorization code with Code Exchange (PKCE)](../../identity-platform/v2-oauth2-auth-code-flow.md)
- [Client credentials](../../identity-platform/v2-oauth2-client-creds-grant-flow.md) using [v2.0 applications](../../identity-platform/reference-app-manifest.md) (preview)
- [Device authorization](../../identity-platform/v2-oauth2-device-code.md)
- [Implicit grant](../../identity-platform/v2-oauth2-implicit-grant-flow.md)
- [OpenID Connect](../../identity-platform/v2-protocols-oidc.md)
- [On-Behalf-Of flow](../../identity-platform/v2-oauth2-on-behalf-of-flow.md)
- Resource Owner Password Credentials (no) - For mobile and single page applications, use [native authentication](concept-native-authentication.md)

### 1.2 SAML relying parties authorization flows

Microsoft Entra supports (preview) [SAML (Security Assertion Markup Language) relaying party](https://learn.microsoft.com/en-us/entra/external-id/customers/how-to-register-saml-app) applications, enabling seamless integration and secure single sign-on (SSO) for enterprise applications.

Microsoft Entra ID supports **SP-initiated** authorization flows. In an SP-initiated SAML flow, the user starts the authentication process from the Service Provider's (relying party) application.

The authorization request may include the following parameters:

- [ForceAuthn](https://learn.microsoft.com/en-us/entra/identity-platform/single-sign-on-saml-protocol) (Boolean value) - if true, it means that the user will be forced to reauthenticate, even if they have a valid session with Microsoft Entra external ID.
- [NameIDPolicy](https://learn.microsoft.com/en-us/entra/identity-platform/single-sign-on-saml-protocol) this element requests a particular name ID format in the response.

### 1.3 Application registration

[Application registration](https://learn.microsoft.com/en-us/entra/identity-platform/quickstart-register-app?toc=%2Fentra%2Fexternal-id%2Ftoc.json&bc=%2Fentra%2Fexternal-id%2Fbreadcrumb%2Ftoc.json) in Microsoft Entra external ID is essential for establishing a trust relationship between your application and Microsoft Entra external ID. By registering your application, you obtain a unique Application (client) ID, and in some cases application secret or certificate. The application registration configure necessary settings like redirect URIs and API permissions. 

#### 1.3.1 Types of application

Microsoft Entra external ID supports authentication for various modern app architectures including:

- **Web Apps**: Traditional server-side web applications that serve web pages to users and use OAuth 2.0, OpenID Connect and [SAML](https://learn.microsoft.com/en-us/entra/external-id/customers/how-to-register-saml-app) for authentication.
- **Single-Page Apps (SPA)**: These apps have a single-page front end written primarily in JavaScript and use frameworks like Angular, React, or Vue.
- **Mobile and Native Apps**: Applications running on mobile devices or desktop environments, using MSAL for authentication. 
- **Web APIs**: Backend services that expose APIs for consumption by other applications, secured using OAuth 2.0.
- **Service, Daemon, and Script Applications**: Background services or scripts that run without user interaction, using client credentials for secure access to APIs.

For mobile and single-page apps, [Microsoft Entra’s native authentication](https://learn.microsoft.com/en-us/entra/external-id/customers/concept-native-authentication) allows you to have full control over the design of your mobile application sign-in experiences.

#### 1.3.2 Application Certificates & secrets

For confidential applications, like web apps that are capable of storing sensitive data, application registration **credentials** in Microsoft Entra external ID are essential for establishing a secure trust relationship with your application. These credentials verify your application's identity, enabling it to safely obtain tokens and access to web APIs. This section lists the supported application registration credentials you application can use.

- [Certificate](https://learn.microsoft.com/en-us/entra/identity-platform/how-to-add-credentials)
- [Client secrets](https://learn.microsoft.com/en-us/entra/identity-platform/how-to-add-credentials)
- [Federated credentials](https://learn.microsoft.com/en-us/entra/identity-platform/how-to-add-credentials)

##### 1.3.2.1 Certificates & secrets rotation

Application registration in Microsoft Entra external ID supports multiple credentials to facilitate rotation. Rotating secrets involves creating new secrets and deleting old ones. This can be done manually through the Microsoft Entra admin center or automated using Graph API or PowerShell script.

##### 1.3.2.2 Certificates for SAML applications

*TBD: confirmation*. Microsoft Entra ID [automatically generates a three-year](https://learn.microsoft.com/en-us/entra/identity/enterprise-apps/application-management-certs-faq) valid X509 certificate when you create a SAML application through the Microsoft Entra Application Gallery. Administrators can manage these certificates through the Microsoft Entra admin center, PowerShell, or Microsoft Graph.

To prevent disruptions due to expired certificates, Microsoft Entra ID sends email notifications 60, 30, and 7 days before a SAML certificate expires.

### 1.3.3 Delegate app registration permissions

Microsoft Entra ID allows you to [delegate application](https://learn.microsoft.com/en-us/entra/identity/role-based-access-control/delegate-app-roles) creation and management permissions in several ways:

- Assigning Application Owners: This method allows you to grant someone the ability to manage all aspects of Microsoft Entra configuration for a specific application.
- Assigning built-in administrative roles like **Application Developer role** or custom role that grants broad application configuration permissions without access to other parts of Microsoft Entra external ID.

### 1.3.4 Single sign-in

Microsoft Entra external ID's single sign-on (SSO) allows users to sign in using one set of credentials to multiple application. Using SSO means a user doesn't have to sign in to every application they use. With SSO, users can access all needed applications without being required to authenticate using different credentials.

#### 1.3.4.1 Keep me signed in (KMSI)

When a user selects the "Stay signed in?" prompt during the sign-in process, a persistent authentication session is set allowing the user to remain signed in even after closing and reopening their browse. The KMSI setting can be enabled or disabled by administrators in the Microsoft Entra admin center.

### 1.3.5 Single sign-out

When a user initiates a sign-out from an application (OpenID Connect and SAML) they are redirected to the Microsoft Entra logout endpoint. This ensures that the user's session is properly terminated both in the application and with Microsoft Entra ID.

Microsoft Entra uses **front-channel** Logout to ensure that when a user signs out from one application, they are also signed out from all other applications that participated in the single sign-on session. OpenID Connect **back-channel** logout is currently not supported.


### 1.3.6 Authentication library

[Microsoft Authentication Library (MSAL)](https://learn.microsoft.com/en-us/entra/identity-platform/msal-overview) enables application developers to authenticate users and obtain security tokens to access secured web APIs, including our own web APIs or Microsoft Graph. MSAL supports both **OpenID Connect** and **OAuth2** protocols and is compatible with various application architectures and platforms, such as .NET, JavaScript, Java, Python, Android, and iOS, making it versatile and suitable for a wide range of applications.

### 1.3.7 Consent to application permissions

Microsoft Entra External ID manages [user consent](https://learn.microsoft.com/en-us/entra/identity/enterprise-apps/user-admin-consent-overview) differently from Microsoft Entra ID for workforce, since it focuses primarily on secure and seamless authentication and authorization for external users. As a result, administrator consent is required. External users don't consent to application permissions. 

### 1.3.8 Terms of use policies

Microsoft Entra external ID allows you to add a [custom attribute](https://learn.microsoft.com/en-us/entra/external-id/customers/how-to-define-custom-attributes#configure-a-single-select-checkbox-checkboxsingleselect) (type of Boolean) to the sign-up page. Before completing the sign-up, users should read and accept your policies. For more information, learn how to collect user attributes during sign-up and configure a single-select checkbox.

Using the [Company branding](https://learn.microsoft.com/en-us/entra/external-id/customers/how-to-customize-branding-customers#to-customize-the-logo-privacy-link-and-terms-of-use) you can add links to "Terms of Use" and "Privacy & Cookies". These links can be localized to direct users to the relevant policy document.

### 1.3.9 Security tokens

### 1.3.9.1 Token customization

Upon successful sign-in, users will be taken back to your application with a security token. These tokens (JWT and SAML) can be customized (per application), including:

- Customize claims issued in security tokens using [claim mapping](https://learn.microsoft.com/en-us/entra/external-id/customers/how-to-add-attributes-to-token).
- Apply a [transformation](https://learn.microsoft.com/en-us/entra/identity-platform/jwt-claims-customization) to a user attribute issued security tokens.
- Include claims from external systems using [custom claims provider](https://learn.microsoft.com/en-us/entra/identity-platform/custom-claims-provider-overview).
- [Configure groups optional](https://learn.microsoft.com/en-us/entra/identity-platform/optional-claims#configure-groups-optional-claims) claims are (limited to the group object ID).
- Application roles are included in the security token by default.
- You can [specify the lifetime](https://learn.microsoft.com/en-us/entra/identity-platform/configurable-token-lifetimes) of security tokens issued by the Microsoft Entra ID. However you cannot configure refresh and session token lifetimes.

### 1.3.9.2 Token signing keys

Microsoft Entra external ID [signing keys roll](https://learn.microsoft.com/en-us/entra/identity-platform/signing-key-rollover) on periodically. In emergency situations, tenant administrators can update them immediately. The public keys are avalible via OpenID Connect discovery document and SAML/WS-Fed federation metadata document. All applications that use the Microsoft Entra external ID should be able to programmatically handle the key rollover process.
 
## 2. Accounts and user profile

User accounts for your consumers and business customers are most commonly created when users sign up for your applications. However, you can also create user accounts in the Microsoft Entra admin center or by using Microsoft Graph.

### 2.1 Type of accounts

There are two types of [user accounts](https://learn.microsoft.com/en-us/entra/external-id/customers/overview-customers-ciam) you can manage in your external tenant:

- **Customer account**: Accounts that represent the customers who access your applications. They can NOT access Azure resources such as the Azure portal. A customer user can be a local account or external account.
    - **Local accounts** are accounts that their credentials are managed in your Microsoft Entra external ID tenant, such as users who sign-in with a username and password, or username and one-time passcode.
    - **External accounts** Are accounts which are managed by external identity providers like Facebook or Google. 

- **Admin account**: Users with work accounts can manage resources in a tenant, and with an administrator role, can also manage tenants. Users with work accounts can create new consumer accounts, reset passwords, block/unblock accounts, and set permissions or assign an account to a security group.

All tasks and features mentioned in this section are applicable to all types of accounts.

### 2.2 User attributes

The user attributes you collect during sign-up are stored with the user's profile in your directory. You can choose from built-in user attributes or create custom user attributes.

- [Built-in user attributes](https://learn.microsoft.com/en-us/entra/external-id/customers/concept-user-attributes#built-in-user-attributes), such as city, country/region, email address, and so on, are available in Microsoft Entra External ID. You can choose the built-in user attributes you want to collect during sign-up.

- For any additional information you want to collect, you can create [custom user attributes](https://learn.microsoft.com/en-us/entra/external-id/customers/concept-user-attributes#custom-user-attributes).

The self-service sign-up offers several [input controls](https://learn.microsoft.com/en-us/entra/external-id/customers/concept-user-attributes#custom-user-attributes-input-types) can be added to the sign-up page to collect the attributes, including text boxes, numeric text boxes radio buttons, and single select and multi-select check boxes.

### 2.3 Account management by admin

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

### 2.4 Self-service account management

Microsoft Entra External ID provides a [self-service profile editing](https://learn.microsoft.com/en-us/entra/identity-platform/quickstart-web-app-node-sign-in-edit-profile?toc=%2Fentra%2Fexternal-id%2Ftoc.json&bc=%2Fentra%2Fexternal-id%2Fbreadcrumb%2Ftoc.json&pivots=external), enabling users to update their profile information securely. Profile edit is performed within your applications by calling the Microsoft Graph API /me endpoint using "delegated permissions" with the user tokens.

Application administrators can configure multifactor authentication (MFA) to allow users to edit their profiles securely. This ensures that only authorized users can make changes to their profiles.

### 2.5 Account restoration (soft delete)

An administrator or an application with appropriate permissions can delete a user account from the directory. After you delete a user, the account remains in a suspended state for 30 days. During that 30-day window, the user account can be restored, along with all its properties.

### 2.6 Sign-in logs and account auditing

- Microsoft Entra ID [sign-in logs](https://learn.microsoft.com/en-us/entra/identity/monitoring-health/concept-sign-ins) provide comprehensive information to assist administrators in monitoring and managing user activities. These logs include detailed data about both the user and the client.
- [Application user activity](https://learn.microsoft.com/en-us/entra/external-id/customers/how-to-user-insights) offers data analytics on the activity of users for registered applications in your tenant.
- Microsoft Entra activity logs include audit logs, which is a comprehensive report on every logged event in Microsoft Entra ID. Changes to applications, groups, users, and licenses are all captured in the Microsoft Entra audit logs.

## 3. Identity providers and authentication methods

The following table lists the authentication methods and external [identity providers](https://learn.microsoft.com/en-us/entra/external-id/identity-providers.md) for primary authentication and multifactor authentication (MFA) for external users.


|Method  |Sign-in  |Sign-up  |Password reset  |MFA  |
|---------|---------|---------|---------|---------|
| [Email with password](https://learn.microsoft.com/en-us/entra/external-id/customers/concept-authentication-methods-customers#email-and-password-sign-in) | Yes | Yes |  |  |
| [Email one-time passcode](https://learn.microsoft.com/en-us/entra/external-id/customers/concept-authentication-methods-customers#email-with-one-time-passcode-sign-in)| Yes | Yes |Yes  | Yes |
| [SMS-based authentication](https://learn.microsoft.com/en-us/entra/external-id/customers/concept-multifactor-authentication-customers#sms-based-authentication)|  |  |  | Yes |
| [Apple federation](https://learn.microsoft.com/en-us/entra/external-id/customers/how-to-apple-federation-customers) (preview)| Yes |Yes  |  |  |
| [Facebook federation](https://learn.microsoft.com/en-us/entra/external-id/customers/how-to-facebook-federation-customers) (preview)| Yes | Yes |  |  |
| [Google federation](https://learn.microsoft.com/en-us/entra/external-id/customers/how-to-google-federation-customers) (preview)| Yes | Yes |  |  |
| Microsoft personal account (via [OpenID Connect federation](https://learn.microsoft.com/en-us/entra/external-id/customers/how-to-custom-oidc-federation-customers) | Yes | Yes |  |  |
| [OpenID Connect federation](https://learn.microsoft.com/en-us/entra/external-id/customers/how-to-custom-oidc-federation-customers)| Yes | Yes |  |  |
| [SAML/WS-Fed](https://learn.microsoft.com/en-us/entra/external-id/direct-federation) to confirm with Bora| Yes| [Yes](https://learn.microsoft.com/en-us/entra/external-id/customers/how-to-saml-ws-federation-self-service-sign-up)||

### 3.1 Enable MFA

Administrators with certain roles can enable Multi-Factor Authentication (MFA) through conditional access policies for individual users and security groups. When a user signs in and is prompted for MFA, they must complete the MFA challenges.

## 4. Security

### 4.1 Password protection

[Smart lockout](https://learn.microsoft.com/en-us/entra/identity/authentication/howto-password-smart-lockout) helps lock out bad actors that try to guess your users' passwords or use brute-force methods to get in.

### 4.2 Password policy

Microsoft Entra ID enforces [password policies](https://learn.microsoft.com/en-us/entra/identity/authentication/concept-password-ban-bad-combined-policy) to enhance security and ensure robust password practices.

TBD: check which features are avalible in MEEID

### 4.2 Bot protection

#### 4.2.1 Arkose fraud protection

Arkose Labs' New Account Fraud Solution is designed to combat the creation of fraudulent accounts to ensure the security of your application while maintaining a seamless onboarding experience for legitimate users.

#### 4.2.2 Cloudflare WAF

[Cloudflare Web Application Firewall](https://learn.microsoft.com/en-us/entra/external-id/customers/tutorial-configure-cloudflare-integration) (Cloudflare WAF) to protect your organization from attacks, such as distributed denial of service (DDoS), malicious bots, Open Worldwide Application Security Project (OWASP) Top-10 security risks, and others

## 5. Authorization

Authorization in an app refers to the process of determining what actions a user is allowed to perform within the application.

### 5.1 Role-based access control

[Role-based access control (RBAC)](https://learn.microsoft.com/en-us/entra/external-id/customers/how-to-use-app-roles-customers) is a popular mechanism to enforce authorization in applications. When an organization uses RBAC, an application developer defines roles for the application. An administrator can then assign roles to different users and groups to control who has access to content and functionality in the application. Then, applications that receive the access token in a request can then make authorization decisions based on the values in the `roles` claim.

#### 5.1.1 Groups-based access control

Application developers can use [security groups](https://learn.microsoft.com/en-us/entra/external-id/customers/how-to-use-app-roles-customers#groups) to implement Role-based access control (RBAC) in their applications, where the memberships of the user in specific groups are interpreted as their role memberships. When an organization uses security groups, a `groups` claim is included in the token with all of the groups to which the user is assigned. Apps that receive the access token then make authorization decisions based on the values in the `groups` claim.

#### 5.1.2 Custom data store

App roles and groups both store information about user assignments in the Microsoft Entra directory. Another option for managing user role information that is available to developers is to maintain the information in the user's profile, like 'job title' or outside of the directory in a custom data store, using [custom claims provider](https://learn.microsoft.com/en-us/entra/identity-platform/custom-claims-provider-overview).

### 5.3 Require user assignment

Applications registered in a Microsoft Entra tenant are, by default, available to all users of the tenant who authenticate successfully. However, it's possible to configure application to [restrict access](https://learn.microsoft.com/en-us/entra/identity-platform/howto-restrict-your-app-to-a-set-of-users) to certain set of users or apps. Unassigned users cannot complete the sign-in process.

## 6. User experince

### 6.1 Auto-fill SMS code

The automatic filling of SMS verification codes into sign-in page is typically a feature provided by the operating system of the device rather than Microsoft Entra ID itself. For example, both iOS and Android have features that can detect SMS messages containing verification codes and offer to auto-fill them into the appropriate field Microsoft Entra ID.

### 6.2 Account selection

When a user attempts to sign in and users are already signed in within the same web browser, an account selector will be displayed. This will prompt users to choose the account with which they wish to sign in.


