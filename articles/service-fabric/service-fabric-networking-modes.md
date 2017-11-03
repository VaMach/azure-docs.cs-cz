---
title: "Konfigurace sítě režimy služby Azure Service Fabric kontejneru | Microsoft Docs"
description: "Zjistěte, jak nastavit různé režimy sítě, které podporuje Azure Service Fabric."
services: service-fabric
documentationcenter: .net
author: mani-ramaswamy
manager: timlt
editor: 
ms.assetid: d552c8cd-67d1-45e8-91dc-871853f44fc6
ms.service: service-fabric
ms.devlang: dotNet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 8/9/2017
ms.author: subramar
ms.openlocfilehash: 1ecded3af6396f50e67dc5d2a9ef8337699046ea
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/11/2017
---
# <a name="service-fabric-container-networking-modes"></a>Režimy sítě kontejneru Service Fabric

Je výchozí sítě režimu nenabízí cluster Service Fabric pro služby kontejneru `nat` sítě režimu. Pomocí `nat` sítě režimu s více než jedna služba kontejnery naslouchání stejný port výsledkem chyby nasazení. Spuštění několik služeb této naslouchání na stejném portu, Service Fabric podporuje `open` sítě režimu (verze 5.7 nebo vyšší). Pomocí `open` sítě režimu, každou službu kontejneru získá dynamicky přiřazenou IP adresu interně povolení více služeb tak, aby naslouchala na stejný port.   

Proto s jednoho typu služby s koncový bod statické definované v service manifest, nové služby může vytvořit a odstraněn bez chyby nasazení pomocí `open` sítě režimu. Podobně stejné můžete použít jednu `docker-compose.yml` souboru s mapováním statický port pro vytváření více služeb.

Použít dynamicky přiřazené IP ke zjištění služby není vhodné od změny IP adresy při restartování služby nebo přesune do jiného uzlu. Použít pouze **Service Fabric Naming Service** nebo **služba DNS** pro zjišťování služby. 


> [!WARNING]
> Na virtuální sítě v Azure jsou povoleny pouze celkem 4096 IP adresy. Proto součet počet uzlů a počet instancí kontejneru služby (s `open` sítě) nesmí být delší než 4096 v rámci virtuální sítě. U scénářů s vysokou hustotou `nat` se doporučuje sítě režimu.
>

## <a name="setting-up-open-networking-mode"></a>Nastavení sítě režim otevření

1. Nastavení šablony Azure Resource Manageru povolením služby DNS a IP zprostředkovatele v rámci `fabricSettings`. 

    ```json
    "fabricSettings": [
                {
                    "name": "DnsService",
                    "parameters": [
                       {
                            "name": "IsEnabled",
                            "value": "true"
                      }
                    ]
                },
                {
                    "name": "Hosting",
                    "parameters": [
                      { 
                            "name": "IPProviderEnabled",
                            "value": "true"
                      }
                    ]
                },
                {
                    "name":  "Trace/Etw", 
                    "parameters": [
                    {
                            "name": "Level",
                            "value": "5"
                    }
                    ]
                },
                {
                    "name": "Setup",
                    "parameters": [
                    {
                            "name": "ContainerNetworkSetup",
                            "value": "true"
                    }
                    ]
                }
            ],
    ```

2. Nastavte části profil sítě a povolit více IP adres nakonfigurovat na každém uzlu clusteru. Následující příklad nastaví pět IP adres na uzel (proto vám může mít pět instancí služby naslouchání na portu na každém uzlu) pro cluster Service Fabric Windows nebo Linuxem.

    ```json
    "variables": {
        "nicName": "NIC",
        "vmName": "vm",
        "virtualNetworkName": "VNet",
        "vnetID": "[resourceId('Microsoft.Network/virtualNetworks',variables('virtualNetworkName'))]",
        "vmNodeType0Name": "[toLower(concat('NT1', variables('vmName')))]",
        "subnet0Name": "Subnet-0",
        "subnet0Prefix": "10.0.0.0/24",
        "subnet0Ref": "[concat(variables('vnetID'),'/subnets/',variables('subnet0Name'))]",
        "lbID0": "[resourceId('Microsoft.Network/loadBalancers',concat('LB','-', parameters('clusterName'),'-',variables('vmNodeType0Name')))]",
        "lbIPConfig0": "[concat(variables('lbID0'),'/frontendIPConfigurations/LoadBalancerIPConfig')]",
        "lbPoolID0": "[concat(variables('lbID0'),'/backendAddressPools/LoadBalancerBEAddressPool')]",
        "lbProbeID0": "[concat(variables('lbID0'),'/probes/FabricGatewayProbe')]",
        "lbHttpProbeID0": "[concat(variables('lbID0'),'/probes/FabricHttpGatewayProbe')]",
        "lbNatPoolID0": "[concat(variables('lbID0'),'/inboundNatPools/LoadBalancerBEAddressNatPool')]"
    }
    "networkProfile": {
                "networkInterfaceConfigurations": [
                  {
                    "name": "[concat(parameters('nicName'), '-0')]",
                    "properties": {
                      "ipConfigurations": [
                        {
                          "name": "[concat(parameters('nicName'),'-',0)]",
                          "properties": {
                            "primary": "true",
                            "loadBalancerBackendAddressPools": [
                              {
                                "id": "[variables('lbPoolID0')]"
                              }
                            ],
                            "loadBalancerInboundNatPools": [
                              {
                                "id": "[variables('lbNatPoolID0')]"
                              }
                            ],
                            "subnet": {
                              "id": "[variables('subnet0Ref')]"
                            }
                          }
                        },
                        {
                          "name": "[concat(parameters('nicName'),'-', 1)]",
                          "properties": {
                            "primary": "false",
                            "subnet": {
                              "id": "[variables('subnet0Ref')]"
                            }
                          }
                        },
                        {
                          "name": "[concat(parameters('nicName'),'-', 2)]",
                          "properties": {
                            "primary": "false",
                            "subnet": {
                              "id": "[variables('subnet0Ref')]"
                            }
                          }
                        },
                        {
                          "name": "[concat(parameters('nicName'),'-', 3)]",
                          "properties": {
                            "primary": "false",
                            "subnet": {
                              "id": "[variables('subnet0Ref')]"
                            }
                          }
                        },
                        {
                          "name": "[concat(parameters('nicName'),'-', 4)]",
                          "properties": {
                            "primary": "false",
                            "subnet": {
                              "id": "[variables('subnet0Ref')]"
                            }
                          }
                        },
                        {
                          "name": "[concat(parameters('nicName'),'-', 5)]",
                          "properties": {
                            "primary": "false",
                            "subnet": {
                              "id": "[variables('subnet0Ref')]"
                            }
                          }
                        }
                      ],
                      "primary": true
                    }
                  }
                ]
              }
   ```
 

3. Pro clustery Windows nastavte pravidlo NSG otevření portů UDP/53 pro virtuální síť s následujícími hodnotami:

   | Priorita |    Name (Název)    |    Zdroj      |  Cíl   |   Služba    | Akce |
   |:--------:|:----------:|:--------------:|:--------------:|:------------:|:------:|
   |     2000 | Custom_Dns | VirtualNetwork | VirtualNetwork | DNS (UDP/53) | Povolit  |


4. Zadejte režim sítě v manifest aplikace pro každou službu `<NetworkConfig NetworkType="open">`.  Režim `open` výsledky v rámci služby získávání vyhrazenou IP adresu. Pokud není zadán režim, nastaví se jako výchozí základní `nat` režimu. Proto v následujícím příkladu manifestu `NodeContainerServicePackage1` a `NodeContainerServicePackage2` můžete každý naslouchání ke stejnému portu (obě služby jsou naslouchá na `Endpoint1`).

    ```xml
    <?xml version="1.0" encoding="UTF-8"?>
    <ApplicationManifest ApplicationTypeName="NodeJsApp" ApplicationTypeVersion="1.0" xmlns="http://schemas.microsoft.com/2011/01/fabric" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">
      <Description>Calculator Application</Description>
      <Parameters>
        <Parameter Name="ServiceInstanceCount" DefaultValue="3"></Parameter>
        <Parameter Name="MyCpuShares" DefaultValue="3"></Parameter>
      </Parameters>
      <ServiceManifestImport>
        <ServiceManifestRef ServiceManifestName="NodeContainerServicePackage1" ServiceManifestVersion="1.0"/>
        <Policies>
          <ContainerHostPolicies CodePackageRef="NodeContainerService1.Code" Isolation="hyperv">
           <NetworkConfig NetworkType="open"/>
           <PortBinding ContainerPort="8905" EndpointRef="Endpoint1"/>
          </ContainerHostPolicies>
        </Policies>
      </ServiceManifestImport>
      <ServiceManifestImport>
        <ServiceManifestRef ServiceManifestName="NodeContainerServicePackage2" ServiceManifestVersion="1.0"/>
        <Policies>
          <ContainerHostPolicies CodePackageRef="NodeContainerService2.Code" Isolation="default">
            <NetworkConfig NetworkType="open"/>
            <PortBinding ContainerPort="8910" EndpointRef="Endpoint1"/>
          </ContainerHostPolicies>
        </Policies>
      </ServiceManifestImport>
    </ApplicationManifest>
    ```
Můžete kombinovat a párovat síťové různé režimy ve službách v rámci aplikace pro Windows cluster. Proto může mít některé služby `open` režimu a některé na `nat` sítě režimu. Když je nakonfigurované služby `nat`, naslouchá na portu musí být jedinečný. Kombinování sítě režimy pro různé služby se nepodporuje v clusterech Linux. 


## <a name="next-steps"></a>Další kroky
V tomto článku jste se dozvěděli o sítě režimy, které nabízí Service Fabric.  

* [Model aplikace Service Fabric](service-fabric-application-model.md)
* [Prostředky manifestu služby Service Fabric](https://docs.microsoft.com/en-us/azure/service-fabric/service-fabric-service-manifest-resources)
* [Nasazení kontejneru systému Windows pro Service Fabric na Windows Server 2016](service-fabric-get-started-containers.md)
* [Nasadit kontejner Docker do Service Fabric v systému Linux](service-fabric-get-started-containers-linux.md)
