---
title: "Přehled Azure zásad | Microsoft Docs"
description: "Azure zásad je služba v Azure, který použijete k vytvoření, přiřazení a spravovat zásady definice v prostředí Azure."
services: azure-policy
keywords: 
author: Jim-Parker
ms.author: jimpark; nini
ms.date: 10/06/2017
ms.topic: overview
ms.service: azure-policy
manager: jochan
ms.custom: mvc
ms.openlocfilehash: 01b0915cdf37ddc00a4e6a38c99ce7f6f8c4f9c9
ms.sourcegitcommit: 51ea178c8205726e8772f8c6f53637b0d43259c6
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/11/2017
---
# <a name="what-is-azure-policy"></a>Co je Azure zásad?

Zásady správného řízení IT vytvoří přehlednost mezi obchodních cílů a IT projekty. Dobrý řízení IT zahrnuje vaše iniciativami v oblasti plánování a nastavování priorit na strategické úrovni. Pokud vaše společnost vyskytne velký počet problémů IT, které nikdy se zdá, že jsou vyřešeny, implementace zásad vám pomůže lépe spravovat a zabránit jejich. Toto je, kde zásad Azure dodává.

Azure zásad je služba v Azure, který použijete k vytvoření, přiřazení a spravovat definice zásady. Definice zásady vynutit různá pravidla a akce přes vaše prostředky, takže tyto prostředky zůstat kompatibilní s firemními standardy a smlouvy o úrovni služeb. Dělá to tak, že spustíte vyhodnocení vaše prostředky, ke kontrole pro které ty, které nejsou kompatibilní s definice zásady, které máte na místě.

## <a name="policy-definition"></a>Definice zásad

Každý definice zásady má podmínky, za které se nevynutí a doprovodné akci, která přebírá umístit, pokud jsou splněny podmínky.

V zásadách Azure nabízíme některé předdefinované zásady, které jsou k dispozici ve výchozím nastavení. Například:

- **Vyžadovat SQL serveru 12.0**: tuto definici zásady má podmínky nebo pravidla zajistit, že všechny servery SQL Server používat verzi 12.0. Jeho akce je tak, aby odepřel všechny servery, které tato kritéria nesplňují.
- **Povolená SKU účet úložiště**: Tato zásada definice obsahuje sadu podmínky nebo pravidla, která určí, zda účet úložiště, který se nasazuje v rámci sady velikostí SKU. Jeho akce je tak, aby odepřel všechny servery, které neodpovídají sadu definované velikosti SKU.
- **Typ prostředku povoleno**: tuto definici zásady obsahuje sadu podmínek/pravidel můžete určit typy prostředků, které můžete nasadit vaší organizace. Jeho akce je tak, aby odepřel všechny prostředky, které nejsou součástí tohoto seznamu definované.

Další informace o struktury definice zásady, podívejte se na tento článek - [struktury definice zásady](../azure-resource-manager/resource-manager-policy.md#policy-definition-structure).

## <a name="policy-assignment"></a>Přiřazení zásad
Přiřazení zásady je definici zásady, který byl přiřazen proběhla v rámci konkrétní obor. Tento obor může být v rozsahu od skupiny pro správu do skupiny prostředků.

Další informace o nastavení zásad definice a přiřazení najdete v tématu [přehled zásad prostředků](../azure-resource-manager/resource-manager-policy.md).

## <a name="policy-parameters"></a>Parametry zásad
Zásady parametry zjednodušit vaší zásady správy snížením počtu definice zásady, které je nutné vytvořit. Při vytváření definice zásad Chcete-li více Obecné můžete definovat parametry. Pak můžete znovu použít, aby definice zásady pro různé scénáře předávání různé hodnoty (například určení jednu sadu umístění pro předplatné) při přiřazování zásady.

Parametry jsou definované nebo vytvořit při vytváření definice zásad. Pokud je parametr definované, je zadaný název a volitelně zadána hodnota. Například může definujte parametr pro zásadu jako umístění a pak poskytněte různé hodnoty, jako *EastUS* nebo *WestUS* při přiřazování zásady.

Další informace o parametrech zásad najdete v tématu [přehled zásad prostředků - parametry](../azure-resource-manager/resource-manager-policy.md#parameters).

## <a name="initiative-definition"></a>Iniciativy definice
Iniciativy definice je kolekce definice zásady, které jsou přizpůsobené k dosahování singulární zastřešující cíle. Můžete například vytvořit initiative s názvem **povolit monitorování v Azure Security Center**, s cílem monitorování všechna doporučení zabezpečení k dispozici ve vaší službě Azure Security Center.

V rámci této iniciativa byste měli definice zásady, jako:

1. **Monitorování bez šifrování databáze SQL ve službě Security Center** – pro monitorování nezašifrované databází SQL a servery.
2. **Monitorování chyb zabezpečení operačního systému ve službě Security Center** – sledování serverů, které nesplňují nakonfigurované směrného plánu.
3. **Sledování chybějící služby Endpoint Protection v Centru zabezpečení** – sledování serverů bez agenta ochrany nainstalovaného koncový bod.

## <a name="initiative-assignment"></a>Iniciativy přiřazení
Podobně jako přiřazení zásady je iniciativy přiřazení iniciativy definice, které jsou přiřazeny k určité oboru. Iniciativy přiřazení snížit potřeba provést několik iniciativy definice pro každý obor. Tento obor může také rozsahu ze skupiny pro správu do skupiny prostředků.

Z předchozího příkladu **povolit monitorování v Azure Security Center** initiative lze přiřadit k různými obory. Například jeden přiřazení lze přiřadit k **subscriptionA**, zatímco jiné lze přiřadit k **subscriptionB**.

## <a name="initiative-parameters"></a>Iniciativy parametry
Jako parametry zásad iniciativy parametry zjednodušit správu iniciativy snížením redundance. Iniciativy parametry jsou v podstatě seznam parametrů, které jsou používány definice zásady v rámci iniciativy.

Například trvat scénáři kde máte iniciativy definice - **initiativeC**, s dvě definice zásady. Každý má jeden parametr definované definice zásady:

|Zásada  |Název parametru     |Typ parametru  |Poznámka                                                                                                |
|--------|----------------------|-------|----------------------------------------------------------------------------------------------------------------|
|policyA |allowedLocations      |Pole  |Tento parametr očekává seznam řetězců pro hodnotu vzhledem k tomu, že typ parametru byla definována jako pole |
|policyB |allowedSingleLocation |Řetězec |Tento parametr očekává jednoho slova pro hodnotu vzhledem k tomu, že typ parametru byla definována jako řetězec          |

V tomto scénáři při definování iniciativy parametry **initiativeC**, máte tři možnosti:

1. Využít parametry definice zásady v rámci této initiative: V tomto příkladu *allowedLocations* a *allowedSingleLocation* stát iniciativy parametry pro  **initiativeC**.
2. Zadejte hodnoty na parametry definice zásad této iniciativy definice. V tomto příkladu můžete zadat seznam umístění **policyA na parametr – allowedLocations** a **policyB na parametr – allowedSingleLocation**.
Můžete také zadat hodnoty při přiřazování této iniciativy.
3. Zadejte seznam *hodnotu* možnosti, které lze použít při přiřazování této iniciativy. To znamená, že při přiřazení této initiative zděděné parametry z definice zásady v rámci iniciativa, může mít pouze hodnoty ze zadaného seznamu.

Například, pokud zadaný seznam hodnota možnosti při vytváření definice iniciativy, má – *EastUS*, *WestUS*, *CentralUS*, a *WestEurope* , nebude možné jako vstup jinou hodnotu, jako *jihovýchodní Asie* během iniciativy přiřazení, protože není součástí seznamu.

## <a name="recommendations-for-managing-policies"></a>Doporučení pro správu zásad

Při vytváření a správě definice zásady a přiřazení, zde je několik ukazatele, doporučujeme, abyste postupovali podle:

- Při vytváření definice zásad ve vašem prostředí, doporučujeme začít s sledovat účinek a odepřít platit, aby auditu dopad vaše definice zásad ve vašem prostředí. Pokud máte skripty již na místě, které chcete používat automatické škálování nahoru aplikace, nastavení Odepřít vliv mohou bránit automatizaci úkoly, které už máte na místě.
- Je důležité mít na paměti organizační hierarchie, při vytváření definice a přiřazení. Doporučujeme, abyste vytváření definic na úrovni předplatného nebo vyšší úrovni, například na skupinu pro správu a přiřazení na další podřízené úrovni. Například pokud vytvoříte definici zásady na úrovni skupiny správy, přiřazení zásad definice lze snížit úrovni předplatného.
- Doporučujeme přes standardní cenovou úroveň, abyste lépe pochopili, stav dodržování předpisů vašeho prostředí.
- Doporučujeme vždy používat iniciativy definice místo definice zásad, i v případě, že máte jenom jednu zásadu v paměti. Pokud máte definici zásady – například *policyDefA* a musíte ji vytvořit v rámci iniciativy definice - *initiativeDefC*, pokud se rozhodnete vytvořit jinou definici zásady s cíle jako u *policyDefA*, můžete ho jednoduše přidat *initiativeDefC* a sledovat je lepší tímto způsobem.

   Mějte na paměti, že po vytvoření iniciativy přiřazení z definice iniciativy žádné nové definice zásady přidané do definice iniciativy automaticky vrátíte v rámci iniciativy přiřazení v rámci této iniciativy definice. Ale pokud je nový parametr seznámili novou definici zásady, budete muset aktualizovat iniciativy definice a přiřazení tak, aby odrážela to úpravou definice nebo přiřazení.

## <a name="next-steps"></a>Další kroky:
Nyní když máte přehled zásad Azure a některé klíčové koncepty, které jsme se představení, zde jsou navrhované další kroky:

- [Přiřadit definici zásady](./assign-policy-definition.md)
- [Přiřadit definici zásady pomocí rozhraní příkazového řádku Azure](./assign-policy-definition-cli.md)
- [Přiřadit definici zásady pomocí prostředí PowerShell](./assign-policy-definition-ps.md)
