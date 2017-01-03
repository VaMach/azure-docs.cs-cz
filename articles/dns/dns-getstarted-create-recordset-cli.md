---
title: "Vytvoření sady záznamů a záznamů zóny DNS pomocí rozhraní příkazového řádku | Dokumentace Microsoftu"
description: "Jak vytvořit záznamy hostitele pro Azure DNS. Nastavení sad záznamů a záznamů pomocí rozhraní příkazového řádku"
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
ms.date: 12/09/2016
ms.author: gwallace
translationtype: Human Translation
ms.sourcegitcommit: bfbffe7843bc178cdf289c999925c690ab82e922
ms.openlocfilehash: e377f176fe24a8e7e42d409f86d6b0093ce5e7c4

---

# <a name="create-dns-record-sets-and-records-by-using-cli"></a>Vytváření sad záznamů a záznamů DNS pomocí rozhraní příkazového řádku

> [!div class="op_single_selector"]
> * [Azure Portal](dns-getstarted-create-recordset-portal.md)
> * [PowerShell](dns-getstarted-create-recordset.md)
> * [Azure CLI](dns-getstarted-create-recordset-cli.md)

Tento článek vás provede procesem vytvoření záznamů a sad záznamů pomocí rozhraní příkazového řádku. Abyste to mohli udělat, musíte se nejdřív seznámit se záznamy a sadami záznamů DNS.

[!INCLUDE [dns-about-records-include](../../includes/dns-about-records-include.md)]

Tato část popisuje postup vytvoření záznamů DNS v DNS Azure. V příkladech se předpokládá, že jste již [nainstalovali rozhraní příkazového řádku Azure CLI, přihlásili jste se a vytvořili jste zónu DNS](dns-getstarted-create-dnszone-cli.md).

Ve všech příkladech na této stránce se používají záznamy DNS typu A. Informace o dalších typech záznamů a další podrobnosti o správě záznamů a sad záznamů DNS najdete v tématu [Správa záznamů a sad záznamů DNS pomocí rozhraní příkazového řádku Azure CLI](dns-operations-recordsets-cli.md).

## <a name="create-a-record-set-and-record"></a>Vytvoření sady záznamů a záznamu

V této části si ukážeme, jak vytvořit sadu záznamů a záznamy. V tomto příkladu vytvoříte sadu záznamů s relativním názvem „www“ v zóně DNS „contoso.com“. Plně kvalifikovaný název záznamů je „www.contoso.com“. Typ záznamu je „A“ a hodnota TTL (Time to Live) je 60 sekund. Po dokončení tohoto kroku bude vytvořená prázdná sada záznamů.

Pokud chcete vytvořit sadu záznamů na vrcholu zóny (v tomto případě „contoso.com“), použijte název záznamu "@", včetně uvozovek. Jde o běžnou konvenci DNS.

### <a name="1-create-a-record-set"></a>1. Vytvoření sady záznamů

Pokud má nový záznam stejný název a typ jako existující záznam, je nutné ho přidat do existující sady záznamů. Tento krok můžete vynechat a přeskočit níže na [Přidání záznamů](#add-records). Naopak pokud má nový záznam jiný název i typ než všechny existující záznamy, je nutné vytvořit novou sadu záznamů.

Sady záznamů vytváříte pomocí příkazu `azure network dns record-set create`. Nápovědu získáte příkazem `azure network dns record-set create -h`.  

Při vytváření sady záznamů budete muset zadat její název, zónu, hodnotu TTL (Time to Live) a typ záznamu. 

```azurecli
azure network dns record-set create myresourcegroup contoso.com www A 60
```

Po dokončení tohoto kroku budete mít prázdnou sadu záznamů „www“. Pokud chcete nově vytvořenou sadu záznamů „www“ používat, musíte do ní nejprve přidat záznamy.

### <a name="2-add-records"></a>2. Přidání záznamů

Záznamy se do sad záznamů přidávají příkazem `azure network dns record-set add-record`. Nápovědu získáte příkazem `azure network dns record-set add-record -h`.

Parametry pro přidání záznamů do sady záznamů se liší podle typu sady záznamů. Pokud například používáte sadu záznamů typu A, můžete zadat záznamy pouze parametrem `-a <IPv4 address>`. Seznam parametrů pro další typy záznamů zobrazíte příkazem `azure network dns record-set add-record -h`.

Záznam A můžete přidat do dříve vytvořené sady záznamů „www“ tímto příkazem:

```azurecli
azure network dns record-set add-record myresourcegroup contoso.com  www A  -a 1.2.3.4
```

### <a name="verify-name-resolution"></a>Ověření překladu adres IP

Přítomnost záznamů DNS na názvových serverech DNS Azure můžete otestovat pomocí nástrojů DNS, jako je například nslookup, dig nebo rutina [Resolve-DnsName prostředí PowerShell](https://technet.microsoft.com/library/jj590781.aspx).

Pokud jste doménu ještě nedelegovali k používání nové zóny v DNS Azure, je nutné [nasměrovat dotaz DNS přímo na jeden z názvových serverů zóny](dns-getstarted-create-dnszone.md#test-name-servers). V následujícím příkazu nezapomeňte dosadit správné hodnoty vaší zóny záznamů.

    nslookup
    > set type=A
    > server ns1-01.azure-dns.com
    > www.contoso.com

    Server:  ns1-01.azure-dns.com
    Address:  40.90.4.1

    Name:    www.contoso.com
    Address:  1.2.3.4

## <a name="next-steps"></a>Další kroky

Zjistěte, jak [delegovat název domény na názvové servery DNS Azure](dns-domain-delegation.md).

Zjistěte, jak [spravovat zóny DNS pomocí rozhraní příkazového řádku Azure CLI](dns-operations-dnszones-cli.md).

Zjistěte, jak [spravovat záznamy a sady záznamů DNS pomocí rozhraní příkazového řádku Azure CLI](dns-operations-recordsets-cli.md).




<!--HONumber=Dec16_HO2-->


