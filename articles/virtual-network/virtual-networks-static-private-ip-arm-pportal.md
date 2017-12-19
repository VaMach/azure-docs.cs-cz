---
title: "Nakonfigurovat privátní IP adresy pro virtuální počítače - portálu Azure | Microsoft Docs"
description: "Zjistěte, jak nakonfigurovat privátní IP adresy pro virtuální počítače pomocí portálu Azure."
services: virtual-network
documentationcenter: na
author: jimdial
manager: timlt
editor: tysonn
tags: azure-resource-manager
ms.assetid: 11245645-357d-4358-9a14-dd78e367b494
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/04/2016
ms.author: jdial
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 672462fad715758e50680fa5bade4b1f9d50e6e5
ms.sourcegitcommit: 821b6306aab244d2feacbd722f60d99881e9d2a4
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 12/18/2017
---
# <a name="configure-private-ip-addresses-for-a-virtual-machine-using-the-azure-portal"></a>Nakonfigurovat privátní IP adresy pro virtuální počítač pomocí portálu Azure

> [!div class="op_single_selector"]
> * [Azure Portal](virtual-networks-static-private-ip-arm-pportal.md)
> * [PowerShell](virtual-networks-static-private-ip-arm-ps.md)
> * [Azure CLI](virtual-networks-static-private-ip-arm-cli.md)
> * [Portál Azure (klasický)](virtual-networks-static-private-ip-classic-pportal.md)
> * [PowerShell (Classic)](virtual-networks-static-private-ip-classic-ps.md)
> * [Rozhraní příkazového řádku Azure (klasický)](virtual-networks-static-private-ip-classic-cli.md)


[!INCLUDE [virtual-networks-static-private-ip-intro-include](../../includes/virtual-networks-static-private-ip-intro-include.md)]

[!INCLUDE [azure-arm-classic-important-include](../../includes/azure-arm-classic-important-include.md)]

Tento článek se týká modelu nasazení Resource Manager. Můžete také [spravovat statickou privátní IP adresou v modelu nasazení classic](virtual-networks-static-private-ip-classic-pportal.md).

[!INCLUDE [virtual-networks-static-ip-scenario-include](../../includes/virtual-networks-static-ip-scenario-include.md)]

Následující ukázkový postup očekávat jednoduché prostředí již vytvořeny. Pokud chcete spustit kroky, jak jsou zobrazeny v tomto dokumentu, nejprve vytvořit testovací prostředí popsané v [vytvoření virtuální sítě](virtual-networks-create-vnet-arm-pportal.md).

## <a name="how-to-create-a-vm-for-testing-static-private-ip-addresses"></a>Postup vytvoření virtuálního počítače pro testování statickou privátní IP adresy
Statickou privátní IP adresu nelze nastavit při vytváření virtuálního počítače v režimu Resource Manager nasazení pomocí portálu Azure. Je nutné nejprve vytvořit virtuální počítač, vložíte jej nastavit jeho privátní IP adresy na být statická.

Vytvoření virtuálního počítače s názvem *DNS01* v *front-endu* podsíť virtuální sítě s názvem *TestVNet*, použijte následující postup.

1. V prohlížeči přejděte na http://portal.azure.com a v případě potřeby se přihlaste pomocí účtu Azure.
2. Klikněte na tlačítko **nový** > **výpočetní** > **Windows Server 2012 R2 Datacenter**, Všimněte si, že **vybrat model nasazení** seznam již ukazuje **Resource Manager**a potom klikněte na **vytvořit**, jak je vidět na obrázku níže.
   
    ![Vytvoření virtuálního počítače na portálu Azure](./media/virtual-networks-static-ip-arm-pportal/figure01.png)
3. V **Základy** okno, zadejte název virtuálního počítače, který se má vytvořit (*DNS01* v tomto scénáři), účet místního správce a hesla, jak je vidět na obrázku níže.
   
    ![Okno Základy](./media/virtual-networks-static-ip-arm-pportal/figure02.png)
4. Zajistěte, aby **umístění** vybraná *střed USA*, pak klikněte na tlačítko **vyberte existující** v části **skupiny prostředků**, klikněte **skupiny prostředků** znovu, klikněte *TestRG*a potom klikněte na **OK**.
   
    ![Okno Základy](./media/virtual-networks-static-ip-arm-pportal/figure03.png)
5. V **zvolte velikost** vyberte **A1 standardní**a potom klikněte na **vyberte**.
   
    ![Zvolte velikost okna](./media/virtual-networks-static-ip-arm-pportal/figure04.png)    
6. V **nastavení** okno, zkontrolujte následující vlastnosti jsou nastavené se nastavují s níže uvedené hodnoty a potom klikněte na **OK**.
   
    -**Účet úložiště**: *vnetstorage*
   
   * **Síť**: *TestVNet*
   * **Podsíť**: *front-endu*
     
     ![Zvolte velikost okna](./media/virtual-networks-static-ip-arm-pportal/figure05.png)     
7. V **Souhrn** okně klikněte na tlačítko **OK**. Všimněte si dlaždice níže zobrazí v řídicím panelu.
   
    ![Vytvoření virtuálního počítače na portálu Azure](./media/virtual-networks-static-ip-arm-pportal/figure06.png)

## <a name="how-to-retrieve-static-private-ip-address-information-for-a-vm"></a>Jak načíst statickou privátní IP adresu informace pro virtuální počítač
Chcete-li zobrazit statických informací privátní IP adresu pro virtuální počítač vytvořený s výše uvedených kroků, proveďte následující postup.

1. Z portálu Azure Azure, klikněte na tlačítko **Procházet vše** > **virtuální počítače** > **DNS01** > **všechna nastavení** > **síťových rozhraní** a potom klikněte na uvedené jenom síťové rozhraní.
   
    ![Nasazení virtuálních počítačů dlaždice](./media/virtual-networks-static-ip-arm-pportal/figure07.png)
2. V **síťové rozhraní** okně klikněte na tlačítko **všechna nastavení** > **IP adresy** a Všimněte si **přiřazení** a **IP adresu** hodnoty.
   
    ![Nasazení virtuálních počítačů dlaždice](./media/virtual-networks-static-ip-arm-pportal/figure08.png)

## <a name="how-to-add-a-static-private-ip-address-to-an-existing-vm"></a>Postup přidání statickou privátní IP adresu do stávajícího virtuálního počítače
Chcete-li přidat statickou privátní IP adresu na virtuální počítač vytvořený pomocí výše uvedených kroků, postupujte podle následujících kroků:

1. Z **IP adresy** uvedené výše, klikněte na **statické** pod **přiřazení**.
2. Typ *192.168.1.101* pro **IP adresu**a potom klikněte na **Uložit**.
   
    ![Vytvoření virtuálního počítače na portálu Azure](./media/virtual-networks-static-ip-arm-pportal/figure09.png)

> [!NOTE]
> Pokud po kliknutí na **Uložit** si všimnete, že je přiřazení stále nastavená na **dynamické**, znamená to, že zadaná adresa IP je již používán. Zkuste jinou IP adresu.
> 
> 

## <a name="how-to-remove-a-static-private-ip-address-from-a-vm"></a>Postup odebrání statickou privátní IP adresu z virtuálního počítače
Chcete-li odebrat statickou privátní IP adresou z virtuálního počítače vytvořili výše, proveďte následující krok:

Z **IP adresy** uvedené výše, klikněte na **dynamické** pod **přiřazení**a potom klikněte na **Uložit**.

## <a name="next-steps"></a>Další kroky
* Další informace o [vyhrazené veřejné IP adresy](virtual-networks-reserved-public-ip.md) adresy.
* Další informace o [veřejné IP (splnění) na úrovni instance](virtual-networks-instance-level-public-ip.md) adresy.
* Obrátit [vyhrazené IP REST API](https://msdn.microsoft.com/library/azure/dn722420.aspx).

