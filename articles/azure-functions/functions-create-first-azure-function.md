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
ms.date: 02/09/2016
ms.author: glenga
translationtype: Human Translation
ms.sourcegitcommit: 7743bfb98552f7fa2334d8daa6a6f6935969f393
ms.openlocfilehash: d76630e0be4a021720d88e6d7e64cf2258f84266


---
# <a name="create-your-first-azure-function"></a>Vytvoření první funkce Azure
## <a name="overview"></a>Přehled
Azure Functions je výpočetní prostředí na vyžádání založené na událostech. Rozšiřuje stávající platformu aplikací Azure o funkce pro implementaci kódu aktivovaného událostmi, ke kterým dochází v jiných službách Azure, produktech SaaS a místních systémech. Díky Azure Functions se aplikace škálují na vyžádání a vy platíte jenom za využité prostředky. Azure Functions umožňuje vytvářet naplánované nebo aktivované jednotky kódu implementované v různých programovacích jazycích. Další informace o Azure Functions najdete v tématu [Přehled Azure Functions](functions-overview.md).

Toto téma vám ukáže, jak pomocí rychlého startu Azure Functions na portálu vytvořit jednoduchou funkci „hello world“ v JavaScriptu, která se vyvolá triggerem protokolu HTTP. Můžete také zhlédnout krátké video a podívat se, jak se tyto kroky na portálu provádějí.

## <a name="watch-the-video"></a>Přehrát video
Následující video ukazuje, jak provést základní kroky obsažené v tomto kurzu. 

> [!VIDEO https://channel9.msdn.com/Series/Windows-Azure-Web-Sites-Tutorials/Create-your-first-Azure-Function-simple/player]
> 
> 

## <a name="create-a-function-from-the-quickstart"></a>Vytvoření funkce z rychlého startu
Provádění funkcí v Azure je hostováno v aplikaci funkce. Podle těchto kroků můžete vytvořit aplikaci Function App s novou funkcí. Aplikace Function App se vytvoří s výchozí konfigurací. Příklad explicitního vytvoření aplikace Function App najdete ve [stručném úvodním kurzu k Azure Functions](functions-create-first-azure-function-azure-portal.md).

Je nutné, abyste před vytvořením první funkce měli aktivní účet Azure. Pokud ještě nemáte účet Azure, [můžete použít bezplatné účty](https://azure.microsoft.com/free/).

1. Přejděte na stránku [portálu Azure Functions](https://functions.azure.com/signin) a přihlaste se pomocí účtu Azure.
2. Zadejte jedinečný **název** nové aplikace funkce nebo přijměte název vygenerovaný automaticky, vyberte upřednostňovanou **Oblast** a klikněte na **Vytvořit a začít**. Všimněte si, že musíte zadat platný název, který může obsahovat pouze písmena, číslice a pomlčky. Podtržítko (**_**) není povolené znak.
3. Na kartě **Rychlý start** klikněte na **WebHook + API** a **JavaScript** a potom klikněte na **Vytvořit funkci**. Vytvoří se nová předdefinovaná funkce v JavaScriptu. 
   
    ![](./media/functions-create-first-azure-function/function-app-quickstart-node-webhook.png)
4. (Volitelné) V rychlém startu teď můžete absolvovat krátkou prohlídku funkcí Azure Functions na portálu. Až prohlídku dokončíte nebo přeskočíte, můžete novou funkci otestovat pomocí triggeru protokolu HTTP.

## <a name="test-the-function"></a>Testování funkce
[!INCLUDE [Functions quickstart test](../../includes/functions-quickstart-test.md)]

## <a name="next-steps"></a>Další kroky
[!INCLUDE [Functions quickstart next steps](../../includes/functions-quickstart-next-steps.md)]

[!INCLUDE [Getting Started Note](../../includes/functions-get-help.md)]




<!--HONumber=Feb17_HO2-->


