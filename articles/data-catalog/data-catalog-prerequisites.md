---
title: "Požadavky Azure Data Catalog | Microsoft Docs"
description: "Další informace o požadavcích, že potřebujete Začínáme s Azure Data Catalog."
services: data-catalog
documentationcenter: 
author: steelanddata
manager: NA
editor: 
tags: 
ms.assetid: ef497a54-dc4d-4820-b5bf-c361b64b964d
ms.service: data-catalog
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-catalog
ms.date: 10/01/2017
ms.author: maroche
ms.openlocfilehash: cf32ef4c80fa0ee68ce3dc1289467a419aab39c9
ms.sourcegitcommit: f8437edf5de144b40aed00af5c52a20e35d10ba1
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/03/2017
---
# <a name="azure-data-catalog-prerequisites"></a>Požadavky na Azure Data Catalog

Budete muset postará o pár věcí, než můžete nastavit Azure Data Catalog. Nemusíte si dělat starosti, není tento proces trvá dlouho.

## <a name="azure-subscription"></a>Předplatné Azure
Pokud chcete nastavit katalogu Data Catalog, musí být vlastníkem nebo spoluvlastník předplatného Azure.

Předplatná Azure, která umožňují přístup k prostředkům cloudových služeb, například Data Catalog. Odběry také umožňují řídit způsob hlášené, účtují a zaplacení využití prostředků. Každý odběr může mít samostatné fakturace a platebních instalace, tak může mít odběry a plány, které se liší podle oddělení, projektů, místní office a tak dále. Každé cloudové služby přísluší k odběru a musíte mít předplatné před instalací katalogu Data Catalog. Více informací naleznete v tématu [Správa účtů, předplatných a správních rolí](../active-directory/active-directory-assign-admin-roles.md).

## <a name="azure-active-directory"></a>Azure Active Directory
Pokud chcete nastavit katalogu Data Catalog, musí být podepsané pomocí uživatelského účtu Azure Active Directory (Azure AD).

Azure AD umožní vaší firmě snadnou správu identity a přístupu, a to jak v cloudu, tak i místně. Uživatele můžete použít pracovní nebo školní účet pro jednom přihlášení na všechny cloudové a místní webové aplikace. Data Catalog používá Azure AD k ověřování přihlášení. Další informace najdete v tématu [co je Azure Active Directory?](../active-directory/active-directory-whatis.md).

> [!NOTE]
> Pomocí [portál Azure](http://portal.azure.com/), se můžete přihlásit se pomocí osobního účtu Microsoft nebo Azure Active Directory pracovní nebo školní účet. Nastavit Data Catalog pomocí portálu Azure nebo [katalogu Data Catalog portál](http://www.azuredatacatalog.com), musí se přihlásit účtu Azure Active Directory, nikoli osobní účet.
>
>

## <a name="active-directory-policy-configuration"></a>Zásady Konfigurace služby Active Directory
Může nastat situace, kde se můžete přihlásit k portálu katalogu Data Catalog, ale při pokusu o přihlášení k nástroj registrace zdroje dat, dojde k chybovou zprávu, která brání přihlášení. Toto chování problému může dojít pouze v případě, že jste na podnikové síti, nebo ho může dojít pouze v případě, že se připojujete z vnějšku firemní sítě.

Nástroj registrace zdroje dat používá ověřování založené na formulářích k ověření pověření uživatele pro službu Active Directory. Můžete úspěšně přihlásit, musí správce služby Active Directory povolit ověřování pomocí formulářů v globální zásady ověřování.

V globální zásady ověřování metody ověřování se dá nastavit samostatně pro intranetové a připojení k síti extranet, jak je znázorněno na následujícím snímku obrazovky. Pokud je ověřování pomocí formulářů není povoleno pro síť, ze kterého se připojujete, může dojít k chybám přihlášení.

 ![Služby Active Directory globální zásady ověřování](./media/data-catalog-prerequisites/global-auth-policy.png)

## <a name="next-steps"></a>Další kroky
Další informace najdete v tématu [konfigurace zásad ověřování](https://technet.microsoft.com/library/dn486781.aspx).
