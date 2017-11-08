---
title: "Škálování Azure prostředky založené na data výkonu nebo plánu | Microsoft Docs"
description: "Vytvoření nastavení automatického škálování pro plán služby app service pomocí metriky dat a plán"
author: anirudhcavale
manager: orenr
services: monitoring-and-diagnostics
documentationcenter: monitoring-and-diagnostics
ms.service: monitoring-and-diagnostics
ms.topic: tutorial
ms.date: 09/25/2017
ms.author: ancav
ms.custom: mvc
ms.openlocfilehash: 3a85e288fa6f7d6c7138b7fea8319bd8dee01c2c
ms.sourcegitcommit: 6a6e14fdd9388333d3ededc02b1fb2fb3f8d56e5
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/07/2017
---
# <a name="create-an-autoscale-setting-for--azure-resources-based-on-performance-data-or-a-schedule"></a>Vytvoření nastavení automatického škálování pro prostředky Azure na základě data výkonu nebo plánu

Nastavení automatického škálování umožňují přidat nebo odebrat instance služby na základě přednastavených podmínek. Tato nastavení lze vytvořit pomocí portálu. Tato metoda poskytuje založené na prohlížeči uživatelské rozhraní pro vytváření a konfiguraci nastavení automatického škálování. 

V tomto kurzu budete 
> [!div class="checklist"]
> * Vytvoření webové aplikace a plán služby App Service
> * Konfigurace automatického škálování pravidla pro škálování in a škálování podle počet požadavků, které obdrží webové aplikace
> * Aktivovat akce škálování a sledujte zvýšit počet instancí
> * Aktivovat akce škálování in a podívejte se na počet instancí snížit
> * Vyčištění prostředků

Pokud ještě nemáte předplatné Azure, vytvořte si [bezplatný účet](https://azure.microsoft.com/free/) před tím, než začnete.

## <a name="log-in-to-the-azure-portal"></a>Přihlášení k portálu Azure Portal

Přihlaste se k portálu [Azure Portal](https://portal.azure.com/).

## <a name="create-a-web-app-and-app-service-plan"></a>Vytvoření webové aplikace a plán služby App Service
Klikněte **nový** možnost v levém navigačním podokně

Vyhledejte a vyberte *webové aplikace* položku a klikněte na tlačítko **vytvořit**

Vyberte název aplikace, jako je *MyTestScaleWebApp*. Vytvořit novou skupinu prostředků * myResourceGroup' a umístěte jej do skupiny prostředků vašeho výběru.

Během několika minut by měl zřídit vašich prostředků. Pomocí webové aplikace a odpovídající plán služby App Service ve zbývající části tohoto kurzu.

    ![Create a new app service in the portal](./media/monitor-tutorial-autoscale-performance-schedule/Web-App-Create.png)

## <a name="navigate-to-autoscale-settings"></a>Přejděte do nastavení automatického škálování
1. V levém navigačním podokně, vyberte **monitorování** možnost. Po načtení stránky vyberte **škálování** kartě.
2. Seznam prostředků v rámci svého předplatného, které podporují škálování jsou zde uvedeny. Identifikovat plán služby App Service vytvořený dříve v tomto kurzu a klikněte na jeho.

    ![Přejděte do nastavení automatického škálování](./media/monitor-tutorial-autoscale-performance-schedule/monitor-blade-autoscale.png)

3. V nastavení automatického škálování, klikněte na **povolit škálování** tlačítko

Další několik Nápověda kroky, naplní obrazovce škálování, aby vypadala jako následující obrázek:

   ![Uložit nastavení škálování](./media/monitor-tutorial-autoscale-performance-schedule/Autoscale-Setting-Save.png)

 ## <a name="configure-default-profile"></a>Konfigurovat výchozí profil
1. Zadejte **název** pro nastavení automatického škálování
2. Ve výchozím nastavení profilu, zkontrolujte **škálování režimu** je nastaven na 'škálování na konkrétní instanci počet.
3. Nastavit počet instancí na **1**. Toto nastavení zajistí, že když žádné jiné profilu je aktivní, nebo v důsledku toho výchozí profil vrátí počet instancí 1.

  ![Přejděte do nastavení automatického škálování](./media/monitor-tutorial-autoscale-performance-schedule/autoscale-setting-profile.png)


## <a name="create-recurrance-profile"></a>Vytvoření profilu recurrance

1. Klikněte na **přidat podmínku škálování** odkazu na výchozí profil

2. Upravit **název** tohoto profilu být pondělí až pátek profilu

3. Ujistěte se, **škálování režimu** je nastaven na 'škálování podle metriky.

4. Pro **Instance omezení** nastavit **minimální** jako '1' **maximální** jako (2) a **výchozí** jako '1'. Toto nastavení zajistí, nebude tento profil není škálování plán služeb tak, aby měl menší než 1 instance nebo více než 2 instance. Pokud je profil nemá dostatek dat k provedení rozhodnutí, používá výchozí počet instancí (v tomto případě 1).

5. Pro **plán**, vyberte opakujte konkrétní dny

6. Nastavte profil opakování pondělí až pátek od 09:00 PST do 18:00 PST. Toto nastavení zajistí, že tento profil je pouze aktivní a použít 9: 00 do 18: 00, pondělí až pátek. Během jinou dobu profil "Výchozí" je profilu, který používá škálování nastavení.

## <a name="create-a-scale-out-rule"></a>Vytvoření pravidla Škálováním na více systémů

1. V pondělí do pátku profilu

2. Klikněte **přidat pravidlo** odkaz

3. Nastavte **metriky zdroj** jako jiný prostředek. Nastavte **typ prostředku** jako 'aplikační služby a **prostředků** jako webové aplikace v tomto kurzu vytvořili.

4. Nastavte **časové agregace** jako "Celkem", **název metriky** jako "Požadavky" a **čas intervalem statistiky** jako "Sum"

5. Nastavit **operátor** jako větší než, **prahová hodnota** jako "10" a **doba trvání** jako "5" minut.

6. Vyberte **operace** jako 'Zvýšení počítání po', **Instance počet** jako '1' a **nástrojů dolů** jako "5" minut

7. Klikněte **přidat** tlačítko

Toto pravidlo zajišťuje, že pokud vaše webová aplikace přijímá více než 10 požadavky během 5 minut nebo méně, je jedna další instance do vašeho plánu služby App Service spravovat zatížení.

   ![Vytvoření pravidla Škálováním na více systémů](./media/monitor-tutorial-autoscale-performance-schedule/Scale-Out-Rule.png)

## <a name="create-a-scale-in-rule"></a>Vytvořit pravidlo škálování v
Doporučujeme vám vždy mít pravidlo škálování in pro pravidlo škálování. Má oba zajistí, že nejsou vaše prostředky přes zřízený. Přes zřizování znamená mít více instancí spuštěných než je potřeba pro zpracování aktuálního zatížení. 

1. V pondělí do pátku profilu

2. Klikněte **přidat pravidlo** odkaz

3. Nastavte **metriky zdroj** jako jiný prostředek. Nastavte **typ prostředku** jako 'aplikační služby a **prostředků** jako webové aplikace v tomto kurzu vytvořili.

4. Nastavit **časové agregace** jako "Celkem", **název metriky** jako "Požadavky" a **čas intervalem statistiky** jako 'průměr.

5. Nastavit **operátor** jako "Je menší než", **prahová hodnota** jako '5' a **doba trvání** jako "5" minut.

6. Vyberte **operace** jako 'Zmenšete počet podle', **Instance počet** jako '1' a **nástrojů dolů** jako "5" minut

7. Klikněte **přidat** tlačítko

    ![Vytvořit pravidlo škálování v](./media/monitor-tutorial-autoscale-performance-schedule/Scale-In-Rule.png)

8. **Uložit** nastavení automatického škálování

    ![Uložit nastavení škálování](./media/monitor-tutorial-autoscale-performance-schedule/Autoscale-Setting-Save.png)

## <a name="trigger-scale-out-action"></a>Akce aktivace Škálováním na více systémů
K aktivaci s podmínkou Škálováním na více systémů v právě vytvořené nastavení automatického škálování, webové aplikace musí mít víc než 10 požadavky za méně než 5 minut.

1. Otevřete okno prohlížeče a přejděte do webové aplikace v tomto kurzu vytvořili. Adresa URL pro webovou aplikaci na portálu Azure můžete najít tak, že přejdete na prostředek vaší webové aplikace a kliknutím na **Procházet** tlačítka na kartě 'Přehled'.

2. Rychle po sobě načtením této stránky více než 10 x

3. V levém navigačním podokně, vyberte **monitorování** možnost. Po načtení stránky vyberte **škálování** kartě.

4. V seznamu vyberte plán služby App Service používaných v celém tomto kurzu

5. V nastavení automatického škálování, klikněte na **historie spouštění** karta

6. Zobrazí graf odrážející počet instancí plánu služby App Service v čase

7. Za několik minut by měl vzrostl počet instancí z 1, 2.

8. V části grafu najdete v části aktivity položky protokolu pro každý akce škálování provedenou toto nastavení automatického škálování.

## <a name="trigger-scale-in-action"></a>Škálování akce aktivace
Škálování v podmínku v škálování nastavení aktivačních událostí, pokud máte méně než 5 požadavky do webové aplikace po dobu 10 minut. 

1. Ujistěte se, že žádné žádosti jsou odesílány do vaší webové aplikace

2. Zatížení portálu Azure

3. V levém navigačním podokně, vyberte **monitorování** možnost. Po načtení stránky vyberte **škálování** kartě.

4. V seznamu vyberte plán služby App Service používaných v celém tomto kurzu

5. V nastavení automatického škálování, klikněte na **historie spouštění** karta

6. Zobrazí graf odrážející počet instancí plánu služby App Service v čase.

7. Za pár minut by počet instancí vyřadit z 2, na 1. Tento proces trvat aspoň 100 minut.  

8. V části grafu jsou odpovídající sadu aktivity položky protokolu pro každý akce škálování provedenou toto nastavení automatického škálování

    ![Zobrazení škálování v akce](./media/monitor-tutorial-autoscale-performance-schedule/Scale-In-Chart.png)

## <a name="clean-up-resources"></a>Vyčištění prostředků

1. Z nabídky na levé straně na portálu Azure, klikněte na tlačítko **všechny prostředky** a pak vyberte Webová aplikace vytvořená v tomto kurzu.

2. Na stránce prostředků, klikněte na tlačítko **odstranit**, potvrďte odstranění zadáním **Ano** textového pole a pak klikněte na **odstranit**.

3. Pak vyberte prostředek plánu služby App Service a klikněte na **odstranit**

4. Potvrzení odstranění zadáním **Ano** textového pole a pak klikněte na **odstranit**.

## <a name="next-steps"></a>Další kroky

V tomto kurzu jste  
> [!div class="checklist"]
> * Vytvořit webovou aplikaci a plán služby App Service
> * Škálování nakonfigurovaných pravidel pro škálování in a škálování podle počet požadavků přijatých webovou aplikaci
> * Spustit akci škálování a sledovaná zvýšit počet instancí
> * Spustit akci škálování in a sledovaná snížit počet instancí
> * Vyčistit vašich prostředků


Další informace o nastavení automatického škálování, pokračujte na [škálování přehled](monitoring-overview-autoscale.md).

> [!div class="nextstepaction"]
> [Archivovat monitorování data](monitor-tutorial-archive-monitoring-data.md)
