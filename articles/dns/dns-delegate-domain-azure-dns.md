---
title: "Delegování domény do Azure DNS | Dokumentace Microsoftu"
description: "Zjistěte, jak změnit delegování domény a pomocí názvových serverů Azure DNS umožněte hosting domén."
services: dns
documentationcenter: na
author: georgewallace
manager: timlt
ms.assetid: 257da6ec-d6e2-4b6f-ad76-ee2dde4efbcc
ms.service: dns
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 04/12/2017
ms.author: gwallace
ms.openlocfilehash: 33b3ec24432ff1268860b9a2e9d5098600a8dedc
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="delegate-a-domain-to-azure-dns"></a>Delegování domény do Azure DNS

Azure DNS vám umožňuje hostovat zónu DNS a spravovat záznamy DNS pro doménu v Azure. Mají-li se dotazy DNS pro doménu dostat k Azure DNS, musí doména být delegovaná z nadřazené domény do Azure DNS. Pamatujte, že Azure DNS není doménový registrátor. Tento článek vysvětluje, jak delegovat doménu do Azure DNS.

U domén zakoupených od doménového registrátora nabídne registrátor možnost nastavit tyto záznamy NS. Pro vytvoření zóny DNS s názvem domény v DNS Azure nemusíte tuto doménu vlastnit. Chcete-li však u registrátora nastavit delegování domény do Azure DNS, musíte tuto doménu vlastnit.

Předpokládejme například, že zakoupíte doménu contoso.net a v Azure DNS vytvoříte zónu s názvem contoso.net. Jako vlastníkovi domény vám registrátor nabídne možnost konfigurovat pro vaši doménu adresy názvových serverů (tj. záznamů NS). Doménový registrátor uloží tyto záznamy NS v nadřazené doméně, v tomto případě „.net“. Klienti po celém světě pak mohou být při pokusu o překlad záznamů DNS v contoso.net přesměrováni na vaši doménu v zóně Azure DNS.

## <a name="create-a-dns-zone"></a>Vytvoření zóny DNS

1. Přihlášení k webu Azure Portal
1. V nabídce centra klikněte na **Nový > Sítě >** a potom kliknutím na **Zóna DNS** otevřete okno Vytvořit zónu DNS.

    ![Zóna DNS](./media/dns-domain-delegation/dns.png)

1. V okně **Vytvořit zónu DNS** zadejte následující hodnoty a pak klikněte na **Vytvořit**:

   | **Nastavení** | **Hodnota** | **Podrobnosti** |
   |---|---|---|
   |**Název**|contoso.net|Název zóny DNS|
   |**Předplatné**|[Vaše předplatné]|Vyberte předplatné, ve kterém se má služba Application Gateway vytvořit.|
   |**Skupina prostředků**|**Vytvořit novou:** contosoRG|Vytvořte skupinu prostředků. Název skupiny prostředků musí být v rámci vybraného předplatného jedinečný. Další informace o skupinách prostředků najdete v článku s přehledem [Resource Manageru](../azure-resource-manager/resource-group-overview.md?toc=%2fazure%2fdns%2ftoc.json#resource-groups).|
   |**Umístění**|Západní USA||

> [!NOTE]
> Skupina prostředků označuje umístění skupiny prostředků a nemá žádný vliv na zónu DNS. Umístění zóny DNS je vždy globální a není zobrazeno.

## <a name="retrieve-name-servers"></a>Načtení názvových serverů

Předtím, než budete moci svoji zónu DNS delegovat do Azure DNS, musíte znát názvy názvových serverů pro vaši zónu. Azure DNS přiděluje názvové servery z fondu vždy, když je vytvořena zóna.

1. Když máte vytvořenou zónu DNS, na webu Azure Portal v podokně **Oblíbené** klikněte na **Všechny prostředky**. V okně **Všechny prostředky** klikněte na zónu DNS **contoso.net**. Pokud předplatné, které jste vybrali, již obsahovalo nějaké prostředky, můžete zadat **contoso.net** do pole Filtrovat podle názvu... pro snadný přístup ke službě Application Gateway. 

1. Načtěte názvové servery z okna zóny DNS. V tomto případě byly pro zónu contoso.net přiřazené názvové servery ns1-01.azure-dns.com, ns2-01.azure-dns.net, ns3-01.azure-dns.org a ns4-01.azure-dns.info:

 ![Názvový server DNS](./media/dns-domain-delegation/viewzonens500.png)

Azure DNS automaticky vytvoří ve vaší zóně záznamy autoritativních NS, které obsahují přiřazené názvové servery.  Chcete-li zobrazit názvy názvových serverů prostřednictvím Azure PowerShellu nebo rozhraní příkazového řádku Azure, stačí jednoduše načíst tyto záznamy.

Následující příklady popisují také postup načtení názvových serverů pro zónu v Azure DNS pomocí PowerShellu a Azure CLI.

### <a name="powershell"></a>PowerShell

```powershell
# The record name "@" is used to refer to records at the top of the zone.
$zone = Get-AzureRmDnsZone -Name contoso.net -ResourceGroupName contosoRG
Get-AzureRmDnsRecordSet -Name "@" -RecordType NS -Zone $zone
```

Dalším příkladem je tato odpověď.

```
Name              : @
ZoneName          : contoso.net
ResourceGroupName : contosorg
Ttl               : 172800
Etag              : 03bff8f1-9c60-4a9b-ad9d-ac97366ee4d5
RecordType        : NS
Records           : {ns1-07.azure-dns.com., ns2-07.azure-dns.net., ns3-07.azure-dns.org.,
                    ns4-07.azure-dns.info.}
Metadata          :
```

### <a name="azure-cli"></a>Azure CLI

```azurecli
az network dns record-set show --resource-group contosoRG --zone-name contoso.net --type NS --name @
```

Dalším příkladem je tato odpověď.

```json
{
  "etag": "03bff8f1-9c60-4a9b-ad9d-ac97366ee4d5",
  "id": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/contosoRG/providers/Microsoft.Network/dnszones/contoso.net/NS/@",
  "metadata": null,
  "name": "@",
  "nsRecords": [
    {
      "nsdname": "ns1-07.azure-dns.com."
    },
    {
      "nsdname": "ns2-07.azure-dns.net."
    },
    {
      "nsdname": "ns3-07.azure-dns.org."
    },
    {
      "nsdname": "ns4-07.azure-dns.info."
    }
  ],
  "resourceGroup": "contosoRG",
  "ttl": 172800,
  "type": "Microsoft.Network/dnszones/NS"
}
```

## <a name="delegate-the-domain"></a>Delegování domény

Teď, když je vytvořena zóna DNS a máte názvové servery, je potřeba aktualizovat nadřazenou doménu o názvové servery Azure DNS. Každý registrátor má vlastní nástroje pro správu DNS, které umožňují měnit záznamy názvových serverů pro doménu. Na stránce správy DNS vašeho registrátora upravte záznamy NS a nahraďte je záznamy NS, které vytvořil Azure DNS.

Při delegování domény do Azure DNS musíte použít názvy názvových serverů, které poskytuje Azure DNS. Doporučuje se vždycky použít všechny čtyři názvy názvových serverů bez ohledu na název domény. Delegování domény nevyžaduje, aby název názvového serveru používal jako vaši doménu stejnou doménu nejvyšší úrovně.

Pro ukazování na IP adresy názvových serverů Azure DNS byste neměli používat „spojovací záznamy“, protože se tyto IP adresy mohou v budoucnu měnit. Delegování pomocí názvů názvových serverů ve vaší vlastní zóně, někdy označovaných jako „jednoduché názvové servery“, v současné době není v Azure DNS podporované.

## <a name="verify-name-resolution-is-working"></a>Ověření, že překlad názvů funguje

Po dokončení delegování můžete ověřit, že překlad názvů funguje, pomocí nástroje, jako je například nslookup, který se dotáže na záznam SOA pro vaši zónu (ten je také automaticky vytvořený při vytváření zóny).

Nemusíte určit názvové servery Azure DNS, protože pokud bylo delegování správně nastaveno, normální proces překladu DNS najde názvové servery automaticky.

```
nslookup -type=SOA contoso.com
```

Následuje příklad odpovědi z předchozího příkazu:

```
Server: ns1-04.azure-dns.com
Address: 208.76.47.4

contoso.com
primary name server = ns1-04.azure-dns.com
responsible mail addr = msnhst.microsoft.com
serial = 1
refresh = 900 (15 mins)
retry = 300 (5 mins)
expire = 604800 (7 days)
default TTL = 300 (5 mins)
```

## <a name="delegate-sub-domains-in-azure-dns"></a>Delegování subdomén v Azure DNS

Chcete-li nastavit samostatnou podřízenou zónu, můžete subdoménu delegovat v Azure DNS. Předpokládejme například, že jste již v Azure DNS nastavili a delegovali contoso.net, a chtěli byste nastavit samostatnou podřízenou zónu partners.contoso.net.

1. V Azure DNS vytvořte podřízenou zónu partners.contoso.net.
2. Vyhledejte záznamy autoritativních NS v podřízené zóně a získejte tak názvové servery hostující podřízenou zónu v Azure DNS.
3. Delegujte podřízenou zónu pomocí konfigurace záznamů NS v nadřazené zóně tak, aby ukazovaly na podřízenou zónu.

### <a name="create-a-dns-zone"></a>Vytvoření zóny DNS

1. Přihlášení k webu Azure Portal
1. V nabídce centra klikněte na **Nový > Sítě >** a potom kliknutím na **Zóna DNS** otevřete okno Vytvořit zónu DNS.

    ![Zóna DNS](./media/dns-domain-delegation/dns.png)

1. V okně **Vytvořit zónu DNS** zadejte následující hodnoty a pak klikněte na **Vytvořit**:

   | **Nastavení** | **Hodnota** | **Podrobnosti** |
   |---|---|---|
   |**Název**|partners.contoso.net|Název zóny DNS|
   |**Předplatné**|[Vaše předplatné]|Vyberte předplatné, ve kterém se má služba Application Gateway vytvořit.|
   |**Skupina prostředků**|**Použít existující:** contosoRG|Vytvořte skupinu prostředků. Název skupiny prostředků musí být v rámci vybraného předplatného jedinečný. Další informace o skupinách prostředků najdete v článku s přehledem [Resource Manageru](../azure-resource-manager/resource-group-overview.md?toc=%2fazure%2fdns%2ftoc.json#resource-groups).|
   |**Umístění**|Západní USA||

> [!NOTE]
> Skupina prostředků označuje umístění skupiny prostředků a nemá žádný vliv na zónu DNS. Umístění zóny DNS je vždy globální a není zobrazeno.

### <a name="retrieve-name-servers"></a>Načtení názvových serverů

1. Když máte vytvořenou zónu DNS, na webu Azure Portal v podokně **Oblíbené** klikněte na **Všechny prostředky**. V okně **Všechny prostředky** klikněte na zónu DNS **partners.contoso.net**. Pokud předplatné, které jste vybrali, již obsahovalo nějaké prostředky, můžete zadat **partners.contoso.net** do pole Filtrovat podle názvu... pro snadný přístup k zóně DNS.

1. Načtěte názvové servery z okna zóny DNS. V tomto případě byly pro zónu contoso.net přiřazené názvové servery ns1-01.azure-dns.com, ns2-01.azure-dns.net, ns3-01.azure-dns.org a ns4-01.azure-dns.info:

 ![Názvový server DNS](./media/dns-domain-delegation/viewzonens500.png)

Azure DNS automaticky vytvoří ve vaší zóně záznamy autoritativních NS, které obsahují přiřazené názvové servery.  Chcete-li zobrazit názvy názvových serverů prostřednictvím Azure PowerShellu nebo rozhraní příkazového řádku Azure, stačí jednoduše načíst tyto záznamy.

### <a name="create-name-server-record-in-parent-zone"></a>Vytvoření záznamu názvového serveru v nadřazené zóně

1. Na webu Azure Portal přejděte k zóně DNS **contoso.net**.
1. Klikněte na **+ Sada záznamů**.
1. V okně **Přidat sadu záznamů** zadejte následující hodnoty a klikněte na **OK**:

   | **Nastavení** | **Hodnota** | **Podrobnosti** |
   |---|---|---|
   |**Název**|partners|Název podřízené zóny DNS|
   |**Typ**|NS|Pro záznamy názvového serveru použijte NS.|
   |**Hodnota TTL**|1|Hodnota TTL (Time to Live).|
   |**Jednotka hodnoty TTL**|Hodiny|Nastaví jednotku hodnoty TTL (Time to Live) na hodiny.|
   |**NÁZVOVÝ SERVER**|{názvové servery ze zóny partners.contoso.net}|Zadejte všechny 4 názvové servery ze zóny partners.contoso.net. |

   ![Názvový server DNS](./media/dns-domain-delegation/partnerzone.png)


### <a name="delegating-sub-domains-in-azure-dns-with-other-tools"></a>Delegování subdomén v Azure DNS pomocí jiných nástrojů

Následující příklady popisují postup delegování subdomén v Azure DNS pomocí PowerShellu a rozhraní příkazového řádku:

#### <a name="powershell"></a>PowerShell

Následující příklad PowerShell ukazuje, jak to funguje. Stejný postup lze provést prostřednictvím webu Azure Portal nebo víceplatformového rozhraní příkazového řádku Azure CLI.

```powershell
# Create the parent and child zones. These can be in same resource group or different resource groups as Azure DNS is a global service.
$parent = New-AzureRmDnsZone -Name contoso.net -ResourceGroupName contosoRG
$child = New-AzureRmDnsZone -Name partners.contoso.net -ResourceGroupName contosoRG

# Retrieve the authoritative NS records from the child zone as shown in the next example. This contains the name servers assigned to the child zone.
$child_ns_recordset = Get-AzureRmDnsRecordSet -Zone $child -Name "@" -RecordType NS

# Create the corresponding NS record set in the parent zone to complete the delegation. The record set name in the parent zone matches the child zone name, in this case "partners".
$parent_ns_recordset = New-AzureRmDnsRecordSet -Zone $parent -Name "partners" -RecordType NS -Ttl 3600
$parent_ns_recordset.Records = $child_ns_recordset.Records
Set-AzureRmDnsRecordSet -RecordSet $parent_ns_recordset
```

Pomocí rutiny `nslookup` můžete vyhledat záznam SOA podřízené zóny a ověřit tak, že je všechno správně nastavené.

```
nslookup -type=SOA partners.contoso.com
```

```
Server: ns1-08.azure-dns.com
Address: 208.76.47.8

partners.contoso.com
    primary name server = ns1-08.azure-dns.com
    responsible mail addr = msnhst.microsoft.com
    serial = 1
    refresh = 900 (15 mins)
    retry = 300 (5 mins)
    expire = 604800 (7 days)
    default TTL = 300 (5 mins)
```

#### <a name="azure-cli"></a>Azure CLI

```azurecli
#!/bin/bash

# Create the parent and child zones. These can be in same resource group or different resource groups as Azure DNS is a global service.
az network dns zone create -g contosoRG -n contoso.net
az network dns zone create -g contosoRG -n partners.contoso.net
```

Načtěte z výstupu názvové servery pro zónu `partners.contoso.net`.

```
{
  "etag": "00000003-0000-0000-418f-250de2b2d201",
  "id": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/contosorg/providers/Microsoft.Network/dnszones/partners.contoso.net",
  "location": "global",
  "maxNumberOfRecordSets": 5000,
  "name": "partners.contoso.net",
  "nameServers": [
    "ns1-09.azure-dns.com.",
    "ns2-09.azure-dns.net.",
    "ns3-09.azure-dns.org.",
    "ns4-09.azure-dns.info."
  ],
  "numberOfRecordSets": 2,
  "resourceGroup": "contosorg",
  "tags": {},
  "type": "Microsoft.Network/dnszones"
}
```

Vytvořte pro každý názvový server sadu záznamů a záznamy NS.

```azurecli
#!/bin/bash

# Create the record set
az network dns record-set ns create --resource-group contosorg --zone-name contoso.net --name partners

# Create a ns record for each name server.
az network dns record-set ns add-record --resource-group contosorg --zone-name contoso.net --record-set-name partners --nsdname ns1-09.azure-dns.com.
az network dns record-set ns add-record --resource-group contosorg --zone-name contoso.net --record-set-name partners --nsdname ns2-09.azure-dns.net.
az network dns record-set ns add-record --resource-group contosorg --zone-name contoso.net --record-set-name partners --nsdname ns3-09.azure-dns.org.
az network dns record-set ns add-record --resource-group contosorg --zone-name contoso.net --record-set-name partners --nsdname ns4-09.azure-dns.info.
```

## <a name="delete-all-resources"></a>Odstranění všech prostředků

Pokud chcete odstranit všechny prostředky vytvořené v rámci tohoto článku, proveďte následující kroky:

1. Na webu Azure Portal v podokně **Oblíbené** klikněte na **Všechny prostředky**. V okně Všechny prostředky klikněte na skupinu prostředků **contosorg**. Pokud předplatné, které jste vybrali, již obsahovalo nějaké prostředky, můžete zadat **contosorg** do pole **Filtrovat podle názvu...** pro snadný přístup ke skupině prostředků.
1. V okně **contosorg** klikněte na tlačítko **Odstranit**.
1. Portál požaduje, abyste zadali název skupiny prostředků pro potvrzení, že ji skutečně chcete odstranit. Jako název skupiny prostředků zadejte *contosorg* a pak klikněte na **Odstranit**. Odstraněním skupiny prostředků se odstraní všechny prostředky v rámci dané skupiny prostředků. Proto nikdy nezapomeňte před odstraněním skupiny prostředků zkontrolovat její obsah. Portál odstraní všechny prostředky v rámci skupiny prostředků a potom odstraní samotnou skupinu prostředků. Tento proces trvá několik minut.

## <a name="next-steps"></a>Další kroky

[Správa zón DNS](dns-operations-dnszones.md)

[Správa záznamů DNS](dns-operations-recordsets.md)
