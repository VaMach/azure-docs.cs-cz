---
title: "Uspořádání prostředků s skupin pro správu Azure - Azure | Microsoft Docs"
description: "Další informace o skupin pro správu a jejich použití."
author: rthorn17
manager: rithorn
editor: 
ms.assetid: 482191ac-147e-4eb6-9655-c40c13846672
ms.service: billing
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 09/25/2017
ms.author: rithorn
ms.openlocfilehash: 18541c68b02ae1b59ae4a6a85122dff614c9978c
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/11/2017
---
# <a name="organize-your-resources-with-azure-management-groups"></a>Uspořádání prostředků s skupin pro správu Azure 

Pokud máte více předplatných, můžete je uspořádat do kontejnerů názvem "skupin pro správu" vám pomůžou spravovat přístup, zásady, náklady a dodržování předpisů ve vašich předplatných. Například můžete použít zásady pro skupinu pro správu tento limit, které typy prostředků lze vytvořit.

> [!Note]
> Tato funkce je aktuálně v soukromém náhledu. [Zaregistrujte si zde](https://aka.ms/MGPreviewSignup) tak, aby měl registraci připojení ve verzi preview.   
 


Skupiny pro správu mohou být uspořádány do hierarchie. Struktura zobrazená je ukázka reprezentace hierarchie skupiny správy, který může existovat:


![Stromu hierarchie](media/billing-enterprise-mgmt-groups/tree.png)



## <a name="how-management-groups-are-related-to-your-azure-enterprise-enrollment"></a>Jak skupin pro správu se vztahují k registraci Azure Enterprise

Uvedení skupiny pro správu je krok v systému přechod větší cesty [podnikový portál](https://ea.azure.com) funkce k [portál Azure](https://portal.azure.com).

Struktura skupiny správy je vytvořen jako byla definovaná v podnikovém portálu. Celou hierarchii registrace, oddělení, a účty se mapují na odpovídající skupiny pro správu. 

Zde je, jak aktuální struktura EA mapuje hierarchie skupiny správy. 

![Stromu hierarchie](media/billing-enterprise-mgmt-groups/tree2.png)

Následující tabulka ukazuje, jak jsou uživatelé z podnikového portálu mapované na hierarchie skupiny správy.

|    EA role                                       |    Role na uzlu skupiny namapované správy    |    Oprávnění na uzlu skupiny správy                                                          |
|--------------------------------------------------|--------------------------------------------------|----------------------------------------------------------------------------------------------------|
|    EA správce                              |    Přispěvatel zásad prostředků                   |    Můžete zobrazit náklady, Správa prostředků zásady a zobrazení hierarchie v a pod uzlu registrace    |
|    Správce EA v režimu jen pro čtení            |    Čtečka fakturace                                |    Může číst náklady a zobrazení hierarchie v a pod uzlu registrace                              |
|    Správce oddělení                      |    Čtečka fakturace                                |    Může číst náklady a zobrazení hierarchie a pod uzlu oddělení                                 |
|    Správce oddělení v režimu jen pro čtení    |    Čtečka fakturace                                |    Může číst náklady a zobrazení hierarchie a pod uzlu oddělení                                 |
|    Vlastníka účtu                                 |    Přispěvatel zásad prostředků                   |    Můžete zobrazit náklady, Správa prostředků zásady a zobrazení hierarchie v a pod uzel účtu       |




## <a name="view-management-groups-in-the-azure-portal"></a>Zobrazení skupiny pro správu na portálu Azure

Pokud chcete zobrazit zápisu, oddělení nebo účet v rámci verze preview, přihlaste se k portálu Azure s v Uvítacího e-mailu na odkaz.   

![Hierarchie](media/billing-enterprise-mgmt-groups/hierarchy.png)

### <a name="viewing-costs"></a>Zobrazení náklady 
Na podrobnosti obrazovky skupin pro správu najdete v části náklady na aktuální měsíc k datu. Tyto náklady jsou založená na využití a nespadá zálohách, poplatky za použití, zahrnuté množství, úpravy a daně. Pro skupinu pro správu odpovídající registraci náklady na části se dozvíte závazků zbývající.  

![Podrobnosti registrace](media/billing-enterprise-mgmt-groups/enrollment.png)

 "Náklady fakturuje samostatně" jsou měsíční nahromadění samostatné změny jako marketplace, poplatky za použití a dalšími náklady, které se nepřenášejí proti vaší registrace závazků.  Další informace o rozdělení nákladů, najdete v článku [podnikový portál](https://ea.azure.com). 

### <a name="enabling-access-to-costs"></a>Povolení přístupu k náklady
Pokud nevidíte náklady, přečtěte si naše [Poradce při potížích enterprise náklady zobrazení](https://aka.ms/enableazurecosts) dokumentu o pomoc.  

### <a name="delays-between-the-enterprise-portal-and-azure-portal"></a>Zpoždění mezi podnikovém portálu a portálu Azure 
* Ve verzi Preview může být zpoždění objemy zobrazit v rámci portálu Azure ve srovnání se hodnoty v podnikovém portálu. Tento problém je dočasný a je pracuje.
* Aktualizované nastavení v podnikovém portálu mají zpoždění několik minut, než se aktualizace se projeví na portálu Azure. 

## <a name="management-groups-have-a-relationship-with-your-directory"></a>Skupiny pro správu mít vztah s adresářem   
Odběry, jako jsou skupiny pro správu také mít vztah důvěryhodnosti s Azure AD. Hierarchie skupin správy důvěřuje jeden adresář k ověřování uživatelů. Všichni správci přidružené hierarchie skupin správy musí patřit do stejného adresáře. 

Jako hierarchii registrace je namapovaný na skupiny pro správu, je vytvořit důvěryhodný vztah s jeden adresář. Pokud je to možné, je vybrána existující adresář spojené s uživatelskými účty zápisu. V některých případech se vytvoří nový adresář a všechny existující registrace uživatelé mohou do tohoto adresáře. Ovlivněné nejsou adresáře spojené s odběry registraci. Proto může v adresáři, liší od předplatná vytvářeny v hierarchii. [Další informace](billing-enterprise-mgmt-grp-find.md) o tom, jak tento proces ovlivňuje možností navigace mezi hierarchii a její odběry.

## <a name="administering-your-management-groups"></a>Správa skupin pro správu
Skupiny pro správu v rámci portálu Azure jsou ve verzi preview a jsou v této verzi počáteční jen pro čtení. Chcete-li všechny aktualizace, přejděte na podnikovém portálu. Vaše aktualizace se projeví na portálu Azure automaticky. Najdete v dokumentaci v rámci portálu enterprise nápovědu pro provádění úprav a dodatky.   

## <a name="policy-management"></a>Správa zásad
Resource Manager umožňuje vytvářet vlastní zásady pro správu prostředků. Zásady skupiny pro správu mohou být přiřazeny žádné úrovni v hierarchii a prostředky dědí těchto zásad.  [Další informace](https://go.microsoft.com/fwlink/?linkid=858942)

> [!Note]
> Zásady nevynucuje ve adresářích. 


