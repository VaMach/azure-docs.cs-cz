---
title: Instalace aplikací Hadoop v HDInsight | Microsoft Docs
description: Informace o instalaci aplikací HDInsight na aplikace HDInsight.
services: hdinsight
documentationcenter: ''
author: mumian
manager: jhubbard
editor: cgronlun
tags: azure-portal

ms.service: hdinsight
ms.devlang: na
ms.topic: hero-article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 09/14/2016
ms.author: jgao

---
# Instalace aplikací HDInsight
Aplikace HDInsight je aplikace, kterou uživatelé mohou nainstalovat na clusteru HDInsight se systémem Linux. Tyto aplikace mohou být vytvořeny společností Microsoft, nezávislými dodavateli softwaru (ISV) nebo vámi samotnými. V tomto článku se seznámíte s postupem instalace publikované aplikace. Pokud instalujete vlastní aplikaci, přečtěte si článek [Instalace vlastních aplikací HDInsight](hdinsight-apps-install-custom-applications.md). 

Aktuálně je dostupná jedna publikovaná aplikace:

* **Datameer**: [Datameer](http://www.datameer.com/documentation/display/DAS50/Home?ls=Partners&lsd=Microsoft&c=Partners&cd=Microsoft) nabízí analytikům interaktivní způsob zjišťování, analýzy a vizualizace výsledků v případě velkých objemů dat. Získávejte dalších zdroje dat snadněji, abyste mohli rychle zjišťovat nové vztahy a získávat potřebné odpovědi.

> [!NOTE]
> Aplikace Datameer je momentálně podporovaná jenom v clusterech Azure HDInsight verze 3.2.
> 
> 

Pokyny uvedené v tomto článku se týkají webu Azure Portal. Šablonu Azure Resource Manageru můžete exportovat z portálu nebo získat její kopii od dodavatelů, a poté k jejímu nasazení můžete použít Azure PowerShell a rozhraní příkazového řádku Azure.  Viz článek [Vytváření clusterů Hadoop na systému Linux v HDInsight pomocí šablon Resource Manageru](hdinsight-hadoop-create-linux-clusters-arm-templates.md).

## Požadavky
Pokud chcete instalovat aplikace HDInsight na stávající cluster HDInsight, musí mít cluster služby HDInsight. Chcete-li jeden vytvořit, prostudujte si část [Tvorba clusterů](hdinsight-hadoop-linux-tutorial-get-started.md#create-cluster). Aplikace HDInsight můžete také nainstalovat při vytváření clusteru HDInsight.

## Instalace aplikací do existujících clusterů
Následující postup ukazuje, jak můžete instalovat aplikace HDInsight do existujícího clusteru HDInsight.

**Pokud chcete instalovat aplikace HDInsight, postupujte následovně:**

1. Přihlaste se k webu [Azure Portal](https://portal.azure.com).
2. Klikněte na tlačítko **Clustery HDInsight** v levé nabídce.  Pokud ho nevidíte, klikněte na tlačítko **Procházet** a pak klikněte na tlačítko **Clustery HDInsight**.
3. Klikněte na cluster HDInsight.  Pokud ho ještě nemáte, vytvořte ho.  Viz článek [Vytvoření clusterů](hdinsight-hadoop-linux-tutorial-get-started.md#create-cluster).
4. Z okna **Nastavení** klikněte na tlačítko **Aplikace** pod kategorií **Obecné**. Okno **Nainstalované aplikace** uvádí všechny nainstalované aplikace. 
   
    ![nabídka portálu pro aplikace hdInsight](./media/hdinsight-apps-install-applications/hdinsight-apps-portal-menu.png)
5. V nabídce okna klikněte na **Přidat**. 
   
    ![nainstalované aplikace aplikací hdInsight](./media/hdinsight-apps-install-applications/hdinsight-apps-installed-apps.png)
   
    Zobrazí se seznam existujících aplikací HDInsight.
   
    ![dostupné aplikace aplikací hdinsight](./media/hdinsight-apps-install-applications/hdinsight-apps-list.png)
6. Klikněte na jednu z aplikací, přijměte smluvní podmínky a potom klikněte na **Vybrat**.

Stav instalace aplikace můžete vidět v oznámeních portálu (klikněte na ikonu zvonku v horní části portálu). Aplikace se po instalaci zobrazí v okně Nainstalované aplikace.

## Instalace aplikací při vytváření clusteru
Během vytváření clusteru máte možnost instalace aplikací HDInsight. Během tohoto procesu se aplikace HDInsight instalují po vytvoření clusteru a jeho přechodu do spuštěného stavu. Následující postup ukazuje, jak můžete instalovat aplikace HDInsight při vytváření clusteru.

**Pokud chcete instalovat aplikace HDInsight, postupujte následovně:**

1. Přihlaste se k [portálu Azure](https://portal.azure.com).
2. Klikněte na **NOVÝ**, **Data + analýzy** a potom klikněte na **HDInsight**.
3. Zadejte **Název clusteru**: Tento název musí být globálně jedinečný.
4. Klikněte na **Předplatné** vyberte předplatné Azure, které se použije pro cluster.
5. Klikněte na **Vybrat typ clusteru** a potom vyberte následující:
   
   * **Typ clusteru**: Pokud nevíte, vyberte možnost **Hadoop**. Jedná se o nejoblíbenější typ clusteru.
   * **Operační systém**: Vyberte **Linux**.
   * **Verze**: Pokud nevíte, použijte výchozí verzi. Další informace najdete v článku [Verze clusterů HDInsight](hdinsight-component-versioning.md).
   * **Úroveň clusteru**: Azure HDInsight nabízí cloud pro velké objemy dat ve dvou kategoriích, Standard a Premium. Další informace najdete v článku [Úrovně clusterů](hdinsight-hadoop-provision-linux-clusters.md#cluster-tiers).
6. Klikněte na **Aplikace**, potom na jednu z publikovaných aplikací a nakonec na **Vybrat**.
7. Klikněte na **Přihlašovací údaje** a potom zadejte heslo uživatele s rolí správce. Musíte zadat také **Uživatelské jméno SSH** a buď **HESLO**, nebo **VEŘEJNÝ KLÍČ**, který se bude používat k ověřování uživatele SSH. Doporučujeme používat veřejný klíč. Klikněte v dolní části na tlačítko **Vybrat** a uložte konfiguraci přihlašovacích údajů.
8. Klikněte na **Zdroj dat**, vyberte jeden z existujících účtů úložiště nebo vytvořte nový účet úložiště, který chcete použít jako výchozí účet úložiště clusteru.
9. Kliknutím na možnost **Skupina prostředků** zobrazíte existující skupinu prostředků, popřípadě klikněte na **Nová** a vytvořte novou skupinu prostředků.
10. V okně **Nový cluster HDInsight** zkontrolujte, jestli je vybraná možnost **Připnout na Úvodní panel **, a potom klikněte na **Vytvořit**. 

## Zobrazení seznamu nainstalovaných aplikací HDInsight a jejich vlastností
Portál zobrazuje seznam nainstalovaných aplikací HDInsight pro cluster a vlastnosti jednotlivých nainstalovaných aplikací.

**Pokud chcete zobrazit seznam aplikací HDInsight a jejich vlastností, postupujte následovně:**

1. Přihlaste se k webu [Azure Portal](https://portal.azure.com).
2. Klikněte na tlačítko **Clustery HDInsight** v levé nabídce.  Pokud ho nevidíte, klikněte na tlačítko **Procházet** a pak klikněte na tlačítko **Clustery HDInsight**.
3. Klikněte na cluster HDInsight.
4. Z okna **Nastavení** klikněte na tlačítko **Aplikace** pod kategorií **Obecné**. Okno Nainstalované aplikace uvádí všechny nainstalované aplikace. 
   
    ![nainstalované aplikace aplikací hdInsight](./media/hdinsight-apps-install-applications/hdinsight-apps-installed-apps-with-apps.png)
5. Kliknutím na jednu z nainstalovaných aplikací zobrazíte její vlastnosti. Okno vlastností uvádí následující:
   
   * Název aplikace: Název aplikace.
   * Stav: Stav aplikace. 
   * Webová stránka: Adresa URL webové aplikace, kterou jste nasadili do hraničního uzlu (pokud nějaký existuje). Přihlašovací údaje jsou stejné jako přihlašovací údaje uživatele protokolu HTTP, které jste nakonfigurovali pro cluster.
   * Koncový bod HTTP: Přihlašovací údaje jsou stejné jako přihlašovací údaje uživatele protokolu HTTP, které jste nakonfigurovali pro cluster. 
   * Koncový bod SSH: Pokud se chcete připojit k hraničnímu uzlu, můžete použít [SSH](hdinsight-hadoop-linux-use-ssh-unix.md). Přihlašovací údaje SSH jsou stejné jako přihlašovací údaje uživatele SSH, které jste nakonfigurovali pro cluster.
6. Pokud chcete aplikaci odstranit, klikněte pravým tlačítkem na aplikaci a potom v místní nabídce klikněte na **Odstranit**.

## Připojení k hraničnímu uzlu
K hraničnímu uzlu se můžete připojit pomocí protokolu HTTP a SSH. Informace o koncových bodech najdete na [portálu](#list-installed-hdinsight-apps-and-properties). Další informace o používání SSH najdete v článku [Používání SSH s linuxovými clustery Hadoop v HDInsight – v systému Linux, Unix nebo OS X](hdinsight-hadoop-linux-use-ssh-unix.md). 

Přihlašovací údaje koncového bodu protokolu HTTP jsou přihlašovací údaje uživatele protokolu HTTP, které jste nakonfigurovali pro cluster HDInsight. Přihlašovací údaje koncového bodu SSH jsou přihlašovací údaje SSH, které jste nakonfigurovali pro cluster HDInsight.

## Řešení potíží
Viz článek [Řešení potíží instalace](hdinsight-apps-install-custom-applications.md#troubleshoot-the-installation).

## Další kroky
* [Instalace vlastní aplikace HDInsight](hdinsight-apps-install-custom-applications.md): naučte se nasazovat nepublikované aplikace HDInsight do HDInsight.
* [Publikování aplikací HDInsight](hdinsight-apps-publish-applications.md): Zjistěte, jak publikovat vlastní aplikace HDInsight do obchodu Azure Marketplace.
* [MSDN: Instalace aplikace HDInsight](https://msdn.microsoft.com/library/mt706515.aspx): Další informace jak definovat aplikace HDInsight.
* [Přizpůsobení clusterů HDInsight v systému Linux pomocí akce skriptu](hdinsight-hadoop-customize-cluster-linux.md): další informace o použití akce skriptu k instalaci dalších aplikací.
* [Vytváření clusterů Hadoop na systému Linux v HDInsight pomocí šablon Resource Manageru](hdinsight-hadoop-create-linux-clusters-arm-templates.md): Zjistěte, jak voláním šablon Resource Manageru vytvoříte clustery HDInsight.
* [Použití prázdných hraničních uzlů v HDInsight](hdinsight-apps-use-edge-node.md): Zjistěte, jak lze pomocí prázdných hraničních uzlů přistupovat ke clusteru HDInsight, testovat aplikace HDInsight a hostovat aplikace HDInsight.

<!--HONumber=Sep16_HO3-->


