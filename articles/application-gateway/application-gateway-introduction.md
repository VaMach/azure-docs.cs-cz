---
title: "Seznámení se službou Azure Application Gateway | Dokumentace Microsoftu"
description: "Tato stránka poskytuje přehled služby Application Gateway pro vyrovnávání zatížení vrstvy 7, včetně velikostí bran, vyrovnávání zatížení HTTP, spřažení relace na základě souborů cookie a přesměrování zpracování SSL."
documentationcenter: na
services: application-gateway
author: georgewallace
manager: timlt
editor: tysonn
ms.assetid: b37a2473-4f0e-496b-95e7-c0594e96f83e
ms.service: application-gateway
ms.devlang: na
ms.topic: hero-article
ms.tgt_pltfrm: na
ms.custom: H1Hack27Feb2017
ms.workload: infrastructure-services
ms.date: 07/10/2017
ms.author: gwallace
ms.translationtype: HT
ms.sourcegitcommit: bde1bc7e140f9eb7bb864c1c0a1387b9da5d4d22
ms.openlocfilehash: c9aafa13ebdeeb84a35e55e25650f334ddc71af5
ms.contentlocale: cs-cz
ms.lasthandoff: 07/21/2017

---
# <a name="overview-of-application-gateway"></a>Přehled služby Application Gateway

Microsoft Azure Application Gateway je vyhrazené virtuální zařízení poskytující kontroler doručování aplikací (ADC) jako službu, která vaší aplikaci nabízí různé možnosti vyrovnávání zatížení vrstvy 7. Umožňuje zákazníkům optimalizovat produktivitu webové farmy tím, že se ukončování protokolu SSL, které je náročné na CPU, přesměruje na aplikační bránu. Nabízí také další možnosti přesměrování vrstvy 7, jako je kruhové dotazování na distribuci příchozích přenosů, spřažení relací na základě souborů cookie, přesměrování založené na cestách URL a možnost hostování několika webů za jedinou službou Application Gateway. Jako součást skladové jednotky firewallu webových aplikací (WAF) aplikační brány je poskytován i firewall webových aplikací, který zajišťuje ochranu webových aplikací před běžným webovým ohrožením zabezpečení a zneužitím. Application Gateway je možné nakonfigurovat jako internetovou bránu nebo jen jako interní bránu, případně jako kombinaci obojího. 

![scénář](./media/application-gateway-introduction/scenario.png)

## <a name="features"></a>Funkce

Služba Application Gateway aktuálně poskytuje následující možnosti:


* **[Firewall webových aplikací](application-gateway-webapplicationfirewall-overview.md)** – Firewall webových aplikací (WAF) ve službě Azure Application Gateway chrání webové aplikace před běžnými webovými útoky, jako jsou například útoky prostřednictvím injektáže SQL, skriptování mezi weby a napadení relace.
* **Vyrovnávání zatížení HTTP** – Služba Application Gateway poskytuje vyrovnávání zatížení kruhovým dotazováním. Vyrovnávání zatížení probíhá na vrstvě 7 a slouží pouze pro přenosy pomocí protokolu HTTP nebo HTTPS.
* **Spřažení relace na základě souborů cookie** – Tato funkce je užitečná v případě, že chcete zachovat uživatelskou relaci na stejném back-endu. Pomocí souborů cookie spravovaných bránou je služba Application Gateway schopna směrovat následný provoz z uživatelské relace k zpracování do stejného back-endu. Tato funkce je důležitá v případech, kdy se stav jednotlivých uživatelských relací ukládá místně na back-endovém serveru.
* **[Přesměrování zpracování Secure Sockets Layer (SSL)](application-gateway-ssl-arm.md)** – Tato funkce zbaví vaše webové servery nákladné úlohy dešifrování přenosu pomocí protokolu HTTPS. Díky ukončení připojení protokolem SSL ve službě Application Gateway a předání požadavku na server v nezašifrované podobě není server zatížený dešifrováním.  Služba Application Gateway znovu šifruje odpověď před tím, než ji odešle zpět klientovi. Tato funkce je užitečná ve scénářích, kdy je back-end umístěn v rámci Azure ve stejné zabezpečené virtuální síti jako služba Application Gateway.
* **[Konečné šifrování protokolu SSL](application-gateway-backend-ssl.md)** – Služba Application Gateway podporuje koncové šifrování provozu. Služba Application Gateway to provádí ukončením připojení protokolem SSL ve službě Application Gateway. Brána následně použije na provoz pravidla směrování, znovu zašifruje paket a předá tento paket do příslušného back-endu na základě nadefinovaných pravidel směrování. Každá odpověď webového serveru prochází ke koncovému uživateli stejným procesem.
* **[Směrování obsahu na základě adresy URL](application-gateway-url-route-overview.md)** – Tato funkce poskytuje možnost používat různé back-endové servery pro různý provoz. Přenosy složky na webovém serveru nebo v síti CDN je možné směrovat na různé back-endy, aby se snížilo jejich nepotřebné zatížení, které neslouží konkrétnímu obsahu.
* **[Směrování více webů](application-gateway-multi-site-overview.md)** – Služba Application Gateway umožňuje konsolidovat až 20 webů v jedné službě Application Gateway.
* **[Podpora protokolu WebSocket](application-gateway-websocket.md)** – Další skvělou funkcí služby Application Gateway je nativní podpora protokolu WebSocket.
* **[Monitorování stavu](application-gateway-probe-overview.md)** – Služba Application Gateway poskytuje výchozí monitorování stavu back-endových prostředků a vlastní monitorovací sondy pro konkrétnější scénáře.
* **[Přesměrování požadavků](application-gateway-redirect-overview.md)** – Tato funkce poskytuje možnost přesměrovat požadavky HTTP do naslouchacího procesu HTTPS.
* **[Rozšířená diagnostika](application-gateway-diagnostics.md)** – Služba Application Gateway poskytuje úplnou diagnostiku a protokoly přístupů. Protokoly brány firewall jsou dostupné pro prostředky služby Application Gateway, které mají povolený Firewall webových aplikací.

## <a name="benefits"></a>Výhody

Služba Application Gateway je užitečná pro:

* Aplikace, které vyžadují, aby se požadavky ze stejné uživatelské/klientské relace pokaždé dostaly ke stejnému back-endovému virtuálnímu počítači. Příklady těchto aplikací by mohly být aplikace nákupních košíků a servery webové pošty.
* Odstranění režie při ukončování protokolu SSL pro farmy webových serverů.
* Aplikace, jako je například síť pro doručování obsahu, které vyžadují, aby bylo během jednoho dlouhotrvajícího připojení TCP více požadavků HTTP směrováno nebo směrováno s vyrovnáváním zatížení na různé back-endové servery.
* Aplikace, které podporují přenos pomocí protokolu WebSocket.
* Ochranu webových aplikací před běžnými webovými útoky, jako jsou například útoky prostřednictvím injektáže SQL, skriptování mezi weby a napadení relace.
* Logickou distribuci přenosu na základě různých kritérií směrování, jako je například cesta URL nebo hlavičky domény.

Službu Application Gateway je možné plně spravovat v Azure, je škálovatelná a vysoce dostupná. Nabízí celou řadu možností diagnostiky a protokolování, které zlepšují správu. Když vytvoříte aplikační bránu, bude příchozímu síťovému přenosu přidružen koncový bod (veřejná virtuální IP adresa nebo interní IP adresa interního nástroje na vyrovnávání zatížení), který se pro něj použije. Tuto virtuální IP adresu nebo IP adresu interního nástroje pro vyrovnávání zatížení poskytuje služba Azure Load Balancer, která funguje na úrovni přenosu (TCP/UDP) a vyrovnává zatížení v podobě veškerých příchozích síťových přenosů mezi instancemi pracovních procesů aplikační brány. Aplikační brána následně přesměruje přenosy HTTP/HTTPS podle vlastní konfigurace bez ohledu na to, jestli se jedná o virtuální počítač, cloudovou službu, interní nebo externí IP adresu.

Vyrovnávání zatížení ve službě Application Gateway jako služba spravovaná Azure umožňuje zřízení nástroje pro vyrovnávání zatížení vrstvy 7 za nástrojem pro vyrovnávání zatížení softwaru Azure. Traffic Manager se dá použít k dokončení scénáře, který je znázorněný na následujícím obrázku, kde Traffic Manager poskytuje přesměrování a dostupnost při provozu mezi několika prostředky služby Application Gateway v různých oblastech, zatímco služba Application Gateway poskytuje vyrovnávání zatížení vrstvy 7 mezi oblastmi. Příklad tohoto scénáře najdete v tématu: [Používání služeb vyrovnávání zatížení v cloudu Azure](../traffic-manager/traffic-manager-load-balancing-azure.md).

![scénář Traffic Manageru a služby Application Gateway](./media/application-gateway-introduction/tm-lb-ag-scenario.png)

[!INCLUDE [load-balancer-compare-tm-ag-lb-include.md](../../includes/load-balancer-compare-tm-ag-lb-include.md)]

## <a name="gateway-sizes-and-instances"></a>Velikosti a instance brány

Služba Application Gateway je v současné době nabízena ve třech velikostech: **Small** (krátkodobé používání), **Medium** (střednědobé používání) a **Large** (dlouhodobé používání). Instance krátkodobého používání jsou určené pro scénáře vývoje a testování.

V rámci jednoho předplatného můžete vytvořit až 50 služeb Application Gateway a každá z nich může mít až 10 instancí. Každá služba Application Gateway se může skládat z 20 naslouchacích procesů HTTP. Úplný seznam omezení služby Application Gateway najdete na stránce [Omezení služby Application Gateway](../azure-subscription-service-limits.md?toc=%2fazure%2fapplication-gateway%2ftoc.json#application-gateway-limits).

Následující tabulka ukazuje průměrnou propustnost výkonu pro jednotlivé instance služby Application Gateway s povoleným přesměrováním zpracování SSL:

| Odezva back-endové stránky | Krátkodobé používání | Střednědobé používání | Dlouhodobé používání |
| --- | --- | --- | --- |
| 6K |7,5 Mb/s |13 Mb/s |50 Mb/s |
| 100K |35 Mb/s |100 Mb/s |200 Mb/s |

> [!NOTE]
> Tyto hodnoty jsou přibližné hodnoty propustnosti služby Application Gateway. Skutečná propustnost závisí na různých podrobnostech o prostředí, jako jsou například průměrná velikost stránky, umístění back-endových instancí a doba zpracování potřebná k doručení stránky. Přesné údaje o výkonu získáte, když spustíte vlastní testy. Tyto hodnoty slouží jenom jako vodítko při plánování kapacity.

## <a name="health-monitoring"></a>Monitorování stavu

Služba Azure Application Gateway automaticky monitoruje stav back-endových instancí prostřednictvím základních nebo vlastních sond stavu. Díky používání sond stavu je zajištěno, že na provoz reagují pouze hostitelé, kteří jsou v pořádku. Další informace najdete v tématu [Přehled monitorování stavu ve službě Application Gateway](application-gateway-probe-overview.md).

## <a name="configuring-and-managing"></a>Konfigurace a správa

Služba Application Gateway může pro svůj koncový bod při konfiguraci mít veřejnou IP adresu, privátní IP adresu nebo obojí. Služba Application Gateway je nakonfigurována ve virtuální síti ve vlastní podsíti. Podsíť vytvořená nebo používaná pro službu Application Gateway nemůže obsahovat žádný jiný typ prostředků, jediné povolené prostředky v podsíti jsou další služby Application Gateway. Pro zabezpečení vašich back-endových prostředků mohou být back-endové servery obsažené v rámci jiné podsítě ve stejné virtuální síti jako služba Application Gateway. Tato další podsíť není vyžadována pro back-endové aplikace. Dokud má služba Application Gateway přístup k IP adrese, je schopna poskytovat back-endovým serverům možnosti ADC. 

Službu Application Gateway můžete vytvořit a spravovat pomocí rozhraní REST API, rutin prostředí PowerShell, rozhraní příkazového řádku Azure nebo webu [Azure Portal](https://portal.azure.com/). V případě dalších dotazů na aplikační bránu si přečtěte článek [Nejčastější dotazy k Application Gateway](application-gateway-faq.md), který obsahuje seznam častých dotazů.

## <a name="pricing"></a>Ceny

Ceny jsou založeny na hodinové sazbě za instanci brány a na poplatcích za zpracování dat. Hodinová sazba za bránu u skladové jednotky WAF se liší od sazby u standardní skladové jednotky. Sazby jsou uvedeny v článku [Podrobnosti o cenách Application Gateway](https://azure.microsoft.com/pricing/details/application-gateway/). Poplatky za zpracování dat zůstávají stejné.

## <a name="faq"></a>Nejčastější dotazy

Nejčastější dotazy k službě Application Gateway najdete v tématu [Nejčastější dotazy k Application Gateway](application-gateway-faq.md).

## <a name="next-steps"></a>Další kroky

Po získání informací o službě Application Gateway můžete [vytvořit službu Application Gateway](application-gateway-create-gateway-portal.md) nebo [vytvořit přesměrování zpracování SSL ve službě Application Gateway](application-gateway-ssl-arm.md) pro vyrovnávání zatížení připojení HTTPS.

Další informace o vytvoření služby Application Gateway používající směrování obsahu na základě adresy URL najdete v tématu [Vytvoření služby Application Gateway používající směrování na základě adresy URL](application-gateway-create-url-route-arm-ps.md).

Další informace o některých dalších klíčových síťových funkcích Azure najdete v tématu [Sítě Azure]((../networking/networking-overview.md).

