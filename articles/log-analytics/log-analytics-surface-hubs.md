---
title: "Monitorování Surface Hubů s Azure Log Analytics | Microsoft Docs"
description: "Použijte Surface Hub řešení ke sledování stavu Surface Huby a pochopit, jak se právě používají."
services: log-analytics
documentationcenter: 
author: bandersmsft
manager: carmonm
editor: 
ms.assetid: 8b4e56bc-2d4f-4648-a236-16e9e732ebef
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/07/2017
ms.author: banders
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: b6ecd0d09589fec85c1633f528afc1165c346b7f
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/11/2017
---
# <a name="monitor-surface-hubs-with-log-analytics-to-track-their-health"></a>Monitorování Surface Huby se analýzy protokolů sledovat jejich stav

![Surface Hub – symbol](./media/log-analytics-surface-hubs/surface-hub-symbol.png)

Tento článek popisuje, jak můžete použít řešení Surface Hub v analýzy protokolů monitorování Microsoft Surface Hub zařízení s Microsoft Operations Management Suite (OMS). Analýzy protokolů vám pomůže sledovat stav Surface Huby stejně jako pochopit, jak se právě používají.

Každý Surface Hub má Microsoft Monitoring Agent nainstalována. Jeho prostřednictvím, může odesílat data z Surface Hub k OMS agenta. Soubory protokolu se načítají z Surface Huby a jsou pak posílají se na službu OMS. Problémy, jako je offline, servery kalendáře nesynchronizuje, nebo pokud účet zařízení se nemůže přihlásit do Skype jsou zobrazeny v OMS na řídicím panelu Surface Hub. Pomocí dat v řídicím panelu můžete identifikovat zařízení, která neběží nebo které jsou s jiné problémy a potenciálně použít opravy zjištěných problémů.

## <a name="installing-and-configuring-the-solution"></a>Instalace a konfigurace řešení
Použijte následující informace k instalaci a konfiguraci řešení. Abyste mohli spravovat Surface Huby z Microsoft Operations Management Suite (OMS), budete potřebovat následující:

* K platnému předplatnému [OMS](http://www.microsoft.com/oms).
* [OMS předplatné](https://azure.microsoft.com/pricing/details/log-analytics/) úroveň, která bude podporovat počet zařízení, které chcete monitorovat. OMS ceny se liší v závislosti na tom, kolik zařízení jsou zaregistrovaná a kolik dat se procesy. Je potřeba vzít v úvahu při plánování zavádění řešení Surface Hub.

Dále budete buď přidat předplatné OMS do svého předplatného Microsoft Azure nebo vytvořit nový pracovní prostor přímo přes portál OMS. Podrobné pokyny pro použití metody je v [začít pracovat s analýzy protokolů](log-analytics-get-started.md). Až nastavíte odběr OMS existují dva způsoby, jak zaregistrovat svoje zařízení Surface Hub:

* Automaticky prostřednictvím služby Intune
* Ručně pomocí **nastavení** na vašem zařízení Surface Hub.

## <a name="set-up-monitoring"></a>Nastavte monitorování
Můžete sledovat stav a aktivitu centra prostor analýzy protokolů v OMS. Surface Hub v OMS můžete zaregistrovat pomocí Intune nebo místně pomocí **nastavení** na Surface Hub.

## <a name="connect-surface-hubs-to-oms-through-intune"></a>Připojit k OMS prostřednictvím Intune Surface Hubů
Pro pracovní prostor OMS, která bude spravovat Surface Huby budete potřebovat ID pracovního prostoru a klíč pracovního prostoru. Můžete získat z portálu OMS.

Intune je produkt společnosti Microsoft, která umožňuje centrálně spravovat OMS nastavení konfigurace, která se použijí na jeden nebo více zařízení. Postupujte podle těchto kroků pro konfiguraci zařízení prostřednictvím služby Intune:

1. Přihlášení k Intune.
2. Přejděte na **nastavení** > **připojené zdroje**.
3. Vytvořte nebo upravte zásadu na základě šablony Surface Hub.
4. Přejděte do části OMS (Statistika provozu Azure) zásad a přidat *ID pracovního prostoru* a *klíč pracovního prostoru* zásad.
5. Zásadu uložte.
6. Přidružení zásad k příslušné skupiny zařízení.

   ![Zásady Intune](./media/log-analytics-surface-hubs/intune.png)

Intune pak synchronizuje OMS nastavení zařízení v cílové skupině, je zaregistrujete v pracovním prostoru OMS.

## <a name="connect-surface-hubs-to-oms-using-the-settings-app"></a>Surface Huby se připojit k OMS pomocí nastavení aplikace
Pro pracovní prostor OMS, která bude spravovat Surface Huby budete potřebovat ID pracovního prostoru a klíč pracovního prostoru. Můžete získat z portálu OMS.

Pokud nepoužíváte Intune ke správě prostředí, můžete zaregistrovat zařízení ručně pomocí **nastavení** na každý Surface Hub:

1. Surface Hub, otevřete **nastavení**.
2. Zadejte pověření správce zařízení po zobrazení výzvy.
3. Klikněte na tlačítko **toto zařízení**a v části **monitorování**, klikněte na tlačítko **konfigurovat nastavení OMS**.
4. Vyberte **povolit sledování**.
5. V dialogovém okně Nastavení OMS, zadejte **ID pracovního prostoru** a zadejte **klíč pracovního prostoru**.  
   ![nastavení](./media/log-analytics-surface-hubs/settings.png)
6. Klikněte na tlačítko **OK** k dokončení konfigurace.

Zobrazení potvrzení o tom, zda byla úspěšně použita konfigurace OMS do zařízení. Pokud byl, zobrazí se zpráva s oznámením, že agent úspěšně připojen ke službě OMS. Zařízení pak spustí odesílání dat do OMS, kde můžete zobrazit a pracovat.

## <a name="monitor-surface-hubs"></a>Monitorování Surface Hubů
Monitorování Surface Huby pomocí OMS je mnohem jako monitorování jiných zaregistrovaných zařízení.

1. Přihlaste se k portálu OMS.
2. Přejděte na řídicí panel řešení pack Surface Hub.
3. Zobrazí se stav vašeho zařízení.

   ![Surface Hub řídicí panel](./media/log-analytics-surface-hubs/surface-hub-dashboard.png)

Můžete vytvořit [výstrahy](log-analytics-alerts.md) založené na protokolu hledání existujících nebo vlastních. Pomocí dat, která OMS shromažďuje z Surface Huby, můžete vyhledat problémy a upozornění na podmínky, které definujete pro vaše zařízení.

## <a name="next-steps"></a>Další kroky
* Použití [hledání přihlásit analýzy protokolů](log-analytics-log-searches.md) na podrobnější data Surface Hub.
* Vytvoření [výstrahy](log-analytics-alerts.md) upozornění v případě problémů, ke kterým dochází u Surface Huby.
