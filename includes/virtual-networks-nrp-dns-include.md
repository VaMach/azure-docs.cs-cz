## <a name="azure-dns"></a>Azure DNS
Azure DNS je hostitelská služba domén DNS poskytnutí překladu názvů pomocí infrastruktury Microsoft Azure.

| Vlastnost | Popis | Hodnota vzorku |
| --- | --- | --- |
| **DNSzones** |Domény informace o zóně záznamy hostitele DNS určité domény |/ subscriptions/{guid}/.../providers/Microsoft.Network/dnszones/contoso.com " |

### <a name="dns-record-sets"></a>Sady záznamů DNS
Zóny DNS mít podřízený objekt s názvem sadu záznamů. Sady záznamů jsou kolekce záznamy hostitele podle typu zóny DNS. Typy záznamů jsou A, AAAA, CNAME, MX, NS, SOA, SRV a TXT.

| Vlastnost | Popis | Ukázková hodnota |
| --- | --- | --- |
| A |Typ záznamu IPv4 |/subscriptions/{GUID}/.../Providers/Microsoft.Network/dnszones/contoso.com/A/www |
| AAAA |Typ záznamu IPv6 |/subscriptions/{GUID}/.../Providers/Microsoft.Network/dnszones/contoso.com/AAAA/hostrecord |
| CNAME |Typ záznamu kanonický název <sup>1</sup> |/subscriptions/{GUID}/.../Providers/Microsoft.Network/dnszones/contoso.com/CNAME/www |
| MX |Typ záznamu e-mailu |/subscriptions/{GUID}/.../Providers/Microsoft.Network/dnszones/contoso.com/MX/mail |
| NS |Typ záznamu název serveru |/subscriptions/{GUID}/.../Providers/Microsoft.Network/dnszones/contoso.com/NS/ |
| SOA |Začátek typ záznamu autority <sup>2</sup> |/subscriptions/{GUID}/.../Providers/Microsoft.Network/dnszones/contoso.com/SOA |
| SRV |Typ záznamu služby |/subscriptions/{GUID}/.../Providers/Microsoft.Network/dnszones/contoso.com/SRV |

<sup>1</sup> umožňuje pouze jednu hodnotu na sady záznamů.

<sup>2</sup> umožňuje pouze jeden typ záznamu SOA za zóny DNS. 

Ukázka zónu DNS ve formátu Json:

    {
      "$schema": "http://schema.management.azure.com/schemas/2014-04-01-preview/deploymentTemplate.json",
      "contentVersion": "1.0.0.0",
      "parameters": {
        "newZoneName": {
          "type": "String",
          "metadata": {
              "description": "The name of the DNS zone to be created."
          }
        },
        "newRecordName": {
          "type": "String",
          "defaultValue": "www",
          "metadata": {
              "description": "The name of the DNS record to be created.  The name is relative to the zone, not the FQDN."
          }
        }
      },
      "resources": 
      [
        {
          "type": "microsoft.network/dnszones",
          "name": "[parameters('newZoneName')]",
          "apiVersion": "2015-05-04-preview",
          "location": "global",
          "properties": {
          }
        },
        {
          "type": "microsoft.network/dnszones/a",
          "name": "[concat(parameters('newZoneName'), concat('/', parameters('newRecordName')))]",
          "apiVersion": "2015-05-04-preview",
          "location": "global",
          "properties": 
          {
            "TTL": 3600,
            "ARecords": 
            [
                {
                    "ipv4Address": "1.2.3.4"
                },
                {
                    "ipv4Address": "1.2.3.5"
                }
            ]
          },
          "dependsOn": [
            "[concat('Microsoft.Network/dnszones/', parameters('newZoneName'))]"
          ]
        }
          ]
    }

## <a name="additional-resources"></a>Další zdroje
Pro čtení [dokumentace k REST API pro zóny DNS ](https://msdn.microsoft.com/library/azure/mt130626.aspx) Další informace.

Pro čtení [dokumentace k REST API pro sady záznamů DNS](https://msdn.microsoft.com/library/azure/mt130627.aspx) Další informace.

