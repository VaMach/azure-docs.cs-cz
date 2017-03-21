---
title: "Začínáme s modulem plug-in Eclipse pro Azure Service Fabric | Dokumentace Microsoftu"
description: "Začínáme s modulem plug-in Eclipse pro Azure Service Fabric."
services: service-fabric
documentationcenter: java
author: sayantancs
manager: timlt
editor: 
ms.assetid: bf84458f-4b87-4de1-9844-19909e368deb
ms.service: service-fabric
ms.devlang: java
ms.topic: get-started-article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 12/27/2016
ms.author: saysa
translationtype: Human Translation
ms.sourcegitcommit: 24d86e17a063164c31c312685c0742ec4a5c2f1b
ms.openlocfilehash: 891df38aa685cac7bbfecb71b15c88d557ac493b
ms.lasthandoff: 03/11/2017


---

# <a name="getting-started-with-eclipse-plugin-for-service-fabric-java-application-development"></a>Začínáme s modulem plug-in Eclipse pro vývoj aplikací Service Fabric Java
Eclipse je jedním z nejpoužívanějších integrovaných vývojových prostředí pro vývojáře v Javě. V tomto článku probereme možnosti nastavení vývojového prostředí Eclipse pro práci se Service Fabric. Tento článek vám pomůže nainstalovat modul plug-in, vytvořit aplikaci Service Fabric a nasadit ji na místní nebo vzdálený cluster Service Fabric.

## <a name="install-or-update-service-fabric-plugin-on-eclipse-neon"></a>Instalace a aktualizace modulu plug-in Service Fabric v prostředí Eclipse Neon
Service Fabric poskytuje modul plug-in pro **integrované vývojové prostředí Eclipse pro vývojáře v Javě**, který může zjednodušit proces sestavování a nasazování služeb v Javě.

1. Ujistěte se, že máte nainstalované nejnovější prostředí Eclipse **Neon** a nejnovější verzi Buildship (1.0.17 nebo novější). Verze nainstalovaných komponent můžete zkontrolovat výběrem ``Help => Installation Details`` (Nápověda > Podrobnosti o instalaci). Buildship můžete aktualizovat pomocí pokynů [zde][buildship-update]. Pokud chcete zkontrolovat a aktualizovat prostředí Eclipse Neon na nejnovější verzi, můžete přejít do části ``Help => Check for Updates`` (Nápověda > Kontrola aktualizací).

2. Chcete-li nainstalovat modul plug-in Service Fabric, zvolte ``Help => Install New Software...`` (Nápověda > Instalace nového softwaru...).
  1. Do textového pole „Work with“ (Pracovat s) zadejte ``http://dl.windowsazure.com/eclipse/servicefabric``.
  2. Klikněte na Přidat.

  ![Modul plug-in Eclipse Neon pro Service Fabric][sf-eclipse-plugin-install]

  3. Zvolte modul plug-in služby Service Fabric a klikněte na Další.
  4. Pokračujte v instalaci a přijměte licenční smlouvu pro koncového zákazníka.

Pokud už máte modul plug-in Service Fabric Eclipse nainstalovaný, ověřte, že používáte nejnovější verzi. V části ``Help => Installation Details`` (Nápověda > Podrobnosti o instalaci) můžete zkontrolovat, jestli je možné ho ještě aktualizovat. Pak vyhledejte Service Fabric v seznamu nainstalovaných modulů plug-in a klikněte na Update (Aktualizovat). Pokud existují nějaké čekající aktualizace, načtou se a nainstalují.

> [!NOTE]
> Pokud instalace nebo aktualizace modulu plug-in Service Fabric Eclipse trvá příliš dlouho, je to proto, že se Eclipse pokaždé snaží načíst metadata všech nových změn, ke kterým došlo na všech aktualizačních serverech registrovaných k vaší instanci Eclipse. Můžete to urychlit použitím jednoduchého triku – přejděte do části `Available Software Sites` (Dostupné softwarové servery) a zrušte zaškrtnutí všech kromě serveru odkazujícího na umístění modulu plug-in Service Fabric `http://dl.windowsazure.com/eclipse/servicefabric`.
>

## <a name="create-service-fabric-application-using-eclipse"></a>Vytvoření aplikace Service Fabric pomocí Eclipse

1. Přejděte do části ``File => New => Other`` (Soubor > Nový > Jiné). Vyberte ``Service Fabric Project`` (Projekt Service Fabric). Klikněte na ``Next`` (Další).

    ![Nový projekt Service Fabric – stránka 1][create-application/p1]

2. Zadejte název projektu. Klikněte na ``Next`` (Další).

    ![Nový projekt Service Fabric – stránka 2][create-application/p2]

3. Z dostupné sady šablon (Actor, Stateless, Container nebo Guest-executable – Služba objektu actor, Bezstavová služba, Kontejner nebo Spustitelný soubor typu Host) vyberte šablonu služby. Klikněte na ``Next`` (Další).

    ![Nový projekt Service Fabric – stránka 3][create-application/p3]

4. Na této stránce zadejte název služby a/nebo související podrobnosti o službě a klikněte na ``Finish`` (Dokončit).

    ![Nový projekt Service Fabric – stránka 4][create-application/p4]

5. Pokud vytváříte první projekt Service Fabric, budete dotázáni, jestli chcete nastavit perspektivu služby Service Fabric – pokračujte výběrem ``yes`` (Ano).

    ![Nový projekt Service Fabric – stránka 5][create-application/p5]

6. Po úspěšném vytvoření bude projekt vypadat nějak takto:

    ![Nový projekt Service Fabric – stránka 6][create-application/p6]

## <a name="build-and-deploy-the-service-fabric-application-using-eclipse"></a>Sestavení a nasazení aplikace Service Fabric pomocí Eclipse

* Klikněte pravým tlačítkem na aplikaci Service Fabric, kterou jste právě vytvořili v předchozím kroku. V místní nabídce vyberte možnost ``Service Fabric``. Otevře se podnabídka s několika možnostmi. Bude vypadat nějak takto:

    ![Nabídka Service Fabric zobrazená po kliknutí pravým tlačítkem][publish/RightClick]

  Jakmile kliknete na možnosti sestavení, opětovného sestavení nebo vyčištění, provedou se příslušné akce.
  - ``Build Application`` (Sestavit aplikaci) sestaví aplikaci bez vyčištění.
  - ``Rebuild Application`` (Znovu sestavit aplikaci) provede vyčištění a nové sestavení aplikace.
  - ``Clean Application`` (Vyčistit aplikaci) vyčistí aplikaci od artefaktů sestavení.


* V této nabídce můžete zvolit také nasazení, zrušení nasazení nebo publikování aplikace.
  - ``Deploy Application`` (Nasadit aplikaci) nasadí aplikaci na místní cluster.
  - ``Publish Application...`` (Publikovat aplikaci) zobrazí dotaz, jestli chcete použít profil publikování ``Local.json``, nebo ``Cloud.json``. Tyto soubory JSON se používají k uložení informací (například koncové body připojení a informace o zabezpečení) potřebných pro připojení k místnímu nebo cloudovému (Azure) clusteru.

  ![Nabídka Service Fabric zobrazená po kliknutí pravým tlačítkem][publish/Publish]

* Existuje alternativní způsob nasazení aplikace Service Fabric pomocí konfigurací spuštění Eclipse.

  1. Zvolte ``Run => Run Configurations`` (Spustit > Konfigurace spuštění). V části ``Grade Project`` (Klasifikace projektu) vyberte konfiguraci spuštění ``ServiceFabricDeployer``.
  2. Na kartě ``Arguments`` (Argumenty) v pravém podokně zadejte jako ``publishProfile`` (Profil publikování) **local** (místní) nebo **cloud** (v cloudu). Výchozí nastavení je **local** (místní). Pro nasazení na vzdálený nebo cloudový cluster vyberte **cloud** (v cloudu).
  3. Ujistěte se, že jsou v profilech publikování vyplněné správné informace a podle potřeby upravte soubor `Local.json` nebo `Cloud.json` s použitím případných podrobností o koncovém bodu a zabezpečovacích přihlašovacích údajů.
  4. Ujistěte se, že ``Working Directory`` (Pracovní adresář) v pravém podokně v části ``Grade Project`` (Klasifikace projektu) odkazuje na aplikaci, kterou chcete nasadit. Pokud ne, stačí kliknout na tlačítko ``Workspace...`` (Pracovní prostor) a vybrat požadovanou aplikaci.
  5. Klikněte na **Apply** (Použít) a **Run** (Spustit).

Vaše aplikace se během chvilky sestaví a nasadí. Ke sledování jejího stavu můžete využít Service Fabric Explorer.  

## <a name="add-new-service-fabric-service-to-your-service-fabric-application"></a>Přidání nové služby Service Fabric do aplikace Service Fabric

Přidat novou službu Service Fabric do stávající aplikace Service Fabric je možné pomocí následujících kroků:

1. Klikněte pravým tlačítkem na projekt, do kterého chcete přidat službu. Otevře se místní nabídka, ve které vyberte možnost Service Fabric. Otevře se podnabídka s několika možnostmi.

    ![Přidat službu Service Fabric – stránka 1][add-service/p1]

2. Vyberte možnost `Add ServiceFabric Service` (Přidat službu Service Fabric) a zobrazí se průvodce dalšími kroky k přidání služby do projektu.
3. Vyberte šablonu služby, kterou chcete přidat do projektu, a klikněte na Next (Další).

    ![Přidat službu Service Fabric – stránka 2][add-service/p2]

4. Zadejte název služby (a podle potřeby další nezbytné podrobnosti) a klikněte tlačítko Add Service (Přidat službu) v dolní části.  

    ![Přidat službu Service Fabric – stránka 3][add-service/p3]

5. Po úspěšném přidání služby bude celá struktura projektu vypadat nějak takto:

    ![Přidat službu Service Fabric – stránka 4][add-service/p4]

## <a name="upgrade-your-service-fabric-java-application"></a>Upgrade aplikace Service Fabric v Javě

Předpokládejme, že jste vytvořili a nasadili projekt ``App1`` pomocí modulu plug-in Service Fabric Eclipse, čímž jste vytvořili aplikaci ``fabric:/App1Application`` typu ``App1AppicationType`` ve verzi 1.0. Nyní chcete provést upgrade aplikace bez jejího zastavení.

Proveďte změnu aplikace a znovu sestavte upravenou službu.  Aktualizujte soubor manifestu upravené služby (``ServiceManifest.xml``) s použitím aktualizovaných verzí pro službu (a podle potřeby i verzí kódu, konfigurace nebo dat). Také upravte manifest aplikace (``ApplicationManifest.xml``) s použitím čísla aktualizované verze pro aplikaci a upravenou službu.  

Pokud chcete upgradovat aplikaci pomocí Eclipse, můžete vytvořit duplicitní konfiguraci spuštění a podle potřeby ji použít k upgradu aplikace následujícím způsobem:
1. Zvolte ``Run => Run Configurations`` (Spustit > Konfigurace spuštění). Klikněte na malou šipku nalevo od možnosti ``Grade Project`` (Klasifikace projektu) v levém podokně.
2. Klikněte pravým tlačítkem na ``ServiceFabricDeployer`` a vyberte ``Duplicate`` (Duplikovat). Zadejte nový název této konfigurace, třeba ``ServiceFabricUpgrader``.
3. V pravém panelu na kartě ``Arguments`` (Argumenty) změňte ``-Pconfig='deploy'`` na ``-Pconfig=upgrade`` a klikněte na ``Apply`` (Použít).
4. Nyní jste vytvořili a uložili konfiguraci spuštění pro upgrade vaší aplikace, který můžete spustit (``Run``) kdykoli chcete. Postará se o získání nejnovější aktualizované verze typu aplikace ze souboru manifestu aplikace.

Nyní můžete monitorovat upgrade aplikace pomocí Service Fabric Exploreru. Za několik minut bude aplikace aktualizovaná.

<!-- Images -->

[sf-eclipse-plugin-install]: ./media/service-fabric-get-started-mac/sf-eclipse-plugin-install.png

[create-application/p1]:./media/service-fabric-get-started-eclipse/create-application/p1.png
[create-application/p2]:./media/service-fabric-get-started-eclipse/create-application/p2.png
[create-application/p3]:./media/service-fabric-get-started-eclipse/create-application/p3.png
[create-application/p4]:./media/service-fabric-get-started-eclipse/create-application/p4.png
[create-application/p5]:./media/service-fabric-get-started-eclipse/create-application/p5.png
[create-application/p6]:./media/service-fabric-get-started-eclipse/create-application/p6.png

[publish/Publish]: ./media/service-fabric-get-started-eclipse/publish/Publish.png
[publish/RightClick]: ./media/service-fabric-get-started-eclipse/publish/RightClick.png

[add-service/p1]: ./media/service-fabric-get-started-eclipse/add-service/p1.png
[add-service/p2]: ./media/service-fabric-get-started-eclipse/add-service/p2.png
[add-service/p3]: ./media/service-fabric-get-started-eclipse/add-service/p3.png
[add-service/p4]: ./media/service-fabric-get-started-eclipse/add-service/p4.png

<!-- Links -->
[buildship-update]: https://projects.eclipse.org/projects/tools.buildship

