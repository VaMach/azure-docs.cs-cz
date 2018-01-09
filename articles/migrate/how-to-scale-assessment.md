---
title: "Škálování zjišťování a hodnocení pomocí Azure migrovat | Microsoft Docs"
description: "Popisuje, jak k vyhodnocení velký počet počítačů místně pomocí služby Azure migrovat."
author: rayne-wiselman
ms.service: azure-migrate
ms.topic: article
ms.date: 01/08/2018
ms.author: raynew
ms.openlocfilehash: 67661e03e65cde3ec2f1aafd5ef755899cf0c77b
ms.sourcegitcommit: 9a8b9a24d67ba7b779fa34e67d7f2b45c941785e
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/08/2018
---
# <a name="discover-and-assess-a-large-vmware-environment"></a>Zjišťovat a vyhodnocení velké prostředí VMware

Tento článek popisuje, jak k vyhodnocení velkého počtu místní virtuální počítače (VM) pomocí [Azure migrovat](migrate-overview.md). Azure migrací vyhodnocuje počítače, které chcete zkontrolovat, zda jsou vhodné pro migraci na Azure. Služba poskytuje velikost a cenově odhady pro spouštění počítačů v Azure.

## <a name="prerequisites"></a>Požadavky

- **VMware**: virtuálních počítačů, která chcete migrovat se musí spravovat přes vCenter Server 5.5, 6.0 nebo 6.5 verze. Kromě toho musíte jedna ESXi hostitele spuštěné verze 5.0 nebo novější k nasazení kolekcí virtuálních počítačů.
- **účet vCenter**: je třeba účet jen pro čtení pro přístup k systému vCenter Server. Azure migrací používá tento účet k vyhledání virtuálních počítačů na místě.
- **Oprávnění**: V systému vCenter Server, potřebujete oprávnění k vytvoření virtuálního počítače importováním souboru ve formátu vajíčka.
- **Nastavení statistiky**: nastavení statistiky pro vCenter Server musí být nastavená na úroveň 3 před zahájením nasazení. Pokud úroveň je nižší než 3, hodnocení bude fungovat, ale nebudou shromažďovány údaje o výkonu pro úložiště a sítě. Velikost doporučení v tomto případě budou založeny na údaje o výkonu pro procesor a paměť a konfigurační data pro disk a síťové adaptéry.

## <a name="plan-azure-migrate-projects"></a>Plánování migrace Azure projekty

Plánování zjišťování a vyhodnocování podle následující omezení:

| **Entity** | **Limit pro počítače** |
| ---------- | ----------------- |
| Project    | 1,500              | 
| Zjišťování  | 1 000              |
| Posouzení | 400               |

- Pokud máte méně než 400 počítače ke zjištění a vyhodnocení, budete potřebovat jeden projekt a jeden zjišťování. V závislosti na požadavcích můžete vyhodnotit všechny počítače v jedné assessment nebo na počítače rozdělí na několik vyhodnocování. 
- Pokud máte 400 na 1000 počítače ke zjištění, je třeba jeden projekt pomocí jednoho zjišťování. Ale budete potřebovat více vyhodnocování k vyhodnocení tyto počítače, protože jeden assessment pojme až 400 počítače.
- Pokud máte počítače 1,001 na 1 500, je třeba jeden projekt se dvěma zjišťování v ní.
- Pokud máte více než 1 500 počítače, budete muset vytvořit více projektů a provádět více zjišťování, podle svých požadavků. Příklad:
    - Pokud máte 3 000 počítačů, můžete nastavit dva projekty s zjišťování dvou nebo tří projektů pomocí jednoho zjišťování.
    - Pokud máte 5 000 počítačů, můžete nastavit čtyři projekty: dva zjišťování 1 500 počítačů a s zjišťování 500 počítačů. Alternativně můžete nastavit pět projekty s jeden zjišťování v každé z nich. 

## <a name="plan-multiple-discoveries"></a>Plánování více zjišťování

Stejné kolekce Azure migraci můžete udělat několik zjišťování na jeden nebo více projekty. Tyto aspekty plánování mějte na paměti:
 
- Při zjišťování pomocí Azure migraci kolekce, můžete nastavit obor zjišťování pro složku na serveru vCenter, datacenter, cluster nebo hostitele.
- Více než jedno zjišťování proveďte ověření v systému vCenter Server, které jsou virtuální počítače, které chcete zjistit v složek, datových center, clusterům nebo hostitele, kteří podporují omezení 1 000 počítačů.
- Doporučujeme, aby pro účely hodnocení, byl počítače s vzájemné závislosti v rámci stejného projektu a hodnocení. V systému vCenter Server Ujistěte se, že závislé počítače jsou ve stejné složce, datacenter nebo clusteru pro hodnocení.


## <a name="create-a-project"></a>Vytvořit projekt

Vytvoření projektu Azure migraci v souladu s vaší požadavky:

1. Na portálu Azure vyberte **vytvořit prostředek**.
2. Vyhledejte **Azure migrovat**a vyberte službu **migrovat Azure (preview)** ve výsledcích hledání. Potom vyberte **Vytvořit**.
3. Zadejte název projektu a předplatné Azure pro projekt.
4. Vytvořte novou skupinu prostředků.
5. Zadejte umístění, ve kterém chcete vytvořit projekt a potom vyberte **vytvořit**. Všimněte si, že můžete stále vyhodnotit virtuálních počítačů pro jiné cílové umístění. K umístění zadanému pro projekt se používá k ukládání metadat získané z virtuálních počítačů na místě.

## <a name="set-up-the-collector-appliance"></a>Nastavení kolekce zařízení

Azure migraci vytvoří místní počítač známé jako kolekce zařízení. Tento virtuální počítač vyhledá virtuální počítače VMware na místě a metadata o nich odešle do služby Azure migrovat. Pokud chcete nastavit zařízení kolekce, stáhněte si soubor vajíčka, importujte je do místní instance serveru vCenter.

### <a name="download-the-collector-appliance"></a>Stáhnout kolekce zařízení

Pokud máte více projektů, budete muset stáhnout kolekce zařízení pouze jednou k systému vCenter Server. Po stažení a nastavení zařízení, můžete spustit pro každý projekt, a zadáte projektu jedinečné ID a klíč.

1. V projektu Azure migrovat vyberte **Začínáme** > **Discover & hodnocení** > **zjišťovat počítače**.
2. V **zjistit počítače**, vyberte **Stáhnout**, chcete-li stáhnout soubor vajíčka.
3. V **zkopírujte projektu pověření**, zkopírujte ID a klíč pro projekt. Tyto musíte při konfiguraci kolekce.

   
### <a name="verify-the-collector-appliance"></a>Ověřte kolekce zařízení

Zkontrolujte, jestli soubor vajíčka zabezpečené před nasazením:

1. Na počítači, do které jste stáhli soubor otevřete příkazové okno na správce.
2. Spusťte následující příkaz pro generování hodnoty hash pro vajíčka:

   ```C:\>CertUtil -HashFile <file_location> [Hashing Algorithm]```

   Příklad použití:```C:\>CertUtil -HashFile C:\AzureMigrate\AzureMigrate.ova SHA256```
3. Ujistěte se, že generované hodnoty hash odpovídá následující nastavení.
 
    Pro verzi vajíčka 1.0.8.49

    **Algoritmus** | **Hodnota hash**
    --- | ---
    MD5 | 8779eea842a1ac465942295c988ac0c7
    SHA1 | c136c52a0f785e1fd98865e16479dd103704887d
    SHA256 | 5143b1144836f01dd4eaf84ff94bc1d2c53f51ad04b1ca43ade0d14a527ac3f9

    Verze vajíčka 1.0.8.40:

    **Algoritmus** | **Hodnota hash**
    --- | ---
    MD5 |afbae5a2e7142829659c21fd8a9def3f
    SHA1 | 1751849c1d709cdaef0b02a7350834a754b0e71d
    SHA256 | d093a940aebf6afdc6f616626049e97b1f9f70742a094511277c5f59eacc41ad

## <a name="create-the-collector-vm"></a>Vytvoření kolekce virtuálních počítačů

Stažený soubor importujte do systému vCenter Server:

1. V konzole klienta vSphere vyberte **soubor** > **nasazení šablony OVF**.

    ![Nasazení OVF](./media/how-to-scale-assessment/vcenter-wizard.png)

2. V Průvodci nasazením šablony OVF > **zdroj**, zadejte umístění souboru vajíčka.
3. V **název** a **umístění**, zadejte popisný název pro kolekci virtuálních počítačů a inventář objektu, ve kterém se bude hostovat virtuální počítač.
5. V **hostiteli/clusteru**, zadejte hostitele nebo clusteru v kolekci virtuálních počítačů se spustí.
7. V úložišti zadejte cíl úložiště pro kolekci virtuálních počítačů.
8. V **formát disku**, zadejte typ disku a velikost.
9. V **mapování sítě**, zadejte síť, ke kterému se připojí kolekce virtuálních počítačů. Síť musí připojení k Internetu k odeslání metadata do Azure. 
10. Zkontrolujte a potvrďte nastavení a potom vyberte **Dokončit**.

## <a name="identify-the-id-and-key-for-each-project"></a>Určit ID a klíč pro každý projekt

Pokud máte více projektů, nezapomeňte určit ID a klíč pro každé z nich. Klíč musíte při spuštění kolekce ke zjišťování virtuálních počítačů.

1. V projektu, vyberte **Začínáme** > **Discover & hodnocení** > **zjišťovat počítače**.
2. V **zkopírujte projektu pověření**, zkopírujte ID a klíč pro projekt. 
    ![Zkopírujte pověření projektu](./media/how-to-scale-assessment/copy-project-credentials.png)

## <a name="set-the-vcenter-statistics-level"></a>Nastavení úrovně statistiky vCenter
Následuje seznam čítačů výkonu, které byly shromážděny během zjišťování. Tyto čítače jsou ve výchozím nastavení k dispozici na různých úrovních v systému vCenter Server. 

Doporučujeme nastavit nejvyšší běžné úroveň (3) pro úroveň statistiky tak, aby všechny čítače jsou shromažďovány správně. Pokud máte vCenter nastavit na nižší úrovni, mohou být pouze několik čítače shromažďovány úplně, se zbytkem nastaven na hodnotu 0. Posouzení pak může zobrazovat neúplná data. 

Následující tabulka uvádí také výsledky hodnocení, které bude mít vliv, pokud nejsou zjištěny jednotlivých čítačů.

|Čítač                                  |Úroveň    |Úroveň za zařízení  |Dopad hodnocení                               |
|-----------------------------------------|---------|------------------|------------------------------------------------|
|CPU.Usage.average                        | 1       |Není k dispozici                |Doporučená velikost virtuálního počítače a náklady                    |
|mem.Usage.average                        | 1       |Není k dispozici                |Doporučená velikost virtuálního počítače a náklady                    |
|virtualDisk.read.average                 | 2       |2                 |Velikost disku, náklady na úložiště a velikost virtuálního počítače         |
|virtualDisk.write.average                | 2       |2                 |Velikost disku, náklady na úložiště a velikost virtuálního počítače         |
|virtualDisk.numberReadAveraged.average   | 1       |3                 |Velikost disku, náklady na úložiště a velikost virtuálního počítače         |
|virtualDisk.numberWriteAveraged.average  | 1       |3                 |Velikost disku, náklady na úložiště a velikost virtuálního počítače         |
|NET.Received.average                     | 2       |3                 |Náklady na velikost a sítě virtuálních počítačů                        |
|NET.Transmitted.average                  | 2       |3                 |Náklady na velikost a sítě virtuálních počítačů                        |

> [!WARNING]
> Pokud jste právě nastavili na vyšší úrovni statistiky, bude trvat jeden den ke generování čítače výkonu. Ano doporučujeme spustit zjišťování po uplynutí jednoho dne.

## <a name="run-the-collector-to-discover-vms"></a>Spustit kolekce k vyhledání virtuálních počítačů

U každého zjišťování, které je třeba provést spusťte kolekce k vyhledání virtuálních počítačů v oboru vyžaduje. Spusťte zjišťování jedna po druhé. Nejsou podporovány souběžných zjišťování a každého zjišťování musí mít jiný rozsah.

1. V konzole klienta vSphere klikněte pravým tlačítkem na virtuální počítač > **otevřít konzolu**.
2. Zadejte jazyk, časové pásmo a heslo předvoleb pro zařízení.
3. Na ploše, vyberte **spustit kolekce** zástupce.
4. V kolekci Azure migrovat otevřete **nastavit požadavky** a pak:

   a. Přijměte licenční podmínky a číst informace o jiných výrobců.

   Kolekce kontroluje, zda virtuální počítač má přístup k Internetu.
   
   b. Pokud virtuální počítač získá přístup k Internetu prostřednictvím proxy serveru, vyberte **nastavení proxy serveru**a zadat adresu proxy serveru a port naslouchání. Pokud proxy server vyžaduje ověřování, zadejte přihlašovací údaje.

   Kolekce kontroluje, zda je spuštěna služba collector. Služba je nainstalovaná ve výchozím nastavení v kolekci virtuálních počítačů.

   c. Stáhněte a nainstalujte VMware PowerCLI.

5. V **zadejte podrobnosti o serveru vCenter**, postupujte takto:
    - Zadejte název (FQDN) nebo IP adresa serveru vCenter.
    - V **uživatelské jméno** a **heslo**, zadejte pověření účtu jen pro čtení, které kolekce použije k vyhledání virtuálních počítačů v systému vCenter Server.
    - V **vyberte obor**, vyberte obor pro zjišťování virtuálních počítačů. Kolekce může zjišťovat pouze virtuální počítače v zadaném oboru. Obor lze nastavit na konkrétní složce, datacenter nebo clusteru. Měl by neměl obsahovat více než 1 000 virtuálních počítačů. 

6. V **zadejte migrace projektu**, zadejte ID a klíč pro projekt. Pokud zkopírujete nebyla je, otevřete portál Azure z kolekce virtuálních počítačů. V projektu **přehled** vyberte **zjišťovat počítače** a zkopírujte hodnoty.  
7. V **sledovat průběh kolekce**, monitorovat proces zjišťování a zkontrolujte, že metadata shromážděných z virtuálních počítačů jsou v oboru. Kolekce poskytuje čas přibližnou zjišťování.


### <a name="verify-vms-in-the-portal"></a>Ověření virtuálních počítačů na portálu

Zjišťování, že doba závisí na tom, kolik virtuálních počítačů zjišťujete. U 100 virtuálních počítačů obvykle zjišťování dokončení kolem hodinu po dokončení spuštění kolekce. 

1. V projektu migrace Planner vyberte **spravovat** > **počítače**.
2. Zkontrolujte, jestli chcete zjistit virtuální počítače se zobrazovat na portálu.


## <a name="next-steps"></a>Další postup

- Zjistěte, jak [vytvořte skupinu](how-to-create-a-group.md) pro vyhodnocení.
- [Další informace](concepts-assessment-calculation.md) o tom, jak jsou vypočítávány vyhodnocování.
