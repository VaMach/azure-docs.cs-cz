---
title: "Správa prostředí s více v Service Fabric | Microsoft Docs"
description: "Service Fabric aplikace můžete spustit na clustery, které rozsahu velikost z jednoho počítače tisíce počítačů. V některých případech můžete ke konfiguraci vaší aplikace pro tato rozmanitých prostředí. Tento článek vysvětluje postup definujte parametry jinou aplikaci na prostředí."
services: service-fabric
documentationcenter: .net
author: mikkelhegn
manager: timlt
editor: 
ms.assetid: f406eac9-7271-4c37-a0d3-0a2957b60537
ms.service: service-fabric
ms.devlang: dotNet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 08/18/2017
ms.author: mikkelhegn
ms.openlocfilehash: 671cc9b0f7b7b37fcf5b052f7e34bc98e66b2838
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/11/2017
---
# <a name="manage-application-parameters-for-multiple-environments"></a>Spravovat aplikace parametry pro prostředí s více
Můžete vytvořit clustery Azure Service Fabric pomocí kdekoli 1 až několika tisíc počítačů. Binární soubory aplikace můžete spustit bez úprav přes tento široké spektrum prostředí, ale často chcete nakonfigurovat aplikaci, jinak, v závislosti na počtu počítačů, které nasazujete na.

Jako jednoduchý příklad, zvažte `InstanceCount` bezstavové služby. Když aplikace běží v Azure, obvykle chcete nastavte tento parametr zvláštní hodnotu-1. Tato konfigurace zajistí, že vaše služba běží na všech uzlech v clusteru (nebo každý uzel v uzlu typu, pokud jste nastavili omezení umístění). Tato konfigurace však není vhodný pro cluster s podporou jeden počítač, protože nemůže mít více procesy naslouchání na stejný koncový bod na jednom počítači. Místo toho je obvykle nastavit `InstanceCount` "1".

## <a name="specifying-environment-specific-parameters"></a>Zadání parametrů specifické pro prostředí
Řešení, aby tento problém je sada parametrizované výchozích služeb a soubory parametrů aplikace, které zadejte tyto hodnoty parametrů pro dané prostředí. Výchozí služby a aplikace parametry jsou nastaveny v manifestů aplikace a služby. Definice schématu pro ServiceManifest.xml a ApplicationManifest.xml soubory se instaluje s Service Fabric SDK a nástroje k *C:\Program Files\Microsoft SDKs\Service Fabric\schemas\ServiceFabricServiceModel.xsd*.

### <a name="default-services"></a>Výchozí služby
Aplikace Service Fabric se skládají z kolekce instancí služby. I když je možné vytvořit prázdnou aplikaci a pak vytvořit všechny instance služby dynamicky, většina aplikací mít sadu základní služby, které mají být vytvořeny vždy při vytváření instance aplikace. Tyto jsou označovány jako "výchozí služby". Jsou uvedené v manifestu aplikace, se zástupnými symboly pro konfiguraci za prostředí, které jsou součástí hranaté závorky:

```xml
  <DefaultServices>
      <Service Name="Stateful1">
          <StatefulService
              ServiceTypeName="Stateful1Type"
              TargetReplicaSetSize="[Stateful1_TargetReplicaSetSize]"
              MinReplicaSetSize="[Stateful1_MinReplicaSetSize]">

              <UniformInt64Partition
                  PartitionCount="[Stateful1_PartitionCount]"
                  LowKey="-9223372036854775808"
                  HighKey="9223372036854775807"
              />
        </StatefulService>
    </Service>
  </DefaultServices>
```

Všechny pojmenované parametry musí být definován v rámci elementu parametry manifestu aplikace:

```xml
    <Parameters>
        <Parameter Name="Stateful1_MinReplicaSetSize" DefaultValue="3" />
        <Parameter Name="Stateful1_PartitionCount" DefaultValue="1" />
        <Parameter Name="Stateful1_TargetReplicaSetSize" DefaultValue="3" />
    </Parameters>
```

Atribut DefaultValue Určuje hodnotu, který se má použít při absenci dalších konkrétní parametr pro dané prostředí.

> [!NOTE]
> Všechny parametry instance služby jsou vhodné pro konfiguraci podle prostředí. V předchozím příkladu jsou hodnoty LowKey a HighKey pro schéma rozdělení oddílů služby explicitně definovány pro všechny instance služby od rozsahu oddílu je funkce domény data, není v prostředí.
> 
> 

### <a name="per-environment-service-configuration-settings"></a>Nastavení konfigurace služby za prostředí
[Model aplikace Service Fabric](service-fabric-application-model.md) umožňuje služby zahrnuté balíčky konfigurace, které obsahují vlastní páry klíč hodnota, které jsou v době běhu čitelné. Hodnoty těchto nastavení můžete také rozlišené pomocí prostředí tak, že zadáte `ConfigOverride` v manifestu aplikace.

Předpokládejme, že máte následující nastavení v souboru Config\Settings.xml `Stateful1` služby:

```xml
  <Section Name="MyConfigSection">
     <Parameter Name="MaxQueueSize" Value="25" />
  </Section>
```
Chcete-li přepsat tuto hodnotu pro konkrétní prostředí aplikace pair, vytvořte `ConfigOverride` při importu service manifest v manifestu aplikace.

```xml
  <ConfigOverrides>
     <ConfigOverride Name="Config">
        <Settings>
           <Section Name="MyConfigSection">
              <Parameter Name="MaxQueueSize" Value="[Stateful1_MaxQueueSize]" />
           </Section>
        </Settings>
     </ConfigOverride>
  </ConfigOverrides>
```
Tento parametr můžete pak nakonfigurovat prostředí, jak je uvedeno výše. To provedete tak, že deklarace v sekci parametrů manifestu aplikace a zadání hodnoty v závislosti na prostředí v soubory parametrů aplikace.

> [!NOTE]
> V případě nastavení konfigurace služby, jsou tři místa, kde můžete nastavit hodnoty klíče: konfigurační balíček service manifest aplikace a parametr souboru aplikace. Service Fabric ze souboru aplikace parametr vždycky vybere první (Pokud je zadána), pak manifest aplikace a nakonec konfigurační balíček.
> 
> 

### <a name="setting-and-using-environment-variables"></a>Nastavení a použití proměnných prostředí 
Můžete zadat a nastavení proměnných prostředí v souboru ServiceManifest.xml a pak přepsání ApplicationManifest.xml souboru na základě za instance.
Následující příklad ukazuje dvou proměnných prostředí, jeden s nastavenou hodnotu a dalších přepsána. Parametry aplikačního slouží k nastavení hodnot proměnných prostředí stejným způsobem, že tyto se používaly k přepsání konfigurace.

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
Pokud chcete přepsat proměnné prostředí v ApplicationManifest.xml, odkazují na balíček kódu v ServiceManifest s `EnvironmentOverrides` elementu.

```xml
  <ServiceManifestImport>
    <ServiceManifestRef ServiceManifestName="FrontEndServicePkg" ServiceManifestVersion="1.0.0" />
    <EnvironmentOverrides CodePackageRef="MyCode">
      <EnvironmentVariable Name="MyEnvVariable" Value="mydata"/>
    </EnvironmentOverrides>
  </ServiceManifestImport>
 ``` 
 Po vytvoření instance služby s názvem dostanete z kódu proměnné prostředí. například v jazyce C# můžete provést následující

```csharp
    string EnvVariable = Environment.GetEnvironmentVariable("MyEnvVariable");
```

### <a name="service-fabric-environment-variables"></a>Proměnné prostředí Service Fabric
Service Fabric má vestavěné proměnné prostředí, nastavte pro každou instanci služby. Úplný seznam proměnných prostředí je níže, kde těm, které jsou v bold jsou ty, které budete používat ve službě, druhé používá modulu runtime Service Fabric. 

* Fabric_ApplicationHostId
* Fabric_ApplicationHostType
* Fabric_ApplicationId
* **Fabric_ApplicationName**
* Fabric_CodePackageInstanceId
* **Fabric_CodePackageName**
* **TypeEndpoint Fabric_Endpoint_ [YourServiceName]**
* **Fabric_Folder_App_Log**
* **Fabric_Folder_App_Temp**
* **Fabric_Folder_App_Work**
* **Fabric_Folder_Application**
* Fabric_NodeId
* **Fabric_NodeIPOrFQDN**
* **Fabric_NodeName**
* Fabric_RuntimeConnectionAddress
* Fabric_ServicePackageInstanceId
* Fabric_ServicePackageName
* Fabric_ServicePackageVersionInstance
* FabricPackageFileName

Belows kód ukazuje, jak do seznamu proměnných prostředí Service Fabric
 ```csharp
    foreach (DictionaryEntry de in Environment.GetEnvironmentVariables())
    {
        if (de.Key.ToString().StartsWith("Fabric"))
        {
            Console.WriteLine(" Environment variable {0} = {1}", de.Key, de.Value);
        }
    }
```
Tady jsou příklady proměnných prostředí pro typ aplikace volá `GuestExe.Application` volat s typem služby `FrontEndService` při spuštění v místním vývojářském počítači.

* **Fabric_ApplicationName = fabric:/GuestExe.Application**
* **Fabric_CodePackageName = kód**
* **Fabric_Endpoint_FrontEndServiceTypeEndpoint = 80**
* **Fabric_NodeIPOrFQDN = localhost**
* **Fabric_NodeName = to uzel _Node_2**

### <a name="application-parameter-files"></a>Soubory parametrů aplikace
Projekt aplikace Service Fabric může obsahovat jeden nebo více soubory parametrů aplikace. Každý z nich definuje konkrétní hodnoty pro parametry, které jsou definovány v manifestu aplikace:

```xml
    <!-- ApplicationParameters\Local.xml -->

    <Application Name="fabric:/Application1" xmlns="http://schemas.microsoft.com/2011/01/fabric">
        <Parameters>
            <Parameter Name ="Stateful1_MinReplicaSetSize" Value="3" />
            <Parameter Name="Stateful1_PartitionCount" Value="1" />
            <Parameter Name="Stateful1_TargetReplicaSetSize" Value="3" />
        </Parameters>
    </Application>
```
Ve výchozím nastavení zahrnuje tři soubory parametrů aplikace s názvem Local.1Node.xml, Local.5Node.xml a Cloud.xml nové aplikace:

![Soubory parametrů aplikace v Průzkumníku řešení][app-parameters-solution-explorer]

Pokud chcete vytvořit soubor s parametry, jednoduše zkopírujte a vložte stávající a dejte mu nový název.

## <a name="identifying-environment-specific-parameters-during-deployment"></a>Identifikace konkrétní prostředí parametrů během nasazování
Při nasazení budete muset zvolit soubor odpovídající parametru pro použití s vaší aplikací. Můžete to provést prostřednictvím dialogové okno publikování v sadě Visual Studio nebo pomocí prostředí PowerShell.

### <a name="deploy-from-visual-studio"></a>Nasazení z Visual Studia
Seznam dostupných parametrů souborů, které lze vybírat při publikování aplikace v sadě Visual Studio.

![Vyberte soubor s parametry v dialogovém okně publikování][publishdialog]

### <a name="deploy-from-powershell"></a>Nasazení z prostředí PowerShell
`Deploy-FabricApplication.ps1` Skript prostředí PowerShell, které jsou součástí šablony projektu aplikace přijímá profil publikování jako parametr a PublishProfile obsahuje odkaz na soubor parametrů aplikace.

  ```PowerShell
    ./Deploy-FabricApplication -ApplicationPackagePath <app_package_path> -PublishProfileFile <publishprofile_path>
  ```

## <a name="next-steps"></a>Další kroky
Další informace o některých základní koncepty, které jsou popsané v tomto tématu najdete v tématu [Service Fabric technický přehled](service-fabric-technical-overview.md). Informace o dalším funkcím správy aplikace, které jsou k dispozici v sadě Visual Studio najdete v tématu [spravovat aplikace Service Fabric v sadě Visual Studio](service-fabric-manage-application-in-visual-studio.md).

<!-- Image references -->

[publishdialog]: ./media/service-fabric-manage-multiple-environment-app-configuration/publish-dialog-choose-app-config.png
[app-parameters-solution-explorer]:./media/service-fabric-manage-multiple-environment-app-configuration/app-parameters-in-solution-explorer.png
