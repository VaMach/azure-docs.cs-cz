---
title: "Upgrade a škálování instance služby Azure API Management | Microsoft Docs"
description: "Toto téma popisuje, jak upgradovat a škálovat instance služby Azure API Management."
services: api-management
documentationcenter: 
author: vladvino
manager: anneta
editor: 
ms.service: api-management
ms.workload: integration
ms.topic: article
ms.date: 08/17/2017
ms.author: apimpm
ms.openlocfilehash: 22cc917eb6f296724bf535e48b0dd6ba8927e5d3
ms.sourcegitcommit: 9c3150e91cc3075141dc2955a01f47040d76048a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/26/2017
---
# <a name="upgrade-and-scale-an-api-management-instance"></a>Upgrade a škálování instance služby API Management 

Zákazníci můžete škálovat instance API Management (APIM) tak, že přidání a odebrání jednotek. A **jednotky** se skládá z vyhrazených prostředků Azure a má určité nosné kapacitu, vyjádřené jako počet rozhraní API volání za měsíc. Skutečné propustnosti a latence budou lišit široce v závislosti na faktorech, například číslo a počet souběžných připojení, typ a počet nakonfigurované zásady, požadavku a odpovědi velikosti a back-end latence. 

Kapacitu a cenu každé jednotky, závisí na **vrstvy** v které jednotka existuje. Můžete si vybrat mezi třech úrovních: **vývojáře**, **standardní**, **Premium**. Pokud je potřeba zvýšit kapacitu služby v rámci vrstvu, měli byste přidat jednotku. Pokud vrstvy, který je aktuálně vybraný v instanci APIM neumožňuje přidávat další jednotky, budete muset upgrade na vyšší úrovni vrstvy. 

Cena za každou položku, možnost Přidat nebo odebrat jednotek, zda máte určité funkce (například nasazení více oblast) závisí na vrstvu, kterou jste zvolili pro vaše instance APIM. [Podrobnosti o cenách](https://azure.microsoft.com/pricing/details/api-management/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio) článek, vysvětluje, co cena za jednotku a funkce získáte v jednotlivých úrovních. 

>[!NOTE]
>[Podrobnosti o cenách](https://azure.microsoft.com/pricing/details/api-management/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio) článek ukazuje přibližný počet jednotek kapacity v jednotlivých úrovních. Chcete-li získat přesnější čísla, podívejte se na scénáři realistické pro vaše rozhraní API. Najdete v části "Jak plánování kapacity", který následuje dále.

## <a name="prerequisites"></a>Požadavky

K provedení kroků popsaných v tomto článku, musíte mít:

+ Aktivní předplatné Azure.

    [!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

+ APIM instance. Další informace najdete v tématu [vytvoření instance služby Azure API Management](get-started-create-service-instance.md).

## <a name="how-to-plan-for-capacity"></a>Postup plánování kapacity?

Pokud chcete zjistit, jestli máte dost jednotky pro zpracování provozu, testovat na úlohy, které byste očekávali. 

Jak je uvedeno nahoře, počet požadavků za druhé APIM může proces závisí na mnoha proměnných. Například připojení vzor, velikost požadavku a odpovědi, zásady, které jsou nakonfigurované na každé rozhraní API, počet klientů, které jsou odesílání požadavků.

Použití **metriky** (používá možnosti monitorování Azure) zjistit, jakou kapacitu se používá v daném okamžiku.

### <a name="use-the-azure-portal-to-examine-metrics"></a>Kontrola metriky pomocí portálu Azure 

1. Přejděte k vaší instanci APIM v [portál Azure](https://portal.azure.com/).
2. Vyberte **metriky**.
3. Vyberte **kapacity** metriky z **dostupné metriky**. 

    Metriky kapacity získáte představu, jakou kapacitu se používá ve vašem klientovi. Můžete otestovat umístěním více a více načíst a zjistit, co si vyberte zatížení. Můžete nastavit metriky upozornění pokaždé, když se děje něco, co není očekáván. Například instanci APIM přistupuje kapacity pro více než 5 minut. 

    >[!TIP]
    > Můžete konfigurovat upozornění pokaždé, když služby dochází na kapacitu nebo zkontrolujte ho volání do aplikace logiky, která bude automaticky škálovat tak, že přidáte jednotku...

## <a name="upgrade-and-scale"></a>Upgrade a škálování 

Jak už bylo zmíněno dříve, můžete si vybrat mezi třech úrovních: **vývojáře**, **standardní**, **Premium**. **Vývojáře** vrstvy se má použít k vyhodnocení služby; nesmí být použita pro produkční prostředí. **Vývojáře** úroveň nemá smlouvy o úrovni služeb a nemůže škálovat tuto vrstvu (Přidat nebo odebrat jednotky). 

**Standardní** a **Premium** jsou produkční úrovně mít SLA a je možné rozšířit. **Standardní** vrstvu lze škálovat až čtyři jednotkách. Můžete přidat libovolný počet jednotek na **Premium** vrstvy. 

**Premium** vrstvy umožňuje distribuovat jedna instance API management napříč jakékoli číslo požadované oblasti Azure. Když vytvoříte původně služby API Management, instance obsahuje pouze jednu jednotku a se nachází v jedné oblasti Azure. Počáteční oblast je určený jako **primární** oblast. Další oblasti lze snadno přidat. Při přidávání oblast, je třeba zadat počet jednotek, které chcete přidělit. Například může mít jednu jednotku v **primární** oblast a pět jednotky v některé jiné oblasti. Můžete přizpůsobit pro libovolnou provoz máte v každé oblasti. Další informace najdete v tématu [nasazení instanci služby Azure API Management na několika oblastmi Azure](api-management-howto-deploy-multi-region.md).

Můžete upgradovat a starší verzi do a z libovolné úrovně. Upozorňujeme, že přechod na starší verzi můžete odebrat některé funkce, například virtuální sítě nebo nasazení s více oblast, když přechod na starší verzi standardního z úrovně Premium.

>[!NOTE]
>Proces upgradu nebo škálování může trvat od 15 do 30 minut použít. Pokud se provádí zobrazí oznámení.

### <a name="use-the-azure-portal-to-upgrade-and-scale"></a>Použijte portál Azure pro upgrade a škálování

1. Přejděte k vaší instanci APIM v [portál Azure](https://portal.azure.com/).
2. Vyberte **škálování a ceny**.
3. Vyberte požadovanou úroveň.
4. Zadejte počet **jednotky** chcete přidat. Můžete buď použít posuvník nebo zadat počet jednotek.<br/>
    Pokud se rozhodnete **Premium** úroveň, je nutné nejprve vybrat oblast.
5. Stiskněte klávesu **uložit**

## <a name="next-steps"></a>Další kroky

[Postup nasazení instanci služby Azure API Management na několika oblastmi Azure](api-management-howto-deploy-multi-region.md)

