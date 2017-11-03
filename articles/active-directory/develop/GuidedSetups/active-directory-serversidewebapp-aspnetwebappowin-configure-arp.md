---
title: "Azure AD v2 ASP.NET Web Server získávání spuštěno – konfigurace | Microsoft Docs"
description: "Implementace přihlašování společnosti Microsoft na řešení technologie ASP.NET s tradiční webovou aplikací využívajících prohlížeč pomocí OpenID Connect standard"
services: active-directory
documentationcenter: dev-center-name
author: andretms
manager: mbaldwin
editor: 
ms.assetid: 820acdb7-d316-4c3b-8de9-79df48ba3b06
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 05/09/2017
ms.author: andret
ms.custom: aaddev
ms.openlocfilehash: 8a1650a65e7980f4a13fa4edc7918b0099bb5464
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/11/2017
---
## <a name="configure-your-aspnet-web-app-with-the-applications-registration-information"></a>Konfigurace webové aplikace ASP.NET s informace o registraci aplikace

V tomto kroku nakonfigurujete svůj projekt na používání protokolu SSL a pak použijte adresu URL SSL konfigurovat informace o registraci vaší aplikace. Potom tuto aplikaci přidat, informace o registraci do řešení prostřednictvím *web.config*.

1.  V Průzkumníku řešení, vyberte projekt a podívejte se na `Properties` okno (Pokud se nezobrazí okno Vlastnosti stisknutím klávesy F4)
2.  Změna `SSL Enabled` na`True`
3.  Zkopírujte hodnotu z `SSL URL` výše a vložte jej do `Redirect URL` pole nahoře na této stránce a pak klikněte na *aktualizace*:<br/><br/>![Vlastnosti projektu](media/active-directory-serversidewebapp-aspnetwebappowin-configure/vsprojectproperties.png)<br />
4.  Přidejte následující v `web.config` soubor umístěný na kořenové složky, části `configuration\appSettings`:

```xml
<add key="ClientId" value="[Enter the application Id here]" />
<add key="RedirectUri" value="[Enter the Redirect URL here]" />
<add key="Tenant" value="common" />
<add key="Authority" value="https://login.microsoftonline.com/{0}/v2.0" /> 
```
