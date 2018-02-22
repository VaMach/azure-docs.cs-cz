---
title: "Vývoj aplikací pro Azure zásobníku | Microsoft Docs"
description: "Další důležité informace pro vývoj v vytváření prototypů aplikací v Azure zásobníku"
services: azure-stack
documentationcenter: 
author: brenduns
manager: femila
editor: 
ms.assetid: d3ebc6b1-0ffe-4d3e-ba4a-388239d6cdc3
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/25/2017
ms.author: brenduns
ms.reviewer: 
ms.openlocfilehash: 4b9e0f5a264296e59579539dd929f5d75a54b62a
ms.sourcegitcommit: d1f35f71e6b1cbeee79b06bfc3a7d0914ac57275
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 02/22/2018
---
# <a name="develop-for-azure-stack"></a>Vývoj pro Azure Stack
Můžete začít používat vývoj aplikací dnes, i když nebudou mít přístup k prostředí Azure zásobníku. Protože zásobník Azure poskytuje služby Microsoft Azure, které běží ve vašem datovém centru, můžete použít podobné procesy a nástroje pro vývoj proti zásobník Azure stejně jako s Azure.  V bit přípravy a pokyny z v následujících tématech můžete použít Azure emulovat prostředí Azure zásobníku:

* V Azure můžete vytvořit šablony Azure Resource Manager, které jsou také nasadit do protokolů Azure.  V tématu [aspekty šablon](azure-stack-develop-templates.md) pokyny k vývoji šablony zajistit přenositelnost.
* Existuje rozdíl v dostupnost služeb a správa verzí služeb mezi Azure a Azure zásobníku. Můžete použít [modul zásad Azure zásobníku](azure-stack-policy-module.md) omezit typy dostupnosti a prostředek služby Azure na co je k dispozici v zásobníku Azure. Omezení služby k dispozici pomůže aplikace závisí na službách, které jsou k dispozici pro Azure zásobníku.
* [Šablony rychlý start Azure zásobníku](https://github.com/Azure/AzureStack-QuickStart-Templates) jsou běžné scénáře příklady, jak vyvíjet šablony, je lze nasadit do Azure a Azure zásobníku.


