---
title: "Škálování počtu instancí ručně nebo automaticky škálovat pomocí portálu Azure | Microsoft Docs"
description: "Zjistěte, jak škálování vašich služeb Azure."
author: anirudhcavale
manager: orenr
editor: 
services: monitoring-and-diagnostics
documentationcenter: monitoring-and-diagnostics
ms.assetid: 2397596a-071f-4d49-8893-bec5f735bd7b
ms.service: monitoring-and-diagnostics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/06/2017
ms.author: ancav
ms.openlocfilehash: 9ff52bd29644c7c585d2a85fcbe49e8800f6a035
ms.sourcegitcommit: 059dae3d8a0e716adc95ad2296843a45745a415d
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 02/09/2018
---
# <a name="scale-instance-count-manually-or-automatically"></a>Ruční nebo automatické škálování počtu instancí
V [portálu Azure](https://portal.azure.com/), můžete nastavit počet instancí služby ručně nebo můžete nastavit parametry, aby ji automaticky škálování na základě poptávky. To se obvykle označuje jako *škálovat* nebo *škálování v*.

Před škálování podle počtu instancí, je třeba zvážit, zda je ovlivňován škálování **cenová úroveň** kromě počet instancí. Jiné cenové úrovně může mít odlišné počty jader a paměti, a proto budou mít lepší výkon pro stejný počet instancí (což je *škálovat* nebo *snižovat*). Tento článek se zabývá konkrétně *škálování v* a *out*.

Je možné škálovat na portálu, a můžete také [REST API](https://msdn.microsoft.com/library/azure/dn931953.aspx) nebo [.NET SDK](http://www.nuget.org/packages/Microsoft.Azure.Management.Monitor) na ručně nebo automaticky upravit měřítko.

## <a name="scaling-manually"></a>Škálování ručně
1. V [portálu Azure](https://portal.azure.com/), klikněte na tlačítko **Procházet**, pak přejděte k prostředku, kterou chcete použít škálování, jako například **plán služby App Service**.
2. Klikněte na tlačítko **Nastavení > horizontální navýšení kapacity (plán služby App Service).**
3. V horní části **škálování** okno v **historie spouštění** můžete zobrazit historii akcí škálování služby.
   
    ![Okno škálování](./media/insights-how-to-scale/Insights_ScaleBladeDayZero.png)
   
   > [!NOTE]
   > V tomto grafu se zobrazí pouze akce, které provádí automatické škálování. Pokud ručně upravit počet instancí, změny se neprojeví v tomto grafu.
   > 
   > 
4. Můžete ručně upravit počet **instance** v **konfigurace** kartě.
5. Klikněte **Uložit** příkazu a vy budete škálovat na počet instancí téměř okamžitě.

## <a name="scaling-based-on-a-pre-set-metric"></a>Škálování podle předem nastavte metriku
Pokud chcete na metriky na základě počtu instancí automaticky upravit, vyberte metriku, které mají být v **škálovat podle** rozevíracího seznamu. Například **plán služby App Service** je možné škálovat podle **procento využití procesoru**.

1. Když vyberete metriky získáte jezdce nebo textová pole k zadání počtu instancí, kterou chcete škálovat mezi:
   
    ![Okno škálování s procento využití procesoru](./media/insights-how-to-scale/Insights_ScaleBladeCPU.png)
   
    Škálování bude trvat nikdy služby menší než nebo větší hranice, které jste nastavili, bez ohledu na to zatížení.
2. Druhý zvolte cílový rozsah pro metriku. Například, pokud jste zvolili **procento využití procesoru**, můžete nastavit cíl pro průměrné využití procesoru pro všechny instance ve službě. Horizontální navýšení kapacity se stane, když průměrné využití procesoru překračuje maximální počet, které definujete, podobně měřítka ve dojde vždy, když průměr procesoru klesne pod minimální.
3. Klikněte **Uložit** příkaz. Škálování zkontroluje každých několik minut, zkontrolujte, zda jsou v rozsahu instance a cíl pro vaše metriku. Když vaše služba přijímá další provoz, zobrazí se více instancí bez jakékoli akce.

## <a name="scale-based-on-other-metrics"></a>Škálování podle jiných metrik
Je možné škálovat podle metriky než přednastavení, které se zobrazují v **škálovat podle** rozevíracího seznamu a to i mají komplexní sadu možností horizontálního rozšíření kapacity a škálování v pravidlech.

### <a name="adding-or-changing-a-rule"></a>Přidání nebo změna pravidla
1. Vyberte **pravidla plánu a výkonu** v **škálovat podle** rozevírací: ![pravidla výkonu](./media/insights-how-to-scale/Insights_PerformanceRules.png)
2. Pokud jste dřív měli škálování, zobrazí se v zobrazení přesná pravidla, které jste měli.
3. Škálování na jiné metriky klikněte na základě **přidat pravidlo** řádek. Můžete také kliknout na jednu z existujících řádků chcete-li změnit z metriku, které jste dřív měli, kterou chcete škálovat podle metriky.
   ![Přidat pravidlo](./media/insights-how-to-scale/Insights_AddRule.png)
4. Nyní je nutné vybrat metriky, které chcete škálovat podle. Při výběru metriku, které jsou potřeba zvážit několik věcí:
   
   * *Prostředků* metriku pochází z. Obvykle to bude stejná jako prostředek, který se škálování. Pokud chcete škálovat podle hloubka fronty úložiště, je prostředek však fronty, kterou chcete škálovat podle.
   * *Název metriky* sám sebe.
   * *Časové agregace* metriky. To je, jak data je zkombinujte přes *doba trvání*.
5. Po výběru vaší metrika zvolíte prahovou hodnotu pro metriku a operátor. Řekněme například, vám může **větší než** **80 %**.
6. Zvolte akci, kterou chcete provést. Existuje několik různých typ akce:
   
   * Zvyšuje nebo snižuje podle – to bude přidat nebo odebrat **hodnotu** počet instancí, které definujete
   * Zvýšení nebo snížení procento – počet instancí se změní podle procento. Například by mohlo 25 **hodnotu** pole, a pokud jste měli aktuálně instancí 8, 2 by byl přidán.
   * Zvýšení nebo snížení k – to nastaví počet instancí **hodnotu** definujete.
7. Nakonec můžete nástrojů dolů – jak dlouho má toto pravidlo čekat po předchozí akce škálování akci škálování.
8. Po dokončení konfigurace pravidla dosáhl **OK**.
9. Po nakonfigurování všechna pravidla chcete nezapomeňte dosáhl **Uložit** příkaz.

### <a name="scaling-with-multiple-steps"></a>Škálování s více kroků
Výše uvedených příkladech jsou poměrně základní. Pokud chcete být agresivnější o škálování nahoru (nebo dolů), můžete i víc pravidel škálování pro stejnou metriku. Můžete například definovat dvě pravidla škálování na procento využití procesoru:

1. Škálování instancí 1, pokud procento využití procesoru je nad 60 %
2. Pokud je procento procesoru vyšší než 85 % škálovat 3 instancí

![Více pravidel škálování](./media/insights-how-to-scale/Insights_MultipleScaleRules.png)

S tímto pravidlem další Pokud zatížení překročí 85 % před akce škálování, zobrazí se dva další instance namísto jeden.

## <a name="scale-based-on-a-schedule"></a>Škálování podle plánu
Ve výchozím nastavení když vytvoříte pravidlo škálování bude vždy platit. Můžete uvidíte, že když kliknete na záhlaví profil:

![Profil](./media/insights-how-to-scale/Insights_Profile.png)

Můžete však mít agresivnější škálování během dne nebo týdne, než na víkendu. Může i vypnout služby zcela mimo pracovní dobu.

1. Chcete-li to provést v profilu máte, vyberte **opakování** místo **vždy,** a vyberte dobu, které chcete použít profil.
2. Například profil, která se použije během do týdne v **dní** zrušte zaškrtnutí políčka rozevírací **sobota** a **neděle**.
3. Pokud chcete, aby profil, který se použije během na denní, nastavte **počáteční čas** na dobu během dne, který chcete spustit v.
   
    ![Výchozí opakování](./media/insights-how-to-scale/Insights_ProfileRecurrence.png)
4. Klikněte na **OK**.
5. Dále musíte přidat profil, který chcete použít v jinou dobu. Klikněte **přidat profil** řádek.
    ![Zaměstnání](./media/insights-how-to-scale/Insights_ProfileOffWork.png)
6. Název vaší nové, druhý profil, například může zavolat **vypnout pracovní**.
7. Potom vyberte **opakování** znovu a vyberte požadovaný instance počet rozsah během této doby.
8. Jako výchozí profil, zvolte **dní** chcete použít, tento profil a **počáteční čas** během dne.
   
   > [!NOTE]
   > Škálování použije pravidla úspory letního času pro podle toho, co **časové pásmo** vyberete. Však během letní čas posunem od standardu UTC zobrazí posun základní časové pásmo, není posun úspory UTC letního času.
   > 
   > 
9. Klikněte na **OK**.
10. Teď je potřeba přidat libovolnou pravidla, které chcete použít během druhý profil. Klikněte na tlačítko **přidat pravidlo**, a poté může vytvořit stejného pravidla máte během výchozí profil.
    
    ![Přidat pravidlo vypnuto pracovní](./media/insights-how-to-scale/Insights_RuleOffWork.png)
11. Je nutné vytvořit i pravidla pro škálování a škálovat v or else během profil počet instancí bude pouze růst (nebo snížit).
12. Nakonec klikněte na **Uložit**.

## <a name="next-steps"></a>Další postup
* [Monitorování služby metriky](insights-how-to-customize-monitoring.md) zkontrolovat služby je k dispozici a dobře reagovaly.
* [Povolit monitorování a Diagnostika](insights-how-to-use-diagnostics.md) ke shromažďování metrik podrobné vysoká frekvence vaší služby.
* [Přijímejte oznámení o výstrahách](insights-receive-alert-notifications.md) vždy, když nastanou provozní události nebo když metriky překročí prahovou hodnotu.
* [Monitorování výkonu aplikací](../application-insights/app-insights-azure-web-apps.md) Pokud chcete zjistit, přesně jak kód provádí v cloudu.
* [Zobrazení událostí a protokolu aktivity](insights-debugging-with-events.md) další vše, co se stalo ve službě.
* [Sledování dostupnosti a odezvy žádné webové stránce](../application-insights/app-insights-monitor-web-app-availability.md) s Application Insights, můžete zjistit, pokud stránka je vypnutý.

