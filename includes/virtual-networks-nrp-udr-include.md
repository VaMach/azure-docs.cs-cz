## <a name="route-tables"></a>Směrovací tabulky
Prostředky tabulky trasy obsahuje trasy používá k definování tok provozu v rámci infrastruktury Azure. Trasy definované uživatelem (UDR) můžete použít k odesílání veškerý provoz z dané podsíti do virtuální zařízení, například Brána firewall nebo neoprávněných vniknutí systém detekce (ID). Můžete přidružit směrovací tabulku k podsítím. 

Směrovací tabulky obsahují následující vlastnosti.

| Vlastnost | Popis | Ukázkové hodnoty |
| --- | --- | --- |
| **trasy** |Kolekce uživatelem definovaných tras ve směrovací tabulce |v tématu [trasy definované uživatelem](#User-defined-routes) |
| **podsítě** |Kolekce podsítě, směrovací tabulka se použije na |v tématu [podsítě](#Subnets) |

### <a name="user-defined-routes"></a>Trasy definované uživatelem
Udr k určení, kde má být odeslán provoz, můžete vytvořit na základě jeho cílové adresy. Trasu si můžete představit jako výchozí definici brány na základě cílové adresy síťového paketu.

Udr obsahují následující vlastnosti. 

| Vlastnost | Popis | Ukázkové hodnoty |
| --- | --- | --- |
| **addressPrefix** |Předpona adresy nebo úplné IP adresu pro cíl |192.168.1.0/24, 192.168.1.101 |
| **nextHopType** |Typ zařízení, které bude odeslán provoz |VirtualAppliance, brány sítě VPN, Internet |
| **nextHopIpAddress** |IP adresa dalšího směrování |192.168.1.4 |

Ukázka směrovací tabulku ve formátu JSON:

    {
        "name": "UDR-BackEnd",
        "id": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/TestRG/providers/Microsoft.Network/routeTables/UDR-BackEnd",
        "etag": "W/\"xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx\"",
        "type": "Microsoft.Network/routeTables",
        "location": "westus",
        "properties": {
            "provisioningState": "Succeeded",
            "routes": [
                {
                    "name": "RouteToFrontEnd",
                    "id": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/TestRG/providers/Microsoft.Network/routeTables/UDR-BackEnd/routes/RouteToFrontEnd",
                    "etag": "W/\"v\"",
                    "properties": {
                        "provisioningState": "Succeeded",
                        "addressPrefix": "192.168.1.0/24",
                        "nextHopType": "VirtualAppliance",
                        "nextHopIpAddress": "192.168.0.4"
                    }
                }
            ],
            "subnets": [
                {
                    "id": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/TestRG/providers/Microsoft.Network/virtualNetworks/TestVNet/subnets/BackEnd"
                }
            ]
        }
    }

### <a name="additional-resources"></a>Další zdroje
* Přečtěte si další informace o [udr](../articles/virtual-network/virtual-networks-udr-overview.md).
* Pro čtení [referenční dokumentace rozhraní API REST](https://msdn.microsoft.com/library/azure/mt502549.aspx) pro směrovací tabulky.
* Pro čtení [referenční dokumentace rozhraní API REST](https://msdn.microsoft.com/library/azure/mt502539.aspx) pro uživatele definované trasy (udr).

