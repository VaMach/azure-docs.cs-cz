<properties 
   pageTitle="Co je skupina zabezpečení sítě (NSG)"
   description="Seznamte se s distribuovanou bránou firewall v Azure, která používá skupiny zabezpečení sítě (NSG), a s používáním skupin NSG k izolování a řízení toku provozu ve virtuálních sítích."
   services="virtual-network"
   documentationCenter="na"
   authors="telmosampaio"
   manager="carmonm"
   editor="tysonn" />
<tags 
   ms.service="virtual-network"
   ms.devlang="na"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="02/11/2016"
   ms.author="telmos" />

# Co je skupina zabezpečení sítě (NSG)?

Skupina zabezpečení sítě (NSG) obsahuje seznam pravidel seznamu řízení přístupu (ACL), která instancím virtuálních počítačů ve službě Virtual Network povolují nebo odpírají síťový provoz. Skupiny NSG můžou být přidružené buď k podsítím, nebo k jednotlivým instancím virtuálních počítačů v této podsíti. Pokud je skupina NSG přidružená k podsíti, pravidla seznamu ACL platí pro všechny instance virtuálních počítačů v této podsíti. Provoz směřující do konkrétního virtuálního počítače se navíc dá dál omezit tím, že se přímo k tomuto virtuálnímu počítači přidruží skupina NSG.

## Prostředek NSG

Skupiny NSG obsahují následující vlastnosti.

|Vlastnost|Popis|Omezení|Požadavky|
|---|---|---|---|
|Název|Název skupiny NSG|Musí být jedinečný v rámci oblasti.<br/>Může obsahovat písmena, číslice, podtržítka, tečky a pomlčky.<br/>Musí začínat písmenem nebo číslicí.<br/>Musí končit písmenem, číslicí nebo podtržítkem.<br/>Může obsahovat maximálně 80 znaků.|Budete pravděpodobně vytvářet víc skupin NGS, a proto je vhodné používat takové názvy, které umožní snadno rozpoznat funkci jednotlivých skupin.|
|Oblast|Oblast Azure, kde je skupina NSG hostovaná.|Skupiny NSG se dají použít jenom na prostředky v oblasti svého vytvoření.|Informace o tom, kolik skupin NSG můžete mít v jedné oblasti, najdete v části popisující [omezení](#Limits).|
|Skupina prostředků|Skupina prostředků, do které skupina NSG patří.|Ačkoli skupina NSG patří do jedné ze skupin prostředků, dá se přidružit k prostředkům v libovolné skupině prostředků, pokud příslušný prostředek patří do stejné oblasti Azure jako skupina NSG.|Skupiny prostředků slouží ke společné správě více prostředků jako jednotky nasazení.<br/>Může být vhodné seskupit NSG s prostředky, ke kterým je přidružená.|
|Pravidla|Pravidla, která definují, jaký provoz je povolený nebo odepřený.||Viz [pravidla NSG](#Nsg-rules) níže.| 

>[AZURE.NOTE] Seznamy ACL založené na koncových bodech a skupiny zabezpečení sítě nejsou podporované ve stejné instanci virtuálního počítače. Pokud chcete použít skupinu NSG a už máte seznam ACL pro koncové body, nejdřív tento seznam odeberte. Informace o tom, jak to provést, najdete v tématu [Správa seznamů řízení přístupu (ACL) pro koncové body pomocí prostředí PowerShell](virtual-networks-acl-powershell.md).

### Pravidla NSG

Pravidla NSG obsahují následující vlastnosti.

|Vlastnost|Popis|Omezení|Požadavky|
|---|---|---|---|
|**Název**|Název pravidla|Musí být jedinečný v rámci oblasti.<br/>Může obsahovat písmena, číslice, podtržítka, tečky a pomlčky.<br/>Musí začínat písmenem nebo číslicí.<br/>Musí končit písmenem, číslicí nebo podtržítkem.<br/>Může obsahovat maximálně 80 znaků.|Ve skupině NGS budete mít pravděpodobně několik pravidel, a proto je vhodné používat takové názvy, které umožní rozpoznat funkci pravidla.|
|**Protocol (Protokol)**|Protokol, který je nutné u pravidla splnit|TCP, UDP nebo \*|Použití \* jako protokolu zahrnuje protokol ICMP (pouze provoz typu East-West) a protokoly UDP a TCP a může snížit počet pravidel, která budete potřebovat.<br/>Použití \* ale současně může být příliš široké, a proto se vždy ujistěte, že je tento přístup skutečně nutný.|
|**Rozsah zdrojových portů**|Rozsah zdrojových portů, který je nutné u pravidla splnit|Jedno číslo portu od 1 do 65535, rozsah portů (tj. 1–65635) nebo \* (pro všechny porty)|Zdrojové porty můžou být dočasné. Pokud váš klientský program nepoužívá konkrétní port, ve většině případů prosím použijte „*“.<br/>Snažte se co nejvíc používat rozsahy portů, aby nebylo potřeba více pravidel.<br/>Není možné seskupit několik portů nebo rozsahů portů oddělených čárkami.
|**Rozsah cílových portů**|Rozsah cílových portů, který je nutné u pravidla splnit|Jedno číslo portu od 1 do 65535, rozsah portů (tj. 1–65535) nebo \* (pro všechny porty)|Snažte se co nejvíc používat rozsahy portů, aby nebylo potřeba více pravidel.<br/>Není možné seskupit několik portů nebo rozsahů portů oddělených čárkami.
|**Předpona zdrojové adresy**|Předpona zdrojové adresy nebo značka, které je nutné u pravidla splnit|Jedna IP adresa (tj. 10.10.10.10), podsíť IP (tj. 192.168.1.0/24), [výchozí značka](#Default-Tags) nebo * (pro všechny adresy)|Zvažte použití rozsahů, výchozích značek a *, abyste snížili počet pravidel.|
|**Předpona cílové adresy**|Předpona cílové adresy nebo značka, které je nutné u pravidla splnit|Jedna IP adresa (tj. 10.10.10.10), podsíť IP (tj. 192.168.1.0/24), [výchozí značka](#Default-Tags) nebo * (pro všechny adresy)|Zvažte použití rozsahů, výchozích značek a *, abyste snížili počet pravidel.|
|**Směr**|Směr provozu, který je nutné u pravidla splnit|Příchozí nebo odchozí|Pravidla pro příchozí a odchozí provoz se zpracovávají odděleně podle směru.|
|**Priorita**|Pravidla se kontrolují v pořadí podle priority, a jakmile se některé pravidlo použije, žádná další se již nekontrolují.|Číslo v rozsahu od 100 do 4096|Doporučujeme u jednotlivých pravidel zadat priority v krocích po 100, aby byl mezi stávajícími pravidly prostor pro další.|
|**Access**|Typ přístupu, který se použije v případě splnění pravidla|Povolit nebo odepřít|Pamatujte, že pokud se pro paket nenajde pravidlo povolení, bude se paket ignorovat.|

Skupiny NSG obsahují dvě sady pravidel: příchozí a odchozí. Priorita pravidla musí být v rámci jednotlivých sad jedinečná. 

![Zpracování pravidel NSG](./media/virtual-network-nsg-overview/figure3.png) 

Výše uvedené schéma ukazuje, jak se zpracovávají pravidla NSG.

### Výchozí značky

Výchozí značky jsou identifikátory poskytnuté systémem, které slouží k adresování určité kategorie IP adres. Výchozí značky můžete použít u libovolného pravidla ve vlastnostech **předpona zdrojové adresy** a **předpona cílové adresy**. Existují tři výchozí značky, které můžete použít.

- **VIRTUAL_NETWORK:** Tato výchozí značka označuje veškerý adresní prostor vaší sítě. Zahrnuje adresní prostor virtuální sítě (rozsahy CIDR definované v Azure) a všechny připojené místní adresní prostory a připojené virtuální sítě Azure (místní sítě).

- **AZURE_LOADBALANCER:** Tato výchozí značka označuje nástroj pro vyrovnávání zatížení infrastruktury Azure. To se přeloží do IP datacentra Azure, kde mají původ testy stavu Azure.

- **INTERNET:** Tato výchozí značka označuje adresní prostor IP adres, který se nachází mimo virtuální síť a je dostupný prostřednictvím veřejného internetu. Tento rozsah taky zahrnuje [veřejný prostor IP adres vlastněný Azure](https://www.microsoft.com/download/details.aspx?id=41653).

### Výchozí pravidla

Všechny skupiny NSG obsahují sadu výchozích pravidel. Výchozí pravidla se nedají odstranit, ale protože je jim přiřazená nejnižší priorita, dají se přepsat pravidly, která vytvoříte. 

Jak vidíte ve výchozích pravidlech níže, provoz směřující z/do virtuální sítě je povolený v příchozím i odchozím směru. Připojení k internetu je povolené pro odchozí směr, ale pro příchozí směr je ve výchozím nastavení blokované. Existuje výchozí pravidlo, které nástroji pro vyrovnávání zatížení Azure povoluje testovat stav virtuálních počítačů a instancí rolí. Pokud nepoužíváte sadu s vyrovnáváním zatížení, můžete toto pravidlo přepsat.

**Příchozí výchozí pravidla**

| Název                              | Priorita | Zdrojová IP adresa          | Zdrojový port | Cílová IP adresa  | Cílový port | Protocol (Protokol) | Access |
|-----------------------------------|----------|--------------------|-------------|-----------------|------------------|----------|--------|
| ALLOW VNET INBOUND                | 65000    | VIRTUAL_NETWORK    | *           | VIRTUAL_NETWORK | *                | *        | POVOLIT  |
| ALLOW AZURE LOAD BALANCER INBOUND | 65001    | AZURE_LOADBALANCER | *           | *               | *                | *        | POVOLIT  |
| DENY ALL INBOUND                  | 65500    | *                  | *           | *               | *                | *        | ODEPŘÍT   |

**Odchozí výchozí pravidla**

| Název                    | Priorita | Zdrojová IP adresa       | Zdrojový port | Cílová IP adresa  | Cílový port | Protocol (Protokol) | Access |
|-------------------------|----------|-----------------|-------------|-----------------|------------------|----------|--------|
| ALLOW VNET OUTBOUND     | 65000    | VIRTUAL_NETWORK | *           | VIRTUAL_NETWORK | *                | *        | POVOLIT  |
| ALLOW INTERNET OUTBOUND | 65001    | *               | *           | INTERNET        | *                | *        | POVOLIT  |
| DENY ALL OUTBOUND       | 65500    | *               | *           | *               | *                | *        | ODEPŘÍT   |

## Přidružení skupin NSG

Skupinu NSG můžete přidružit k virtuálním počítačům, síťovým kartám a podsítím, a to v závislosti na modelu nasazení, který používáte.

[AZURE.INCLUDE [learn-about-deployment-models-both-include.md](../../includes/learn-about-deployment-models-both-include.md)]
 
- **Přidružení skupiny NSG k virtuálnímu počítači (pouze nasazení Classic).** Pokud přidružíte skupinu NSG k virtuálnímu počítači, pravidla pro přístup k síti obsažená v této skupině se použijí na veškerý provoz směřující z/do virtuálního počítače. 

- **Přidružení skupiny NSG k síťové kartě (pouze nasazení Resource Manager).** Pokud přidružíte skupinu NSG k síťové kartě, pravidla pro přístup k síti obsažená v této skupině se použijí jenom na tuto síťovou kartu. To znamená, že pokud se skupina NSG použije na jednu síťovou kartu ve virtuálním počítači s více síťovými kartami, nebude mít vliv na provoz směřující do ostatních síťových karet. 

- **Přidružení skupiny NSG k podsíti (všechna nasazení).** Pokud přidružíte skupinu NSG k podsíti, pravidla pro přístup k síti obsažená v této skupině se použijí na všechny prostředky IaaS a PaaS v této podsíti. 

K virtuálnímu počítači (nebo síťové kartě, podle modelu nasazení) a podsíti, ke které je síťová karta nebo virtuální počítač vázán, můžete přidružit odlišné skupiny NSG. Pokud k tomu dojde, na provoz se použijí všechna pravidla pro přístup k síti, a to podle priority v jednotlivých skupinách NSG v následujícím pořadí:

- **Příchozí provoz**
    1. Skupina NSG použitá na podsíť. 
    
           If subnet NSG has a matching rule to deny traffic, packet will be dropped here.
    2. Skupina NSG použitá na síťovou kartu (Resource Manager) nebo virtuální počítač (Classic). 
       
           If VM\NIC NSG has a matching rule to deny traffic, packet will be dropped at VM\NIC, although subnet NSG has a matching rule to allow traffic.
- **Odchozí provoz**
    1. Skupina NSG použitá na síťovou kartu (Resource Manager) nebo virtuální počítač (Classic). 
      
           If VM\NIC NSG has a matching rule to deny traffic, packet will be dropped here.
    2. Skupina NSG použitá na podsíť.
       
           If subnet NSG has a matching rule to deny traffic, packet will be dropped here, although VM\NIC NSG has a matching rule to allow traffic.

![Seznamy ACL skupiny NSG](./media/virtual-network-nsg-overview/figure2.png)

>[AZURE.NOTE] Přestože k podsíti, virtuálnímu počítači nebo síťové kartě se dá přidružit jenom jedna skupina NSG, tutéž skupinu NSG můžete přidružit k libovolnému počtu prostředků.

## Implementace
Skupiny NSG můžete implementovat v modelech nasazení Resource Manager nebo Classic pomocí různých nástrojů uvedených níže.

|Nástroj pro nasazení|Classic|Resource Manager|
|---|---|---|
|Portál Classic|![Ne][red]|![Ne][red]|
|Portál Azure|![Ano][green]|<a href="https://azure.microsoft.com/documentation/articles/virtual-networks-create-nsg-arm-pportal">![Ano][green]</a>|
|PowerShell|<a href="https://azure.microsoft.com/documentation/articles/virtual-networks-create-nsg-classic-ps">![Ano][green]</a>|<a href="https://azure.microsoft.com/documentation/articles/virtual-networks-create-nsg-arm-ps">![Ano][green]</a>|
|Azure CLI|<a href="https://azure.microsoft.com/documentation/articles/virtual-networks-create-nsg-classic-cli">![Ano][green]</a>|<a href="https://azure.microsoft.com/documentation/articles/virtual-networks-create-nsg-arm-cli">![Ano][green]</a>|
|Šablona ARM|![Ne][red]|<a href="https://azure.microsoft.com/documentation/articles/virtual-networks-create-nsg-arm-template">![Ano][green]</a>|

|**Klíč**|![Ano][green] Podporuje se. Kliknutím zobrazíte článek.|![Ne][red] Nepodporuje se.|
|---|---|---|

## Plánování

Před implementací skupin NSG je nutné odpovědět na tyto otázky:   

1. Do/z jakých typů prostředků chcete filtrovat provoz (síťové karty ve stejném virtuálním počítači, virtuální počítače nebo jiné prostředky, například cloudové služby nebo prostředí aplikačních služeb připojené ke stejné podsíti, nebo mezi prostředky připojenými k různým podsítím)?

2. Jsou prostředky, z/do kterých chcete filtrovat provoz, připojené k podsítím v existujících virtuálních sítích, nebo budou připojené k novým virtuálním sítím nebo podsítím?
 
Další informace o plánování zabezpečení sítě v Azure najdete v tématu popisujícím [osvědčené postupy pro cloudové služby a zabezpečení sítě](../best-practices-network-security.md). 

## Na co dát pozor při navrhování

Pokud znáte odpovědi na otázky uvedené v části [Plánování](#Planning), projděte si následující informace předtím, než budete definovat skupiny NSG.

### Omezení

Při navrhování skupin NSG je nutné vzít v úvahu následující omezení.

|**Popis**|**Výchozí omezení**|**Důsledky**|
|---|---|---|
|Počet skupin NSG, které můžete přidružit k podsíti, virtuálnímu počítači nebo síťové kartě|1|To znamená, že nejde kombinovat skupiny NSG. Ujistěte se, jestli všechna pravidla potřebná pro příslušnou sadu prostředků jsou obsažená v jedné skupině NSG.|
|Počet skupin NSG na oblast na předplatné|100|Ve výchozím nastavení se vytvoří nová skupina NSG pro každý virtuální počítač, který na Portálu Azure vytvoříte. Pokud toto výchozí chování povolíte, rychle skupiny NSG vyčerpáte. Při navrhování mějte toto omezení na paměti, a pokud to bude nutné, rozdělte prostředky do více oblastí nebo předplatných. |
|Počet pravidel NSG na skupinu NSG|200|Používejte široký rozsah IP adres a portů, abyste toto omezení nepřekročili. |

>[AZURE.IMPORTANT] Před navržením řešení si nezapomeňte projít všechna [omezení týkající se síťových služeb v Azure](../azure-subscription-service-limits.md#networking-limits). Některá omezení je možné zvýšit otevřením lístku podpory.

### Navrhování virtuálních sítí a podsítí

Vzhledem k tomu, že skupiny NSG se dají použít na podsítě, můžete minimalizovat počet skupin NSG tím, že svoje prostředky seskupíte podle podsítí a použijete skupiny NSG na podsítě.  Pokud se rozhodnete použít skupiny NSG na podsítě, možná zjistíte, že vaše stávající virtuální sítě a podsítě byly definované bez zohlednění skupin NSG. Je možné, že bude nutné definovat nové virtuální sítě a podsítě, které podpoří váš návrh skupin NSG. Taky nasaďte nové prostředky do nových podsítí. Potom můžete definovat strategii migrace k přesunutí stávajících prostředků do nových podsítí. 

### Zvláštní pravidla

Je nutné vzít v úvahu zvláštní pravidla uvedené níže. Ujistěte se, že neblokujete provoz povolený těmito pravidly, jinak vaše infrastruktura nebude moct komunikovat se základními službami Azure.

- **Virtuální IP adresa uzlu hostitele:** Základní služby infrastruktury, například DHCP, DNS a sledování stavu, jsou poskytované prostřednictvím virtualizované IP adresy hostitele 168.63.129.16. Tato veřejná IP adresa patří společnosti Microsoft a bude to jediná virtualizovaná IP adresa používaná pro tento účel ve všech oblastech. Tato IP adresa se mapuje na fyzickou IP adresu počítače serveru (uzlu hostitele) hostujícího virtuální počítač. Uzel hostitele funguje jako přenos DHCP, rekurzivní překladač DNS a zdroj testů pro test stavu a zdroj testu pro test stavu nástroje pro vyrovnávání zatížení a test stavu počítače. Komunikace na tuto IP adresu by se neměla považovat za útok.

- **Licencování (Služba správy klíčů):** Bitové kopie systému Windows spuštěné ve virtuálních počítačích by měly být licencované. Za tímto účelem se odesílají žádosti o licenci na hostitelské servery Služby správy klíčů, které takové dotazy zpracovávají. To bude vždy probíhat na odchozím portu 1688.

### Provoz protokolu ICMP

Aktuální pravidla NSG povolují pouze protokoly *TCP* nebo *UDP*. Pro *ICMP* neexistuje žádná konkrétní značka. Provoz ICMP je nicméně ve výchozím nastavení povolený v rámci služby Virtual Network prostřednictvím pravidla Inbound VNet (výchozí pravidlo 65000 příchozí), které povoluje provoz z/do jakéhokoli portu a protokolu v rámci virtuální sítě.

### Podsítě

- Zvažte, jaký počet úrovní vaše úlohy vyžadují. Každá úroveň se dá izolovat pomocí podsítě, přičemž skupina NSG se použije na podsíť. 
- Pokud potřebujete implementovat podsíť pro bránu VPN, nebo okruh ExpressRoute, ujistěte se, že jste u této podsítě **NEPOUŽILI** skupinu NSG. Pokud ji použijete, připojení mezi virtuálními sítěmi nebo mezi více místy nebude fungovat.
- Pokud je nutné implementovat virtuální zařízení, nasaďte ho ve vlastní podsíti, aby trasy definované uživatelem (UDR) správně fungovaly. Můžete implementovat skupinu NSG na úrovni podsítě a filtrovat provoz do/z této podsítě. Další informace o [řízení toku provozu a použití virtuálních zařízení](virtual-networks-udr-overview.md).

### Nástroje pro vyrovnávání zatížení

- Vezměte v úvahu pravidla vyrovnávání zatížení a NAT pro jednotlivé nástroje pro vyrovnávání zatížení používané vašimi úlohami. Tato pravidla jsou vázaná na fond typu Back End, který obsahuje síťové karty (nasazení Resource Manager) nebo virtuální počítače / instance rolí (nasazení Classic). Zvažte, jestli není vhodné vytvořit skupinu NSG pro každý fond typu Back End a povolit pouze provoz mapovaný prostřednictvím pravidel implementovaných v nástrojích pro vyrovnávání zatížení. To zaručuje, že se bude filtrovat i provoz, který přichází do fondu typu Back End přímo, aniž by prošel nástrojem pro vyrovnávání zatížení.
- V nasazení Classic vytvoříte koncové body, které mapují porty nástroje pro vyrovnávání zatížení do portů virtuálních počítačů nebo instancí rolí. Můžete taky vytvořit vlastní jednotlivý veřejně přístupný nástroj pro vyrovnávání zatížení v nasazení Resource Manager. Pokud pomocí skupin NSG omezujete provoz směřující do virtuálních počítačů a instancí rolí, které jsou součástí fondu typu Back End v nástroji pro vyrovnávání zatížení, nezapomeňte, že cílovým portem příchozího provozu je skutečný port ve virtuálním počítači nebo instanci role, a nikoli port zpřístupněný nástrojem pro vyrovnávání zatížení. Nezapomeňte taky, že zdrojovým portem a zdrojovou adresou pro připojení k virtuálnímu počítači jsou port a adresa ve vzdáleném počítači v internetu, a nikoli port a adresa zpřístupněné nástrojem pro vyrovnávání zatížení.
- Podobně, jako tomu je u veřejně přístupných nástrojů pro vyrovnávání zatížení, když vytvoříte skupiny NSG k filtrování provozu procházejícího interním nástrojem pro vyrovnávání zatížení (ILB), je nutné pamatovat, že se jako zdrojový port a rozsah adres použijí port a rozsah adres z počítače, který volání vytvořil, a nikoli z nástroje pro vyrovnávání zatížení. A konečně, cílový port a rozsah adres se vztahují k počítači, který provoz přijímá, a nikoli k nástroji pro vyrovnávání zatížení.

### Ostatní

- Skupiny NSG a seznamy ACL založené na koncových bodech nejsou podporované ve stejné instanci virtuálního počítače. Pokud chcete použít skupinu NSG a už máte seznam ACL pro koncové body, nejdřív tento seznam odeberte. Informace o tom, jak to provést, najdete v tématu [Správa seznamů ACL pro koncové body](virtual-networks-acl-powershell.md).
- V modelu nasazení Resource Manager můžete použít skupinu NSG přidruženou k síťové kartě u virtuálních počítačů s více síťovými kartami k povolení správy (vzdáleného přístupu) síťovou kartou, a tím oddělit provoz.
- Podobně, jako tomu je při použití nástrojů pro vyrovnávání zatížení, když filtrujete provoz z jiných virtuálních sítí, je nutné použít zdrojový rozsah adres vzdáleného počítače, nikoli brány připojující tyto virtuální sítě.
- Mnoho služeb Azure nejde připojit k virtuálním sítím Azure, a proto provoz do/z těchto služeb není možné filtrovat pomocí skupin NSG.  Pokud potřebujete zjistit, jestli vámi používané služby jde připojit k virtuálním sítím, přečtěte si jejich dokumentaci.

## Ukázka nasazení

Pro ilustraci použití informací obsažených v tomto článku budeme definovat skupiny NSG k filtrování provozu sítě přenosů v případě dvouvrstvého řešení úloh s těmito požadavky:

1. Oddělení provozu mezi částí Front End (webové servery Windows) a částí Back End (databázové servery SQL).
2. Pravidla vyrovnávání zatížení předávají provoz směřující do nástroje pro vyrovnávání zatížení do všech webových serverů na portu 80.
3. Pravidla NAT předávají provoz přicházející do portu 50001 nástroje pro vyrovnávání zatížení do portu 3389 jenom jednoho virtuálního počítače v části Front End.
4. Z internetu není přístup k virtuálním počítačům v části Front End a Back End, s výjimkou požadavku číslo 1.
5. Z části Front End a Back End není přístup k internetu.
6. Přístup k portu 3389 k libovolnému webovému serveru v části Front End, pro provoz přicházející ze samotné podsítě Front End.
7. Přístup k portu 3389 ke všem virtuálním počítačům systému SQL Server v části Back End jenom z podsítě Front End.
8. Přístup k portu 1433 ke všem virtuálním počítačům systému SQL Server v části Back End jenom z podsítě Front End.
9. Oddělení provozu správy (port 3389) a provozu databáze (1433) na různých síťových kartách ve virtuálních počítačích v části Back End.

![Skupiny NSG](./media/virtual-network-nsg-overview/figure1.png)

Jak je vidět v diagramu výše, virtuální počítače *Web1* a *Web2* jsou připojené k podsíti *FrontEnd* a virtuální počítače *DB1* a *DB2* jsou připojené k podsíti *BackEnd*.  Obě podsítě jsou součástí virtuální sítě *TestVNet*. Všechny prostředky jsou přiřazené k oblasti Azure *West US*.

Výše uvedené požadavky 1–6 (s výjimkou požadavku 3) jsou omezeny na prostory podsítě. Abychom minimalizovali počet pravidel potřebných pro jednotlivé skupiny NSG a usnadnili přidávání dalších virtuálních počítačů do podsítí provádějících stejné typy úloh jako stávající virtuální počítače, můžeme implementovat následující skupiny NSG na úrovni podsítě.

### Skupina NSG pro podsíť FrontEnd

**Příchozí pravidla**

|Pravidlo|Access|Priorita|Zdrojový rozsah adres|Zdrojový port|Cílový rozsah adres|Cílový port|Protocol (Protokol)|
|---|---|---|---|---|---|---|---|
|povolit HTTP|Povolit|100|INTERNET|\*|\*|80|TCP|
|povolit RDP z FrontEndu|Povolit|200|192.168.1.0/24|\*|\*|3389|TCP|
|odepřít cokoli z internetu|Odepřít|300|INTERNET|\*|\*|\*|TCP|

**Odchozí pravidla**

|Pravidlo|Access|Priorita|Zdrojový rozsah adres|Zdrojový port|Cílový rozsah adres|Cílový port|Protocol (Protokol)|
|---|---|---|---|---|---|---|---|
|odepřít internet|Odepřít|100|\*|\*|INTERNET|\*|\*|

### NSG pro podsíť BackEnd

**Příchozí pravidla**

|Pravidlo|Access|Priorita|Zdrojový rozsah adres|Zdrojový port|Cílový rozsah adres|Cílový port|Protocol (Protokol)|
|---|---|---|---|---|---|---|---|
|odepřít internet|Odepřít|100|INTERNET|\*|\*|\*|\*|

**Odchozí pravidla**

|Pravidlo|Access|Priorita|Zdrojový rozsah adres|Zdrojový port|Cílový rozsah adres|Cílový port|Protocol (Protokol)|
|---|---|---|---|---|---|---|---|
|odepřít internet|Odepřít|100|\*|\*|INTERNET|\*|\*|

### NSG pro jeden virtuální počítač (síťovou kartu) ve FrontEndu pro RDP z internetu

**Příchozí pravidla**

|Pravidlo|Access|Priorita|Zdrojový rozsah adres|Zdrojový port|Cílový rozsah adres|Cílový port|Protocol (Protokol)|
|---|---|---|---|---|---|---|---|
|povolit RDP z internetu|Povolit|100|INTERNET|*|\*|3389|TCP|

>[AZURE.NOTE] Všimněte si, že zdrojový rozsah adres pro toto pravidlo je **Internet**, a nikoli virtuální IP adresa pro nástroj pro vyrovnávání zatížení; zdrojový port je **\***, a nikoli 500001. Nezaměňujte pravidla NAT / pravidla vyrovnávání zatížení a pravidla NSG. Pravidla NSG se vždy týkají původního zdroje a konečného cíle provozu, **NIKOLI** nástroje pro vyrovnávání zatížení mezi nimi. 

### Skupina NSG pro správu síťových karet v BackEndu

**Příchozí pravidla**

|Pravidlo|Access|Priorita|Zdrojový rozsah adres|Zdrojový port|Cílový rozsah adres|Cílový port|Protocol (Protokol)|
|---|---|---|---|---|---|---|---|
|Povolit RDP z Front Endu|Povolit|100|192.168.1.0/24|*|\*|3389|TCP|

### Skupina NSG pro databázový přístup síťových karet v Back Endu

**Příchozí pravidla**

|Pravidlo|Access|Priorita|Zdrojový rozsah adres|Zdrojový port|Cílový rozsah adres|Cílový port|Protocol (Protokol)|
|---|---|---|---|---|---|---|---|
|povolit SQL z Front Endu|Povolit|100|192.168.1.0/24|*|\*|1433|TCP|

Některé z výše uvedených skupin NSG je nutné přidružit k jednotlivým síťovým kartám, a proto musíte tento scénář nasadit jako nasazení Resource Manager. Všimněte si, jak jsou pravidla pro úroveň podsítí a síťových karet zkombinovaná podle toho, jak se mají použít. 

## Další kroky

- [Nasazení skupin NSG v modelu nasazení Classic](virtual-networks-create-nsg-classic-ps.md).
- [Nasazení skupin NSG v modelu Resource Manager](virtual-networks-create-nsg-arm-pportal.md).
- [Správa protokolů NSG](virtual-network-nsg-manage-log.md).

[zelená]: ./media/virtual-network-nsg-overview/green.png
[žlutá]: ./media/virtual-network-nsg-overview/yellow.png
[červená]: ./media/virtual-network-nsg-overview/red.png



<!--HONumber=Jun16_HO2-->


