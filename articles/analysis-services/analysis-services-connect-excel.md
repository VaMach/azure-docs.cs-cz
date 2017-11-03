---
title: "Připojení ke službě Azure Analysis Services v aplikaci Excel | Microsoft Docs"
description: "Zjistěte, jak se připojit k serveru Azure Analysis Services pomocí aplikace Excel."
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
ms.date: 11/01/2017
ms.author: owend
ms.openlocfilehash: 7597792a525583497ec6a400e668eda2adc9a93e
ms.sourcegitcommit: d41d9049625a7c9fc186ef721b8df4feeb28215f
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/02/2017
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


