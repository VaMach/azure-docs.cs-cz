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
ms.openlocfilehash: 233e4eea8439320c1c7446e2c2b2e9d379351a3e
ms.sourcegitcommit: b5c6197f997aa6858f420302d375896360dd7ceb
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 12/21/2017
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
2. V levém horním rohu portálu, klikněte na tlačítko **nový**>>**výpočetní**>**Windows Server 2012 R2 Datacenter**.
3. V **vybrat model nasazení** vyberte **Resource Manager** a klikněte na tlačítko **vytvořit**.
4. V **Základy** okno, zadejte informace o virtuálních počítačů, jak je uvedeno níže a pak klikněte na tlačítko **OK**.
   
    ![Portál Azure – základy](./media/virtual-network-deploy-static-pip-arm-portal/figure1.png)
5. V **zvolte velikost** okně klikněte na tlačítko **A1 standardní** jak je uvedeno níže a potom klikněte na **vyberte**.
   
    ![Portál Azure – zvolte velikost](./media/virtual-network-deploy-static-pip-arm-portal/figure2.png)
6. V **nastavení** okně klikněte na tlačítko **veřejnou IP adresu**, potom v **vytvoření veřejné IP adresy** okno, v části **přiřazení**, klikněte na tlačítko **statické** jak je uvedeno níže. A pak klikněte na **OK**.
   
    ![Portál Azure – vytvoření veřejné IP adresy](./media/virtual-network-deploy-static-pip-arm-portal/figure3.png)
7. V **nastavení** okně klikněte na tlačítko **OK**.
8. Zkontrolujte **Souhrn** okno, jak je uvedeno níže a pak klikněte na tlačítko **OK**.
   
    ![Portál Azure – vytvoření veřejné IP adresy](./media/virtual-network-deploy-static-pip-arm-portal/figure4.png)
9. Všimněte si nové dlaždice na řídicím panelu.
   
    ![Portál Azure – vytvoření veřejné IP adresy](./media/virtual-network-deploy-static-pip-arm-portal/figure5.png)
10. Po vytvoření virtuálního počítače **nastavení** okno se zobrazí, jak je uvedeno níže
    
    ![Portál Azure – vytvoření veřejné IP adresy](./media/virtual-network-deploy-static-pip-arm-portal/figure6.png)

