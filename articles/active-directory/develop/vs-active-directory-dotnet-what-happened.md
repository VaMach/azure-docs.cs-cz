---
title: "Změny provedené na projekt MVC při připojení k Azure AD | Microsoft Docs"
description: "Popisuje, co se stane do projektu MVC při připojení k Azure AD pomocí sady Visual Studio připojené služby"
services: active-directory
documentationcenter: na
author: kraigb
manager: ghogen
editor: 
ms.assetid: 8b24adde-547e-4ffe-824a-2029ba210216
ms.service: active-directory
ms.workload: web
ms.tgt_pltfrm: vs-what-happened
ms.devlang: na
ms.topic: article
ms.date: 03/01/2017
ms.author: kraigb
ms.custom: aaddev
ms.openlocfilehash: 095411a7fc854f4dce11921adb0f57c5389a8e13
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="what-happened-to-my-mvc-project-visual-studio-azure-active-directory-connected-service"></a>Co se stalo s MVC projektu (Visual Studio Azure Active Directory připojeno service)?
> [!div class="op_single_selector"]
> * [Začínáme](vs-active-directory-dotnet-getting-started.md)
> * [Co se přihodilo](vs-active-directory-dotnet-what-happened.md)
> 
> 

## <a name="references-have-been-added"></a>Byly přidány odkazy
### <a name="nuget-package-references"></a>Odkazy na balíček NuGet
* **Microsoft.IdentityModel.Protocol.Extensions**
* **Microsoft.Owin**
* **Microsoft.Owin.Host.SystemWeb**
* **Microsoft.Owin.Security**
* **Microsoft.Owin.Security.Cookies**
* **Microsoft.Owin.Security.OpenIdConnect**
* **Owin**
* **System.IdentityModel.Tokens.Jwt**

### <a name="net-references"></a>Odkazy na rozhraní .NET
* **Microsoft.IdentityModel.Protocol.Extensions**
* **Microsoft.Owin**
* **Microsoft.Owin.Host.SystemWeb**
* **Microsoft.Owin.Security**
* **Microsoft.Owin.Security.Cookies**
* **Microsoft.Owin.Security.OpenIdConnect**
* **Owin**
* **System.IdentityModel**
* **System.IdentityModel.Tokens.Jwt**
* **System.Runtime.Serialization**

## <a name="code-has-been-added"></a>Byla přidána kódu
### <a name="code-files-were-added-to-your-project"></a>Soubory kódu byly přidány do projektu
Spuštění třídu ověřování **App_Start/Startup.Auth.cs** byl přidán do projektu obsahující logika spuštění pro ověřování Azure AD. Třída kontroleru, Controllers/AccountController.cs byla přidána navíc obsahující **SignIn()** a **SignOut()** metody. Nakonec částečné zobrazení, **Views/Shared/_LoginPartial.cshtml** byl přidán obsahující odkaz akce pro přihlášení/odhlášení.

### <a name="startup-code-was-added-to-your-project"></a>Kód spuštění byl přidán do projektu
Pokud jste již měli třída při spuštění ve vašem projektu **konfigurace** metoda byla aktualizována zahrnout volání **ConfigureAuth(app)**. Třída při spuštění, jinak hodnota byl přidán do projektu.

### <a name="your-appconfig-or-webconfig-has-new-configuration-values"></a>App.config nebo web.config obsahuje nové hodnoty konfigurace
Byly přidány následující položky konfigurace.

    <appSettings>
        <add key="ida:ClientId" value="ClientId from the new Azure AD App" />
        <add key="ida:AADInstance" value="https://login.microsoftonline.com/" />
        <add key="ida:Domain" value="The selected Azure AD Domain" />
        <add key="ida:TenantId" value="The Id of your selected Azure AD Tenant" />
        <add key="ida:PostLogoutRedirectUri" value="Your project start page" />
    </appSettings>

### <a name="an-azure-active-directory-ad-app-was-created"></a>Aplikace Azure Active Directory (AD) byla vytvořena.
Aplikaci Azure AD byl vytvořen v adresáři, který jste vybrali v průvodci.

## <a name="if-i-checked-disable-individual-user-accounts-authentication-what-additional-changes-were-made-to-my-project"></a>Pokud po kontrole *zakázat jednotlivé uživatelské účty ověřování*, jaké další změny byly provedeny na Můj projekt?
Byly odebrány odkazy balíčku NuGet, a soubory byly odebrány a zálohovat. V závislosti na stavu projektu možná budete muset ručně odebrat další odkazy nebo soubory, nebo upravit kód podle potřeby.

### <a name="nuget-package-references-removed-for-those-present"></a>Odkazy na balíček NuGet odebrat (pro ty existuje)
* **Microsoft.AspNet.Identity.Core**
* **Microsoft.AspNet.Identity.EntityFramework**
* **Microsoft.AspNet.Identity.Owin**

### <a name="code-files-backed-up-and-removed-for-those-present"></a>Soubory kódu zálohovat a odebrat (pro ty existuje)
Každý z následujících souborů byl zálohován a odebrat z projektu. Záložní soubory jsou umístěny ve složce 'Zálohování' v kořenovém adresáři projektu.

* **App_Start\IdentityConfig.cs**
* **Controllers\ManageController.cs**
* **Models\IdentityModels.cs**
* **Models\ManageViewModels.cs**

### <a name="code-files-backed-up-for-those-present"></a>Soubory kódu zálohovat (pro ty existuje)
Každý z následujících souborů byla zálohována před nahrazují. Záložní soubory jsou umístěny ve složce 'Zálohování' v kořenovém adresáři projektu.

* **Startup.cs**
* **App_Start\Startup.auth.cs**
* **Controllers\AccountController.cs**
* **Views\Shared\_LoginPartial.cshtml**

## <a name="if-i-checked-read-directory-data-what-additional-changes-were-made-to-my-project"></a>Pokud po kontrole *čtení dat adresáře*, jaké další změny byly provedeny na Můj projekt?
Byly přidány další odkazy.

### <a name="additional-nuget-package-references"></a>Další informace o balíčku NuGet
* **EntityFramework**
* **Microsoft.Azure.ActiveDirectory.GraphClient**
* **Microsoft.Data.Edm**
* **Microsoft.Data.OData**
* **Microsoft.Data.Services.Client**
* **Microsoft.IdentityModel.Clients.ActiveDirectory**
* **System.Spatial**

### <a name="additional-net-references"></a>Další odkazy na rozhraní .NET
* **EntityFramework**
* **EntityFramework.SqlServer**
* **Microsoft.Azure.ActiveDirectory.GraphClient**
* **Microsoft.Data.Edm**
* **Microsoft.Data.OData**
* **Microsoft.Data.Services.Client**
* **Microsoft.IdentityModel.Clients.ActiveDirectory**
* **Microsoft.IdentityModel.Clients.ActiveDirectory.WindowsForms**
* **System.Spatial**

### <a name="additional-code-files-were-added-to-your-project"></a>Další kód byly přidány do projektu
K podpoře ukládání tokenu do mezipaměti nebyly přidány dva soubory: **Models\ADALTokenCache.cs** a **Models\ApplicationDbContext.cs**.  Pro ilustraci informace o přístupu k profilu uživatele pomocí Azure graph API byly přidány dalšího řadiče a zobrazení.  Tyto soubory jsou **Controllers\UserProfileController.cs** a **Views\UserProfile\Index.cshtml**.

### <a name="additional-startup-code-was-added-to-your-project"></a>Další spouštěcí kód byl přidán do projektu
V **startup.auth.cs** souboru novou **OpenIdConnectAuthenticationNotifications** objekt byl přidán do **oznámení** členem  **OpenIdConnectAuthenticationOptions**.  Toto je umožnit přijetí kód OAuth a výměna ho pro přístupový token.

### <a name="additional-changes-were-made-to-your-appconfig-or-webconfig"></a>Byly provedeny další změny app.config nebo web.config
Byly přidány následující položky další konfiguraci.

    <appSettings>
        <add key="ida:ClientSecret" value="Your Azure AD App's new client secret" />
    </appSettings>

Byly přidány následující konfigurační oddíly a připojovací řetězec.

    <configSections>
        <!-- For more information on Entity Framework configuration, visit http://go.microsoft.com/fwlink/?LinkID=237468 -->
        <section name="entityFramework" type="System.Data.Entity.Internal.ConfigFile.EntityFrameworkSection, EntityFramework, Version=6.0.0.0, Culture=neutral, PublicKeyToken=b77a5c561934e089" requirePermission="false" />
    </configSections>
    <connectionStrings>
        <add name="DefaultConnection" connectionString="Data Source=(localdb)\MSSQLLocalDB;AttachDbFilename=|DataDirectory|\aspnet-[AppName + Generated Id].mdf;Initial Catalog=aspnet-[AppName + Generated Id];Integrated Security=True" providerName="System.Data.SqlClient" />
    </connectionStrings>
    <entityFramework>
        <defaultConnectionFactory type="System.Data.Entity.Infrastructure.LocalDbConnectionFactory, EntityFramework">
          <parameters>
            <parameter value="mssqllocaldb" />
          </parameters>
        </defaultConnectionFactory>
        <providers>
          <provider invariantName="System.Data.SqlClient" type="System.Data.Entity.SqlServer.SqlProviderServices, EntityFramework.SqlServer" />
        </providers>
    </entityFramework>


### <a name="your-azure-active-directory-app-was-updated"></a>Aplikace Azure Active Directory byla aktualizována.
Aplikace Azure Active Directory byla aktualizována zahrnout *čtení dat adresáře* oprávnění a další klíč byl vytvořen, které se pak použije jako *ida: ClientSecret* v  **soubor Web.config** souboru.

## <a name="next-steps"></a>Další kroky
- [Další informace o službě Azure Active Directory](https://azure.microsoft.com/services/active-directory/)

