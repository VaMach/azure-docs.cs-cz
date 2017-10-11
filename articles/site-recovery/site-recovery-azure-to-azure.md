---
title: "Replikovat virtuální počítače Azure mezi oblastmi Azure pro zotavení po havárii vyžaduje: Azure do Azure | Microsoft Docs"
description: "Shrnuje kroky je potřeba replikovat virtuální počítače Azure mezi oblastmi Azure (Azure Azure) se službou Azure Site Recovery pro potřebovat obnovení po havárii."
services: site-recovery
documentationcenter: 
author: rayne-wiselman
manager: carmon
editor: 
ms.assetid: dab98aa5-9c41-4475-b7dc-2e07ab1cfd18
ms.service: site-recovery
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/10/2017
ms.author: raynew
ms.openlocfilehash: 9ca33057f6030fdcc233f6053fdc392d62f8f9f4
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="replicate-azure-vms-between-regions-with-azure-site-recovery"></a>Replikace virtuálních počítačů Azure mezi oblastmi s Azure Site Recovery

>[!NOTE]
>
> Azure Site Recovery replikaci pro virtuální počítače Azure (VM) je aktuálně ve verzi preview.

Tento článek popisuje, jak replikovat virtuální počítače Azure mezi oblastmi Azure pomocí [Site Recovery](site-recovery-overview.md) službu na portálu Azure.

POST dotazy a v dolní části tohoto článku nebo na [fóru Azure Recovery Services](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).

## <a name="disaster-recovery-in-azure"></a>Zotavení po havárii v Azure

Předdefinované infrastrukturu Azure možností a funkcí přispívat k strategie dostupnosti robustní a odolné pro úlohy, které běží na virtuálních počítačích Azure. Existují však mnoha důvodů, proč potřebujete k vytvoření plánu zotavení po havárii mezi oblastmi Azure sami:

- Je třeba splňovat pokyny dodržování předpisů pro konkrétní aplikace a úlohy, které vyžadují kontinuity podnikových procesů a strategie po havárii (BCDR).
- Chcete mít možnost k ochraně a obnovování virtuálních počítačích Azure, podle obchodních rozhodnutí a to není pouze podle integrované funkce Azure.
- Je potřeba provést testování převzetí služeb při selhání a obnovení v souladu s potřebám vaší firmy a dodržování předpisů bez dopadu na produkční.
- Budete muset převzetí služeb při selhání oblasti obnovení v případě havárie a navrácení služeb po obnovení původního zdrojového oblast bezproblémově.

Pomocí Site Recovery pro replikaci virtuálního počítače Azure do Azure můžete provést tyto úlohy.


## <a name="why-use-site-recovery"></a>Proč používat Site Recovery?      

Site Recovery poskytuje jednoduchý způsob, jak replikovat virtuální počítače Azure mezi oblastmi:

- **Automatické nasazení**. Na rozdíl od model replikace aktivní aktivní není nutné pro složité a nákladné infrastruktury v sekundární oblasti. Když povolíte replikaci, Site Recovery ve automaticky vytvoří požadované prostředky cílová oblast na základě nastavení oblasti zdroje.
- **Řízení oblasti**. Pomocí Site Recovery můžete pro všechny oblasti v rámci kontinentě replikaci z libovolné oblasti. Toto porovnání s přístupem pro čtení geograficky redundantní úložiště, která asynchronně replikuje mezi standard [spárovat oblasti](https://docs.microsoft.com/azure/best-practices-availability-paired-regions) pouze. Geograficky redundantní úložiště s přístupem pro čtení poskytuje přístup jen pro čtení k datům v cílové oblasti.
- **Automatizované replikace**. Site Recovery poskytuje automatizované průběžnou replikaci. Převzetí služeb při selhání a navrácení služeb po obnovení můžete spustit s jedním kliknutím.
- **RTO a plánovaný bod obnovení**. Site Recovery se využívá Azure síťové infrastruktury, která se připojuje oblasti zachovat velmi nízkou RTO a plánovaný bod obnovení.
- **Testování**. Cvičení zotavení po havárii můžete spustit s na vyžádání testovací převzetí služeb při selhání podle potřeby, aniž by to ovlivňovalo produkční zatížení nebo probíhající replikace.
- **Plány obnovení**. Plány obnovení můžete použít k orchestraci převzetí služeb při selhání a navrácení služeb po obnovení celé aplikace běžící na několika virtuálních počítačích. Funkce plán obnovení má bohaté prvotřídní integrace s runbooky služby Azure automation.


## <a name="deployment-summary"></a>Souhrn nasazení

Zde je uveden seznam, co musíte udělat pro nastavení replikace virtuálních počítačů mezi oblastmi Azure:

1. Vytvořte trezor služby Recovery Services. Trezor obsahuje nastavení konfigurace a orchestruje replikaci.
2. Povolení replikace pro virtuální počítače Azure.
3. Spusťte převzetí služeb při selhání a ujistěte se, zda vše funguje podle očekávání.

>[!IMPORTANT]
>
> Můžete zkontrolovat [matici podpory pro replikaci virtuálního počítače Azure](./site-recovery-support-matrix-azure-to-azure.md).

>[!IMPORTANT]
>
> Informace o tom, jak nakonfigurovat požadované síťové odchozí připojení pro virtuální počítače Azure Site Recovery replikace najdete v tématu [sítě pokyny dokumentu](./site-recovery-azure-to-azure-networking-guidance.md).

### <a name="before-you-start"></a>Než začnete

* Azure uživatelský účet musí mít určité [oprávnění](site-recovery-role-based-linked-access-control.md#permissions-required-to-enable-replication-for-new-virtual-machines) k povolení replikace virtuálního počítače Azure.
* Vašeho předplatného Azure by měly být povoleny k vytvoření virtuálních počítačů v cílovém umístění, které chcete použít jako oblasti obnovení po havárii. Obraťte se na podporu, aby umožnil požadované kvótu.

## <a name="create-a-recovery-services-vault"></a>Vytvoření trezoru Služeb zotavení

[!INCLUDE [site-recovery-create-vault](../../includes/site-recovery-create-vault.md)]

>[!NOTE]
>
> Doporučujeme vytvořit trezor služeb zotavení v umístění, kde chcete replikovat virtuální počítače. Například, pokud cílové umístění je střed USA, vytvořte trezor v **střed USA**.

## <a name="enable-replication"></a>Povolení replikace

V **trezory služeb zotavení**, klikněte na název trezoru. V trezoru, klikněte **+ replikovat** tlačítko.

### <a name="step-1-configure-the-source"></a>Krok 1. Nastavit zdroj
1. V **zdroj**, vyberte **Azure - PREVIEW**.
2. V **umístění zdroje**, vyberte zdroj oblast Azure, kde vaše virtuální počítače jsou aktuálně spuštěné.
3. Vyberte model nasazení virtuálních počítačů: **Resource Manager** nebo **Classic**.
4. Vyberte **zdrojové skupiny prostředků** pro virtuální počítače správce prostředků nebo **Cloudová služba** pro klasické virtuální počítače.

    ![Nastavit zdroj](./media/site-recovery-azure-to-azure/source.png)

### <a name="step-2-select-virtual-machines"></a>Krok 2. Vyberte virtuální počítače

* Vyberte virtuální počítače, které chcete replikovat a pak klikněte na tlačítko **OK**.

    ![Vyberte virtuální počítače](./media/site-recovery-azure-to-azure/vms.png)

### <a name="step-3-configure-settings"></a>Krok 3. Konfigurace nastavení

1. Pokud chcete přepsat výchozí nastavení cílové a zadejte nastavení podle vaší volby, klikněte na tlačítko **přizpůsobit**. Další informace najdete v tématu [přizpůsobit cílové prostředky](site-recovery-replicate-azure-to-azure.md##customize-target-resources).

    ![Konfigurace nastavení](./media/site-recovery-azure-to-azure/settings.png)


2. Site Recovery ve výchozím nastavení vytvoří zásadu replikace, která přebírá snímky konzistentní aplikace každé 4 hodiny a uchovává body obnovení po dobu 24 hodin. Chcete-li vytvořit zásadu s různá nastavení, klikněte na tlačítko **přizpůsobit** vedle **zásady replikace**.

    ![Úpravy zásad](./media/site-recovery-azure-to-azure/customize-policy.png)

3. Chcete-li spustit zřizování cílové prostředky, klikněte na tlačítko **vytvoření cílové prostředky**. Zřizování trvá minutu. Nemáte zavřete okno při zřizování, nebo je nutné začít znovu.

4. Chcete-li aktivovat replikaci vybraného virtuálního počítače, klikněte na tlačítko **povolit replikaci**.

5. Můžete sledovat průběh **povolení ochrany** úlohy v **nastavení** > **úlohy** > **úlohy Site Recovery**.

6. V **nastavení** > **replikované položky**, můžete zobrazit stav virtuálních počítačů a probíhá počáteční replikace. Klikněte na virtuální počítač můžete rozbalit jeho nastavení.

## <a name="run-a-test-failover"></a>Spuštění testovacího převzetí služeb při selhání

Po nastavení vše, spusťte převzetí služeb při selhání a ujistěte se, zda že vše funguje podle očekávání:

1. K převzetí služeb při selhání jednom počítači, v **nastavení** > **replikované položky**, klikněte na virtuální počítač **+ testovací převzetí služeb při selhání** ikonu.

2. K převzetí služeb při selhání plánu obnovení v **nastavení** > **plány obnovení**, klikněte pravým tlačítkem na plán **testovací převzetí služeb při selhání**. Pokud chcete vytvořit plán obnovení, [postupujte podle těchto pokynů](site-recovery-create-recovery-plans.md). 

3. V **testovací převzetí služeb při selhání**, vyberte cíl virtuální síť Azure ke které virtuální počítače Azure připojeni potom, co dojde převzetí služeb při selhání.

4. Chcete-li spustit převzetí služeb při selhání, klikněte na tlačítko **OK**. Chcete-li sledovat průběh, klikněte na virtuálním počítači otevřete jeho vlastnosti. Nebo můžete kliknout **testovací převzetí služeb při selhání** úlohy v název trezoru > **nastavení** > **úlohy** > **úlohy Site Recovery**.

5. Po dokončení převzetí repliky počítač Azure se zobrazí na portálu Azure > **virtuální počítače**. Ujistěte se, že virtuální počítač odpovídající velikost, byl připojený k příslušné síti, a jestli je spuštěná.

6. Chcete-li odstranit virtuální počítače, které byly vytvořeny během testu převzetí služeb, klikněte na tlačítko **vyčistit testovací převzetí služeb při selhání** na replikované položky nebo plán obnovení. V **poznámky**, zaznamenejte a uložte jakékoli připomínky související s testovací převzetí služeb. 

[Další informace](site-recovery-test-failover-to-azure.md) o testovací převzetí služeb při selhání.


## <a name="next-steps"></a>Další kroky

Po testovací nasazení:

- Přečtěte si [další informace](site-recovery-failover.md) o různých typech převzetí služeb při selhání a o tom, jak je spustit.
- Další informace o [pomocí plánů obnovení](site-recovery-create-recovery-plans.md) ke snížení RTO.
