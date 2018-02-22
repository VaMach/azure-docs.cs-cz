---
title: "Zjištění místních virtuálních počítačů VMware a posouzení vhodnosti jejich migrace do Azure pomocí služby Azure Migrate | Microsoft Docs"
description: "Toto téma popisuje, jak zjistit místní virtuální počítače VMware a posoudit vhodnost jejich migrace do Azure pomocí služby Azure Migrate."
author: rayne-wiselman
ms.service: azure-migrate
ms.topic: tutorial
ms.date: 01/08/2018
ms.author: raynew
ms.custom: mvc
ms.openlocfilehash: bbcfe95f5427681f8d55d647b102d35fc37f15ee
ms.sourcegitcommit: 95500c068100d9c9415e8368bdffb1f1fd53714e
ms.translationtype: HT
ms.contentlocale: cs-CZ
ms.lasthandoff: 02/14/2018
---
# <a name="discover-and-assess-on-premises-vmware-vms-for-migration-to-azure"></a>Zjištění místních virtuálních počítačů VMware a posouzení vhodnosti jejich migrace do Azure

Služba [Azure Migrate](migrate-overview.md) posuzuje místní úlohy pro migraci do Azure.

V tomto kurzu se naučíte:

> [!div class="checklist"]
> * Jak vytvořit projekt Azure Migrate.
> * Jak nastavit místní virtuální počítač kolektoru ke zjištění a posouzení místních virtuálních počítačů VMware.
> * Jak seskupit virtuální počítače a posoudit, jestli jsou vhodné pro migraci.


Pokud ještě nemáte předplatné Azure, vytvořte si [bezplatný účet](https://azure.microsoft.com/pricing/free-trial/) před tím, než začnete.


## <a name="prerequisites"></a>Požadavky

- **VMware**: Virtuální počítače, pro které plánujete migraci, musí být spravované přes vCenter Server verze 5.5, 6.0, nebo 6.5. Pro vytvoření virtuálního počítače kolektoru budete také potřebovat jednoho hostitele ESXi ve verzi 5.0 nebo vyšší. 
 
> [!NOTE]
> Podpora Hyper-V je součástí našich plánů a bude brzy k dispozici. 

- **Účet vCenter Serveru**: Abyste měli přístup k vCenter Serveru, potřebujete účet jen pro čtení. Azure Migrate ho použije ke zjištění místních virtuálních počítačů.
- **Oprávnění**: Na vCenter Serveru potřebujete oprávnění k vytvoření virtuálního počítače pomocí importu souboru ve formátu .OVA. 
- **Nastavení statistiky**: Než začnete s nasazením, statistika pro vCenter Server by byla být nastavená na úrovni 3. Pokud bude nižší než na úrovni 3, posouzení bude fungovat, ale neshromáždí se údaje o výkonu úložišť a sítí. Kapacitní doporučení budou v tomto případě vycházet z údajů o výkonu procesoru a paměti a z údajů o konfiguraci disku a síťových adaptérů. 

## <a name="log-in-to-the-azure-portal"></a>Přihlášení k portálu Azure Portal
Přihlaste se k portálu [Azure Portal](https://portal.azure.com).

## <a name="create-a-project"></a>Vytvoření projektu

1. Na portálu Azure Portal klikněte na **Vytvořit prostředek**.
2. Vyhledejte **Azure Migrate** a ve výsledcích hledání vyberte službu **Azure Migrate (Preview)**. Poté klikněte na **Vytvořit**.
3. Zadejte název projektu a předplatné Azure, do kterého spadá.
4. Vytvořte novou skupinu prostředků.
5. Zadejte umístění, ve kterém chcete projekt vytvořit, a pak klikněte na **Vytvořit**. Projekt Azure Migrate můžete ve službě verze Preview vytvořit pouze v oblasti Středozápad USA. Přesto ale můžete naplánovat migraci do libovolného cílového umístění Azure. Umístění vybrané pro tento projekt slouží jen k uložení metadat získaných z místních virtuálních počítačů. 

    ![Azure Migrate](./media/tutorial-assessment-vmware/project-1.png)
    


## <a name="download-the-collector-appliance"></a>Stažení zařízení kolektoru

Azure Migrate vytvoří místní virtuální počítač, kterému se říká zařízení kolektoru. Tento virtuální počítač vyhledá místní virtuální počítače VMware a odešle jejich metadata do služby Azure Migrate. Pro nastavení zařízení kolektoru je nutné stáhnout soubor .OVA, importovat ho do místního vCenter Serveru a vytvořit tak virtuální počítač.

1. V projektu služby Azure Migrate klikněte na **Začínáme** > **Zjistit a posoudit** > **Zjistit počítače**.
2. V nabídce **Zjistit počítače** klikněte na **Stáhnout**. Tím stáhnete soubor .OVA.
3. V části **Kopírování přihlašovacích údajů projektu** zkopírujte ID a klíč projektu. Budete je potřebovat při konfiguraci kolektoru.

    ![Stažení souboru .OVA](./media/tutorial-assessment-vmware/download-ova.png)

### <a name="verify-the-collector-appliance"></a>Ověření zařízení kolektoru

Než nasadíte soubor .OVA, zkontrolujte, jestli je bezpečný.

1. Na počítači, do kterého jste soubor stáhli, otevřete jako správce příkazový řádek.
2. Spusťte následující příkaz, kterým vygenerujete hodnotu hash pro soubor OVA:
    - ```C:\>CertUtil -HashFile <file_location> [Hashing Algorithm]```
    - Příklady použití: ```C:\>CertUtil -HashFile C:\AzureMigrate\AzureMigrate.ova SHA256```
3. Vygenerovaná hodnota hash by měla odpovídat následujícímu nastavení.
    
    Pro soubory OVA verze 1.0.8.49:
    **Algoritmus** | **Hodnota hash**
    --- | ---
    MD5 | cefd96394198b92870d650c975dbf3b8 
    SHA1 | 4367a1801cf79104b8cd801e4d17b70596481d6f
    SHA256 | fda59f076f1d7bd3ebf53c53d1691cc140c7ed54261d0dc4ed0b14d7efef0ed9

    Pro soubory OVA verze 1.0.8.40:

    **Algoritmus** | **Hodnota hash**
    --- | ---
    MD5 | afbae5a2e7142829659c21fd8a9def3f
    SHA1 | 1751849c1d709cdaef0b02a7350834a754b0e71d
    SHA256 | d093a940aebf6afdc6f616626049e97b1f9f70742a094511277c5f59eacc41ad

## <a name="create-the-collector-vm"></a>Vytvoření virtuálního počítače kolektoru

Importujte stažený soubor do vCenter Serveru.

1. V konzoli vSphere Client klikněte na **File** (Soubor) > **Deploy OVF Template** (Nasadit šablonu OVF).

    ![Nasazení šablony OVF](./media/tutorial-assessment-vmware/vcenter-wizard.png)

2. V nabídce Deploy OVF Template Wizard (Průvodce nasazením šablony OVF) > **Source** (Zdroj) vyberte umístění souboru .OVA.
3. V části **Name** (Název) a **Location** (Umístění) zadejte popisný název virtuálního počítače kolektoru a objekt inventáře, ve kterém bude daný virtuální počítač hostovaný.
5. V části **Host/Cluster** (Hostitel/cluster) zadejte hostitele nebo cluster, na kterém se bude virtuální počítač kolektoru spouštět.
7. V části Storage (Úložiště) zadejte cílové úložiště pro virtuální počítač kolektoru.
8. V části **Disk Format** (Formát disku) zadejte typ a velikost disku.
9. V části **Network Mapping** (Mapování sítě) zadejte síť, ke které se bude virtuální počítač kolektoru připojovat. Aby mohla síť odesílat metadata do Azure, potřebuje připojení k internetu. 
10. Zkontrolujte a ověřte všechna nastavení a pak klikněte na **Finish** (Dokončit).

## <a name="run-the-collector-to-discover-vms"></a>Spuštění kolektoru pro vyhledání virtuálních počítačů

1. V konzoli vSphere Client klikněte pravým tlačítkem na daný virtuální počítač a pak na **Open Console** (Otevřít konzolu).
2. Nastavte pro zařízení preferovaný jazyk, časové pásmo a heslo.
3. Na ploše klikněte na zástupce **Spustit kolektor**.
4. Ve službě Azure Migrate Collector otevřete nabídku **Nastavit požadavky**.
    - Přijměte licenční podmínky a přečtěte si informace třetích stran.
    - Kolektor zkontrolujte, jestli má virtuální počítač přístup k internetu.
    - Pokud má virtuální počítač přístup k internetu přes proxy server, klikněte na **Nastavení proxy**, zadejte adresu proxy serveru a nastavte naslouchající port. Pokud proxy server potřebuje přihlašovací údaje, zadejte je.

    > [!NOTE]
    > Adresa proxy musí být zadaná ve formátu http://IPAdresaProxyServeru nebo http://PlněKvalifikovanýNázevDoményProxyServeru. Podporuje se jen proxy protokolu HTTP.

    - Kolektor zkontroluje, jestli je spuštěná služba Azure Migrate Collector. Ta je ve výchozím nastavení nainstalovaná na virtuálním počítači kolektoru.
    - Stáhněte a nainstalujte řešení VMware PowerCLI.

5. V části **Zadejte podrobnosti vCenter Serveru** udělejte toto:
    - Zadejte název (plně kvalifikovaný název domény) nebo IP adresu vCenter Serveru.
    - V části **Uživatelské jméno** a **Heslo** zadejte přihlašovací údaje k účtu pouze pro čtení, který kolektor použije ke zjištění virtuálních počítačů na vCenter Serveru.
    - V části **Rozsah kolekce** vyberte kolekci pro zjišťování virtuálních počítačů. Kolektor může vyhledat jen virtuální počítače v rámci zadaného rozsahu. Jako rozsah můžete vybrat konkrétní složku, datové centrum nebo cluster. Neměl by obsahovat víc než 1000 virtuálních počítačů. 

6. V části **Zadejte projekt migrace** zadejte ID projektu služby Azure Migrate a klíč, který jste zkopírovali z portálu. Pokud jste ho nezkopírovali, na virtuálním počítači kolektoru otevřete Azure Portal. Na stránce **Přehled** projektu klikněte na **Zjistit počítače** a zkopírujte příslušné hodnoty.  
7. V části **Zobrazit průběh shromažďování** můžete sledovat zjišťování a kontrolovat, jestli metadata shromážděná z virtuálních počítačů patří do zadaného rozsahu. Kolektor vás informuje o tom, jak dlouho bude zjišťování přibližně trvat.

> [!NOTE]
> Kolektor podporuje jako jazyk operačního systému a jazyk rozhraní kolektoru jen angličtinu (Spojené státy). Brzy bude k dispozici podpora pro další jazyky.


### <a name="verify-vms-in-the-portal"></a>Kontrola virtuálních počítačů na portálu

Doba zjišťování závisí na tom, kolik virtuálních počítačů vyhledáváte. Od chvíle, kdy kolektor spustí zjišťování, obvykle trvá vyhledání stovky virtuálních počítačů přibližně hodinu. 

1. V projektu Azure Migrate klikněte na **Spravovat** > **Počítače**.
2. Zkontrolujte, jestli se virtuální počítače, které jste chtěli vyhledat, zobrazí na portálu.


## <a name="create-and-view-an-assessment"></a>Vytvoření a zobrazení posouzení

Jakmile vyhledáte požadované virtuální počítače, můžete je seskupit a posoudit, jestli jsou vhodné pro migraci. 

1. Na stránce **Přehled** projektu klikněte na **Vytvořit posouzení**.
2. Kliknutím na **Zobrazit vše** zobrazíte vlastnosti posouzení.
3. Vytvořte skupinu a pojmenujte ji.
4. Vyberte virtuální počítače, které do ní chcete přidat.
5. Kliknutím na tlačítko **Vytvořit posouzení** vytvoříte skupinu a posouzení.
6. Jakmile bude posouzení hotové, zobrazíte ho v části **Přehled** > **Řídicí panel**.
7. Klikněte na **Exportovat posouzení** a stáhněte ho jako excelový soubor.

### <a name="sample-assessment"></a>Ukázka posouzení

Tady je příklad sestavy posouzení. Obsahuje informace o tom, jestli jsou virtuální počítače kompatibilní s Azure, a odhad měsíčních nákladů. 

![Sestava posouzení](./media/tutorial-assessment-vmware/assessment-report.png)

#### <a name="azure-readiness"></a>Připravenost pro Azure

Toto zobrazení informuje o stavu připravenosti jednotlivých počítačů.

- U virtuálních počítačů, které jsou pro migraci vhodné, doporučí Azure Migrate velikost virtuálního počítače v Azure.
- U virtuálních počítačů, které nejsou pro migraci vhodné, vysvětlí Azure Migrate důvody a doporučí kroky pro odstranění problémů.
- Azure Migrate doporučí i nástroje, které můžete při migraci použít. Pokud je daný počítač vhodný pro migraci typu „lift and shift“, doporučuje se služba Azure Site Recovery. Pokud daný počítač hostuje databázi, doporučuje se služba Azure Database Migration Service.

  ![Posouzení připravenosti](./media/tutorial-assessment-vmware/assessment-suitability.png)  

#### <a name="monthly-cost-estimate"></a>Odhad měsíčních nákladů

Toto zobrazení informuje o celkových nákladech na výpočetní kapacitu a úložiště, které s sebou nese provoz virtuálních počítačů v Azure. Také nabízí podrobné údaje o jednotlivých počítačích. Odhadované náklady se počítají na základě doporučené výpočetní kapacity jednotlivých počítačů a jejich disků a také zohledňují vlastnosti posouzení. 

> [!NOTE]
> Služba Azure Migrate odhaduje náklady, které by s sebou přinesla migrace místních virtuálních počítačů na virtuální počítače zajišťované službou Azure v modelu IaaS (infrastruktura jako služba). Azure Migrate se nezabývá náklady modelů PaaS (platforma jako služba) ani SaaS (software jako služba). 

Odhadované měsíční náklady na výpočetní kapacitu a úložiště jsou agregované pro všechny virtuální počítače dané skupiny. 

![Odhadované náklady na virtuální počítače](./media/tutorial-assessment-vmware/assessment-vm-cost.png) 

Procházením hierarchie můžete zobrazit podrobnosti pro konkrétní počítač.

![Odhadované náklady na virtuální počítače](./media/tutorial-assessment-vmware/assessment-vm-drill.png) 

## <a name="next-steps"></a>Další kroky

- [Přečtěte si](how-to-scale-assessment.md), jak zjistit a posoudit virtuální počítače v rozsáhlém prostředí VMware.
- Zjistěte, jak vytvořit vysoce spolehlivé skupiny posouzení pomocí [mapování závislosti počítačů](how-to-create-group-machine-dependencies.md).
- [Přečtěte si další informace](concepts-assessment-calculation.md) o tom, jak se v rámci posouzení počítají náklady.
