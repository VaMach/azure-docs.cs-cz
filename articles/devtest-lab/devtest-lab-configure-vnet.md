---
title: "Konfigurace virtuální sítě v Azure DevTest Labs | Microsoft Docs"
description: "Zjistěte, jak nakonfigurovat existující virtuální síť a podsíť a využít ve virtuálním počítači s Azure DevTest Labs"
services: devtest-lab,virtual-machines
documentationcenter: na
author: tomarcher
manager: douge
editor: 
ms.assetid: 6cda99c2-b87e-4047-90a0-5df10d8e9e14
ms.service: devtest-lab
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/16/2017
ms.author: tarcher
ms.openlocfilehash: 19dbb1625f46f8864413dc538a96b2413bc6eea0
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/11/2017
---
# <a name="configure-a-virtual-network-in-azure-devtest-labs"></a>Konfigurace virtuální sítě v Azure DevTest Labs
Jak je popsáno v článku, [Přidání virtuálního počítače s artefakty do testovacího prostředí](devtest-lab-add-vm-with-artifacts.md), při vytváření virtuálních počítačů v testovacím prostředí, můžete zadat nakonfigurované virtuální sítě. Jeden scénář tohoto postupu je, pokud potřebujete přístup k prostředkům corpnet z virtuálních počítačů pomocí virtuální sítě, která byla konfigurovaná s ExpressRoute nebo VPN typu site-to-site. Následující části ukazují, jak přidat existující virtuální sítě do testovacího prostředí na nastavení virtuální sítě, aby bylo možné zvolit při vytváření virtuálních počítačů.

## <a name="configure-a-virtual-network-for-a-lab-using-the-azure-portal"></a>Konfigurace virtuální sítě pro testovacím prostředí pomocí portálu Azure
Následující postup vás provede procesem přidání existující virtuální síť (a podsítě) do testovacího prostředí, aby se můžete použít při vytváření virtuálního počítače ve stejné testovací prostředí. 

1. Přihlaste se k webu [Azure Portal](http://go.microsoft.com/fwlink/p/?LinkID=525040).
2. Vyberte **více služeb**a potom vyberte **DevTest Labs** ze seznamu.
3. Ze seznamu labs vyberte požadované testovací prostředí. 
4. V okně v prostředí, vyberte **konfiguraci a zásady**.
5. V tomto prostředí **konfiguraci a zásady** vyberte **virtuální sítě**.
6. Na **virtuální sítě** okno, zobrazí se seznam virtuálních sítí, které jsou nakonfigurované pro aktuální prostředí a také výchozí virtuální síť, která se vytvoří pro testovací prostředí. 
7. Vyberte **+ přidat**.
   
    ![Přidat existující virtuální síť do testovacího prostředí](./media/devtest-lab-configure-vnet/lab-settings-vnet-add.png)
8. Na **virtuální síť** vyberte **[vyberte virtuální síť]**.
   
    ![Vyberte existující virtuální síť](./media/devtest-lab-configure-vnet/lab-settings-vnets-vnet1.png)
9. Na **zvolte virtuální síť** okně, vyberte požadované virtuální sítě. V okně zobrazí všechny virtuální sítě, které jsou ve stejné oblasti v rámci předplatného jako testovací prostředí.  
10. Po výběru virtuální sítě, vrátíte se **virtuální síť** podsíť v seznamu v dolní části okna klikněte na tlačítko.

    ![Seznam podsítí](./media/devtest-lab-configure-vnet/lab-settings-vnets-vnet2.png)
    
    Zobrazí se okno podsíť testovacího prostředí.

    ![Okno podsíť testovacího prostředí](./media/devtest-lab-configure-vnet/lab-subnet.png)

11. Zadejte **název podsítě testovacího prostředí**.
12. Chcete-li povolit podsíť, kterou chcete použít v testovacím prostředí vytvoření virtuálního počítače, vyberte **použití v vytvoření virtuálního počítače**.
13. Chcete-li povolit [sdílené veřejnou IP adresu](devtest-lab-shared-ip.md), vyberte **povolení sdíleného veřejnou IP adresu**.
14. Chcete-li povolit veřejné IP adresy v podsíti, vyberte **povolit vytvoření veřejné IP**.
15. V **maximální počet virtuálních počítačů na uživatele** pole zadejte maximální virtuálních počítačů na uživatele pro každou podsíť. Pokud chcete neomezený počet virtuálních počítačů, nechte pole prázdné.
16. Vyberte **OK** zavřete okno podsíť testovacího prostředí.
17. Vyberte **Uložit** zavřete okno virtuální sítě.
18. Teď, když je nakonfigurovaný virtuální sítě, můžete vybrat při vytváření virtuálního počítače. 
    Informace o tom, jak vytvořit virtuální počítač a zadejte virtuální síť, naleznete v článku [Přidání virtuálního počítače s artefakty do testovacího prostředí](devtest-lab-add-vm-with-artifacts.md). 

[!INCLUDE [devtest-lab-try-it-out](../../includes/devtest-lab-try-it-out.md)]

## <a name="next-steps"></a>Další kroky
Po přidání požadované virtuální sítě do vašeho testovacího prostředí, dalším krokem je [přidat virtuální počítač do testovacího prostředí](devtest-lab-add-vm-with-artifacts.md).

