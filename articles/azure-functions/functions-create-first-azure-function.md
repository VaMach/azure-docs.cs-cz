---
title: "Vytvoření první funkce Azure | Dokumentace Microsoftu"
description: "Vytvořte svoji první funkci Azure, aplikaci bez serveru, za méně než dvě minuty."
services: functions
documentationcenter: na
author: ggailey777
manager: erikre
editor: 
tags: 
ms.assetid: 4a1669e7-233e-4ea2-9b83-b8624f2dbe59
ms.service: functions
ms.devlang: multiple
ms.topic: hero-article
ms.tgt_pltfrm: multiple
ms.workload: na
ms.date: 03/14/2016
ms.author: glenga
translationtype: Human Translation
ms.sourcegitcommit: afe143848fae473d08dd33a3df4ab4ed92b731fa
ms.openlocfilehash: a797910e286cd2aacf5a8aa6c509e2b0f5f60276
ms.lasthandoff: 03/17/2017


---
# <a name="create-your-first-azure-function"></a>Vytvoření první funkce Azure

Toto téma vám ukáže, jak pomocí rychlého startu Azure Functions na portálu vytvořit jednoduchou funkci „hello world“, která se vyvolá požadavkem HTTP. Další informace o Azure Functions najdete v tématu [Přehled Azure Functions](functions-overview.md).

Než začnete, musíte mít účet Azure. K dispozici jsou [Bezplatné účty](https://azure.microsoft.com/free/). Můžete také [vyzkoušet Azure Functions](https://azure.microsoft.com/try/app-service/functions/) bez nutnosti registrace do Azure.

## <a name="create-a-function-from-the-portal-quickstart"></a>Rychlý úvod k vytvoření funkce z portálu

1. Přejděte na [portál Azure Functions](https://functions.azure.com/signin) a přihlaste se pomocí účtu Azure. 
 
2. Zadejte jedinečný **název** nové aplikace funkce nebo přijměte název vygenerovaný automaticky, vyberte upřednostňovanou **Oblast** a klikněte na **Vytvořit a začít**. Platný název může obsahovat pouze písmena, číslice a pomlčky. Podtržítko (**_**) není povolené znak.

3. Na kartě **Rychlý start** klikněte na **WebHook + API**, zvolte pro funkci jazyk a potom klikněte na **Vytvořit funkci**. Nová předdefinovaná funkce se vytvoří ve zvoleném jazyce. 
   
    ![](./media/functions-create-first-azure-function/function-app-quickstart-node-webhook.png)

4. (Volitelné) V rychlém startu teď můžete absolvovat krátkou prohlídku funkcí Azure Functions na portálu. Až prohlídku dokončíte nebo přeskočíte, můžete novou funkci otestovat odesláním požadavku HTTP.

## <a name="test-the-function"></a>Testování funkce
[!INCLUDE [Functions quickstart test](../../includes/functions-quickstart-test.md)]

## <a name="watch-the-video"></a>Přehrát video
Následující video ukazuje, jak provést základní kroky obsažené v tomto kurzu. 

> [!VIDEO https://channel9.msdn.com/Series/Windows-Azure-Web-Sites-Tutorials/Create-your-first-Azure-Function-simple/player]
> 


## <a name="next-steps"></a>Další kroky
[!INCLUDE [Functions quickstart next steps](../../includes/functions-quickstart-next-steps.md)]

[!INCLUDE [Getting Started Note](../../includes/functions-get-help.md)]


