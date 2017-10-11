---
title: "Připojení ke službě Azure Analysis Services s Power BI | Microsoft Docs"
description: "Zjistěte, jak se připojit k serveru Azure Analysis Services pomocí Power BI."
services: analysis-services
documentationcenter: 
author: minewiskan
manager: erikre
editor: 
tags: 
ms.assetid: 
ms.service: analysis-services
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: na
ms.date: 08/15/2017
ms.author: owend
ms.openlocfilehash: 3a2af043feddb4a1d6d63f50e838c8a39035449f
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="connect-with-power-bi"></a>Připojit k Power BI

Jakmile jste vytvořili serveru v Azure a nasadili do ní tabulkový model, jste připravení připojit a začít zkoumat data uživatele ve vaší organizaci. 

> [!TIP]
> Nezapomeňte použít nejnovější verzi [Power BI Desktop](https://powerbi.microsoft.com/desktop/).
> 
> 
  
## <a name="connect-in-power-bi-desktop"></a>Připojení v Power BI Desktop

1. V Power BI Desktop, klikněte na **načíst Data** > **Azure** > **databáze Azure Analysis Services**.

2. V **Server**, zadejte název serveru. 
    
    Nezapomeňte zahrnout úplnou adresu URL. Například asazure://westcentralus.asazure.windows.net/advworks.

3. V **databáze**, pokud znáte název databázi tabulkového modelu nebo perspektivy, kterou chcete připojit, vložte jej zde. Jinak můžete ponechat toto pole prázdné a vybrat databázi nebo perspektivy později.

4. Ponechte výchozí nastavení **připojit za provozu** možnost a potom stiskněte klávesu **Connect**. 

5. Po zobrazení výzvy zadejte své přihlašovací údaje. 

6. V **Navigátor**, rozbalte server a pak vyberte modelu nebo perspektivy chcete připojit, pak klikněte na tlačítko **Connect**. Klikněte na modelu nebo perspektivy zobrazíte všechny objekty pro toto zobrazení.

    Model se otevře v Power BI Desktop s prázdnou sestavou v zobrazení sestavy. V seznamu polí zobrazí všechny objekty modelu nebyla skrytá. Stav připojení je zobrazen v pravém dolním rohu.

## <a name="connect-in-power-bi-service"></a>Připojení v Power BI (služba)

1. Vytvořte soubor Power BI Desktop, která má aktivní připojení k vašemu modelu na vašem serveru.
2. V [Power BI](https://powerbi.microsoft.com), klikněte na tlačítko **načíst Data** > **soubory**. Vyhledejte a vyberte soubor.



## <a name="see-also"></a>Viz také
[Připojení ke službě Azure Analysis Services](analysis-services-connect.md)   
[Knihovny klienta](analysis-services-data-providers.md)

