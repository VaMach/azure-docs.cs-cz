---
title: "Zatížení testování vaší aplikace pomocí sady Visual Studio Team Services | Microsoft Docs"
description: "Naučte se zátěžový test aplikace Azure Service Fabric pomocí Visual Studio Team Services."
services: service-fabric
documentationcenter: na
author: cawams
manager: timlt
editor: 
ms.assetid: fc743585-0d1b-483f-981d-493f4552ac07
ms.service: multiple
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: multiple
ms.date: 11/18/2016
ms.author: cawa
ms.openlocfilehash: e8e270ce865d4da3ee219958b308db2c1c89b11b
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/11/2017
---
# <a name="load-test-your-application-by-using-visual-studio-team-services"></a>Zatížení testování vaší aplikace pomocí sady Visual Studio Team Services
Tento článek ukazuje, jak zátěžový test pomocí sady Microsoft Visual Studio zatížení testovací funkcí aplikace. Používá Azure Service Fabric stavové služby back-end a bezstavové služby webového uživatelského rozhraní. Ukázková aplikace použít zde je simulátoru letadle umístění. Poskytnete ID letadle, čas odeslání a cíl. Back-end aplikace zpracovává požadavky a front-endu zobrazí na mapě letadle odpovídající kritériím.

Následující diagram znázorňuje aplikace Service Fabric, která budete testování.

![Diagram příkladu letadle umístění aplikace][0]

## <a name="prerequisites"></a>Požadavky
Než začnete, musíte udělat následující:

* Získáte účet Visual Studio Team Services. Můžete jej získat zdarma v [Visual Studio Team Services](https://www.visualstudio.com).
* Získejte a nainstalujte Visual Studio 2013 nebo Visual Studio 2015. Tento článek používá edice Visual Studio 2015 Enterprise, ale Visual Studio 2013 a jiné edice by měly fungovat podobně.
* Nasazení aplikace do pracovního prostředí. V tématu [nasazení aplikací do clusteru s podporou vzdálené pomocí sady Visual Studio](service-fabric-publish-app-remote-cluster.md) informace o této.
* Pochopení vzor používání vaší aplikace. Tyto informace slouží k simulaci vzor zatížení.
* Pochopení cílem pro zátěžové testování. To vám pomůže interpretovat a analýza výsledků zátěžových testů.

## <a name="create-and-run-the-web-performance-and-load-test-project"></a>Vytvoření a spuštění projektu výkonu webu a zátěžového testu
### <a name="create-a-web-performance-and-load-test-project"></a>Vytvoření projektu výkonu webu a zátěžového testu
1. Otevřete sadu Visual Studio 2015. Zvolte **soubor** > **nový** > **projektu** v řádku nabídek otevřete **nový projekt** dialogové okno.
2. Rozbalte **Visual C#** uzel a zvolte **Test** > **výkonu webu a zátěžového testu projektu**. Pojmenujte projekt a zvolte **OK** tlačítko.

    ![Snímek obrazovky dialogového okna Nový projekt][1]

    Měli byste vidět nový projekt výkonu webu a zátěžového testu v Průzkumníku řešení.

    ![Snímek obrazovky Průzkumník řešení zobrazující nový projekt][2]

### <a name="record-a-web-performance-test"></a>Záznam testu výkonnosti webu
1. Otevřete projekt .webtest.
2. Vyberte **přidat záznam** ikonu pro spuštění relace záznam v prohlížeči.

    ![Snímek obrazovky ikonu Přidat záznam v prohlížeči][3]

    ![Snímek obrazovky znázorňující tlačítko záznam v prohlížeči][4]
3. Přejděte do aplikace Service Fabric. Na panelu záznam by měl zobrazit webových požadavků.

    ![Snímek obrazovky webové požadavky v panelu záznam][5]
4. Proveďte posloupnost akcí, které budou uživatelé k provedení. Tyto akce slouží jako vzor k vygenerování zatížení.
5. Až budete hotoví, vyberte **Zastavit** tlačítko záznam zastavit.

    ![Snímek obrazovky tlačítka Zastavit][6]

    .Webtest projektu v sadě Visual Studio mají zaznamenány řadu požadavků. Dynamické parametry jsou automaticky nahradit. V tomto okamžiku můžete odstranit všechny závislosti navíc, opakované žádosti, které nejsou součástí vaší testovací scénáře.
6. Uložte projekt a potom zvolte **spustit Test** příkaz ke spuštění testu výkonnosti webu místně a zajistěte, aby vše funguje správně.

    ![Snímek obrazovky příkaz Spustit Test][7]

### <a name="parameterize-the-web-performance-test"></a>Parametrizace testu výkonnosti webu
Převod na programového testu výkonnosti webu a potom upravit kód můžete parametrizovat testu výkonnosti webu. Jako alternativu můžete vázat testu výkonnosti webu na seznamu dat tak, aby test iteruje data. V tématu [generování a spuštění testu výkonnosti webu programové](https://msdn.microsoft.com/library/ms182552.aspx) pro podrobnosti o tom, jak převést výkonu webových testů do programového testu. V tématu [přidání zdroje dat do testu výkonnosti webu](https://msdn.microsoft.com/library/ms243142.aspx) informace o tom, jak vytvořit vazbu dat do testu výkonnosti webu.

V tomto příkladu jsme budete převést testu výkonnosti webu do programového testu, můžete nahradit ID letadle vytvářenému identifikátoru GUID a přidat další požadavky na odeslání lety do různých umístění.

### <a name="create-a-load-test-project"></a>Vytvoření projektu testování zatížení
Projekt testu zatížení se skládá z jednoho nebo více scénářů k testu výkonnosti webu a testování částí, společně s nastavením testu další zadaný zatížení. Následující postup popisuje vytvoření projektu testování zatížení:

1. V místní nabídce projektu výkonu webu a zátěžového testu, zvolte **přidat** > **zátěžový Test**. V **testování zatížení** průvodce, vyberte **Další** tlačítko ke konfiguraci nastavení testu.
2. V **načíst vzor** zvolte, zda chcete zatížení konstantní uživatele nebo krok zatížení, která začíná na několik uživatelů a zvyšuje uživatele v čase.

    Pokud máte funkční odhad množství zatížení uživatele a chcete zobrazit, jak aktuální systém provede, zvolte **konstantní načíst**. Pokud vaším cílem je naučit jestli systém provede konzistentní v rámci různých zatížením, zvolte **krok zatížení**.
3. V **kombinace testů** zvolte **přidat** tlačítko a potom vyberte test, který chcete zahrnout do zátěžový test. Můžete použít **distribuční** sloupec zadejte procentuální podíl celkové testy spustit pro všechny testy.
4. V **spustit nastavení** , určete dobu trvání testu zatížení.

   > [!NOTE]
   > **Testování iterací** možnost je dostupná pouze při spuštění zátěžového testu místně pomocí sady Visual Studio.
   >
   >
5. V **umístění** části **spustit nastavení**, zadejte umístění, kde se generují požadavky na testovací zatížení. Průvodce můžete být vyzváni k přihlášení do vašeho účtu Team Services. Přihlaste se a potom zvolte geografické umístění. Až budete hotoví, vyberte **Dokončit** tlačítko.
6. Po vytvoření zátěžový test, otevřete projekt .loadtest a zvolte aktuální spuštění nastavení, například **spustit nastavení** > **spustit Settings1 [Active]**. Otevře nastavení spuštění v **vlastnosti** okno.
7. V **výsledky** části **spustit nastavení** vlastnosti – okno, **úložiště podrobností časování** by měl mít nastavení **žádné** jako výchozí hodnota. Změna této hodnoty na **všech podrobností o jednotlivých** získat další informace o zatížení výsledky testů. V tématu [načíst testování](https://www.visualstudio.com/load-testing.aspx) Další informace o tom, jak připojit k Visual Studio Team Services a spuštění zátěžového testu.

### <a name="run-the-load-test-by-using-visual-studio-team-services"></a>Spusťte zátěžový test pomocí sady Visual Studio Team Services
Vyberte **spustit Test zatížení** příkaz ke spuštění testovacím běhu.

![Snímek obrazovky příkaz spuštění zátěžového testu][8]

## <a name="view-and-analyze-the-load-test-results"></a>Zobrazení a analýza výsledků zátěžových testů
Jako zatížení test postupuje, informace o výkonu jsou vynesena do grafu. Měli byste vidět něco podobného jako v následujícím grafu.

![Snímek obrazovky graf výkonu pro výsledků zátěžového testu][9]

1. Vyberte **stáhnout sestavu** odkaz v horní části stránky. Po stažení sestavy, vyberte **zobrazit sestavu** tlačítko.

    Na **grafu** můžete zobrazit grafy pro různé čítače výkonu. Na **Souhrn** kartě zobrazují celkové výsledky testů. **Tabulky** kartě se zobrazují celkový počet předaný a k selhání zátěžové testy.
2. Vyberte počet odkazů na **Test** > **se nezdařilo** a **chyby** > **počet** sloupce zobrazíte podrobnosti o chybě.

    **Podrobností** kartě se zobrazují virtuální uživatele a testovací scénáře informace pro chybné žádosti. Tato data mohou být užitečné, pokud obsahuje více scénářů zátěžový test.

V tématu [analýza načíst výsledky testu v zobrazení grafů analyzéru načíst testování](https://www.visualstudio.com/load-testing.aspx) Další informace o zobrazení zatížení výsledky testu.

## <a name="automate-your-load-test"></a>Automatizovat zátěžového testu
Visual Studio Team Services načíst testovací poskytuje rozhraní API pro správu zátěžových testů a analýza výsledků v účtu Team Services. V tématu [rozhraní Rest API testování zatížení cloudu](http://blogs.msdn.com/b/visualstudioalm/archive/2014/11/03/cloud-load-testing-rest-apis-are-here.aspx) Další informace.

## <a name="next-steps"></a>Další kroky
* [Monitorování a Diagnostika služby v instalačním programu pro vývoj místním počítači](service-fabric-diagnostics-how-to-monitor-and-diagnose-services-locally.md)

[0]: ./media/service-fabric-vso-load-test/OverviewDiagram.png
[1]: ./media/service-fabric-vso-load-test/NewProjectDialog.png
[2]: ./media/service-fabric-vso-load-test/Project.png
[3]: ./media/service-fabric-vso-load-test/AddRecording.png
[4]: ./media/service-fabric-vso-load-test/AddRecording2.png
[5]: ./media/service-fabric-vso-load-test/ActionSequence.png
[6]: ./media/service-fabric-vso-load-test/StopRecording.png
[7]: ./media/service-fabric-vso-load-test/RunTest.png
[8]: ./media/service-fabric-vso-load-test/RunTest2.png
[9]: ./media/service-fabric-vso-load-test/Graph.png
