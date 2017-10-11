---
title: "Začínáme s DNS Azure pomocí Azure CLI 1.0 | Dokumentace Microsoftu"
description: "Naučíte se vytvořit zónu a záznam DNS v DNS Azure. Pomocí tohoto podrobného průvodce můžete vytvořit a spravovat první zónu a záznam DNS pomocí Azure CLI 1.0."
services: dns
documentationcenter: na
author: jtuliani
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: fb0aa0a6-d096-4d6a-b2f6-eda1c64f6182
ms.service: dns
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 03/10/2017
ms.author: jonatul
ms.openlocfilehash: f7943b71bbd16c36df09436973d92539eb62b210
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="get-started-with-azure-dns-using-azure-cli-10"></a>Začínáme s DNS Azure pomocí Azure CLI 1.0

> [!div class="op_single_selector"]
> * [Azure Portal](dns-getstarted-portal.md)
> * [PowerShell](dns-getstarted-powershell.md)
> * [Azure CLI 1.0](dns-getstarted-cli-nodejs.md)
> * [Azure CLI 2.0](dns-getstarted-cli.md)

Tento článek vás provede kroky k vytvoření první zóny a záznamu DNS pomocí Azure CLI 1.0 pro různé platformy, které je dostupné pro Windows, Mac a Linux. Tyto kroky můžete provést také pomocí webu Azure Portal nebo Azure PowerShellu.

K hostování záznamů DNS v určité doméně se používá zóna DNS. Pokud chcete začít hostovat svou doménu v DNS Azure, musíte vytvořit zónu DNS pro daný název domény. Všechny záznamy DNS pro vaši doménu se pak vytvoří v této zóně DNS. Nakonec, pokud chcete zónu DNS publikovat na internetu, bude potřeba nakonfigurovat pro doménu názvové servery. Jednotlivé kroky jsou popsány níže.

Tyto pokyny předpokládají, že již máte nainstalované Azure CLI 1.0 a jste k němu přihlášeni. Nápovědu získáte v tématu [Správa zón DNS pomocí Azure CLI 1.0](dns-operations-dnszones-cli-nodejs.md).

## <a name="create-the-resource-group"></a>Vytvoření skupiny prostředků

Před vytvořením zóny DNS se vytvoří skupina prostředků, která bude obsahovat zónu DNS. Následuje ukázka příkazu.

```azurecli
azure group create --name MyResourceGroup --location "West US"
```

## <a name="create-a-dns-zone"></a>Vytvoření zóny DNS

Zóna DNS se vytvoří příkazem `azure network dns zone create`. Pokud chcete zobrazit nápovědu k tomuto příkazu, zadejte `azure network dns zone create -h`.

Následující příklad vytvoří zónu DNS s názvem *contoso.com* ve skupině prostředků s názvem *MyResourceGroup*. Nahraďte hodnoty vlastními a použijte tento příklad k vytvoření zóny DNS.

```azurecli
azure network dns zone create MyResourceGroup contoso.com
```


## <a name="create-a-dns-record"></a>Vytvoření záznamu DNS

K vytvoření záznamu DNS použijte příkaz `azure network dns record-set add-record`. Nápovědu získáte příkazem `azure network dns record-set add-record -h`.

Následující příklad vytvoří záznam s relativním názvem „www“ v zóně DNS „contoso.com“ ve skupině prostředků „MyResourceGroup“. Plně kvalifikovaný název sady záznamů je „www.contoso.com“. Typ záznamu je A a používá se IP adresa 1.2.3.4 a výchozí hodnota TTL 3 600 sekund (1 hodina).

```azurecli
azure network dns record-set add-record MyResourceGroup contoso.com www A -a 1.2.3.4
```

Informace o dalších typech záznamů, sadách záznamů s více než jedním záznamem, alternativních hodnotách TTL a úpravě existujících záznamů najdete v tématu [Správa záznamů a sad záznamů DNS pomocí Azure CLI 1.0](dns-operations-recordsets-cli-nodejs.md).


## <a name="view-records"></a>Zobrazení záznamů

K výpisu záznamů DNS ve vaší zóně použijte:

```azurecli
azure network dns record-set list MyResourceGroup contoso.com
```


## <a name="update-name-servers"></a>Aktualizace názvových serverů

Jakmile budete spokojeni se správným nastavením zóny a záznamů DNS, bude potřeba nakonfigurovat váš název domény tak, aby používal názvové servery DNS Azure. Tím umožníte ostatním uživatelům na internetu najít vaše záznamy DNS.

Názvové servery vaší zóny můžete zobrazit příkazem `azure network dns zone show`:

```azurecli
azure network dns zone show MyResourceGroup contoso.com

info:    Executing command network dns zone show
+ Looking up the dns zone "contoso.com"
data:    Id                              : /subscriptions/a385a691-bd93-41b0-8084-8213ebc5bff7/resourceGroups/myresourcegroup/providers/Microsoft.Network/dnszones/contoso.com
data:    Name                            : contoso.com
data:    Type                            : Microsoft.Network/dnszones
data:    Location                        : global
data:    Number of record sets           : 3
data:    Max number of record sets       : 5000
data:    Name servers:
data:        ns1-01.azure-dns.com.
data:        ns2-01.azure-dns.net.
data:        ns3-01.azure-dns.org.
data:        ns4-01.azure-dns.info.
data:    Tags                            :
info:    network dns zone show command OK
```

Tyto názvové servery by měly být nakonfigurované u registrátora názvu domény (u kterého jste zakoupili název domény). Registrátor vám nabídne možnost nastavit názvové servery pro doménu. Další informace najdete v tématu [Delegování domény do DNS Azure](dns-domain-delegation.md).

## <a name="delete-all-resources"></a>Odstranění všech prostředků
 
Pokud chcete odstranit všechny prostředky vytvořené v rámci tohoto článku, proveďte následující krok:

```azurecli
azure group delete --name MyResourceGroup
```

## <a name="next-steps"></a>Další kroky

Další informace o DNS Azure najdete v tématu [Přehled DNS Azure](dns-overview.md).

Další informace o správě zón DNS v DNS Azure najdete v tématu [Správa zón DNS v DNS Azure pomocí Azure CLI 1.0](dns-operations-dnszones-cli-nodejs.md).

Další informace o správě záznamů DNS v DNS Azure najdete v tématu [Správa záznamů a sad záznamů DNS v DNS Azure pomocí Azure CLI 1.0](dns-operations-recordsets-cli-nodejs.md).

