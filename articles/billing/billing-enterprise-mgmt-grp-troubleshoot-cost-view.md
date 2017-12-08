---
title: "Řešení potíží s enterprise náklady zobrazení - Azure | Microsoft Docs"
description: "Přečtěte si o řešení problémů, které můžete mít s náklady na organizační zobrazení v portálu Azure."
author: rthorn17
manager: rithorn
editor: 
ms.assetid: 
ms.service: billing
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 09/25/2017
ms.author: rithorn
ms.openlocfilehash: eca1ac9ed51e6c2243be451a074792fbec2840d2
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/11/2017
---
# <a name="troubleshoot-enterprise-cost-views"></a>Řešení potíží s enterprise náklady zobrazení 

V rámci podnikové registrace existuje několik nastavení, které by mohly způsobit uživatelům v rámci registrace není možné zobrazit náklady.  Tato nastavení jsou spravovány správcem registrace nebo partner poskytující prostředky, pokud registrace není zakoupili přímo se společností Microsoft.  Tento článek vám pomůže pochopit je nastavení a jak budou mít vliv na registraci. Tato nastavení jsou nezávislé [role RBAC Azure](https://docs.microsoft.com/en-us/azure/active-directory/role-based-access-control-configure). 

> [!Note]
> Tato funkce je aktuálně v soukromém náhledu. [Zaregistrujte si zde](https://forms.office.com/Pages/DesignPage.aspx#FormId=v4j5cvGGr0GRqy180BHbR0YtfU6ham9OsGsPPYdu2xdUNk1BQUwzTkUyOVc5NUpCTFcwR0pIOVFETS4u) tak, aby měl registraci připojení ve verzi preview.     

## <a name="enabling-access-to-costs"></a>Povolení přístupu k náklady

Jsou vidíte zprávu Neautorizováno, nebo *"zobrazení náklady jsou zakázaná v registraci."* Pokud hledáte informace o nákladech? ![neoprávněným](media/billing-enterprise-mgmt-groups/unauthorized.png)

Může to být způsobeno jedním z následujících důvodů:

1. Azure zakoupené prostřednictvím partnerem enterprise a partnerovi nevydala ještě ceny. Chcete-li uvolnit, ceny, získáte od svého partnera, se aktualizovat nastavení v rámci [podnikový portál](https://ea.azure.com).
2. Případně pokud jste EA přímé zákazníka, existují dvě možnosti:
    * Jsou vlastníka účtu a správce registrace zakázal "AO zobrazení poplatků" nastavení.  
    * Oddělení správce a správce registrace zakázal "DA zobrazení poplatků" nastavení.
    * Obraťte se na správce registrace získat přístup. Správce registrace najdete [podnikový portál](https://ea.azure.com/manage/enrollment) a aktualizujte nastavení, jak je vidět tady:

![Nastavení portálu Enterprise](media/billing-enterprise-mgmt-groups/ea-portal-settings.png)


## <a name="asset-is-unavailable"></a>Prostředek není k dispozici? 
Pokud se vám zobrazí zpráva "Tento prostředek je k dispozici" při pokusu o přístup k odběru nebo správu skupinu a potom nemají správnou roli zobrazíte tuto položku.  

![prostředek není nalezena](media/billing-enterprise-mgmt-groups/asset-not-found.png)

Obraťte se na Spravovat předplatné nebo správu skupin má být poskytnut přístup.  
* Odběry, odkazovat [řízení řízení přístupu (RBAC)](https://docs.microsoft.com/en-us/azure/active-directory/role-based-access-control-configure) dokumentu nápovědy, na kterém je vyžadována role.
* Pro skupiny pro správu přístupu RBAC není k dispozici a bude brzo. Obraťte se na podnikový portál spravovat tak, aby měl přístup přiřazen.   
