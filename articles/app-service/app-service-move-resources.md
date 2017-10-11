---
title: "Přesunout prostředky webové aplikace do jiné skupiny prostředků"
description: "Popisuje scénáře, kde můžete přesunout webové aplikace a aplikace služby z jedné skupiny prostředků do jiné."
services: app-service
documentationcenter: 
author: ZainRizvi
manager: erikre
editor: 
ms.assetid: 22f97607-072e-4d1f-a46f-8d500420c33c
ms.service: app-service
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/21/2016
ms.author: zarizvi
ms.openlocfilehash: 1b5059dc052005b6079f70ecf6771a3771df8d87
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="supported-move-configurations"></a>Přesunutí podporované konfigurace
Přesunutím prostředků webové aplikace Azure pomocí [Resource Manager přesunout prostředky API](../azure-resource-manager/resource-group-move-resources.md).

Webové aplikace Azure aktuálně podporuje následující scénáře přesunutí:

* Celý obsah skupiny prostředků (webové aplikace, plány služby app a certifikáty) přesunete do jiné skupiny prostředků. 
   > [!Note]
   > Cílové skupiny prostředků nesmí obsahovat žádné Microsoft.Web prostředky v tomto scénáři.

* Přesunete jednotlivé webové aplikace do jiné skupině prostředků, a současně stále je hostování v jejich aktuální plán služby app service (plán služby app service zůstává ve skupině původního prostředků).


