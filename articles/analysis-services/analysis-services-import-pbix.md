---
title: Importovat soubor Power BI Desktop do Azure Analysis Services | Microsoft Docs
description: "Popisuje, jak importovat soubor Power BI Desktop (soubor pbix) pomocí portálu Azure."
services: analysis-services
documentationcenter: 
author: minewiskan
manager: kfile
editor: 
tags: 
ms.assetid: 
ms.service: analysis-services
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: na
ms.date: 02/26/2018
ms.author: owend
ms.openlocfilehash: 43eab587a1e5209069a248f1e2e1f57af158a2b8
ms.sourcegitcommit: 088a8788d69a63a8e1333ad272d4a299cb19316e
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 02/27/2018
---
# <a name="import-a-power-bi-desktop-file"></a>Importovat soubor Power BI Desktop

Můžete vytvořit nový model v Azure AS importováním souboru Power BI Desktop soubor (soubor pbix). Metadata modelu, data uložená v mezipaměti a připojení zdroje dat jsou importovány. Nelze importovat, sestavy a vizualizací. Jednou na vašem serveru modelu změny můžete provést aktualizaci a znovu importujte soubor pbix, pomocí funkce webové návrháře (preview) na portálu nebo pomocí SQL Server Management Studio (SSMS). Importované modely nelze otevřít ani exportovat k sadě Visual Studio.

> [!NOTE]
> Pokud se soubor pbix modelu připojuje k místním zdrojům dat, [místní brána](analysis-services-gateway.md) musí být nakonfigurované pro váš server.

## <a name="to-import-from-pbix"></a>Chcete-li importovat z soubor pbix

1. Na vašem serveru **přehled** > **Návrhář**, klikněte na tlačítko **otevřete**.

    ![Vytvoření modelu na portálu Azure](./media/analysis-services-create-model-portal/aas-create-portal-overview-wd.png)

2. V **Návrhář** > **modely**, klikněte na tlačítko **+ přidat**.

    ![Vytvoření modelu na portálu Azure](./media/analysis-services-create-model-portal/aas-create-portal-models.png)

3. V **nový model**, zadejte název modelu a potom vyberte soubor Power BI Desktop.

    ![Dialogové okno Nový model na portálu Azure](./media/analysis-services-import-pbix/aas-import-pbix-new-model.png)

4. V **Import**, vyhledejte a vyberte soubor.

     ![Připojit dialogové okno na portálu Azure](./media/analysis-services-import-pbix/aas-import-pbix-select-file.png)

## <a name="see-also"></a>Další informace najdete v tématech

[Vytvoření modelu na portálu Azure](analysis-services-create-model-portal.md)   
[Připojení ke službě Azure Analysis Services](analysis-services-connect.md)  
