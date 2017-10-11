---
title: "Správa zón DNS v Azure DNS - 1.0 rozhraní příkazového řádku Azure | Microsoft Docs"
description: "Můžete spravovat pomocí Azure CLI 1.0 zóny DNS. Tento článek ukazuje, jak k aktualizaci, odstranění a vytvoření zóny DNS na Azure DNS."
services: dns
documentationcenter: na
author: georgewallace
manager: timlt
ms.assetid: 8ab63bc4-5135-4ed8-8c0b-5f0712b9afed
ms.service: dns
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 12/21/2016
ms.author: gwallace
ms.openlocfilehash: 588c87749f049eff5b9e0729f6769c8367ba41e4
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="how-to-manage-dns-zones-in-azure-dns-using-the-azure-cli-10"></a>Správa zón DNS v Azure DNS pomocí Azure CLI 1.0

> [!div class="op_single_selector"]
> * [Azure Portal](dns-operations-dnszones-portal.md)
> * [PowerShell](dns-operations-dnszones.md)
> * [Azure CLI 1.0](dns-operations-dnszones-cli-nodejs.md)
> * [Azure CLI 2.0](dns-operations-dnszones-cli.md)

Tato příručka ukazuje, jak spravovat zóny DNS pomocí 1.0 rozhraní příkazového řádku Azure a platformy, která je dostupná pro Windows, Mac a Linux. Můžete také spravovat zóny DNS pomocí [prostředí Azure PowerShell](dns-operations-dnszones.md) nebo portálu Azure.

## <a name="cli-versions-to-complete-the-task"></a>Verze rozhraní příkazového řádku pro dokončení úlohy

K dokončení úlohy můžete využít jednu z následujících verzí rozhraní příkazového řádku:

* [Azure CLI 1.0](dns-operations-dnszones-cli-nodejs.md) – naše rozhraní příkazového řádku pro klasické modely nasazení a modely nasazení správy prostředků
* [Azure CLI 2.0](dns-operations-dnszones-cli.md) – naše rozhraní příkazového řádku nové generace pro model nasazení správy prostředků

## <a name="introduction"></a>Úvod

[!INCLUDE [dns-create-zone-about](../../includes/dns-create-zone-about-include.md)]

[!INCLUDE [dns-cli-setup](../../includes/dns-cli-setup-include.md)]

## <a name="getting-help"></a>Získání nápovědy

Všechny příkazy rozhraní příkazového řádku 1.0 týkající se Azure DNS začínat `azure network dns`. Je k dispozici pro každý příkaz pomocí Nápověda `--help` možnost (krátký tvar `-h`).  Například:

```azurecli
azure network dns -h
azure network dns zone -h
azure network dns zone create -h
```

## <a name="create-a-dns-zone"></a>Vytvoření zóny DNS

Zóna DNS se vytvoří příkazem `azure network dns zone create`. Nápovědu získáte příkazem `azure network dns zone create -h`.

Následující příklad vytvoří zónu DNS s názvem *contoso.com* ve skupině prostředků s názvem *MyResourceGroup*:

```azurecli
azure network dns zone create MyResourceGroup contoso.com
```

### <a name="to-create-a-dns-zone-with-tags"></a>Vytvoření zóny DNS pomocí značek

Následující příklad ukazuje, jak vytvořit zónu DNS se dvěma [Azure Resource Manager značky](dns-zones-records.md#tags), *projektu = demo* a *env = test*, pomocí `--tags` parametr (krátký tvar `-t`):

```azurecli
azure network dns zone create MyResourceGroup contoso.com -t "project=demo";"env=test"
```

## <a name="get-a-dns-zone"></a>Získat zóny DNS

Chcete-li načíst zónu DNS, použijte `azure network dns zone show`. Nápovědu získáte příkazem `azure network dns zone show -h`.

Následující příklad vrací zónu DNS *contoso.com* a související data ze skupiny prostředků *MyResourceGroup*. 

```azurecli
azure network dns zone show MyResourceGroup contoso.com
```

Dalším příkladem je tato odpověď.

```
info:    Executing command network dns zone show
+ Looking up the dns zone "contoso.com"
data:    Id                              : /subscriptions/.../contoso.com
data:    Name                            : contoso.com
data:    Type                            : Microsoft.Network/dnszones
data:    Location                        : global
data:    Number of record sets           : 2
data:    Max number of record sets       : 5000
data:    Name servers:
data:        ns1-01.azure-dns.com.
data:        ns2-01.azure-dns.net.
data:        ns3-01.azure-dns.org.
data:        ns4-01.azure-dns.info.
data:    Tags                            : project=demo;env=test
info:    network dns zone show command OK
```

Všimněte si, že nejsou vrácené záznamy DNS `azure network dns zone show`. Chcete-li seznam záznamů DNS, použijte `azure network dns record-set list`.


## <a name="list-dns-zones"></a>Seznam zón DNS

Chcete-li vytvořit výčet zóny DNS, použijte `azure network dns zone list`. Nápovědu získáte příkazem `azure network dns zone list -h`.

Zadání skupiny prostředků jsou uvedeny pouze pásma v rámci skupiny prostředků:

```azurecli
azure network dns zone list MyResourceGroup
```

Vynechání skupina prostředků obsahuje seznam všech zón v předplatném:

```azurecli
azure network dns zone list 
```

## <a name="update-a-dns-zone"></a>Aktualizaci zóny DNS

Změny k prostředku zóny DNS lze provést pomocí `azure network dns zone set`. Nápovědu získáte příkazem `azure network dns zone set -h`.

Tento příkaz nelze aktualizovat všechny sady záznamů DNS v rámci zóny (viz [postup záznamy DNS spravovat](dns-operations-recordsets-cli-nodejs.md)). Toto pravidlo se používá pouze k aktualizaci vlastností prostředku zóny sám sebe. Tyto vlastnosti jsou aktuálně omezená na [Azure Resource Manager, značky'](dns-zones-records.md#tags) prostředku zóny.

Následující příklad ukazuje, jak k aktualizaci značek u zóny DNS. Zadaná hodnota se nahradí stávající značky.

```azurecli
azure network dns zone set MyResourceGroup contoso.com -t "team=support"
```

## <a name="delete-a-dns-zone"></a>Odstranit zónu DNS

Zóny DNS lze odstranit pomocí `azure network dns zone delete`. Nápovědu získáte příkazem `azure network dns zone delete -h`.

> [!NOTE]
> Odstranění zóny DNS se také odstraní všechny záznamy DNS v rámci zóny. Tuto operaci nelze vrátit zpět. Pokud je zóna DNS se používá, službám pomocí zóny selže při odstranění zóny.
>
>Chcete-li chránit proti náhodnému zóny odstranění, přečtěte si téma [jak chránit zóny DNS a záznamy](dns-protect-zones-recordsets.md).

Tento příkaz zobrazí výzvu k potvrzení. Volitelné `--quiet` přepínače (krátký tvar `-q`) potlačí tuto výzvu.

Následující příklad ukazuje, jak odstranit zónu *contoso.com* ze skupiny prostředků *MyResourceGroup*.

```azurecli
azure network dns zone delete MyResourceGroup contoso.com
```

## <a name="next-steps"></a>Další kroky

Zjistěte, jak [spravovat sady záznamů a záznamy](dns-getstarted-create-recordset-cli-nodejs.md) ve vaší zóně DNS.

Zjistěte, jak [delegování domény do Azure DNS](dns-domain-delegation.md).

