## <a name="nic"></a>SÍŤOVÝ ADAPTÉR
Karta (NIC) prostředku síťového rozhraní poskytuje síťové připojení k existující podsíť virtuální sítě prostředku. I když můžete vytvořit síťový adaptér jako samostatný objekt, je třeba ji přidružit k jinému objektu ve skutečnosti poskytovat připojení. Síťový adaptér slouží k připojení virtuálního počítače na podsíť, veřejnou IP adresu nebo Vyrovnávání zatížení.  

| Vlastnost | Popis | Ukázkové hodnoty |
| --- | --- | --- |
| **virtuální počítač** |Virtuální počítač na síťový adaptér je přidružen. |/subscriptions/{GUID}/../microsoft.COMPUTE/virtualMachines/vm1 |
| **macAddress** |Adresa MAC pro síťový adaptér |Libovolná hodnota od 4 do 30. |
| **skupinu zabezpečení sítě** |NSG přidruženou na síťový adaptér |/subscriptions/{GUID}/../microsoft.Network/networkSecurityGroups/myNSG1 |
| **dnsSettings** |Nastavení DNS pro síťový adaptér |v tématu [PIP](#Public-IP-address) |

Síťový adaptér nebo síťový adaptér představuje síťové rozhraní, které lze přidružit k virtuálnímu počítači (VM). Virtuální počítač může mít jeden nebo více síťových adaptérů.

![Karty síťového rozhraní na jeden virtuální počítač](./media/resource-groups-networking/Figure3.png)

### <a name="ip-configurations"></a>Konfigurace protokolu IP
Síťové adaptéry mít podřízený objekt s názvem **ipConfigurations** obsahující následující vlastnosti:

| Vlastnost | Popis | Ukázkové hodnoty |
| --- | --- | --- |
| **podsíť** |Podsíť síťového adaptéru je onnected k. |/subscriptions/{GUID}/../microsoft.Network/virtualNetworks/myvnet1/subnets/mysub1 |
| **privateIPAddress** |IP adresu pro síťový adaptér v podsíti |10.0.0.8 |
| **privateIPAllocationMethod** |Metoda přidělení IP |Dynamická nebo statická |
| **enableIPForwarding** |Jestli se dá použít na síťový adaptér pro směrování |hodnotu true nebo false |
| **primární** |Jestli síťový adaptér je primární síťový adaptér pro virtuální počítač. |hodnotu true nebo false |
| **publicIPAddress** |PIP přidružené síťového adaptéru |v tématu [nastavení DNS](#DNS-settings) |
| **pravidlo loadBalancerBackendAddressPools** |Back end fondy adres, který je přidružen síťový adaptér | |
| **loadBalancerInboundNatRules** |Příchozí pravidla NAT nástroje pro vyrovnávání zatížení, síťový adaptér je spojené s | |

Ukázka veřejnou IP adresu ve formátu JSON:

    {
        "name": "lb-nic1-be",
        "id": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/nrprg/providers/Microsoft.Network/networkInterfaces/lb-nic1-be",
        "etag": "W/\"xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx\"",
        "type": "Microsoft.Network/networkInterfaces",
        "location": "eastus",
        "properties": {
            "provisioningState": "Succeeded",
            "resourceGuid": "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx",
            "ipConfigurations": [
                {
                    "name": "NIC-config",
                    "id": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/nrprg/providers/Microsoft.Network/networkInterfaces/lb-nic1-be/ipConfigurations/NIC-config",
                    "etag": "W/\"0027f1a2-3ac8-49de-b5d5-fd46550500b1\"",
                    "properties": {
                        "provisioningState": "Succeeded",
                        "privateIPAddress": "10.0.0.4",
                        "privateIPAllocationMethod": "Dynamic",
                        "subnet": {
                            "id": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/NRPRG/providers/Microsoft.Network/virtualNetworks/NRPVnet/subnets/NRPVnetSubnet"
                        },
                        "loadBalancerBackendAddressPools": [
                            {
                                "id": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/nrprg/providers/Microsoft.Network/loadBalancers/nrplb/backendAddressPools/NRPbackendpool"
                            }
                        ],
                        "loadBalancerInboundNatRules": [
                            {
                                "id": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/nrprg/providers/Microsoft.Network/loadBalancers/nrplb/inboundNatRules/rdp1"
                            }
                        ]
                    }
                }
            ],
            "dnsSettings": { ... },
            "macAddress": "00-0D-3A-10-F1-29",
            "enableIPForwarding": false,
            "primary": true,
            "virtualMachine": {
                "id": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/nrprg/providers/Microsoft.Compute/virtualMachines/web1"
            }
        }
    }

### <a name="additional-resources"></a>Další zdroje
* Pro čtení [referenční dokumentace rozhraní API REST](https://msdn.microsoft.com/library/azure/mt163579.aspx) pro síťové adaptéry.

