---
title: "Řešení potíží s Azure RBAC | Microsoft Docs"
description: "Získáte pomoc s problémy nebo dotazy týkající se řízení přístupu na základě Role prostředky."
services: azure-portal
documentationcenter: na
author: andredm7
manager: femila
ms.assetid: df42cca2-02d6-4f3c-9d56-260e1eb7dc44
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/12/2017
ms.author: andredm
ms.reviewer: rqureshi
ms.openlocfilehash: 407c030ea159915d4d7ac21760a3d17ec2204372
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/11/2017
---
# <a name="role-based-access-control-troubleshooting"></a>Na základě rolí řešení potíží s řízení přístupu

Tento dokument článku naleznete odpovědi na časté otázky týkající se konkrétní přístupová práva, kterým je uděleno oprávnění s rolemi, abyste věděli, co očekávat při použití role v portálu Azure a může poradce při potížích přístup. Tyto tři role popisuje všechny typy prostředků:

* Vlastník  
* Přispěvatel  
* Čtenář  

Vlastníci a přispěvatelé mají plný přístup k prostředí pro správu, ale Přispěvatel nelze poskytnout přístup k jiné uživatele nebo skupiny. Věcí získat něco zajímavějšího k roli čtečky tak, aby se, kde budete Věnujte nějaký čas. Najdete v článku [řízení přístupu na základě Role get-started článku](role-based-access-control-configure.md) podrobnosti o tom, jak udělit přístup.

## <a name="app-service-workloads"></a>Úlohy služby aplikace
### <a name="write-access-capabilities"></a>Možnosti přístup pro zápis
Když udělíte přístup jen pro čtení uživatelů do jedné webové aplikace, některé funkce jsou vypnuté, nemusí očekáváte. Následující funkce správy vyžadují **zápisu** přístup do webové aplikace (Přispěvatel nebo vlastníka) a nejsou k dispozici v žádném scénáři jen pro čtení.

* Příkazy (například spuštění, zastavení, atd.)
* Změna nastavení, jako je obecná konfigurace, nastavení škálování, nastavení zálohování a nastavení monitorování
* Přístup k pověření k publikování a jiné tajné klíče, jako je nastavení aplikace a připojovacích řetězců
* Protokoly streamování
* Konfigurace diagnostických protokolů
* Konzole (příkazového řádku)
* Aktivní a poslední nasazení (pro místní git průběžné nasazování)
* Odhadované tráví
* Testy webu
* Virtuální síť (viditelné pouze pro čtečku, pokud uživatel s přístup pro zápis byl dříve nakonfigurován virtuální sítě).

Pokud nemůžete použít žádnou z těchto dlaždicích, budete muset požádat správce pro přispěvatele přístup do webové aplikace.

### <a name="dealing-with-related-resources"></a>Práci s související informační zdroje
Webové aplikace jsou ztěžuje přítomnost několik různých prostředků, které vztahu. Tady je skupina prostředků typické se několik webů:

![Skupina prostředků webové aplikace](./media/role-based-access-control-troubleshooting/website-resource-model.png)

V důsledku toho, pokud někdo udělíte přístup do právě webové aplikace, mnoho funkcí v okně Web na portálu Azure je zakázán.

Tyto položky vyžadují **zápisu** přístup k **plán služby App Service** odpovídající na váš web:  

* Zobrazení webové aplikace je cenová úroveň (volné nebo Standard)  
* Škálování konfigurace (počet instancí, velikost virtuálního počítače, nastavení automatického škálování)  
* Kvóty (úložiště, šířku pásma, procesoru)  

Tyto položky vyžadují **zápisu** přístup k celé **skupiny prostředků** svůj web, který obsahuje:  

* Certifikáty SSL a vazeb (certifikáty SSL lze sdílet mezi lokalitami ve stejné skupině prostředků a geografického umístění)  
* Pravidla výstrah  
* nastavení automatického škálování  
* Přehled součásti aplikace  
* Testy webu  

## <a name="virtual-machine-workloads"></a>Úlohy virtuálních počítačů
Podobně jako s webovými aplikacemi, některé funkce v okně virtuálního počítače vyžadují přístup pro zápis k virtuálnímu počítači nebo k jiným prostředkům ve skupině prostředků.

Virtuální počítače jsou související s názvy domény, virtuálních sítí, účty úložiště a pravidla výstrah.

Tyto položky vyžadují **zápisu** přístup k **virtuální počítač**:

* Koncové body  
* IP adresy  
* Disky  
* Rozšíření  

Vyžadují **zápisu** přístup do obou **virtuálního počítače**a **skupiny prostředků** (spolu s názvem domény) je to v:  

* Skupina dostupnosti  
* Skupině s vyrovnáváním zatížení  
* Pravidla výstrah  

Pokud nemůžete použít žádnou z těchto dlaždicích, požádejte správce pro přispěvatele přístup ke skupině prostředků.

## <a name="see-more"></a>Přečíst si k tomu víc
* [Řízení přístupu na základě role](role-based-access-control-configure.md): Začínáme s RBAC na portálu Azure.
* [Předdefinované role](role-based-access-built-in-roles.md): získat tak podrobné údaje o rolích, které jsou v RBAC standardní.
* [Vlastní role v Azure RBAC](role-based-access-control-custom-roles.md): Naučte se vytvářet vlastní role podle vašich potřeb přístup.
* [Vytvoření sestavy historie změn přístupu](role-based-access-control-access-change-history-report.md): udržování přehledu o změně přiřazení rolí v RBAC.

