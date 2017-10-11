---
title: Model aplikace Azure Service Fabric | Microsoft Docs
description: "Jak modelu a popisují aplikace a služby v Service Fabric."
services: service-fabric
documentationcenter: .net
author: rwike77
manager: timlt
editor: mani-ramaswamy
ms.assetid: 17a99380-5ed8-4ed9-b884-e9b827431b02
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 8/9/2017
ms.author: ryanwi
ms.openlocfilehash: e30482427b88eb3e58d39075c7f0734664b79aa2
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/18/2017
---
# <a name="model-an-application-in-service-fabric"></a>Model aplikace v Service Fabric
Tento článek obsahuje přehled aplikačního modelu Azure Service Fabric a jak definovat aplikace a služby pomocí souborů manifestu.

## <a name="understand-the-application-model"></a>Pochopení aplikačního modelu
Aplikace je kolekce základních služeb, které provádějí určité funkce nebo funkce. Služba provede kompletní a samostatné funkce a začít a spustit nezávisle na jiné služby.  Služba se skládá z kódu, konfigurace a data. Pro každou službu kódu se skládá z spustitelné binární soubory, konfigurace se skládá z nastavení služby, které je možné načíst v době běhu a data se skládá z libovolné statických dat, který se má používat služba. Jednotlivé komponenty v tomto modelu hierarchické aplikace může být verzí a upgradovali nezávisle.

![Aplikační model Service Fabric][appmodel-diagram]

Typ aplikace kategorizaci aplikace a skládá se z sady typy služeb. Typ služby je kategorizaci služby. Kategorizaci podle služby může mít různá nastavení a konfigurace, ale základní funkce zůstává stejná. Instance služby jsou variacím konfigurace jinou službu stejného typu služby.  

Třídy (nebo "typů") aplikací a služeb, které jsou popsané prostřednictvím souborů XML (manifestů aplikace a služby manifesty).  Manifesty jsou šablony, kterými může být aplikace vytvořena z úložiště bitových kopií clusteru. Definice schématu pro soubor ServiceManifest.xml a ApplicationManifest.xml je nainstalován pomocí Service Fabric SDK a nástroje k *C:\Program Files\Microsoft SDKs\Service Fabric\schemas\ServiceFabricServiceModel.xsd*.

Kód pro jinou aplikaci instance spustit jako samostatné procesy, i když jsou hostované ve stejném uzlu Service Fabric. Kromě toho je možné spravovat životní cyklus každá instance aplikace (například upgradovat) nezávisle. Následující diagram znázorňuje, jak typy aplikací se skládají z typů služeb, které pak se skládají z kódu, konfigurace a data balíčky. Pro zjednodušení diagramu, jenom kód/config/data balíčky pro `ServiceType4` se zobrazují, když každý typ služby by mělo zahrnovat některé nebo všechny ty typy balíčků.

![Typy aplikací Service Fabric a typů služeb][cluster-imagestore-apptypes]

K popisu služby a aplikace se používají dva různé soubory manifestu: service manifest a manifest aplikace. Manifesty jsou podrobně popsány v následujících částech.

Aktivní v clusteru může být jeden nebo více instancí typu služby. Například instance stavové služby nebo repliky, dosáhnout vysokou spolehlivostí nastavením stavu replikace mezi replikami, které jsou umístěné na různých uzlech v clusteru. Replikace v podstatě poskytuje redundanci pro službu být k dispozici i v případě selhání jednoho uzlu v clusteru. A [oddíly služby](service-fabric-concepts-partitioning.md) další rozdělí stavu (a vzory přístupu na tento stav) mezi uzly v clusteru.

Následující diagram znázorňuje vztah mezi aplikací a instance služby, oddíly a repliky.

![Oddíly a repliky v rámci služby][cluster-application-instances]

> [!TIP]
> Rozložení aplikace si můžete prohlédnout v clusteru s podporou pomocí nástroje Service Fabric Explorer k dispozici v http://&lt;yourclusteraddress&gt;: 19080/Explorer. Další informace najdete v tématu [vizualizace vašeho clusteru pomocí Service Fabric Exploreru](service-fabric-visualizing-your-cluster.md).
> 
> 

## <a name="describe-a-service"></a>Popis služby
Manifest služby deklarativně definuje typ služby a verze. Určuje metadata služby, jako je například typ služby, Vlastnosti stavu, metriky Vyrovnávání zatížení, binární soubory služby a konfigurační soubory.  Jinými slovy, popisuje kód, konfigurace a data balíčky, které tvoří balíček služby pro podporu jeden nebo více typů služeb. Zde je jednoduchý příklad manifest služby:

```xml
<?xml version="1.0" encoding="utf-8" ?>
<ServiceManifest Name="MyServiceManifest" Version="SvcManifestVersion1" xmlns="http://schemas.microsoft.com/2011/01/fabric" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">
  <Description>An example service manifest</Description>
  <ServiceTypes>
    <StatelessServiceType ServiceTypeName="MyServiceType" />
  </ServiceTypes>
  <CodePackage Name="MyCode" Version="CodeVersion1">
    <SetupEntryPoint>
      <ExeHost>
        <Program>MySetup.bat</Program>
      </ExeHost>
    </SetupEntryPoint>
    <EntryPoint>
      <ExeHost>
        <Program>MyServiceHost.exe</Program>
      </ExeHost>
    </EntryPoint>
    <EnvironmentVariables>
      <EnvironmentVariable Name="MyEnvVariable" Value=""/>
      <EnvironmentVariable Name="HttpGatewayPort" Value="19080"/>
    </EnvironmentVariables>
  </CodePackage>
  <ConfigPackage Name="MyConfig" Version="ConfigVersion1" />
  <DataPackage Name="MyData" Version="DataVersion1" />
</ServiceManifest>
```

**Verze** atributy jsou nestrukturovaných řetězce a analyzovat není v systému. Verze atributy se používá k verze jednotlivých součástí pro upgrade.

**ServiceTypes** deklaruje, jaké typy služby podporuje **CodePackages** v tomto manifestu. Při vytváření instance služby pro jeden z těchto typů služeb aktivují se všechny balíčky kódu, které jsou deklarované v manifestu tento spuštěním jejich vstupní body. Výsledný procesy se očekává registrace podporovaných typů služeb v době běhu. Typů služeb jsou deklarované v manifestu úroveň a úroveň typu balíček kódu. Takže pokud je více balíčků kódu, všechny aktivují se vždy, když systém hledá pro každý typ deklarované služby.

**SetupEntryPoint** je privilegované vstupního bodu, který běží se stejnými pověřeními, jako Service Fabric (obvykle *LocalSystem* účtu) před další vstupní bod. Spustitelný soubor určený podle **EntryPoint** je obvykle dlouho běžící hostitele služby. Přítomnost vstupní bod samostatného instalačního zabraňuje nutnosti spuštění hostitele služby s vysokou úrovní oprávnění pro dlouhou dobu. Spustitelný soubor určený podle **EntryPoint** běží **SetupEntryPoint** ukončí úspěšně. Pokud proces se někdy ukončí nebo dojde k chybě, výsledná proces monitorovat a restartuje (počínaje znovu **SetupEntryPoint**).  

Typické scénáře použití **SetupEntryPoint** jsou při spustit spustitelný soubor před spuštěním služby nebo k provedení operace se zvýšenými oprávněními. Například:

* Nastavení a inicializace proměnných prostředí, které potřebuje spustitelný soubor služby. Toto není omezen na napsané pomocí Service Fabric programovací modely pouze spustitelné soubory. Například npm.exe musí některé proměnné prostředí nakonfigurované pro nasazení aplikace node.js.
* Nastavení řízení přístupu nainstalováním certifikáty zabezpečení.

Další informace o tom, jak nakonfigurovat **SetupEntryPoint** najdete v části [nakonfigurovat zásady pro bod služby instalační položka](service-fabric-application-runas-security.md)

**EnvironmentVariables** poskytuje seznam proměnných prostředí, které jsou nastavené pro tento balíček kódu. Environmentment proměnné mohou být přepsána nastaveními v `ApplicationManifest.xml` zajistit různé hodnoty pro instance různé služby. 

**DataPackage** deklaruje do složky s názvem podle **název** atribut, který obsahuje libovolné statických dat, který se má používat v procesu v době běhu.

**ConfigPackage** deklaruje do složky s názvem podle **název** atribut, který obsahuje *souborech Settings.xml* souboru. Soubor nastavení obsahuje oddíly pár definovaný uživatelem, klíč hodnota nastavení, která čte proces zpět za běhu. Během upgradu, pokud je to pouze **ConfigPackage** **verze** došlo ke změně, pak není běžící proces restartovat. Místo toho zpětné volání upozorní proces, který se změnila nastavení konfigurace, může být dynamicky znovu. Tady je příklad *souborech Settings.xml* souboru:

```xml
<Settings xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns="http://schemas.microsoft.com/2011/01/fabric">
  <Section Name="MyConfigurationSection">
    <Parameter Name="MySettingA" Value="Example1" />
    <Parameter Name="MySettingB" Value="Example2" />
  </Section>
</Settings>
```

> [!NOTE]
> Manifest služby může obsahovat více kód, konfigurace a data balíčky. Každý z nich může být verzí nezávisle.
> 
> 

<!--
For more information about other features supported by service manifests, refer to the following articles:

*TODO: LoadMetrics, PlacementConstraints, ServicePlacementPolicies
*TODO: Resources
*TODO: Health properties
*TODO: Trace filters
*TODO: Configuration overrides
-->

## <a name="describe-an-application"></a>Popis aplikace
Manifest aplikace deklarativně popisuje typ aplikace a verze. Určuje metadata služby složení například stabilní názvy, vytváření oddílů schématu, faktor počet/replikace instance, zásady zabezpečení nebo izolace, omezení umístění, konfigurace přepsání a typů základní služby. Vyrovnávání zatížení domény, do které se umístí aplikace jsou také popsány.

Manifest aplikace proto popisuje elementy na úrovni aplikace a odkazuje na jeden nebo více manifesty služby k vytváření typ aplikace. Zde je jednoduchý příklad manifest aplikace:

```xml
<?xml version="1.0" encoding="utf-8" ?>
<ApplicationManifest
      ApplicationTypeName="MyApplicationType"
      ApplicationTypeVersion="AppManifestVersion1"
      xmlns="http://schemas.microsoft.com/2011/01/fabric"
      xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">
  <Description>An example application manifest</Description>
  <ServiceManifestImport>
    <ServiceManifestRef ServiceManifestName="MyServiceManifest" ServiceManifestVersion="SvcManifestVersion1"/>
    <ConfigOverrides/>
    <EnvironmentOverrides CodePackageRef="MyCode"/>
  </ServiceManifestImport>
  <DefaultServices>
     <Service Name="MyService">
         <StatelessService ServiceTypeName="MyServiceType" InstanceCount="1">
             <SingletonPartition/>
         </StatelessService>
     </Service>
  </DefaultServices>
</ApplicationManifest>
```

Jako služba manifesty **verze** atributy jsou nestrukturovaných řetězce a nejsou analyzovat v systému. Verze atributy jsou také použít k verze jednotlivých součástí pro upgrade.

**ServiceManifestImport** obsahuje odkazy na manifesty služby, které tvoří tento typ aplikace. Manifesty importované služba zjistit, jaké typy služby jsou platné v rámci tento typ aplikace. V rámci ServiceManifestImport můžete přepsat hodnoty konfigurace v souborech Settings.xml a prostředí proměnné v ServiceManifest.xml soubory. 


**DefaultServices** deklaruje instance služby, které se automaticky vytvoří vždy, když aplikace je vytvořena instance pro tento typ aplikace. Výchozí služby jsou pouze pro vaše pohodlí a chovají se jako normální služby, každý po jejich vytvoření. Tyto jsou upgradovány spolu s jinými službami v instanci aplikace a může být odebrán také.

> [!NOTE]
> Manifest aplikace může obsahovat více manifestu importy služby a výchozí služby. Každý importu manifestu služby může být nezávisle verzí.
> 
> 

Zjistěte, jak udržovat různé aplikace a služby parametry pro jednotlivá prostředí, najdete v tématu [Správa parametry aplikace pro prostředí s více](service-fabric-manage-multiple-environment-app-configuration.md).

<!--
For more information about other features supported by application manifests, refer to the following articles:

*TODO: Application parameters
*TODO: Security, Principals, RunAs, SecurityAccessPolicy
*TODO: Service Templates
-->



## <a name="next-steps"></a>Další kroky
[Balíček aplikace](service-fabric-package-apps.md) a nastavit jej jako připraveny k nasazení.

[Nasazení a odebírat aplikace] [ 10] popisuje, jak pomocí prostředí PowerShell pro správu instancí aplikace.

[Správa aplikací parametry pro prostředí s více] [ 11] popisuje postup konfigurace parametrů a proměnných prostředí pro instance jinou aplikaci.

[Konfigurovat zásady zabezpečení pro vaši aplikaci] [ 12] popisuje, jak ke spouštění služeb v rámci zásad zabezpečení, které omezují přístup.

[Aplikace hostující modely] [ 13] popisují vztah mezi nasazená služba a služba hostitelský proces repliky (nebo instance).

<!--Image references-->
[appmodel-diagram]: ./media/service-fabric-application-model/application-model.png
[cluster-imagestore-apptypes]: ./media/service-fabric-application-model/cluster-imagestore-apptypes.png
[cluster-application-instances]: media/service-fabric-application-model/cluster-application-instances.png

<!--Link references--In actual articles, you only need a single period before the slash-->
[10]: service-fabric-deploy-remove-applications.md
[11]: service-fabric-manage-multiple-environment-app-configuration.md
[12]: service-fabric-application-runas-security.md
[13]: service-fabric-hosting-model.md
