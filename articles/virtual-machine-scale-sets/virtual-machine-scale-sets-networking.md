---
title: "Síťové služby pro škálovací sady virtuálních počítačů Azure | Dokumentace Microsoftu"
description: "Konfigurace síťových vlastností pro škálovací sady virtuálních počítačů Azure."
services: virtual-machine-scale-sets
documentationcenter: 
author: gbowerman
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 76ac7fd7-2e05-4762-88ca-3b499e87906e
ms.service: virtual-machine-scale-sets
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 07/06/2017
ms.author: guybo
ms.translationtype: HT
ms.sourcegitcommit: f76de4efe3d4328a37f86f986287092c808ea537
ms.openlocfilehash: 1c9487be5415d05a8699f458259d872591280d3d
ms.contentlocale: cs-cz
ms.lasthandoff: 07/10/2017


---
# <a name="networking-for-azure-virtual-machine-scale-sets"></a>Síťové služby pro škálovací sady virtuálních počítačů Azure

Když nasadíte škálovací sadu virtuálních počítačů Azure prostřednictvím portálu, pro určité vlastnosti sítě se nastaví výchozí hodnoty, jako například příchozí pravidla NAT pro Azure Load Balancer. Tento článek popisuje, jak používat některé pokročilejší síťové funkce, které můžete nakonfigurovat se škálovacími sadami.

Všechny funkce popsané v tomto článku můžete nakonfigurovat pomocí šablon Azure Resource Manageru. Pro vybrané funkce jsou zahrnuté také příklady Azure CLI. Použijte verzi rozhraní příkazového řádku z července 2017 nebo novější. Další příklady rozhraní příkazového řádku a PowerShellu se brzy přidají.

## <a name="accelerated-networking"></a>Akcelerované síťové služby
[Akcelerované síťové služby](https://docs.microsoft.com/en-us/azure/virtual-network/virtual-network-create-vm-accelerated-networking) Azure zlepšují výkon sítě tím, že na virtuálním počítači povolují rozhraní SR-IOV (single-root I/O virtualization). Pokud chcete používat akcelerované síťové služby se škálovacími sadami, v nastavení networkInterfaceConfigurations vaší škálovací sady nastavte enableAcceleratedNetworking na hodnotu _true_. Například:
```json
"networkProfile": {
    "networkInterfaceConfigurations": [
    {
      "name": "niconfig1",
      "properties": {
        "primary": true,
        "enableAcceleratedNetworking" : true,
        "ipConfigurations": [
          ...
        ]
      }
    }
   ]
}
```

## <a name="create-a-scale-set-that-references-an-existing-azure-load-balancer"></a>Vytvoření škálovací sady odkazující na existující Azure Load Balancer
Při vytvoření škálovací sady pomocí webu Azure Portal se pro většinu možností konfigurace vytvoří nový nástroj pro vyrovnávání zatížení. Pokud vytváříte škálovací sadu, která potřebuje odkazovat na existující nástroj pro vyrovnávání zatížení, můžete to provést pomocí rozhraní příkazového řádku. Následující ukázkový skript vytvoří nástroj pro vyrovnávání zatížení a potom vytvoří škálovací sadu, která na něj odkazuje:
```bash
az network lb create -g lbtest -n mylb --vnet-name myvnet --subnet mysubnet --public-ip-address-allocation Static --backend-pool-name mybackendpool

az vmss create -g lbtest -n myvmss --image Canonical:UbuntuServer:16.04-LTS:latest --admin-username negat --ssh-key-value /home/myuser/.ssh/id_rsa.pub --upgrade-policy-mode Automatic --instance-count 3 --vnet-name myvnet --subnet mysubnet --lb mylb --backend-pool-name mybackendpool

```

## <a name="configurable-dns-settings"></a>Konfigurovatelná nastavení DNS
Ve výchozím nastavení škálovací sady přebírají konkrétní nastavení DNS virtuální sítě a podsítě, ve kterých byly vytvořeny. Nastavení DNS pro škálovací sadu ale můžete nakonfigurovat i přímo.

### <a name="creating-a-scale-set-with-configurable-dns-servers"></a>Vytvoření škálovací sady s konfigurovatelnými servery DNS
Pokud chcete vytvořit škálovací sadu s vlastní konfigurací DNS pomocí CLI 2.0, přidejte do příkazu _vmss create_ argument --dns-servers následovaný mezerami oddělenými IP adresami serverů. Například:
```bash
--dns-servers 10.0.0.6 10.0.0.5
```
Pokud chcete nakonfigurovat vlastní servery DNS v šabloně Azure, přidejte do části networkInterfaceConfigurations škálovací sady vlastnost dnsSettings. Například:
```json
"dnsSettings":{
    "dnsServers":["10.0.0.6", "10.0.0.5"]
}
```

### <a name="creating-a-scale-set-with-configurable-virtual-machine-domain-names"></a>Vytvoření škálovací sady s konfigurovatelnými názvy domén virtuálních počítačů
Pokud chcete vytvořit škálovací sadu s vlastním názvem DNS pro virtuální počítače pomocí CLI 2.0, přidejte do příkazu _vmss create_ argument _--vm-domain-name_ následovaný řetězcem představujícím název domény.

Pokud chcete nastavit název domény v šabloně Azure, přidejte do části networkInterfaceConfigurations škálovací sady vlastnost dnsSettings. Například:

```json
"networkProfile": {
  "networkInterfaceConfigurations": [
    {
    "name": "nic1",
    "properties": {
      "primary": "true",
      "ipConfigurations": [
      {
        "name": "ip1",
        "properties": {
          "subnet": {
            "id": "[concat('/subscriptions/', subscription().subscriptionId,'/resourceGroups/', resourceGroup().name, '/providers/Microsoft.Network/virtualNetworks/', variables('vnetName'), '/subnets/subnet1')]"
          },
          "publicIPAddressconfiguration": {
            "name": "publicip",
            "properties": {
            "idleTimeoutInMinutes": 10,
              "dnsSettings": {
                "domainNameLabel": "[parameters('vmssDnsName')]"
              }
            }
          }
        }
      }
    ]
    }
}
```

Výstup pro každý jednotlivý název DNS virtuálního počítače by měl následující podobu: 
```
<vmname><vmindex>.<specifiedVmssDomainNameLabel>
```

## <a name="ipv6-preview-for-public-ips-and-load-balancer-pools"></a>Protokol IPv6 ve verzi Preview pro veřejné IP adresy a fondy služby Load Balancer
Ve službě Azure Load Balancer můžete nakonfigurovat veřejnou IP adresu IPv6 a směrovat připojení do back-endových fondů škálovací sady virtuálních počítačů. Pokud chcete použít protokol IPv6, který je aktuálně ve verzi Preview, vytvořte nejprve prostředek veřejné IPv6 adresy. Například:
```json
{
    "apiVersion": "2016-03-30",
    "type": "Microsoft.Network/publicIPAddresses",
    "name": "[parameters('ipv6PublicIPAddressName')]",
    "location": "[parameters('location')]",
    "properties": {
        "publicIPAddressVersion": "IPv6",
        "publicIPAllocationMethod": "Dynamic",
        "dnsSettings": {
            "domainNameLabel": "[parameters('dnsNameforIPv6LbIP')]"
        }
    }
}
```
Dál podle potřeby nakonfigurujte konfigurace IP front-endu vašeho nástroje pro vyrovnávání zařízení pro protokoly IPv4 a IPv6:

```json
"frontendIPConfigurations": [
    {
        "name": "LoadBalancerFrontEndIPv6",
        "properties": {
            "publicIPAddress": {
                "id": "[resourceId('Microsoft.Network/publicIPAddresses',parameters('ipv6PublicIPAddressName'))]"
            }
        }
    }
]
```
Definujte požadované back-endové fondy:
```json
"backendAddressPools": [
    {
        "name": "BackendPoolIPv4"
    },
    {
        "name": "BackendPoolIPv6"
    }
]
```
Definujte případná pravidla nástroje pro vyrovnávání zatížení:
```json
{
    "name": "LBRuleIPv6-46000",
    "properties": {
        "frontendIPConfiguration": {
            "id": "[variables('ipv6FrontEndIPConfigID')]"
        },
        "backendAddressPool": {
            "id": "[variables('ipv6LbBackendPoolID')]"
        },
        "protocol": "tcp",
        "frontendPort": 46000,
        "backendPort": 60001,
        "probe": {
            "id": "[variables('ipv4ipv6lbProbeID')]"
        }
    }
}
```
Nakonec přidejte odkaz na fond IPv6 adres do části IPConfigurations síťových vlastností škálovací sady:
```json
{
    "name": "ipv6IPConfig",
    "properties": {
        "privateIPAddressVersion": "IPv6",
        "loadBalancerBackendAddressPools": [
            {
                "id": "[variables('ipv6LbBackendPoolID')]"
            }
        ]
    }
}
```

## <a name="public-ipv4-per-virtual-machine"></a>Veřejná IPv4 adresa na virtuální počítač
Obecně platí, že virtuální počítače Azure ve škálovací sadě nevyžadují vlastní veřejné IP adresy. Pro většinu scénářů je úspornější a bezpečnější přidružit veřejnou IP adresu k nástroji pro vyrovnávání zatížení nebo samostatnému virtuálnímu počítači (neboli jumpbox), který pak podle potřeby bude směrovat příchozí připojení do virtuálních počítačů ve škálovací sadě (například prostřednictvím příchozích pravidel NAT).

Některé scénáře však vyžadují, aby virtuální počítače ve škálovací sadě měly vlastní veřejné IP adresy. Příkladem jsou hry, kdy konzola potřebuje navázat přímé připojení ke cloudovému virtuálnímu počítači, který provádí zpracování herní fyziky. Dalším příkladem je situace, kdy virtuální počítače mezi sebou potřebují vytvořit externí připojení napříč oblastmi v distribuované databázi.

### <a name="creating-a-scale-set-with-public-ip-per-virtual-machine"></a>Vytvoření škálovací sady s veřejnou IP adresou na virtuální počítač
Pokud chcete pomocí CLI 2.0 vytvořit škálovací sadu, která každému virtuálnímu počítači přiřadí veřejnou IP adresu, přidejte do příkazu _vmss create_ parametr _--public-ip-per-vm_. 

Pokud chcete vytvořit škálovací sadu pomocí šablony Azure, ujistěte se, že verze rozhraní API prostředku Microsoft.Compute/virtualMachineScaleSets je alespoň 2017-03-30, a do části ipConfigurations škálovací sady přidejte vlastnost JSON _publicIpAddressConfiguration_. Například:

```json
"publicIpAddressConfiguration": {
    "name": "pub1",
    "properties": {
      "idleTimeoutInMinutes": 15
    }
}
```
Ukázková šablona: [201-vmss-public-ip-linux](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vmss-public-ip-linux)

### <a name="querying-the-public-ip-addresses-of-the-virtual-machines-in-a-scale-set"></a>Dotazování veřejných IP adres virtuálních počítačů ve škálovací sadě
Pokud chcete zobrazit seznam veřejných IP adres přiřazených k virtuálním počítačům ve škálovací sadě pomocí CLI 2.0, použijte příkaz _az vmss list-instance-public-ips_.

Dotazovat veřejné IP adresy přiřazené k virtuálním počítačům ve škálovací sadě můžete také pomocí [Azure Resource Exploreru](https://resources.azure.com) nebo rozhraní Azure REST API verze _2017-03-30_ nebo vyšší.

Pokud chcete zobrazit veřejné IP adresy pro škálovací sadu pomocí Resource Exploreru, podívejte se do části _publicipaddresses_ v rámci vaší škálovací sady. Například: https://resources.azure.com/subscriptions/_id_vašeho_předplatného_/resourceGroups/_vaše_skupina_prostředků_/providers/Microsoft.Compute/virtualMachineScaleSets/_vaše_škálovací_sada_virtuálních_počítačů_/publicipaddresses

```
GET https://management.azure.com/subscriptions/{your sub ID}/resourceGroups/{RG name}/providers/Microsoft.Compute/virtualMachineScaleSets/{scale set name}/publicipaddresses?api-version=2017-03-30
```

Příklad výstupu:
```json
{
  "value": [
    {
      "name": "pub1",
      "id": "/subscriptions/your-subscription-id/resourceGroups/your-rg/providers/Microsoft.Compute/virtualMachineScaleSets/pipvmss/virtualMachines/0/networkInterfaces/pipvmssnic/ipConfigurations/yourvmssipconfig/publicIPAddresses/pub1",
      "etag": "W/\"a64060d5-4dea-4379-a11d-b23cd49a3c8d\"",
      "properties": {
        "provisioningState": "Succeeded",
        "resourceGuid": "ee8cb20f-af8e-4cd6-892f-441ae2bf701f",
        "ipAddress": "13.84.190.11",
        "publicIPAddressVersion": "IPv4",
        "publicIPAllocationMethod": "Dynamic",
        "idleTimeoutInMinutes": 15,
        "ipConfiguration": {
          "id": "/subscriptions/your-subscription-id/resourceGroups/your-rg/providers/Microsoft.Compute/virtualMachineScaleSets/yourvmss/virtualMachines/0/networkInterfaces/yourvmssnic/ipConfigurations/yourvmssipconfig"
        }
      }
    },
    {
      "name": "pub1",
      "id": "/subscriptions/your-subscription-id/resourceGroups/your-rg/providers/Microsoft.Compute/virtualMachineScaleSets/yourvmss/virtualMachines/3/networkInterfaces/yourvmssnic/ipConfigurations/yourvmssipconfig/publicIPAddresses/pub1",
      "etag": "W/\"5f6ff30c-a24c-4818-883c-61ebd5f9eee8\"",
      "properties": {
        "provisioningState": "Succeeded",
        "resourceGuid": "036ce266-403f-41bd-8578-d446d7397c2f",
        "ipAddress": "13.84.159.176",
        "publicIPAddressVersion": "IPv4",
        "publicIPAllocationMethod": "Dynamic",
        "idleTimeoutInMinutes": 15,
        "ipConfiguration": {
          "id": "/subscriptions/your-subscription-id/resourceGroups/your-rg/providers/Microsoft.Compute/virtualMachineScaleSets/yourvmss/virtualMachines/3/networkInterfaces/yourvmssnic/ipConfigurations/yourvmssipconfig"
        }
      }
    }
```

## <a name="multiple-ip-addresses-per-nic"></a>Několik IP adres na síťové rozhraní
Každé síťové rozhraní připojené k virtuálnímu počítači ve škálovací sadě může mít přidruženu jednu nebo několik konfigurací IP. Každá konfigurace má přiřazenu jednu privátní IP adresu. Každá konfigurace také může mít přiřazen jeden prostředek veřejné IP adresy. Informace o tom, kolik IP adres může mít síťové rozhraní přiřazených a kolik veřejných IP adres můžete použít v předplatném Azure, najdete v [omezeních Azure](../azure-subscription-service-limits.md?toc=%2fazure%2fvirtual-network%2ftoc.json#azure-resource-manager-virtual-networking-limits).

## <a name="multiple-nics-per-virtual-machine"></a>Několik síťových rozhraní na virtuální počítač
Na jeden virtuální počítač můžete mít v závislosti na jeho velikosti až 8 síťových rozhraní. Maximální počet síťových rozhraní na počítač najdete v [článku o velikostech virtuálních počítačů](../virtual-machines/windows/sizes.md). V následujícím příkladu je profil sítě škálovací sady ukazující několik záznamů síťových rozhraní a několik veřejných IP adres na virtuální počítač:
```json
"networkProfile": {
    "networkInterfaceConfigurations": [
        {
        "name": "nic1",
        "properties": {
            "primary": "true",
            "ipConfigurations": [
            {
                "name": "ip1",
                "properties": {
                "subnet": {
                    "id": "[concat('/subscriptions/', subscription().subscriptionId,'/resourceGroups/', resourceGroup().name, '/providers/Microsoft.Network/virtualNetworks/', variables('vnetName'), '/subnets/subnet1')]"
                },
                "publicipaddressconfiguration": {
                    "name": "pub1",
                    "properties": {
                    "idleTimeoutInMinutes": 15
                    }
                },
                "loadBalancerInboundNatPools": [
                    {
                    "id": "[concat('/subscriptions/', subscription().subscriptionId,'/resourceGroups/', resourceGroup().name, '/providers/Microsoft.Network/loadBalancers/', variables('lbName'), '/inboundNatPools/natPool1')]"
                    }
                ],
                "loadBalancerBackendAddressPools": [
                    {
                    "id": "[concat('/subscriptions/', subscription().subscriptionId,'/resourceGroups/', resourceGroup().name, '/providers/Microsoft.Network/loadBalancers/', variables('lbName'), '/backendAddressPools/addressPool1')]"
                    }
                ]
                }
            }
            ]
        }
        },
        {
        "name": "nic2",
        "properties": {
            "primary": "false",
            "ipConfigurations": [
            {
                "name": "ip1",
                "properties": {
                "subnet": {
                    "id": "[concat('/subscriptions/', subscription().subscriptionId,'/resourceGroups/', resourceGroup().name, '/providers/Microsoft.Network/virtualNetworks/', variables('vnetName'), '/subnets/subnet1')]"
                },
                "publicipaddressconfiguration": {
                    "name": "pub1",
                    "properties": {
                    "idleTimeoutInMinutes": 15
                    }
                },
                "loadBalancerInboundNatPools": [
                    {
                    "id": "[concat('/subscriptions/', subscription().subscriptionId,'/resourceGroups/', resourceGroup().name, '/providers/Microsoft.Network/loadBalancers/', variables('lbName'), '/inboundNatPools/natPool1')]"
                    }
                ],
                "loadBalancerBackendAddressPools": [
                    {
                    "id": "[concat('/subscriptions/', subscription().subscriptionId,'/resourceGroups/', resourceGroup().name, '/providers/Microsoft.Network/loadBalancers/', variables('lbName'), '/backendAddressPools/addressPool1')]"
                    }
                ]
                }
            }
            ]
        }
        }
    ]
}
```

## <a name="nsg-per-scale-set"></a>NSG na škálovací sadu
Skupiny zabezpečení sítě se můžou použít přímo na škálovací sadu přidáním odkazu do části konfigurace síťového rozhraní ve vlastnostech virtuálního počítače ve škálovací sadě.

Například: 
```
"networkProfile": {
    "networkInterfaceConfigurations": [
        {
            "name": "nic1",
            "properties": {
                "primary": "true",
                "ipConfigurations": [
                    {
                        "name": "ip1",
                        "properties": {
                            "subnet": {
                                "id": "[concat('/subscriptions/', subscription().subscriptionId,'/resourceGroups/', resourceGroup().name, '/providers/Microsoft.Network/virtualNetworks/', variables('vnetName'), '/subnets/subnet1')]"
                            }
                "loadBalancerInboundNatPools": [
                                {
                                    "id": "[concat('/subscriptions/', subscription().subscriptionId,'/resourceGroups/', resourceGroup().name, '/providers/Microsoft.Network/loadBalancers/', variables('lbName'), '/inboundNatPools/natPool1')]"
                                }
                            ],
                            "loadBalancerBackendAddressPools": [
                                {
                                    "id": "[concat('/subscriptions/', subscription().subscriptionId,'/resourceGroups/', resourceGroup().name, '/providers/Microsoft.Network/loadBalancers/', variables('lbName'), '/backendAddressPools/addressPool1')]"
                                }
                            ]
                        }
                    }
                ],
                "networkSecurityGroup": {
                    "id": "[concat('/subscriptions/', subscription().subscriptionId,'/resourceGroups/', resourceGroup().name, '/providers/Microsoft.Network/networkSecurityGroups/', variables('nsgName'))]"
                }
            }
        }
    ]
}
```

## <a name="next-steps"></a>Další kroky
Další informace o virtuálních sítích Azure najdete v [této dokumentaci](../virtual-network/virtual-networks-overview.md).

