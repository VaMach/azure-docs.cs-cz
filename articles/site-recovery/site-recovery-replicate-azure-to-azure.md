---
title: Replikovat aplikace (Azure do Azure) | Microsoft Docs
description: "Tento článek popisuje, jak nastavit replikaci virtuálních počítačů spuštěných v jedné oblasti Azure do jiné oblasti v Azure."
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
ms.date: 5/22/2017
ms.author: asgang
ms.openlocfilehash: f9f97cf840b722c8cfee169dd1640e0682f287ff
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="replicate-azure-virtual-machines-to-another-azure-region"></a>Jiné oblasti Azure replikovat virtuální počítače Azure



>[!NOTE]
>
> Replikace obnovení lokality pro virtuální počítače Azure je aktuálně ve verzi preview.

Tento článek popisuje, jak nastavit replikaci virtuálních počítačů spuštěných v jedné oblasti Azure jiné oblasti Azure.

## <a name="prerequisites"></a>Požadavky

* Článek předpokládá, že již víte o Site Recovery a trezoru služeb zotavení. Je potřeba mít po předem trezoru služeb zotavení vytvořili.

    >[!NOTE]
    >
    > Doporučuje se vytvořit trezor služeb zotavení v umístění, kde chcete replikovat virtuální počítače. Například pokud cílové umístění, střed USA", vytvořte trezor v, střed USA".

* Pokud používáte k řízení přístupu k odchozí připojení k Internetu na virtuálních počítačích Azure, pravidla skupiny zabezpečení sítě (NSG) nebo proxy server brány firewall, ujistěte se, že je seznam povolených adres požadované adresy URL nebo IP adresy. Odkazovat na [sítě pokyny dokumentu](./site-recovery-azure-to-azure-networking-guidance.md) další podrobnosti.

* Pokud máte ExpressRoute nebo VPN připojení mezi místními a umístění zdroje v Azure, postupujte podle [obnovení lokality požadavky pro Azure ExpressRoute místní nebo konfiguraci sítě VPN](site-recovery-azure-to-azure-networking-guidance.md#guidelines-for-existing-azure-to-on-premises-expressroutevpn-configuration) dokumentu.

* Azure uživatelský účet musí mít určité [oprávnění](site-recovery-role-based-linked-access-control.md#permissions-required-to-enable-replication-for-new-virtual-machines) k povolení replikace virtuálního počítače Azure.

* Lze povolit vašeho předplatného Azure k vytvoření virtuálních počítačů v cílovém umístění, které chcete použít jako oblast zotavení po Havárii. Obraťte se na podporu, aby umožnil požadované kvótu.

## <a name="enable-replication-from-azure-site-recovery-vault"></a>Povolit replikaci z trezoru Azure Site Recovery
Pro tento obrázek jsme bude replikovat virtuální počítače spuštěné ve východní Asie Azure umístění, do umístění, jihovýchodní Asie '. Kroky jsou následující:

 Klikněte na tlačítko **+ replikovat** v trezoru povolíte replikaci pro virtuální počítače.

1. **Zdroj:** odkazuje na bod původu počítačů, které v tomto případě je **Azure**.

2. **Umístění zdroje:** je oblast Azure, ze které chcete chránit virtuální počítače. Pro tento obrázek bude umístění zdroje, východní Asie.

3. **Model nasazení:** odkazuje na modelu nasazení Azure zdrojový počítač. Můžete vybrat buď classic nebo resource manager a počítače patřící do určité modelu budou uvedené pro ochranu v dalším kroku.

      >[!NOTE]
      >
      > Můžete replikovat klasické virtuální počítač a obnovit jako virtuální počítač s classic. Nelze obnovit jako virtuální počítač Resource Manager.

4. **Skupina prostředků:** je skupina prostředků, do které patří zdroj virtuálních počítačů. Objeví se všechny virtuální počítače v části s vybranou skupinou prostředků pro ochranu v dalším kroku.

    ![Povolení replikace](./media/site-recovery-replicate-azure-to-azure/enabledrwizard1.png)

V **virtuální počítače > vyberte virtuální počítače**, klikněte na tlačítko a vyberte každý počítač, který chcete replikovat. Můžete vybrat pouze počítače, pro které je možné povolit replikaci. Pak klikněte na OK.
    ![Povolení replikace](./media/site-recovery-replicate-azure-to-azure/virtualmachine_selection.png)


V části nastavení můžete nakonfigurovat vlastnosti cílové lokality

1. **Cílové umístění:** Toto je umístění, kde bude replikován zdrojová data virtuálního počítače. V závislosti na vaší umístění vybrané počítače se Site Recovery poskytuje seznam vhodný cílové oblasti.

    > [!TIP]
    > Doporučuje se zachovat cílové umístění stejné od vaší obnovení služeb trezoru.

2. **Cílová skupina prostředků:** je skupina prostředků, na kterém jsou všechny bude patřit replikovaných virtuálních počítačů. Ve výchozím nastavení automatické obnovení systému vytvoří novou skupinu prostředků v cílová oblast s názvem, který má příponu "Automatické obnovení systému". V případě, že skupiny prostředků vytvořené automatické obnovení systému již existuje, bude znovu. Můžete také přizpůsobit, jak je znázorněno v následující části.    
3. **Cílová virtuální síť:** ve výchozím nastavení, automatické obnovení systému vytvoří novou virtuální síť v oblasti cíl s názvem, který má příponu "Automatické obnovení systému". To budou mapována na zdrojové síti a bude se používat pro všechny budoucí ochranu.

    > [!NOTE]
    > [Sítě v podrobnostech](site-recovery-network-mapping-azure-to-azure.md) Další informace o mapování sítě.

4. **Cílové úložiště účtů:** ve výchozím nastavení, automatické obnovení systému se vytvoří nový účet cílového úložiště mimicking konfiguraci zdrojového virtuálního počítače úložiště. V případě, že účet úložiště, které jsou vytvořené automatické obnovení systému již existuje, bude znovu.

5. **Účty úložiště do mezipaměti:** automatické obnovení systému potřebuje účet úložiště s názvem úložiště mezipaměti v oblasti zdroje. Všechny změny děje na zdrojové virtuální počítače jsou sledovány a odesílat k účtu úložiště mezipaměti před replikaci do cílového umístění.

6. **Skupina dostupnosti:** ve výchozím nastavení, vytvoří nástroj Automatické obnovení systému novou skupinou dostupnosti ve cílová oblast s názvem, který má příponu "Automatické obnovení systému". V případě, že skupina dostupnosti vytvořené automatické obnovení systému již existuje, bude znovu.

7.  **Zásady replikace:** definuje nastavení pro obnovení bodu uchování historie a aplikace snímky konzistentní s četnost. Ve výchozím nastavení bude automatické obnovení systému vytvořte novou zásadu replikace s výchozím nastavením: 24 hodin se pro uchování bodu obnovení a ' 60 minut frekvence snímky konzistentní s aplikací.

    ![Povolení replikace](./media/site-recovery-replicate-azure-to-azure/enabledrwizard3.PNG)

## <a name="customize-target-resources"></a>Přizpůsobení cílové prostředky

V případě, že chcete změnit výchozí nastavení používané automatické obnovení systému, můžete změnit nastavení na základě potřeb.

1. **Přizpůsobení:** klikněte na něj chcete-li změnit výchozí hodnoty používané automatické obnovení systému.

2. **Cílová skupina prostředků:** skupiny prostředků můžete vybrat ze seznamu všech skupin prostředků existující v cílovém umístění v rámci předplatného.

3. **Cílová virtuální síť:** seznamu všechny virtuální sítě můžete najít v cílovém umístění.

4. **Skupina dostupnosti:** nastavení sad dostupnosti můžete přidat pouze pro virtuální počítače, které jsou součástí dostupnosti v oblasti zdroje.

5. **Cílem účtů úložiště:**

![Povolit replikaci](./media/site-recovery-replicate-azure-to-azure/customize.PNG) klikněte na **vytvořit cílový prostředek** a zapnout replikaci


Jakmile jsou chráněné virtuální počítače můžete zkontrolovat stav stavu virtuálních počítačů v rámci **replikované položky**

>[!NOTE]
>V době počáteční replikace existuje může možnost, že stav trvá určitou dobu k aktualizaci a nevidíte průběh nějakou dobu. Kliknutím na tlačítko Aktualizovat nahoře v okně a získat nejnovější stav.
>

![Povolení replikace](./media/site-recovery-replicate-azure-to-azure/replicateditems.PNG)


## <a name="next-steps"></a>Další kroky
- [Další informace](site-recovery-test-failover-to-azure.md) o spuštění převzetí služeb při selhání.
- [Další informace](site-recovery-failover.md) o různých typech převzetí služeb při selhání a jak je spouštět.
- Další informace o [pomocí plánů obnovení](site-recovery-create-recovery-plans.md) ke snížení RTO.
- Další informace o [opětovnou ochranu virtuálních počítačů Azure](site-recovery-how-to-reprotect.md) po převzetí služeb při selhání.
