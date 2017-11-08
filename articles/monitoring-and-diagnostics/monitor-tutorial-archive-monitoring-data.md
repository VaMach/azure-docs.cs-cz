---
title: "Archivovat Azure dat monitorování | Microsoft Docs"
description: "Archiv protokolu a metriku, data vytvořená v rámci Azure na účet úložiště."
author: johnkemnetz
manager: orenr
services: monitoring-and-diagnostics
documentationcenter: monitoring-and-diagnostics
ms.service: monitoring-and-diagnostics
ms.topic: tutorial
ms.date: 09/25/2017
ms.author: johnkem
ms.custom: mvc
ms.openlocfilehash: f19cf8fddd9ffcf08b8ce18db070a7482ce012df
ms.sourcegitcommit: 6a6e14fdd9388333d3ededc02b1fb2fb3f8d56e5
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/07/2017
---
# <a name="archive-azure-monitoring-data"></a>Archiv Azure dat monitorování

Několik vrstev prostředí Azure vytvořit protokolu a metriky data, která mohou být archivovány k účtu úložiště Azure. Můžete k tomu zachovat historii po uplynutí jeho doba uchovávání dat v Log Analytics nebo Azure monitorování dat monitorování dat časem v úložišti nenákladné, prohledávatelné. Tento kurz postup provede procesem konfigurace prostředí Azure k archivaci dat do účtu úložiště.

> [!div class="checklist"]
> * Vytvořit účet úložiště pro uložení dat monitorování
> * Protokoly odběru trasy k němu 
> * Data trasy prostředků do ní 
> * Směrovat dat virtuálního počítače (hostovaný operační systém) 
> * Zobrazení dat monitorování v ní 
> * Vyčištění prostředků 

Pokud ještě nemáte předplatné Azure, vytvořte si [bezplatný účet](https://azure.microsoft.com/free/) před tím, než začnete.

## <a name="sign-in-to-the-azure-portal"></a>Přihlášení k webu Azure Portal

Přihlaste se k webu [Azure Portal](https://portal.azure.com/).

## <a name="create-a-storage-account"></a>vytvořit účet úložiště

Nejdřív je potřeba nastavit účet úložiště, do které bude archivován data monitorování. K tomu, [postupujte podle kroků v tomto poli](../storage/common/storage-create-storage-account.md).

## <a name="route-subscription-logs-to-the-storage-account"></a>Protokoly odběru trasy k účtu úložiště

Nyní jste připraveni začít nastavení prostředí Azure pro účet úložiště se bude směrovat data monitorování. Nejprve nakonfigurujeme data na úrovni předplatného (obsažené v protokol činnosti Azure) k přesměrování na účet úložiště. [ **Protokol činnosti Azure** ](monitoring-overview-activity-logs.md) poskytuje historii událostí na úrovni předplatného v Azure. Můžete ho vyhledat na webu Azure portal k určení *kdo* vytvořené, aktualizovat nebo odstranit *co* prostředky a *při* tomu bylo ho.

1. Klikněte **monitorování** tlačítko nachází na levé navigační seznamu, pak na **protokol aktivit**.

   ![Část protokolu aktivit](media/monitor-tutorial-archive-monitoring-data/activity-log-home.png)

2. V části aktivity protokolu, který se zobrazí, klikněte na **exportovat** tlačítko.

3. V **Export protokolu aktivit** oddíl, který se zobrazí, zaškrtněte políčko pro **exportovat do účtu úložiště** a klikněte na tlačítko **vyberte účet úložiště.**

   ![Export protokolu aktivit](media/monitor-tutorial-archive-monitoring-data/activity-log-export.png)

4. V oddílu, který se zobrazí, použijte **účet úložiště** rozevíracího seznamu vyberte název účtu úložiště, který jste vytvořili v předchozím **vytvořit účet úložiště** krok a pak klikněte na **OK**.

   ![Vyberte účet úložiště](media/monitor-tutorial-archive-monitoring-data/activity-log-storage.png)

5. Nastavte **uchovávání dat (dny)** posuvníku 30. Tento jezdec nastaví počet dní, které chcete zachovat data sledování v účtu úložiště. Azure monitorování automaticky odstraní data starší než počet dní, zadaný. Uchování nulový počet dnů po neomezenou dobu ukládá data.

6. Klikněte na tlačítko **Uložit** a zavřete v této části.

Data ze svého předplatného monitorování je nyní předávaných do účtu úložiště.

## <a name="route-resource-data-to-the-storage-account"></a>Data trasy prostředek k účtu úložiště

Nyní nakonfigurujeme data na úrovni prostředků (metrika prostředků a diagnostických protokolů) směrovat do účtu úložiště nastavením **nastavení diagnostiky pro prostředek**.

1. Klikněte **monitorování** tlačítko nachází na levé navigační seznamu, pak na **nastavení pro diagnostiku**. Zde se zobrazí seznam všech prostředků v rámci vašeho předplatného, které vytváří monitorování dat pomocí Azure sledování. Pokud nemáte žádné prostředky v tomto seznamu, můžete [vytvoření aplikace logiky](../logic-apps/logic-apps-create-a-logic-app.md) než budete pokračovat, tak, aby na prostředek, který můžete nakonfigurovat nastavení diagnostiky na.

2. Klikněte na prostředek v seznamu a pak klikněte na tlačítko **zapněte diagnostiku**.
   
   ![Zapněte diagnostiku](media/monitor-tutorial-archive-monitoring-data/diagnostic-settings-turn-on.png)

   Pokud je již nakonfigurována nastavení, zobrazí místo stávající nastavení a tlačítko pro **přidat nastavení diagnostiky**. Kliknutím na toto tlačítko.

   Nastavení diagnostiky prostředků je definice *co* dat monitorování by měl směrovat z konkrétního zdroje a *kde* , by měli přejít dat monitorování.

3. V oddílu, který se zobrazí, zadejte vaše nastavení **název** a zaškrtněte políčko pro **archivu do účtu úložiště**.

   ![Část nastavení diagnostiky](media/monitor-tutorial-archive-monitoring-data/diagnostic-settings-home.png)

4. Klikněte na **konfigurace** tlačítko pod **archivu do účtu úložiště** a vyberte účet úložiště, který jste vytvořili v předchozí části. Klikněte na **OK**.

   ![Nastavení diagnostiky účtu úložiště](media/monitor-tutorial-archive-monitoring-data/diagnostic-settings-storage.png)

5. Zkontrolujte všechna pole v **protokolu** a **metrika**. V závislosti na typu prostředku může pouze mít jeden z těchto možností. Tato zaškrtnutí jednotlivých políček: řízení které kategorie protokolu a metriku, dat, které jsou k dispozici pro typ prostředku odešlou, které jste vybrali, v takovém případě cílového účtu úložiště.

   ![Nastavení diagnostiky kategorií](media/monitor-tutorial-archive-monitoring-data/diagnostic-settings-categories.png)
   
6. Nastavte **uchovávání dat (dny)** posuvníku 30. Tento jezdec nastaví počet dní, které chcete zachovat data sledování v účtu úložiště. Azure monitorování automaticky odstraní data starší než počet dní, zadaný. Uchování nulový počet dnů po neomezenou dobu ukládá data.

7. Klikněte na **Uložit**.

Monitorování dat z prostředku je nyní předávaných do účtu úložiště.

## <a name="route-virtual-machine-guest-os-data-to-the-storage-account"></a>Virtuální počítač (hostovaný operační systém) data trasy k účtu úložiště

1. Pokud jste ještě není virtuální počítač v rámci vašeho předplatného, [vytvořit virtuální počítač](../virtual-machines/windows/quick-create-portal.md).

2. V levém navigačním seznamu na portálu, klikněte na **virtuální počítače**.

3. V seznamu virtuálních počítačů, který se zobrazí klikněte na virtuální počítač, který jste vytvořili.

4. V oddílu, který se zobrazí, klikněte na **nastavení pro diagnostiku** na levé straně. V této části můžete nastavit rozšíření monitorování out-of-box z Azure monitorování na virtuální počítač a data trasy, která se vytváří pomocí systému Windows nebo Linux na účet úložiště.

   ![Přejděte na nastavení pro diagnostiku](media/monitor-tutorial-archive-monitoring-data/guest-navigation.png)

5. Klikněte na tlačítko **povolit sledování na úrovni hosta** v oddílu, který se zobrazí.

   ![Povolit nastavení diagnostiky](media/monitor-tutorial-archive-monitoring-data/guest-enable.png)

6. Po nastavení diagnostiky má správně uložené, **přehled** karta zobrazuje seznam shromažďovaných údajů a kde se ukládají. Klikněte na **čítače výkonu** čítače části Zkontrolovat sadu výkon systému Windows nejsou shromažďována.

   ![Nastavení čítačů výkonu](media/monitor-tutorial-archive-monitoring-data/guest-perf-counters.png)
   
7. Klikněte na **protokoly** kartě a zaškrtněte políčka pro **informace** úroveň protokolů v aplikaci a protokoly systému.

   ![Nastavení protokolů](media/monitor-tutorial-archive-monitoring-data/guest-logs.png)

8. Klikněte na **agenta** kartě a v části **účet úložiště** klikněte na název účtu úložiště, zobrazí.

   ![Aktualizovat účet úložiště](media/monitor-tutorial-archive-monitoring-data/guest-storage-home.png)

9. V oddílu, který se zobrazí, vyberte účet úložiště, který jste vytvořili v předchozím **vytvořit účet úložiště** krok.

10. Klikněte na **Uložit**.

Data z virtuálních počítačů monitorování je nyní předávaných do účtu úložiště.

## <a name="view-the-monitoring-data-in-the-storage-account"></a>Zobrazení dat monitorování v účtu úložiště

Pokud jste postupovali podle předchozích kroků, zahájil dat předávaných do účtu úložiště.

1. Pro některé typy dat, například protokol aktivit, je potřeba některé aktivity, která vygeneruje událost v účtu úložiště. Chcete-li vygenerovat aktivity v protokolu aktivit, postupujte podle [tyto pokyny](./monitor-quick-audit-notify-action-in-subscription.md). Musíte počkat až pět minut, než se zobrazí události v účtu úložiště.

2. Na portálu, přejděte na **účty úložiště** oddílu tak, že najdete na levém navigačním panelu.

3. Identifikace účtu úložiště, kterou jste vytvořili v předchozí části a klikněte na něm.

4. Klikněte na **objekty BLOB**, pak na kontejner s názvem bez přípony **statistiky operační protokoly** a nakonec na kontejner s názvem bez přípony **name = výchozí**. Toto je kontejner, který se nachází váš protokol aktivit. Sledování dat je rozdělená do kontejnerů podle ID prostředku (právě ID předplatného pro protokol aktivit) a pak podle data a času. Úplné formátování pro tyto objekty BLOB je:

   Statistika provozní protokoly/name = výchozí/resourceId = v odběry / {ID předplatného} / y = {čtyřciferné číselné year} / m = {dvoumístné číslo měsíce} / d = {letopočty numerický den} / h = {hour}/m=00/PT1H.json letopočty 24 hodin

5. Přejděte k souboru PT1H.json kliknutím na kontejnery pro ID prostředku, datum a čas. Kliknutím na soubor PT1H.json a klikněte na tlačítko **Stáhnout**. Každý objekt blob PT1H.json obsahuje objekt blob JSON událostí, které nastaly během hodiny zadaného v adrese URL objektu blob (například h = 12). Během přítomen hodinu jsou události připojen k souboru PT1H.json, kdy k nim dojde. Hodnota minutu (m = 00) je vždy 00, protože události protokolu jsou rozdělená do jednotlivých objektů blob za hodinu.

   Teď si můžete zobrazit JSON událost, která byla uložená v účtu úložiště. Diagnostické protokoly prostředků je formát pro objekty BLOB:

   insights - logs-{název kategorie protokolu} / resourceId = / {ResourceId} / y = {čtyřciferné číselné year} / m = {dvoumístné číslo měsíce} / d = {letopočty numerický den} / h = {hour}/m=00/PT1H.json letopočty 24 hodin

6. Data monitorování hostovaného operačního systému jsou uloženy v tabulkách. Přejděte zpět na domovskou účet úložiště a klikněte na tlačítko **tabulky**. Nejsou k dispozici tabulky pro metriku, čítače výkonu a protokoly událostí.

Jste teď úspěšně nastavili monitorování dat archivaci na účet úložiště.

## <a name="clean-up-resources"></a>Vyčištění prostředků

1. Přejděte zpět na **Export protokolu aktivit** části předchozím **směrovat protokoly odběru k účtu úložiště** kroku a klikněte na tlačítko **resetovat**.

2. Přejděte na **nastavení pro diagnostiku** části, klikněte na prostředek, na který jste vytvořili nastavení diagnostiky v předchozím **směrovat data prostředků k účtu úložiště** krok, pak nastavení najít můžete vytvořili, klikněte na **upravit nastavení** tlačítko a klikněte na tlačítko **odstranit**.

3. Přejděte na **nastavení pro diagnostiku** části na virtuálním počítači, který jste nakonfigurovali v předchozím **směrování dat virtuálního počítače (hostovaný operační systém) k účtu úložiště** kroku a v části  **Agent** klikněte na možnost **odebrat** (pod **odebrat Azure Diagnostics agenta** části).

4. Přejděte na účet úložiště, který jste vytvořili v předchozím **vytvořit účet úložiště** krok a klikněte na tlačítko **odstranit účet úložiště**. Zadejte název účtu úložiště a pak klikněte na tlačítko **odstranit**.

5. Pokud jste vytvořili virtuálního počítače nebo aplikace logiky předchozí kroky, i ty odstraňte.

## <a name="next-steps"></a>Další kroky

V tomto kurzu jste zjistili, jak nastavit dat monitorování od prostředí Azure (předplatné, prostředků a hostovaný operační systém) archivaci na účet úložiště. 


> [!div class="checklist"]
> * Vytvořit účet úložiště pro uložení dat monitorování
> * Protokoly odběru trasy k němu 
> * Data trasy prostředků do ní 
> * Směrovat dat virtuálního počítače (hostovaný operační systém) 
> * Zobrazení dat monitorování v ní 
> * Vyčištění prostředků 

Pokud chcete získat další mimo vaše data a jsou odvozeny další statistiky, také odesílat data do analýzy protokolů.

> [!div class="nextstepaction"]
> [Začínáme s analýzy protokolů](../log-analytics/log-analytics-get-started.md)
