---
title: "Rozšířené témata týkající se upgradu aplikace | Microsoft Docs"
description: "Tento článek se zabývá některá Pokročilá témata týkající se upgradu aplikace Service Fabric."
services: service-fabric
documentationcenter: .net
author: mani-ramaswamy
manager: timlt
editor: 
ms.assetid: e29585ff-e96f-46f4-a07f-6682bbe63281
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 8/9/2017
ms.author: subramar;chackdan
ms.openlocfilehash: 8d3b922f3d50b645ac9db2cc879a319df1262e0a
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/11/2017
---
# <a name="service-fabric-application-upgrade-advanced-topics"></a>Upgrade aplikace Service Fabric: advanced témata
## <a name="adding-or-removing-services-during-an-application-upgrade"></a>Přidání nebo odebrání služby během upgradu aplikace
Pokud nové služby je přidán do aplikace, která je již nasazen a publikovat jako upgrade, nové služby se přidá do nasazené aplikace.  Takové upgrade neovlivní žádné služby, které již byly součástí aplikace. Však musí být spuštěná instance služby, která byla přidána nové služby, aby byl aktivní (pomocí `New-ServiceFabricService` rutiny).

Služby může být odebrán také z aplikace jako součást upgradu. Ale musí být všechny aktuální instance služby k-be odstraněné zastaven před pokračováním v upgradu (pomocí `Remove-ServiceFabricService` rutiny).

## <a name="manual-upgrade-mode"></a>Režim manuálního upgradu
> [!NOTE]
> Měli byste zvážit sledována ruční režim pouze pro upgrade selhání nebo pozastavený. Monitorované režim je doporučené režimu upgradu pro Service Fabric aplikace.
>
>

Azure Service Fabric nabízí více upgradu režimy pro podporu vývoje a produkční clustery. Zvolené možnosti nasazení se může lišit pro různá prostředí.

Monitorované postupného upgradu aplikace je nejčastější upgrade pro použití v provozním prostředí. Pokud je zadána zásad upgradu, Service Fabric zajistí, že aplikace je v pořádku, před upgradem.

 Správce aplikací můžete použít ruční postupného upgradu režim aplikací mít úplnou kontrolu nad průběh upgradu prostřednictvím různých domén upgradu. Tento režim je užitečné, když se děje-konvenční upgrade nebo zásad vyhodnocení stavu přizpůsobené nebo komplexní, je požadována (například aplikace je již dojít ke ztrátě dat.).

Nakonec automatizované postupného upgradu aplikace je užitečné pro vývoj nebo testování prostředí zajistit rychlou iterační cyklus během vývoje služby.

## <a name="change-to-manual-upgrade-mode"></a>Změnit na režim manuálního upgradu
**Ruční**– zastavit upgradu aplikace na aktuální UD a změňte režim upgradu na sledována ručně. Správci musí ručně volání **MoveNextApplicationUpgradeDomainAsync** pokračovat v upgradu nebo aktivovat vrácení zpět pomocí inicializace nového upgradu. Po upgradu vstoupí v ručním režimu, zůstane v ručním režimu dokud nového upgradu je zahájeno. **GetApplicationUpgradeProgressAsync** příkaz vrátí FABRIC\_aplikace\_UPGRADE\_stavu\_kolejová\_dál\_čeká na vyřízení.

## <a name="upgrade-with-a-diff-package"></a>Upgrade s balíčkem rozdílů
Aplikace Service Fabric může být upgradována pomocí zřizování s balíčkem aplikace úplné, úplný a samostatný. Aplikace lze také upgradovat pomocí rozdílové balíček, který obsahuje pouze soubory aktualizované aplikace, manifest aktualizované aplikace a soubory manifestu služby.

Celou aplikaci balíček obsahuje všechny soubory potřebné ke spuštění a spuštění aplikace Service Fabric. Diff balíček obsahuje pouze soubory, které změnily mezi poslední zřídit a aktuální upgrade a další manifest úplné aplikace a služby manifest soubory. Všechny odkazy v manifestu aplikace nebo služby manifestu, který nebyl nalezen v sestavení rozložení je hledán v úložišti bitové kopie.

Celou aplikaci balíčky jsou požadovány pro první instalaci aplikace do clusteru. Následné aktualizace může být balíček celou aplikaci nebo balíček rozdílové.

Situace při pomocí balíčku rozdílové by být vhodné použít:

* Diff balíček je upřednostňované, když máte velké aplikace balíček, který odkazuje na několik souborů manifestu služby nebo několik balíčků kódu, konfigurace balíčky nebo balíčky data.
* Diff balíčku jsou upřednostňované, když máte nasazení systému, který generuje rozložení sestavení přímo z vašeho procesu sestavení aplikace. V tomto případě to i v případě, že kód se nezměnila, nově vytvořený sestavení získat různé kontrolního součtu. Pomocí balíčku pro celou aplikaci by vyžadují aktualizaci verze na všechny balíčky kódu. Pomocí balíčku rozdílů, je jenom zadat soubory, které změnily a souborů manifestu nichž došlo ke změně verze.

Při upgradu aplikace pomocí sady Visual Studio rozdílové balíček je automaticky publikován. Vytvoření balíčku rozdílové ručně, musí být aktualizovány manifest aplikace a služby manifesty, ale pouze změněné balíčky by měl být součástí balíčku poslední aplikace.

Například začneme následující aplikace (čísla verzí zadaná pro snadné pochopení):

```text
app1           1.0.0
  service1     1.0.0
    code       1.0.0
    config     1.0.0
  service2     1.0.0
    code       1.0.0
    config     1.0.0
```

Nyní předpokládejme, chcete-li aktualizovat pouze kód balíček service1 pomocí balíčku rozdílové pomocí prostředí PowerShell. Nyní aplikace aktualizovaná má následující strukturu složek:

```text
app1           2.0.0      <-- new version
  service1     2.0.0      <-- new version
    code       2.0.0      <-- new version
    config     1.0.0
  service2     1.0.0
    code       1.0.0
    config     1.0.0
```

V takovém případě je aktualizovat manifest aplikace a 2.0.0, service manifest pro service1 tak, aby odrážela aktualizace balíčku kódu. Složku balíčku aplikace by mít následující strukturu:

```text
app1/
  service1/
    code/
```

## <a name="next-steps"></a>Další kroky
[Upgrade vaší aplikace pomocí sady Visual Studio](service-fabric-application-upgrade-tutorial.md) vás provede upgrade aplikace pomocí sady Visual Studio.

[Upgrade vaší aplikace pomocí prostředí Powershell](service-fabric-application-upgrade-tutorial-powershell.md) vás provede upgrade aplikace pomocí prostředí PowerShell.

Řídí, jak vaše aplikace upgraduje pomocí [Upgrade parametry](service-fabric-application-upgrade-parameters.md).

Zkontrolujte upgradů aplikace kompatibilní podle naučit se používat [serializace dat](service-fabric-application-upgrade-data-serialization.md).

Řešení běžných potíží v upgradů aplikací podle kroků v části [řešení potíží s aplikací upgrady](service-fabric-application-upgrade-troubleshooting.md).
