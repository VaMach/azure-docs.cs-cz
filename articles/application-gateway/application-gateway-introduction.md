<properties
   pageTitle="Seznámení se službou Application Gateway | Microsoft Azure"
   description="Tato stránka poskytuje přehled služby Application Gateway pro vyrovnávání zatížení vrstvy 7, včetně velikostí bran, vyrovnávání zatížení HTTP, spřažení relace na základě souborů cookie a přesměrování zpracování SSL."
   documentationCenter="na"
   services="application-gateway"
   authors="georgewallace"
   manager="carmonm"
   editor="tysonn"/>
<tags
   ms.service="application-gateway"
   ms.devlang="na"
   ms.topic="hero-article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="09/26/2016"
   ms.author="gwallace"/>


# <a name="application-gateway-overview"></a>Přehled služby Application Gateway

## <a name="what-is-application-gateway"></a>Co je Application Gateway

Služba Microsoft Azure Application Gateway poskytuje Application Delivery Controller (ADC) jako službu s mnoha možnostmi vyrovnávání zatížení vrstvy 7. Jednoduše řečeno funguje tak, že přijímá provoz a na základě pravidel definovaných ve službě provoz směruje do příslušných back-endových instancí.

Vyrovnávání zatížení aplikací umožňuje správcům IT a vývojářům vytvářet pravidla směrování síťového provozu na základě protokolu HTTP.  Služba Azure Application Gateway je vysoce dostupná a měřená. Informace o smlouvě SLA a cenách najdete na příslušných stránkách [SLA](https://azure.microsoft.com/support/legal/sla/) a [Ceny](https://azure.microsoft.com/pricing/details/application-gateway/).

Služba Application Gateway používá pravidla směrování pro přenosy pomocí protokolu HTTP a poskytuje tak vyrovnávání zatížení vrstvy 7 (HTTP). Když vytvoříte službu Application Gateway, k příchozímu síťovému přenosu se přidruží koncový bod (virtuální IP adresa), která se použije jako veřejná IP adresa. Azure poskytuje vyrovnávání zatížení vrstvy 4 prostřednictvím nástroje Azure Load Balancer, který funguje na úrovni přenosu (TCP/UDP) a vyrovnává zatížení veškerého příchozího síťového přenosu do služby Application Gateway. Služba Application Gateway na základě své konfigurace směruje přenos pomocí protokolu HTTP, ať už se jedná o virtuální počítač, cloudovou službu nebo externí IP adresu.

## <a name="features"></a>Funkce

Služba Application Gateway v současné době podporuje doručování aplikací vrstvy 7 pomocí následujících funkcí:

- **[Firewall webových aplikací (Preview)](application-gateway-webapplicationfirewall-overview.md)** – Firewall webových aplikací (WAF) ve službě Azure Application Gateway chrání webové aplikace před běžnými webovými útoky, jako jsou například útoky prostřednictvím injektáže SQL, skriptování mezi weby a napadení relace.
- **Vyrovnávání zatížení HTTP** – Služba Application Gateway poskytuje vyrovnávání zatížení kruhovým dotazováním. Vyrovnávání zatížení probíhá na vrstvě 7 a slouží pouze pro přenosy pomocí protokolu HTTP nebo HTTPS.
- **Spřažení relace na základě souborů cookie** – Tato funkce je užitečná v případě, že chcete zachovat uživatelskou relaci na stejném back-endu. Pomocí souborů cookie spravovaných bránou je služba Application Gateway schopna směrovat následný provoz z uživatelské relace k zpracování do stejného back-endu. Tato funkce je důležitá v případech, kdy se stav jednotlivých uživatelských relací ukládá místně na back-endovém serveru.
- **[Přesměrování zpracování Secure Sockets Layer (SSL)](application-gateway-ssl-arm.md)** – Tato funkce zbaví vaše webové servery nákladné úlohy dešifrování přenosu pomocí protokolu HTTPS. Díky ukončení připojení protokolem SSL ve službě Application Gateway a předání požadavku na server v nezašifrované podobě, není server zatížen dešifrováním.  Služba Application Gateway znovu šifruje odpověď před tím, než ji odešle zpět klientovi. Tato funkce je užitečná ve scénářích, kdy je back-end umístěn v rámci Azure ve stejné zabezpečené virtuální síti jako služba Application Gateway.
- **[Konečné šifrování protokolu SSL](application-gateway-backend-ssl.md)** – Služba Application Gateway podporuje koncové šifrování provozu. Služba Application Gateway to provádí ukončením připojení protokolem SSL ve službě Application Gateway. Brána následně použije na provoz pravidla směrování, znovu zašifruje paket a předá tento paket do příslušného back-endu na základě nadefinovaných pravidel směrování. Každá odpověď webového serveru prochází ke koncovému uživateli stejným procesem.
- **[Směrování obsahu na základě adresy URL](application-gateway-url-route-overview.md)** – Tato funkce poskytuje možnost používat různé back-endové servery pro různý provoz. Provoz pro složku na webovém serveru nebo pro síť CDN lze směrovat na různé back-endy a tím snížit nepotřebné zatížení back-endů, které neposkytují konkrétní obsah.
- **[Směrování více webů](application-gateway-multi-site-overview.md)** – Služba Application Gateway umožňuje konsolidovat až 20 webů v jedné službě Application Gateway.
- **[Podpora protokolu WebSocket](application-gateway-websocket.md)** – Další skvělou funkcí služby Application Gateway je nativní podpora protokolu WebSocket.
- **[Monitorování stavu](application-gateway-probe-overview.md)** – Služba Application Gateway poskytuje výchozí monitorování stavu back-endových prostředků a vlastní monitorovací sondy pro konkrétnější scénáře.

## <a name="benefits"></a>Výhody

Služba Application Gateway je užitečná pro:

- Aplikace, které vyžadují, aby se požadavky ze stejné uživatelské/klientské relace pokaždé dostaly ke stejnému back-endovému virtuálnímu počítači. Příklady těchto aplikací by mohly být aplikace nákupních košíků a servery webové pošty.
- Aplikace, které chtějí zbavit webové serverové farmy režie spojené s ukončováním protokolu SSL.
- Aplikace, jako je například síť pro doručování obsahu, které vyžadují, aby bylo během jednoho dlouhotrvajícího připojení TCP více požadavků HTTP směrováno nebo směrováno s vyrovnáváním zatížení na různé back-endové servery.
- Aplikace, které podporují přenos pomocí protokolu WebSocket.
- Ochranu webových aplikací před běžnými webovými útoky, jako jsou například útoky prostřednictvím injektáže SQL, skriptování mezi weby a napadení relace.

[AZURE.INCLUDE [load-balancer-compare-tm-ag-lb-include.md](../../includes/load-balancer-compare-tm-ag-lb-include.md)]

## <a name="gateway-sizes-and-instances"></a>Velikosti a instance brány

Služba Application Gateway je v současné době nabízena ve třech velikostech: krátkodobé používání, střednědobé používání a dlouhodobé používání. Instance krátkodobého používání jsou určené pro scénáře vývoje a testování.

V současnosti existují pro službu Application Gateway dvě skladové jednotky (SKU): WAF a Standard.

V rámci jednoho předplatného můžete vytvořit až 50 služeb Application Gateway a každá z nich může mít až 10 instancí. Každá služba Application Gateway se může skládat z 20 naslouchacích procesů HTTP. Vyrovnávání zatížení ve službě Application Gateway jako služba spravovaná Azure umožňuje zřízení nástroje pro vyrovnávání zatížení vrstvy 7 za nástrojem pro vyrovnávání zatížení softwaru Azure.

Následující tabulka ukazuje průměrnou propustnost výkonu pro jednotlivé instance služby Application Gateway:

| Odezva back-endové stránky | Krátkodobé používání | Střednědobé používání | Dlouhodobé používání|
|---|---|---|---|
| 6K | 7,5 Mb/s | 13 Mb/s | 50 Mb/s |
|100K | 35 Mb/s | 100 Mb/s| 200 Mb/s |

>[AZURE.NOTE] Tyto hodnoty jsou přibližné hodnoty propustnosti služby Application Gateway. Skutečná propustnost závisí na různých podrobnostech o prostředí, jako jsou například průměrná velikost stránky, umístění back-endových instancí a doba zpracování potřebná k doručení stránky. Pro přesné údaje o výkonu byste měli spustit vlastní testy, tyto hodnoty slouží pouze jako pomůcka pro plánování kapacity.

## <a name="health-monitoring"></a>Monitorování stavu

Služba Azure Application Gateway automaticky monitoruje stav back-endových instancí prostřednictvím základních nebo vlastních sond stavu. Díky používání sond stavu je zajištěno, že na provoz reagují pouze hostitelé, kteří jsou v pořádku. Další informace najdete v tématu [Přehled monitorování stavu ve službě Application Gateway](application-gateway-probe-overview.md).

## <a name="configuring-and-managing"></a>Konfigurace a správa

Služba Application Gateway může pro svůj koncový bod při konfiguraci mít veřejnou IP adresu, privátní IP adresu nebo obojí. Služba Application Gateway je nakonfigurována ve virtuální síti ve vlastní podsíti. Podsíť vytvořená nebo používaná pro službu Application Gateway nemůže obsahovat žádný jiný typ prostředků, jediné povolené prostředky v podsíti jsou další služby Application Gateway. Pro zabezpečení vašich back-endových prostředků mohou být back-endové servery obsažené v rámci jiné podsítě ve stejné virtuální síti jako služba Application Gateway. Tato další podsíť není vyžadována pro back-endové aplikace. Dokud má služba Application Gateway přístup k IP adrese, je schopna poskytovat back-endovým serverům možnosti ADC.

Službu Application Gateway můžete vytvořit a spravovat pomocí rozhraní REST API, rutin prostředí PowerShell, rozhraní příkazového řádku Azure nebo webu [Azure Portal](https://portal.azure.com/).

## <a name="next-steps"></a>Další kroky

Po získání informací o službě Application Gateway můžete [vytvořit službu Application Gateway](application-gateway-create-gateway-portal.md) nebo [vytvořit přesměrování zpracování SSL ve službě Application Gateway](application-gateway-ssl-arm.md) pro vyrovnávání zatížení připojení HTTPS.

Další informace o vytvoření služby Application Gateway používající směrování obsahu na základě adresy URL najdete v tématu [Vytvoření služby Application Gateway používající směrování na základě adresy URL](application-gateway-create-url-route-arm-ps.md).




<!--HONumber=Oct16_HO3-->


