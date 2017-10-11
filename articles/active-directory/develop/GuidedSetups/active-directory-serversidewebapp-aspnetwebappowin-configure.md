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
ms.openlocfilehash: 0c627802ccfba230dcde2dafffee26cb1c895791
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
## <a name="create-an-application-express"></a>Vytvoření aplikace (Express)
Nyní je nutné zaregistrovat aplikaci v *portálu pro registraci aplikace Microsoft*:
1. Registrace vaší aplikace pomocí [portálu pro registraci aplikace Microsoft](https://apps.dev.microsoft.com/portal/register-app?appType=serverSideWebApp&appTech=aspNetWebAppOwin&step=configure)
2.  Zadejte název vaší aplikace a e-mailu
3.  Ujistěte se, že je zaškrtnuté políčko pro instalaci na základě
4.  Postupujte podle pokynů k přidání do aplikace adresy URL pro přesměrování

## <a name="add-your-application-registration-information-to-your-solution-advanced"></a>Přidat informace o registraci aplikace k řešení (Upřesnit)
Nyní je nutné zaregistrovat aplikaci v *portálu pro registraci aplikace Microsoft*:
1. Přejděte na [portálu pro registraci aplikace Microsoft](https://apps.dev.microsoft.com/portal/register-app) zaregistrovat aplikaci
2. Zadejte název vaší aplikace a e-mailu 
3.  Ujistěte se, že není zaškrtnuto políčko pro instalaci na základě
4.  Klikněte na tlačítko `Add Platform`, zvolte položku`Web`
5.  Přejděte zpět do Visual Studio a v Průzkumníku řešení vyberte projekt a podívejte se na okno vlastností (Pokud se nezobrazí okno Vlastnosti stisknutím klávesy F4)
6.  Změna SSL povoleno`True`
7.  Zkopírujte adresu URL protokolu SSL a přidejte tuto adresu URL do seznamu adres URL pro přesměrování v portálu pro registraci seznam adres URL pro přesměrování:<br/><br/>![Vlastnosti projektu](media/active-directory-serversidewebapp-aspnetwebappowin-configure/vsprojectproperties.png)<br />
8.  Přidejte následující v `web.config` umístěné v kořenové složce části `configuration\appSettings`:

```xml
<add key="ClientId" value="Enter_the_Application_Id_here" />
<add key="redirectUri" value="Enter_the_Redirect_URL_here" />
<add key="Tenant" value="common" />
<add key="Authority" value="https://login.microsoftonline.com/{0}/v2.0" /> 
```
<!-- Workaround for Docs conversion bug -->
<ol start="9">
<li>
Nahraďte `ClientId` s Id aplikace, který jste právě zaregistrovali
</li>
<li>
Nahraďte `redirectUri` pomocí adresy URL protokolu SSL vašeho projektu
</li>
</ol>
<!-- End Docs -->
