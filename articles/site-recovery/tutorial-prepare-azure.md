---
title: "Tvorba zdrojů pro použití s Azure Site Recovery | Microsoft Docs"
description: "Zjistěte, jak připravit Azure pro replikaci lokálních počítačů pomocí služby Azure Site Recovery."
services: site-recovery
author: rayne-wiselman
ms.service: site-recovery
ms.topic: tutorial
ms.date: 01/16/2018
ms.author: raynew
ms.custom: MVC
ms.openlocfilehash: 63290127b298efced14ad34e9223840f3229f046
ms.sourcegitcommit: 7edfa9fbed0f9e274209cec6456bf4a689a4c1a6
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/17/2018
---
# <a name="prepare-azure-resources-for-replication-of-on-premises-machines"></a>Příprava prostředků Azure pro replikaci počítačů na místě

[Azure Site Recovery](site-recovery-overview.md) služby přispívá ke strategii obchodní kontinuitu a po havárii (BCDR) obnovení ponechat obchodních aplikací stále a spuštěním během plánovaných a neplánovaných výpadků. Site Recovery spravuje a orchestruje zotavení po havárii místní počítače a virtuální počítače Azure (VM), včetně replikace, převzetí služeb při selhání a obnovení.

V tomto kurzu se dozvíte, jak připravit Azure součásti, pokud chcete replikovat místní virtuální počítače (Hyper-V nebo VMware) nebo fyzických serverů Windows nebo Linuxem do Azure. V tomto kurzu se naučíte:

> [!div class="checklist"]
> * Ověřte, že váš účet nemá oprávnění k replikaci
> * Vytvoření účtu úložiště Azure
> * Nastavte síť Azure. Virtuální počítače Azure vytvořené po převzetí služeb při selhání, budou se připojené k této síti Azure.

Pokud ještě nemáte předplatné Azure, vytvořte si [bezplatný účet](https://azure.microsoft.com/pricing/free-trial/) před tím, než začnete.

## <a name="log-in-to-azure"></a>Přihlášení k Azure

Přihlaste se k webu Azure Portal na adrese http://portal.azure.com.

## <a name="verify-account-permissions"></a>Ověřte oprávnění pro uživatelský účet

Pokud jste právě vytvořili vaší bezplatný účet Azure, jsou správce předplatného. Pokud nejste správci předplatného, spolupracovat se správcem přiřadit oprávnění, která potřebujete. Pokud chcete povolit replikaci pro nový virtuální počítač, musíte mít:

- Oprávnění k vytvoření virtuálního počítače ve vybrané skupině prostředků
- Oprávnění k vytvoření virtuálního počítače ve vybrané virtuální síti
- Oprávnění k zápisu do vybraného účtu úložiště

Předdefinovaná role, Přispěvatel virtuálních počítačů, má tato oprávnění. Budete také potřebovat oprávnění ke správě operací Azure Site Recovery. Role, Přispěvatel obnovení lokality, má všechna oprávnění vyžadovaná ke správě operací obnovení lokality v trezoru služeb zotavení.

## <a name="create-a-storage-account"></a>vytvořit účet úložiště

Bitové kopie replikovaných počítačů jsou uložené v úložišti Azure. Virtuální počítače Azure se vytvářejí z úložiště při selhání z místního do Azure.

1. V [portál Azure](https://portal.azure.com) nabídky, klikněte na tlačítko **nový** -> **úložiště** -> **účet úložiště**.
2. V **vytvořit účet úložiště**, zadejte název pro účet. Tyto kurzy použijeme název **contosovmsacct1910171607**. Název musí být jedinečné v rámci Azure a musí být v rozmezí 3 až 24 znaků s jenom číslice a malá písmena.
3. Použití **Resource Manager** modelu nasazení.
4. Vyberte **obecné účely** > **standardní**. Nevybírejte úložiště objektů blob.
5. Vyberte výchozí **RA-GRS** redundance úložiště.
6. Vyberte předplatné, ve kterém chcete vytvořit nový účet úložiště.
7. Zadejte novou skupinu prostředků. Skupina prostředků Azure je logický kontejner, ve kterém se nasazují a spravují prostředky Azure. Tyto kurzy používáme název **ContosoRG**.
8. Vyberte zeměpisné umístění účtu úložiště. Účet úložiště musí být ve stejné oblasti jako trezor služeb zotavení. Tyto kurzy používáme **západní Evropa** oblast.

   ![Vytvoření storageacct](media/tutorial-prepare-azure/create-storageacct.png)

9. Vytvořte účet úložiště kliknutím na **Vytvořit**.

## <a name="create-a-vault"></a>Vytvoření trezoru

1. V nabídce portálu Azure, klikněte na tlačítko **nový** > **pro monitorování a správu** >
    **zálohování a obnovení lokality**.
2. Do pole **Název** zadejte popisný název pro identifikaci trezoru. V tomto kurzu používáme **ContosoVMVault**.
3. Vyberte existující skupinu prostředků s názvem **contosoRG**.
4. Zadejte oblast Azure **západní Evropa**, který používáme v této sadě kurzy.
5. Chcete-li rychle získat přístup k úložišti z řídicího panelu, klikněte na tlačítko **připnout na řídicí panel** > **vytvořit**.

   ![Nový trezor](./media/tutorial-prepare-azure/new-vault-settings.png)

   Nový trezor se zobrazí na **řídicí panel** > **všechny prostředky**a v hlavním **trezory služeb zotavení** stránky.

## <a name="set-up-an-azure-network"></a>Nastavení sítě Azure

Virtuální počítače Azure vytvořené z úložiště po převzetí služeb při selhání, budou se připojené k této síti.

1. V [portál Azure](https://portal.azure.com) nabídky, klikněte na tlačítko **nový** > **sítě** >
    **virtuální síť**
2. Nechte **Resource Manager** vybraný jako model nasazení. Správce prostředků je model upřednostňované nasazení.
   - Zadejte název sítě. Název musí být jedinečný v rámci skupiny prostředků Azure. Budeme používat název **ContosoASRnet**
   - Použít existující skupinu prostředků **contosoRG**.
   - Zadejte rozsah adres 10.0.0.0/24 sítě.
   - V tomto kurzu jsme nepotřebují podsíť.
   - Vyberte odběr, do kterého chcete vytvořit síť.
   - Vyberte umístění **západní Evropa**. Síť musí být ve stejné oblasti jako trezor Služeb zotavení.
3. Klikněte na možnost **Vytvořit**.

   ![vytvořit síť](media/tutorial-prepare-azure/create-network.png)

   Virtuální síť trvá několik sekund k vytvoření. Po vytvoření zobrazení v řídicím panelu portálu Azure.

## <a name="next-steps"></a>Další postup

> [!div class="nextstepaction"]
> [Příprava na místní infrastrukturu VMware pro zotavení po havárii do Azure](tutorial-prepare-on-premises-vmware.md)
