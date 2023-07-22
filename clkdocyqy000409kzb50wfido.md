---
title: "Security Best Practices for APIs"
datePublished: Wed Jun 14 2023 07:13:18 GMT+0000 (Coordinated Universal Time)
cuid: clkdocyqy000409kzb50wfido
slug: security-best-practices-for-apis
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1690009898837/c7d16662-d14d-4c60-a305-ff95b4eb3506.png
tags: security, apis

---

# Introduction

Modern digital transformation is built on APIs, driving a new operating model that provides direct access to business logic, applications, and institutional data. While this access is invaluable for partners and customers, it also makes APIs attractive targets for hackers and cybercriminals. Therefore, it is very important to focus on API security.

In this article, you will learn why it is so important to protect your API, some API security methodologies and best practices, and the role API plays in the DevSecOps paradigm.

## Why is Security Important?

Organizations use APIs (application programming interfaces) to communicate with other systems and transfer data. A poorly developed API can expose sensitive medical, financial, and/or personal data. Privacy of user information is a high priority, and this data must be guarded with care.

There have been several cases in which companies have been hacked because of an open and insecure API, thereby exposing user data. [Venmo](https://venmo.com/), a peer-to-peer mobile payment service, was hacked by a computer science student who was able to access information on seven million Venmo transactions, including the full names of people sending money through the platform. This happened, at least in part, because [Venmo was lax](https://www.csoonline.com/article/3410044/6-lessons-from-venmos-lax-approach-to-api-security.html) in making data accessible through a public API.

Similarly, [Ledger](https://www.ledger.com/), a French cryptocurrency hardware wallet company, experienced a data breach due to an insecure API. While the wallets and cryptocurrencies were well protected, [a third-party API misconfiguration ended up leaking the personal data of their customers](https://cryptobriefing.com/ledger-breach-clients-data-leaked/?amp). The breach resulted in over 270,000 phone numbers and addresses being leaked and the exposure of more than a million customer email addresses. As you can see, ensuring that your API is well-developed and properly protected is of huge importance.

## Role of APIs in DevSecOps

While traditional security teams rely on testing software at the end of the build process, this often causes inefficiencies and delays as developers must spend time implementing security fixes to new versions before releasing features to the end customer. However, with DevSecOps—a philosophy that involves integrating security best practices during the development and operations processes—performing security tests is no longer done at the end of the build pipeline. Instead, it becomes an integral part of the development process, allowing issues like vulnerable or outdated libraries, wrong API configurations, or possible sensitive data leakages to be discovered and fixed earlier.

## API Security Methodologies

There are several techniques you can implement to increase your API’s security, each with a unique set of benefits. Here are just a few of the possible methods you can use to authenticate your API:

### API Keys

API keys are good for developer quickstart and allow users to have access to all the resources on a platform as long as the `API-Key` is provided on every request. Essentially, it’s an encrypted string that identifies an application without paying attention to the user of the application. API keys provide project authorization and identification; in other words, the platform identifies the project or application making a request to the API and checks to see if the application is authorized to make a request to the platform. The calling application needs to add the key to each API request, and the API can use the key to identify the application and authorize the request.

However, API keys are not totally secure as they are usually accessible to the client. This may make it easy for a hacker to steal the key, and since API keys don’t usually have an expiration time, a stolen key can be used indefinitely unless the owner revokes or generates a new key.

On the plus side, API keys can be used when you need to block anonymous traffic if you want to allow only traffic from a particular application. You can also use them to control and limit the number of calls made to your API, to filter application logs or to identify application usage patterns.

### Basic Auth

Basic Authentication, or basic auth for short, is a simple method of authenticating API requests. It uses a header called Authorisation, with a base64 encoded representation of the username and password of the user. For example, a request using basic authentication for the user `tomiwa` and password `123456` looks like this:

```typescript
GET / HTTP/1.1
Host: example.com
Authorization: Basic dG9taXdhOjEyMzQ1Ng==
```

Even though basic auth is easy to implement and suitable for server-server communication, using it for client-server communication can pose several threats. Sending user credentials for every request would be considered bad practice, as the user is not aware of what the app will use the credentials for, and the only way to revoke access is to change the password. Also, the passwords are usually long-lived, and if an attacker has access to the password and username, this can lead to significant damage.

Basic Authentication can be used in a scenario where you want a simple way of authenticating users while enforcing security. It can also save a lot of time when you need to quickly get up and running with authentication and don’t want to spend much time thinking about roles, permissions scopes, etc.

### JWT

JSON Web Tokens, also known as JWT, is a standard for safely exchanging claims between two parties. These claims are assertions about a certain object to ensure its validity. JWT provides various types of signatures and encryption. The signatures are used for validation to guard against data tampering, while the encryption is useful for protecting data from being accessed by third parties. The process starts by sending a username and password to the server and then validating the information sent.

Once validated, the server generates a token, which is usually made up of a header, payload, and signature separated by dots based on a secret key that only the server knows. The client can then include this token in the headers of subsequent requests, and the server will validate it using the secret key. The generated token is usually valid for a period of time, after which the client can use a refresh token to request a new one. This allows the server to block access to clients if required.

There are several benefits of using JWTs. They are more secure as they provide a public/private key pair in the form of an X.509 certificate for signing. They can also be used in federated identities. For example, the ID Token returned when a user logs in successfully with their credential in the [OpenID Connect’s](https://openid.net/specs/openid-connect-core-1_0.html#IDToken) spec is a JSON Web Token. JSON Web Tokens are very common and are used at the internet scale. They can also be used on multiple platforms, especially mobile.

## Authorization Methods

While Authentication checks if a user exists on a platform, authorization focuses more on verifying if a user or entity has the right to perform certain operations, such as whether a user can view the photos of other users in a photo-sharing application. There are several methods to be aware of, some of which include the following:

### RBAC – Role-Based Access Control

Role-Based Access Control (RBAC) is a security paradigm that allows users to have restricted access to resources based on their roles in the organization. RBAC allows you to assign roles to users; each role grants access to one or more sets of rights and that in turn determines the kind of operation that particular user can perform on the platform.

The basic principle of Role-Based Access Control is simple; for example, the Human Resources department can’t see Finance data, and vice versa. When implemented correctly, RBAC will be transparent to the users. Role assignment happens behind the scenes, and each user has access only to the applications and data that they need to do their job. When you have a structured workgroup and want to be able to define the rights to a system by specific roles, RBAC is a great option.

### ABAC – Attribute-Based Access Control

Attribute-Based Access Control (ABAC) is an authorization model that evaluates the characteristics or attributes of an entity, instead of roles, to determine access. For example, you might only want to allow users of a particular type, such as permitting employees in the HR department to access the HR/Payroll system, and only during business hours within the same time zone as the company.

At its core, ABAC enables flexible and fine-grained access control that allows for more input variables into an access control decision. Any available attribute in the directory can be used by itself or in combination with another to define the right filter for controlling access to a resource.

Because you can define access by employee type, location, and business hours, ABAC is usually suitable for geographically dispersed workgroups.

### OAuth2.0

OAuth 2.0, which stands for “Open Authorisation,” is a standard that allows a website or application to access resources hosted by other web apps on behalf of a user. It’s a way of securely saying that it’s okay for a platform to use one of your trusted authentications to allow access to the platform's resources. For example, you might use it to tell GitHub that it’s okay for Linkedin to use your GitHub profile.

OAuth is mainly used for authorization and doesn’t share password data, but instead uses tokens to prove an identity between consumers and service providers. It also provides consented access and restricts actions that the client app can perform on resources on behalf of the user, without ever sharing the user’s credentials.

It is important to note that OAuth 2.0 is an authorization protocol and **not** an authentication protocol. As such, it is designed primarily as a means of granting access to a set of resources available on another system, such as remote APIs or user data.

### Open Policy Agent

The Open Policy Agent (OPA) is a domain-agnostic, general-purpose policy engine that gives you the ability to decouple policy and decision-making of a dedicated system. It automates and unifies policy enforcement and implementation across a wide range of technologies and across several IT environments, especially in cloud-native applications. OPA was originally created by [Styra](https://www.styra.com/) and has since been accepted by the Cloud Native Computing Foundation. The OPA is offered for use under an open-source license.

Organizations use the OPA to automatically enforce, monitor and remediate policies across all relevant components. You can use OPA to centralize operational, security, and compliance functions across Kubernetes, application programming interface (API) gateways, continuous integration/continuous delivery (CI/CD) pipelines, data protection, and more.

## Other API Security Best Practices

Finally, there are a few other best practices in API security that are worth mentioning:

### Define Ownership for Security

With the introduction of the DevSecOps paradigm, security becomes the responsibility of everyone on the team—from the developers to the QA and DevOps engineers. That means not only the security team is responsible for ensuring the software’s security, but all stakeholders must take a vested interest in the API’s security.

The benefits of ensuring that all stakeholders accept responsibility for a software’s security are enormous. It reduces the time it takes to identify issues and bottlenecks in software and the time it takes to resolve them. It also speeds up the time it takes to deliver value to end customers, and encourages accountability at each stage of the development as each stakeholder must put their best foot forward toward the achievement of the team goal.

### Audit Logs

An audit log is a record of events as they happen within a computer system. A system of log-keeping and records becomes an audit trail where anyone investigating actions within a system can trace the actions of users, access to given files, or other activities, such as the execution of files under root or administrator permissions, or changes to OS-wide security and access settings.

Audit logs are very useful when there is a need to identify or track the cause of an issue or event. For example, they can be used to track how data went missing on a platform. They can also be used to make informed decisions in the future as the data logged in real-time can also serve as feedback on how to improve the system going forward.

### Identity Providers (IdPs)

An identity provider (IdP or IDP) stores and manages users’ digital identities. Think of an IdP as being like a guest list but for digital and cloud-hosted applications instead of an event. An IdP may check user identities via username-password combinations and other factors, or it may simply provide a list of user identities that another service provider (like an SSO) checks.

IdPs are not limited to verifying human users. Technically, an IdP can authenticate any entity connected to a network or a system, including computers and other devices. Any entity stored by an IdP is known as a “principal” (instead of a “user”). However, IdPs are most often used in cloud computing to manage user identities.

IdPs can be used when organizations need to delegate or outsource the managing and controlling of employee information from a central source without having to build a custom solution to do so. This can save time as well as provide a platform to manage all employee data in the long run while ensuring that the security of user information remains tight.

## Conclusion

API security is a very important topic as many applications today use APIs to communicate between systems. In this article, you have learned what API security is, how it can affect your software, and various methods and best practices you can apply to mitigate cybercrime or exposure of confidential data. You also learned the role DevSecOps culture plays in securing APIs and how security testing should be performed at each stage of the software development lifecycle, thereby reducing the time it takes to develop and deploy applications to the end users.