---
title: "Nejčastější dotazy pro Azure Application Gateway | Microsoft Docs"
description: "Tato stránka obsahuje odpovědi na nejčastější dotazy týkající se Azure Application Gateway"
documentationcenter: na
services: application-gateway
author: davidmu1
manager: timlt
editor: tysonn
ms.assetid: d54ee7ec-4d6b-4db7-8a17-6513fda7e392
ms.service: application-gateway
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 07/19/2017
ms.author: davidmu
ms.openlocfilehash: 2ae53d5aceab21b08ccdae5bf5192529cd7f09da
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/11/2017
---
# <a name="frequently-asked-questions-for-application-gateway"></a>Nejčastější dotazy pro službu Application Gateway

## <a name="general"></a>Obecné

**OTÁZKY. Co je aplikační brána?**

Služba Azure Application Gateway je řadiče doručení aplikace (ADC) jako služba nabízí různé vrstvy 7 možnosti vyrovnávání zatížení pro vaše aplikace. Nabízí vysoce dostupné a škálovatelné služby, která je plně spravovaná službou Azure.

**OTÁZKY. Jaké funkce podporuje Application Gateway?**

Aplikační brána podporuje snižování zátěže protokolu SSL a koncová SSL, brány Firewall webových aplikací, spřažení na základě souboru cookie relace, adresa url na základě cesty směrování, hostování více lokality a ostatní. Úplný seznam podporovaných funkcích najdete na adrese [Úvod aplikační brány](application-gateway-introduction.md)

**OTÁZKY. Jaký je rozdíl mezi aplikační bránu a nástroj pro vyrovnávání zatížení Azure?**

Application Gateway je Vyrovnávání zatížení vrstvy 7, což znamená, že pracuje pouze webový provoz (HTTP nebo HTTPS/WebSocket). Podporuje možnosti, například ukončení protokolu SSL, spřažení relace na základě souborů cookie a kruhové dotazování pro provoz služby Vyrovnávání zatížení. Nástroj pro vyrovnávání zatížení a načtěte provozu zůstatky na vrstvě 4 (TCP/UDP).

**OTÁZKY. Jaké protokoly podporuje Application Gateway?**

Aplikační brána podporuje protokoly HTTP, HTTPS a protokolu WebSocket.

**OTÁZKY. Jaké prostředky jsou dnes podporovány v rámci fondu back-end?**

Back-endové fondy může skládat ze síťových adaptérů, sady škálování virtuálního počítače, veřejné IP adresy, názvy interní IP adres, plně kvalifikované domény (FQDN) a víceklientské back EndY jako Azure Web Apps. Členy fondu back-end brány aplikace nejsou vázaný na sadu dostupnosti. Členy fondu back-end může být napříč clustery, datových center, nebo mimo Azure, dokud mají připojení pomocí protokolu IP.

**OTÁZKY. Jaké oblasti je služba k dispozici v?**

Application Gateway je k dispozici ve všech oblastech globální Azure. Je také dostupná v [Azure China](https://www.azure.cn/) a [Azure Government.](https://azure.microsoft.com/en-us/overview/clouds/government/)

**OTÁZKY. To je vyhrazený pro Moje předplatné nasazení nebo je sdílen na zákazníky?**

Application Gateway je vyhrazené nasazení ve virtuální síti.

**OTÁZKY. Je HTTP -> HTTPS přesměrování podporovány?**

Přesměrování je podporována. Navštivte [přehled přesměrování Application Gateway](application-gateway-redirect-overview.md) Další informace.

**OTÁZKY. Pořadí, v jakém jsou naslouchací procesy zpracování?**

Moduly pro naslouchání jsou zpracovány v pořadí, ve kterém jsou uvedené. Z tohoto důvodu Pokud základní naslouchací proces odpovídá příchozí požadavek zpracuje jej nejprve.  Moduly pro naslouchání více lokalit by měl být nakonfigurovaný před základní naslouchací proces pro zajištění, že provoz se směruje na správný back-end.

**OTÁZKY. Kde najít IP a DNS Application Gateway?**

Pokud používáte veřejnou IP adresu jako koncový bod, tyto informace naleznete na prostředek veřejné IP adresy nebo na stránce Přehled pro službu Application Gateway na portálu. Pro interní IP adresy najdete na stránce Přehled.

**OTÁZKY. IP adresa nebo DNS mění za dobu existence služby Application Gateway?**

Virtuální IP adresu můžete změnit, pokud brána po zastavení a spuštění zákazník. DNS přidružené Application Gateway přes životního cyklu brány nezmění. Z tohoto důvodu doporučujeme použít CNAME alias a přejděte na adresu DNS aplikační brány.

**OTÁZKY. Podporuje Application Gateway statickou IP adresu?**

Ne, aplikační brána nepodporuje statické veřejné IP adresy, ale podporuje statické interní IP adresy.

**OTÁZKY. Podporuje Application Gateway na bráně víc veřejných IP adres?**

Na aplikační brány je podporovaná pouze jednu veřejnou IP adresu.

**OTÁZKY. Podporuje Application Gateway hlavičky x předávaných pro?**

Ano, Application Gateway vloží záhlaví x předávaných pro, x předávaných proto a x předávaných portu do žádosti předávaných do back-end. Formát hlavičky x předávaných pro je čárkami oddělený seznam IP: port. Platné hodnoty pro x předávaných proto jsou protokolu http nebo https. X předávaných port Určuje port, kdy požadavek dosáhla aplikační brány.

**OTÁZKY. Jak dlouho trvá nasazení služby Application Gateway? Moje aplikace brány stále funguje při aktualizaci?**

Nová nasazení aplikací brány může trvat až 20 minut zřídit. Změny velikosti nebo počet instancí nejsou rušivý a během této doby je nadále aktivní brány.

## <a name="configuration"></a>Konfigurace

**OTÁZKY. Je aplikační brána vždy nasazený ve virtuální síti?**

Ano, aplikační brány je vždy nasazena v podsíti virtuální sítě. Tato podsíť může obsahovat pouze Application Gateway.

**OTÁZKY. Můžete aplikační brány komunikují s instancí mimo jeho virtuální síť?**

Aplikační brána může komunikovat instancí mimo virtuální síť, která je v, dokud není IP připojení. Pokud máte v úmyslu používat interní IP adresy jako členy fondu back-end, pak vyžaduje [VNET Peering](../virtual-network/virtual-network-peering-overview.md) nebo [brány VPN](../vpn-gateway/vpn-gateway-about-vpngateways.md).

**OTÁZKY. Lze nasadit nic jiného v podsíť brány aplikace?**

Ne, ale můžete nasadit další application Gateway v podsíti.

**OTÁZKY. Skupiny zabezpečení sítě podporuje na podsíť brány aplikace?**

Skupiny zabezpečení sítě jsou podporovány v podsíti Application Gateway s následujícími omezeními:

* Výjimky musíte umístit pro příchozí komunikaci na portech 65503 65534 pro back-end stavu fungovala správně.

* Odchozí připojení k Internetu, nejde blokovat.

* Provoz z značka AzureLoadBalancer musí být povoleno.

**OTÁZKY. Jaké jsou omezení pro aplikační bránu? Může tyto limity zvýšit?**

Navštivte [omezení brány aplikací](../azure-subscription-service-limits.md#application-gateway-limits) zobrazíte omezení.

**OTÁZKY. Lze použít aplikační brány pro externí i interní provoz současně?**

Ano, podporuje aplikační brány s jeden interní IP adresy a jeden externí IP adresu na aplikační brány.

**OTÁZKY. VNet peering je podporovaný?**

Ano, partnerský vztah virtuální sítě je podporovaný a je výhodné pro provoz v jiných virtuálních sítí Vyrovnávání zatížení.

**OTÁZKY. Mi může sdělit místních serverů, když jsou připojeni pomocí ExpressRoute nebo VPN tunely?**

Ano, tak dlouho, dokud provoz je povolený.

**OTÁZKY. Může mít jeden fond back-end obsluhující mnoho aplikací na jiné porty?**

Architektura malých služby je podporována. Potřebovali byste více nastavení http, které jsou nakonfigurované tak, aby sběru dat na jiné porty.

**OTÁZKY. Podporují vlastní testy paměti na data odpovědi zástupné znaky/regex?**

Vlastní testy paměti nepodporují zástupných znaků nebo regex na data odpovědi. 

**OTÁZKY. Jak se zpracovávají pravidla?**

Pravidla se zpracovávají v pořadí, ve kterém jsou nakonfigurované. Doporučuje se, zda pravidla více lokalit jsou nakonfigurovány před basic pravidla může snížit pravděpodobnost, že provoz se směruje na nevhodných back-end jako základní pravidlo odpovídá provozu na port před vyhodnocovaný pravidlo více lokalit.

**OTÁZKY. Jak se zpracovávají pravidla?**

Pravidla se zpracovávají v pořadí, v jakém byly vytvořeny. Doporučuje se, že před základních pravidel jsou nakonfigurovaná pravidla více lokalit. Nakonfigurováním nejprve naslouchací procesy více lokalit, tato konfigurace snižuje riziko, že provoz se směruje na nevhodných back-end. Tento problém směrování může docházet k základní pravidlo odpovídá provozu na port před vyhodnocovaný pravidlo více lokalit.

**OTÁZKY. Co označují pole hostitel pro vlastní testy paměti?**

Pole hostitel Určuje název odeslat sondy k. Platí jenom v případě více lokalit je nakonfigurovaná na aplikační bránu, v opačném případě použijte "127.0.0.1". Tato hodnota se liší od názvu hostitele virtuálních počítačů a je ve formátu \<protokol\>://\<hostitele\>:\<port\>\<cestu\>.

**OTÁZKY. Můžete vytvořit bílou Application Gateway přístup k několika zdrojové IP adresy?**

Tento scénář lze provést pomocí skupin Nsg na podsítě brány aplikace. Následující omezení měly být umístěny v podsíti v uvedených pořadí podle priority:

* Povolí příchozí provoz ze zdrojových rozsah IP/IP.

* Povolit příchozí požadavky ze všech zdrojů na porty 65503 65534 [komunikace stavu back-end](application-gateway-diagnostics.md).

* Povolit příchozí nástroj pro vyrovnávání zatížení Azure sondy (značka AzureLoadBalancer) a příchozí přenosy virtuální sítě (virtuální síť značky) na [NSG](../virtual-network/virtual-networks-nsg.md).

* Blokovat všechna ostatní příchozí přenosy s odepření všechna pravidla.

* Povolte odchozí přenosy na Internet pro všechna místa určení.

## <a name="performance"></a>Výkon

**OTÁZKY. Jak Application Gateway podporuje vysokou dostupnost a škálovatelnost?**

Aplikační brána podporuje scénáře s vysokou dostupností, až budete mít dva nebo více instancí nasazení. Azure distribuuje tyto instance napříč doménami aktualizace a odolnost zajistit, aby všechny instance nedošlo k selhání ve stejnou dobu. Aplikační brána podporuje škálovatelnost přidáním více instancí stejné bráně sdílení zatížení.

**OTÁZKY. Jak dosáhnout scénář zotavení po Havárii napříč datovými centry s Application Gateway?**

Zákazníci mohou Traffic Manager slouží k distribuci provoz napříč více bran aplikace v různých datových centrech.

**OTÁZKY. Automatické škálování je podporovaný?**

Ne, ale Application Gateway poskytuje metriky propustnosti, který slouží k upozornění, když je dosaženo prahové hodnoty. Ruční přidání instance nebo změna velikosti nerestartuje brány a nemá negativní vliv na stávající přenosy dat.

**OTÁZKY. Podporuje ruční škálování nahoru/dolů příčina výpadku?**

Neexistuje žádné výpadky, instancí jsou rozmístěny v upgradu domén a domén selhání.

**OTÁZKY. Můžete změnit velikost instance ze střední velké bez přerušení?**

Ano, Azure distribuuje instancí napříč doménami aktualizace a odolnost zajistit, aby všechny instance nedošlo k selhání ve stejnou dobu. Aplikační brána podporuje škálování přidáním více instancí stejné bráně sdílení zatížení.

## <a name="ssl-configuration"></a>Konfigurace protokolu SSL

**OTÁZKY. Jaké certifikáty jsou podporovány ve Application Gateway?**

Vlastní podepsané certifikáty, certifikátů certifikační Autority a certifikátů zástupnému znaku. jsou podporovány. Rozšířené ověření certifikátů nejsou podporovány.

**OTÁZKY. Jaké jsou aktuální šifrovací sada podporovaná Application Gateway?**

Níže jsou uvedeny aktuální šifrovací sada podporovaná aplikační brány. Navštivte: [konfigurace SSL verze zásad a šifrovací sady ve Application Gateway](application-gateway-configure-ssl-policy-powershell.md) se dozvíte, jak přizpůsobit možnosti protokolu SSL.

- TLS_ECDHE_RSA_WITH_AES_256_GCM_SHA384
- TLS_ECDHE_RSA_WITH_AES_128_GCM_SHA256
- TLS_ECDHE_RSA_WITH_AES_256_CBC_SHA384
- TLS_ECDHE_RSA_WITH_AES_128_CBC_SHA256
- TLS_ECDHE_RSA_WITH_AES_256_CBC_SHA
- TLS_ECDHE_RSA_WITH_AES_128_CBC_SHA
- TLS_DHE_RSA_WITH_AES_256_GCM_SHA384
- TLS_DHE_RSA_WITH_AES_128_GCM_SHA256
- TLS_DHE_RSA_WITH_AES_256_CBC_SHA
- TLS_DHE_RSA_WITH_AES_128_CBC_SHA
- TLS_RSA_WITH_AES_256_GCM_SHA384
- TLS_RSA_WITH_AES_128_GCM_SHA256
- TLS_RSA_WITH_AES_256_CBC_SHA256
- TLS_RSA_WITH_AES_128_CBC_SHA256
- TLS_RSA_WITH_AES_256_CBC_SHA
- TLS_RSA_WITH_AES_128_CBC_SHA
- TLS_ECDHE_ECDSA_WITH_AES_256_GCM_SHA384
- TLS_ECDHE_ECDSA_WITH_AES_128_GCM_SHA256
- TLS_ECDHE_ECDSA_WITH_AES_256_CBC_SHA384
- TLS_ECDHE_ECDSA_WITH_AES_128_CBC_SHA256
- TLS_ECDHE_ECDSA_WITH_AES_256_CBC_SHA
- TLS_ECDHE_ECDSA_WITH_AES_128_CBC_SHA
- TLS_DHE_DSS_WITH_AES_256_CBC_SHA256
- TLS_DHE_DSS_WITH_AES_128_CBC_SHA256
- TLS_DHE_DSS_WITH_AES_256_CBC_SHA
- TLS_DHE_DSS_WITH_AES_128_CBC_SHA
- TLS_RSA_WITH_3DES_EDE_CBC_SHA
- TLS_DHE_DSS_WITH_3DES_EDE_CBC_SHA

**OTÁZKY. Application Gateway také podporuje znova šifrovat provoz na back-end?**

Ano, aplikační brána podporuje přesměrování zpracování SSL a koncová SSL, který znovu zašifruje provoz na back-end.

**OTÁZKY. Můžete nakonfigurovat zásady protokolu SSL pro řízení verzí protokolu SSL?**

Ano, můžete nakonfigurovat tak, aby odepřel TLS1.0, TLS1.1 a TLS1.2 Application Gateway. Protokol SSL 2.0 a 3.0 jsou už ve výchozím nastavení zakázána a se nedají konfigurovat.

**OTÁZKY. Můžete nakonfigurovat šifrovací sady a pořadí zásad?**

Ano, [konfigurace šifrovací sady](application-gateway-ssl-policy-overview.md) je podporována. Při definování vlastní zásady, musí být povolena alespoň jeden z následujících sad šifer. Aplikační brána používá SHA256 k pro správu back-end.

* TLS_ECDHE_RSA_WITH_AES_128_GCM_SHA256 
* TLS_ECDHE_RSA_WITH_AES_128_CBC_SHA256
* TLS_DHE_RSA_WITH_AES_128_GCM_SHA256
* TLS_RSA_WITH_AES_128_GCM_SHA256
* TLS_RSA_WITH_AES_256_CBC_SHA256
* TLS_RSA_WITH_AES_128_CBC_SHA256

**OTÁZKY. Jak velký počet certifikátů SSL jsou podporovány?**

Až 20 SSL certifikáty jsou podporovány.

**OTÁZKY. Kolik ověřovací certifikáty pro back-end znova šifrovat jsou podporovány?**

Až 10 ověřování certifikáty jsou podporovány výchozí hodnota je 5.

**OTÁZKY. Umožňuje Application Gateway integraci s Azure Key Vault nativně?**

Ne, není integrované s Azure Key Vault.

## <a name="web-application-firewall-waf-configuration"></a>Konfigurace brány Firewall (firewall webových aplikací) webové aplikace

**OTÁZKY. SKU firewall webových aplikací nabízí všechny funkce, které jsou k dispozici standardní SKU systém?**

Ano, firewall webových aplikací podporuje všechny funkce v standardní SKU.

**OTÁZKY. Co je verze řádku Application Gateway podporuje?**

Aplikační brána podporuje řádku [2.2.9](application-gateway-crs-rulegroups-rules.md#owasp229) a řádku [3.0](application-gateway-crs-rulegroups-rules.md#owasp30).

**OTÁZKY. Jak se monitorování firewall webových aplikací?**

Firewall webových aplikací je monitorována prostřednictvím protokolování diagnostiky, další informace o protokolování diagnostiky naleznete na [protokolování diagnostiky a metriky pro službu Application Gateway](application-gateway-diagnostics.md)

**OTÁZKY. Detekce režimu blokování provozu?**

Ne, detekce režimu jenom protokoly přenosy, která spustí pravidlo firewall webových aplikací.

**OTÁZKY. Jak lze přizpůsobit pravidla firewall webových aplikací?**

Ano, pravidla firewall webových aplikací jsou přizpůsobitelné, další informace o tom, jak přizpůsobit návštěvu [skupiny pravidel přizpůsobit firewall webových aplikací a pravidla](application-gateway-customize-waf-rules-portal.md)

**OTÁZKY. Jaká pravidla jsou nyní k dispozici?**

Firewall webových aplikací v současné době podporuje řádku [2.2.9](application-gateway-crs-rulegroups-rules.md#owasp229) a [3.0](application-gateway-crs-rulegroups-rules.md#owasp30), které poskytují základní zabezpečení proti většinu prvních 10 ohrožení zabezpečení, identifikovat pomocí otevřít webové aplikace zabezpečení projektu (OWASP) je zde uveden [OWASP top 10 ohrožení zabezpečení](https://www.owasp.org/index.php/Top10#OWASP_Top_10_for_2013)

* Ochrana před útoky prostřednictvím injektáže SQL.

* Ochrana před skriptováním mezi weby.

* Ochrana před běžnými webovými útoky, jako je například injektáž příkazů, pronášení požadavků HTTP, rozdělování odpovědí protokolu HTTP a útok pomocí vložení vzdáleného souboru.

* Ochrana před narušením protokolu HTTP.

* Ochrana před anomáliemi protokolu HTTP, jako například chybějící údaj user-agent hostitele nebo hlavičky Accept.

* Ochrana před roboty, prohledávacími moduly a skenery.

* Detekce časté nesprávné konfigurace aplikace (tedy Apache, IIS, atd.)

**OTÁZKY. Firewall webových aplikací taky podporuje DDoS prevence?**

Ne, firewall webových aplikací neposkytuje DDoS prevence.

## <a name="diagnostics-and-logging"></a>Protokolování a diagnostiky

**OTÁZKY. Jaké typy protokolů jsou k dispozici s Application Gateway?**

Nejsou k dispozici pro službu Application Gateway tři protokoly. Další informace o těchto protokolů a jiné diagnostické funkce, najdete v článku [back-end stavu, protokolů diagnostiky a metriky pro službu Application Gateway](application-gateway-diagnostics.md).

- **ApplicationGatewayAccessLog** – protokol přístupu obsahuje každý odeslaný požadavek na front-endu Application Gateway. Data obsahují volajícího IP adresy, požadovaná, adresa URL odpovědi latence, návratový kód, bajtů a odhlášení. Protokol přístupu se shromažďují každých 300 sekund. Tento protokol obsahuje jeden záznam za instance aplikační brány.
- **ApplicationGatewayPerformanceLog** -výkonu protokolu zaznamená informace o výkonu na základě za instance včetně celkový požadavek zpracovat, propustnost v bajtech, počtu žádostí o obsloužit se nezdařila celkový počet požadavků, počet instancí back-end v pořádku a není v pořádku.
- **ApplicationGatewayFirewallLog** -protokolu brány firewall obsahuje požadavky, které se protokolují prostřednictvím zjišťování nebo zabránění režim služby application gateway, která je konfigurovaná pomocí brány firewall webových aplikací.

**OTÁZKY. Jak poznám, pokud jsou moje členy fondu back-end v pořádku?**

Můžete použít rutiny prostředí PowerShell `Get-AzureRmApplicationGatewayBackendHealth` nebo ověřte stav prostřednictvím portálu, navštivte stránky [diagnostiku brány aplikace](application-gateway-diagnostics.md)

**OTÁZKY. Co je zásady uchovávání informací na protokolů diagnostiky?**

Diagnostické protokoly toku k účtu úložiště zákazníků a zákazníků můžete nastavit zásady uchovávání informací podle jejich předvoleb. Diagnostické protokoly můžete odeslat také do centra událostí nebo analýzy protokolů. Navštivte [Application Diagnostics brány](application-gateway-diagnostics.md) další podrobnosti.

**OTÁZKY. Získání protokolů auditu pro službu Application Gateway**

Protokoly auditu jsou k dispozici pro službu Application Gateway. Na portálu, klikněte na tlačítko **protokol aktivit** v okně nabídky aplikační brány pro přístup k protokolu auditu. 

**OTÁZKY. Můžete nastavit výstrahy s Application Gateway?**

Ano, aplikační brána podporuje výstrahy, se konfigurují vypnout metriky.  Aplikační brána v současnosti má metriky propustnosti"", kterého lze nakonfigurovat výstrahy. Další informace o výstrahách naleznete [dostávat oznámení o výstrahách](../monitoring-and-diagnostics/insights-receive-alert-notifications.md).

**OTÁZKY. Back-end stavu vrátí Neznámý stav, co by mohlo být příčinou tento stav?**

Nejčastější příčinou je skupina NSG nebo vlastní DNS je blokován přístup k back-end. Navštivte [back-end stavu, protokolování diagnostiky a metriky pro službu Application Gateway](application-gateway-diagnostics.md) Další informace.

## <a name="next-steps"></a>Další kroky

Další informace o návštěvě Application Gateway [Úvod do Application Gateway](application-gateway-introduction.md).
