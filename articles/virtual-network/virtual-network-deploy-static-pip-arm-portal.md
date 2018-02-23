---
title: "Vytvoření virtuálního počítače se statickou veřejnou IP adresu - portálu Azure | Microsoft Docs"
description: "Naučte se vytvořit virtuální počítač se statickou veřejnou IP adresu pomocí portálu Azure."
services: virtual-network
documentationcenter: na
author: jimdial
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: e9546bcc-f300-428f-b94a-056c5bd29035
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/04/2016
ms.author: jdial
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 53e9b55a82e7750393dfb6c4818681028c5c4d2f
ms.sourcegitcommit: d87b039e13a5f8df1ee9d82a727e6bc04715c341
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 02/21/2018
---
# <a name="create-a-vm-with-a-static-public-ip-address-using-the-azure-portal"></a>Vytvoření virtuálního počítače se statickou veřejnou IP adresu pomocí portálu Azure

> [!div class="op_single_selector"]
> * [portál Azure Portal](virtual-network-deploy-static-pip-arm-portal.md)
> * [PowerShell](virtual-network-deploy-static-pip-arm-ps.md)
> * [Azure CLI](virtual-network-deploy-static-pip-arm-cli.md)
> * [Šablona](virtual-network-deploy-static-pip-arm-template.md)
> * [PowerShell (Classic)](virtual-networks-reserved-public-ip.md)

[!INCLUDE [virtual-network-deploy-static-pip-intro-include.md](../../includes/virtual-network-deploy-static-pip-intro-include.md)]

> [!NOTE]
> Azure má dva různé modely nasazení pro vytváření prostředků a práci s nimi: [Resource Manager a klasický model](../resource-manager-deployment-model.md). Tento článek se zabývá pomocí modelu nasazení Resource Manager, které společnost Microsoft doporučuje pro většinu nových nasazení místo modelu nasazení classic.

[!INCLUDE [virtual-network-deploy-static-pip-scenario-include.md](../../includes/virtual-network-deploy-static-pip-scenario-include.md)]

## <a name="create-a-vm-with-a-static-public-ip"></a>Vytvoření virtuálního počítače se statickou veřejnou IP adresu

Postup vytvoření virtuálního počítače se statickou veřejnou IP adresu na portálu Azure, proveďte následující kroky:

1. V prohlížeči přejděte na portál [Azure Portal](https://portal.azure.com) a v případě potřeby se přihlaste pomocí účtu Azure.
2. V levém horním rohu na portálu klikněte na tlačítko **vytvořit prostředek**>>**výpočetní**>**Windows Server 2012 R2 Datacenter**.
3. V **vybrat model nasazení** vyberte **Resource Manager** a klikněte na tlačítko **vytvořit**.
4. V **Základy** podokně zadejte následující informace o virtuálních počítačů a pak klikněte na tlačítko **OK**.
   
    ![Portál Azure – základy](./media/virtual-network-deploy-static-pip-arm-portal/figure1.png)
5. V **zvolte velikost** podokně klikněte na tlačítko **A1 standardní** způsobem a potom klikněte na **vyberte**.
   
    ![Portál Azure – zvolte velikost](./media/virtual-network-deploy-static-pip-arm-portal/figure2.png)
6. V **nastavení** podokně klikněte na tlačítko **veřejnou IP adresu**, potom v **vytvoření veřejné IP adresy** podokně v části **přiřazení**, klikněte na tlačítko  **Statické** jako následujícím způsobem. A pak klikněte na **OK**.
   
    ![Portál Azure – vytvoření veřejné IP adresy](./media/virtual-network-deploy-static-pip-arm-portal/figure3.png)
7. V **nastavení** podokně klikněte na tlačítko **OK**.
8. Zkontrolujte **Souhrn** podokně způsobem a potom klikněte na **OK**.
   
    ![Portál Azure – vytvoření veřejné IP adresy](./media/virtual-network-deploy-static-pip-arm-portal/figure4.png)
9. Všimněte si nové dlaždice na řídicím panelu.
   
    ![Portál Azure – vytvoření veřejné IP adresy](./media/virtual-network-deploy-static-pip-arm-portal/figure5.png)
10. Po vytvoření virtuálního počítače **nastavení** podokně zobrazí následujícím způsobem:
    
    ![Portál Azure – vytvoření veřejné IP adresy](./media/virtual-network-deploy-static-pip-arm-portal/figure6.png)

