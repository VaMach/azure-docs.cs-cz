---
title: "Odstranit pracovní prostor služby Azure Log Analytics | Microsoft Docs"
description: "Zjistěte, jak odstranit pracovní prostor analýzy protokolů, pokud jste vytvořili v odběru osobní nebo změny struktury modelu pracovního prostoru."
services: log-analytics
documentationcenter: log-analytics
author: mgoedtel
manager: carmonm
editor: 
ms.assetid: 
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/27/2017
ms.author: magoedte
ms.custom: mvc
ms.openlocfilehash: 4f455e26f078360f17f8118f8b5b1db5bd75d473
ms.sourcegitcommit: d87b039e13a5f8df1ee9d82a727e6bc04715c341
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 02/21/2018
---
# <a name="delete-an-azure-log-analytics-workspace-with-the-azure-portal"></a>Odstranit pracovní prostor služby Azure Log Analytics pomocí portálu Azure
Toto téma ukazuje, jak pomocí portálu Azure můžete odstranit workpace analýzy protokolů, které už můžou vyžadovat. 

## <a name="to-delete-a-workspace"></a>Odstranění pracovního prostoru 
Při odstranění pracovní prostor analýzy protokolů všech dat spojených s pracovního prostoru je odstranit ze služby do 30 dní.  Chcete postupujte opatrně při odstranění pracovního prostoru, protože může být důležitá data a konfiguraci, která může mít negativní vliv na vaše operace služby.  
 
Pokud jste správce a k pracovnímu prostoru bylo přidruženo víc uživatelů, přidružení těchto uživatelů s pracovním prostorem se přeruší. Pokud jsou uživatelé přidružené jiných pracovních prostorech, potom můžou pokračovat pomocí těchto jiných pracovních prostorech analýzy protokolů. Ale pokud nejsou přidruženy jiných pracovních prostorech pak potřebují k vytvoření pracovního prostoru analýzy protokolů použít. 

1. Přihlaste se k webu [Azure Portal](http://portal.azure.com). 
2. Na portálu Azure klikněte na tlačítko **všechny služby**. V seznamu prostředků zadejte **Log Analytics**. Seznam se průběžně filtruje podle zadávaného textu. Vyberte **Log Analytics**.
3. V podokně analýzy protokolů předplatných, vyberte pracovní prostor a pak klikněte na tlačítko **odstranit** z horní části podokna uprostřed.<br><br> ![Odstranit možnost z podokna Vlastnosti pracovního prostoru](media/log-analytics-manage-del-workspace/log-analytics-delete-workspace.png)<br>  
4. Když se zobrazí okno potvrzovací zpráva s výzvou k potvrzení odstranění pracovního prostoru, klikněte na tlačítko **Ano**.<br><br> ![Potvrdit odstranění pracovního prostoru](media/log-analytics-manage-del-workspace/log-analytics-delete-workspace-confirm.png)

