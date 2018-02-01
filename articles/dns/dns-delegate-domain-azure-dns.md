---
title: "Delegování domény do Azure DNS | Dokumentace Microsoftu"
description: "Zjistěte, jak změnit delegování domény a pomocí názvových serverů Azure DNS umožněte hosting domén."
services: dns
documentationcenter: na
author: KumudD
manager: jeconnoc
ms.assetid: 257da6ec-d6e2-4b6f-ad76-ee2dde4efbcc
ms.service: dns
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 12/18/2017
ms.author: kumud
ms.openlocfilehash: a13d9b360e2c43aa2a3d4f62215e4570ce42cd5b
ms.sourcegitcommit: 28178ca0364e498318e2630f51ba6158e4a09a89
ms.translationtype: HT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/24/2018
---
# <a name="delegate-a-domain-to-azure-dns"></a>Delegování domény do Azure DNS

Azure DNS můžete použít k hostování zóny DNS a správě záznamů DNS pro doménu v Azure. Aby se dotazy DNS pro doménu dostaly až k Azure DNS, musí být doména delegovaná do Azure DNS z nadřazené domény. Pamatujte, že Azure DNS není doménový registrátor. Tento článek vysvětluje, jak delegovat doménu do Azure DNS.

U domén zakoupených od doménového registrátora nabídne registrátor možnost nastavit záznamy NS (názvových serverů). Pro vytvoření zóny DNS s daným názvem domény v Azure DNS nemusíte tuto doménu vlastnit. Chcete-li však u registrátora nastavit delegování domény do Azure DNS, musíte tuto doménu vlastnit.

Předpokládejme například, že zakoupíte doménu contoso.net a v Azure DNS vytvoříte zónu s názvem contoso.net. Jako vlastníkovi domény vám registrátor nabídne možnost konfigurovat pro vaši doménu adresy názvových serverů (tj. záznamů NS). Doménový registrátor uloží tyto záznamy NS v nadřazené doméně, v tomto případě .net. Klienti po celém světě pak mohou být při pokusu o překlad adresy z domény contoso.net pomocí DNS přesměrováni na vaši doménu v zóně Azure DNS.

## <a name="create-a-dns-zone"></a>Vytvoření zóny DNS

1. Přihlaste se k portálu Azure.
1. V nabídce **Centrum** vyberte **Nový** > **Sítě** > **Zóna DNS**. Otevře se stránka **Vytvořit zónu DNS**.

   ![Zóna DNS](./media/dns-domain-delegation/dns.png)

1. Na stránce **Vytvořit zónu DNS** zadejte následující hodnoty a pak vyberte **Vytvořit**:

   | **Nastavení** | **Hodnota** | **Podrobnosti** |
   |---|---|---|
   |**Název**|contoso.net|Zadejte název zóny DNS.|
   |**Předplatné**|[Vaše předplatné]|Vyberte předplatné, ve kterém se má služba Application Gateway vytvořit.|
   |**Skupina prostředků**|**Vytvořit novou:** contosoRG|Vytvořte skupinu prostředků. Název skupiny prostředků musí být v rámci vybraného předplatného jedinečný. Další informace o skupinách prostředků najdete v přehledovém článku [Azure Resource Manager](../azure-resource-manager/resource-group-overview.md?toc=%2fazure%2fdns%2ftoc.json#resource-groups).|
   |**Umístění**|Západní USA||

> [!NOTE]
> Umístění skupiny prostředků nemá žádný vliv na zónu DNS. Umístění zóny DNS je vždy globální a nezobrazuje se.

## <a name="retrieve-name-servers"></a>Načtení názvových serverů

Předtím, než budete moci svoji zónu DNS delegovat do Azure DNS, musíte znát názvové servery pro vaši zónu. Azure DNS přiděluje názvové servery z fondu vždy, když je vytvořena zóna.

1. Když máte vytvořenou zónu DNS, na webu Azure Portal v podokně **Oblíbené** vyberte **Všechny prostředky**. Na stránce **Všechny prostředky** vyberte zónu DNS **contoso.net**. Pokud předplatné, které jste vybrali, už obsahovalo nějaké prostředky, můžete zadat **contoso.net** do pole **Filtrovat podle názvu** a snadno se tak dostat k bráně aplikace. 

1. Načtěte názvové servery ze stránky zóny DNS. V tomto příkladu byly pro zónu contoso.net přiřazené názvové servery ns1-01.azure-dns.com, ns2-01.azure-dns.net, ns3-01.azure-dns.org a ns4-01.azure-dns.info:

   ![Seznam názvových serverů](./media/dns-domain-delegation/viewzonens500.png)

Azure DNS automaticky vytvoří ve vaší zóně autoritativní záznamy NS, které obsahují přiřazené názvové servery. Chcete-li zobrazit názvové servery prostřednictvím Azure PowerShellu nebo Azure CLI, stačí jednoduše načíst tyto záznamy.

Následující příklady popisují také postup načtení názvových serverů pro zónu v Azure DNS pomocí PowerShellu a Azure CLI.

### <a name="powershell"></a>PowerShell

```powershell
# The record name "@" is used to refer to records at the top of the zone.
$zone = Get-AzureRmDnsZone -Name contoso.net -ResourceGroupName contosoRG
Get-AzureRmDnsRecordSet -Name "@" -RecordType NS -Zone $zone
```

Dalším příkladem je tato odpověď:

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
az network dns record-set list --resource-group contosoRG --zone-name contoso.net --type NS --name @
```

Dalším příkladem je tato odpověď:

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

Teď, když je vytvořena zóna DNS a máte názvové servery, je potřeba aktualizovat nadřazenou doménu s názvovými servery Azure DNS. Každý registrátor má vlastní nástroje pro správu DNS, které umožňují měnit záznamy názvových serverů pro doménu. Na stránce správy DNS vašeho registrátora upravte záznamy NS a nahraďte je záznamy NS vytvořenými v Azure DNS.

Při delegování domény do Azure DNS musíte použít názvové servery, které poskytuje Azure DNS. Doporučuje se vždycky použít všechny čtyři názvové servery bez ohledu na název domény. Delegování domény nevyžaduje, aby názvový server měl stejnou doménu nejvyšší úrovně jako vaše doména.

Pro odkazování na IP adresy názvových serverů Azure DNS nepoužívejte *spojovací záznamy* (glue records), protože se tyto IP adresy mohou v budoucnu měnit. Delegování pomocí názvových serverů ve vaší vlastní zóně, někdy označovaných jako *jednoduché názvové servery*, v současné době není v Azure DNS podporované.

## <a name="verify-that-name-resolution-is-working"></a>Ověření, že překlad názvů funguje

Po dokončení delegování můžete ověřit, že překlad názvů funguje, pomocí nástroje, jako je například nslookup, který se dotáže na záznam SOA pro vaši zónu. (Záznam SOA se automaticky vytvoří při vytváření zóny.)

Nemusíte určit názvové servery Azure DNS. Pokud bylo delegování správně nastaveno, normální proces překladu DNS najde názvové servery automaticky.

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

## <a name="delegate-subdomains-in-azure-dns"></a>Delegování subdomén v Azure DNS

Chcete-li nastavit samostatnou podřízenou zónu, můžete subdoménu delegovat v Azure DNS. Předpokládejme například, že v Azure DNS vytvoříte delegovanou doménu contoso.net. Teď chcete vytvořit samostatnou podřízenou zónu, partners.contoso.net.

1. Vytvořte podřízenou zónu partners.contoso.net v Azure DNS.
2. Vyhledejte autoritativní NS záznamy v podřízené zóně, získáte tak názvové servery hostující podřízenou zónu v Azure DNS.
3. Delegujte podřízenou zónu pomocí konfigurace záznamů NS v nadřazené zóně tak, aby ukazovaly na podřízenou zónu.

### <a name="create-a-dns-zone"></a>Vytvoření zóny DNS

1. Přihlaste se k portálu Azure.
1. V nabídce **Centrum** vyberte **Nový** > **Sítě** > **Zóna DNS**. Otevře se stránka **Vytvořit zónu DNS**.

   ![Zóna DNS](./media/dns-domain-delegation/dns.png)

1. Na stránce **Vytvořit zónu DNS** zadejte následující hodnoty a pak vyberte **Vytvořit**:

   | **Nastavení** | **Hodnota** | **Podrobnosti** |
   |---|---|---|
   |**Název**|partners.contoso.net|Zadejte název zóny DNS.|
   |**Předplatné**|[Vaše předplatné]|Vyberte předplatné, ve kterém se má služba Application Gateway vytvořit.|
   |**Skupina prostředků**|**Použít existující:** contosoRG|Vytvořte skupinu prostředků. Název skupiny prostředků musí být v rámci vybraného předplatného jedinečný. Další informace o skupinách prostředků najdete v článku s přehledem [Resource Manageru](../azure-resource-manager/resource-group-overview.md?toc=%2fazure%2fdns%2ftoc.json#resource-groups).|
   |**Umístění**|Západní USA||

> [!NOTE]
> Umístění skupiny prostředků nemá žádný vliv na zónu DNS. Umístění zóny DNS je vždy globální a nezobrazuje se.

### <a name="retrieve-name-servers"></a>Načtení názvových serverů

1. Když máte vytvořenou zónu DNS, na webu Azure Portal v podokně **Oblíbené** vyberte **Všechny prostředky**. Na stránce **Všechny prostředky** vyberte zónu DNS **partners.contoso.net**. Pokud předplatné, které jste vybrali, už obsahovalo nějaké prostředky, můžete zadat **contoso.net** do pole **Filtrovat podle názvu** a snadno se tak dostat k zóně DNS.

1. Načtěte názvové servery ze stránky zóny DNS. V tomto příkladu byly pro zónu contoso.net přiřazené názvové servery ns1-01.azure-dns.com, ns2-01.azure-dns.net, ns3-01.azure-dns.org a ns4-01.azure-dns.info:

   ![Seznam názvových serverů](./media/dns-domain-delegation/viewzonens500.png)

Azure DNS automaticky vytvoří ve vaší zóně autoritativní záznamy NS, které obsahují přiřazené názvové servery.  Chcete-li zobrazit názvové servery prostřednictvím Azure PowerShellu nebo Azure CLI, stačí jednoduše načíst tyto záznamy.

### <a name="create-a-name-server-record-in-the-parent-zone"></a>Vytvoření záznamu názvového serveru v nadřazené zóně

1. Na webu Azure Portal přejděte k zóně DNS **contoso.net**.
1. Vyberte **+ Sada záznamů**.
1. Na stránce **Přidat sadu záznamů** zadejte následující hodnoty a vyberte **OK**:

   | **Nastavení** | **Hodnota** | **Podrobnosti** |
   |---|---|---|
   |**Název**|partners|Zadejte název podřízené zóny DNS.|
   |**Typ**|NS|Pro záznamy názvového serveru použijte NS.|
   |**Hodnota TTL**|1|Zadejte hodnotu TTL (Time to Live).|
   |**Jednotka hodnoty TTL**|Hodiny|Nastavte jednotku hodnoty TTL (Time to Live) na hodiny.|
   |**NÁZVOVÝ SERVER**|{názvové servery ze zóny partners.contoso.net}|Zadejte všechny čtyři názvové servery ze zóny partners.contoso.net. |

   ![Hodnoty pro záznam názvového serveru](./media/dns-domain-delegation/partnerzone.png)


### <a name="delegate-subdomains-in-azure-dns-by-using-other-tools"></a>Delegování subdomén v Azure DNS pomocí jiných nástrojů

Následující příklady popisují postup delegování subdomén v Azure DNS pomocí PowerShellu a Azure CLI.

#### <a name="powershell"></a>PowerShell

Následující příklad PowerShell ukazuje, jak to funguje. Stejný postup lze provést prostřednictvím webu Azure Portal nebo víceplatformového nástroje v Azure CLI.

```powershell
# Create the parent and child zones. These can be in the same resource group or different resource groups, because Azure DNS is a global service.
$parent = New-AzureRmDnsZone -Name contoso.net -ResourceGroupName contosoRG
$child = New-AzureRmDnsZone -Name partners.contoso.net -ResourceGroupName contosoRG

# Retrieve the authoritative NS records from the child zone as shown in the next example. This information contains the name servers assigned to the child zone.
$child_ns_recordset = Get-AzureRmDnsRecordSet -Zone $child -Name "@" -RecordType NS

# Create the corresponding NS record set in the parent zone to complete the delegation. The record set name in the parent zone matches the child zone name (in this case, "partners").
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

# Create the parent and child zones. These can be in the same resource group or different resource groups, because Azure DNS is a global service.
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

# Create an NS record for each name server.
az network dns record-set ns add-record --resource-group contosorg --zone-name contoso.net --record-set-name partners --nsdname ns1-09.azure-dns.com.
az network dns record-set ns add-record --resource-group contosorg --zone-name contoso.net --record-set-name partners --nsdname ns2-09.azure-dns.net.
az network dns record-set ns add-record --resource-group contosorg --zone-name contoso.net --record-set-name partners --nsdname ns3-09.azure-dns.org.
az network dns record-set ns add-record --resource-group contosorg --zone-name contoso.net --record-set-name partners --nsdname ns4-09.azure-dns.info.
```

## <a name="delete-all-resources"></a>Odstranění všech prostředků

Pokud chcete odstranit všechny prostředky vytvořené v rámci tohoto článku, proveďte následující kroky:

1. Na webu Azure Portal v podokně **Oblíbené** vyberte **Všechny prostředky**. Na stránce **Všechny prostředky** vyberte skupinu prostředků **contosorg**. Pokud předplatné, které jste vybrali, už obsahovalo nějaké prostředky, můžete zadat **contoso.net** do pole **Filtrovat podle názvu** a snadno se tak dostat ke skupině prostředků.
1. Na stránce **contosorg** vyberte **Odstranit**.
1. Portál požaduje, abyste zadali název skupiny prostředků pro potvrzení, že ji skutečně chcete odstranit. Jako název skupiny prostředků zadejte **contosorg** a pak vyberte **Odstranit**. 

Odstranění skupiny prostředků odstraní všechny prostředky, které v ní jsou. Před odstraněním vždy zkontrolujte obsah skupiny prostředků. Portál odstraní všechny prostředky v rámci skupiny prostředků a potom odstraní samotnou skupinu prostředků. Tento proces trvá několik minut.

## <a name="next-steps"></a>Další kroky

[Správa zón DNS](dns-operations-dnszones.md)

[Správa záznamů DNS](dns-operations-recordsets.md)
