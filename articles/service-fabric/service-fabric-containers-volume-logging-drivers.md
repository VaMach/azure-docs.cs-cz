---
title: Azure Service Fabric Docker Compose Preview | Microsoft Docs
description: "Azure Service Fabric přijme Docker Compose formátu, aby bylo snazší orchestraci kontejnerů exsiting pomocí Service Fabric. Tato podpora je aktuálně ve verzi preview."
services: service-fabric
documentationcenter: .net
author: mani-ramaswamy
manager: timlt
editor: 
ms.assetid: ab49c4b9-74a8-4907-b75b-8d2ee84c6d90
ms.service: service-fabric
ms.devlang: dotNet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 8/9/2017
ms.author: subramar
ms.openlocfilehash: 955f84e5656bbf568234cbaf69faa4dd0a741206
ms.sourcegitcommit: 6a22af82b88674cd029387f6cedf0fb9f8830afd
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/11/2017
---
# <a name="using-volume-plugins-and-logging-drivers-in-your-container"></a>Pomocí svazku modulů plug-in a ovladače protokolování v vašeho kontejneru
Service Fabric podporuje určení [modulů plug-in svazku Docker](https://docs.docker.com/engine/extend/plugins_volume/) a [Docker protokolování ovladače](https://docs.docker.com/engine/admin/logging/overview/) pro vaši službu kontejneru.  To vám umožní zachovat data v [Azure Files](https://azure.microsoft.com/en-us/services/storage/files/) i v případě, že jste kontejneru je přesunout nebo restartováním jiného hostitele.

Momentálně nejsou k dispozici pouze svazku ovladače pro Linux kontejnery, jak je uvedeno níže.  Pokud používáte Windows kontejnery, je možné namapovat svazek k Azure Files [SMB3 sdílení](https://blogs.msdn.microsoft.com/clustering/2017/08/10/container-storage-support-with-cluster-shared-volumes-csv-storage-spaces-direct-s2d-smb-global-mapping/) bez ovladač svazek pomocí nejnovější verze. 1709 systému Windows Server. Bude vyžadovat aktualizaci na verzi Windows serveru. 1709 virtuálních počítačů v clusteru.


## <a name="install-volumelogging-driver"></a>Nainstalujte ovladač svazku nebo protokolování

Pokud ovladač svazku nebo protokolování Docker není nainstalovaný na počítači, nainstalujte ji ručně pomocí protokolu RDP/SSH-ing do počítače, prostřednictvím [VMSS spuštění skriptu](https://azure.microsoft.com/en-us/resources/templates/201-vmss-custom-script-windows/) nebo pomocí [SetupEntryPoint](https://docs.microsoft.com/en-us/azure/service-fabric/service-fabric-application-model#describe-a-service) skriptu. Vyberete jednu z uvedených metod, můžete napsat skript pro instalaci [Docker svazku ovladač pro Azure](https://docs.docker.com/docker-for-azure/persistent-data-volumes/):


```bash
docker plugin install --alias azure --grant-all-permissions docker4x/cloudstor:17.09.0-ce-azure1  \
    CLOUD_PLATFORM=AZURE \
    AZURE_STORAGE_ACCOUNT="[MY-STORAGE-ACCOUNT-NAME]" \
    AZURE_STORAGE_ACCOUNT_KEY="[MY-STORAGE-ACCOUNT-KEY]" \
    DEBUG=1
```

## <a name="specify-the-plugin-or-driver-in-the-manifest"></a>Zadejte modul plug-in nebo ovladač v manifestu
Moduly plug-in jsou určené v manifestu aplikace, jak je znázorněno v následujícím manifestu:

```xml
?xml version="1.0" encoding="UTF-8"?>
<ApplicationManifest ApplicationTypeName="WinNodeJsApp" ApplicationTypeVersion="1.0" xmlns="http://schemas.microsoft.com/2011/01/fabric" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">
    <Description>Calculator Application</Description>
    <Parameters>
        <Parameter Name="ServiceInstanceCount" DefaultValue="3"></Parameter>
      <Parameter Name="MyCpuShares" DefaultValue="3"></Parameter>
    </Parameters>
    <ServiceManifestImport>
        <ServiceManifestRef ServiceManifestName="NodeServicePackage" ServiceManifestVersion="1.0"/>
     <Policies>
       <ContainerHostPolicies CodePackageRef="NodeService.Code" Isolation="hyperv"> 
        <PortBinding ContainerPort="8905" EndpointRef="Endpoint1"/>
        <RepositoryCredentials PasswordEncrypted="false" Password="****" AccountName="test"/>
        <LogConfig Driver="etwlogs" >
          <DriverOption Name="test" Value="vale"/>
        </LogConfig>
        <Volume Source="c:\workspace" Destination="c:\testmountlocation1" IsReadOnly="false"></Volume>
        <Volume Source="d:\myfolder" Destination="c:\testmountlocation2" IsReadOnly="true"> </Volume>
        <Volume Source="myvolume1" Destination="c:\testmountlocation2" Driver="azure" IsReadOnly="true">
           <DriverOption Name="share" Value="models"/>
        </Volume>
       </ContainerHostPolicies>
   </Policies>
    </ServiceManifestImport>
    <ServiceTemplates>
        <StatelessService ServiceTypeName="StatelessNodeService" InstanceCount="5">
            <SingletonPartition></SingletonPartition>
        </StatelessService>
    </ServiceTemplates>
</ApplicationManifest>
```

V předchozím příkladu `Source` označení pro `Volume` odkazuje do zdrojové složky. Zdrojová složka může být do složky ve virtuálním počítači, který je hostitelem kontejnery nebo trvalé vzdálené úložiště. `Destination` Značka je umístění, `Source` je namapována na spuštěné kontejneru.  Cíl proto nemůže být již existující umístění v rámci vašeho kontejneru.

Při zadávání modulu plug-in svazku, Service Fabric automaticky vytvoří svazku, pomocí zadaných parametrů. `Source` Značka je název svazku a `Driver` značky Určuje modul plug-in ovladač svazku. Možnosti lze zadat pomocí `DriverOption` značky, jak je znázorněno v následujícím fragmentu kódu:

```xml
<Volume Source="myvolume1" Destination="c:\testmountlocation4" Driver="azure" IsReadOnly="true">
           <DriverOption Name="share" Value="models"/>
</Volume>
```
Pokud je zadaný ovladač protokolu Docker, je nezbytné pro nasazení agentů (nebo kontejnery) pro zpracování protokoly v clusteru.  `DriverOption` Značky lze zadat také možnosti protokolu ovladačů.

Najdete v těchto článcích nasazení kontejnerů do clusteru Service Fabric:


[Nasazení kontejneru v Service Fabric](service-fabric-deploy-container.md)

