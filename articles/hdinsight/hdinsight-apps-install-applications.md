---
title: "Instalace aplikací Hadoop jiných výrobců v Azure HDInsight | Dokumentace Microsoftu"
description: "Přečtěte si, jak v Azure HDInsight instalovat aplikace Hadoop jiných výrobců."
services: hdinsight
documentationcenter: 
author: mumian
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: eaf5904d-41e2-4a5f-8bec-9dde069039c2
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 08/16/2017
ms.author: jgao
ms.openlocfilehash: 3b3ff8d33959978ddd648e59a6a301f00c247964
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/18/2017
---
# <a name="install-third-party-hadoop-applications-on-azure-hdinsight"></a>Instalace aplikací Hadoop jiných výrobců v Azure HDInsight

V tomto článku zjistěte, jak nainstalovat již publikované aplikace třetích stran Hadoop v Azure HDInsight. Pokyny pro instalaci vašich vlastních aplikací najdete v článku [Instalace vlastních aplikací HDInsight](hdinsight-apps-install-custom-applications.md).

Aplikace HDInsight je aplikace, kterou uživatelé mohou nainstalovat na clusteru HDInsight se systémem Linux. Tyto aplikace mohou být vytvořeny společností Microsoft, nezávislými dodavateli softwaru (ISV) nebo vámi samotnými.  

Aktuálně jsou dostupné čtyři publikované aplikace:

* **DATAIKU DDS v HDInsight:** Dataiku DSS (Data Science Studio) je software, který umožňuje odborníkům v oblasti dat (datoví vědci, obchodní analytici, vývojáři...) vytvářet prototypy, sestavovat a nasazovat vysoce specifické služby, které transformují nezpracovaná data na účinné obchodní předpovědi.
* **Datameer**: [Datameer](http://www.datameer.com/documentation/display/DAS50/Home?ls=Partners&lsd=Microsoft&c=Partners&cd=Microsoft) nabízí analytikům interaktivní způsob zjišťování, analýzy a vizualizace výsledků v případě velkých objemů dat. Získávejte dalších zdroje dat snadněji, abyste mohli rychle zjišťovat nové vztahy a získávat potřebné odpovědi.
* **Kolekce dat Streamsets pro HDInsight** poskytuje plně vybavené integrované vývojové prostředí (IDE) umožňující navrhovat, testovat, nasazovat a spravovat kanály ingestace typu any-to-any, které zachytávají streamovaná data a data dávek a zahrnují celou řadu transformací v průběhu streamování – to vše bez nutnosti psát vlastní kód. 
* **Obalového souboru CDAP pro HDInsight** poskytuje první integrace jednotnou platformu pro velké objemy dat, která snižuje čas do produkčního prostředí pro data aplikací a dat jezera 80 %. Tato aplikace podporuje pouze clustery Standard HBase 3.4.
* **H2O umělé Intelligence pro HDInsight (Beta)** H2O šumivého horních podporuje následující algoritmy distribuovaného: GLM, Naïve Bayes, distribuované náhodných doménové struktury, přechodu počítače zvyšovat skóre, hluboké Neuronové sítě, hluboké učení, K-means, PCA, zobecněn modely pořadí nízká, detekce anomálií a Autoencoders.
* **Kyligence Analytics Platform** Kyligence Analytics Platform (KAP) je připravené pro podniky datový sklad, který používá technologii Apache Kylin a Apache Hadoop, umožňuje dílčí sekundu latence dotazu pro datovou sadu masivním měřítku a zjednodušuje analýza dat pro podnikoví uživatelé a analytici. 
* **SnapLogic Hadooplex** SnapLogic Hadooplex systémem HDInsight umožňuje zákazníkům přístup k podnikových statistik rychlejší zadáním přijímání dat samoobslužné služby a příprava z prakticky libovolný zdroj na Cloudová platforma Microsoft Azure.
* **Serveru úloh Spark pro KNIME Spark vykonavatele** serveru úloh Spark pro KNIME Spark vykonavatele slouží k připojení ke clusterům HDInsight KNIME Analytics Platform.

Pokyny uvedené v tomto článku se týkají webu Azure Portal. Šablonu Azure Resource Manageru můžete exportovat z portálu nebo získat její kopii od dodavatelů, a poté k jejímu nasazení můžete použít Azure PowerShell a rozhraní příkazového řádku Azure.  Viz článek [Vytváření clusterů Hadoop na systému Linux v HDInsight pomocí šablon Resource Manageru](hdinsight-hadoop-create-linux-clusters-arm-templates.md).

## <a name="prerequisites"></a>Požadavky
Pokud chcete instalovat aplikace HDInsight na stávající cluster HDInsight, musí mít cluster služby HDInsight. Chcete-li jeden vytvořit, prostudujte si část [Tvorba clusterů](hdinsight-hadoop-linux-tutorial-get-started.md#create-cluster). Aplikace HDInsight můžete také nainstalovat při vytváření clusteru HDInsight.

## <a name="install-applications-to-existing-clusters"></a>Instalace aplikací do existujících clusterů
Následující postup ukazuje, jak můžete instalovat aplikace HDInsight do existujícího clusteru HDInsight.

**Pokud chcete instalovat aplikace HDInsight, postupujte následovně:**

1. Přihlaste se k webu [Azure Portal](https://portal.azure.com).
2. Klikněte na tlačítko **Clustery HDInsight** v levé nabídce.  Pokud ho nevidíte, klikněte na **Další služby** a pak klikněte na **Clustery HDInsight**.
3. Klikněte na cluster HDInsight.  Pokud ho ještě nemáte, vytvořte ho.  Viz článek [Vytvoření clusterů](hdinsight-hadoop-linux-tutorial-get-started.md#create-cluster).
4. Klikněte na **Aplikace** v kategorii **Konfigurace**. Zobrazí se seznam nainstalovaných aplikací. Pokud nemůžete najít aplikace, znamená to, že žádné aplikace pro tuto verzi clusteru HDInsight neexistují.
   
    ![Nabídka portálu pro aplikace HDInsight](./media/hdinsight-apps-install-applications/hdinsight-apps-portal-menu.png)
5. V nabídce okna klikněte na **Přidat**. 
   
    ![Nainstalované aplikace aplikací HDInsight](./media/hdinsight-apps-install-applications/hdinsight-apps-installed-apps.png)
   
    Zobrazí se seznam existujících aplikací HDInsight.
   
    ![Dostupné aplikace aplikací HDInsight](./media/hdinsight-apps-install-applications/hdinsight-apps-list.png)
6. Klikněte na jednu z aplikací, přijměte smluvní podmínky a potom klikněte na **Vybrat**.

Stav instalace aplikace můžete vidět v oznámeních portálu (klikněte na ikonu zvonku v horní části portálu). Aplikace se po instalaci zobrazí v okně Nainstalované aplikace.

## <a name="install-applications-during-cluster-creation"></a>Instalace aplikací při vytváření clusteru
Během vytváření clusteru máte možnost instalace aplikací HDInsight. Během tohoto procesu se aplikace HDInsight instalují po vytvoření clusteru a jeho přechodu do spuštěného stavu. Následující postup ukazuje, jak můžete instalovat aplikace HDInsight při vytváření clusteru.

**Pokud chcete instalovat aplikace HDInsight, postupujte následovně:**

1. Přihlaste se k [portálu Azure](https://portal.azure.com).
2. Klikněte na **NOVÝ**, **Data + analýzy** a potom klikněte na **HDInsight**.
3. Zadejte **Název clusteru**: Tento název musí být globálně jedinečný.
4. Klikněte na tlačítko **předplatné** vyberte předplatné Azure, který se používá pro cluster.
5. Klikněte na **Vybrat typ clusteru** a potom vyberte následující:
   
   * **Typ clusteru**: Pokud nevíte, vyberte možnost **Hadoop**. Jedná se o nejoblíbenější typ clusteru.
   * **Operační systém**: Vyberte **Linux**.
   * **Verze**: Pokud nevíte, použijte výchozí verzi. Další informace najdete v článku [Verze clusterů HDInsight](hdinsight-component-versioning.md).
   * **Úroveň clusteru**: Azure HDInsight nabízí cloud pro velké objemy dat ve dvou kategoriích, Standard a Premium. Další informace najdete v článku [Úrovně clusterů](hdinsight-hadoop-provision-linux-clusters.md#cluster-tiers).
6. Klikněte na **Aplikace**, potom na jednu z publikovaných aplikací a nakonec na **Vybrat**.
7. Klikněte na **Přihlašovací údaje** a potom zadejte heslo uživatele s rolí správce. Musíte také zadat **uživatelské jméno SSH** a buď **heslo** nebo **veřejný klíč**, který se používá k ověření uživatele SSH. Doporučujeme používat veřejný klíč. Klikněte v dolní části na tlačítko **Vybrat** a uložte konfiguraci přihlašovacích údajů.
8. Klikněte na tlačítko **zdroj dat**, vyberte jednu z existující účty úložiště, nebo vytvořit nový účet úložiště, který se má použít jako výchozí účet úložiště pro cluster.
9. Kliknutím na možnost **Skupina prostředků** zobrazíte existující skupinu prostředků, popřípadě klikněte na **Nová** a vytvořte novou skupinu prostředků.
10. V okně **Nový cluster HDInsight** zkontrolujte, jestli je vybraná možnost **Připnout na Úvodní panel**, a potom klikněte na **Vytvořit**. 

## <a name="list-installed-hdinsight-apps-and-properties"></a>Zobrazení seznamu nainstalovaných aplikací HDInsight a jejich vlastností
Portál zobrazuje seznam nainstalovaných aplikací HDInsight pro cluster a vlastnosti jednotlivých nainstalovaných aplikací.

**Pokud chcete zobrazit seznam aplikací HDInsight a jejich vlastností, postupujte následovně:**

1. Přihlaste se k webu [Azure Portal](https://portal.azure.com).
2. Klikněte na tlačítko **Clustery HDInsight** v levé nabídce.  Pokud ho nevidíte, klikněte na tlačítko **Procházet** a pak klikněte na tlačítko **Clustery HDInsight**.
3. Klikněte na cluster HDInsight.
4. Z okna **Nastavení** klikněte na tlačítko **Aplikace** pod kategorií **Obecné**. Okno Nainstalované aplikace uvádí všechny nainstalované aplikace. 
   
    ![Nainstalované aplikace aplikací HDInsight](./media/hdinsight-apps-install-applications/hdinsight-apps-installed-apps-with-apps.png)
5. Kliknutím na jednu z nainstalovaných aplikací zobrazíte její vlastnosti. Okno vlastností uvádí následující:
   
   * Název aplikace: Název aplikace.
   * Stav: Stav aplikace. 
   * Webová stránka: Adresa URL webové aplikace, které jste nasadili, k uzlu edge. Přihlašovací údaje jsou stejné jako přihlašovací údaje uživatele protokolu HTTP, které jste nakonfigurovali pro cluster.
   * Koncový bod HTTP: Přihlašovací údaje jsou stejné jako přihlašovací údaje uživatele protokolu HTTP, které jste nakonfigurovali pro cluster. 
   * Koncový bod SSH: SSH můžete použít pro připojení k uzlu edge. Přihlašovací údaje SSH jsou stejné jako přihlašovací údaje uživatele SSH, které jste nakonfigurovali pro cluster. Další informace najdete v tématu [Použití SSH se službou HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md).
6. Aplikaci odstranit, klikněte pravým tlačítkem na aplikaci a pak klikněte na tlačítko **odstranit** v místní nabídce.

## <a name="connect-to-the-edge-node"></a>Připojení k hraničnímu uzlu
K hraničnímu uzlu se můžete připojit pomocí protokolu HTTP a SSH. Informace o koncových bodech najdete na [portálu](#list-installed-hdinsight-apps-and-properties). Další informace najdete v tématu [Použití SSH se službou HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md).

Přihlašovací údaje koncového bodu protokolu HTTP jsou přihlašovací údaje uživatele protokolu HTTP, které jste nakonfigurovali pro cluster HDInsight. Přihlašovací údaje koncového bodu SSH jsou přihlašovací údaje SSH, které jste nakonfigurovali pro cluster HDInsight.

## <a name="troubleshoot"></a>Řešení potíží
Viz článek [Řešení potíží instalace](hdinsight-apps-install-custom-applications.md#troubleshoot-the-installation).

## <a name="next-steps"></a>Další kroky
* [Instalace vlastních aplikací HDInsight](hdinsight-apps-install-custom-applications.md): naučit se nasazovat nepublikované aplikace HDInsight do HDInsight.
* [Publikování aplikací HDInsight](hdinsight-apps-publish-applications.md): Zjistěte, jak publikovat vlastní aplikace HDInsight do obchodu Azure Marketplace.
* [MSDN: Instalace aplikace HDInsight](https://msdn.microsoft.com/library/mt706515.aspx): Další informace jak definovat aplikace HDInsight.
* [Přizpůsobení clusterů HDInsight v systému Linux pomocí akce skriptu](hdinsight-hadoop-customize-cluster-linux.md): další informace o použití akce skriptu k instalaci dalších aplikací.
* [Vytváření clusterů Hadoop na systému Linux v HDInsight pomocí šablon Resource Manageru](hdinsight-hadoop-create-linux-clusters-arm-templates.md): Zjistěte, jak voláním šablon Resource Manageru vytvoříte clustery HDInsight.
* [Použití prázdných hraničních uzlů v HDInsight](hdinsight-apps-use-edge-node.md): Zjistěte, jak lze pomocí prázdných hraničních uzlů přistupovat ke clusteru HDInsight, testovat aplikace HDInsight a hostovat aplikace HDInsight.

