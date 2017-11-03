---
title: "Nasazení šablon pomocí sady Visual Studio v zásobníku Azure | Microsoft Docs"
description: "Informace o nasazení šablony pomocí sady Visual Studio v zásobníku Azure."
services: azure-stack
documentationcenter: 
author: HeathL17
manager: byronr
editor: 
ms.assetid: 628da2ae-64cc-42e0-b8b7-a6a3724cb974
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/25/2017
ms.author: helaw
ms.openlocfilehash: 8fc32dc50d96d202dfc982cbdc52d8e479c3a3eb
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/11/2017
---
# <a name="deploy-templates-in-azure-stack-using-visual-studio"></a>Nasazení šablon v zásobníku Azure pomocí sady Visual Studio

*Platí pro: Azure zásobníku integrované systémy a Azure zásobníku Development Kit*

Pomocí sady Visual Studio k nasazení šablony Azure Resource Manager zásobník Azure development Kit.

1. [Nainstalujte a připojte](azure-stack-install-visual-studio.md) do protokolů Azure pomocí sady Visual Studio.
2. Otevřete sadu Visual Studio.
3. Klikněte na tlačítko **soubor**, klikněte na tlačítko **nový**a v **nový projekt** dialogovém okně **skupiny prostředků Azure**.
4. Zadejte **název** pro nový projekt a pak klikněte na tlačítko **OK**.
5. V **vybrat šablonu Azure** dialogové okno, změny *zobrazit šablony z tohoto umístění* rozevírací seznam pro **zásobník Azure rychlý start**
6. Klikněte na tlačítko **101-create-storage-account**a potom klikněte na **OK**.  
7. V nový projekt, se zobrazí seznam dostupných šablon, které rozšířením **šablony** uzlu **Průzkumníku řešení** podokně.
8. V **Průzkumníku řešení** podokně klikněte pravým tlačítkem na název projektu, klikněte na tlačítko **nasadit**, pak klikněte na tlačítko **nové nasazení**.
9. V **nasadit do skupiny prostředků** dialogu **předplatné** rozevíracího seznamu, vyberte předplatné Microsoft Azure zásobníku.
10. V **skupiny prostředků** seznamu, vyberte existující skupinu prostředků nebo vytvořte novou.
11. V **umístění skupiny prostředků** seznam, vyberte umístění a pak klikněte na tlačítko **nasadit**.
12. V **upravit parametry** dialogovém okně zadejte hodnoty pro parametry (které se liší podle šablony) a potom klikněte na **Uložit**.

## <a name="next-steps"></a>Další kroky
[Nasazení šablon pomocí příkazového řádku](azure-stack-deploy-template-command-line.md)

[Vývoj šablon pro Azure zásobníku](azure-stack-develop-templates.md)

