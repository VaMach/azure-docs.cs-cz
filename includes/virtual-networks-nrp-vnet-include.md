## <a name="virtual-network"></a>Virtual Network
Virtuální sítí (VNET) a podsítě prostředky pomohl definovat hranici zabezpečení pro úlohy běžící v Azure. Virtuální síť je charakterizovaná kolekce adresní prostory, které jsou definované jako bloků CIDR. 

> [!NOTE]
> Správci sítě se seznámíte s notaci CIDR. Pokud nejste obeznámeni s CIDR, [Další informace o](http://whatismyipaddress.com/cidr).
> 
> 

![Virtuální síť s více podsítěmi](./media/resource-groups-networking/Figure4.png)

Virtuální sítě obsahují následující vlastnosti.

| Vlastnost | Popis | Ukázkové hodnoty |
| --- | --- | --- |
| **adresní prostor** |Kolekce předpon adres, které tvoří virtuální sítě v notaci CIDR |192.168.0.0/16 |
| **podsítě** |Kolekce podsítě, které tvoří virtuální sítě |v tématu [podsítě](#Subnets) níže. |
| **IP adresa** |Přiřazené objektu IP adresy. Toto je vlastnost jen pro čtení. |104.42.233.77 |

### <a name="subnets"></a>Podsítě
Podsíť je prostředkem podřízené virtuální sítě, a pomáhá definovat segmenty adresní prostory v rámci blok CIDR pomocí předpony IP adres. Síťové adaptéry můžete přidat do podsítí a připojení k virtuálním počítačům, poskytuje připojení pro různé úlohy.

Podsítě obsahují následující vlastnosti. 

| Vlastnost | Popis | Ukázkové hodnoty |
| --- | --- | --- |
| **addressPrefix** |Jedna adresa předponu, která tvoří podsíť v notaci CIDR |192.168.1.0/24 |
| **skupinu zabezpečení sítě** |Skupina NSG použije na podsíť |v tématu [skupiny Nsg](#Network-Security-Group) |
| **routeTable** |Směrovací tabulka použije na podsíť |v tématu [UDR](#Route-table) |
| **Konfigurace IP adresy** |Kolekce objektů configruation IP používané síťové adaptéry připojené k podsíti |v tématu [UDR](#Route-table) |

Ukázka VNet ve formátu JSON:

    {
        "name": "TestVNet",
        "id": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/TestRG/providers/Microsoft.Network/virtualNetworks/TestVNet",
        "etag": "W/\"xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx\"",
        "type": "Microsoft.Network/virtualNetworks",
        "location": "westus",
        "tags": {
            "displayName": "VNet"
        },
        "properties": {
            "provisioningState": "Succeeded",
            "resourceGuid": "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx",
            "addressSpace": {
                "addressPrefixes": [
                    "192.168.0.0/16"
                ]
            },
            "subnets": [
                {
                    "name": "FrontEnd",
                    "id": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/TestRG/providers/Microsoft.Network/virtualNetworks/TestVNet/subnets/FrontEnd",
                    "etag": "W/\"xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx\"",
                    "properties": {
                        "provisioningState": "Succeeded",
                        "addressPrefix": "192.168.1.0/24",
                        "networkSecurityGroup": {
                            "id": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/TestRG/providers/Microsoft.Network/networkSecurityGroups/NSG-BackEnd"
                        },
                        "routeTable": {
                            "id": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/TestRG/providers/Microsoft.Network/routeTables/UDR-FrontEnd"
                        },
                        "ipConfigurations": [
                            {
                                "id": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/TestRG/providers/Microsoft.Network/networkInterfaces/NICWEB1/ipConfigurations/ipconfig1"
                            },
                            ...]
                    }
                },
                ...]
        }
    }

### <a name="additional-resources"></a>Další zdroje
* Přečtěte si další informace o [VNet](../articles/virtual-network/virtual-networks-overview.md).
* Pro čtení [referenční dokumentace rozhraní API REST](https://msdn.microsoft.com/library/azure/mt163650.aspx) pro virtuální sítě.
* Pro čtení [referenční dokumentace rozhraní API REST](https://msdn.microsoft.com/library/azure/mt163618.aspx) pro podsítě.

