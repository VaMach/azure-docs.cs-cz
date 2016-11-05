---
title: Povolení zásad protokolu SSL a koncového šifrování protokolu SSL ve službě Application Gateway | Microsoft Docs
description: Tato stránka poskytuje přehled podpory koncového šifrování protokolu SSL ve službě Application Gateway.
documentationcenter: na
services: application-gateway
author: amsriva
manager: rossort
editor: amsriva

ms.service: application-gateway
ms.devlang: na
ms.topic: hero-article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 09/26/2016
ms.author: amsriva

---
# <a name="enabling-ssl-policy-and-end-to-end-ssl-on-application-gateway"></a>Povolení zásad protokolu SSL a koncového šifrování protokolu SSL ve službě Application Gateway
## <a name="overview"></a>Přehled
Služba Application Gateway podporuje ukončení protokolu SSL na bráně, po čemž provoz typicky teče nešifrován na back-endové servery. To webovým serverům umožňuje snížení nákladné režie spojené s šifrováním a dešifrováním. Pro některé zákazníky je však nešifrovaná komunikace s back-endovými servery nepřijatelnou možností. Může to být z důvodu požadavků na zabezpečení nebo dodržování předpisů nebo protože aplikace může přijímat pouze zabezpečená připojení. Pro takové aplikace služba Application Gateway nyní podporuje koncové šifrování protokolu SSL.

Koncové šifrování protokolu SSL umožňuje bezpečně přenášet citlivá data do back-endu v zašifrované podobě, zatímco využívá výhody funkcí vyrovnávání zatížení vrstvy 7, které služba Application Gateway nabízí. Jedná se například o spřažení na základě souborů cookie, směrování na základě adresy URL, podpora směrování založeném na webech nebo schopnost injektovat hlavičky X-Forwarded-*.

Když je nakonfigurována s režimem komunikace koncového šifrování protokolu SSL, služba Application Gateway ukončuje na bráně uživatelské relace protokolu SSL a dešifruje provoz uživatelů. Následně použije nakonfigurovaná pravidla k výběru příslušné instance back-endového fondu, na kterou provoz přesměruje. Služba Application Gateway poté zahájí nové připojení protokolem SSL k back-endovému serveru a před odesláním požadavku na back-end znovu zašifruje data pomocí certifikátu veřejného klíče back-endového serveru. Koncové šifrování protokolu SSL se povoluje nastavením nastavení protokolu v BackendHTTPSetting na hodnotu Https. Toto nastavení se následně použije na back-endový fond. Každý back-endový server v back-endovém fondu s povoleným koncovým šifrováním protokolu SSL musí být pro umožnění bezpečné komunikace nakonfigurován s certifikátem.

![imageURLroute](./media/application-gateway-multi-site-overview/multisite.png)

V tomto příkladu lze požadavky na https://contoso.com směrovat na FondServeruContoso prostřednictvím protokolu HTTP a požadavky na https://fabrikam.com budou směrovány na FondServeruFabrikam prostřednictvím protokolu HTTPS pomocí koncového šifrování protokolu SSL.

## <a name="end-to-end-ssl-and-white-listing-of-certificates"></a>Koncové šifrování protokolu SSL a vytváření seznamu povolených certifikátů
Služba Application Gateway komunikuje pouze se známými back-endovými instancemi, jejichž certifikáty jsou uvedené v seznamu povolených certifikátů ve službě Application Gateway. Chcete-li povolit vytváření seznamu povolených certifikátů, je nutné nahrát do služby Application Gateway veřejný klíč certifikátů back-endového serveru. Poté budou povolená pouze připojení ke známému back-endu uvedenému v seznamu povolených back-endů. Ostatní připojení způsobí chybu brány. Certifikáty podepsané svým držitelem slouží pouze k testování a nedoporučují se pro úlohy v produkčním prostředí. Před použitím těchto certifikátů také musí být uvedené v seznamu povolených certifikátů ve službě Application Gateway.

## <a name="application-gateway-ssl-policy"></a>Zásady protokolu SSL ve službě Application Gateway
Služba Application Gateway také podporuje uživatelsky konfigurovatelné zásady vyjednávání SSL, které zákazníkům umožňují jemnější kontrolu připojení protokolem SSL ve službě Application Gateway.

1. Protokoly SSL 2.0 a 3.0 jsou vynuceně zakázané pro všechny služby Application Gateway. Nelze je vůbec konfigurovat.
2. Definice zásad protokolu SSL umožňuje zakázat libovolný z těchto 3 protokolů – TLSv1_0, TLSv1_1, TLSv1_2.
3. Pokud nejsou definovány žádné zásady protokolu SSL, jsou povolené všechny tři protokoly (TLSv1_0, TLSv1_1, TLSv1_2).

## <a name="next-steps"></a>Další kroky
Po získání informací o koncovém šifrování protokolu SSL a zásadách protokolu SSL přejděte k tématu [Povolení kompletního protokolu SSL ve službě Application Gateway](application-gateway-end-to-end-ssl-powershell.md) a vytvořte službu Application Gateway se schopností posílat provoz do back-endu v zašifrované podobě.

<!--HONumber=Oct16_HO3-->


