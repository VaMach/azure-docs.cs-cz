---
title: "Instalaci sady Visual Studio a připojte se k Azure zásobníku | Microsoft Docs"
description: "Další kroky potřebné k instalaci sady Visual Studio a připojte se k Azure zásobníku"
services: azure-stack
documentationcenter: 
author: heathl17
manager: byronr
editor: 
ms.assetid: 2022dbe5-47fd-457d-9af3-6c01688171d7
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/25/2017
ms.author: helaw
ms.openlocfilehash: 5487125095f05b2fbfa9489c5b4733f61c0212d4
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/11/2017
---
# <a name="install-visual-studio-and-connect-to-azure-stack"></a>Instalaci sady Visual Studio a připojte se k Azure zásobníku

*Platí pro: Azure zásobníku integrované systémy a Azure zásobníku Development Kit*

Použijte sadu Visual Studio pro vytváření a nasazení Azure Resource Manager [šablony](user/azure-stack-arm-templates.md) v zásobníku Azure. Můžete použít kroků popsaných v tomto článku nainstalujte sadu Visual Studio buď z [Azure zásobníku Development Kit](azure-stack-connect-azure-stack.md#connect-to-azure-stack-with-remote-desktop), nebo z založené na Windows externí klienta Pokud jste připojení prostřednictvím [VPN](azure-stack-connect-azure-stack.md#connect-to-azure-stack-with-vpn). Tyto kroky provést nové instalace nástroje Visual Studio 2015 Community Edition. Další informace o [koexistence](https://msdn.microsoft.com/library/ms246609.aspx) mezi ostatních verzí sady Visual Studio.

## <a name="install-visual-studio"></a>Instalace sady Visual Studio
1. Stažení a spuštění [instalačního programu webové platformy](https://www.microsoft.com/web/downloads/platform.aspx).             
2. Vyhledejte **Visual Studio Community 2015 s Microsoft Azure SDK - 2.9.6**, klikněte na tlačítko **přidat**, a **nainstalovat**.

    ![Postup instalace – snímek obrazovky WebPI](./media/azure-stack-install-visual-studio/image1.png) 

3. Odinstalace **Microsoft Azure PowerShell** který je nainstalován jako součást sady Azure SDK.

    ![Snímek obrazovky Přidat nebo odebrat programy rozhraní pro prostředí Azure PowerShell](./media/azure-stack-install-visual-studio/image2.png) 

4. [Instalace PowerShellu pro Azure Stack](azure-stack-powershell-install.md)

5. Po dokončení instalace, restartujte operační systém.

## <a name="connect-to-azure-stack"></a>Připojení ke službě Azure Stack

1. Spusťte sadu Visual Studio.

2. Z **zobrazení** nabídce vyberte možnost **Průzkumník cloudu**.

3. V podokně nové vyberte **přidat účet** a přihlaste se pomocí přihlašovacích údajů Azure Active Directory.  
    ![Průzkumník cloudu snímek obrazovky po přihlášení a připojení k Azure zásobníku](./media/azure-stack-install-visual-studio/image6.png)

Po přihlášení, můžete [nasazení šablon](user/azure-stack-deploy-template-visual-studio.md) nebo vyhledejte dostupných typů prostředků a skupin prostředků, můžete vytvořit vlastní šablony.  

## <a name="next-steps"></a>Další kroky

 - [Vývoj šablon pro Azure zásobníku](user/azure-stack-develop-templates.md)
