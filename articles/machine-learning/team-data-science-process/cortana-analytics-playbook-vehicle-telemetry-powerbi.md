---
title: "Řídicí panel Power BI pro vehicle stavu a řídí zvyklosti - Azure | Microsoft Docs"
description: "Pomocí možnosti Cortana Intelligence získat přehledy v reálném čase a prediktivní na vehicle stavu a řídí zvyklosti."
services: machine-learning
documentationcenter: 
author: bradsev
manager: jhubbard
editor: cgronlun
ms.assetid: aaeb29a5-4a13-4eab-bbf1-885690d86c56
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/16/2016
ms.author: bradsev
ms.openlocfilehash: 39be936520d62cb1c1c28de9bd72f8f489166082
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/11/2017
---
# <a name="vehicle-telemetry-analytics-solution-template-power-bi-dashboard-setup-instructions"></a>Vehicle telemetrie analytics řešení šablony řídicí panel Power BI pokyny pro instalaci
To **nabídky** odkazy na kapitoly v této scénářem. 

[!INCLUDE [cap-vehicle-telemetry-playbook-selector](../../../includes/cap-vehicle-telemetry-playbook-selector.md)]

Jak dealerům car, automobilů výrobců a pojištění společností můžete využít možnosti Cortana Intelligence a získáte přehled o v reálném čase a prediktivní na vehicle stavu a řízení zvyklosti na jednotce vylepšení v oblasti zákazníka zkušenosti, výzkumu a vývoje a marketingových kampaní, umožňující prezentovat řešení Vehicle Telemetrie analýzy. Tento dokument obsahuje podrobné pokyny, jak můžete nakonfigurovat Power BI sestavy a řídicí panel po je řešení nasazeno v rámci vašeho předplatného. 

## <a name="prerequisites"></a>Požadavky
1. Nasazení [Telemetrie Analytics](https://gallery.cortanaintelligence.com/Solution/5bdb23f3abb448268b7402ab8907cc90) řešení  
2. [Nainstalujte Microsoft Power BI Desktop](http://www.microsoft.com/download/details.aspx?id=45331)
3. [Předplatné](https://azure.microsoft.com/pricing/free-trial/). Pokud nemáte předplatné Azure, Začínáme s Azure bezplatné předplatné
4. Účet Microsoft Power BI

## <a name="cortana-intelligence-suite-components"></a>Cortana Intelligence Suite součásti
Jako součást šablona řešení Analytics Telemetrie Vehicle jsou tyto služby Cortana Intelligence nasazené v rámci vašeho předplatného.

* **Centra událostí** pro příjem miliony událostí vehicle telemetrická data do Azure.
* **Stream Analytics** pro získání přehledy v reálném čase na vehicle stavu a přetrvává tato data do dlouhodobého úložiště pro širší batch analýzu.
* **Strojového učení** pro zjišťování anomálií v reálném čase a dávkové zpracování a získáte přehled o prediktivní.
* **HDInsight** je využít k transformaci dat ve velkém měřítku
* **Objekt pro vytváření dat** zpracovává orchestration, plánování, správy prostředků a monitorování dávkové zpracování kanálu.

**Power BI** toto řešení poskytuje bohaté řídicí panel pro data v reálném čase a vizualizací prediktivní analýzy. 

Toto řešení využívá dvou různých zdrojů dat.: **Simulated vehicle signály a diagnostiky datovou sadu** a **vehicle katalogu**.

Vehicle telematika jsme je součástí tohoto řešení. Vysílá diagnostické informace a signály odpovídající stav nástroj a řídí vzor k danému bodu v čase. 

Vehicle katalog je obsahující VIN odkaz na datovou sadu pro mapování modelu

## <a name="power-bi-dashboard-preparation"></a>Příprava Power BI řídicí panel
### <a name="setup-power-bi-real-time-dashboard"></a>Instalační program v reálném čase řídicí panel Power BI

**Spusťte aplikaci v reálném čase řídicí panel** po dokončení nasazení, postupujte podle pokynů operaci ruční

* Stažení aplikace řídicího panelu v reálném čase RealtimeDashboardApp.zip a rozbalte ho.
*  V rozbalené složce otevřete konfigurační soubor aplikace RealtimeDashboardApp.exe.config, nahraďte appSettings pro Eventhub, úložiště objektů Blob a ML připojení služby s hodnotami v pokynech operaci ruční a uložte změny.
* Spusťte aplikaci RealtimeDashboardApp.exe. Okno přihlášení bude překryvné, zadejte svoje platné přihlašovací údaje PowerBI a klikněte na **přijmout** tlačítko. Aplikace se pak spustí ke spuštění.

   ![Přihlaste se do Power BI](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/5-sign-into-powerbi.png)
   
   ![Power BI Dashboard oprávnění](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/6-powerbi-dashboard-permissions.png)

* Přihlašovací údaje pro web PowerBI a vytvořit řídicí panel v reálném čase.

Nyní jste připraveni ke konfiguraci řídicí panel Power BI s bohatých vizualizací k získání v reálném čase a zvyklosti prediktivní Statistika na vehicle stavu a řídí. Trvá přibližně 45 minut za hodinu vytváření všech sestav a konfiguraci řídicího panelu. 

### <a name="configure-power-bi-reports"></a>Konfigurace sestav Power BI
V reálném čase sestavy a řídicí panel trvat asi 30 – 45 minut na dokončení. Přejděte do [http://powerbi.com](http://powerbi.com) a přihlaste se.

![Přihlaste se do Power BI](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/6-1-powerbi-signin.png)

Nová datová sada se generuje ve službě Power BI. Klikněte **ConnectedCarsRealtime** datovou sadu.

![Vybráno připojené v reálném čase datové sady automobilů](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/7-select-connected-cars-realtime-dataset.png)

Uložit pomocí prázdné sestavy **Ctrl + s**.

![Uložit prázdné sestavy](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/8-save-blank-report.png)

Zadejte název sestavy *Vehicle Telemetrie analýzy v reálném čase - sestavy*.

![Zadejte název sestavy](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/9-provide-report-name.png)

## <a name="real-time-reports"></a>Sestavy v reálném čase
V tomto řešení jsou tři sestavy v reálném čase:

1. Vozidel v operaci
2. Vozidel údržby
3. Statistiky vozidel stavu

Můžete nakonfigurovat všechny tři v reálném čase sestavy nebo zastavení po jakékoli fázi a pokračujte v další části Konfigurace batch sestavy. Doporučujeme, abyste vytvořili všechny tři sestav můžete znázorňovat úplné statistiky v reálném čase cesty řešení.  

### <a name="1-vehicles-in-operation"></a>1. Vozidel v operaci
Klikněte dvakrát na **stránka 1** a přejmenujte jej na "Vozidel v operaci"  
    ![Připojených vozidel v operaci aut-](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4a.png)  

Vyberte **vin** pole z **pole** a vyberte typ vizualizace jako **"Kartou"**.  

Karta vizualizace se vytvoří, jak je znázorněno na obrázku.  
    ![Připojené aut - vyberte vin](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4b.png)

Klikněte na prázdnou oblast přidat nové vizualizace.  

Vyberte **města** a **vin** z pole. Změnit vizualizace k **"Map"**. Přetáhněte **vin** v oblasti hodnoty. Přetáhněte **města** z pole **legendy** oblasti.   
    ![Připojené aut – karta vizualizace](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4c.png)

Vyberte **formátu** část z **vizualizace**, klikněte na tlačítko **název** a změňte **Text** k **"Vozidel v operaci podle města"**.  
    ![Připojených vozidel v operaci podle města aut-](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4d.png)   

Poslední vizualizace vypadá, jak je znázorněno na obrázku.    
    ![Připojené aut - poslední vizualizace](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4e.png)

Klikněte na prázdnou oblast přidat nové vizualizace.  

Vyberte **města** a **vin**, změnit typ vizualizace na **skupinový sloupcový graf**. Ujistěte se, **města** pole **osy oblasti** a **vin** v **hodnotu oblasti**  

Graf řazení podle **"Počet vin"**  
    ![Připojené aut - počet vin](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4f.png)  

Změna grafu **název** k **"Vozidel v operaci podle města"**  

Klikněte na tlačítko **formátu** části a pak vyberte **Data barvy**, klikněte na tlačítko **"Na"** k **Zobrazit vše**  
    ![Připojené aut – zobrazit všechny Data barvy](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4g.png)  

Změníte barvu jednotlivých města kliknutím na ikonu barev.  
    ![Připojené aut - Změna barev](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4h.png)  

Klikněte na prázdnou oblast přidat nové vizualizace.  

Vyberte **skupinový sloupcový graf** vizualizace z vizualizace, přetáhněte **města** pole **osy** oblasti **modelu** v **legendy** oblasti a **vin** v **hodnotu** oblasti.  
    ![Připojené aut - skupinový sloupcový graf](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4i.png)  
    ![Připojené aut - vykreslování](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4j.png)

Změna uspořádání všechny vizualizace na této stránce, jak je znázorněno na obrázku.  
    ![Připojené aut - vizualizace](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4k.png)

Úspěšně jste nakonfigurovali v reálném čase sestavy "Vozidel v operaci". Můžete přejít k vytvoření další sestavy v reálném čase nebo zde zastavte a konfigurujte řídicího panelu. 

### <a name="2-vehicles-requiring-maintenance"></a>2. Vozidel údržby
Klikněte na tlačítko ![přidat](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4add.png) přidat nové sestavy, přejmenujte ji na **"Vozidel nutnosti Údržba"**

![Připojené aut - vozidel údržby](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4l.png)  

Vyberte **vin** pole a změnit typ vizualizace na **karty**.  
    ![Připojené aut – karta Vin vizualizace](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4m.png)  

Máme pole s názvem "MaintenanceLabel" v datové sadě. Toto pole může mít hodnotu "0" nebo "1"." Je nastavena podle modelu Azure Machine Learning zřízené v rámci řešení a integraci s cestou v reálném čase. Hodnotu "1" označuje, že že vehicle vyžaduje údržbu. 

Chcete-li přidat **úrovně stránky** filtr zobrazující vozidel data, která jsou nutnosti údržby: 

1. Přetáhněte **"MaintenanceLabel"** pole do **stránky úroveň filtry**.  
   ![Připojené aut - filtry úrovně stránky](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4n1.png)  
2. Klikněte na tlačítko **základní filtrování** nabídky nachází v dolní části MaintenanceLabel stránky úroveň filtru.  
   ![Připojené aut – základní filtrování](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4n2.png)  
3. Nastavte ho na hodnotu filtru **"1"**    
   ![Připojené aut - hodnota filtru](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4n3.png)  

Klikněte na prázdnou oblast přidat nové vizualizace.  

Vyberte **skupinový sloupcový graf** z vizualizace  
![Připojené aut – karta Vind vizualizace](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4o.png)  
![Připojené aut - skupinový sloupcový graf](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4p.png)

Přetáhněte pole **modelu** do **osy** oblasti **Vin** k **hodnotu** oblasti. Seřaďte vizualizace podle **počet vin**.  Změna grafu **název** k **"Vozidel nutnosti údržby modelem"**  

Přetáhněte **vin** polí do **sytost barev** u **pole** ![pole](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4field.png) části **vizualizace** karta  
![Připojené aut - sytost barev](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4q.png)  

Změna **Data barvy** v vizualizace z **formát** části  
Změnit barvu minimální: **F2C812**  
Změnit barvu maximální: **FF6300**  
![Připojené aut - změny barev](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4r.png)  
![Připojené aut - nové Vizualizační barvy](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4s.png)  

Klikněte na prázdnou oblast přidat nové vizualizace.  

Vyberte **Clustered sloupcový graf** z vizualizace, přetáhněte **vin** pole do **hodnotu** oblasti, přetáhněte **města** pole do **osy** oblasti. Graf řazení podle **"Počet vin"**. Změna grafu **název** k **"Vozidel údržby podle města"**   
![Připojené aut - vozidel údržby podle města](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4t.png)  

Klikněte na prázdnou oblast přidat nové vizualizace.  

Vyberte **více řádků karty** vizualizace z vizualizace, přetáhněte **modelu** a **vin** do **pole** oblasti.  
![Připojené aut – karta více řádků](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4u.png)    

Změna uspořádání všechny vizualizaci, poslední sestava vypadá takto:  
![Připojené aut – karta více řádků](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4v.png)  

Úspěšně jste nakonfigurovali v reálném čase sestavy "Vozidel nutnosti údržby". Můžete přejít k vytvoření další sestavy v reálném čase nebo zde zastavte a konfigurujte řídicího panelu. 

### <a name="3-vehicles-health-statistics"></a>3. Statistiky vozidel stavu
Klikněte na tlačítko ![přidat](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4add.png) Pokud chcete přidat novou sestavu, přejmenujte ji na **"Vozidel stavu statistika"**  

Vyberte **měřidla** vizualizace z vizualizace, přetáhněte **rychlost** pole do **hodnotu, hodnotu minimální, maximální hodnota** oblasti.  
![Připojené aut – karta více řádků](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4w.png)  

Změňte výchozí agregace **rychlost** v **hodnotu oblasti** k **průměrná** 

Změňte výchozí agregace **rychlost** v **minimální oblasti** k **minimální**

Změňte výchozí agregace **rychlost** v **maximální oblasti** k **maximální**

![Připojené aut – karta více řádků](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4x.png)  

Přejmenujte **název měřidla** k **"Průměrná rychlost"** 

![Připojené aut - měřidla](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4y.png)  

Klikněte na prázdnou oblast přidat nové vizualizace.  

Podobně přidat **měřidla** pro **průměrná těžba ropy modul**, **průměrná paliva**, a **průměrná modul mírného**.  

Změňte výchozí agregace polí v každé měřidla dle výše uvedených kroků v **"Průměrná rychlost"** měřidla.

![Připojené aut - měřidla](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4z.png)

Klikněte na prázdnou oblast přidat nové vizualizace.

Vyberte **spojnicový a skupinový sloupcový graf** z vizualizace, přetáhněte **města** pole do **sdílené osy**, přetáhněte **rychlost**, **pole tirepressure a engineoil** do **hodnoty ve sloupcích** oblasti, změnit jejich typ agregace na **průměrná**. 

Přetáhněte **engineTemperature** pole do **hodnoty řádku** oblasti změnit typ agregace na **průměrná**. 

![Připojené aut - vizualizace pole](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4aa.png)

Změnit grafu **název** k **"Průměrná rychlost, můžete zadat naléhavost, modul těžba ropy a teploty modul"**.  

![Připojené aut - vizualizace pole](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4bb.png)

Klikněte na prázdnou oblast přidat nové vizualizace.

Vyberte **vlastnosti Treemap** vizualizace z vizualizace, přetáhněte **modelu** pole do **skupiny** oblasti a přetáhněte pole **MaintenanceProbability** do **hodnoty** oblasti.

Změnit grafu **název** k **"Vehicle modely údržby"**.

![Připojené aut - změnit název grafu](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4cc.png)

Klikněte na prázdnou oblast přidat nové vizualizace.

Vyberte **100 % skládaný sloupcový graf** z vizualizace, přetáhněte **města** pole do **osy** oblasti a přetáhněte **MaintenanceProbability**, **RecallProbability** polí do **hodnotu** oblasti.

![Připojené aut - přidat nové vizualizace](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4dd.png)

Klikněte na tlačítko **formátu**, vyberte **Data barvy**a nastavte **MaintenanceProbability** barvy, která má hodnotu **"F2C80F"**.

Změna **název** grafu můžete **"pravděpodobnosti z Vehicle údržby a odvolat pomocí Město,**.

![Připojené aut - přidat nové vizualizace](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4ee.png)

Klikněte na prázdnou oblast přidat nové vizualizace.

Vyberte **plošný graf** z vizualizace z vizualizace, přetáhněte **modelu** pole do **osy** oblasti a přetáhněte **engineOil, tirepressure, rychlosti a MaintenanceProbability** polí do **hodnoty** oblasti. Změnit jejich typ agregace na **"Průměrné"**. 

![Připojené aut - změnit typ agregace](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4ff.png)

Změnit název graf **"Průměrná těžba ropy modul, přestanou zatížení, rychlosti a údržba pravděpodobnosti modelem"**.

![Připojené aut - změnit název grafu](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4gg.png)

Klikněte na prázdnou oblast přidat nové vizualizace:

1. Vyberte **bodový graf** vizualizace z vizualizace.
2. Přetáhněte **modelu** pole do **podrobnosti** a **legendy** oblasti.
3. Přetáhněte **paliva** pole do **osy x** oblasti změnit agregace na **průměrná**.
4. Přetáhněte **engineTemparature** do **osy y oblasti**, změňte agregace na **průměrná**
5. Přetáhněte **vin** pole do **velikost** oblasti.

![Připojené aut - přidat nové vizualizace](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4hh.png)

Změnit grafu **název** k **"Průměry paliva, modul teploty modelem"**.

![Připojené aut - změnit název grafu](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4ii.png)

Závěrečnou zprávu bude vypadat, jak je uvedeno níže.

![Připojené aut konečné sestavy](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4jj.png)

### <a name="pin-visualizations-from-the-reports-to-the-real-time-dashboard"></a>Vizualizace kódu PIN ze sestav na řídicím panelu v reálném čase
Vytvořte prázdný řídicí panel kliknutím na ikonu plus vedle řídicí panely. Pojmenujte ji "Panelu analýzy Telemetrie Vehicle"

![Připojené aut – řídicí panel](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.5.png)

Připnete na řídicí panel vizualizaci z výše uvedených sestav. 

![Připojené aut – řídicí panel](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.6.png)

Po vytvoření všechny tři sestavy a odpovídající vizualizacemi jsou připnuli k řídicímu panelu, řídicí panel by měla vypadat takto. Pokud jste dosud nevytvořili všechny sestavy, může vypadat jinak řídicího panelu. 

![Připojené aut – řídicí panel](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-4.0.png)

Blahopřejeme! Úspěšně jste vytvořili řídicím panelu v reálném čase. Při dalším spuštění CarEventGenerator.exe a RealtimeDashboardApp.exe, měli byste vidět živé aktualizace na řídicím panelu. Pomocí následujících kroků má trvat asi 10 až 15 minut.

## <a name="setup-power-bi-batch-processing-dashboard"></a>Instalační program řídicí panel Power BI dávkové zpracování
> [!NOTE]
> Pro kanál koncová dávkové zpracování dokončí provádění a zpracovat v roce vhodné generované datové trvá o dvou hodin (z úspěšné dokončení nasazení). Proto čekající na zpracování ukončíte než budete pokračovat v dalších krocích. 
> 
> 

**Stáhněte si soubor návrháře Power BI**

* Je součástí nasazení pokyny pro ruční provoz předem nakonfigurovaný soubor návrháře Power BI
* Podívejte se na 2. Instalační program PowerBI dávkové zpracování řídicí panel si můžete stáhnout šablony PowerBI pro dávkové zpracování řídicí panel zde názvem **ConnectedCarsPbiReport.pbix**.
* Uložit místně

**Konfigurace sestav Power BI**

* Otevřete soubor návrháře '**ConnectedCarsPbiReport.pbix**' pomocí Power BI Desktop. Pokud jste ještě není, nainstalujte Power BI Desktop z [instalace Power BI Desktop](http://www.microsoft.com/download/details.aspx?id=45331). 
* Klikněte **upravit dotazy**.

![Upravit dotaz Power BI](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/10-edit-powerbi-query.png)

* Dvakrát klikněte **zdroj**.

![Zdroj sady Power BI](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/11-set-powerbi-source.png)

* Aktualizujte připojovací řetězec serveru se serverem Azure SQL, které byly zřízeny jako součást nasazení.  Podívejte se v pokynech ruční operace v části 

    4. Azure SQL Database
    
    * Server: somethingsrv.database.windows.net
    * Databáze: connectedcar
    * Uživatelské jméno: uživatelské jméno
    * Heslo: Heslo SQL serveru můžete spravovat z portálu Azure

* Nechte **databáze** jako *connectedcar*.

![Nastavte Power BI databázi](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/12-set-powerbi-database.png)

* Klikněte na **OK**.
* Zobrazí se **pověření systému Windows** vybraná karta ve výchozím nastavení, změňte ho na **databáze pověření** kliknutím na **databáze** kartě vpravo.
* Zadejte **uživatelské jméno** a **heslo** vaší databáze SQL Azure, který byl zadán během instalace jeho nasazení.

![Zadejte přihlašovací údaje databáze](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/13-provide-database-credentials.png)

* Klikněte na tlačítko **připojení**
* Zopakujte výše uvedené kroky pro každý tři zbývající dotaz, který je přítomen v pravém podokně a pak aktualizujte podrobnosti připojení zdroje dat.
* Klikněte na tlačítko **zavřete a načíst**. Power BI Desktop souboru datové sady jsou připojené k tabulky databáze SQL Azure.
* **Zavřít** souboru Power BI Desktop.

![Zavřít Power BI desktop](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/14-close-powerbi-desktop.png)

* Klikněte na tlačítko **Uložit** tlačítko a uložte změny. 

Nyní máte nakonfigurovaný všechny sestavy odpovídající cesta zpracování dávky v řešení. 

## <a name="upload-to-powerbicom"></a>Nahrajte do *powerbi.com*
1. Přejděte na webový portál Power BI na http://powerbi.com a přihlášení.
2. Klikněte na tlačítko **získat Data**  
3. Nahrajte soubor Power BI Desktop.  
4. Chcete-li nahrát, klikněte na tlačítko **načíst Data -> získat soubory -> místního souboru**  
5. Přejděte na **"**ConnectedCarsPbiReport.pbix**"**  
6. Po nahrání souboru přejde zpět na váš pracovní prostor Power BI.  

Datové sady, sestavy a řídicí panel prázdné, bude vytvořena pro vás.  

Grafy kód PIN na nové řídicí panel názvem **panelu analýzy Telemetrie Vehicle** v **Power BI**. Klikněte na prázdný řídicí panel vytvořili výše a potom přejděte na **sestavy** části klikněte na nově nahraném sestavu.  

![Vehicle Telemetrie Power BI.com](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/vehicle-telemetry-dashboard1.png) 

**Poznámka: Sestava má šest stránky:**  
Stránka 1: Hustotu Vehicle  
Stránka 2: Stav v reálném čase vehicle  
Stránka 3: Intenzivně řízené vozidel   
Stránka 4: Připomenout vozidel  
5 stránky: Efektivní řízené vozidel paliva  
6 stránky: Logo společnosti Contoso  

![Připojené aut Power BI.com](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/vehicle-telemetry-dashboard2.png)

**Ze stránky 3**, připnout následující:  

1. Počet VIN  
   ![Připojené aut Power BI.com](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/vehicle-telemetry-dashboard3.png) 
2. Intenzivně řídí vozidel modelu – vodopádu grafu  
   ![Vehicle Telemetrie – Pin grafy 4](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/vehicle-telemetry-dashboard4.png)

**Ze stránky 5**, připnout následující: 

1. Počet vin    
   ![Vehicle Telemetrie – Pin grafy 5](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/vehicle-telemetry-dashboard5.png)  
2. Paliva efektivní vozidel modelem: Skupinový sloupcový graf  
   ![Vehicle Telemetrie – Pin grafy 6](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/vehicle-telemetry-dashboard6.png)

**Stránka 4**, připnout následující:  

1. Počet vin  
   ![Vehicle Telemetrie – Pin grafy 7](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/vehicle-telemetry-dashboard7.png) 
2. Odvolané vozidel podle města, model: vlastnosti Treemap  
   ![Vehicle Telemetrie – Pin grafy 8](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/vehicle-telemetry-dashboard8.png)  

**Ze stránky 6**, připnout následující:  

1. Logo společnosti Contoso motory  
   ![Vehicle Telemetrie – Pin grafy 9](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/vehicle-telemetry-dashboard9.png)

**Uspořádání řídicí panel**  

1. Přejděte na řídicí panel
2. Podržte ukazatel nad každý graf a přejmenujte ji v závislosti na pojmenování uvedené v následující obrázek kompletní řídicí panel. Také pohyb grafy, aby vypadala jako na řídicím panelu.  
   ![Vehicle Telemetrie – uspořádání řídicí panel 2](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/vehicle-telemetry-organize-dashboard2.png)  
   ![Vehicle Telemetrie Power BI.com](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/vehicle-telemetry-dashboard.png)
3. Pokud jste vytvořili všechny sestavy, jak je uvedeno v tomto dokumentu, poslední dokončené řídicí panel by měl vypadat jako na následujícím obrázku. 

![Vehicle Telemetrie – uspořádání řídicí panel 2](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/vehicle-telemetry-organize-dashboard3.png)

Blahopřejeme! Úspěšně jste vytvořili sestavy a řídicí panel můžete získat v reálném čase, prediktivní a dávky o vehicle stavu a řídí zvyklosti.  
