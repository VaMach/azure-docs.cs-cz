---
title: "Připojení ke službě Azure Analysis Services v aplikaci Excel | Microsoft Docs"
description: "Zjistěte, jak se připojit k serveru Azure Analysis Services pomocí aplikace Excel."
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
ms.openlocfilehash: d51b6980120f1cf9bc8d053d463a73ac600b915f
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="connect-with-excel"></a>Propojit s Excelem

Jakmile jste vytvořili serveru v Azure a nasadili do ní tabulkový model, budete připraveni k připojení a začít zkoumat data.


## <a name="connect-in-excel"></a>Připojení v aplikaci Excel

Připojení k serveru v aplikaci Excel pomocí příkazu Get Data v aplikaci Excel 2016 podporuje. Připojení pomocí Průvodce importem tabulky v doplňku Power Pivot není podporováno. 

**Pokud chcete v aplikaci Excel 2016 připojit**

1. V aplikaci Excel 2016, na **Data** pásu karet, klikněte na tlačítko **načíst externí Data** > **z jiných zdrojů** > **ze služby Analysis Services** .

2. V průvodci připojení dat v **název serveru**, zadejte název serveru, včetně protokolu a identifikátor URI. Potom v **přihlašovací údaje**, vyberte **použít následující uživatelské jméno a heslo**a pak zadejte název organizační uživatele, například nancy@adventureworks.coma heslo.

    ![Připojení z aplikace Excel přihlášení](./media/analysis-services-connect-excel/aas-connect-excel-logon.png)

3. V **vybrat databázi a tabulku**, vyberte databázi a modelu nebo perspektivy a pak klikněte na tlačítko **Dokončit**.
   
    ![Připojení z vyberte model aplikace Excel](./media/analysis-services-connect-excel/aas-connect-excel-select.png)


## <a name="see-also"></a>Viz také
[Knihovny klienta](analysis-services-data-providers.md)   
[Správa serveru](analysis-services-manage.md)     


