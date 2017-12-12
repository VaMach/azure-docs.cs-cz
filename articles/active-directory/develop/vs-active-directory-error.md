---
title: "Diagnostika chyb pomocí Průvodce Azure Active Directory připojení"
description: "Průvodce připojením k službě active directory zjistila typu nekompatibilní ověřování"
services: active-directory
documentationcenter: 
author: kraigb
manager: mtillman
editor: 
ms.assetid: dd89ea63-4e45-4da1-9642-645b9309670a
ms.service: active-directory
ms.workload: web
ms.tgt_pltfrm: vs-getting-started
ms.devlang: na
ms.topic: article
ms.date: 03/05/2017
ms.author: kraigb
ms.custom: aaddev
ms.openlocfilehash: 186bb1ede11c869b1352906b7ebafe57025f4f09
ms.sourcegitcommit: e266df9f97d04acfc4a843770fadfd8edf4fa2b7
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 12/11/2017
---
# <a name="diagnosing-errors-with-the-azure-active-directory-connection-wizard"></a>Diagnostikování chyb pomocí Průvodce Azure Active Directory připojení
Při zjišťování předchozí kód ověřování, Průvodce zjistil typu nekompatibilní ověřování.   

## <a name="what-is-being-checked"></a>Co se kontroluje?
**Poznámka:** správně zjistit předchozí ověřovacího kódu v projektu, musí být vytvořená projektu.  Pokud došlo k této chybě a nemáte předchozí ověřovacího kódu v projektu, znovu sestavte a zkuste to znovu.

### <a name="project-types"></a>Typy projektů
Průvodce zkontroluje typu projektu, které vyvíjíte, takže ho můžete vložit správné ověřování do projektu.  Pokud je každý kontroler, který je odvozen od `ApiController` v projektu projekt se považuje za WebAPI projektu.  Pokud jsou pouze řadiče, které jsou odvozeny od `MVC.Controller` v projektu projekt se považuje za projektu MVC.  Cokoliv jiného Průvodce nepodporuje.

### <a name="compatible-authentication-code"></a>Kompatibilní ověřovací kód
Průvodce také zkontroluje nastavení ověřování, které byly dříve nakonfigurovány pomocí průvodce nebo jsou kompatibilní s průvodcem.  Pokud jsou v něm všechna nastavení, bude považován za vícenásobně případu a otevře se průvodce zobrazit nastavení.  Pokud jenom některá nastavení jsou v něm, bude považován za případ k chybě.

V projektu aplikace MVC Průvodce zkontroluje pro žádné z následujících nastavení, které jsou výsledkem předchozí pomocí průvodce:

    <add key="ida:ClientId" value="" />
    <add key="ida:Tenant" value="" />
    <add key="ida:AADInstance" value="" />
    <add key="ida:PostLogoutRedirectUri" value="" />

Kromě toho Průvodce zkontroluje pro žádné z následujících nastavení v projektu webového rozhraní API, které jsou výsledkem předchozí pomocí průvodce:

    <add key="ida:ClientId" value="" />
    <add key="ida:Tenant" value="" />
    <add key="ida:Audience" value="" />

### <a name="incompatible-authentication-code"></a>Nekompatibilní ověřovací kód
Nakonec průvodce se pokusí zjistit verzích ověřovací kód, které byly nakonfigurovány s předchozími verzemi sady Visual Studio. Pokud se tato chyba, znamená to, že váš projekt obsahuje typu nekompatibilní ověřování. Průvodce rozpozná následující typy ověřování z předchozích verzí sady Visual Studio:

* Ověřování systému Windows 
* Jednotlivých uživatelských účtů 
* Účty organizace 

Ke zjištění ověřování systému Windows v projektu aplikace MVC, Průvodce hledá `authentication` element z vaší **web.config** souboru.

<pre>
    &lt;configuration&gt;
        &lt;system.web&gt;
            <span style="background-color: yellow">&lt;authentication mode="Windows" /&gt;</span>
        &lt;/system.web&gt;
    &lt;/configuration&gt;
</pre>

Ke zjištění ověřování systému Windows v projektu webového rozhraní API, Průvodce hledá `IISExpressWindowsAuthentication` element ze svého projektu **.csproj** souboru:

<pre>
    &lt;Project&gt;
        &lt;PropertyGroup&gt;
            <span style="background-color: yellow">&lt;IISExpressWindowsAuthentication&gt;enabled&lt;/IISExpressWindowsAuthentication&gt;</span>
        &lt;/PropertyGroup>
    &lt;/Project&gt;
</pre>

Ke zjištění ověřování jednotlivých uživatelských účtů, Průvodce hledá element balíček z vaší **Packages.config** souboru.

<pre>
    &lt;packages&gt;
        <span style="background-color: yellow">&lt;package id="Microsoft.AspNet.Identity.EntityFramework" version="2.1.0" targetFramework="net45" /&gt;</span>
    &lt;/packages&gt;
</pre>

Ke zjištění staré formu ověřování účtu organizace, Průvodce hledá následující element z **web.config**:

<pre>
    &lt;configuration&gt;
        &lt;appSettings&gt;
            <span style="background-color: yellow">&lt;add key="ida:Realm" value="***" /&gt;</span>
        &lt;/appSettings&gt;
    &lt;/configuration&gt;
</pre>

Chcete-li změnit typ ověřování, odeberte typ nekompatibilní ověřování a opakujte akci.

Další informace najdete v tématu [scénáře ověřování pro Azure AD](active-directory-authentication-scenarios.md).

#<a name="next-steps"></a>Další kroky
- [Scénáře ověřování pro Azure AD](active-directory-authentication-scenarios.md)