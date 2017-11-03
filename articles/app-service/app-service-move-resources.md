---
title: "Přesunout do jiné skupiny prostředků webové aplikace Azure | Microsoft Docs"
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
ms.date: 10/19/2017
ms.author: zarizvi
ms.openlocfilehash: 4d5d6540dd0b51a4f172649a1fd9a2343efeceee
ms.sourcegitcommit: 963e0a2171c32903617d883bb1130c7c9189d730
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/20/2017
---
# <a name="move-an-azure-web-app-to-another-resource-group"></a>Přesunutí webové aplikace Azure do jiné skupiny prostředků
Webové aplikace a její související prostředky můžete přesunout do jiné skupiny prostředků v rámci stejného předplatného nebo do skupiny prostředků v jiném předplatném. To se provádí v rámci správy standardní prostředků v Azure. Další informace najdete v tématu [prostředky Azure přesunout do nové skupiny předplatné nebo prostředek](../azure-resource-manager/resource-group-move-resources.md).

## <a name="limitations-when-moving-within-the-same-subscription"></a>Omezení při přesunu v rámci stejného předplatného.

Při přesunu webové aplikace _v rámci stejného předplatného_, nemůžete přesunout nahrané certifikáty SSL. Ale webové aplikace do nové skupiny prostředků můžete přesunout bez přesouvání jeho odeslaný certifikát SSL a funkce SSL vaší aplikace stále funguje. 

Pokud chcete přesunout certifikát SSL s webovou aplikaci, postupujte podle těchto kroků:

1.  Odstraníte se nahraný certifikát z webové aplikace.
2.  Přesuňte webové aplikace.
3.  Nahrajte certifikát do přesunutý webové aplikace.

## <a name="limitations-when-moving-across-subscriptions"></a>Omezení při přesouvání mezi odběrů

Při přesunu webové aplikace _ve předplatných_, platí následující omezení:

- Cílové skupiny prostředků nesmí mít žádné existující prostředky služby App Service. Služby App Service prostředky zahrnují:
    - Web Apps
    - Plány služby App Service
    - Nahrané nebo importované certifikáty SSL
    - Prostředí App Service
- Všechny prostředky služby App Service ve skupině prostředků je třeba přesunout společně.
- Prostředky aplikace služby lze přesunout pouze ze skupiny prostředků, ve které byly původně vytvořili. Pokud prostředek služby App Service je již v jeho původní skupin prostředků, je nutné je přesunout zpět do této původní skupiny prostředků nejprve a pak ho lze přesunout mezi odběry. 
