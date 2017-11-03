---
title: "Stav monitorování přehled Azure Application Gateway. | Microsoft Docs"
description: "Další informace o možnosti monitorování v Azure Application Gateway"
services: application-gateway
documentationcenter: na
author: davidmu1
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 7eeba328-bb2d-4d3e-bdac-7552e7900b7f
ms.service: application-gateway
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 12/14/2016
ms.author: davidmu
ms.openlocfilehash: 83a0b1be1aba48146aa1aaedb36ad9d9d23f17d6
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/11/2017
---
# <a name="application-gateway-health-monitoring-overview"></a>Monitorování přehled stavu aplikace brány

Ve výchozím nastavení služba Azure Application Gateway monitoruje stav všechny prostředky v jeho fond back-end a automaticky odebere jakémukoli prostředku, považoval za poškozený z fondu. Aplikační brána pokračuje v monitorování není v pořádku instancí a přidá je zpět do fondu back-end v pořádku, jakmile jsou k dispozici a reagovat na sondy stavu. Aplikační brána odešle sondy stavu s stejný port, který je definován v nastavení HTTP back-end. Tato konfigurace zajistí, že tato kontrola je testování stejný port, který zákazníky se používají pro připojení k back-end.

![Příklad testu brány aplikace][1]

Kromě použití výchozího stavu testu monitorování, můžete také upravit test stavu podle potřeb vaší aplikace. V tomto článku jsou popsané výchozí i vlastní stavu sondy.

> [!NOTE]
> Pokud je skupina NSG na podsítě brány aplikace, musí být otevřen rozsahy portů 65503 65534 na podsíť aplikační brány pro příchozí provoz. Tyto porty jsou povinné pro back-end stav rozhraní API pro práci.

## <a name="default-health-probe"></a>Výchozí kontroly stavu

Služby application gateway automaticky nakonfiguruje výchozí kontrolu stavu, když nemáte nastavíte žádnou konfiguraci vlastní test paměti. Monitorování chování funguje tak, že požadavek HTTP na IP adresy nakonfigurované pro fond back-end. Pro výchozí sondy Pokud nastavení http back-end jsou nakonfigurované pro protokol HTTPS, že test využívá protokol HTTPS také k testování stavu back-EndY.

Příklad: Konfigurace brány aplikace používat back-end serverů A, B a C přijímat síťový provoz protokolu HTTP na portu 80. Sledování stavu výchozí testy tři servery každých 30 sekund pro pořádku odpověď HTTP. Je v pořádku odpovědi HTTP [stavový kód](https://msdn.microsoft.com/library/aa287675.aspx) 200 až 399.

Pokud kontrola výchozí test nezdaří serveru A, služby application gateway odstraní ji z jeho fond back-end a síťový provoz bude zastaven k tomuto serveru. Výchozí kontroly stále bude kontrolovat pro server každých 30 sekund. Pokud serveru A odpoví na žádost o jeden z výchozí stav kontroly úspěšně, ho je přidat do fondu back-end zpět jako v pořádku a provozu spustí předávaných na server znovu.

### <a name="default-health-probe-settings"></a>Výchozí nastavení kontroly stavu

| Vlastnost testu | Hodnota | Popis |
| --- | --- | --- |
| Adresa URL testu |http://127.0.0.1:\<portu\>/ |Cesta adresy URL |
| Interval |30 |Interval testu paměti v sekundách |
| Časový limit |30 |Časový limit testu v sekundách |
| Prahová hodnota špatného stavu |3 |Počet opakování testu. Back-end serverů je označena po počet po sobě jdoucích test selhání dosáhne prahová hodnota špatného stavu. |

> [!NOTE]
> Port je stejný port jako nastavení HTTP back-end.

Výchozí kontroly zjistí pouze http://127.0.0.1:\<port\> k určení stavu. Pokud potřebujete nakonfigurovat test stavu na Přejít na vlastní adresu URL nebo změnit další nastavení, musíte použít vlastní testy paměti, jak je popsáno v následujících krocích:

## <a name="custom-health-probe"></a>Test vlastní stavu

Vlastní testy paměti umožňují podrobnější ovládat sledování stavu. Pokud používáte vlastní testy paměti, můžete nakonfigurovat interval testu, adresu URL a cesty k testování a kolik neúspěšných odpovědí tak, aby přijímal před označením fond back-end instance jako chybné.

### <a name="custom-health-probe-settings"></a>Nastavení testu vlastní stavu

Následující tabulka obsahuje definice pro vlastnosti test vlastní stavu.

| Vlastnost testu | Popis |
| --- | --- |
| Name (Název) |Název kontroly. Tento název se používá k odkazování na test v nastavení HTTP back-end. |
| Protocol (Protokol) |Protokol používaný k odesílání sonda. Tato kontrola používá protokol definované v nastavení HTTP back-end |
| Hostitel |Název hostitele k odeslání test. Platí jenom v případě více lokalit je nakonfigurovaná na aplikační bránu, v opačném případě použijte "127.0.0.1". Tato hodnota se liší od název hostitele virtuálního počítače. |
| Cesta |Relativní cesta kontroly. Platná cesta spustí z '/'. |
| Interval |Interval testu paměti v sekundách. Tato hodnota je časový interval mezi dvě po sobě jdoucích sondy. |
| Časový limit |Časový limit testu v sekundách. Pokud není platnou odpověď v rámci tento časový limit, tato kontrola je označeno jeho selhání.  |
| Prahová hodnota špatného stavu |Počet opakování testu. Back-end serverů je označena po počet po sobě jdoucích test selhání dosáhne prahová hodnota špatného stavu. |

> [!IMPORTANT]
> Pokud aplikace brána je nakonfigurovaná pro jednu lokalitu, ve výchozím nastavení hostitele název musí být zadán jako "127.0.0.1", pokud nebudou jinak nakonfigurovaná v vlastní test paměti.
> Pro referenci vlastní test paměti posílá \<protokol\>://\<hostitele\>:\<port\>\<cestu\>. Port používaný bude stejný port, jak jsou definovány v nastavení HTTP back-end.

## <a name="next-steps"></a>Další kroky
Po informací o sledování stavu Application Gateway, můžete nakonfigurovat [test vlastní stavu](application-gateway-create-probe-portal.md) na portálu Azure nebo [test vlastní stavu](application-gateway-create-probe-ps.md) pomocí prostředí PowerShell a modelu nasazení Azure Resource Manager.

[1]: ./media/application-gateway-probe-overview/appgatewayprobe.png
