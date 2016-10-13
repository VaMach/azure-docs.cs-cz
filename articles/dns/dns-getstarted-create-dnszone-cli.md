<properties
   pageTitle="Vytvoření zóny DNS pomocí rozhraní příkazového řádku | Microsoft Azure"
   description="Naučte se krok za krokem vytvářet zóny DNS pro Azure DNS a začněte svoji doménu DNS hostovat pomocí rozhraní příkazového řádku"
   services="dns"
   documentationCenter="na"
   authors="sdwheeler"
   manager="carmonm"
   editor=""/>

<tags
   ms.service="dns"
   ms.devlang="na"
   ms.topic="hero-article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="08/16/2016"
   ms.author="sewhee"/>


# Vytvoření zóny DNS pomocí rozhraní příkazového řádku


> [AZURE.SELECTOR]
- [Azure Portal](dns-getstarted-create-dnszone-portal.md)
- [PowerShell](dns-getstarted-create-dnszone.md)
- [Azure CLI](dns-getstarted-create-dnszone-cli.md)


Tento článek vás provede jednotlivými kroky při vytváření zóny DNS pomocí rozhraní příkazového řádku. Zónu DNS taky můžete vytvořit pomocí prostředí PowerShell nebo Portálu Azure.

[AZURE.INCLUDE [dns-create-zone-about](../../includes/dns-create-zone-about-include.md)]


## Než začnete

Tyto pokyny používají rozhraní příkazového řádku Microsoft Azure CLI. Nezapomeňte aktualizovat na nejnovější rozhraní příkazového řádku Azure CLI (0.9.8 nebo novější), abyste mohli používat příkazy Azure DNS. Pokud chcete zkontrolovat, která verze rozhraní příkazového řádku Azure CLI je na počítači aktuálně nainstalovaná, napište `azure -v`.

## Krok 1 – Nastavení rozhraní příkazového řádku Azure CLI

### 1. Instalace rozhraní příkazového řádku Azure CLI

Můžete nainstalovat rozhraní příkazového řádku Azure CLI pro Windows, Linux nebo MAC. Je nutné provést následující kroky, abyste mohli spravovat Azure DNS pomocí rozhraní příkazového řádku Azure CLI. Další informace najdete v tématu [Instalace rozhraní příkazového řádku Azure CLI](../xplat-cli-install.md). Příkazy DNS vyžadují rozhraní příkazového řádku Azure CLI verze 0.9.8 nebo novější.

Všechny příkazy poskytovatele sítě v rozhraní příkazového řádku se dají najít tímto příkazem:

    azure network

### 2. Přepnutí režimu rozhraní příkazového řádku

Azure DNS používá Azure Resource Manager. Nezapomeňte přepnout režim rozhraní příkazového řádku tak, aby se používaly příkazy ARM.

    azure config mode arm

### 3. Přihlášení k účtu Azure

Zobrazí se výzva k ověření pomocí přihlašovacích údajů. Nezapomeňte, že lze použít pouze účty ORGID.

    azure login -u "username"

### 4. Výběr předplatného

Zvolte předplatné Azure, které chcete použít.

    azure account set "subscription name"

### 5. Vytvoření skupiny prostředků

Azure Resource Manager vyžaduje, aby všechny skupiny prostředků určily umístění. To slouží jako výchozí umístění pro prostředky v příslušné skupině prostředků. Všechny prostředky DNS jsou ale globální, a ne místní, takže volba umístění skupiny prostředků nemá na Azure DNS žádný vliv.

Pokud používáte některou ze stávajících skupin prostředků, můžete tento krok přeskočit.

    azure group create -n myresourcegroup --location "West US"


### 6. Registrace

Službu Azure DNS spravuje poskytovatel prostředků Microsoft.Network. Abyste mohli používat Azure DNS, je nutné zaregistrovat předplatné Azure k používání tohoto poskytovatele prostředků. Jedná se o jednorázovou operaci u každého odběru.

    azure provider register --namespace Microsoft.Network


## Krok 2 – Vytvoření zóny DNS

Zóna DNS se vytvoří příkazem `azure network dns zone create`. Pokud chcete, můžete zónu DNS vytvořit společně se značkami. Značky jsou páry název-hodnota, které Azure Resource Manager používá k označování prostředků pro účely fakturace nebo seskupování. Další informace o značkách najdete v tématu [Použití značek k uspořádání prostředků Azure](../resource-group-using-tags.md).

V Azure DNS je nutné zadávat názvy zón bez závěrečné tečky **„.“**. Například jako „**contoso.com**“ místo „**contoso.com.**“.


### Postup vytvoření zóny DNS

Následující příklad vytvoří zónu DNS s názvem *contoso.com* ve skupině prostředků s názvem *MyResourceGroup*.

Nahraďte hodnoty vlastními a použijte tento příklad k vytvoření zóny DNS.

    azure network dns zone create myresourcegroup contoso.com

### Postup vytvoření zóny DNS a značek

Rozhraní příkazového řádku Azure DNS podporuje určení značek zón DNS pomocí volitelného parametru *-Tag*. Následující příklad ukazuje, jak vytvořit zónu DNS se dvěma značkami: project = demo a env = test.

Nahraďte hodnoty vlastními a použijte níže uvedený příklad k vytvoření zóny DNS a značek.

    azure network dns zone create myresourcegroup contoso.com -t "project=demo";"env=test"

## Zobrazení záznamů

Při vytvoření zóny DNS se taky vytvoří tyto záznamy DNS:

- Záznam Start of Authority (SOA). Tento záznam se nachází v kořenovém adresáři každé zóny DNS.

- Záznamy autoritativního názvového serveru (NS). Tyto záznamy ukazují, které názvové servery zónu hostují. Azure DNS používá fond názvových serverů, takže různým zónám se můžou v Azure DNS přiřadit různé názvové servery. Další informace najdete v tématu [Delegování domény do Azure DNS](dns-domain-delegation.md).

Pokud chcete tyto záznamy zobrazit, použijte `azure network dns-record-set show`.<BR>
*Použití: network dns record-set show <resource-group> <dns-zone-name> <name> <type>*


Pokud tento příkaz v níže uvedeném příkladu spustíte se skupinou prostředků *myresourcegroup*, názvem sady záznamů *„@“* (pro kořenový záznam) a typem *SOA*, získáte tento výstup:


    azure network dns record-set show myresourcegroup "contoso.com" "@" SOA
    info:    Executing command network dns-record-set show
    + Looking up the DNS record set "@"
    data:    Id                              : /subscriptions/#######################/resourceGroups/myresourcegroup/providers/Microsoft.Network/dnszones/contoso.com/SOA/@
    data:    Name                            : @
    data:    Type                            : Microsoft.Network/dnszones/SOA
    data:    Location                        : global
    data:    TTL                             : 3600
    data:    SOA record:
    data:      Email                         : msnhst.microsoft.com
    data:      Expire time                   : 604800
    data:      Host                          : edge1.azuredns-cloud.net
    data:      Minimum TTL                   : 300
    data:      Refresh time                  : 900
    data:      Retry time                    : 300
    data:                                    :
<BR>
Pokud chcete zobrazit záznamy NS vytvořené se zónou, použijte tento příkaz:

    azure network dns record-set show myresourcegroup "contoso.com" "@" NS
    info:    Executing command network dns-record-set show
    + Looking up the DNS record set "@"
    data:    Id                              : /subscriptions/#######################/resourceGroups/myresourcegroup/providers/Microsoft.Network/dnszones/contoso.com/NS/@
    data:    Name                            : @
    data:    Type                            : Microsoft.Network/dnszones/NS
    data:    Location                        : global
    data:    TTL                             : 3600
    data:    NS records
    data:        Name server domain name     : ns1-05.azure-dns.com
    data:        Name server domain name     : ns2-05.azure-dns.net
    data:        Name server domain name     : ns3-05.azure-dns.org
    data:        Name server domain name     : ns4-05.azure-dns.info
    data:
    info:    network dns-record-set show command OK

>[AZURE.NOTE] Sady záznamů v kořenovém adresáři (tj. *vrcholu*) zóny DNS používají jako název sady záznamu **@**.

## Test

Zónu DNS můžete otestovat pomocí nástrojů DNS, jako je například nslookup, DIG nebo rutina `Resolve-DnsName` prostředí PowerShell.

Pokud jste doménu ještě nedelegovali k používání nové zóny v Azure DNS, je nutné dotaz DNS nasměrovat přímo na jeden z názvových serverů zóny. Názvové servery zóny jsou uvedené v záznamech NS, jejichž seznam se dá zobrazit příkazem „azure network dns record-set show“, viz výše. V níže uvedeném příkazu nezapomeňte dosadit správné hodnoty zóny.

Následující příklad používá DIG k dotazu na doménu contoso.com pomocí názvových serverů přiřazených zóně DNS. Dotaz musí ukazovat na názvový server, u kterého jsme použili *@<name server for the zone>*, a název zóny, který používá DIG.

     <<>> DiG 9.10.2-P2 <<>> @ns1-05.azure-dns.com contoso.com
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
    contoso.com.         300     IN      SOA     edge1.azuredns-cloud.net.
    msnhst.microsoft.com. 6 900 300 604800 300

    Query time: 93 msec
    SERVER: 208.76.47.5#53(208.76.47.5)
    WHEN: Tue Jul 21 16:04:51 Pacific Daylight Time 2015
    MSG SIZE  rcvd: 120

## Další kroky

Po vytvoření zóny DNS vytvořte [sady záznamů a záznamy](dns-getstarted-create-recordset-cli.md), abyste mohli začít překládat názvy své internetové domény.



<!--HONumber=Oct16_HO1-->


