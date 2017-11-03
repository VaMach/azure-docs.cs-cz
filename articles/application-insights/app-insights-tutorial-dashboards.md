---
title: "Vytvořit vlastní řídicí panely ve službě Azure Application Insights | Microsoft Docs"
description: "Kurz k vytvoření vlastních řídicích panelů klíčového ukazatele výkonu pomocí služby Azure Application Insights."
keywords: 
services: application-insights
author: mrbullwinkle
ms.author: mbullwin
ms.date: 09/20/2017
ms.service: application-insights
ms.custom: mvc
ms.topic: tutorial
manager: carmonm
ms.openlocfilehash: 0d2f98ca2fb39289b2916ddd24590924856507d6
ms.sourcegitcommit: e462e5cca2424ce36423f9eff3a0cf250ac146ad
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/01/2017
---
# <a name="create-custom-kpi-dashboards-using-azure-application-insights"></a>Vytvořit vlastní řídicí panely klíčového ukazatele výkonu pomocí služby Azure Application Insights

Můžete vytvořit více řídicí panely na portálu Azure, každý zahrnout dlaždice v různých skupinách prostředků a předplatná vizualizace dat z více prostředků Azure.  Budete moct připnout různých grafů a zobrazení z Azure Application Insights k vytvoření vlastní řídicí panely, které poskytují úplný přehled o stavu a výkonu vaší aplikace.  Tento kurz vás provede procesem vytvoření vlastní řídicí panel, který obsahuje více typů data a vizualizace z Azure Application Insights.  Získáte informace o těchto tématech:

> [!div class="checklist"]
> * Vytvořte vlastní řídicí panel v Azure
> * Přidat dlaždice z Galerie dlaždice
> * Přidání standardní metriky ve službě Application Insights na řídicí panel 
> * Přidejte vlastní metriky graf Application Insights na řídicí panel
> * Přidat výsledků dotazu Analytics na řídicí panel 



## <a name="prerequisites"></a>Požadavky

Pro absolvování tohoto kurzu potřebujete:

- Nasazení aplikace .NET do Azure a [povolit Application Insights SDK](app-insights-asp-net.md). 

## <a name="log-in-to-azure"></a>Přihlaste se k Azure.
Přihlaste se k portálu Azure v [https://portal.azure.com](https://portal.azure.com).

## <a name="create-a-new-dashboard"></a>Vytvořit nový řídicí panel
Jednoho řídicího panelu může obsahovat prostředky z víc aplikací, skupiny prostředků a předplatná.  Tento kurz začněte vytvořením nového řídicího panelu pro vaši aplikaci.  

2.  V hlavním okně portálu, vyberte **novým řídicím panelem**.

    ![Nový řídicí panel](media/app-insights-tutorial-dashboards/new-dashboard.png)

3. Zadejte název pro řídicí panel.
4. Podívejte se na **dlaždice Galerie** pro celou řadu dlaždice, které můžete přidat do řídicího panelu.  Kromě přidávat dlaždice z galerie můžete připnout grafů a dalšími zobrazeními přímo z Application Insights na řídicí panel.
5. Vyhledejte **Markdownu** dlaždici a přetáhněte ji na řídicí panel.  Tuto dlaždici umožňuje přidat text ve formátu v markdownu, který je ideální pro přidání popisný text do řídicího panelu.
6. Přidat text na dlaždici vlastnosti a jeho velikost na plátně řídicího panelu.
    
    ![Upravit dlaždice markdownu](media/app-insights-tutorial-dashboards/edit-markdown.png)

6. Klikněte na tlačítko **provádí přizpůsobení** v horní části obrazovky ukončíte dlaždice přizpůsobení režimu a potom **publikovat změny** uložte provedené změny.

    ![Řídicí panel se dlaždice markdownu](media/app-insights-tutorial-dashboards/dashboard-01.png)


## <a name="add-health-overview"></a>Přidat přehled stavu
Řídicí panel se právě statický text není velmi zajímavé, takže teď přidejte dlaždice z Application Insights se mají zobrazit informace o vaší aplikaci.  Můžete přidat Application Insights dlaždice z Galerie dlaždice, nebo budete moct připnout přímo z obrazovky Application Insights.  To umožňuje nakonfigurovat grafy a zobrazení, které jste již obeznámeni s před jejich Připnutí na řídicí panel.  Začněte přidáním přehled standardní stavu pro vaši aplikaci.  To vyžaduje žádná konfigurace a umožňuje minimální přizpůsobení v řídicím panelu.


1. Vyberte **Application Insights** v nabídce Azure a pak vyberte svou aplikaci.
2. V **časová osa přehled**, vyberte příslušnou kontextovou nabídku a klikněte na **připnout na řídicí panel**.  Tento postup přidá na dlaždici na poslední řídicí panel, který měla zobrazení.  

    ![Časová osa Přehled kódu PIN](media/app-insights-tutorial-dashboards/pin-overview-timeline.png)
 
3. V horní části obrazovky klikněte na tlačítko **zobrazení řídicího panelu** vrátit na řídicí panel.
4. Časová osa přehled je nyní přidána do řídicího panelu.  Klikněte na tlačítko a přetáhněte ji do pozice a potom klikněte na **provádí přizpůsobení** a **publikovat změny**.  Řídicí panel má nyní dlaždici některé užitečné informace.

    ![Řídicí panel se časová osa – přehled](media/app-insights-tutorial-dashboards/dashboard-02.png)



## <a name="add-custom-metric-chart"></a>Přidat vlastní metriky grafu
**Metriky** panel umožňuje graf metriky shromážděné pomocí Application Insights časem pomocí volitelné filtry a seskupení.  Podobně jako všechny ostatní ve službě Application Insights můžete přidat tento graf na řídicí panel.  Musíte nejdřív provést trochu přizpůsobení.

1. Vyberte **Application Insights** v nabídce Azure a pak vyberte svou aplikaci.
1. Vyberte **metriky**.  
2. Prázdný grafu již byly vytvořeny a se zobrazí výzva k přidání metriky.  Do grafu přidat metriky a volitelně přidejte filtr a seskupení.  Následující příklad ukazuje počet požadavků serveru seskupené podle úspěch.  To dává spuštěné zobrazení úspěšné a neúspěšné žádosti.

    ![Přidat metrika](media/app-insights-tutorial-dashboards/metrics-chart.png)

4. Vyberte příslušnou kontextovou nabídku pro graf a vyberte **připnout na řídicí panel**.  Poslední řídicí panel, kterou jste pracovali přidá zobrazení.

    ![Graf metriky kódu PIN](media/app-insights-tutorial-dashboards/pin-metrics-chart.png)

3. V horní části obrazovky klikněte na tlačítko **zobrazení řídicího panelu** vrátit na řídicí panel.

4. Časová osa přehled je nyní přidána do řídicího panelu.  Klikněte na tlačítko a přetáhněte ji do pozice a potom klikněte na **provádí přizpůsobení** a potom **publikovat změny**. 

    ![řídicí panel se metriky](media/app-insights-tutorial-dashboards/dashboard-03.png)


## <a name="metrics-explorer"></a>Průzkumníku metrik
**Průzkumníku metrik** je podobná metriky, i když umožňuje výrazně další přizpůsobení při přidání na řídicí panel.  Který použijete k graf vaše metriky závisí na konkrétní předvoleb a požadavky.

1. Vyberte **Application Insights** v nabídce Azure a pak vyberte svou aplikaci.
1. Vyberte **Průzkumníku metrik**. 
2. Kliknutím na upravit graf a vyberte jeden nebo více metriky a volitelně podrobnou konfiguraci.  V příkladu se zobrazí spojnicový graf sledování doba odezvy průměrná stránky.
3. Kliknutím na ikonu připnutí v horním pravém přidat graf na řídicí panel a poté ji přetáhněte do pozice.

    ![Průzkumníku metrik](media/app-insights-tutorial-dashboards/metrics-explorer.png)

4. Na dlaždici Průzkumníku metrik umožňuje další přizpůsobení, jakmile se přidá na řídicí panel.  Klikněte pravým tlačítkem na dlaždici a vyberte **upravit název** přidejte vlastní název.  Pokračujte a provádět další úpravy, pokud chcete.

    ![Řídicí panel se Průzkumníku metrik](media/app-insights-tutorial-dashboards/dashboard-04a.png)

5. Nyní máte Průzkumníku metrik grafu přidat na řídicí panel.

    ![Řídicí panel se Průzkumníku metrik](media/app-insights-tutorial-dashboards/dashboard-04.png)

## <a name="add-analytics-query"></a>Přidání dotazu Analytics
Azure Application Insights Analytics poskytuje plnohodnotný dotazovací jazyk, který umožňuje analýzu všech dat shromážděných Application Insights.  Stejně jako grafy a jiné zobrazení můžete přidat výstup dotaz Analytics na řídicí panel.   

Vzhledem k tomu, že Azure aplikace Statistika Analytics je samostatná služba, budete muset sdílet tento řídicí panel pro tak, aby obsahovala dotaz Analytics.  Když sdílíte Azure řídicí panel, publikovat jako prostředek služby Azure, který může být k dispozici pro ostatní uživatele a prostředky.  

1. V horní části obrazovky řídicí panel, klikněte na tlačítko **sdílené složky**.

    ![publikovat řídicí panel](media/app-insights-tutorial-dashboards/publish-dashboard.png)

2. Zachovat **název řídicího panelu** stejné a vyberte **název odběru** sdílení řídicího panelu.  Klikněte na **Publikovat**.  Řídicí panel je teď dostupná k dalším službám a předplatným.  Volitelně můžete definovat konkrétní uživatele, kteří mají mít přístup k řídicímu panelu.
1. Vyberte **Application Insights** v nabídce Azure a pak vyberte svou aplikaci.
2. Klikněte na tlačítko **Analytics** v horní části obrazovky, otevřete portál Analytics.

    ![Zahájení analýzy](media/app-insights-tutorial-dashboards/start-analytics.png)

3. Zadejte následující dotaz, který vrátí 10 nejžádanějších stránek a jejich počtu žádostí o:

    ```
    requests
    | summarize count() by name
    | sort by count_ desc
    | take 10 
    ```

4. Klikněte na tlačítko **přejděte** ověřit výsledky dotazu.
5. Klikněte na ikonu Připnutí a vyberte název řídicího panelu.  Z důvodu, že má tuto možnost můžete vybrat řídicí panel na rozdíl od předchozí kroky, kde byl použit poslední řídicí panel je, protože konzole Analytics je samostatná služba a je nutné vybrat ze všech dostupných sdílených řídicí panely.

    ![Dotaz analýzy kódu PIN](media/app-insights-tutorial-dashboards/analytics-pin.png)

5. Než přejdete zpět na řídicí panel, přidejte další dotaz, ale tentokrát vykreslit ho jako graf tak, aby zobrazila různé způsoby, jak vizualizovat dotaz analýzy v řídicím panelu.  Začněte s následující dotaz, který shrnuje nejlepší 10 operace s většina výjimky.

    ```
    exceptions
    | summarize count() by operation_Name
    | sort by count_ desc
    | take 10 
    ```

6. Vyberte **grafu** a poté změňte na **prstencový** k vizualizaci výstup.

    ![Graf analýzy](media/app-insights-tutorial-dashboards/analytics-chart.png)

6. Kliknutím na ikonu Připnutí připnete graf na řídicí panel a tentokrát vyberte odkaz se vraťte do řídicího panelu.
4. Výsledky dotazů jsou nyní přidána do řídicího panelu ve formátu, který jste vybrali.  Klikněte na tlačítko a přetáhněte každý do pozice a potom klikněte na **dokončení úprav**.
5. Klikněte pravým tlačítkem na každý z dlaždice a vyberte **upravit název** jim dát popisný název.

    ![Řídicí panel s Analytics](media/app-insights-tutorial-dashboards/dashboard-05.png)

5. Klikněte na tlačítko **publikovat změny** k provedení změn na váš řídicí panel, který nyní zahrnuje celou řadu grafy a vizualizace z Application Insights.


## <a name="next-steps"></a>Další kroky
Teď, když jste se naučili jak vytvořit vlastní řídicí panely, podívejte se na zbytek včetně Případová studie dokumentaci Application Insights.

> [!div class="nextstepaction"]
> [Hloubkové diagnostiky](app-insights-devops.md)