---
title: "Ověřování na základě tokenu (HTTP/2) pro služby APN v Azure Notification Hubs | Microsoft Docs"
description: "Toto téma vysvětluje, jak můžete využít nové tokenu ověřování pro služby APN"
services: notification-hubs
documentationcenter: .net
author: kpiteira
manager: erikre
editor: 
ms.service: notification-hubs
ms.workload: mobile
ms.tgt_pltfrm: mobile-multiple
ms.devlang: dotnet
ms.topic: article
ms.date: 05/17/2017
ms.author: kapiteir
ms.openlocfilehash: 5a21bcd9f12fc3f96b17a556ba15526c35ababe2
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="token-based-http2-authentication-for-apns"></a>Ověřování na základě tokenu (HTTP/2) pro služby APN
## <a name="overview"></a>Přehled
Tento článek podrobně popisují pomocí tokenu ověřování založené na nový protokol APNS HTTP/2.

Klíčové výhody použití nového protokolu patří:
-   Generování tokenů je poměrně starosti, (ve srovnání s certifikáty)
-   Žádná další data vypršení – jsou v ovládacím prvku ověřovací tokeny a jejich odvolání
-   Datové části teď může být až 4 KB
- Synchronní zpětné vazby
-   Jste nejnovější protokol společnosti Apple – certifikátů dál používat binární protokol, který je označen pro vyřazení

Pomocí tohoto nového mechanismu lze provést ve dvou krocích za pár minut:
1.  Získat informace potřebné z portálu Apple Developer účtu
2.  Konfigurace centra oznámení s novými informacemi

Centra oznámení je nyní všechny nastaven na nový systém ověřování pomocí služby APN. 

Pamatujte, že pokud jste migrovali ze pomocí přihlašovacích údajů certifikát služby APN:
- token vlastnosti přepsat svůj certifikát v našem systému
- ale aplikace i nadále bezproblémově přijímat oznámení.

## <a name="obtaining-authentication-information-from-apple"></a>Získávání informací o ověření od společnosti Apple
Pokud chcete povolit ověřování na základě tokenu, potřebujete následující vlastnosti z vašeho účtu Apple Developer:
### <a name="key-identifier"></a>Identifikátor klíče
Identifikátor klíče můžete získat na stránce "Klíče" ve vašem účtu vývojáře Apple

![](./media/notification-hubs-push-notification-http2-token-authentification/obtaining-auth-information-from-apple.png)

### <a name="application-identifier--application-name"></a>Identifikátor aplikace & název aplikace
Název aplikace je k dispozici prostřednictvím stránky vývojářský účet na ID aplikace. 
![](./media/notification-hubs-push-notification-http2-token-authentification/app-name.png)

Identifikátor aplikace je k dispozici prostřednictvím stránce s podrobnostmi o členství v vývojářský účet.
![](./media/notification-hubs-push-notification-http2-token-authentification/app-id.png)


### <a name="authentication-token"></a>Ověřovací token
Ověřovací token si můžete stáhnout po vygenerování tokenu pro vaši aplikaci. Podrobnosti o tom, jak generovat tento token [dokumentaci pro vývojáře Apple](http://help.apple.com/xcode/mac/current/#/dev11b059073?sub=dev1eb5dfe65).

## <a name="configuring-your-notification-hub-to-use-token-based-authentication"></a>Konfigurace centra oznámení pro použití ověřování na základě tokenu
### <a name="configure-via-the-azure-portal"></a>Konfigurování prostřednictvím portálu Azure
Pokud chcete povolit tokenu ověřování založené na portálu, přihlaste se k portálu Azure a přejděte do vašeho centra oznámení > oznámení služby > panely služby APN. 

Existuje nový vlastnost – *režim ověřování*. Výběr Token umožňuje aktualizovat všechny relevantní tokenu vlastnosti vašeho centra.

![](./media/notification-hubs-push-notification-http2-token-authentification/azure-portal-apns-settings.png)

- Zadejte vlastnosti, které jste získali z vývojářského účtu Apple 
- Zvolte režim vaší aplikace (produkční nebo izolovaného prostoru) 
- Kliknutím na Uložit aktualizovat přihlašovací údaje APNS. 

### <a name="configure-via-management-api-rest"></a>Nakonfigurovat přes rozhraní API (REST) pro správu

Můžete použít naše [rozhraní API pro správu](https://msdn.microsoft.com/library/azure/dn495827.aspx) k aktualizaci centra oznámení pro použití ověřování na základě tokenu.
V závislosti na tom, zda je aplikace, které konfigurujete izolovaného prostoru nebo produkční aplikace (zadané v účtu vývojáře Apple) použijte jednu z odpovídající koncových bodů:

- Koncový bod izolovaného prostoru: [https://api.development.push.apple.com:443, 3 nebo zařízení](https://api.development.push.apple.com:443/3/device)
- Koncový bod produkční: [https://api.push.apple.com:443, 3 nebo zařízení](https://api.push.apple.com:443/3/device)

> [!IMPORTANT]
> Tokeny ověřování vyžaduje verzi rozhraní API z: **2017-04 nebo novější**.
> 
> 

Tady je příklad požadavek PUT aktualizovat rozbočovač s ověřováním na základě tokenu:


        PUT https://{namespace}.servicebus.windows.net/{Notification Hub}?api-version=2017-04
          "Properties": {
            "ApnsCredential": {
              "Properties": {
                "KeyId": "<Your Key Id>",
                "Token": "<Your Authentication Token>",
                "AppName": "<Your Application Name>",
                "AppId": "<Your Application Id>",
                "Endpoint":"<Sandbox/Production Endpoint>"
              }
            }
          }
        

### <a name="configure-via-the-net-sdk"></a>Konfigurovat pomocí sady .NET SDK
Můžete nakonfigurovat vaše Centrum používat pomocí tokenu ověřování založené na našem [nejnovější verzi klienta SDK](https://www.nuget.org/packages/Microsoft.Azure.NotificationHubs/1.0.8). 

Zde je ukázka kódu ilustrující správné použití:


        NamespaceManager nm = NamespaceManager.CreateFromConnectionString(_endpoint);
        string token = "YOUR TOKEN HERE";
        string keyId = "YOUR KEY ID HERE";
        string appName = "YOUR APP NAME HERE";
        string appId = "YOUR APP ID HERE";
        NotificationHubDescription desc = new NotificationHubDescription("PATH TO YOUR HUB");
        desc.ApnsCredential = new ApnsCredential(token, keyId, appId, appName);
        desc.ApnsCredential.Endpoint = @"https://api.development.push.apple.com:443/3/device";
        nm.UpdateNotificationHubAsync(desc);

## <a name="reverting-to-using-certificate-based-authentication"></a>Návrat k použití ověřování na základě certifikátu
Kdykoli můžete vrátit zpět k používání ověřování pomocí certifikátů pomocí jakékoli předchozí metody a předání certifikátu, místo token vlastností. Tato akce přepíše dříve uložené přihlašovací údaje.
