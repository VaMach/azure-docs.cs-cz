## <a name="network-security-group"></a>Skupina zabezpečení sítě
Prostředek NSG umožňuje vytvoření hranice zabezpečení pro úlohy, implementací povolit a zakázat pravidla. Tato pravidla lze použít pro virtuální počítač, síťový adaptér nebo podsíť.

| Vlastnost | Popis | Ukázkové hodnoty |
| --- | --- | --- |
| **podsítě** |Seznam ID podsítě, NSG se použije na. |/subscriptions/XXXXXXXX-XXXX-XXXX-XXXX-XXXXXXXXXXXX/resourceGroups/TestRG/providers/Microsoft.Network/virtualNetworks/TestVNet/subnets/FrontEnd |
| **securityRules** |Seznam pravidel zabezpečení, které tvoří NSG |V tématu [pravidlo zabezpečení](#Security-rule) níže |
| **defaultSecurityRules** |Seznam výchozích pravidel zabezpečení, které jsou v každé skupiny NSG |V tématu [výchozí pravidla zabezpečení](#Default-security-rules) níže |

* **Pravidlo zabezpečení** -NSG můžete mít je definováno více pravidel zabezpečení. Každé pravidlo můžete povolit nebo odepřít různé typy provozu.

### <a name="security-rule"></a>Pravidlo zabezpečení
Pravidlo zabezpečení je prostředkem podřízenou skupinu NSG obsahující vlastnosti níže.

| Vlastnost | Popis | Ukázkové hodnoty |
| --- | --- | --- |
| **Popis** |Popis pravidla |Povolí příchozí komunikaci pro všechny virtuální počítače v podsíti X |
| **protokol** |Protokol, který je nutné u pravidla splnit |TCP, UDP nebo * |
| **sourcePortRange** |Rozsah zdrojových portů, který je nutné u pravidla splnit |80, 100-200, * |
| **destinationPortRange** |Rozsah cílových portů, který je nutné u pravidla splnit |80, 100-200, * |
| **sourceAddressPrefix** |Předpona zdrojové adresy tak, aby odpovídaly pro pravidlo |10.10.10.1 10.10.10.0/24, virtuální síť |
| **destinationAddressPrefix** |Předpona cílové adresy tak, aby odpovídaly pro pravidlo |10.10.10.1 10.10.10.0/24, virtuální síť |
| **směr** |Směr provozu, který je nutné u pravidla splnit |Příchozí nebo odchozí |
| **Priorita** |Priorita pravidla. Pravidla se kontrolují v pořadí podle priority, jakmile se pravidlo vztahuje, žádná další pravidla se již nekontrolují. |10, 100, 65000 |
| **přístup** |Typ přístupu, který se použije v případě splnění pravidla |Povolit nebo odepřít |

Ukázka NSG ve formátu JSON:

    {
        "name": "NSG-BackEnd",
        "id": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/TestRG/providers/Microsoft.Network/networkSecurityGroups/NSG-BackEnd",
        "etag": "W/\"xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx\"",
        "type": "Microsoft.Network/networkSecurityGroups",
        "location": "westus",
        "tags": {
            "displayName": "NSG - Front End"
        },
        "properties": {
            "provisioningState": "Succeeded",
            "resourceGuid": "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx",
            "securityRules": [
                {
                    "name": "rdp-rule",
                    "id": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/TestRG/providers/Microsoft.Network/networkSecurityGroups/NSG-BackEnd/securityRules/rdp-rule",
                    "etag": "W/\"xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx\"",
                    "properties": {
                        "provisioningState": "Succeeded",
                        "description": "Allow RDP",
                        "protocol": "Tcp",
                        "sourcePortRange": "*",
                        "destinationPortRange": "3389",
                        "sourceAddressPrefix": "Internet",
                        "destinationAddressPrefix": "*",
                        "access": "Allow",
                        "priority": 100,
                        "direction": "Inbound"
                    }
                }
            ],
            "defaultSecurityRules": [
                { [...],
            "subnets": [
                {
                    "id": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/TestRG/providers/Microsoft.Network/virtualNetworks/TestVNet/subnets/FrontEnd"
                }
            ]
        }
    }

### <a name="default-security-rules"></a>Výchozí pravidla zabezpečení

Výchozí zabezpečení pravidla mají stejné vlastnosti, které jsou k dispozici v pravidla zabezpečení. Existují poskytují základní připojení mezi prostředky, které mají skupiny Nsg na ně použity. Ujistěte se, které znáte [výchozí pravidla zabezpečení](../articles/virtual-network/virtual-networks-nsg.md#default-rules) neexistuje.

### <a name="additional-resources"></a>Další zdroje
* Přečtěte si další informace o [skupiny Nsg](../articles/virtual-network/virtual-networks-nsg.md).
* Pro čtení [referenční dokumentace rozhraní API REST](https://msdn.microsoft.com/library/azure/mt163615.aspx) pro skupiny Nsg.
* Pro čtení [referenční dokumentace rozhraní API REST](https://msdn.microsoft.com/library/azure/mt163580.aspx) pro pravidla zabezpečení.
