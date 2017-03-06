---
title: "Vytvoření záznamů DNS pomocí Azure CLI 1.0 | Dokumentace Microsoftu"
description: "Jak vytvořit záznamy hostitele pro Azure DNS Nastavení záznamů a sad záznamů pomocí Azure CLI 1.0"
services: dns
documentationcenter: na
author: georgewallace
manager: timlt
ms.assetid: 02b897d3-e83b-4257-b96d-5c29aa59e843
ms.service: dns
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 12/21/2016
ms.author: gwallace
translationtype: Human Translation
ms.sourcegitcommit: 36fa9cd757b27347c08f80657bab8a06789a3c2f
ms.openlocfilehash: 45599ea87b536b74cc652ef28f91a6058c7c8e4a
ms.lasthandoff: 02/27/2017

---

# <a name="create-dns-records-using-the-azure-cli-10"></a>Vytvoření záznamů DNS pomocí Azure CLI 1.0

> [!div class="op_single_selector"]
> * [Azure Portal](dns-getstarted-create-recordset-portal.md)
> * [PowerShell](dns-getstarted-create-recordset.md)
> * [Azure CLI 1.0](dns-getstarted-create-recordset-cli-nodejs.md)
> * [Azure CLI 2.0](dns-getstarted-create-recordset-cli.md)

Tento článek vás provede procesem vytvoření záznamů a sad záznamů pomocí Azure CLI 1.0.

## <a name="cli-versions-to-complete-the-task"></a>Verze rozhraní příkazového řádku pro dokončení úlohy

K dokončení úlohy můžete využít jednu z následujících verzí rozhraní příkazového řádku:

* [Azure CLI 1.0](dns-getstarted-create-recordset-cli-nodejs.md) – naše rozhraní příkazového řádku pro klasické modely nasazení a modely nasazení správy prostředků
* [Azure CLI 2.0](dns-getstarted-create-recordset-cli.md) – naše rozhraní příkazového řádku nové generace pro model nasazení správy prostředků

## <a name="introduction"></a>Úvod

Před vytvářením záznamů DNS v DNS Azure je nejprve nutné pochopit, jak DNS Azure organizuje záznamy DNS v sadách záznamů DNS.

[!INCLUDE [dns-about-records-include](../../includes/dns-about-records-include.md)]

Další informace o záznamech DNS v DNS Azure najdete v tématu [Zóny a záznamy DNS](dns-zones-records.md).

## <a name="create-a-record-set-and-record"></a>Vytvoření sady záznamů a záznamu

Tato část popisuje postup vytvoření záznamů DNS v DNS Azure. V příkladech se předpokládá, že jste už [nainstalovali Azure CLI 1.0, přihlásili jste se a vytvořili jste zónu DNS](dns-getstarted-create-dnszone-cli-nodejs.md).

Ve všech příkladech na této stránce se používají záznamy DNS typu A. Informace o dalších typech záznamů a další podrobnosti o správě záznamů a sad záznamů DNS najdete v tématu [Správa záznamů a sad záznamů DNS pomocí rozhraní příkazového řádku Azure CLI 1.0](dns-operations-recordsets-cli-nodejs.md).

## <a name="create-a-dns-record"></a>Vytvoření záznamu DNS

K vytvoření záznamu DNS použijte příkaz `azure network dns record-set add-record`. Nápovědu získáte příkazem `azure network dns record-set add-record -h`.

Při vytváření záznamu je třeba zadat název skupiny prostředků, název zóny, název sady záznamů a podrobnosti o vytvářeném záznamu.

Pokud sada záznamů ještě neexistuje, tento příkaz ji vytvoří. Pokud sada záznamů již existuje, tento příkaz přidá zadaný záznam do existující sady záznamů. 

Pokud se vytváří nová sada záznamů, použije se výchozí hodnota TTL (Time to Live) 3 600. Pokyny k používání různých hodnot TTL najdete v tématu [Správa záznamů DNS v DNS Azure pomocí Azure CLI 1.0](dns-operations-recordsets-cli-nodejs.md).

Následující příklad vytvoří záznam A s názvem *www* v zóně *contoso.com* ve skupině prostředků *MyResourceGroup*. IP adresa záznamu A je *1.2.3.4*.

```azurecli
azure network dns record-set add-record MyResourceGroup contoso.com www A -a 1.2.3.4
```

Pokud chcete vytvořit sadu záznamů na vrcholu zóny (v tomto případě „contoso.com“), použijte název záznamu "@" včetně uvozovek:

```azurecli
azure network dns record-set add-record MyResourceGroup contoso.com "@" A -a 1.2.3.4
```

Parametry použité k zadání dat záznamu se liší podle typu záznamu. Například pro záznam typu A zadáváte adresu IPv4 pomocí parametru `-a <IPv4 address>`. Seznam parametrů pro další typy záznamů zobrazíte příkazem `azure network dns record-set add-record -h`. Příklady pro jednotlivé typy záznamů najdete v tématu [Správa záznamů a sad záznamů DNS pomocí Azure CLI 1.0](dns-operations-recordsets-cli-nodejs.md).


## <a name="verify-name-resolution"></a>Ověření překladu adres IP

Přítomnost záznamů DNS na názvových serverech DNS Azure můžete otestovat pomocí nástrojů DNS, jako je například nslookup, dig nebo rutina [Resolve-DnsName prostředí PowerShell](https://technet.microsoft.com/library/jj590781.aspx).

Pokud jste doménu ještě nedelegovali k používání nové zóny v DNS Azure, je nutné [nasměrovat dotaz DNS přímo na jeden z názvových serverů zóny](dns-getstarted-create-dnszone.md#test-name-servers). V následujícím příkazu nezapomeňte dosadit správné hodnoty vaší zóny záznamů.

```
nslookup
> set type=A
> server ns1-01.azure-dns.com
> www.contoso.com

Server:  ns1-01.azure-dns.com
Address:  40.90.4.1

Name:    www.contoso.com
Address:  1.2.3.4
```

## <a name="next-steps"></a>Další kroky

Zjistěte, jak [delegovat název domény na názvové servery DNS Azure](dns-domain-delegation.md).

Zjistěte, jak [spravovat zóny DNS pomocí rozhraní příkazového řádku Azure CLI 1.0](dns-operations-dnszones-cli-nodejs.md).

Zjistěte, jak [spravovat záznamy a sady záznamů DNS pomocí rozhraní příkazového řádku Azure CLI 1.0](dns-operations-recordsets-cli-nodejs.md).


