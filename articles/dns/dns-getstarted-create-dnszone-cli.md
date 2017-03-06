---
title: "Vytvoření zóny DNS pomocí Azure CLI 2.0 | Dokumentace Microsoftu"
description: "Naučte se vytvářet zóny DNS v DNS Azure. Pomocí tohoto podrobného průvodce můžete vytvořit a spravovat svou první zónu DNS pomocí rozhraní příkazového řádku Azure CLI 2.0."
services: dns
documentationcenter: na
author: georgewallace
manager: timlt
ms.assetid: 1514426a-133c-491a-aa27-ee0962cea9dc
ms.service: dns
ms.devlang: na
ms.topic: hero-article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/27/2017
ms.author: gwallace
translationtype: Human Translation
ms.sourcegitcommit: 1481fcb070f383d158c5a6ae32504e498de4a66b
ms.openlocfilehash: 00a4dfbe070129ce6dc84f006d793007d55dba3f
ms.lasthandoff: 03/01/2017

---

# <a name="create-an-azure-dns-zone-using-azure-cli-20"></a>Vytvoření zóny DNS pomocí Azure CLI 2.0

> [!div class="op_single_selector"]
> * [Azure Portal](dns-getstarted-create-dnszone-portal.md)
> * [PowerShell](dns-getstarted-create-dnszone.md)
> * [Azure CLI 1.0](dns-getstarted-create-dnszone-cli-nodejs.md)
> * [Azure CLI 2.0](dns-getstarted-create-dnszone-cli.md)

Tento článek vás provede kroky k vytvoření zóny DNS pomocí rozhraní příkazového řádku Azure CLI pro různé platformy, které je dostupné pro Windows, Mac a Linux. Zónu DNS můžete vytvořit také pomocí [Azure PowerShellu](dns-getstarted-create-dnszone.md) nebo webu [Azure Portal](dns-getstarted-create-dnszone-portal.md).

## <a name="cli-versions-to-complete-the-task"></a>Verze rozhraní příkazového řádku pro dokončení úlohy

K dokončení úlohy můžete využít jednu z následujících verzí rozhraní příkazového řádku:

* [Azure CLI 1.0](dns-getstarted-create-dnszone-cli-nodejs.md) – naše rozhraní příkazového řádku pro klasické modely nasazení a modely nasazení správy prostředků
* [Azure CLI 2.0](dns-getstarted-create-dnszone-cli.md) – naše rozhraní příkazového řádku nové generace pro model nasazení správy prostředků

## <a name="introduction"></a>Úvod

[!INCLUDE [dns-create-zone-about](../../includes/dns-create-zone-about-include.md)]

## <a name="set-up-azure-cli-20-for-azure-dns"></a>Nastavení Azure CLI 2.0 pro Azure DNS

### <a name="before-you-begin"></a>Než začnete

Před zahájením konfigurace ověřte, zda máte následující.

* Předplatné Azure. Pokud ještě nemáte předplatné Azure, můžete si aktivovat [výhody pro předplatitele MSDN](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/) nebo si zaregistrovat [bezplatný účet](https://azure.microsoft.com/pricing/free-trial/).

* Nainstalujte nejnovější verzi rozhraní příkazového řádku Azure, která je dostupná pro Windows, Linux a Mac. Další informace najdete v tématu [Instalace rozhraní příkazového řádku Azure CLI 2.0](https://docs.microsoft.com/en-us/cli/azure/install-az-cli2).

### <a name="sign-in-to-your-azure-account"></a>Přihlášení k účtu Azure

Otevřete okno konzoly a proveďte ověření pomocí svých přihlašovacích údajů. Další informace najdete v tématu Přihlášení k Azure z rozhraní příkazového řádku Azure.

```azurecli
az login
```

### <a name="select-the-subscription"></a>Výběr předplatného

Zkontrolujte předplatná pro příslušný účet.

```azurecli
az account list
```

### <a name="choose-which-of-your-azure-subscriptions-to-use"></a>Zvolte předplatné Azure, které chcete použít.

```azurecli
az account set --subscription "subscription name"
```

### <a name="create-a-resource-group"></a>Vytvoření skupiny prostředků

Azure Resource Manager vyžaduje, aby všechny skupiny prostředků určily umístění. To slouží jako výchozí umístění pro prostředky v příslušné skupině prostředků. Všechny prostředky DNS jsou ale globální, a ne místní, takže volba umístění skupiny prostředků nemá na Azure DNS žádný vliv.

Pokud používáte některou ze stávajících skupin prostředků, můžete tento krok přeskočit.

```azurecli
az group create --name myresourcegroup --location "West US"
```

## <a name="create-a-dns-zone"></a>Vytvoření zóny DNS

Zóna DNS se vytvoří příkazem `az network dns zone create`. Pokud chcete zobrazit nápovědu k tomuto příkazu, zadejte `az network dns zone create --help`.

Následující příklad vytvoří zónu DNS s názvem *contoso.com* ve skupině prostředků s názvem *MyResourceGroup*. Nahraďte hodnoty vlastními a použijte tento příklad k vytvoření zóny DNS.

```azurecli
az network dns zone create --resource-group myresourcegroup --name contoso.com
```

## <a name="verify-your-dns-zone"></a>Ověření zóny DNS

### <a name="view-records"></a>Zobrazení záznamů

Při vytvoření zóny DNS se taky vytvoří tyto záznamy DNS:

* Záznam *Start of Authority* (SOA). Tento záznam se nachází v kořenovém adresáři každé zóny DNS.
* Záznamy autoritativního názvového serveru (NS). Tyto záznamy ukazují, které názvové servery zónu hostují. DNS Azure používá fond názvových serverů, takže různým zónám se můžou v DNS Azure přiřadit různé názvové servery. Další informace najdete v tématu [Delegování domény do DNS Azure](dns-domain-delegation.md).

Pokud chcete tyto záznamy zobrazit, použijte příkaz `az network dns record-set list`:

```azurecli
az network dns record-set list --resource-group MyResourceGroup --zone-name contoso.com
```

Odpověď pro `az network dns record-set list` je zobrazená níž:

```json
[
  {
    "etag": "510f4711-8b7f-414e-b8d5-c0383ea3d62e",
    "id": "/subscriptions/147a22e9-2356-4e56-b3de-1f5842ae4a3b/resourceGroups/myresourcegroup/providers/Microsoft.Network/dnszones/contoso.com/NS/@",
    "metadata": null,
    "name": "@",
    "nsRecords": [
      {
        "nsdname": "ns1-04.azure-dns.com."
      },
      {
        "nsdname": "ns2-04.azure-dns.net."
      },
      {
        "nsdname": "ns3-04.azure-dns.org."
      },
      {
        "nsdname": "ns4-04.azure-dns.info."
      }
    ],
    "resourceGroup": "myresourcegroup",
    "ttl": 172800,
    "type": "Microsoft.Network/dnszones/NS"
  },
  {
    "etag": "0e48e82f-f194-4d3e-a864-7e109a95c73a",
    "id": "/subscriptions/147a22e9-2356-4e56-b3de-1f5842ae4a3b/resourceGroups/myresourcegroup/providers/Microsoft.Network/dnszones/contoso.com/SOA/@",
    "metadata": null,
    "name": "@",
    "resourceGroup": "myresourcegroup",
    "soaRecord": {
      "email": "azuredns-hostmaster.microsoft.com",
      "expireTime": 2419200,
      "host": "ns1-04.azure-dns.com.",
      "minimumTtl": 300,
      "refreshTime": 3600,
      "retryTime": 300,
      "serialNumber": 1
    },
    "ttl": 3600,
    "type": "Microsoft.Network/dnszones/SOA"
  }
]
```

> [!NOTE]
> Sady záznamů v kořenovém adresáři (tj. *vrcholu*) zóny DNS používají jako název sady záznamu **@**.

### <a name="test-name-servers"></a>Testování názvových serverů

Přítomnost zóny DNS na názvových serverech DNS Azure můžete otestovat pomocí nástrojů DNS, jako je například nslookup, dig nebo rutina `Resolve-DnsName` prostředí PowerShell.

Pokud jste doménu ještě nedelegovali k používání nové zóny v Azure DNS, je nutné dotaz DNS nasměrovat přímo na jeden z názvových serverů zóny. Názvové servery zóny jsou uvedené v záznamech NS, které můžete zobrazit příkazem `az network dns record-set list`.

Následující příklad používá nástroj dig k dotazu na doménu contoso.com pomocí názvových serverů přiřazených zóně DNS. Nezapomeňte dosadit správné hodnoty vaší zóny.

```
  > dig @ns1-01.azure-dns.com contoso.com
  
  <<>> DiG 9.10.2-P2 <<>> @ns1-01.azure-dns.com contoso.com
(1 server found)
global options: +cmd
  Got answer:
->>HEADER<<- opcode: QUERY, status: NOERROR, id: 60963
  flags: qr aa rd; QUERY: 1, ANSWER: 0, AUTHORITY: 1, ADDITIONAL: 1
  WARNING: recursion requested but not available

  OPT PSEUDOSECTION:
  EDNS: version: 0, flags:; udp: 4000
  QUESTION SECTION:
contoso.com.                        IN      A

  AUTHORITY SECTION:
contoso.com.         3600     IN      SOA     ns1-01.azure-dns.com. azuredns-hostmaster.microsoft.com. 1 3600 300 2419200 300

Query time: 93 msec
SERVER: 208.76.47.5#53(208.76.47.5)
WHEN: Tue Jul 21 16:04:51 Pacific Daylight Time 2015
MSG SIZE  rcvd: 120
```

## <a name="next-steps"></a>Další kroky

Po vytvoření zóny DNS v ní [vytvořte záznamy a sady záznamů DNS](dns-getstarted-create-recordset-cli.md).


