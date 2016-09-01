<properties
   pageTitle="Delegování domény do Azure DNS | Microsoft Azure"
   description="Zjistěte, jak změnit delegování domény a pomocí názvových serverů Azure DNS umožněte hosting domén."
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
   ms.date="06/30/2016"
   ms.author="cherylmc"/>


# Delegování domény do Azure DNS

Azure DNS vám umožňuje hostovat zónu DNS a spravovat záznamy DNS pro doménu v Azure. Mají-li se dotazy DNS pro doménu dostat k Azure DNS, musí doména být delegovaná z nadřazené domény do Azure DNS. Pamatujte, že Azure DNS není doménový registrátor. Tento článek vysvětluje princip fungování delegování domén a ukazuje, jak lze domény delegovat do Azure DNS.




## Jak funguje delegování DNS

### Domény a zóny

Domain Name System je hierarchie domén. Hierarchie začíná od kořenové domény, jejíž název je jednoduše „**.**“.  Následují domény nejvyšší úrovně, jako jsou „com“, „net“, „org“, „uk“ nebo „jp“.  Následují domény druhé úrovně, jako jsou „org.uk“ nebo „co.jp“.  A tak dále. Domény v hierarchii DNS jsou hostované pomocí oddělených zón DNS. Tyto zóny jsou globálně distribuované a hostované názvovými servery DNS po celém světě.

**Zóna DNS**

Doména je jedinečný název v Domain Name Systemu, například „contoso.com“. K hostování záznamů DNS pro konkrétní doménu se používá zóna DNS. Třeba doména „contoso.com“ může obsahovat několik záznamů DNS, třeba „mail.contoso.com“ (pro poštovní server) a „www.contoso.com“ (pro webový server).

**Doménový registrátor**

Doménový registrátor je společnost, která poskytuje názvy internetových domén. Ověří, zda je doména, kterou chcete použít, volná a umožní vám ji zakoupit. Jakmile je název domény registrovaný, stanete se jejím právoplatným vlastníkem. Pokud již máte internetovou doménu, použijete pro delegování do Azure DNS současného doménového registrátora.

>[AZURE.NOTE] Další informace o tom, kdo vlastní určitý název domény, nebo o tom, jak doménu zakoupit, naleznete v tématu [Správa internetových domén v Azure AD](https://msdn.microsoft.com/library/azure/hh969248.aspx).

### Překládání a delegování

Existují dva typy serverů DNS:

- _Autoritativní_ server DNS hostí zóny DNS. Odpovídá pouze na dotazy DNS pro záznamy v těchto zónách.
- _Rekurzivní_ server DNS nehostuje zóny DNS. Odpovídá na všechny dotazy DNS voláním autoritativních serverů DNS, které shromáždí potřebná data.

Klient DNS v počítačích nebo na mobilních zařízeních obvykle pro všechny dotazy DNS klientské aplikace volá rekurzivní server DNS.

Když rekurzivní server DNS obdrží dotaz na záznam DNS, třeba „www.contoso.com“, nejprve musí najít názvový server, který hostuje zónu pro doménu „contoso.com“. To provede tak, že začne u kořenových názvových serverů a vyhledá názvové servery, které hostují zónu „com“. Následně se dotazuje názvových serverů „com“ a tak najde názvové servery, které hostují zónu „contoso.com“.  Nakonec se těchto názvových serverů může dotázat na „www.contoso.com“.  

Tento postup se nazývá překlad názvu DNS. Přesněji řečeno, překlad DNS zahrnuje ještě další kroky, jako třeba sledování záznamů CNAME, ale pro pochopení, jak funguje delegování DNS, to není důležité.

Jak nadřazená zóna „ukáže“ na názvové servery pro podřízenou zónu? Používá k tomu speciální typ záznamu DNS, který se nazývá záznam NS (NS zastupuje „názvový server“). Například kořenová zóna obsahuje záznam NS pro „com“ a ukazuje názvové servery pro zónu „com“. Zóna „com“ pak obsahuje záznamy NS pro „contoso.com“, které ukazují názvové servery pro zónu „contoso.com“. Nastavení záznamů NS v nadřazené zóně pro podřízenou zónu se nazývá delegování domény.


![Názvový server DNS](./media/dns-domain-delegation/image1.png)

Každé delegování má ve skutečnosti dvě kopie záznamů NS – jednu v nadřazené zóně, která ukazuje na podřízenou zónu, a druhou v samotné podřízené zóně. Zóna „contoso.com“ obsahuje záznamy NS pro „contoso.com“ (vedle záznamů NS v „com“). Ty se nazývají záznamy autoritativních NS a nacházejí se na vrcholu podřízené zóny.


## Delegování domény do Azure DNS

Jakmile v Azure DNS vytvoříte vlastní zónu DNS, je potřeba nastavit záznamy NS v nadřazené zóně a tak udělat z Azure DNS autoritativní zdroj překládání názvů pro vaši zónu. U domén zakoupených od doménového registrátora nabídne registrátor možnost nastavit tyto záznamy NS.

>[AZURE.NOTE] Pro vytvoření zóny DNS s názvem domény v Azure DNS nemusíte tuto doménu vlastnit. Chcete-li však u registrátora nastavit delegování domény do Azure DNS, musíte tuto doménu vlastnit.

Předpokládejme například, že zakoupíte doménu „contoso.com“ a v Azure DNS vytvoříte zónu s názvem „contoso.com“. Jako vlastníkovi domény vám registrátor nabídne možnost konfigurovat pro vaši doménu adresy názvových serverů (tj. záznamů NS). Doménový registrátor uloží tyto záznamy NS v nadřazené doméně, v tomto případě „.com“. Klienti po celém světě pak budou při pokusu o překlad záznamů DNS v „contoso.com“ přesměrováni na vaši doménu v zóně Azure DNS.

### Získání názvů názvových serverů

Předtím, než budete moci svoji zónu DNS delegovat do Azure DNS, musíte znát názvy názvových serverů pro vaši zónu. Azure DNS přiděluje názvové servery z fondu vždy, když je vytvořena zóna.

Nejjednodušší způsob, jak zobrazit názvové servery přiřazené pro vaší zónu, je prostřednictvím portálu Azure.  V tomto případě byly pro zónu „contoso.net“ přiřazené názvové servery „ns1-01.azure-dns.com“, „ns2-01.azure-dns.net“, „ns3-01.azure-dns.org“ a „ns4-01.azure-dns.info“:

 ![Názvový server DNS](./media/dns-domain-delegation/viewzonens500.png)

Azure DNS automaticky vytvoří ve vaší zóně záznamy autoritativních NS, které obsahují přiřazené názvové servery.  Chcete-li zobrazit názvy názvových serverů prostřednictvím Azure PowerShellu nebo rozhraní příkazového řádku Azure, stačí jednoduše načíst tyto záznamy.

Pomocí Azure PowerShellu lze záznamy autoritativních NS načíst následujícím způsobem. Všimněte si, že název záznamu „@“ se používá k odkazování na záznamy na vrcholu zóny. 

    PS> $zone = Get-AzureRmDnsZone –Name contoso.net –ResourceGroupName MyResourceGroup
    PS> Get-AzureRmDnsRecordSet –Name “@” –RecordType NS –Zone $zone

    Name              : @
    ZoneName          : contoso.net
    ResourceGroupName : MyResourceGroup
    Ttl               : 3600
    Etag              : 5fe92e48-cc76-4912-a78c-7652d362ca18
    RecordType        : NS
    Records           : {ns1-01.azure-dns.com, ns2-01.azure-dns.net, ns3-01.azure-dns.org,
                        ns4-01.azure-dns.info}
    Tags              : {}

Pro načtení záznamů autoritativních NS můžete použít také víceplatformové rozhraní příkazového řádku Azure a tak zjistíte, jaké názvové servery jsou přiřazené pro vaši zónu.

    C:\> azure network dns record-set show MyResourceGroup contoso.net @ NS
    info:    Executing command network dns record-set show
        + Looking up the DNS Record Set "@" of type "NS"
    data:    Id                              : /subscriptions/.../resourceGroups/MyResourceGroup/providers/Microsoft.Network/dnszones/contoso.net/NS/@
    data:    Name                            : @
    data:    Type                            : Microsoft.Network/dnszones/NS
    data:    Location                        : global
    data:    TTL                             : 172800
    data:    NS records
    data:        Name server domain name     : ns1-01.azure-dns.com.
    data:        Name server domain name     : ns2-01.azure-dns.net.
    data:        Name server domain name     : ns3-01.azure-dns.org.
    data:        Name server domain name     : ns4-01.azure-dns.info.
    data:
    info:    network dns record-set show command OK

### Nastavení delegování

Každý registrátor má vlastní nástroje pro správu DNS, které umožňují měnit záznamy názvových serverů pro doménu. Na stránce správy DNS vašeho registrátora upravte záznamy NS a nahraďte je záznamy NS, které vytvořil Azure DNS.

Při delegování domény do Azure DNS musíte použít názvy názvových serverů, které poskytuje Azure DNS.  Vždycky byste měli použít všechny čtyři názvy názvových serverů bez ohledu na název domény.  Delegování domény nevyžaduje, aby název názvového serveru používal jako vaši doménu stejnou doménu nejvyšší úrovně.

Pro ukazování na IP adresy názvových serverů Azure DNS byste neměli používat „spojovací záznamy“, protože se tyto IP adresy mohou v budoucnu měnit. Delegování pomocí názvů názvových serverů ve vaší vlastní zóně, někdy označovaných jako „jednoduché názvové servery“, v současné době není v Azure DNS podporované.

### Ověření, že překlad názvů funguje

Po dokončení delegování můžete ověřit, že překlad názvů funguje, pomocí nástroje, jako je například „nslookup“, který se dotáže na záznam SOA pro vaši zónu (ten je také automaticky vytvořený při vytváření zóny).

Všimněte si, že nemusíte určit názvové servery Azure DNS, protože normální proces překladu DNS nalezne názvové servery automaticky, pokud bylo delegování správně nastavené.

    nslookup –type=SOA contoso.com

    Server: ns1-04.azure-dns.com
    Address: 208.76.47.4

    contoso.com
    primary name server = ns1-04.azure-dns.com
    responsible mail addr = msnhst.microsoft.com
    serial = 1
    refresh = 900 (15 mins)
    retry = 300 (5 mins)
    expire = 604800 (7 days)
    default TTL = 300 (5 mins)

## Delegování subdomén v Azure DNS

Chcete-li nastavit samostatnou podřízenou zónu, můžete subdoménu delegovat v Azure DNS. Předpokládejme například, že jste již v Azure nastavili a delegovali „contoso.com“, a chtěli byste nastavit samostatnou podřízenou zónu „partners.contoso.com“. 

Nastavení subdomény probíhá podobně jako normální delegování. Jediným rozdílem je, že v kroku 3 musí být záznamy NS vytvořené v nadřazené zóně „contoso.com“ v Azure DNS místo toho, aby je nastavoval registrátor.


1. V Azure DNS vytvořte podřízenou zónu „partners.contoso.com“.
2. Vyhledejte záznamy autoritativních NS v podřízené zóně a získejte tak názvové servery hostující podřízenou zónu v Azure DNS.
3. Delegujte podřízenou zónu pomocí konfigurace záznamů NS v nadřazené zóně tak, aby ukazovaly na podřízenou zónu.


### Delegování subdomény

Následující příklad PowerShell ukazuje, jak to funguje. Stejný postup lze provést přes portál Azure nebo prostřednictvím víceplatformového rozhraní příkazového řádku Azure.

#### Krok 1. Vytvoření nadřazených a podřízených zón

Nejdříve Vytvoříme nadřazené a podřízené zóny. Ty mohou být ve stejné skupině prostředků, nebo v různých skupinách prostředků.

    $parent = New-AzureRmDnsZone -Name contoso.com -ResourceGroupName RG1
    $child = New-AzureRmDnsZone -Name partners.contoso.com -ResourceGroupName RG1

#### Krok 2. Načtení záznamů NS

Dále načteme z podřízené zóny záznamy autoritativních NS, viz následující příklad.  To obsahuje názvové servery přiřazené pro podřízenou zónu. 

    $child_ns_recordset = Get-AzureRmDnsRecordSet -Zone $child -Name "@" -RecordType NS

#### Krok 3. Delegování podřízené zóny

Delegování dokončíte vytvořením odpovídající sady záznamů NS v nadřazené zóně. Všimněte si, že název sady záznamů v nadřazené zóně odpovídá názvu podřízené zóny, v tomto případě „partners“.

    $parent_ns_recordset = New-AzureRmDnsRecordSet -Zone $parent -Name "partners" -RecordType NS -Ttl 3600
    $parent_ns_recordset.Records = $child_ns_recordset.Records
    Set-AzureRmDnsRecordSet -RecordSet $parent_ns_recordset

### Ověření, že překlad názvů funguje

Vyhledáním záznamu SOA podřízené zóny můžete ověřit, že je všechno správně nastavené.

    nslookup –type=SOA partners.contoso.com

    Server: ns1-08.azure-dns.com
    Address: 208.76.47.8

    partners.contoso.com
        primary name server = ns1-08.azure-dns.com
        responsible mail addr = msnhst.microsoft.com
        serial = 1
        refresh = 900 (15 mins)
        retry = 300 (5 mins)
        expire = 604800 (7 days)
        default TTL = 300 (5 mins)

## Další kroky

[Správa zón DNS](dns-operations-dnszones.md)

[Správa záznamů DNS](dns-operations-recordsets.md)




<!--HONumber=Aug16_HO4-->


