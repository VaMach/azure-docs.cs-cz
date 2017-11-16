---
title: "Správa záznamů DNS v Azure DNS pomocí Azure CLI 1.0 | Microsoft Docs"
description: "Správa sad záznamů DNS a záznamy v Azure DNS při hostování vaší domény ve službě Azure DNS. Všechny příkazy 1.0 rozhraní příkazového řádku pro operace na sady záznamů a záznamy."
services: dns
documentationcenter: na
author: jtuliani
manager: carmonm
ms.assetid: 5356a3a5-8dec-44ac-9709-0c2b707f6cb5
ms.service: dns
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 12/20/2016
ms.author: jonatul
ms.openlocfilehash: 76473349b2c6121d8f289a86b46c10a8b8697b34
ms.sourcegitcommit: afc78e4fdef08e4ef75e3456fdfe3709d3c3680b
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/16/2017
---
# <a name="manage-dns-records-in-azure-dns-using-the-azure-cli-10"></a>Správa záznamů DNS v Azure DNS pomocí Azure CLI 1.0

> [!div class="op_single_selector"]
> * [Azure Portal](dns-operations-recordsets-portal.md)
> * [Azure CLI 1.0](dns-operations-recordsets-cli-nodejs.md)
> * [Azure CLI 2.0](dns-operations-recordsets-cli.md)
> * [PowerShell](dns-operations-recordsets.md)

Tento článek ukazuje, jak spravovat záznamy DNS pro zónu DNS s použitím různé platformy Azure rozhraní příkazového řádku (CLI), která je k dispozici pro Windows, Mac a Linux. Můžete také spravovat svoje záznamy DNS pomocí [prostředí Azure PowerShell](dns-operations-recordsets.md) nebo [portál Azure](dns-operations-recordsets-portal.md).

## <a name="cli-versions-to-complete-the-task"></a>Verze rozhraní příkazového řádku pro dokončení úlohy

K dokončení úlohy můžete využít jednu z následujících verzí rozhraní příkazového řádku:

* [Azure CLI 1.0](dns-operations-recordsets-cli-nodejs.md) – naše rozhraní příkazového řádku pro klasické modely nasazení a modely nasazení správy prostředků
* [Azure CLI 2.0](dns-operations-recordsets-cli.md) – naše rozhraní příkazového řádku nové generace pro model nasazení správy prostředků

V příkladech v tomto článku předpokládá, že už máte [nainstalovat Azure CLI 1.0, přihlášení a vytvořit zónu DNS](dns-operations-dnszones-cli-nodejs.md).

## <a name="introduction"></a>Úvod

Před vytvářením záznamů DNS v DNS Azure je nejprve nutné pochopit, jak DNS Azure organizuje záznamy DNS v sadách záznamů DNS.

[!INCLUDE [dns-about-records-include](../../includes/dns-about-records-include.md)]

Další informace o záznamech DNS v DNS Azure najdete v tématu [Zóny a záznamy DNS](dns-zones-records.md).

## <a name="create-a-dns-record"></a>Vytvoření záznamu DNS

K vytvoření záznamu DNS použijte příkaz `azure network dns record-set add-record`. Nápovědu získáte příkazem `azure network dns record-set add-record -h`.

Při vytváření záznamu je třeba zadat název skupiny prostředků, název zóny, název sady záznamů a podrobnosti o vytvářeném záznamu. Musí být zadaný název sady záznamů *relativní* název, což znamená, je nutné vyloučit název zóny.

Pokud sada záznamů ještě neexistuje, tento příkaz ji vytvoří. Pokud již existuje sada záznamů, adda tento příkaz, který zadáte do stávajícího záznamu sady záznamu.

Pokud se vytváří nová sada záznamů, použije se výchozí hodnota TTL (Time to Live) 3 600. Pokyny týkající se používání různých TTLs najdete v tématu [vytvoření sady záznamů DNS](#create-a-dns-record-set).

Následující příklad vytvoří záznam A s názvem *www* v zóně *contoso.com* ve skupině prostředků *MyResourceGroup*. IP adresa záznamu A je *1.2.3.4*.

```azurecli
azure network dns record-set add-record MyResourceGroup contoso.com www A -a 1.2.3.4
```

Vytvoří záznam na vrcholu zóny (v tomto případě "contoso.com"), použijte název záznamu "@", včetně uvozovek:

```azurecli
azure network dns record-set add-record MyResourceGroup contoso.com "@" A -a 1.2.3.4
```

## <a name="create-a-dns-record-set"></a>Vytvoření sady záznamů DNS

V uvedených příkladech záznam DNS buď přidala do existující sady záznamů, nebo bylo vytvořeno sady záznamů *implicitně*. Můžete také vytvořit sadu záznamů *explicitně* před přidání záznamů do ní. Azure DNS podporuje 'prázdné, sady záznamů, které může fungovat jako zástupný symbol rezervovat název DNS před vytvořením záznamy DNS. Prázdný sady záznamů jsou viditelné v rovině řízení Azure DNS, ale nejsou zobrazeny na názvových serverů Azure DNS.

Sady záznamů jsou vytvořené pomocí `azure network dns record-set create` příkaz. Nápovědu získáte příkazem `azure network dns record-set create -h`.

Vytvoření záznamu explicitně nastaven umožňuje určit vlastnosti sady záznamů, jako [Time To Live (TTL)](dns-zones-records.md#time-to-live) a metadata. [Sady záznamů metadat](dns-zones-records.md#tags-and-metadata) lze přidružit každá sada záznamů specifická data jako páry klíč hodnota.

Následující příklad vytvoří záznam prázdný nastavit hodnotu TTL 60 sekund, pomocí `--ttl` parametr (krátký tvar `-l`):

```azurecli
azure network dns record-set create MyResourceGroup contoso.com www A --ttl 60
```

Následující příklad vytvoří záznamů s dvě položky metadat "oddělení = finanční" a "prostředí = produkční", pomocí `--metadata` parametr (krátkodobých formuláře `-m`):

```azurecli
azure network dns record-set create MyResourceGroup contoso.com www A --metadata "dept=finance;environment=production"
```

Vytvoření prázdná sada záznamů záznamy lze přidat pomocí `azure network dns record-set add-record` jak je popsáno v [vytvořit záznam DNS](#create-a-dns-record).

## <a name="create-records-of-other-types"></a>Vytvořte záznamy jiné typy

S zobrazená v podrobnosti o tom, jak vytvořit záznamy 'A', následující příklady ukazují, jak vytvořit záznam jiné typy záznamů nepodporuje Azure DNS.

Parametry použité k zadání dat záznamu se liší podle typu záznamu. Například pro záznam typu A zadáváte adresu IPv4 pomocí parametru `-a <IPv4 address>`. Parametry pro každý typ záznamu může být uvedený pomocí `azure network dns record-set add-record -h`.

V každém případě ukážeme, jak vytvořit jeden záznam. Záznam je přidat do existující sady záznamů, nebo sadu záznamů vytvořena implicitně. Další informace o vytvoření sady záznamů a definování záznam nastavte parametr explicitně najdete v tématu [vytvoření sady záznamů DNS](#create-a-dns-record-set).

Jsme neudělujte příklad se vytvořit sadu záznamů SOA, protože se vytvářejí SOAs a odstranit s každou zónou DNS a nelze vytvořit nebo odstranit samostatně. Ale [SOA lze upravit, jak je znázorněno v příkladu novější](#to-modify-an-SOA-record).

### <a name="create-an-aaaa-record"></a>Vytvoření záznamů AAAA

```azurecli
azure network dns record-set add-record MyResourceGroup contoso.com test-aaaa AAAA --ipv6-address 2607:f8b0:4009:1803::1005
```

### <a name="create-a-caa-record"></a>Vytvořte záznam KÁ

Následující příklad ukazuje, jak vytvořit záznam KÁ. 

```azurecli
azure network dns record-set add-record MyResourceGroup contoso.com ca1 CAA --caaflags 0 --caatag issue --caavalue "ca1.contoso.com"
```

### <a name="create-a-cname-record"></a>Vytvořte záznam CNAME

> [!NOTE]
> Standardech DNS nepovoluje záznamy CNAME na vrcholu zóny (`-Name "@"`), ani se povolit sad záznamů obsahující více než jeden záznam.
> 
> Další informace najdete v tématu [záznamy CNAME](dns-zones-records.md#cname-records).

```azurecli
azure network dns record-set add-record  MyResourceGroup contoso.com  test-cname CNAME --cname www.contoso.com
```

### <a name="create-an-mx-record"></a>Vytvoření záznamů MX

V tomto příkladu vytvoříme s využitím názvu sady záznamů „@“ záznam MX ve vrcholu zóny (v tomto „contoso.com“).

```azurecli
azure network dns record-set add-record MyResourceGroup contoso.com  "@" MX --exchange mail.contoso.com --preference 5
```

### <a name="create-an-ns-record"></a>Vytvoření záznamů NS

```azurecli
azure network dns record-set add-record MyResourceGroup  contoso.com  test-ns NS --nsdname ns1.contoso.com
```

### <a name="create-a-ptr-record"></a>Vytvořit záznam PTR

V takovém případě se Moje-arpa-server zone.com' představuje zóny ARPA představující rozsahu IP adres. Každá sada záznamů PTR v této zóně odpovídá IP adrese v rámci tohoto rozsahu IP adres.  Název záznamu "10" je poslední oktet IP adresy v tomto rozsahu IP reprezentována tento záznam.

```azurecli
azure network dns record-set add-record MyResourceGroup my-arpa-zone.com "10" PTR --ptrdname "myservice.contoso.com"
```

### <a name="create-an-srv-record"></a>Vytvoření záznamů SRV

Při vytváření [sady záznamů SRV](dns-zones-records.md#srv-records), zadejte  *\_služby* a  *\_protokol* v názvu sady záznamů. Není nutné zahrnout "@" v názvu sady záznamů při vytváření záznamu SRV nastavit ve vrcholu zóny.

```azurecli
azure network dns record-set add-record MyResourceGroup contoso.com  "_sip._tls" SRV --priority 10 --weight 5 --port 8080 --target "sip.contoso.com"
```

### <a name="create-a-txt-record"></a>Vytvořit záznam TXT

Následující příklad ukazuje, jak vytvořit záznam TXT. Další informace o maximální délku řetězce v záznamů TXT podporovány, naleznete v části [záznamů TXT](dns-zones-records.md#txt-records).

```azurecli
azure network dns record-set add-record MyResourceGroup contoso.com test-txt TXT --text "This is a TXT record"
```

## <a name="get-a-record-set"></a>Získání sady záznamů

Chcete-li načíst existující sady záznamů, použijte `azure network dns record-set show`. Nápovědu získáte příkazem `azure network dns record-set show -h`.

Jako při vytváření záznamu nebo sady záznamů, musí být zadaný název sady záznamů *relativní* název, což znamená, je nutné vyloučit název zóny. Také budete muset zadat typ záznamu, zónu, která obsahuje sady záznamů a skupinu prostředků obsahující zóny.

Následující příklad načte záznam *www* a typ ze zóny *contoso.com* ve skupině prostředků *MyResourceGroup*:

```azurecli
azure network dns record-set show MyResourceGroup contoso.com www A
```

## <a name="list-record-sets"></a>Seznam sad záznamů

Můžete vytvořit seznam všech záznamů v zóně DNS pomocí `azure network dns record-set list` příkaz. Nápovědu získáte příkazem `azure network dns record-set list -h`.

Tento příklad vrátí sady všech záznamů v zóně *contoso.com*, ve skupině prostředků *MyResourceGroup*bez ohledu na název a typ záznamu:

```azurecli
azure network dns record-set list MyResourceGroup contoso.com
```

Tento příklad vrátí všechny sady záznamů, které odpovídají daného typu záznamu (v tomto případě "A" záznamů):

```azurecli
azure network dns record-set list MyResourceGroup contoso.com --type A
```

## <a name="add-a-record-to-an-existing-record-set"></a>Přidání záznamu do existující sady záznamů

Můžete použít `azure network dns record-set add-record` i k vytvoření záznamu v sadě záznamů nové nebo přidání záznamu do existující sady záznamů.

Další informace najdete v tématu [vytvořit záznam DNS](#create-a-dns-record) a [vytvořit záznamy jiné typy](#create-records-of-other-types) výše.

## <a name="remove-a-record-from-an-existing-record-set"></a>Odeberte záznam z existující sady záznamů.

Chcete-li odebrat záznam DNS z existující sady záznamů, použijte `azure network dns record-set delete-record`. Nápovědu získáte příkazem `azure network dns record-set delete-record -h`.

Tento příkaz odstraní záznam DNS ze sady záznamů. Pokud se odstraní poslední záznam v sadě záznamů, samotné sady záznamů je **není** odstranit. Místo toho je ponecháno prázdná sada záznamů. Pokud chcete odstranit místo sady záznamů, najdete v části [odstranit sadu záznamů](#delete-a-record-set).

Je třeba zadat odstranit záznam a zóna je nutné ji odstranit, pomocí stejné parametrů jako při vytváření záznamu pomocí `azure network dns record-set add-record`. Tyto parametry jsou popsané v [vytvořit záznam DNS](#create-a-dns-record) a [vytvořit záznamy jiné typy](#create-records-of-other-types) výše.

Tento příkaz zobrazí výzvu k potvrzení. Tuto výzvu lze potlačit pomocí `--quiet` přepínače (krátký tvar `-q`).

Následující příklad odstraní záznam A s hodnotou '1.2.3.4"ze záznamu nastavení s názvem *www* v zóně *contoso.com*, ve skupině prostředků *MyResourceGroup*. Je potlačeno výzvu k potvrzení.

```azurecli
azure network dns record-set delete-record MyResourceGroup contoso.com www A -a 1.2.3.4 --quiet
```

## <a name="modify-an-existing-record-set"></a>Upravit existující sady záznamů

Každá sada záznamů obsahuje [time to live (TTL)](dns-zones-records.md#time-to-live), [metadata](dns-zones-records.md#tags-and-metadata)a záznamy DNS. Následující části popisují postup úpravy každé z těchto vlastností.

### <a name="to-modify-an-a-aaaa-mx-ns-ptr-srv-or-txt-record"></a>Úprava záznamu A, AAAA, MX, NS, PTR, SRV a TXT

Pokud chcete upravit existující záznam typu A, AAAA, MX, NS, PTR, SRV nebo TXT, by měl nejprve přidejte nový záznam a pak odstraňte stávající záznam. Podrobné pokyny o tom, jak odstranit a přidat záznamy najdete v předchozích částech tohoto článku.

Následující příklad ukazuje, jak upravit záznam "A" z IP adresy 1.2.3.4 na IP adresu 5.6.7.8:

```azurecli
azure network dns record-set add-record MyResourceGroup contoso.com www A -a 5.6.7.8
azure network dns record-set delete-record MyResourceGroup contoso.com www A -a 1.2.3.4
```

### <a name="to-modify-a-cname-record"></a>Chcete-li upravit záznam CNAME

Chcete-li upravit záznam CNAME, použijte `azure network dns record-set add-record` přidat nová hodnota záznamu. Na rozdíl od jiných typů záznamů sadu záznamů CNAME obsahovat jenom jeden záznam. Proto je stávajícího záznamu *nahradit* když nový záznam je přidána a není nutné odstranit samostatně.  Zobrazí se výzva k přijmout tento nahrazení.

V příkladu upraví sady záznamů CNAME *www* v zóně *contoso.com*, ve skupině prostředků *MyResourceGroup*, přejděte na položku "www.fabrikam.net" namísto jeho existující hodnoty:

```azurecli
azure network dns record-set add-record MyResourceGroup contoso.com www CNAME --cname www.fabrikam.net
``` 

### <a name="to-modify-an-soa-record"></a>Úprava záznamu SOA

Použití `azure network dns record-set set-soa-record` k úpravě SOA pro danou zónu DNS. Nápovědu získáte příkazem `azure network dns record-set set-soa-record -h`.

Následující příklad ukazuje, jak nastavit vlastnost 'e-mailu, záznamu SOA zóny *contoso.com* ve skupině prostředků *MyResourceGroup*:

```azurecli
azure network dns record-set set-soa-record rg1 contoso.com --email admin.contoso.com
```


### <a name="to-modify-ns-records-at-the-zone-apex"></a>Chcete-li upravit NS záznamů ve vrcholu zóny

S každou zónou DNS se automaticky vytvoří záznam NS nastavit ve vrcholu zóny. Obsahuje názvy názvových serverů Azure DNS přiřadit do zóny.

Můžete přidat další název nastavit servery na tento záznam NS, podporuje společné hosting domén s více než jednoho poskytovatele DNS. Můžete také upravit TTL a metadat pro tuto sadu záznamů. Však nelze odebrat ani změnit předem vyplněná názvových serverů Azure DNS.

Všimněte si, že vztahuje se pouze na vrcholu zóny sady záznamů NS. Jiné sady záznamů NS v pásmu (jak je používá delegovat podřízených zónách) můžete změnit bez omezení.

Následující příklad ukazuje, jak přidat do sady ve vrcholu zóny záznamů NS k další název serveru:

```azurecli
azure network dns record-set add-record MyResourceGroup contoso.com "@" --nsdname ns1.myotherdnsprovider.com 
```

### <a name="to-modify-the-ttl-of-an-existing-record-set"></a>Chcete-li změnit hodnotu TTL existující sady záznamů

Chcete-li změnit hodnotu TTL existující sady záznamů, použijte `azure network dns record-set set`. Nápovědu získáte příkazem `azure network dns record-set set -h`.

Následující příklad ukazuje, jak upravit sadu záznamů TTL, v takovém případě na 60 sekund:

```azurecli
azure network dns record-set set MyResourceGroup contoso.com www A --ttl 60
```

### <a name="to-modify-the-metadata-of-an-existing-record-set"></a>Chcete-li upravit metadata existující sady záznamů

[Sady záznamů metadat](dns-zones-records.md#tags-and-metadata) lze přidružit každá sada záznamů specifická data jako páry klíč hodnota. Chcete-li upravit metadata existující sady záznamů, použijte `azure network dns record-set set`. Nápovědu získáte příkazem `azure network dns record-set set -h`.

Následující příklad ukazuje, jak upravit záznamů s dvě položky metadat "oddělení = finanční" a "prostředí = produkční", pomocí `--metadata` parametr (krátkodobých formuláře `-m`). Všimněte si, že všechny existující metadata jsou *nahradit* podle zadané hodnoty.

```azurecli
azure network dns record-set set MyResourceGroup contoso.com www A --metadata "dept=finance;environment=production"
```

## <a name="delete-a-record-set"></a>Odstranit sadu záznamů

Sady záznamů lze odstranit pomocí `azure network dns record-set delete` příkaz. Nápovědu získáte příkazem `azure network dns record-set delete -h`. Odstranění sady záznamů se také odstraní všechny záznamy v sadě záznamů.

> [!NOTE]
> Nelze odstranit, SOA a sady záznamů NS ve vrcholu zóny (`-Name "@"`).  Tyto soubory jsou vytvořeny automaticky při zóny byl vytvořen a jsou automaticky odstraněna při odstranění zóny.

Následující příklad odstraní záznam nastavení s názvem *www* a typ ze zóny *contoso.com* ve skupině prostředků *MyResourceGroup*:

```azurecli
azure network dns record-set delete MyResourceGroup contoso.com www A
```

Zobrazí se výzva k potvrzení operace odstranění. Chcete-li potlačit tuto výzvu, použijte `--quiet` přepínače (krátký tvar `-q`).

## <a name="next-steps"></a>Další kroky

Další informace o [zóny a záznamy v Azure DNS](dns-zones-records.md).
<br>
Zjistěte, jak [chránit zóny a záznamy](dns-protect-zones-recordsets.md) při použití Azure DNS.
