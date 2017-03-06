---
title: "Vytvoření zóny DNS pomocí Azure CLI 1.0 | Dokumentace Microsoftu"
description: "Naučte se vytvářet zóny DNS v DNS Azure. Pomocí tohoto podrobného průvodce můžete vytvořit a spravovat svou první zónu DNS pomocí rozhraní příkazového řádku Azure CLI 1.0."
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
ms.date: 12/21/2016
ms.author: gwallace
translationtype: Human Translation
ms.sourcegitcommit: 36fa9cd757b27347c08f80657bab8a06789a3c2f
ms.openlocfilehash: 1e8645e8554bc8f8ec72a6df98bfe9ffc5be13b3
ms.lasthandoff: 02/27/2017

---

# <a name="create-an-azure-dns-zone-using-azure-cli-10"></a>Vytvoření zóny DNS pomocí Azure CLI 1.0

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

[!INCLUDE [dns-cli-setup](../../includes/dns-cli-setup-include.md)]

## <a name="create-a-dns-zone"></a>Vytvoření zóny DNS

Zóna DNS se vytvoří příkazem `azure network dns zone create`. Pokud chcete zobrazit nápovědu k tomuto příkazu, zadejte `azure network dns zone create -h`.

Následující příklad vytvoří zónu DNS s názvem *contoso.com* ve skupině prostředků s názvem *MyResourceGroup*. Nahraďte hodnoty vlastními a použijte tento příklad k vytvoření zóny DNS.

```azurecli
azure network dns zone create MyResourceGroup contoso.com
```

## <a name="verify-your-dns-zone"></a>Ověření zóny DNS

### <a name="view-records"></a>Zobrazení záznamů

Při vytvoření zóny DNS se taky vytvoří tyto záznamy DNS:

* Záznam *Start of Authority* (SOA). Tento záznam se nachází v kořenovém adresáři každé zóny DNS.
* Záznamy autoritativního názvového serveru (NS). Tyto záznamy ukazují, které názvové servery zónu hostují. DNS Azure používá fond názvových serverů, takže různým zónám se můžou v DNS Azure přiřadit různé názvové servery. Další informace najdete v tématu [Delegování domény do DNS Azure](dns-domain-delegation.md).

Pokud chcete tyto záznamy zobrazit, použijte příkaz `azure network dns-record-set list`:

```azurecli
azure network dns record-set list MyResourceGroup contoso.com

info:    Executing command network dns record-set list
+ Looking up the DNS Record Sets
data:    Name                            : @
data:    Type                            : NS
data:    TTL                             : 172800
data:    Records:
data:      ns1-01.azure-dns.com.
data:      ns2-01.azure-dns.net.
data:      ns3-01.azure-dns.org.
data:      ns4-01.azure-dns.info.
data:
data:    Name                            : @
data:    Type                            : SOA
data:    TTL                             : 3600
data:    Email                           : azuredns-hostmaster.microsoft.com
data:    Host                            : ns1-01.azure-dns.com.
data:    Serial Number                   : 2
data:    Refresh Time                    : 3600
data:    Retry Time                      : 300
data:    Expire Time                     : 2419200
data:    Minimum TTL                     : 300
data:
info:    network dns record-set list command OK
```

> [!NOTE]
> Sady záznamů v kořenovém adresáři (tj. *vrcholu*) zóny DNS používají jako název sady záznamu **@**.

### <a name="test-name-servers"></a>Testování názvových serverů

Přítomnost zóny DNS na názvových serverech DNS Azure můžete otestovat pomocí nástrojů DNS, jako je například nslookup, dig nebo rutina `Resolve-DnsName` prostředí PowerShell.

Pokud jste doménu ještě nedelegovali k používání nové zóny v Azure DNS, je nutné dotaz DNS nasměrovat přímo na jeden z názvových serverů zóny. Názvové servery zóny jsou uvedené v záznamech NS, které můžete zobrazit příkazem `azure network dns record-set list`.

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


