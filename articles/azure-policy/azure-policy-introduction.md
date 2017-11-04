---
title: "Přehled Azure zásad | Microsoft Docs"
description: "Azure zásad je služba v Azure, který použijete k vytvoření, přiřazení a spravovat zásady definice v prostředí Azure."
services: azure-policy
keywords: 
author: Jim-Parker
ms.author: jimpark; nini
ms.date: 10/25/2017
ms.topic: overview
ms.service: azure-policy
manager: jochan
ms.custom: mvc
ms.openlocfilehash: 82721fe984ff7b3c7440b11d7526a9413b0770de
ms.sourcegitcommit: 3df3fcec9ac9e56a3f5282f6c65e5a9bc1b5ba22
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/04/2017
---
# <a name="what-is-azure-policy"></a>Co je Azure zásad?

Zásady správného řízení IT vytvoří přehlednost mezi obchodních cílů a IT projekty. Dobrý řízení IT zahrnuje vaše iniciativami v oblasti plánování a nastavování priorit na strategické úrovni. Vaše společnost prostředí velký počet problémů IT, které zdá se, že nikdy potřebujete vyřešit? Implementace zásad vám pomůže lépe spravovat a zabránit jejich. Implementace zásad přichází zásad Azure.

Azure zásad je služba v Azure, který použijete k vytvoření, přiřazení a spravovat definice zásady. Definice zásady vynutit různá pravidla a akce přes vaše prostředky, takže tyto prostředky zůstat kompatibilní s firemními standardy a smlouvy o úrovni služeb. Azure zásad běží zkušební verzi vašich prostředků kontrolu pro ty, které nejsou kompatibilní s definice zásady, které máte. Například můžete mít zásady a umožní, jsou pouze určitý typ virtuálních počítačů. Jiné vyžaduje, aby všechny prostředky bylo konkrétní značku. Tyto zásady jsou proto vyhodnocována při vytváření nebo aktualizaci prostředky.

## <a name="how-is-it-different-from-rbac"></a>Jak se liší od RBAC?

Existuje několik hlavní rozdíly mezi zásadami a řízení přístupu na základě role (RABC). RBAC se zaměřuje na akcí uživatele na různých místech. Například je může být přidán k roli Přispěvatel pro skupinu prostředků na požadovaný rozsah. Role umožňuje provést změny do této skupiny prostředků. Zásady se zaměřuje na vlastnosti prostředku během nasazení a pro již existující prostředky. Například prostřednictvím zásad, můžete řídit typy prostředků, které mohou být zřízeny. Nebo můžete omezit umístění, ve kterých se dá zřídit prostředky. Na rozdíl od RBAC, je povolit výchozí zásady a explicitní odepřít systému.

Pokud chcete používat zásady, musí být ověřeny pomocí RBAC. Konkrétně, musí váš účet:

- `Microsoft.Authorization/policydefinitions/write`oprávnění k definování zásad.
- `Microsoft.Authorization/policyassignments/write`oprávnění k přiřazení zásad.

Tato oprávnění nejsou součástí **Přispěvatel** role.


## <a name="policy-definition"></a>Definice zásad

Každý definice zásady má podmínky, za kterých je vynucená. A má doprovodné akci, která probíhá, pokud jsou splněny podmínky.

V zásadách Azure nabízíme některé předdefinované zásady, které jsou k dispozici ve výchozím nastavení. Například:

- **Vyžadovat SQL serveru 12.0**: tuto definici zásady má podmínky nebo pravidla zajistit, že všechny servery SQL Server používat verzi 12.0. Jeho akce je tak, aby odepřel všechny servery, které tato kritéria nesplňují.
- **Povolená SKU účet úložiště**: Tato zásada definice obsahuje sadu podmínky nebo pravidla, která určí, zda účet úložiště, který se nasazuje v rámci sady velikostí SKU. Jeho akce je tak, aby odepřel všechny servery, které neodpovídají sadu definované velikosti SKU.
- **Typ prostředku povoleno**: tuto definici zásady obsahuje sadu podmínek/pravidel můžete určit typy prostředků, které můžete nasadit vaší organizace. Jeho akce je tak, aby odepřel všechny prostředky, které nejsou součástí tohoto seznamu definované.
- **Povolené umístění**: Tato zásada umožňuje omezit umístění, které vaše organizace můžete zadat při nasazení prostředků. Jeho akce slouží k vynucení požadavků geo-dodržování předpisů.
- **Povolená SKU virtuální počítač**: Tato zásada umožňuje určit sadu položek SKU, které vaše organizace můžete nasadit virtuální počítač.
- **Použít značky a jeho výchozí hodnotu**: Tato zásada se aplikuje požadované značky a jeho výchozí hodnotu, pokud není zadaný uživatelem.
- **Vynutit značku a její hodnota**: tyto zásady vynucují požadované značky a jeho hodnota k prostředku.
- **Není povoleno typy prostředků**: Tato zásada umožňuje určit typy prostředků, které vaše organizace nelze nasadit.

Můžete přiřadit některé z těchto zásad prostřednictvím portálu Azure, PowerShell nebo rozhraní příkazového řádku Azure.

Další informace o struktury definice zásady, podívejte se na tento článek - [struktury definice zásady](../azure-resource-manager/resource-manager-policy.md#policy-definition-structure).

## <a name="policy-assignment"></a>Přiřazení zásad

Přiřazení zásady je definici zásady, který byl přiřazen proběhla v rámci konkrétní obor. Tento obor může být v rozsahu od skupiny pro správu do skupiny prostředků. Přiřazení zásad jsou zdědí všechny podřízené prostředky. Takže pokud je zásada pro skupinu prostředků, se použije na všechny prostředky v příslušné skupině prostředků. Termín *oboru* odkazuje na všechny skupiny prostředků, odběry nebo skupin pro správu, které definice zásady je přiřazena k.

Další informace o nastavení zásad definice a přiřazení najdete v tématu [vytvoří přiřazení zásady k identifikaci nekompatibilní prostředky v prostředí Azure](assign-policy-definition.md).

## <a name="policy-parameters"></a>Parametry zásad

Zásady parametry zjednodušit vaší zásady správy snížením počtu definice zásady, které je nutné vytvořit. Při vytváření definice zásad Chcete-li více Obecné můžete definovat parametry. Pak můžete znovu použít tuto definici zásady pro různé scénáře. Můžete to provést předávání různé hodnoty při přiřazování definice zásady. Například když zadáte jednu sadu umístění pro předplatné.

Parametry jsou definované nebo vytvořit při vytváření definice zásad. Pokud je parametr definované, je zadaný název a volitelně zadána hodnota. Můžete například definovat parametr pro zásady s názvem *umístění*. Pak můžete jí přiřadit různé hodnoty, jako *EastUS* nebo *WestUS* při přiřazování zásady.

<!--
Next link should point to new Concept page for Parameters
-->
Další informace o parametrech zásad najdete v tématu [přehled zásad prostředků - parametry](../azure-resource-manager/resource-manager-policy.md#parameters).

## <a name="initiative-definition"></a>Iniciativy definice
Iniciativy definice je kolekce definice zásady, které jsou přizpůsobené k dosahování singulární zastřešující cíle. Iniciativy definice zjednodušit správu a přiřazení zásad definice. Jejich zjednodušit seskupením sady zásad jako jeden jednu položku. Můžete například vytvořit initiative s názvem **povolit monitorování v Azure Security Center**, s cílem monitorování všechna doporučení zabezpečení k dispozici ve vaší službě Azure Security Center.

V rámci této iniciativa byste měli definice zásady, jako:

1. **Monitorování bez šifrování databáze SQL ve službě Security Center** – pro monitorování nezašifrované databází SQL a servery.
2. **Monitorování chyb zabezpečení operačního systému ve službě Security Center** – sledování serverů, které nesplňují nakonfigurované směrného plánu.
3. **Sledování chybějící služby Endpoint Protection v Centru zabezpečení** – sledování serverů bez agenta ochrany nainstalovaného koncový bod.

<!--
For more information about initiative definitions, see Initiative Definitions.+ (instead of linking to this, link out to Concept page on Initiative Definitions)
-->

## <a name="initiative-assignment"></a>Iniciativy přiřazení
Podobně jako přiřazení zásady je iniciativy přiřazení iniciativy definice, které jsou přiřazeny k určité oboru. Iniciativy přiřazení snížit potřeba provést několik iniciativy definice pro každý obor. Tento obor může také rozsahu ze skupiny pro správu do skupiny prostředků.

Z předchozího příkladu **povolit monitorování v Azure Security Center** initiative lze přiřadit k různými obory. Například jeden přiřazení lze přiřadit k **subscriptionA**. Jiné lze přiřadit k **subscriptionB**.

## <a name="initiative-parameters"></a>Iniciativy parametry
Jako parametry zásad iniciativy parametry zjednodušit správu iniciativy snížením redundance. Iniciativy parametry jsou v podstatě seznam parametrů, které jsou používány definice zásady v rámci iniciativy.

Například trvat scénáři kde máte iniciativy definice - **initiativeC**, s dvě definice zásady. Každý má jeden parametr definované definice zásady:

| Zásada | Název parametru |Typ parametru  |Poznámka |
|---|---|---|---|
| policyA | allowedLocations | Pole  |Tento parametr očekává seznam řetězců pro hodnotu vzhledem k tomu, že typ parametru byla definována jako pole |
| policyB | allowedSingleLocation |Řetězec |Tento parametr očekává jednoho slova pro hodnotu vzhledem k tomu, že typ parametru byla definována jako řetězec |

V tomto scénáři při definování iniciativy parametry **initiativeC**, máte tři možnosti:

1. Použití parametrů definice zásady v rámci této initiative: V tomto příkladu *allowedLocations* a *allowedSingleLocation* stát iniciativy parametry pro **initiativeC** .
2. Zadejte hodnoty na parametry definice zásad této iniciativy definice. V tomto příkladu můžete zadat seznam umístění **policyA na parametr – allowedLocations** a **policyB na parametr – allowedSingleLocation**.
Můžete také zadat hodnoty při přiřazování této iniciativy.
3. Zadejte seznam *hodnotu* možnosti, které lze použít při přiřazování této iniciativy. Přiřadíte-li tento initiative, zděděné parametry z definice zásady v rámci iniciativa, může mít pouze hodnoty ze zadaného seznamu.

Například může vytvořit seznam možnosti hodnoty v iniciativy definice, které obsahují *EastUS*, *WestUS*, *CentralUS*, a *WestEurope*. Pokud ano, nebudete moci, jako vstup jinou hodnotu *jihovýchodní Asie* během iniciativy přiřazení, protože není součástí seznamu.

## <a name="recommendations-for-managing-policies"></a>Doporučení pro správu zásad

Při vytváření a správě definice zásady a přiřazení, zde je několik ukazatele, doporučujeme, abyste postupovali podle:

- Při vytváření definice zásad ve vašem prostředí, doporučujeme začít s auditu vliv, a efekt odepřít ke sledování dopadu vaší definice zásady s prostředky ve vašem prostředí. Pokud máte skripty již na místě, které chcete používat automatické škálování nahoru aplikace, nastavení Odepřít vliv mohou bránit automatizaci úkoly, které už máte na místě.
- Je důležité mít na paměti organizační hierarchie, při vytváření definice a přiřazení. Doporučujeme, abyste vytváření definic na úrovni předplatného nebo vyšší úrovni, například na skupinu pro správu a přiřazení na další podřízené úrovni. Například pokud vytvoříte definici zásady na úrovni skupiny správy, přiřazení zásad definice lze snížit úrovni předplatného v rámci této skupiny pro správu.
- Doporučujeme přes standardní cenovou úroveň, abyste lépe pochopili, stav dodržování předpisů vašeho prostředí. Další informace o našich cenovou modely a co každý z nich nabízejí, podívejte se na [cenová](https://azure.microsoft.com/pricing/details/azure-policy).
- Doporučujeme vždy používat iniciativy definice místo definice zásad, i v případě, že máte jenom jednu zásadu v paměti. Pokud máte definici zásady – například *policyDefA* a musíte ji vytvořit v rámci iniciativy definice - *initiativeDefC*, pokud se rozhodnete vytvořit jinou definici zásady později pro *policyDefB* s cíle podobná *policyDefA*, můžete ho přidat *initiativeDefC* a sledovat je lepší tímto způsobem.

   Mějte na paměti, že po vytvoření iniciativy přiřazení z definice iniciativy žádné nové definice zásady přidané do definice iniciativy automaticky vrátit v rámci iniciativy přiřazení v rámci této iniciativy definice. Ale pokud je nový parametr seznámili novou definici zásady, musíte aktualizovat iniciativy definice a přiřazení úpravou iniciativy definice nebo přiřazení.

## <a name="next-steps"></a>Další kroky

Nyní když máte přehled zásad Azure a některé klíčové koncepty, které jsme se představení, zde jsou navrhované další kroky:

- [Přiřadit definici zásady](./assign-policy-definition.md)
- [Přiřadit definici zásady pomocí rozhraní příkazového řádku Azure](./assign-policy-definition-cli.md)
- [Přiřadit definici zásady pomocí prostředí PowerShell](./assign-policy-definition-ps.md)
