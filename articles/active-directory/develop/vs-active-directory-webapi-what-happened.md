---
title: "Změny provedené na projekt WebApi při připojení k Azure AD | Microsoft Docs"
description: "Popisuje, co se stane do projektu WebApi při připojení k Azure AD pomocí sady Visual Studio"
services: active-directory
documentationcenter: 
author: kraigb
manager: ghogen
editor: 
ms.assetid: 57630aee-26a2-4326-9dbb-ea2a66daa8b0
ms.service: active-directory
ms.workload: web
ms.tgt_pltfrm: vs-what-happened
ms.devlang: na
ms.topic: article
ms.date: 03/01/2017
ms.author: kraigb
ms.custom: aaddev
ms.openlocfilehash: 086e5a9622cad681cd282345d97e0c28ee7de2fa
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/11/2017
---
# <a name="what-happened-to-my-webapi-project-visual-studio-azure-active-directory-connected-service"></a>Co se stalo s WebApi projektu (Visual Studio Azure Active Directory připojení služby)
> [!div class="op_single_selector"]
> * [Začínáme](vs-active-directory-webapi-getting-started.md)
> * [Co se přihodilo](vs-active-directory-webapi-what-happened.md)
> 
> 

## <a name="references-have-been-added"></a>Byly přidány odkazy
### <a name="nuget-package-references"></a>Odkazy na balíček NuGet
* `Microsoft.Owin`
* `Microsoft.Owin.Host.SystemWeb`
* `Microsoft.Owin.Security`
* `Microsoft.Owin.Security.ActiveDirectory`
* `Microsoft.Owin.Security.Jwt`
* `Microsoft.Owin.Security.OAuth`
* `Owin`
* `System.IdentityModel.Tokens.Jwt`

### <a name="net-references"></a>Odkazy na rozhraní .NET
* `Microsoft.Owin`
* `Microsoft.Owin.Host.SystemWeb`
* `Microsoft.Owin.Security`
* `Microsoft.Owin.Security.ActiveDirectory`
* `Microsoft.Owin.Security.Jwt`
* `Microsoft.Owin.Security.OAuth`
* `Owin`
* `System.IdentityModel.Tokens.Jwt`

## <a name="code-changes"></a>Změny kódu
### <a name="code-files-were-added-to-your-project"></a>Soubory kódu byly přidány do projektu
Spuštění třídu ověřování **App_Start/Startup.Auth.cs** byl přidán do projektu obsahující logika spuštění pro ověřování Azure AD.

### <a name="startup-code-was-added-to-your-project"></a>Kód spuštění byl přidán do projektu
Pokud jste již měli třída při spuštění ve vašem projektu **konfigurace** metoda byla aktualizována zahrnout volání `ConfigureAuth(app)`. Třída při spuštění, jinak hodnota byl přidán do projektu.

### <a name="your-appconfig-or-webconfig-file-has-new-configuration-values"></a>Váš soubor app.config nebo web.config obsahuje nové hodnoty konfigurace.
Byly přidány následující položky konfigurace.

```
    <appSettings>
            <add key="ida:ClientId" value="ClientId from the new Azure AD App" />
            <add key="ida:Tenant" value="Your selected Azure AD Tenant" />
            <add key="ida:Audience" value="The App ID Uri from the wizard" />
    </appSettings>`
```

### <a name="an-azure-ad-app-was-created"></a>Byla vytvořena aplikace Azure AD
Aplikaci Azure AD byl vytvořen v adresáři, který jste vybrali v průvodci.

[Další informace o službě Azure Active Directory](https://azure.microsoft.com/services/active-directory/)

## <a name="if-i-checked-disable-individual-user-accounts-authentication-what-additional-changes-were-made-to-my-project"></a>Pokud po kontrole *zakázat jednotlivé uživatelské účty ověřování*, jaké další změny byly provedeny na Můj projekt?
Byly odebrány odkazy balíčku NuGet, a soubory byly odebrány a zálohovat. V závislosti na stavu projektu možná budete muset ručně odebrat další odkazy nebo soubory, nebo upravit kód podle potřeby.

### <a name="nuget-package-references-removed-for-those-present"></a>Odkazy na balíček NuGet odebrat (pro ty existuje)
* `Microsoft.AspNet.Identity.Core`
* `Microsoft.AspNet.Identity.EntityFramework`
* `Microsoft.AspNet.Identity.Owin`

### <a name="code-files-backed-up-and-removed-for-those-present"></a>Soubory kódu zálohovat a odebrat (pro ty existuje)
Každý z následujících souborů byl zálohován a odebrat z projektu. Záložní soubory jsou umístěny ve složce 'Zálohování' v kořenovém adresáři projektu.

* `App_Start\IdentityConfig.cs`
* `Controllers\AccountController.cs`
* `Controllers\ManageController.cs`
* `Models\IdentityModels.cs`
* `Providers\ApplicationOAuthProvider.cs`

### <a name="code-files-backed-up-for-those-present"></a>Soubory kódu zálohovat (pro ty existuje)
Každý z následujících souborů byla zálohována před nahrazují. Záložní soubory jsou umístěny ve složce 'Zálohování' v kořenovém adresáři projektu.

* `Startup.cs`
* `App_Start\Startup.Auth.cs`

## <a name="if-i-checked-read-directory-data-what-additional-changes-were-made-to-my-project"></a>Pokud po kontrole *čtení dat adresáře*, jaké další změny byly provedeny na Můj projekt?
### <a name="additional-changes-were-made-to-your-appconfig-or-webconfig"></a>Byly provedeny další změny app.config nebo web.config
Byly přidány následující položky další konfiguraci.

```
    <appSettings>
        <add key="ida:Password" value="Your Azure AD App's new password" />
    </appSettings>`
```

### <a name="your-azure-active-directory-app-was-updated"></a>Aplikace Azure Active Directory byla aktualizována.
Aplikace Azure Active Directory byla aktualizována zahrnout *čtení dat adresáře* oprávnění a další klíč byl vytvořen, které se pak použije jako *ida: heslo* v `web.config` souboru.

## <a name="next-steps"></a>Další kroky
- [Další informace o službě Azure Active Directory](https://azure.microsoft.com/services/active-directory/)

