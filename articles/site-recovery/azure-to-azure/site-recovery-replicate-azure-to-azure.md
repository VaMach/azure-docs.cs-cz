---
title: "Replikace virtuálních počítačů Azure v sekundární oblasti s Azure Site Recovery | Microsoft Docs"
description: "Tento článek popisuje, jak k replikaci adresáře Azure spuštěné virtuální počítače v jedné oblasti Azure do jiné oblasti, pomocí služby Azure Site Recovery."
services: site-recovery
documentationcenter: 
author: asgang
manager: rochakm
editor: 
ms.assetid: 
ms.service: site-recovery
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
ms.date: 11/29/2017
ms.author: asgang
ms.openlocfilehash: 114390712f5e5bf62ec515db62469e09361b8f85
ms.sourcegitcommit: cfd1ea99922329b3d5fab26b71ca2882df33f6c2
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/30/2017
---
# <a name="replicate-azure-virtual-machines-to-another-azure-region"></a>Jiné oblasti Azure replikovat virtuální počítače Azure


Tento článek popisuje, jak replikovat virtuální počítače Azure (VM) v jedné oblasti Azure, sekundární oblasti Azure, pomocí služby Azure Site Recovery.

>[!NOTE]
>
> Azure replikace virtuálního počítače pomocí Site Recovery je aktuálně ve verzi preview.

## <a name="prerequisites"></a>Požadavky

* Měli byste mít trezoru služeb zotavení na místě. Doporučujeme vám, vytvořte trezor nejprve v oblasti cíl, u kterého chcete replikovat virtuální počítače.
* Pokud používáte k řízení přístupu k odchozí připojení k Internetu na virtuálních počítačích Azure pravidel skupiny zabezpečení sítě (NSG) nebo proxy server brány firewall, ujistěte se, povolíte požadované adresy URL nebo IP adresy. [Další informace](./site-recovery-azure-to-azure-networking-guidance.md).
* Pokud máte ExpressRoute nebo VPN připojení mezi místními a umístění zdroje v Azure, [zjistěte, jak je nastavit](site-recovery-azure-to-azure-networking-guidance.md#guidelines-for-existing-azure-to-on-premises-expressroutevpn-configuration).
* Azure uživatelský účet potřebuje [konkrétní oprávnění](../site-recovery-role-based-linked-access-control.md#permissions-required-to-enable-replication-for-new-virtual-machines)k povolení replikace virtuálního počítače Azure.
Vašeho předplatného Azure by měly být povoleny k vytvoření virtuálních počítačů v cílovém umístění, které chcete použít jako oblast pro obnovení po havárii. Obraťte se na podporu, aby umožnil požadované kvótu.

## <a name="enable-replication"></a>Povolení replikace

V tomto postupu východní Asie slouží jako umístění zdroje a jihovýchodní Asie jako cíl.

1. Klikněte na tlačítko **+ replikovat** v trezoru povolíte replikaci pro virtuální počítače.
2. Ověřte, že **zdroj:** je nastaven na **Azure**.
3. Nastavit **umístění zdroje** k východní Asie.
4. V **modelu nasazení**, vyberte **classic** nebo **Resource Manager**.
5. V **skupiny prostředků**, vyberte skupinu, do které patří vaší zdrojové virtuální počítače. Jsou uvedeny všechny virtuální počítače v části s vybranou skupinou prostředků.

    ![Povolení replikace](./media/site-recovery-replicate-azure-to-azure/enabledrwizard1.png)

6. V **virtuální počítače > vyberte virtuální počítače**, klikněte na tlačítko a vyberte každý virtuální počítač, který chcete replikovat. Můžete vybrat pouze počítače, pro které je možné povolit replikaci. Pak klikněte na **OK**.

    ![Povolení replikace](./media/site-recovery-replicate-azure-to-azure/virtualmachine_selection.png)
    
7. V části **nastavení** > **cílové umístění**, určete, kde se replikuje zdroj dat virtuálního počítače. Site Recovery poskytuje seznam vhodnou cílovou oblastí, v závislosti na oblasti vybrané virtuální počítače.
8. Site Recovery nastaví výchozí nastavení cíle. To se dá změnit podle potřeby.

    - **Cílová skupina prostředků**. Ve výchozím nastavení Site Recovery vytvoří novou skupinu prostředků v cílové oblasti s příponou "Automatické obnovení systému". Pokud skupina vytvořený prostředek již existuje, je znovu použít.
    - **Cílová virtuální síť**. Ve výchozím nastavení Site Recovery vytvoří novou virtuální síť v oblasti cíl s příponou "Automatické obnovení systému". Tato síť je namapována na zdrojovou síť. [Další informace](site-recovery-network-mapping-azure-to-azure.md) o mapování sítě.
    - **Cílové úložiště účtů**. Ve výchozím nastavení Site Recovery vytvoří nový účet úložiště cíl, který odpovídá konfiguraci úložiště zdrojového virtuálního počítače. Pokud vytvořený účet již existuje, je znovu použít.
    - **Účty úložiště mezipaměti**. Azure Site Recovery vytvoří účet úložiště navíc mezipaměti oblasti zdroje. Všechny změny na zdrojové virtuální počítače jsou sledovány a odeslat do účtu úložiště mezipaměti před replikace do cílového umístění.
    - **Skupina dostupnosti**. Ve výchozím nastavení vytvoří Site Recovery novou skupinou dostupnosti ve cílová oblast s příponou "Automatické obnovení systému". Pokud sadu vytvořený již existuje, je znovu použít.
    - **Zásady replikace**. Site Recovery definuje nastavení historie uchování bodu obnovení a četnost snímky konzistentní s aplikací. Ve výchozím nastavení Site Recovery vytvoří novou zásadu replikace s výchozím nastavením 24 hodin pro uchování bodu obnovení a 60 minut, než frekvence snímků konzistentní vzhledem k aplikaci.

    ![Povolení replikace](./media/site-recovery-replicate-azure-to-azure/enabledrwizard3.PNG)
9. Klikněte na tlačítko **povolit replikaci** a spustit tak ochranu virtuálních počítačů.

## <a name="customize-target-resources"></a>Přizpůsobení cílové prostředky

1. Tato výchozí nastavení cílové upravte:

    - **Cílová skupina prostředků**. Vyberte libovolnou skupinu prostředků ze seznamu všech skupin prostředků v cílovém umístění, v rámci předplatného.
    - **Cílová virtuální síť**. Vyberte ze seznamu všechny virtuální sítě v cílovém umístění.
    - **Skupina dostupnosti** nastavení sad dostupnosti můžete přidat pouze pro virtuální počítače umístěné v sadě v oblasti zdroje.
    - **Cíl účty úložiště**: Přidejte libovolný účet, který je k dispozici.

        ![Povolení replikace](./media/site-recovery-replicate-azure-to-azure/customize.PNG)

2. Klikněte na **vytvořit cílový prostředek** > **povolit replikaci**. Během úvodní replikace stav virtuálního počítače může trvat nějakou dobu aktualizace. Klikněte na tlačítko **aktualizovat** a získat nejnovější stav.

    ![Povolení replikace](./media/site-recovery-replicate-azure-to-azure/replicateditems.PNG)
    
3. Poté, co jsou chráněné virtuální počítače, zkontrolujte stav virtuálního počítače **replikované položky**.



## <a name="next-steps"></a>Další kroky
[Další informace](../azure-to-azure-tutorial-dr-drill.md) jak spustit testovací převzetí služeb.

