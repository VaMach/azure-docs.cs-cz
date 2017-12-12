---
title: "Spravovat přístup k prostředkům Azure s Privileged Identity Management (PIM)"
description: "Další informace o použití správy přístupu na základě rolí v PIM pro přístup k prostředkům Azure."
services: active-directory
documentationcenter: 
author: skwan
manager: mtillman
editor: bryanla
ms.assetid: ba06b8dd-4a74-4bda-87c7-8a8583e6fd14
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 09/22/2017
ms.author: skwan
ms.openlocfilehash: 1f31d8b76351ac8871f8a5b03d513f7b6704c709
ms.sourcegitcommit: e266df9f97d04acfc4a843770fadfd8edf4fa2b7
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 12/11/2017
---
# <a name="manage-access-to-azure-resources-with-privileged-identity-management-preview"></a>Spravovat přístup k prostředkům Azure s Privileged Identity managementu (Preview)

K ochraně před internetovými útoky privilegované účty, můžete použít Azure Active Directory Privileged Identity Management (PIM) Pokud chcete snížit čas ohrožení oprávnění a zvýšit vaši přehled o jejich používání prostřednictvím sestavy a výstrahy. PIM tomu omezením uživatelům na jejich oprávnění pouze trvá "právě v čase" (JIT), nebo přiřazením oprávnění zkrácení doby trvání, po které jsou oprávnění automaticky odvolat. 

Nyní můžete PIM s řízení řízení přístupu (RBAC) pro správu, řízení a monitorování přístupu k prostředkům Azure. PIM můžete spravovat členství předdefinované a vlastní role můžete: 

- Povolit "právě v čase" přístup k prostředkům Azure na vyžádání
- Platnost přístupu k prostředkům automaticky pro přiřazené uživatele a skupiny
- Přiřadit dočasný přístup k prostředkům Azure pro Rychlé úlohy nebo plány na volání
- Zasílání upozornění, když nové uživatele nebo skupiny jsou přiřazeny přístup k prostředkům a aktivují oprávněné přiřazení

Další informace najdete v tématu [Overview of Role-Based řízení přístupu v Azure PIM](privileged-identity-management/azure-pim-resource-rbac.md).