## <a name="public-ip-address"></a>Veřejná IP adresa
Prostředek veřejné IP adresy obsahuje buď vyhrazené nebo dynamické internetové IP adresu. I když můžete vytvořit veřejnou IP adresu jako samostatný objekt, je třeba ji přidružit k jinému objektu skutečně použít adresu. Můžete přidružit veřejnou IP adresu, která nástroj pro vyrovnávání zatížení, aplikační bránu nebo síťový adaptér zajistit přístup k Internetu na tyto prostředky.  

| Vlastnost | Popis | Ukázkové hodnoty |
| --- | --- | --- |
| **publicIPAllocationMethod** |Určuje, zda je adresa IP *statické* nebo *dynamické*. |static, dynamické |
| **idleTimeoutInMinutes** |Definuje časový limit nečinnosti, s výchozí hodnotou 4 minuty. Pokud v tuto chvíli je přijatá žádné další pakety pro dané relace, relace je ukončena. |Libovolná hodnota od 4 do 30. |
| **IP adresa** |Přiřazené objektu IP adresy. Toto je vlastnost jen pro čtení. |104.42.233.77 |

### <a name="dns-settings"></a>Nastavení DNS
Veřejné IP adresy mít podřízený objekt s názvem **dnsSettings** obsahující následující vlastnosti:

| Vlastnost | Popis | Ukázkové hodnoty |
| --- | --- | --- |
| **domainNameLabel** |Hostitel s názvem používají pro překlad. |WWW, ftp, vm1 |
| **plně kvalifikovaný název domény** |Plně kvalifikovaný název pro veřejnou IP adresu. |www.westus.cloudapp.Azure.com |
| **reverseFqdn** |Plně kvalifikovaný název domény na IP adresu a je zaregistrován ve službě DNS, jako záznam PTR. |www.contoso.com. |

Ukázka veřejnou IP adresu ve formátu JSON:

    {
       "name": "PIP01",
       "location": "North US",
       "tags": { "key": "value" },
       "properties": {
          "publicIPAllocationMethod": "Static",
          "idleTimeoutInMinutes": 4,
          "ipAddress": "104.42.233.77",
          "dnsSettings": {
             "domainNameLabel": "mylabel",
             "fqdn": "mylabel.westus.cloudapp.azure.com",
             "reverseFqdn": "contoso.com."
          }
       }
    } 

### <a name="additional-resources"></a>Další zdroje
* Přečtěte si další informace o [veřejné IP adresy](../articles/virtual-network/virtual-networks-reserved-public-ip.md).
* Další informace o [instance úrovně veřejné IP adresy](../articles/virtual-network/virtual-networks-instance-level-public-ip.md).
* Pro čtení [referenční dokumentace rozhraní API REST](https://msdn.microsoft.com/library/azure/mt163638.aspx) pro veřejné IP adresy.

