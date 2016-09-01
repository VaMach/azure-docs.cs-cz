<properties
   pageTitle="Vytvoření sady záznamů a záznamů zóny DNS pomocí rozhraní příkazového řádku | Microsoft Azure"
   description="Jak vytvořit záznamy hostitele pro Azure DNS. Nastavení sad záznamů a záznamů pomocí rozhraní příkazového řádku"
   services="dns"
   documentationCenter="na"
   authors="cherylmc"
   manager="carmonm"
   editor=""/>

<tags
   ms.service="dns"
   ms.devlang="na"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="08/16/2016"
   ms.author="cherylmc"/>

# Vytváření sad záznamů a záznamů DNS pomocí rozhraní příkazového řádku

> [AZURE.SELECTOR]
- [Azure Portal](dns-getstarted-create-recordset-portal.md)
- [PowerShell](dns-getstarted-create-recordset.md)
- [Azure CLI](dns-getstarted-create-recordset-cli.md)


Tento článek vás provede procesem vytvoření záznamů a sad záznamů pomocí rozhraní příkazového řádku. Po vytvoření zóny DNS je nutné přidat záznamy DNS domény. Abyste to mohli udělat, musíte se nejdřív seznámit se záznamy a sadami záznamů DNS.

[AZURE.INCLUDE [dns-about-records-include](../../includes/dns-about-records-include.md)]

## Vytvoření sady záznamů a záznamu

V této části si ukážeme, jak vytvořit sadu záznamů a záznamy. V tomto příkladu vytvoříte sadu záznamů s relativním názvem „www“ v zóně DNS „contoso.com“. Plně kvalifikovaný název záznamů je „www.contoso.com“. Typ záznamu je „A“ a hodnota TTL (Time to Live) je 60 sekund. Po dokončení tohoto kroku bude vytvořená prázdná sada záznamů.

Pokud chcete vytvořit sadu záznamů na vrcholu zóny (v tomto případě „contoso.com“), použijte název záznamu "@", včetně uvozovek. Jde o běžnou konvenci DNS.

### 1. Vytvoření sady záznamů

Pokud chcete vytvořit sadu záznamů, použijte `azure network dns record-set create`. Zadejte skupinu prostředků, název zóny, relativní název sady záznamů, typ záznamu a hodnotu TTL. Pokud není parametr `--ttl` definovaný, má výchozí hodnotu čtyři (v sekundách). Po dokončení tohoto kroku budete mít prázdnou sadu záznamů „www“.

*Použití: network dns record-set create <resource-group> <dns-zone-name> <name> <type> <ttl>*

    azure network dns record-set create myresourcegroup  contoso.com  www A  60

### 2. Přidání záznamů

Pokud chcete nově vytvořenou sadu záznamů „www“ používat, musíte do ní přidat záznamy. Záznamy se do sad záznamů přidávají příkazem `azure network dns record-set add-record`.

Parametry pro přidání záznamů do sady záznamů se liší podle typu sady záznamů. Pokud například používáte sadu záznamů typu A, budete moci určit záznamy jenom parametrem `-a <IPv4 address>`.

Záznamy IPv4 *A* můžete přidat do sady záznamů „www“ tímto příkazem:

*Použití: network dns record-set add-record <resource-group> <dns-zone-name> <record-set-name> <type>*

    azure network dns record-set add-record myresourcegroup contoso.com  www A  -a 134.170.185.46

## Příklady dalších typů záznamu

Následující příklady ukazují, jak vytvořit sadu záznamů jednotlivých typů záznamu. Každá sada záznamů obsahuje jediný záznam.

[AZURE.INCLUDE [dns-add-record-cli-include](../../includes/dns-add-record-cli-include.md)]

## Další kroky

Pokud chcete spravovat sady záznamů a záznamy, informace najdete v tématu [Správa sad záznamů a záznamů DNS pomocí rozhraní příkazového řádku](dns-operations-recordsets-portal.md).

Další informace o službě Azure DNS najdete v tématu [Přehled Azure DNS](dns-overview.md).



<!--HONumber=Aug16_HO4-->


