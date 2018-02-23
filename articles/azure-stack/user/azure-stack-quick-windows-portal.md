---
title: "Azure zásobníku rychlý Start – vytvoření virtuálního počítače s Windows"
description: "Azure zásobníku rychlý Start - vytvořit virtuální počítač s Windows pomocí portálu"
services: azure-stack
author: brenduns
manager: femila
ms.service: azure-stack
ms.topic: quickstart
ms.date: 09/15/2017
ms.author: brenduns
ms.reviewer: 
ms.custom: mvc
ms.openlocfilehash: 9df976fd26318038ab4152863638e262d7f284aa
ms.sourcegitcommit: d1f35f71e6b1cbeee79b06bfc3a7d0914ac57275
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 02/22/2018
---
# <a name="create-a-windows-virtual-machine-with-the-azure-stack-portal"></a>Vytvoření virtuálního počítače s Windows pomocí portálu Azure zásobníku

Vytvořením virtuálního počítače s Windows pomocí portálu Azure zásobníku. Tento portál je rozhraní založené na prohlížeči uživatele, kde můžete vytvářet, konfigurovat a spravovat prostředky.

## <a name="sign-in-to-the-azure-stack-portal"></a>Přihlaste se k portálu Azure zásobníku

Přihlaste se k portálu Azure zásobníku. Na adresu na portálu Azure zásobníku závisí, na který produkt Azure zásobníku se připojujete k:

* Pro Azure zásobníku Development Kit (ASDK) přejděte na: https://portal.local.azurestack.external.
* Pro systém Azure zásobníku integrované přejděte na adresu URL, kterou vaše operátor zásobník Azure poskytuje.

## <a name="create-a-virtual-machine"></a>Vytvoření virtuálního počítače

1. Klikněte na tlačítko **nové** > **výpočetní** > **zkušební verze systému Windows Server 2016 Datacenter** > **vytvořit**. Pokud nevidíte **Windows Server 2016 Datacenter Eval** položku, obraťte se na vaše operátor zásobník Azure. Požádejte si ho jak je popsáno v přidat na Marketplace s cílem [přidat bitovou kopii virtuálního počítače Windows serveru 2016 do zásobníku Azure marketplace](../azure-stack-add-default-image.md) článku. 
    ![](media/azure-stack-quick-windows-portal/image01.png)
2. V části **Základy**, zadejte **název**, **uživatelské jméno**, a **heslo**. Vyberte **předplatné**. Vytvoření **skupiny prostředků**, nebo vyberte existující jeden, vyberte možnost **umístění**a potom klikněte na **OK**.

    ![](media/azure-stack-quick-windows-portal/image02.png)
3. V části **zvolte velikost**, klikněte na tlačítko **D1 standardní** > **vyberte**.
    ![](media/azure-stack-quick-windows-portal/image03.png)
4. V části **nastavení**, přijměte výchozí hodnoty a klikněte na **OK**.
    ![](media/azure-stack-quick-windows-portal/image04.png)
5. V části **Souhrn**, klikněte na tlačítko **OK** k vytvoření virtuálního počítače. 
    ![](media/azure-stack-quick-windows-portal/image05.png)
6. Chcete-li zobrazit nový virtuální počítač, klikněte na tlačítko **všechny prostředky**, vyhledejte virtuální počítač a klikněte na jeho název.
    ![](media/azure-stack-quick-windows-portal/image06.png)

## <a name="clean-up-resources"></a>Vyčištění prostředků

Když virtuální počítač již nepotřebujete, odstraňte skupinu prostředků, virtuální počítač a všechny související prostředky. To pokud chcete udělat, vyberte skupinu prostředků ze strany virtuálního počítače a klikněte na tlačítko **odstranit**.

## <a name="next-steps"></a>Další postup
V této úvodní jste nasadili jednoduchého virtuálního počítače Windows. Další informace o virtuálních počítačích Azure zásobníku, nadále [důležité informace pro virtuální počítače v Azure zásobníku](azure-stack-vm-considerations.md).
