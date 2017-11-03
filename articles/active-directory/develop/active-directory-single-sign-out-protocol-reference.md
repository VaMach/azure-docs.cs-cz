---
title: "Azure jednotné přihlašování SAML protokol | Microsoft Docs"
description: "Tento článek popisuje jeden protokol SAML Sign-Out v Azure Active Directory"
services: active-directory
documentationcenter: .net
author: priyamohanram
manager: mbaldwin
editor: 
ms.assetid: 0e4aa75d-d1ad-4bde-a94c-d8a41fb0abe6
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/19/2017
ms.author: priyamo
ms.custom: aaddev
ms.openlocfilehash: 45e4705f53d80b5fe852c484b5e64d18a8e24f09
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/11/2017
---
# Protokol jeden odhlašování SAML
Azure Active Directory (Azure AD) podporuje SAML 2.0 webové prohlížeče jediného odhlašování profilu. Pro jeden odhlašování fungovala správně **LogoutURL** pro aplikace, musí být explicitně zaregistrované v Azure AD při registraci aplikace. Azure AD se používá LogoutURL přesměrovat uživatele, jakmile se odhlásili.

Tento diagram zobrazuje pracovním procesu jeden odhlašování Azure AD.

![Jednotné přihlašování se pracovní postup](media/active-directory-single-sign-out-protocol-reference/active-directory-saml-single-sign-out-workflow.png)

## LogoutRequest
Odešle služba cloudu `LogoutRequest` zprávy do služby Azure AD, která označuje, že relace byla ukončena. Následující výpis zobrazí ukázku `LogoutRequest` elementu.

```
<samlp:LogoutRequest xmlns="urn:oasis:names:tc:SAML:2.0:metadata" ID="idaa6ebe6839094fe4abc4ebd5281ec780" Version="2.0" IssueInstant="2013-03-28T07:10:49.6004822Z" xmlns:samlp="urn:oasis:names:tc:SAML:2.0:protocol">
  <Issuer xmlns="urn:oasis:names:tc:SAML:2.0:assertion">https://www.workaad.com</Issuer>
  <NameID xmlns="urn:oasis:names:tc:SAML:2.0:assertion"> Uz2Pqz1X7pxe4XLWxV9KJQ+n59d573SepSAkuYKSde8=</NameID>
</samlp:LogoutRequest>
```

### LogoutRequest
`LogoutRequest` Prvek odeslaných do služby Azure AD vyžaduje následující atributy:

* `ID`: Toto identifikuje odhlašování žádosti. Hodnota `ID` nesmí začínat číslem. Typické postupem je připojit **id** k řetězcovou reprezentaci identifikátor GUID.
* `Version`: Nastavte hodnotu pro tento element **2.0**. Tato hodnota se vyžaduje.
* `IssueInstant`: Toto je `DateTime` řetězec s hodnotou koordinaci světový čas (UTC) a [odezvy formátu ("o")](https://msdn.microsoft.com/library/az4se3k1.aspx). Azure AD očekává hodnotu typu, ale nedokáže vynutit.

### Vystavitel
`Issuer` Element v `LogoutRequest` musí přesně shodovat s jedním z **ServicePrincipalNames** v rámci cloudové služby ve službě Azure AD. Je standardně nastavena **identifikátor ID URI aplikace** , který je určen při registraci aplikace.

### NameID
Hodnota `NameID` element musí přesně shodovat `NameID` uživatele, který je právě odhlášení.

## LogoutResponse
Odešle Azure AD `LogoutResponse` v reakci `LogoutRequest` element. Následující výpis zobrazí ukázku `LogoutResponse`.

```
<samlp:LogoutResponse ID="_f0961a83-d071-4be5-a18c-9ae7b22987a4" Version="2.0" IssueInstant="2013-03-18T08:49:24.405Z" InResponseTo="iddce91f96e56747b5ace6d2e2aa9d4f8c" xmlns:samlp="urn:oasis:names:tc:SAML:2.0:protocol">
  <Issuer xmlns="urn:oasis:names:tc:SAML:2.0:assertion">https://sts.windows.net/82869000-6ad1-48f0-8171-272ed18796e9/</Issuer>
  <samlp:Status>
    <samlp:StatusCode Value="urn:oasis:names:tc:SAML:2.0:status:Success" />
  </samlp:Status>
</samlp:LogoutResponse>
```

### LogoutResponse
Azure AD Nastaví `ID`, `Version` a `IssueInstant` hodnoty ve `LogoutResponse` elementu. Nastaví taky `InResponseTo` element na hodnotu `ID` atribut `LogoutRequest` který vyvolaná odpovědi.

### Vystavitel
Azure AD nastavuje tuto hodnotu `https://login.microsoftonline.com/<TenantIdGUID>/` kde <TenantIdGUID> je klientské ID klienta Azure AD.

Vyhodnotit hodnotu `Issuer` elementu, použijte hodnotu **identifikátor ID URI aplikace** zadané při registraci aplikace.

### Status
Používá Azure AD `StatusCode` element v `Status` element indikující úspěch nebo selhání odhlášení. Při odhlašování pokus selže, `StatusCode` element může také obsahovat vlastní chybové zprávy.