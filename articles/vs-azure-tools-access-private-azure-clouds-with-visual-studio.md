---
title: "Přístup k privátní cloud Azure pomocí sady Visual Studio | Microsoft Docs"
description: "Zjistěte, jak získat přístup k prostředkům privátního cloudu pomocí sady Visual Studio."
services: visual-studio-online
documentationcenter: na
author: kraigb
manager: ghogen
editor: 
ms.assetid: 9d733c8d-703b-44e7-a210-bb75874c45c8
ms.service: multiple
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: multiple
ms.date: 03/19/2017
ms.author: kraigb
ms.openlocfilehash: b2578c837732ab05d538e9b896ed3a3035075a70
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/11/2017
---
# <a name="accessing-private-azure-clouds-with-visual-studio"></a>Přístup k privátní cloud Azure pomocí sady Visual Studio
Ve výchozím nastavení Visual Studio podporuje koncové body REST veřejného cloudu Azure. V tomto tématu se naučíte používat privátní cloud certifikát pro přístup k – a interakci s - privátního cloudu ze sady Visual Studio.

## <a name="to-access-a-private-azure-cloud-in-visual-studio"></a>Pro přístup k privátním Azure cloud v sadě Visual Studio
1. V [portál Azure classic](http://go.microsoft.com/fwlink/?LinkID=213885) pro daný privátní cloud, stáhněte soubor nastavení publikování, nebo se obraťte na svého správce pro soubor nastavení publikování. Ve veřejné verzi Azure, je odkaz na stažení to [https://manage.windowsazure.com/publishsettings/](https://manage.windowsazure.com/publishsettings/). (Stažený soubor by měl mít příponu `.publishsettings`)

1. Otevřete Visual Studio

1. V **Průzkumníka serveru**, klikněte pravým tlačítkem myši **Azure** uzel a v místní nabídce vyberte **spravovat a odběry filtru**.
   
    ![Spravovat odběry příkaz](./media/vs-azure-tools-access-private-azure-clouds-with-visual-studio/IC790778.png)

1. V **spravovat předplatná Microsoft Azure** dialogovém okně, vyberte **certifikáty** a pak vyberte **Import**.
   
    ![Import certifikátů Azure](./media/vs-azure-tools-access-private-azure-clouds-with-visual-studio/IC790779.png)

1. V **předplatná Microsoft Azure Import** dialogovém okně, vyberte **Procházet**.

    ![V dialogovém okně předplatná Microsoft Azure Import tlačítko Procházet](./media/vs-azure-tools-access-private-azure-clouds-with-visual-studio/browse-button.png)

1. V **otevřete** dialogové okno, přejděte do adresáře, kam jste uložili soubor nastavení publikování, vyberte soubor a pak vyberte **otevřete**.

    ![Vyberte soubor nastavení publikování](./media/vs-azure-tools-access-private-azure-clouds-with-visual-studio/select-publish-settings-file.png)

1. Když se vrátíte na **předplatná Microsoft Azure Import** dialogovém okně, vyberte **Import**.

    ![Importujte soubor nastavení publikování](./media/vs-azure-tools-access-private-azure-clouds-with-visual-studio/IC790780.png)

    Certifikáty jsou importovány ze souboru nastavení publikování do sady Visual Studio a nyní můžete pracovat s vaší privátní cloudové prostředky.
   
## <a name="next-steps"></a>Další kroky
- [Publikování do cloudové služby Azure ze sady Visual Studio](https://msdn.microsoft.com/library/azure/ee460772.aspx)
- [Postupy: stahování a Import publikovat nastavení a informace o předplatném](https://msdn.microsoft.com/library/dn385850\(v=nav.70\).aspx)
