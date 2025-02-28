---
title: Microsoft Identity Web authentication library overview
description: Learn about Microsoft Identity Web, an authentication and authorization library for ASP.NET Core applications that integrate with Azure Active Directory, Azure AD B2C, and Microsoft Graph and other web APIs.
services: active-directory
author: jmprieur
manager: CelesteDG

ms.service: active-directory
ms.subservice: develop
ms.topic: conceptual
ms.workload: identity
ms.date: 11/19/2021
ms.author: jmprieur
ms.reviewer: marsma
ms.custom: "devx-track-csharp, aaddev"
# Customer intent: As an application developer, I want to learn how to add authentication to ASP.NET Core web apps and authorization to protected web APIs.
---

# Microsoft Identity Web authentication library

Microsoft Identity Web is a set of ASP.NET Core libraries that simplifies adding authentication and authorization support to web apps and web APIs integrating with the Microsoft identity platform. It provides a single-surface API convenience layer that ties together ASP.NET Core, its authentication middleware, and the [Microsoft Authentication Library (MSAL) for .NET](https://github.com/azuread/microsoft-authentication-library-for-dotnet).

You can get Microsoft.Identity.Web from NuGet or by using a Visual Studio project template to create a new app project.

## Supported application scenarios

If you're building ASP.NET Core web apps or web APIs and want to use Azure Active Directory (Azure AD) or Azure AD B2C for identity and access management (IAM), we recommend using Microsoft Identity Web for all of these scenarios:

- [Web app that signs in users](scenario-web-app-sign-user-overview.md)
- [Web app that signs in users and calls a web API on their behalf](scenario-web-app-call-api-overview.md)
- [Protected web API that only authenticated users can access](scenario-protected-web-api-overview.md)
- [Protected web API that calls another (downstream) web API on behalf of the signed-in user](scenario-web-api-call-api-overview.md)

## Install from NuGet

Microsoft Identity Web is available on NuGet as a set of packages that provide modular functionality based on your app's needs. Use the .NET CLI's `dotnet add` command or Visual Studio's **NuGet Package Manager** to install the packages appropriate for your project:

- [Microsoft.Identity.Web](https://www.nuget.org/packages/Microsoft.Identity.Web) - The main package. Required by all apps that use Microsoft Identity Web.
- [Microsoft.Identity.Web.UI](https://www.nuget.org/packages/Microsoft.Identity.Web.UI) - Optional. Adds UI for user sign-in and sign-out and an associated controller for web apps.
- [Microsoft.Identity.Web.MicrosoftGraph](https://www.nuget.org/packages/Microsoft.Identity.Web.MicrosoftGraph) - Optional. Provides simplified interaction with the Microsoft Graph API.
- [Microsoft.Identity.Web.MicrosoftGraphBeta](https://www.nuget.org/packages/Microsoft.Identity.Web.MicrosoftGraphBeta) - Optional. Provides simplified interaction with the Microsoft Graph API [beta endpoint](/graph/api/overview?view=graph-rest-beta&preserve-view=true).

## Install by using a Visual Studio project template

Several project templates that use Microsoft Identity Web are included in .NET SDK versions 5.0 and above. The project templates aren't included in the ASP.NET Core 3.1 SDK, but you can install them separately.

### .NET 5.0+ - Project templates included

The Microsoft Identity Web project templates are included in .NET SDK versions 5.0 and above.

This example .NET CLI command creates a Blazor Server project that includes Microsoft Identity Web.

```dotnetcli
dotnet new blazorserver --auth SingleOrg --calls-graph --client-id "00000000-0000-0000-0000-000000000000" --tenant-id "11111111-1111-1111-1111-111111111111" --output my-blazor-app
```

Don't append a `2` to the application type argument (`blazorserver` in the example) if you're using the templates included in .NET SDK 5.0+. Include the `2` suffix *only* if you're on ASP.NET Core 3.1 and you installed the templates separately as described in the next section.

### ASP.NET Core 3.1 - Install the project templates

If you're using ASP.NET Core 3.1, install the project templates from NuGet.

```dotnetcli
dotnet new --install Microsoft.Identity.Web.ProjectTemplates
```

For ASP.NET Core 3.1 *only*, append a `2` to the application type argument when you create a new project:

```dotnetcli
dotnet new blazorserver2 --auth SingleOrg --calls-graph --client-id "00000000-0000-0000-0000-000000000000" --tenant-id "11111111-1111-1111-1111-111111111111" --output my-blazor-app
```

The following diagram shows several of the available app type templates and their arguments. Append a `2` to the app type argument (`blazorserver2` in the example) only if you're using the ASP.NET Core 3.1 SDK and you installed the templates by using `dotnet new --install`.

:::image type="content" source="media/microsoft-identity-web-overview/diagram-microsoft-identity-web-templates.png" lightbox="media/microsoft-identity-web-overview/diagram-microsoft-identity-web-templates.png" alt-text="Diagram of the available dot net CLI project templates for Microsoft Identity Web":::
<br /><sup><b>*</b></sup> `MultiOrg` is not supported with `webapi2`, but can be enabled in *appsettings.json* by setting tenant to `common` or `organizations`
<br /><sup><b>**</b></sup> `--calls-graph` is not supported for Azure AD B2C

## Features of the project templates

Microsoft Identity Web includes several features not available in the default ASP.NET Core 3.1 project templates.

| Feature                                                                                  | ASP.NET Core 3.1                                                     | Microsoft Identity Web                                                                                  |
|------------------------------------------------------------------------------------------|----------------------------------------------------------------------|---------------------------------------------------------------------------------------------------------|
| [Sign in users](scenario-web-app-sign-user-app-configuration.md) in web apps             | <li>Work or school accounts<li>Social identities (with Azure AD B2C) | <li>Work or school accounts<li>Personal Microsoft accounts<li>Social identities (with Azure AD B2C)     |
| [Protect web APIs](scenario-protected-web-api-app-configuration.md#microsoftidentityweb) | <li>Work or school accounts<li>Social identities (with Azure AD B2C) | <li>Work or school accounts<li>Personal Microsoft accounts<li>Social identities (with Azure AD B2C)     |
| Issuer validation in multi-tenant apps                                                   | No                                                                   | Yes, for [all clouds](authentication-national-cloud.md) and [Azure AD B2C](../../active-directory-b2c/index.yml) |
| Web app/API [calls Microsoft graph][scenario-api-call-graph]                             | No                                                                   | Yes                                                                                                     |
| Web app/API [calls web API][scenario-api-call-api]                                       | No                                                                   | Yes                                                                                                     |
| Supports certificate credentials                                                         | No                                                                   | Yes, including Azure Key Vault                                                                          |
| Incremental consent and conditional access support in web apps                           | No                                                                   | Yes, in MVC, Razor pages, and Blazor                                                                    |
| Token encryption certificates in web APIs                                                | No                                                                   | Yes                                                                                                     |
| [Scopes/app role validation][scenario-api-validation] in web APIs                        | No                                                                   | Yes                                                                                                     |
| `WWW-Authenticate` header generation in web APIs                                         | No                                                                   | Yes                                                                                                     |

## Next steps

To see Microsoft Identity Web in action, try our Blazor Server tutorial:

[Tutorial: Create a Blazor Server app that uses the Microsoft identity platform for authentication](tutorial-blazor-server.md)

The Microsoft Identity Web wiki on GitHub contains extensive reference documentation for various aspects of the library. For example, certificate usage, incremental consent, and conditional access reference can be found here:

- <a href="https://github.com/AzureAD/microsoft-identity-web/wiki/Using-certificates" target="_blank">Using certificates with Microsoft.Identity.Web</a> (GitHub)
- <a href="https://github.com/AzureAD/microsoft-identity-web/wiki/Managing-incremental-consent-and-conditional-access" target="_blank">Incremental consent and conditional access</a> (GitHub)

<!-- LINKS -->
<!--  [miw-certs]: microsoft-identity-web-certificates.md  -->
<!--  [miw-certs-decrypt]: microsoft-identity-web-certificates.md#decryption-certificates  -->
<!--  [miw-inc-consent-ca-header]: microsoft-identity-web-consent-conditional-access.md#handling-incremental-consent-or-conditional-access-in-web-apis  -->
<!--  [miw-inc-consent-ca]: microsoft-identity-web-consent-conditional-access.md  -->
[scenario-api-call-api]: scenario-web-api-call-api-call-api.md#option-1-call-microsoft-graph-with-the-sdk  
[scenario-api-call-graph]: scenario-web-api-call-api-call-api.md#option-1-call-microsoft-graph-with-the-sdk  
[scenario-api-validation]: scenario-protected-web-api-verification-scope-app-roles.md  
