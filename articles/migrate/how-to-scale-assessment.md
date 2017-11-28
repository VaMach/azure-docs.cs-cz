---
title: "Škálování zjišťování a hodnocení s Azure migrovat | Microsoft Docs"
description: "Popisuje, jak k vyhodnocení velkého počtu místní počítače se službou Azure migrovat."
services: migrate
documentationcenter: 
author: rayne-wiselman
manager: carmonm
editor: 
ms.assetid: dde0d07f-94b7-4b6a-a158-a89aa9324a35
ms.service: migrate
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
ms.date: 11/22/2017
ms.author: raynew
ms.openlocfilehash: 930ec182cf329e7dda072dc49bd7f70abb413f2d
ms.sourcegitcommit: f847fcbf7f89405c1e2d327702cbd3f2399c4bc2
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/28/2017
---
# <a name="discover-and-assess-a-large-vmware-environment"></a>Zjišťovat a vyhodnocení velké prostředí VMware

Tento článek popisuje, jak k vyhodnocení velký počet počítačů v místě s [Azure migrovat](migrate-overview.md). Azure migrací vyhodnocuje počítačů zkontrolujte, jestli jste vhodné pro migraci na Azure a poskytuje velikost a cenově odhady pro spuštění na počítač v Azure.

## <a name="prerequisites"></a>Požadavky

- **VMware**: budete potřebovat alespoň jeden virtuální počítač VMware umístěný na hostiteli ESXi nebo clusteru spuštěna verze 5.0 nebo vyšší. Server vCenter používá verzi 5.5 nebo 6.0 musí spravovat hostitele nebo cluster.
- **účet vCenter**: je třeba účet jen pro čtení s přihlašovacími údaji správce pro vCenter server. Azure migrací používá tento účet k vyhledání virtuálních počítačů.
- **Oprávnění**: na serveru vCenter server, potřebujete oprávnění k vytvoření virtuálního počítače importováním souboru v. VAJÍČKA formátu.
- **Nastavení statistiky**: nastavení statistiky pro vCenter server měli nastavit úroveň 2 nebo vyšší, než zahájíte nasazení.

## <a name="plan-azure-migrate-projects"></a>Plánování migrace Azure projekty

Projekt migrovat Azure můžete vyhodnotit až 1500 počítače. Jeden zjišťování v projektu může zjišťovat až 1 000 počítačů.

- Pokud máte méně než 1 000 počítačů, které chcete zjistit, je nutné jeden projekt pomocí jednoho zjišťování.
- Pokud máte mezi 1 000 a 1500 počítače, je třeba jeden projekt se dvěma zjišťování v ní.
- Pokud máte více než 1500 počítače, budete muset vytvořit více projektů a provádět více zjišťování, podle svých požadavků. Například:
    - Pokud máte 3000 počítače, můžete nastavit dva projekty s zjišťování dvou nebo tří projektů pomocí jednoho zjišťování.
    - Pokud máte 5000 počítače, můžete nastavit až čtyři projekty. Dva s zjišťování počítačů 1500 a s zjišťování 500 počítačů. Alternativně můžete nastavit až pět projekty s jeden zjišťování v každé z nich. 
- Při zjišťování v Azure migrace, můžete nastavit obor zjišťování ke složce VMware, datacenter nebo clusteru.
- Udělat víc než jedno zjišťování, ověřte v vCenter, virtuální počítače, které chcete zjistit, jsou v složek, datových centrech nebo clustery, které podporují omezení 1 000 počítačů.
- Doporučujeme, abyste pro účely hodnocení, udržovat počítače, které mají mezi závislosti v rámci stejného projektu a hodnocení. Proto v vCenter, ujistěte se, že závislé počítače jsou ve stejné složce, datacenter nebo clusteru pro účely hodnocení.


## <a name="create-a-project"></a>Vytvoření projektu

Vytvoření projektu Azure migraci v souladu s vašim požadavkům.

1. Na portálu Azure klikněte na tlačítko **vytvořit prostředek**.
2. Vyhledejte **Azure migrovat**a vyberte službu (**migrovat Azure (preview)** ve výsledcích hledání. Poté klikněte na **Vytvořit**.
3. Zadejte název projektu a předplatné Azure pro projekt.
4. Vytvořte novou skupinu prostředků.
5. Zadejte oblast, ve kterém vytvořte projekt a pak klikněte na **vytvořit**. Metadata získané z místní virtuálních počítačů jsou uložené v této oblasti.

## <a name="set-up-the-collector-appliance"></a>Nastavení kolekce zařízení

Azure migraci vytvoří místní počítač známé jako kolekce zařízení. Tento virtuální počítač vyhledá virtuální počítače VMware na místě a metadata o nich se odešle do služby Azure migrovat. Chcete-li nastavit kolekce zařízení, je stáhnout. VAJÍČKA souboru a naimportujte ho do místního serveru vCenter vytvořte virtuální počítač.

### <a name="download-the-collector-appliance"></a>Stáhnout kolekce zařízení

Pokud máte více projektů, stačí jednou stáhnout kolekce zařízení k serveru vCenter. Po stažení a nastavení zařízení, spusťte pro každý projekt a zadejte jedinečný projektu ID a klíč.

1. V projektu Azure migrovat, klikněte na tlačítko **Začínáme** > **Discover & hodnocení** > **zjišťovat počítače**.
2. V **zjistit počítače**, klikněte na tlačítko **Stáhnout**, chcete-li stáhnout. VAJÍČKA soubor.
3. V **zkopírujte projektu pověření**, zkopírujte ID a klíč pro projekt. Tyto musíte při konfiguraci kolekce.

   
### <a name="verify-the-collector-appliance"></a>Ověřte kolekce zařízení

Zkontrolujte, zda. Soubor vajíčka se zabezpečení, před nasazením.

1. Na počítači, do které jste stáhli soubor otevřete příkazové okno na správce.
2. Spusťte následující příkaz pro generování hodnoty hash pro vajíčka:
    - ```C:\>CertUtil -HashFile <file_location> [Hashing Algorithm]```
    - Příklad použití:```C:\>CertUtil -HashFile C:\AzureMigrate\AzureMigrate.ova SHA256```
3. Toto nastavení by měl odpovídat generované hodnoty hash.

    **Algoritmus** | **Hodnota hash**
    --- | ---
    ALGORITMUS MD5 | c283f00f46484bf673dc8383b01d0741 
    SHA1 | 8f49b47f53d051af1780bbc725659ce64e717ed4
    SHA256 | 7aecdbdb2aea712efe99d0c1444503f52d16de5768e783465e226fbbca71501d

## <a name="create-the-collector-vm"></a>Vytvoření kolekce virtuálních počítačů

Stažený soubor importujte do serveru vCenter server.

1. V konzole klienta vSphere klikněte na tlačítko **soubor** > **nasazení šablony OVF**.

    ![Nasazení OVF](./media/how-to-scale-assessment/vcenter-wizard.png)

2. V Průvodci nasazením šablony OVF > **zdroj**, zadejte umístění souboru .ova.
3. V **název** a **umístění**, zadejte popisný název pro kolekci virtuálních počítačů a inventář objektu, ve kterém se bude hostovat virtuální počítač.
5. V **hostiteli/clusteru**, zadejte hostitele nebo clusteru v kolekci virtuálních počítačů se spustí.
7. V úložišti zadejte cíl úložiště pro kolekci virtuálních počítačů.
8. V **formát disku**, zadejte typ disku a velikost.
9. V **mapování sítě**, zadejte síť, ke kterému se připojí kolekce virtuálních počítačů. Síť musí připojení k Internetu, k odeslání metadata do Azure. 
10. Zkontrolujte a potvrďte nastavení a pak klikněte na **Dokončit**.

## <a name="identify-the-key-and-id-for-each-project"></a>Identifikace klíče a ID pro každý projekt

Pokud máte více projektů, ujistěte se, určit ID a klíč pro každé z nich. Klíč musíte při spuštění kolekce ke zjišťování virtuálních počítačů.

1. V projektu, klikněte na tlačítko **Začínáme** > **Discover & hodnocení** > **zjišťovat počítače**.
2. V **zkopírujte projektu pověření**, zkopírujte ID a klíč pro projekt. 
    ![ID projektu](./media/how-to-scale-assessment/project-id.png)

## <a name="vcenter-statistics-level-to-collect-the-performance-counters"></a>vCenter úroveň statistiky ke shromažďování čítačů výkonu
Následuje seznam čítačů, které byly shromážděny během zjišťování. Tyto čítače jsou ve výchozím nastavení k dispozici na různé úrovni v serveru vCenter server. Doporučujeme že nastavit nejvyšší běžné úrovni (úroveň 3) pro úroveň statistiky tak, aby všechny čítače jsou shromažďovány správně. Pokud máte vCenter nastavit na nižší úrovni, mohou získat jen pár čítače shromažďovány úplně, se zbytkem nastaven na hodnotu 0. Posouzení proto může zobrazovat neúplná data. Níže uvedená tabulka také uvádí výsledky hodnocení, které bude mít vliv, pokud nejsou zjištěny jednotlivých čítačů.

|Čítač                                  |Úroveň    |Na úrovni zařízení  |Dopad hodnocení                               |
|-----------------------------------------|---------|------------------|------------------------------------------------|
|CPU.Usage.average                        | 1       |Není k dispozici                |Doporučená velikost virtuálního počítače a náklady                    |
|mem.Usage.average                        | 1       |Není k dispozici                |Doporučená velikost virtuálního počítače a náklady                    |
|virtualDisk.read.average                 | 2       |2                 |Velikost disku, náklady na úložiště a velikost virtuálního počítače         |
|virtualDisk.write.average                | 2       |2                 |Velikost disku, náklady na úložiště a velikost virtuálního počítače         |
|virtualDisk.numberReadAveraged.average   | 1       |3                 |Velikost disku, náklady na úložiště a velikost virtuálního počítače         |
|virtualDisk.numberWriteAveraged.average  | 1       |3                 |Velikost disku, náklady na úložiště a velikost virtuálního počítače         |
|NET.Received.average                     | 2       |3                 |Velikost virtuálního počítače a sítě náklady.                        |
|NET.Transmitted.average                  | 2       |3                 |Velikost virtuálního počítače a sítě náklady.                        |

> [!WARNING]
> Pokud jste právě nastavili na vyšší úrovni statistiky, bude trvat až denně ke generování čítače výkonu. Proto se doporučuje, spuštění zjišťování po uplynutí jednoho dne.

## <a name="run-the-collector-to-discover-vms"></a>Spustit kolekce k vyhledání virtuálních počítačů

Pro každého zjišťování je třeba provést spusťte kolekce ke zjišťování virtuálních počítačů v oboru vyžaduje. Spusťte zjišťování jedna po druhé. Nejsou podporovány souběžných zjišťování a každého zjišťování musí mít jiný rozsah.

1. V konzole klienta vSphere klikněte pravým tlačítkem na virtuální počítač > **otevřít konzolu**.
2. Zadejte jazyk, časové pásmo a heslo předvoleb pro zařízení.
3. Na ploše klikněte **spustit kolekce** zástupce.
4. V Azure migraci kolekce, otevřete **nastavit požadavky**.
    - Přijměte licenční podmínky a číst informace o jiných výrobců.
    - Kolekce kontroluje, zda virtuální počítač má přístup k Internetu.
    - Pokud virtuální počítač získá přístup k Internetu prostřednictvím proxy serveru, klikněte na tlačítko **nastavení proxy serveru**a zadat adresu proxy serveru a port naslouchání. Pokud proxy server vyžaduje ověřování, zadejte přihlašovací údaje.
    - Kolekce kontroluje, zda je spuštěna služba systému Windows profileru. Služba je nainstalovaná ve výchozím nastavení v kolekci virtuálních počítačů.
    - Stáhněte a nainstalujte VMware PowerCLI.
. V **zjišťovat počítače**, postupujte takto:
    - Zadejte název (FQDN) nebo IP adresa serveru vCenter.
    - V **uživatelské jméno** a **heslo**, zadejte pověření účtu jen pro čtení, které kolekce použije k vyhledání virtuálních počítačů na serveru vCenter server.
    - V **kolekce oboru**, vyberte obor pro zjišťování virtuálních počítačů. Kolekce může zjistit pouze virtuální počítače v zadaném oboru. Obor lze nastavit na konkrétní složce, datacenter nebo clusteru. Měl by neměl obsahovat více než 1 000 virtuálních počítačů. 
    - n **Značka kategorii pro seskupení**, vyberte **žádné**.

        ![Vyberte obor](./media/how-to-scale-assessment/select-scope.png)

1. V **vyberte projekt**, zadejte ID a klíč pro projekt. Pokud nebylo je zkopírovat, otevřete portál Azure z kolekce virtuálních počítačů. V projektu **přehled** klikněte na tlačítko **zjišťovat počítače**a zkopírujte hodnoty.  
V **dokončení zjišťování**, monitorovat proces zjišťování a zkontrolujte, že metadata shromážděných z virtuálních počítačů jsou v oboru. Kolekce poskytuje čas přibližnou zjišťování.


### <a name="verify-vms-in-the-portal"></a>Ověření virtuálních počítačů na portálu

Zjišťování, že doba závisí na tom, kolik virtuálních počítačů zjišťujete. Obvykle u 100 virtuálních počítačů po dokončení kolekce jeho spuštění trvá přibližně za hodinu zjišťování. 

1. V projektu Planner migrace, klikněte na tlačítko **spravovat** > **počítače**.
2. Zkontrolujte, jestli chcete zjistit virtuální počítače se zobrazovat na portálu.


## <a name="next-steps"></a>Další kroky

- Zjistěte, jak [vytvořte skupinu](how-to-create-a-group.md) pro vyhodnocení.
- [Další informace](concepts-assessment-calculation.md) o tom, jak jsou vypočítávány vyhodnocování.