---
title: "Správa Azure DDoS ochrany standardní pomocí portálu Azure | Microsoft Docs"
description: "Naučte se používat Azure DDoS ochrany standardní telemetrie v Azure monitorování zmírnit útok."
services: virtual-network
documentationcenter: na
author: kumudD
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 09/22/2017
ms.author: kumud
ms.openlocfilehash: 5c599b4cc867dbc9a081af3a081195b998f63954
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/11/2017
---
# <a name="manage-azure-ddos-protection-standard-using-the-azure-portal"></a>Správa Azure DDoS ochrany standardní pomocí portálu Azure

>[!IMPORTANT]
>Azure DDoS ochrany Standard (Ochrana proti útoku DDoS) se aktuálně ve verzi preview. Omezený počet prostředků Azure podpory ochrana proti útoku DDoS a vyberte počet oblastí. Budete muset [zaregistrovat pro službu](http://aka.ms/ddosprotection) během omezené preview získat ochrana proti útoku DDoS pro vaše předplatné povolený. Se vás kontaktovat tým Azure DDoS při registraci, který vás provede procesem povolování. Ochrana proti útoku DDoS je k dispozici v oblastech USA – Východ USA – západ a – Západ střední USA. Verzi Preview se vám neúčtují poplatky za používání služby.

Tento článek ukazuje, jak používat portál Azure k povolení ochrana proti útoku DDoS, zakažte ochranu DDoS a zmírnit útok pomocí telemetrie. 

Pokud ještě nemáte předplatné Azure, vytvořte si [bezplatný účet](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) před tím, než začnete.

## <a name="log-in-to-azure"></a>Přihlaste se k Azure.

Přihlaste se k webu Azure Portal na adrese http://portal.azure.com.

## <a name="enable-ddos-protection"></a>Povolit ochrana proti útoku DDoS

Povolení ochrany DDoS na nový nebo existující virtuální síť.

### <a name="create-a-new-virtual-network-and-enable-ddos-protection"></a>Vytvoření nové virtuální sítě a povolení ochrana proti útoku DDoS

1. Klikněte na tlačítko **Nový** v levém horním rohu webu Azure Portal.
2. Vyberte **sítě**a potom vyberte **virtuální sítě**.
3. Zadejte informace o virtuální síti. V části *ochrana proti útoku DDoS*, klikněte na tlačítko **povoleno**a potom klikněte na **vytvořit**.

    ![Vytvoření virtuální sítě](./media/ddos-protection-manage-portal/ddos-create-vnet.png)   

Upozornění s oznámením, že povolení ochrana proti útoku DDoS budou vám účtovány poplatky. Během preview se vám neúčtují žádné poplatky za ochrana proti útoku DDoS. Poplatky jsou vzniklé při obecné dostupnosti (GA) a zákazníci obdrží oznámení 30 dní před začátek poplatky a všeobecné

### <a name="enable-ddos-protection-on-an-existing-virtual-network"></a>Povolit ochrana proti útoku DDoS na existující virtuální síť 

1. Klikněte na tlačítko **virtuální sítě** v nabídce portálu Azure a potom vyberte virtuální síť.
2. Klikněte na tlačítko **ochrana proti útoku DDoS**, klikněte na tlačítko **povoleno** na *ochrana proti útoku DDoS* obrazovky a pak klikněte na tlačítko **Uložit**. 

Upozornění s oznámením, že povolení ochrana proti útoku DDoS budou vám účtovány poplatky. Během preview se vám neúčtují žádné poplatky za ochrana proti útoku DDoS. Poplatky jsou vzniklé při obecné dostupnosti (GA) a zákazníci obdrží oznámení 30 dní před začátek poplatky a všeobecné

## <a name="disable-ddos-protection"></a>Zakažte ochranu DDoS

1. Klikněte na tlačítko **virtuální sítě** v nabídce portálu Azure a potom vyberte virtuální síť.
2. Klikněte na tlačítko **ochrana proti útoku DDoS**, klikněte na tlačítko **zakázané** na *ochrana proti útoku DDoS* obrazovky a pak klikněte na tlačítko **Uložit**.

## <a name="configure-alerts-on-ddos-protection-metrics"></a>Nakonfigurujte upozornění na metriky ochrana proti útoku DDoS

Využití konfiguraci výstrah monitorování Azure, můžete vybrat všechny dostupné metriky ochrana proti útoku DDoS pro upozornění, když dojde active zmírnění při útoku. Když podmínky jsou splněny, zobrazí se výstrahy e-mailu na zadanou adresu.

1. Klikněte na tlačítko **monitorování**a potom klikněte na **metriky**.
2. Na *metriky* obrazovky, vyberte skupinu prostředků, typ prostředku **veřejnou IP adresu**a Azure veřejnou IP adresu.
3. Chcete-li nakonfigurovat e-mailové upozornění pro metriku, klikněte na tlačítko **klikněte na tlačítko Přidat výstrahu**. Na všechny metriky lze vytvořit e-mailové výstrahy, ale nejobvyklejší metrika **útoku DDoS pod nebo není**. Toto je logická hodnota, 1 nebo 0. A **1** znamená, můžete v útoku. A **0** znamená nejsou napadené.
4. Se má v případě útoku, nastavte metriku pro **útoku DDoS pod nebo není** a **podmínku, která má větší než nula (0) za posledních 5 minut**. Podobné výstrahy můžete nastavit pro jiné metriky.

    ![Konfigurace metriky](./media/ddos-protection-manage-portal/ddos-metrics.png)

    Během několika minut detekce útoku budete upozorněni, používání Azure monitorování metriky.

    ![Útok výstrahy](./media/ddos-protection-manage-portal/ddos-alert.png) 

Můžete si také přečíst informace o [konfigurace webhooky](../monitoring-and-diagnostics/insights-webhooks-alerts.md) a [aplikace logiky](../logic-apps/logic-apps-what-are-logic-apps.md) pro vytvoření výstrahy.

## <a name="configure-logging-on-ddos-protection-metrics"></a>Konfigurovat protokolování na metriky ochrana proti útoku DDoS

1. Klikněte na tlačítko **monitorování**a potom klikněte na **nastavení pro diagnostiku**.
2. Na *metriky* obrazovky, vyberte skupinu prostředků, typ prostředku **veřejnou IP adresu**a Azure veřejnou IP adresu.
3. Klikněte na tlačítko **zapněte diagnostiku shromažďovat následující data**.

K dispozici pro protokolování jsou tři možnosti:

- **Archiv na účet úložiště** – zapisuje protokoly do účtu úložiště.
- **Datový proud do centra událostí** – umožňuje protokol příjemce pro vyzvednutí protokolů pomocí centra událostí. To umožňuje integraci s Splunk nebo jiných systémů SIEM.
- **Odeslat k analýze protokolů** – zapisuje protokoly službě Analýza protokolů Azure OMS.

## <a name="use-ddos-protection-telemetry"></a>Pomocí telemetrie ochrana proti útoku DDoS

Telemetrie na útok je zajišťováno prostřednictvím Azure monitorování v reálném čase. Telemetrie je k dispozici pouze po dobu, která veřejnou IP adresu je pod zmírnění dopadů. Neuvidíte telemetrie před nebo po zmírnit útok.

1. Klikněte na tlačítko **monitorování**a potom klikněte na **metriky**. 
2. Na *metriky* obrazovky, vyberte skupinu prostředků, typ prostředku **veřejnou IP adresu**a Azure veřejnou IP adresu. Na levé straně obrazovky se zobrazí řadu dostupné metriky. Tyto metriky při výběru vykreslovacích v grafu Azure monitorování metriky na obrazovce Přehled. 

Metriky názvy představovat různých typů paketů a bajtů oproti paketů, základní konstrukce názvů značky na jednotlivé metriky následujícím způsobem:

- **Název značky zahozené (například příchozí pakety vyřadit DDoS):** počet paketů očistí ochrany systémem DDoS jejich vyřadit.
- **Název značky předané (např: příchozí pakety předají DDoS):** počet paketů, které jsou předávány DDoS systému do cílového umístění VIP – provoz, který nebyl nefiltruje.
- **Žádný název značky (např: příchozí pakety DDoS):** celkový počet paketů, které byly dodány do čištění dat systému – představující součet pakety vyřazen a předávat.

## <a name="next-steps"></a>Další kroky

- [Další informace o diagnostických protokolů Azure.](../monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs.md)
- [Analýza protokolů z úložiště Azure s analýzy protokolů](../log-analytics/log-analytics-azure-storage.md)
- [Začínáme s Event Hubs](../event-hubs/event-hubs-csharp-ephcs-getstarted.md)