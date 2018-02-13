---
title: "Migrace virtuálních počítačů Azure mezi oblastmi Azure pomocí Azure Site Recovery | Microsoft Docs"
description: "Pomocí Azure Site Recovery k migraci virtuálních počítačů Azure IaaS z jedné oblasti Azure do druhého."
services: site-recovery
author: rayne-wiselman
ms.service: site-recovery
ms.topic: tutorial
ms.date: 01/07/2018
ms.author: raynew
ms.openlocfilehash: e7b925d2daed11ee4e070cda6bcbd4a3511d9c17
ms.sourcegitcommit: 6fb44d6fbce161b26328f863479ef09c5303090f
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/10/2018
---
# <a name="migrate-azure-vms-to-another-region"></a>Migrace virtuálních počítačů Azure do jiné oblasti

Kromě použití [Azure Site Recovery](site-recovery-overview.md) služby ke správě a orchestraci zotavení po havárii místní počítače a virtuální počítače Azure pro účely provozní kontinuitu a zotavení po havárii (BCDR), můžete použít také lokality Spravujte migraci virtuálních počítačů Azure v sekundární oblasti obnovení. Pokud chcete migrovat virtuální počítače Azure, povolení replikace pro ně a převzetí služeb při selhání je z primární oblasti sekundární oblast podle svého výběru.

V tomto kurzu se dozvíte, jak migrovat virtuální počítače Azure do jiné oblasti. V tomto kurzu se naučíte:

> [!div class="checklist"]
> * Vytvoření trezoru služeb zotavení
> * Povolení replikace pro virtuální počítač
> * Spustit převzetí služeb při selhání k migraci virtuálního počítače

Tento kurz předpokládá, že již máte předplatné Azure. Pokud ne, vytvořit [bezplatný účet](https://azure.microsoft.com/pricing/free-trial/) před zahájením.

>[!NOTE]
>
> Replikace obnovení lokality pro virtuální počítače Azure je aktuálně ve verzi preview.



## <a name="prerequisites"></a>Požadavky

K dokončení tohoto kurzu, musíte virtuální počítače Azure v oblasti Azure, ze kterého chcete migrovat. Kromě toho existuje několik nastavení, které byste měli ověřit před zahájením.


### <a name="verify-target-resources"></a>Zkontrolujte cílové prostředky

1. Ověřte, že vaše předplatné Azure umožňuje vytvářet virtuální počítače v cílová oblast pro obnovení po havárii. Obraťte se na podporu, aby umožnil požadované kvótu.

2. Zajistěte, aby že vaše předplatné nemá dostatek prostředků pro podporu virtuálních počítačů s velikostí, které odpovídají zdrojové virtuální počítače. Site Recovery vybere stejnou velikost nebo nejbližší možné velikost pro cílový počítač.


### <a name="verify-account-permissions"></a>Ověřte oprávnění pro uživatelský účet

Pokud jste právě vytvořili účet Azure zdarma jste správcem předplatného. Pokud nejste správci předplatného, spolupracovat se správcem přiřadit oprávnění, která potřebujete. Pokud chcete povolit replikaci pro nový virtuální počítač, musíte mít:

1. Oprávnění k vytvoření virtuálního počítače v prostředků Azure. Předdefinovaná role, Přispěvatel virtuálních počítačů, má tato oprávnění, mezi které patří:
    - Oprávnění k vytvoření virtuálního počítače ve vybrané skupině prostředků
    - Oprávnění k vytvoření virtuálního počítače ve vybrané virtuální síti
    - Oprávnění k zápisu do vybraného účtu úložiště

2. Budete také potřebovat oprávnění ke správě operací Azure Site Recovery. Role, Přispěvatel obnovení lokality, má všechna oprávnění vyžadovaná ke správě operací obnovení lokality v trezoru služeb zotavení.


### <a name="verify-vm-outbound-access"></a>Ověřte odchozí přístup virtuálních počítačů

1. Ujistěte se, že nepoužíváte proxy služby ověřování k řízení připojení k síti pro virtuální počítače, které chcete migrovat. 
2. Pro účely tohoto kurzu předpokládáme, že virtuální počítače chcete migrovat přístup k Internetu a nepoužívají proxy server brány firewall řídit odchozí přístup. Pokud jste, zkontrolujte požadavky na [zde](azure-to-azure-tutorial-enable-replication.md#configure-outbound-network-connectivity).

### <a name="verify-vm-certificates"></a>Ověření certifikátů virtuálního počítače

Zkontrolujte, jestli jsou k dispozici na virtuálních počítačích Azure, kterou chcete migrovat všechny nejnovější kořenové certifikáty. Pokud nejsou nejnovější kořenové certifikáty, virtuální počítač nelze zaregistrovat k obnovení lokality z důvodu omezení zabezpečení.

- Pro virtuální počítače Windows nainstalujte všechny nejnovější aktualizace systému Windows na virtuální počítač tak, aby byly všechny důvěryhodných kořenových certifikátů na počítači. V odpojeném prostředí postupujte podle standardní Windows Update a procesy aktualizace certifikátů pro vaši organizaci.
- Pro virtuální počítače s Linuxem využijte tohoto vodítka poskytované vaší Linux distributora, chcete-li získat nejnovější důvěryhodných kořenových certifikátů a seznam odvolaných certifikátů ve virtuálním počítači.



## <a name="create-a-vault"></a>Vytvoření trezoru

Vytvořte trezor nejprve v libovolné oblasti, s výjimkou oblasti zdroje.

1. Přihlaste se k webu [Azure Portal](https://portal.azure.com) > **Recovery Services**.
2. Klikněte na **Nový** > **Monitorování a správa** > **Backup a Site Recovery**.
3. V **název**, zadejte popisný název **ContosoVMVault**. Pokud máte více než jedno předplatné, vyberte příslušný.
4. Vytvořte skupinu prostředků **ContosoRG**.
5. Zadejte oblast Azure. Informace o tom, které oblasti jsou podporované, najdete v části s geografickou dostupností v tématu s [podrobnostmi o cenách Azure Site Recovery](https://azure.microsoft.com/pricing/details/site-recovery/).
6. Chcete-li rychle získat přístup k úložišti z řídicího panelu, klikněte na tlačítko **připnout na řídicí panel** a pak klikněte na **vytvořit**.

   ![Nový trezor](./media/tutorial-migrate-azure-to-azure/azure-to-azure-vault.png)

Nový trezor se přidá do **řídicí panel** pod **všechny prostředky**a v hlavním **trezory služeb zotavení** stránky.






## <a name="select-the-source"></a>Vyberte zdroj

1. V trezory služeb zotavení, klikněte na **ConsotoVMVault** > **+ replikovat**.
2. V **zdroj**, vyberte **Azure - PREVIEW**.
3. V **umístění zdroje**, vyberte zdroj oblast Azure, kde vaše virtuální počítače jsou aktuálně spuštěné.
4. Vyberte model nasazení Resource Manager. Vyberte **zdrojové skupiny prostředků**.
5. Kliknutím na **OK** uložte nastavení.


## <a name="enable-replication-for-azure-vms"></a>Povolení replikace pro virtuální počítače Azure

Site Recovery načte seznam virtuálních počítačů spojené s předplatné a skupina prostředků.


1. Na portálu Azure klikněte na tlačítko **virtuální počítače**.
2. Vyberte virtuální počítač, který chcete migrovat. Pak klikněte na **OK**.
3. V **nastavení**, klikněte na tlačítko **zotavení po havárii (preview)**.
4. V **konfigurace zotavení po havárii** > **cílová oblast** vyberte oblast cíl, do kterého budete replikovat.
5. V tomto kurzu přijměte výchozí nastavení.
6. Klikněte na tlačítko **povolit replikaci**. Spustí se úloha k povolení replikace pro virtuální počítač.

    ![Povolení replikace](media/tutorial-migrate-azure-to-azure/settings.png)

>[!NOTE]
  >
  > Replikace virtuálních počítačů Azure s spravované disky v současné době se nepodporuje. 

## <a name="run-a-failover"></a>Spuštění převzetí služeb při selhání

1. V **nastavení** > **replikované položky**, klikněte na počítač a pak klikněte na tlačítko **převzetí služeb při selhání**.
2. V **převzetí služeb při selhání**, vyberte **nejnovější**. Nastavení klíče šifrování není relevantní pro tento scénář.
3. Vyberte **vypnout počítač před zahájením převzetí služeb při selhání**. Site Recovery se pokusí vypnout zdrojový virtuální počítač před spuštěním převzetí služeb při selhání. Převzetí služeb při selhání pokračovat i v případě, že vypnutí selže. Můžete sledovat průběh převzetí služeb při selhání **úlohy** stránky.
4. Zkontrolujte, že virtuální počítač Azure se zobrazí v Azure podle očekávání.
5. V **replikované položky**, klikněte pravým tlačítkem na virtuální počítač > **dokončení migrace**. Dokončení procesu migrace a zastavení replikace pro virtuální počítač.



## <a name="next-steps"></a>Další postup

V tomto kurzu jste migrovali virtuální počítač Azure v jiné oblasti Azure. Teď můžete nakonfigurovat zotavení po havárii pro migrovaný virtuální počítač.

> [!div class="nextstepaction"]
> [Nastavit zotavení po havárii po migraci](azure-to-azure-quickstart.md)

