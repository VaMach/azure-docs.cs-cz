---
title: "Nakonfigurovat privátní IP adresy pro virtuální počítače (klasické) - portálu Azure | Microsoft Docs"
description: "Zjistěte, jak nakonfigurovat privátní IP adresy pro virtuální počítače (klasické) pomocí portálu Azure."
services: virtual-network
documentationcenter: na
author: jimdial
manager: timlt
editor: tysonn
tags: azure-service-management
ms.assetid: b8ef8367-58b2-42df-9f26-3269980950b8
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/04/2016
ms.author: jdial
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: bde6de3495c2909b63b1f85e420a4ff5e7ac2c1a
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/11/2017
---
# <a name="configure-private-ip-addresses-for-a-virtual-machine-classic-using-the-azure-portal"></a>Nakonfigurovat privátní IP adresy pro virtuální počítač (klasický) pomocí portálu Azure

[!INCLUDE [virtual-networks-static-private-ip-selectors-classic-include](../../includes/virtual-networks-static-private-ip-selectors-classic-include.md)]

[!INCLUDE [virtual-networks-static-private-ip-intro-include](../../includes/virtual-networks-static-private-ip-intro-include.md)]

[!INCLUDE [azure-arm-classic-important-include](../../includes/azure-arm-classic-important-include.md)]

Tento článek se týká modelu nasazení Classic. Můžete také [spravovat statickou privátní IP adresou v modelu nasazení Resource Manager](virtual-networks-static-private-ip-arm-pportal.md).

[!INCLUDE [virtual-networks-static-ip-scenario-include](../../includes/virtual-networks-static-ip-scenario-include.md)]

Následující ukázkový postup očekávat jednoduché prostředí již vytvořeny. Pokud chcete spustit kroky, jak jsou zobrazeny v tomto dokumentu, nejprve vytvořit testovací prostředí popsané v [vytvoření virtuální sítě](virtual-networks-create-vnet-classic-pportal.md).

## <a name="how-to-specify-a-static-private-ip-address-when-creating-a-vm"></a>Postup při vytváření virtuálního počítače zadat statickou privátní IP adresu
Vytvoření virtuálního počítače s názvem *DNS01* v *front-endu* podsíť virtuální sítě s názvem *TestVNet* se statickou privátní IP z *192.168.1.101*, postupujte podle následujících kroků:

1. V prohlížeči přejděte na http://portal.azure.com a v případě potřeby se přihlaste pomocí účtu Azure.
2. Klikněte na tlačítko **nový** > **výpočetní** > **Windows Server 2012 R2 Datacenter**, Všimněte si, že **vybrat model nasazení** seznam již ukazuje **Classic**a potom klikněte na **vytvořit**.
   
    ![Vytvoření virtuálního počítače na portálu Azure](./media/virtual-networks-static-ip-classic-pportal/figure01.png)
3. V **vytvoření virtuálního počítače** okno, zadejte název virtuálního počítače, který se má vytvořit (*DNS01* v tomto scénáři), účet místního správce a heslo.
   
    ![Vytvoření virtuálního počítače na portálu Azure](./media/virtual-networks-static-ip-classic-pportal/figure02.png)
4. Klikněte na tlačítko **volitelné konfiguraci** > **sítě** > **virtuální sítě**a potom klikněte na **TestVNet**. Pokud **TestVNet** není k dispozici, ujistěte se, že používáte *střed USA* umístění a vytvořili testovací prostředí popsané na začátku tohoto článku.
   
    ![Vytvoření virtuálního počítače na portálu Azure](./media/virtual-networks-static-ip-classic-pportal/figure03.png)
5. V **sítě** okno, zkontrolujte, zda je aktuálně vybrané podsítě *front-endu*, pak klikněte na tlačítko **IP adresy**v části **přiřazení IP adresy** klikněte na tlačítko **statické**a potom zadejte *192.168.1.101* pro **IP adresu** jak vidíte níže.
   
    ![Vytvoření virtuálního počítače na portálu Azure](./media/virtual-networks-static-ip-classic-pportal/figure04.png)    
6. Klikněte na tlačítko **OK** v **IP adresy** okno, pak klikněte na tlačítko **OK** v **sítě** a klikněte na **OK** v **volitelné konfigurace** okno.
7. V **vytvoření virtuálního počítače** okně klikněte na tlačítko **vytvořit**. Všimněte si dlaždice níže zobrazí v řídicím panelu.
   
    ![Vytvoření virtuálního počítače na portálu Azure](./media/virtual-networks-static-ip-classic-pportal/figure05.png)

## <a name="how-to-retrieve-static-private-ip-address-information-for-a-vm"></a>Jak načíst statickou privátní IP adresu informace pro virtuální počítač
Chcete-li zobrazit statických informací privátní IP adresu pro virtuální počítač vytvořený s výše uvedených kroků, proveďte následující postup.

1. Z portálu Azure Azure, klikněte na tlačítko **Procházet vše** > **virtuálních počítačů (klasické)** > **DNS01** > **všechna nastavení** > **IP adresy** a Všimněte si přiřazení IP adresy a IP adresu, jak vidíte níže.
   
    ![Vytvoření virtuálního počítače na portálu Azure](./media/virtual-networks-static-ip-classic-pportal/figure06.png)

## <a name="how-to-remove-a-static-private-ip-address-from-a-vm"></a>Postup odebrání statickou privátní IP adresu z virtuálního počítače
Chcete-li odebrat statickou privátní IP adresou z virtuálního počítače vytvořili výše, postupujte podle následujících kroků.

1. Z **IP adresy** uvedené výše, klikněte na **dynamické** napravo od **přiřazení IP adresy**, pak klikněte na tlačítko **Uložit**a potom klikněte na **Ano**.
   
    ![Vytvoření virtuálního počítače na portálu Azure](./media/virtual-networks-static-ip-classic-pportal/figure07.png)

## <a name="how-to-add-a-static-private-ip-address-to-an-existing-vm"></a>Postup přidání statickou privátní IP adresu do stávajícího virtuálního počítače
Chcete-li přidat statickou privátní IP adresu na virtuální počítač vytvořený pomocí výše uvedených kroků, postupujte podle následujících kroků:

1. Z **IP adresy** uvedené výše, klikněte na **statické** napravo od **přiřazení IP adresy**.
2. Typ *192.168.1.101* pro **IP adresu**, pak klikněte na tlačítko **Uložit**a potom klikněte na **Ano**.

## <a name="next-steps"></a>Další kroky
* Další informace o [vyhrazené veřejné IP adresy](virtual-networks-reserved-public-ip.md) adresy.
* Další informace o [veřejné IP (splnění) na úrovni instance](virtual-networks-instance-level-public-ip.md) adresy.
* Obrátit [vyhrazené IP REST API](https://msdn.microsoft.com/library/azure/dn722420.aspx).

