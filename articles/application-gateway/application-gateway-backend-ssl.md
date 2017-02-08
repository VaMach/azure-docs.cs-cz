---
title: "Povolení zásad protokolu SSL a koncového šifrování protokolu SSL ve službě Application Gateway | Dokumentace Microsoftu"
description: "Tato stránka poskytuje přehled podpory koncového šifrování protokolu SSL ve službě Application Gateway."
documentationcenter: na
services: application-gateway
author: amsriva
manager: rossort
editor: amsriva
ms.assetid: 3976399b-25ad-45eb-8eb3-fdb736a598c5
ms.service: application-gateway
ms.devlang: na
ms.topic: hero-article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 12/12/2016
ms.author: amsriva
translationtype: Human Translation
ms.sourcegitcommit: cb2b7bc626294e12c6e19647c1e787e1f671595b
ms.openlocfilehash: a49a93b11ab3e965ac1ddaec919bfcbf43381dee


---
# <a name="enabling-ssl-policy-and-end-to-end-ssl-on-application-gateway"></a>Povolení zásad protokolu SSL a koncového šifrování protokolu SSL ve službě Application Gateway

Služba Application Gateway podporuje ukončení protokolu SSL na bráně, po čemž provoz typicky teče nešifrován na back-endové servery. Tato funkce webovým serverům umožňuje snížení nákladné režie spojené s šifrováním a dešifrováním. Pro některé zákazníky je však nešifrovaná komunikace s back-endovými servery nepřijatelnou možností. Může to být z důvodu požadavků na zabezpečení nebo dodržování předpisů nebo protože aplikace může přijímat pouze zabezpečená připojení. Pro takové aplikace služba Application Gateway nyní podporuje koncové šifrování protokolu SSL.

## <a name="overview"></a>Přehled

Koncové šifrování protokolu SSL umožňuje bezpečně přenášet citlivá data do back-endu v zašifrované podobě a současně využívat výhody funkcí pro vyrovnávání zatížení vrstvy 7, které nabízí aplikační brána. Jde například o spřažení na základě souborů cookie, směrování na základě adres URL, podporu směrování založeného na webech nebo možnost vkládat hlavičky X-Forwarded-*.

Když je nakonfigurována s režimem komunikace koncového šifrování protokolu SSL, služba Application Gateway ukončuje na bráně uživatelské relace protokolu SSL a dešifruje provoz uživatelů. Následně použije nakonfigurovaná pravidla k výběru příslušné instance back-endového fondu, na kterou provoz přesměruje. Služba Application Gateway poté zahájí nové připojení SSL k back-endovému serveru a před odesláním požadavku do back-endu znovu zašifruje data pomocí certifikátu s veřejným klíčem back-endového serveru. Koncové šifrování protokolu SSL se povoluje nastavením nastavení protokolu v BackendHTTPSetting na hodnotu Https. Toto nastavení se následně použije na back-endový fond. Každý back-endový server v back-endovém fondu s povoleným koncovým šifrováním protokolu SSL musí být pro umožnění bezpečné komunikace nakonfigurován s certifikátem.

![scénář koncového šifrování protokolu ssl][1]

V tomto příkladu jsou požadavky, které používají šifrování TLS&1;.2, přesměrované koncovým šifrováním protokolu SSL na back-endové servery fondu Pool1.

## <a name="end-to-end-ssl-and-whitelisting-of-certificates"></a>Koncové šifrování protokolu SSL a vytváření seznamu povolených certifikátů

Služba Application Gateway komunikuje pouze se známými back-endovými instancemi, jejichž certifikáty jsou uvedeny v seznamu povolených certifikátů ve službě Application Gateway. Chcete-li povolit vytváření seznamu povolených certifikátů, musíte do aplikační brány nahrát veřejný klíč certifikátů back-endového serveru (nikoliv kořenový certifikát). Potom budou povolena jenom připojení ke známým back-endům uvedeným v seznamu. Zbývající back-endy způsobí chybu brány. Certifikáty podepsané svým držitelem slouží pouze k testování a nedoporučují se pro úlohy v produkčním prostředí. Takové certifikáty také musí být uvedeny v seznamu povolených certifikátů aplikační brány (viz popis v předchozích krocích), aby bylo možné je použít.

## <a name="application-gateway-ssl-policy"></a>Zásady protokolu SSL ve službě Application Gateway

Služba Application Gateway podporuje uživatelsky konfigurovatelné zásady vyjednávání SSL, které zákazníkům dávají větší kontrolu nad připojeními SSL v aplikační bráně.

1. Protokoly SSL 2.0 a 3.0 jsou ve výchozím nastavení zakázané pro všechny brány Application Gateway. Nelze je vůbec konfigurovat.
2. Definice zásad protokolu SSL umožňuje zakázat kterýkoliv z těchto tří protokolů – **TLSv1\_0**, **TLSv1\_1**, **TLSv1\_2**.
3. Pokud nejsou definované žádné zásady protokolu SSL, jsou povolené všechny tři protokoly (TLSv1\_0, TLSv1\_1, TLSv1_2).

## <a name="next-steps"></a>Další kroky

Po získání informací o koncovém šifrování protokolu SSL a zásadách protokolu SSL přejděte k tématu [Povolení koncového šifrování protokolu SSL v aplikační bráně](application-gateway-end-to-end-ssl-powershell.md) a vytvořte aplikační bránu s koncovým šifrováním protokolu SSL.

<!--Image references-->

[1]: ./media/application-gateway-backend-ssl/scenario.png



<!--HONumber=Dec16_HO2-->


