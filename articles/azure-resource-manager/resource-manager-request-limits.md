---
title: "Omezení žádostí o Azure Resource Manager | Microsoft Docs"
description: "Popisuje způsob použití omezení šířky pásma s Azure Resource Manager požadavky při dosáhli limity předplatného."
services: azure-resource-manager
documentationcenter: na
author: tfitzmac
manager: timlt
editor: tysonn
ms.assetid: e1047233-b8e4-4232-8919-3268d93a3824
ms.service: azure-resource-manager
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 01/26/2018
ms.author: tomfitz
ms.openlocfilehash: dc109cdaeade900e239624f408cea2a1f448ae5a
ms.sourcegitcommit: ded74961ef7d1df2ef8ffbcd13eeea0f4aaa3219
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/29/2018
---
# <a name="throttling-resource-manager-requests"></a>Omezení žádostí Resource Manager
Omezení správce prostředků pro každé předplatné a klienta, přečtěte si požadavky na 15 000 za hodinu a požadavků na zápis na 1 200 za hodinu. Tato omezení se použijí na každou instanci Azure Resource Manager. Existuje více instancí v každé oblasti Azure a Azure Resource Manager je nasazen na všechny oblasti Azure.  Tím, že v praxi, jsou omezení efektivně mnohem vyšší než tato omezení, jako uživatel požadavky jsou obvykle obsluhovány pomocí mnoha různými instancemi.

Pokud vaše aplikace nebo skriptu dosáhne tyto limity, budete muset omezení své žádosti. Tento článek ukazuje, jak určit zbývající požadavků, které máte před dosažením limitu a jak reagovat, když jste dosáhli limitu.

Když dostanete tento limit, zobrazí se stavový kód HTTP **429 příliš mnoho požadavků**.

Počet požadavků, které je vymezen na vašeho předplatného nebo tenanta. Pokud máte více souběžných zasílání požadavků v rámci vašeho předplatného, požadavky z těchto aplikací se aplikace přidávají společně k určení počet zbývajících požadavků.

Předplatné obor požadavky jsou ty, které zahrnují předávání vaše předplatné ID, jako je například načítání skupin prostředků v rámci vašeho předplatného. Požadavky klienta obor nezahrnují svoje ID předplatného, jako je například načítání platná umístění Azure.

## <a name="remaining-requests"></a>Zbývající požadavků
Počet zbývajících požadavků, které můžete zjistit tak, že prověří hlavičky odpovědi. Každý požadavek obsahuje hodnoty pro číslo zbývající čtení a zápisu požadavků. Následující tabulka popisuje hlavičky odpovědi, které můžete zkontrolovat pro tyto hodnoty:

| Hlavička odpovědi | Popis |
| --- | --- |
| x-ms-ratelimit-remaining-subscription-reads |Předplatné obor čte zbývající |
| x-ms-ratelimit-remaining-subscription-writes |Předplatné obor zapíše zbývající |
| x-ms-ratelimit-remaining-tenant-reads |Obor klienta čte zbývající |
| x-ms-ratelimit-remaining-tenant-writes |Obor klienta zapíše zbývající |
| x-ms-ratelimit-remaining-subscription-resource-requests |Předplatné obor zbývající žádosti typu prostředku.<br /><br />Hodnotu této hlavičky je vrácena pouze v případě služby má přepsat výchozí limit. Resource Manager přidá tato hodnota místo předplatné čtení nebo zápisu. |
| x-ms-ratelimit-remaining-subscription-resource-entities-read |Předplatné obor zbývající žádosti pro kolekci prostředků typu.<br /><br />Hodnotu této hlavičky je vrácena pouze v případě služby má přepsat výchozí limit. Tuto hodnotu poskytuje počet zbývajících požadavků kolekce (seznam prostředky). |
| x-ms-ratelimit-remaining-tenant-resource-requests |Klienta obor zbývající žádosti typu prostředku.<br /><br />Tuto hlavičku přidat jenom pro požadavky na úrovni klienta a pouze v případě služby má přepsat výchozí limit. Resource Manager přidá tato hodnota místo klienta čtení nebo zápisu. |
| x-ms-ratelimit-remaining-tenant-resource-entities-read |Klienta obor zbývající žádosti pro kolekci prostředků typu.<br /><br />Tuto hlavičku přidat jenom pro požadavky na úrovni klienta a pouze v případě služby má přepsat výchozí limit. |

## <a name="retrieving-the-header-values"></a>Načítání hodnoty hlavičky
Načítání tyto hodnoty hlavičky v kódu nebo skriptu je nejsou jiné než načítání žádnou hodnotu hlavičky. 

Například v **C#**, načtení hodnota hlavičky ze **HttpWebResponse** objekt s názvem **odpovědi** následujícím kódem:

```cs
response.Headers.GetValues("x-ms-ratelimit-remaining-subscription-reads").GetValue(0)
```

V **prostředí PowerShell**, načtení hodnota hlavičky z operace Invoke-WebRequest.

```powershell
$r = Invoke-WebRequest -Uri https://management.azure.com/subscriptions/{guid}/resourcegroups?api-version=2016-09-01 -Method GET -Headers $authHeaders
$r.Headers["x-ms-ratelimit-remaining-subscription-reads"]
```

Nebo, pokud chcete zobrazit zbývající požadavky pro ladění, můžete zadat **– ladění** parametr na vaše **prostředí PowerShell** rutiny.

```powershell
Get-AzureRmResourceGroup -Debug
```

Která vrací mnoho hodnot, včetně následující hodnotu odpovědi:

```powershell
...
DEBUG: ============================ HTTP RESPONSE ============================

Status Code:
OK

Headers:
Pragma                        : no-cache
x-ms-ratelimit-remaining-subscription-reads: 14999
...
```

V **rozhraní příkazového řádku Azure**, je hodnota hlavičky načíst pomocí podrobnější možnosti.

```azurecli
az group list --verbose --debug
```

Která vrací mnoho hodnot, včetně následujících objektů:

```azurecli
...
silly: returnObject
{
  "statusCode": 200,
  "header": {
    "cache-control": "no-cache",
    "pragma": "no-cache",
    "content-type": "application/json; charset=utf-8",
    "expires": "-1",
    "x-ms-ratelimit-remaining-subscription-reads": "14998",
    ...
```

## <a name="waiting-before-sending-next-request"></a>Čekání před odesláním další požadavek
Po dosažení limitu požadavků Resource Manager vrátí **429** stavový kód HTTP a **opakovat po** hodnota v hlavičce. **Opakovat po** hodnota určuje počet sekund, které má aplikace čekat (nebo spánku) před odesláním další požadavek. Pokud odešlete žádost předtím, než dojde k uplynutí hodnoty opakování, váš požadavek nebude zpracován a se vrátí novou hodnotu opakování.

## <a name="next-steps"></a>Další postup

* Další informace o omezení a kvóty najdete v tématu [předplatného Azure a omezení služby, kvóty a omezení](../azure-subscription-service-limits.md).
* Další informace o zpracování asynchronní REST požadavky najdete v tématu [sledovat asynchronní operace Azure](resource-manager-async-operations.md).
