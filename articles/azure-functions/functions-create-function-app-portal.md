---
title: "Vytvoření aplikace pro funkce na portálu Azure | Microsoft Docs"
description: "Vytvořte novou aplikaci funkce ve službě Azure App Service z portálu."
services: functions
documentationcenter: na
author: ggailey777
manager: cfowler
editor: 
tags: 
ms.assetid: 
ms.service: functions
ms.devlang: multiple
ms.topic: quickstart
ms.tgt_pltfrm: multiple
ms.workload: na
ms.date: 04/11/2017
ms.author: glenga
ms.custom: mvc
ms.openlocfilehash: 4f9882ebc86b8119e6e683c7442ee3448a8d111b
ms.sourcegitcommit: e38120a5575ed35ebe7dccd4daf8d5673534626c
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/13/2017
---
# <a name="create-a-function-app-from-the-azure-portal"></a>Vytvoření funkce aplikace z portálu Azure

Aplikace Azure funkce používá infrastrukturu Azure App Service. Toto téma ukazuje, jak vytvořit aplikaci function app na portálu Azure. Funkce aplikace je kontejner, který je hostitelem provádění jednotlivých funkcí. Když vytvoříte aplikaci funkce v hostování plán služby App Service, funkce aplikace můžete používat všechny funkce služby App Service.

## <a name="create-a-function-app"></a>Vytvoření Function App

[!INCLUDE [functions-create-function-app-portal](../../includes/functions-create-function-app-portal.md)]

Když vytvoříte aplikaci function app, zadejte platný **název aplikace**, který může obsahovat pouze písmena, číslice a pomlčky. Podtržítko (**_**) není povolené znak.

Názvy účtů úložiště musí mít od 3 do 24 znaků a můžou obsahovat jenom číslice a malá písmena. Váš název účtu úložiště musí být jedinečný v rámci Azure. 

Po vytvoření aplikace funkce vytvořením jednotlivých funkcí na jeden nebo více různých jazyků. Vytvoření funkce [pomocí portálu](functions-create-first-azure-function.md#create-function), [průběžné nasazování](functions-continuous-deployment.md), nebo pomocí [odesílání spolu s FTP](https://github.com/projectkudu/kudu/wiki/Accessing-files-via-ftp).

## <a name="service-plans"></a>Plány služeb

Azure Functions nabízí dva druhy různých služeb: plánování využívání a plán služby App Service. Plánu spotřeby automaticky přiděluje výpočetní výkon, pokud kód je spuštěný, měřítka na více systémů v případě potřeby zpracování zátěže, pak měřítka in a pokud kód není spuštěna. Plán služby App Service poskytuje vaší funkce aplikace přístup k veškeré prostředky služby App Service. Když je vytvořena funkce aplikace a nelze ji změnit, musíte zvolit plán služby. Další informace najdete v tématu [zvolte Azure Functions hostování plán](functions-scale.md).

Pokud plánujete spouštět funkce jazyka JavaScript na plán služby App Service, měli byste vybrat plán s menším počtem jader. Další informace najdete v tématu [referenční dokumentace technologie JavaScript pro funkce](functions-reference-node.md#choose-single-vcpu-app-service-plans).

<a name="storage-account-requirements"></a>

## <a name="storage-account-requirements"></a>Požadavky na účet úložiště

Když vytváříte aplikaci funkce ve službě App Service, musíte vytvořit nebo odkaz na účet pro obecné účely Azure Storage, který podporuje úložiště Blob, fronty a tabulky. Funkce interně používá úložiště pro operace, jako je například Správa aktivační události a protokolování spuštěních funkce. Některé účty úložiště nepodporují fronty a tabulky, jako je například účty pouze objekt blob úložiště Azure Premium Storage a účty úložiště pro obecné účely replikaci ZRS. Tyto účty jsou filtrovány mimo v okně účtu úložiště při vytváření aplikaci funkce.

>[!NOTE]
>Při použití spotřeby hostování plán, funkce kód a vazba konfiguračních souborů jsou uložené v Azure File storage v účtu hlavní úložiště. Pokud odstraníte účet úložiště hlavní, tento obsah se odstraní a nelze jej obnovit.

Další informace o typech účtu úložiště najdete v tématu [Představení služby Azure Storage](../storage/common/storage-introduction.md#introducing-the-azure-storage-services). 

## <a name="next-steps"></a>Další kroky

[!INCLUDE [Functions quickstart next steps](../../includes/functions-quickstart-next-steps.md)]



