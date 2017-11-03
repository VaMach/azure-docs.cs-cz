---
title: "Importovat a exportovat soubor zóny domény do Azure DNS pomocí Azure CLI 1.0 | Microsoft Docs"
description: "Zjistěte, jak importovat a exportovat souboru zóny DNS do Azure DNS pomocí Azure CLI 1.0"
services: dns
documentationcenter: na
author: georgewallace
manager: timlt
ms.assetid: f5797782-3005-4663-a488-ac0089809010
ms.service: dns
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 08/16/2016
ms.author: gwallace
ms.openlocfilehash: d6d3fa7aa0e8b2462b3a6b4b66d3d87ab5535314
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/11/2017
---
# <a name="import-and-export-a-dns-zone-file-using-the-azure-cli-10"></a>Import a export souboru zóny DNS pomocí Azure CLI 1.0 

Tento článek vás provede jak importovat a exportovat soubory zóny DNS pro Azure DNS pomocí Azure CLI 1.0.

## <a name="introduction-to-dns-zone-migration"></a>Úvod do migrace zóny DNS

Soubor zóny DNS je textový soubor, který obsahuje podrobnosti o každé systému DNS (Domain Name) záznamu v zóně. Postupuje standardní formát, takže je vhodné pro přenos mezi systémy DNS záznamy DNS. Použití souboru zóny je rychlé, spolehlivé a pohodlný způsob pro přenos zóny DNS do nebo z Azure DNS.

Azure DNS podporuje import a export souborů zóny pomocí rozhraní příkazového řádku Azure (CLI). Import souboru zóny je **není** aktuálně podporované pomocí prostředí Azure PowerShell nebo portálu Azure.

Azure CLI 1.0 je nástroj příkazového řádku a platformy, použít pro správu služby Azure. Je k dispozici pro platformy Windows, Mac a Linux z [stránky Azure stahování](https://azure.microsoft.com/downloads/). Podpora více platforem je důležité pro import a export souborů zóny, protože nejběžnější název serverového softwaru [vazby](https://www.isc.org/downloads/bind/), obvykle běží na systému Linux.

> [!NOTE]
> Aktuálně neexistují dvě verze rozhraní příkazového řádku Azure. CLI1.0 je založena na Node.js a má příkazy počínaje "azure".
> CLI2.0 je založena na Python a má příkazy počínaje "az". Při importu souboru zóny je podporovaná v obou verzích, doporučujeme používat příkazy CLI1.0, jak je popsáno v této stránce.

## <a name="obtain-your-existing-dns-zone-file"></a>Získat existující soubor zóny DNS

Před importem souboru zóny DNS do Azure DNS, musíte získat kopii souboru zóny. Zdroj tohoto souboru závisí na je aktuálně hostitelem zóny DNS.

* Pokud zónu DNS je hostitelem služby partnera (například doménového registrátora, vyhrazené poskytovatele hostingu DNS nebo poskytovatele alternativní cloudu), měl by poskytnout služby možnost stažení souboru zóny DNS.
* Pokud DNS systému Windows je hostitelem zóny DNS, je výchozí složku pro soubory zóny **%systemroot%\system32\dns**. Úplná cesta k souboru každé zóny také ukazuje na **Obecné** kartě konzolu DNS.
* Pokud zónu DNS je hostitelem pomocí vazby, je umístění souboru zóny pro každou zónu zadaný v konfiguračním souboru vazby **named.conf**.

> [!NOTE]
> Zóny soubory stahované z GoDaddy mají mírně nestandardní formát. Budete muset opravu před importem tyto soubory zóny do Azure DNS.
>
> Názvy DNS v RDATA každého záznamu DNS jsou určené jako plně kvalifikované názvy, ale nemají ukončující "." To znamená, že se interpretují jinými systémy DNS jako relativních názvů. Je nutné upravit soubor zóny připojit ukončení "." na jejich názvy před importem do Azure DNS.
>
> Například by mělo být změněno záznam CNAME "v doméně contoso.com CNAME www 3600" na "www 3600 v doméně contoso.com CNAME."
> (s ukončující ".").

## <a name="import-a-dns-zone-file-into-azure-dns"></a>Import souboru zóny DNS do Azure DNS

Import souboru zóny vytvoří nové zóny v Azure DNS, pokud ještě neexistuje. Pokud zóna již existuje, je potřeba sloučit sady záznamů v souboru zóny s existující sady záznamů.

### <a name="merge-behavior"></a>Sloučení chování

* Ve výchozím nastavení jsou sloučeny stávající a nové sady záznamů. Jsou identické záznamy v sadě záznamů sloučené zrušte duplicitní.
* Můžete také zadáním `--force` možnost, nahradí proces importu, který se stávajícím záznamem, nastaví se nové sady záznamů. Existující sady záznamů, které nemají odpovídající záznam v souboru importované zóny se neodeberou.
* Když sady záznamů jsou sloučeny, použije se čas live (TTL) z dříve existující sady záznamů. Když `--force` se používá, se používá hodnota TTL nové sady záznamů.
* Začátek parametry záznam Authority (SOA) (s výjimkou `host`) jsou vždy převzat ze souboru importované zóny, bez ohledu na to, jestli se `--force` se používá. Pro záznam názvového serveru nastavit ve vrcholu zóny, se podobně interval TTL, ZÍSKÁ bodů vždy převzat ze souboru importované zóny.
* Importovaný záznam CNAME nenahrazuje existující záznam CNAME se stejným názvem, pokud `--force` je zadán parametr.
* Když dojde ke konfliktu mezi záznam CNAME a jiný záznam stejným názvem, ale jiný typ (bez ohledu na to, který je existující nebo nové), se zachová stávající záznam. Je nezávislé na použití `--force`.

### <a name="additional-information-about-importing"></a>Další informace o importu

Následující poznámky k poskytují další technické podrobnosti o zóny importu.

* `$TTL` – Direktiva je volitelný a je podporované. Pokud ne `$TTL` – direktiva je zadána, jsou záznamy bez explicitního TTL importované nastaven na výchozí hodnotu TTL 3 600 sekund. Když dva záznamy v sadě záznamů stejnou zadat jiný TTLs, použije se nižší hodnotu.
* `$ORIGIN` – Direktiva je volitelný a je podporované. Pokud ne `$ORIGIN` není nastaven, je výchozí hodnota používaná pro název zóny jako zadaného na příkazovém řádku (plus ukončení ".").
* `$INCLUDE` a `$GENERATE` direktivy nejsou podporovány.
* Jsou podporovány těchto typů záznamů: A, AAAA, CNAME, MX, NS, SOA, SRV a TXT.
* Záznam SOA je vytvářena automaticky nástrojem Azure DNS, když je vytvořena zóna. Když importujete soubor zóny, jsou všechny parametry SOA převzat ze souboru zóny *s výjimkou* `host` parametr. Tento parametr používá hodnotu poskytovanou infrastrukturou Azure DNS. Je to proto, že tento parametr musí odkazovat na název primárního serveru poskytuje Azure DNS.
* Záznam názvového serveru nastavit ve vrcholu zóny se také automaticky vytvoří Azure DNS při vytváření zóny. Je importovat pouze hodnotu TTL této sady záznamů. Tyto záznamy obsahovat názvy názvových serverů poskytuje Azure DNS. Záznam dat není přepsány hodnoty obsažené v souboru importované zóny.
* Během verzi Public Preview Azure DNS podporuje pouze jeden řetězec záznamů TXT. Jsou nahrazován záznamů TXT zřetězených a zkrácen na 255 znaků.

### <a name="cli-format-and-values"></a>Formát rozhraní příkazového řádku a hodnoty

Formát příkazu příkazového řádku Azure k importu zóny DNS je:

```azurecli
azure network dns zone import [options] <resource group> <zone name> <zone file name>
```

Hodnoty:

* `<resource group>`je název skupiny prostředků pro zónu v Azure DNS.
* `<zone name>`je název zóny.
* `<zone file name>`je cesta nebo název souboru zóny určených k importu.

Pokud zóna s tímto názvem neexistuje ve skupině prostředků, vytvoří se pro vás. Pokud zóna již existuje, importované sady záznamů jsou sloučeny s existující sady záznamů. Pokud chcete přepsat existující sady záznamů, použijte `--force` možnost.

K ověření formát souboru zóny bez provedení ve skutečnosti importu, použijte `--parse-only` možnost.

### <a name="step-1-import-a-zone-file"></a>Krok 1. Importovat soubor zóny

Chcete-li importovat soubor zóny pro zónu **contoso.com**.

1. Přihlaste se k předplatnému Azure pomocí Azure CLI 1.0.

    ```azurecli
    azure login
    ```

2. Vyberte předplatné, kde chcete vytvořit novou zónu DNS.

    ```azurecli
    azure account set <subscription name>
    ```

3. Azure DNS je jen správce prostředků služby Azure, takže Azure CLI musí přepnout do režimu Resource Manager.

    ```azurecli
    azure config mode arm
    ```

4. Než použijete službu Azure DNS, je nutné zaregistrovat vaše předplatné používalo poskytovatel prostředků Microsoft.Network. (Jde o jednorázovou operaci u každého předplatného.)

    ```azurecli
    azure provider register Microsoft.Network
    ```

5. Pokud nemáte již, musíte taky vytvořit skupinu prostředků Resource Manager.

    ```azurecli
    azure group create myresourcegroup westeurope
    ```

6. Chcete-li importovat zóny **contoso.com** ze souboru **contoso.com.txt** do nové zóny DNS ve skupině prostředků **myresourcegroup**, spusťte příkaz `azure network dns zone import`.<BR>Tento příkaz načte soubor zóny a analyzovat ho. Příkaz spustí řadu příkazů ve službě Azure DNS k vytvoření zóny a sady všech záznamů v zóně. Příkaz sestavy průběhu v okně konzoly společně s žádné chyby nebo upozornění. Protože sady záznamů jsou vytvářeny v řadě, může trvat několik minut pro import souboru velké zóny.

    ```azurecli
    azure network dns zone import myresourcegroup contoso.com contoso.com.txt
    ```

### <a name="step-2-verify-the-zone"></a>Krok 2. Ověřte zóny

Ověření zónu DNS po importu souboru, můžete použít jednu z následujících metod:

* Pomocí následujícího příkazu příkazového řádku Azure můžete vytvořit seznam záznamů:

    ```azurecli
    azure network dns record-set list myresourcegroup contoso.com
    ```

* Můžete vytvořit seznam záznamů pomocí rutiny prostředí PowerShell `Get-AzureRmDnsRecordSet`.
* Můžete použít `nslookup` ověření překlad názvů pro záznamy. Protože ještě není přidělena zóny, budete muset explicitně zadat správné názvových serverů Azure DNS. Následující příklad ukazuje, jak načíst názvy názvových serverů, které jsou přiřazeny k zóně. IT také ukazuje postup dotazování záznamů "www" pomocí `nslookup`.

        C:\>azure network dns record-set show myresourcegroup contoso.com @ NS
        info:Executing command network dns record-set show
        + Looking up the DNS Record Set "@" of type "NS"
        data:Id: /subscriptions/.../resourceGroups/myresourcegroup/providers/Microsoft.Network/dnszones/contoso.com/NS/@
        data:Name: @
        data:Type: Microsoft.Network/dnszones/NS
        data:Location: global
        data:TTL : 3600
        data:NS records
        data:Name server domain name : ns1-01.azure-dns.com
        data:Name server domain name : ns2-01.azure-dns.net
        data:Name server domain name : ns3-01.azure-dns.org
        data:Name server domain name : ns4-01.azure-dns.info
        data:
        info:network dns record-set show command OK

        C:\> nslookup www.contoso.com ns1-01.azure-dns.com

        Server: ns1-01.azure-dns.com
        Address:  40.90.4.1

        Name:www.contoso.com
        Addresses:  134.170.185.46
        134.170.188.221

### <a name="step-3-update-dns-delegation"></a>Krok 3. Aktualizovat delegování DNS

Po ověření, že správně naimportované zóny, potřebujete aktualizovat delegování DNS tak, aby odkazoval na názvové servery Azure DNS. Další informace najdete v článku [aktualizovat delegování DNS](dns-domain-delegation.md).

## <a name="export-a-dns-zone-file-from-azure-dns"></a>Exportovat souboru zóny DNS z Azure DNS.

Formát příkazu příkazového řádku Azure k importu zóny DNS je:

```azurecli
azure network dns zone export [options] <resource group> <zone name> <zone file name>
```

Hodnoty:

* `<resource group>`je název skupiny prostředků pro zónu v Azure DNS.
* `<zone name>`je název zóny.
* `<zone file name>`je cesta nebo název souboru zóny export.

Jako v zóně importu můžete nejprve nutné se přihlásit, zvolte vaše předplatné a konfigurace rozhraní příkazového řádku Azure pro použití režimu Resource Manager.

### <a name="to-export-a-zone-file"></a>Chcete-li exportovat soubor zóny

1. Přihlaste se k předplatnému Azure pomocí rozhraní příkazového řádku Azure.

    ```azurecli
    azure login
    ```

2. Vyberte předplatné, kde chcete vytvořit zónu DNS.

    ```azurecli
    azure account set <subscription name>
    ```

3. Azure DNS je služba Azure Resource Manager. Azure CLI musí přepnout do režimu Resource Manager.

    ```azurecli
    azure config mode arm
    ```

4. Chcete-li exportovat stávající zóny DNS **contoso.com** ve skupině prostředků **myresourcegroup** do souboru **contoso.com.txt** (v aktuální složce), spusťte `azure network dns zone export`. Tento příkaz volá službu Azure DNS vytvořit výčet sady záznamů v zóně a exportovat výsledky do souboru zóny vazby kompatibilní.

    ```azurecli
    azure network dns zone export myresourcegroup contoso.com contoso.com.txt
    ```
