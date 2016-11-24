---
title: "Úvod do Firewallu webových aplikací (WAF) služby Application Gateway | Dokumentace Microsoftu"
description: "Na této stránce najdete přehled firewallu webových aplikací (WAF) služby Application Gateway. "
documentationcenter: na
services: application-gateway
author: amsriva
manager: rossort
editor: amsriva
ms.assetid: 04b362bc-6653-4765-86f6-55ee8ec2a0ff
ms.service: application-gateway
ms.devlang: na
ms.topic: hero-article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 11/10/2016
ms.author: amsriva
translationtype: Human Translation
ms.sourcegitcommit: 486ed096bba7accfb3ca54aa97fb0b57b756d291
ms.openlocfilehash: b44be47e49e0d0a49c2fb7c30368f4245c01dedd


---
# <a name="application-gateway-web-application-firewall-preview"></a>Firewall webových aplikací služby Application Gateway (Preview)
Firewall webových aplikací (WAF) je funkce služby Azure Application Gateway, která poskytuje ochranu webovým aplikacím využívajícím aplikační bránu pro standardní funkce ADC (Application Delivery Controller). Firewall webových aplikací chrání webové aplikace před většinou z 10 nejčastějších webových hrozeb podle OWASP. Webové aplikace se čím dál častěji stávají cílem škodlivých útoků, které zneužívají běžně známé chyby zabezpečení. Mezi tyto běžné typy zneužití patří mimo jiné například útoky prostřednictvím injektáže SQL nebo skriptování mezi weby. Předcházet takovým útokům v kódu aplikace může být náročné a může vyžadovat pečlivou údržbu, opravy a monitorování několika vrstev topologie aplikace. Centralizovaný firewall webových aplikací, který chrání před webovými útoky, značně zjednodušuje správu zabezpečení a nabízí lepší ochranu aplikací před hrozbami neoprávněného vniknutí. Řešení Firewall webových aplikací (WAF) může také rychleji reagovat na ohrožení zabezpečení, protože opravuje známé chyby zabezpečení v centrálním umístění, namísto zabezpečování jednotlivých webových aplikací. Firewall webových aplikací umožňuje jednoduše převést stávající aplikační brány do služby Application Gateway.

![imageURLroute](./media/application-gateway-webapplicationfirewall-overview/WAF1.png)

Služba Application Gateway funguje jako ovladač doručování aplikací a nabízí ukončení protokolu SSL, spřažení relace na základě souborů cookie, distribuci zatížení pomocí kruhového dotazování, směrování na základě obsahu, schopnost hostovat více webů a vylepšení zabezpečení. Vylepšení zabezpečení nabízená službou Application Gateway zahrnují správu zásad protokolu SSL a podporu koncového šifrování protokolu SSL. Možnosti zabezpečení aplikací v naší službě posilujeme představením Firewallu webových aplikací (WAF) přímo integrovaného do nabídky ADC. Získáte tak snadno konfigurovatelné centrální umístění pro správu a ochranu svých webových aplikací před běžnými webovými chybami zabezpečení.

Konfigurace WAF ve službě Application Gateway poskytuje následující výhody:

* Ochráníte své webové aplikace před webovými chybami zabezpečení a útoky bez nutnosti upravovat back-endový kód.
* Ochráníte více webových aplikací současně za službou Application Gateway. Služba Application Gateway podporuje za jednou bránou hostování až 20 webů, které můžete ochránit před webovými útoky.
* Můžete monitorovat útoky na své webové aplikace pomocí sestavy vygenerované v reálném čase z protokolů WAF služby Application Gateway.
* Některé kontroly dodržování předpisů vyžadují, aby všechny internetové koncové body byly chráněné řešením WAF. Používáním služby Application Gateway s povoleným WAF můžete splnit tyto požadavky dodržování předpisů.

## <a name="overview"></a>Přehled
Firewall webových aplikací služby Application Gateway je nabízen v nové skladové jednotce (SKU WAF) a je předem nakonfigurovaný se sadou nástrojů ModSecurity a hlavní sadou pravidel OWASP. Díky tomu nabízí základní ochranu před většinou z 10 nejčastějších webových chyb zabezpečení podle OWASP.

* Ochrana před útoky prostřednictvím injektáže SQL.
* Ochrana před skriptováním mezi weby.
* Ochrana před běžnými webovými útoky, jako je například injektáž příkazů, pronášení požadavků HTTP, rozdělování odpovědí protokolu HTTP a útok pomocí vložení vzdáleného souboru.
* Ochrana před narušením protokolu HTTP.
* Ochrana před anomáliemi protokolu HTTP, jako například chybějící údaj user-agent hostitele nebo hlavičky Accept.
* Ochrana před útoky DoS přes protokol HTTP, včetně prevence zaplavení protokolu HTTP a pomalého útoku DoS přes protokol HTTP.
* Ochrana před roboty, prohledávacími moduly a skenery.
* Detekce běžných chyb v konfiguraci aplikací (tj. Apache, IIS atd.).

## <a name="waf-modes"></a>Režimy WAF
Firewall webových aplikací (WAF) služby Application Gateway lze nakonfigurovat ke spuštění v těchto dvou režimech:

* **Režim detekce** – Když je nakonfigurován ke spuštění v režimu detekce, WAF služby Application Gateway monitoruje a protokoluje všechny výstrahy na hrozby do souboru protokolu. Je nutné se ujistit, že je zapnutá diagnostika protokolování pro službu Application Gateway, a to v části Diagnostika. Bude také třeba zajistit, aby byl vybrán a zapnut protokol WAF.
* **Režim prevence** – Když je nakonfigurován ke spuštění v režimu prevence, služba Application Gateway aktivně blokuje vniknutí a útoky detekované pomocí svých pravidel. Útočník obdrží výjimku 403 – Neoprávněný přístup a připojení se ukončí. Režim prevence takové útoky nadále protokoluje do protokolů WAF.

## <a name="application-gateway-waf-reports"></a>Sestavy WAF služby Application Gateway
Firewall webových aplikací (WAF) služby Application Gateway poskytuje podrobné generování sestav o každém útoky, který detekuje. Protokolování je integrované v Protokolech diagnostiky Azure a výstrahy se zaznamenávají ve formátu JSON.

![imageURLroute](./media/application-gateway-webapplicationfirewall-overview/waf2.png)

```json
{
    "resourceId": "/SUBSCRIPTIONS/<subscriptionId>/RESOURCEGROUPS/<resourceGroupName>/PROVIDERS/MICROSOFT.NETWORK/APPLICATIONGATEWAYS/<applicationGatewayName>",
    "operationName": "ApplicationGatewayFirewall",
    "time": "2016-09-20T00:40:04.9138513Z",
    "category": "ApplicationGatewayFirewallLog",
    "properties":     {
        "instanceId":"ApplicationGatewayRole_IN_0",
        "clientIp":"108.41.16.164",
        "clientPort":1815,
        "requestUri":"/wavsep/active/RXSS-Detection-Evaluation-POST/",
        "ruleId":"OWASP_973336",
        "message":"XSS Filter - Category 1: Script Tag Vector",
        "action":"Logged",
        "site":"Global",
        "message":"XSS Filter - Category 1: Script Tag Vector",
        "details":{"message":" Warning. Pattern match "(?i)(<script","file":"/owasp_crs/base_rules/modsecurity_crs_41_xss_attacks.conf","line":"14"}}
}
```

## <a name="application-gateway-waf-sku-pricing"></a>Ceny SKU WAF služby Application Gateway
Během období Preview se na používání WAF služby Application Gateway nevztahují žádné další poplatky. Nadále se vám budou účtovat stávající poplatky za základní SKU. Poplatky za SKU WAF oznámíme v období všeobecné dostupnosti. Zákazníkům, kteří se rozhodli nasadit službu Application Gateway v SKU WAF, začnou poplatky za SKU WAF nabíhat až po oznámení veřejné dostupnosti.

## <a name="next-steps"></a>Další kroky
Po získání informací o možnostech WAF navštivte téma [Postup konfigurace Firewallu webových aplikací ve službě Application Gateway](application-gateway-web-application-firewall-portal.md).




<!--HONumber=Nov16_HO2-->


