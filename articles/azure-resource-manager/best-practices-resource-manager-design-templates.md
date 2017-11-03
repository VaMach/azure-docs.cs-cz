---
title: "Návrh šablony Azure pro komplexní řešení | Microsoft Docs"
description: "Ukazuje osvědčené postupy pro navrhování šablon Azure Resource Manageru pro komplexní scénáře"
services: azure-resource-manager
documentationcenter: 
author: tfitzmac
manager: timlt
editor: tysonn
ms.assetid: ce1141d6-ece7-4976-acea-1db1f775409e
ms.service: azure-resource-manager
ms.workload: multiple
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/19/2016
ms.author: tomfitz
ms.openlocfilehash: dcc31f7a8c85a8f7fbd554371a66fb1e348bca17
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/11/2017
---
# <a name="design-patterns-for-azure-resource-manager-templates-when-deploying-complex-solutions"></a>Vzory návrhu pro šablony Azure Resource Manager při nasazení komplexní řešení
Pomocí flexibilní přístup na základě šablon Azure Resource Manager, můžete nasadit komplexních topologiích rychle a konzistentně. Můžete přizpůsobit tato nasazení snadno jako základní nabídky momentální nebo aby bylo možné ošetřit varianty pro scénáře outlier nebo zákazníků.

Toto téma je součástí větší dokument White Paper. Číst úplnou dokumentu, stáhněte si [World – třída Azure Resource Manager šablony požadavky a osvědčené postupy](http://download.microsoft.com/download/8/E/1/8E1DBEFA-CECE-4DC9-A813-93520A5D7CFE/World Class ARM Templates - Considerations and Proven Practices.pdf).

Šablony kombinovat výhody z základní Azure Resource Manager s přizpůsobivost a čitelnost z JSON JavaScript Object Notation (). Pomocí šablony, můžete:

* Topologie a jejich úloh nasaďte konzistentně.
* Spravovat všechny prostředky v aplikaci společně použití skupin prostředků.
* Použití řízení přístupu na základě role (RBAC) k udělení přístupu příslušné uživatelům, skupinám a službám.
* Použijte označování přidružení ke zjednodušení úlohy, jako je fakturace souhrny.

Tento článek poskytuje podrobné informace o spotřebě scénáře, architekturu a vzory implementace identifikované při našich relace návrhu a implementace reálného šablony u zákazníků Azure poradní tým (AzureCAT). Daleko od academic, jsou tyto přístupy osvědčené postupy o tom informováni prostřednictvím vývoj šablon pro 12 technologií horní operačních systémů se systémem Linux, včetně: Apache Kafka, Apache Spark, Cloudera, Couchbase, Hortonworks HDP, Enterprise DataStax používá technologii Apache Cassandra, Elasticsearch, volaných, MongoDB, PostgreSQL, Redis a Nagios. 

Tento článek sdílí tyto osvědčené postupy při architektury šablony Azure Resource Manager třídy world.  

V našem práce se zákazníky jsme určili několik možnosti spotřeby šablony správce prostředků mezi podniky, s systémových Integrátorech (SI) a sdílené svazky clusteru. Následující části obsahují základní přehled běžné scénáře a vzory pro typy různých zákazníků.

## <a name="enterprises-and-system-integrators"></a>Podniky a integrátory systémů
Ve velkých organizacích, vidíme běžně dvě příjemci šablony Resource Manageru: vnitřní softwaru vývojové týmy a podnikové IT. Jsme zjistili, scénáře pro mapu SIs scénáře pro firmy, abyste použít stejné aspekty.

### <a name="internal-software-development-teams"></a>Interní softwaru vývojové týmy
Pokud váš tým sama vyvinula softwaru pro podporu vaší firmě, šablony poskytují snadný způsob, jak rychle nasadit technologie pro použití v řešeních pro specifické pro firmy. Můžete také použít šablony rychle vytvořit školení prostředí, které umožňují členové týmu pro získání dovednosti nezbytné.

Můžete použít šablony jako-je rozšířit nebo tvoří jejich přizpůsobí vašim potřebám. Pomocí označování šablony, můžete poskytnout fakturace souhrn různých zobrazení, například jako tým, projektu, jednotlivé a vzdělávání.

Podniky mají často softwaru vývojové týmy k vytvoření šablony pro konzistentní nasazení řešení. Šablona usnadňuje omezení tak, aby některé položky v rámci prostředí pevná a nelze přepsat. Banka může například vyžadovat šablonu, která má obsahovat RBAC tak programátorem nelze zkontrolovat, jestli řešení bankovní k odesílání dat do účtu osobní úložiště.

### <a name="corporate-it"></a>Podnikové IT
Podniková oddělení IT se obvykle používají šablony pro doručování cloudu možnosti kapacity a hostovaných v cloudu.

#### <a name="cloud-capacity"></a>Kapacita cloudu
Běžný způsob pro podnikové IT skupiny k zajištění kapacity cloudu pro týmy je s "tričko velikostí", které jsou standardní nabídky velikosti například malé, střední a velké. Nabídky tričko velikost můžete kombinovat různé typy prostředků a počty při současném poskytování úrovně standardizaci, který umožňuje použití šablon. Šablony poskytovat kapacity konzistentní způsobem, který vynucuje podnikové zásady a používá označení zajistit vrácení peněz na využívání organizace.

Potřebujete například poskytovat vývoj, testovací nebo produkční prostředí v rámci, které můžete nasadit software vývojové týmy jejich řešení. Prostředí obsahuje předdefinované síťové topologie a elementy, které nelze změnit vývojové týmy softwaru, jako například pravidla pro přístup k veřejného Internetu a paketu kontroly. Můžete mít rovněž organizace specifické role u těchto prostředí s odlišné přístupová práva pro prostředí.

#### <a name="cloud-hosted-capabilities"></a>Možnosti hostovaných v cloudu
Šablony můžete použít k podpoře funkce hostovaných v cloudu, včetně jednotlivých softwarových balíčků nebo složený nabídky, které jsou nabízeny interní odvětví podnikání. Příkladem složené nabídky může být analýza jako služby – analýzy, vizualizace a další technologie – doručit v konfiguraci optimalizované, propojených na předdefinované síťové topologie.

Aspekty zabezpečení a role vymezenému kapacitu cloudu, na kterém jste vytvořené nabídky se týká funkcí hostovaných v cloudu. Tyto možnosti jsou nabízena, jak jsou, nebo jako spravovanou službu. K povolení přístupu do prostředí pro účely správy jsou potřeba pro pozdější, omezeného přístupu role.

## <a name="cloud-service-vendors"></a>Dodavatelé cloudové služby
Až po rozhovoru s mnoha sdílené svazky clusteru, myslíme, že více přístupů, které můžete použít k nasazení služeb pro vaše zákazníky a přidružené požadavky.

### <a name="csv-hosted-offering"></a>CSV hostované nabídky
Pokud hostujete vaší nabídky v předplatného Azure, jsou společné hostování dva přístupy: nasazení odlišné nasazení pro každý zákazníka nebo nasazování jednotek škálování, které jsou základem sdílené infrastruktury používá pro všechny zákazníky.

* **Odlišné nasazení pro každého zákazníka.** Odlišné nasazení na zákazníka vyžadují pevné topologie různé známé konfigurace. Tato nasazení může mít jiný virtuální počítač (VM) velikosti, proměnlivým počtem uzlů a různé množství přidruženého úložiště. Označení nasazení se používá pro fakturaci souhrnné jednotlivých zákazníků. RBAC lze ho zapnout pro povolení přístupu k aspekty jejich cloudového prostředí zákazníků.
* **Jednotky škálování ve sdíleném víceklientské prostředí.** Jednotka škálování pro prostředí s více klientů může představovat šablonu. V takovém případě se používá stejnou infrastrukturu pro podporu všem zákazníkům. Nasazení představují skupinu prostředků, které doručují úroveň kapacity pro hostované nabídky, jako je počet uživatelů a počet transakcí. Tyto jednotky škálování jsou zvětšit nebo zmenšit, protože vyžaduje.

### <a name="csv-offering-injected-into-customer-subscription"></a>Nabídka CSV vloženy do předplatné zákazníka
Můžete k nasazení softwaru do odběrů vlastníkem koncovým zákazníkům. Šablony můžete použít k nasazení odlišné nasazení do Azure účtu zákazníka.

Tato nasazení používat funkci RBAC, abyste mohli aktualizovat a spravovat nasazení v rámci účtu zákazníka.

### <a name="azure-marketplace"></a>Azure Marketplace
Inzerovat a prodeje nabídkami přes marketplace, jako je například Azure Marketplace můžete vyvíjet šablony k poskytování odlišné typy nasazení, které běží v Azure účtu zákazníka. Tato odlišné nasazení mohou obvykle popsané jako velikost trička (malé, střední, velký), typ produktu nebo cílové skupiny (komunity, developer, enterprise) nebo typ funkce (základní, vysoké dostupnosti).  V některých případech se tyto typy umožňují určit určité atributy nasazení, například typ virtuálního počítače nebo počet disků.

## <a name="oss-projects"></a>Projekty operačních systémů
V rámci projekty s otevřeným zdrojem povolte šablony Resource Manageru komunita nasazení řešení rychle pomocí osvědčené postupy. Šablony můžete ukládat v úložišti GitHub, komunity můžete zkontrolovat, jestli je v čase. Uživatelé nasazení těchto šablon do svých vlastních předplatných Azure.

V následujících částech akcí, které je potřeba vzít v úvahu před navržením řešení.

## <a name="identifying-what-is-outside-and-inside-a-vm"></a>Identifikace, co je mimo a uvnitř virtuálního počítače
Při navrhování vaší šablony, je vhodné podívat se na požadavky z hlediska novinky mimo a ve virtuálních počítačích (VM):

* Mimo znamená virtuálních počítačů a dalším prostředkům vaše nasazení, například síťové topologie, označování, odkazuje na certifikátů nebo tajných klíčů a řízení přístupu na základě rolí. Všechny tyto prostředky jsou součástí šablony.
* Uvnitř znamená nainstalovaného softwaru a celkové požadovaný stav konfigurace. Jiné mechanismy, jako je například rozšíření virtuálního počítače nebo skripty, se používají v celé nebo částečně. Tyto mechanismy může identifikovat a provést šablonou, ale nejsou v ní.

Běžné činnosti, které by se "uvnitř pole" Příklady-  

* Instalace nebo odebrání rolí a funkcí serveru
* Instalace a konfigurace softwaru na úrovni uzlu nebo clusteru
* Nasazení webů na webovém serveru
* Nasazení databáze schémata
* Správa registru nebo jiné typy nastavení konfigurace
* Správa souborů a adresářů
* Spuštění, zastavení a správu procesů a služeb
* Správa místních skupin a uživatelských účtů
* Instalaci a správě balíčků (.msi, .exe, yum atd.)
* Spravovat proměnné prostředí
* Spouštění skriptů nativní (prostředí Windows PowerShell, bash, atd.)

### <a name="desired-state-configuration-dsc"></a>Konfigurace požadovaného stavu (DSC)
Přemýšlíte o vnitřní stav virtuálních počítačů nad rámec nasazení, chcete zajistěte, aby toto nasazení nebude "soubor" z konfigurace, který jste definovali a zaškrtnutí do správy zdrojového kódu. Tento přístup zajišťuje vaše vývojáři nebo provozní personál nemusíte měnit ad-hoc prostředí, které nejsou vetted, testovat nebo zaznamenávají do správy zdrojového kódu. Tento ovládací prvek je důležité, protože nejsou ručních změn ve správě zdrojového kódu. Nejsou také součástí standardního nasazení a ovlivní budoucí automatizované nasazení softwaru.

Nad rámec zaměstnancům vnitřní konfigurace požadovaného stavu taky je důležité z hlediska zabezpečení. Hackery jsou pravidelně pokusu o ohrožení a zneužití systémy softwaru. Po úspěšné, je běžné instalace souborů a v opačném případě změňte stavu ohrožení zabezpečení systému. Konfigurace požadovaného stavu můžete identifikovat rozdílů mezi požadované a aktuální stav a obnovení známé konfigurace.

Existují rozšíření prostředků pro nejoblíbenější mechanismy pro DSC - PowerShell DSC, Chef a Puppet. Každý z těchto rozšíření můžete nasadit počáteční stav virtuálního počítače a také použít k Ujistěte se, že se zachová požadovaný stav.

## <a name="common-template-scopes"></a>Běžné obory šablony
V našich zkušeností jsme viděli tři obory šablony klíče řešení vznikat. Tyto tři obory – kapacitu, funkce a řešení pro kompletní – jsou popsané v následujících částech.

### <a name="capacity-scope"></a>Kapacity oboru
Obor kapacity přináší sadu prostředků v topologii standardní, který je předem nakonfigurovaný k dosažení souladu s předpisy a zásadami. Nejčastější nasazuje ve scénáři s Enterprise IT nebo ve standardním vývojovém prostředí.

### <a name="capability-scope"></a>Obor funkce
Obor funkce se zaměřuje na nasazení a konfiguraci topologie pro danou technologii. Běžné scénáře, včetně technologií, jako je například SQL Server, Cassandra, Hadoop.

### <a name="end-to-end-solution-scope"></a>Komplexní řešení rozsahu
V oboru začátku do konce řešení je cílem nad rámec jednoho schopnosti a místo toho se zaměřuje na řešení s koncová skládá z více možností.  

Obor s rozsahem řešení šablony projevuje jako sadu jeden nebo více obor funkce šablon s prostředky pro konkrétní řešení, logiku a požadovaný stav. Příklad šablony obor řešení je šablonu koncová datového kanálu řešení. Šablona může kombinovat specifické řešení topologie a stavu s více šablon funkce obor řešení například Kafka, Storm a Hadoop.

## <a name="choosing-free-form-vs-known-configurations"></a>Výběr vlastní oproti známé konfigurace
Zpočátku byste očekávali šablonu měl dát příjemci co nejvíce flexibilitu, ale volba, jestli se má použít vlastní konfigurace oproti známé konfigurace ovlivnit několik rozhodnutí. Tato část popisuje požadavky zákazníků a technické aspekty, které ve tvaru přístup sdílené v tomto dokumentu.

### <a name="free-form-configurations"></a>Vlastní konfigurace
Na ploše zvukových ideální vlastní konfigurace. Se vám umožní vybrat typ virtuálního počítače a zadat libovolný počet uzlů a připojené disky pro tyto uzly – a to provést jako parametry do šablony. Tento přístup je však není ideální pro některé scénáře.

V [velikosti virtuálních počítačů](../virtual-machines/windows/sizes.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json), jsou identifikovány různé typy virtuálních počítačů a dostupných velikostí a každý počtu trvale disky (2, 4, 8, 16 nebo 32), který lze připojit. Každý připojený disk poskytuje 500 IOPS a násobků, které z těchto disků může být ve fondu pro násobitel tohoto počtu IOPS. 16 disků může být například ve fondu, zajistit 8000 IOPS. Sdružování se provádí s konfigurací v operačním systému, pomocí prostorů úložiště Microsoft Windows nebo redundantní pole málo nákladných disků (RAID) v systému Linux.

Vlastní konfigurace umožňuje výběr několik instancí virtuálních počítačů, virtuální počítač různé typy a velikosti pro ty instance, různých disků pro typ virtuálního počítače a jeden nebo více skripty ke konfiguraci virtuálních počítačů obsah.

Je běžné, že nasazení může mít víc typů uzlů, jako je hlavní server a datové uzly, tak tuto flexibilitu potřebují je často poskytnuté pro každý typ uzlu.

Když začnete nasazení clusterů žádné násobek, začnete pracovat s tyto složité scénáře. Pokud byly nasazení clusteru Hadoop, například s 8 řídicí uzly a uzly 200 dat a ve fondu 4 připojených disků na každý hlavní uzel a ve fondu 16 připojených disků na jeden uzel dat by máte 208 virtuální počítače a 3,232 disky, které slouží ke správě.

Účet úložiště bude omezení výše uvedené požadavky jeho identifikovaných 20 000 transakcí za sekundu omezit, tak, aby měli podívejte se na vytváření oddílů účtu úložiště a výpočty použijte k určení odpovídající počet účtů úložiště pro umístění této topologii. Zadána velkého množství kombinace nepodporuje vlastní přístup dynamické výpočty vyžadovaných pro určení příslušné oddíly. Jazyk šablony Azure Resource Manager v současné době neposkytuje matematické funkce, takže je nutné provést tyto výpočty v kódu, generování šablonu jedinečné, pevně s příslušné podrobnosti.

V podnikovém IT a ve scénářích někdo musí udržovat šablony a podporovat nasazené topologie pro jeden nebo více organizace. Tato další režii – různých konfigurací a šablony pro každého zákazníka – je daleko od žádoucí.

Tyto šablony můžete použít k nasazení prostředí v rámci předplatného Azure vašich zákazníků, ale podnikové IT týmy i sdílené svazky clusteru obvykle nasazení je do svých vlastních předplatných, pomocí funkce vrácení peněz k vyúčtování zákazníků. V těchto scénářích cílem je nasazení kapacity pro několik zákazníků v rámci fondu předplatných a udržovat nasazení hustě importují do odběrů minimalizovat živelný předplatné růst – to znamená, více odběrů ke správě. S velikostí skutečně dynamické nasazení dosažení tento typ hustotu vyžaduje pečlivé plánování a další vývoj pro generování uživatelského rozhraní pracovní jménem organizace.

Kromě toho nelze vytvářet odběry prostřednictvím volání rozhraní API ale musíte to provést ručně přes portál. Jak se zvyšuje počet odběrů, všechny výsledné živelný růst předplatné vyžaduje lidského zásahu – nelze je možné automatizovat. S mnoho variabilita velikosti nasazení by musel předem poskytnout několik odběrů ručně ověřte, že odběry jsou k dispozici.

S ohledem na tyto faktory je přitažlivými méně než v první blush skutečně vlastní konfigurace.

### <a name="known-configurations--the-t-shirt-sizing-approach"></a>Známé konfigurace – přístup nastavení velikosti tričko
Spíše než nabízí šablonu, která poskytuje celkový flexibilitu a obrovském množství variant, v našich zkušeností běžný vzor je poskytnout možnost vybrat známé konfigurace – ve skutečnosti standardní tričko velikosti například izolovaného prostoru, malé, střední a velké. Další příklady tričko velikosti jsou nabídky produktů, jako je například community edition nebo enterprise edition.  V jiných případech může být určeného pro konkrétní úlohy konfigurace technologii – například mapy snížit nebo žádné sql.

Mnoho podnikových IT organizace, operačních systémů dodavatelů a SIs zpřístupnit jejich nabídky dnes tímto způsobem ve virtualizovaných prostředích (podniky) na místě, nebo jako nabídek softwaru jako služby (SaaS), (sdílené svazky clusteru a OSVs).

Tento přístup poskytuje dobrý, známé konfigurace různou velikost předem konfigurovaných pro zákazníky. Bez známé konfigurace musí koncovým zákazníkům určit cluster pro změnu velikosti na své vlastní, zohlednit v omezení prostředků platformy a proveďte matematické k identifikaci výsledné oddíly účty úložiště a dalším prostředkům (z důvodu velikost clusteru a prostředků omezení). Známé konfigurace umožňují zákazníkům snadno vybrat správnou velikost tričko – to znamená, dané nasazení. Kromě vytváření lepší podmínky pro zákazníka, je snazší malý počet známé konfigurace podporují a můžete poskytovat vyšší úroveň hustotou.

O přístupu známé konfigurace zaměřené na velikosti tričko také může mít různý počet uzlů v rámci velikost. Malá velikost trička může být například mezi uzly 3 až 10.  Velikost trička by být navržena k uložení až 10 uzly a zadejte příjemce možnost volného formátu možnosti až po maximální velikost identifikovat.  

Velikost trička, v závislosti na typu úloh, může být více volného formátu ve své podstatě z hlediska počtu uzlů, které mohou být nasazeny, ale bude mít odlišné uzlu velikost pracovního vytížení a konfigurace softwaru na uzlu.

Tričko velikosti podle nabídky produktů, například komunity nebo Enterprise, může mít typy různých prostředků a maximální počet uzlů, které mohou být nasazeny, obvykle svázané s licencování aspekty nebo dostupnosti funkce napříč jiné nabídky.

Bylo možné ošetřit zákazníků s jedinečný variant pomocí šablony založené na JSON. Při plánování práce s odlehlé hodnoty, můžete začlenit na příslušné plánování a důležité informace pro vývoj, podpory a výpočet nákladů.

Na základě scénáře spotřeba šablon zákazníka a požadavky na začátku tohoto dokumentu identifikovat, myslíme, vzor šablony rozložením.

## <a name="capacity-and-capability-scoped-solution-templates"></a>Kapacity a schopností obor řešení šablony
Rozložením poskytuje modulární přístup ke vývoj šablon, že podporuje opakovaně, rozšiřitelnosti, testování a nástrojů. Tato část obsahuje podrobnosti o rozložením přístup jak lze použít pro šablony s rozsahem kapacity nebo funkce.

V tento postup hlavní šablonu přijímá hodnoty parametrů z šablony příjemce, a potom odkazuje na několik typů šablon a skripty po proudu, jak je uvedeno níže. Parametry, statické proměnné a vygenerovaný proměnné se používají k zadání hodnot a deaktivovat propojených šablon.

![Parametry šablony](./media/best-practices-resource-manager-design-templates/template-parameters.png)

**Parametry jsou předány hlavní šablonu, která pak má propojených šablon**

V následujících částech se zaměřit na typů šablon a skripty, které je jediné šabloně rozložit na. V části jsou uvedeny přístupy k předání informace o stavu z šablon. Každé šablony a typy skript v bitové kopii jsou popsány společně s příklady. Kontextová příklad najdete v tématu "jeho uvedení společně: Ukázka implementace" dále v tomto dokumentu.

### <a name="template-metadata"></a>Metadata šablony
Metadata šablony (soubor metadata.json) obsahuje dvojice klíč/hodnota, které popisují šablonu ve formátu JSON, který může číst člověkem a systémy softwaru.

![Metadata šablony](./media/best-practices-resource-manager-design-templates/template-metadata.png)

**Metadata šablony je popsaný v souboru metadata.json**

Agenti softwaru můžete načíst soubor metadata.json a publikovat informace a odkaz na šablonu v webové stránky nebo adresář. Prvky patří *itemDisplayName*, *popis*, *souhrnné*, *githubUsername*, a *dateUpdated*.

Příklad souboru jsou uvedeny níže v celé jeho šíři.

    {
        "itemDisplayName": "PostgreSQL 9.3 on Ubuntu VMs",
        "description": "This template creates a PostgreSQL streaming-replication between a master and one or more slave servers each with 2 striped data disks. The database servers are deployed into a private-only subnet with one publicly accessible jumpbox VM in a DMZ subnet with public IP.",
        "summary": "PostgreSQL stream-replication with multiple slave servers and a publicly accessible jumpbox VM",
        "githubUsername": "arsenvlad",
        "dateUpdated": "2015-04-24"
    }

### <a name="main-template"></a>Hlavní šablony
Hlavní šablona přijímá parametry od uživatele, používá tyto informace k vyplnění komplexní objektové proměnné a provede propojených šablon.

![Hlavní šablony](./media/best-practices-resource-manager-design-templates/main-template.png)

**Hlavní šablonu přijímá parametry od uživatele.**

Jeden parametr, který je k dispozici je typ známé konfigurace také označované jako parametr velikosti tričko z důvodu jeho standardizované hodnoty, například malé, střední nebo velké. V praxi můžete tento parametr několika způsoby. Podrobnosti najdete v tématu "Známé konfigurace prostředků šablona" dál v tomto dokumentu.

Některé prostředky nasadí bez ohledu na to známé konfigurace určený parametrem uživatele. Tyto prostředky se zřizují pomocí šablony jeden sdílený prostředek a jsou sdíleny jiné šablony, takže sdílený prostředek šablony je spustit jako první.

Některé prostředky bez ohledu na zadaný známé konfigurace nasazených volitelně.

### <a name="shared-resources-template"></a>Sdílené prostředky šablony
Tato šablona poskytuje prostředky, které jsou společné pro všechny známé konfigurace. Obsahuje virtuální síť, skupiny dostupnosti a další prostředky, které se vyžadují bez ohledu na šabloně známé konfigurace, která je nasazena.

![Šablony prostředků](./media/best-practices-resource-manager-design-templates/template-resources.png)

**Sdílené prostředky šablony**

Názvy prostředků, jako je například název virtuální sítě, jsou založené na šabloně hlavní. Můžete je zadat jako proměnné v rámci dané šablony nebo přijímat jako parametr od uživatele, podle požadavků vaší organizace.

### <a name="optional-resources-template"></a>Volitelné prostředků šablony
Šablona volitelné prostředků obsahuje prostředky, které jsou nasazeny prostřednictvím kódu programu na základě hodnoty, parametr nebo proměnná.

![Volitelné prostředky](./media/best-practices-resource-manager-design-templates/optional-resources.png)

**Volitelné prostředků šablony**

Například můžete použít šablonu volitelné prostředky ke konfiguraci jumpbox, která umožňuje nepřímý přístup k prostředí nasazené z veřejného Internetu. Parametr nebo proměnná byste použili k určení, zda by měla být povolená jumpbox a *concat* funkce k sestavení cílová pro danou šablonu, jako například *jumpbox_enabled.json*. Propojování šablony by proměnnou výsledné použít k instalaci jumpbox.

Můžete propojit šabloně volitelné prostředky z několika míst:

* V případě potřeby pro každé nasazení vytvořte propojení řízené parametry z šablony sdílených prostředků.
* V případě potřeby vyberte známé konfigurace – například nainstalovat pouze na velkých nasazeních – vytvořte odkaz řízené parametr nebo proměnná řízené ze známé konfigurace šablony.

Jestli daný prostředek volitelné nemusí být vycházejí z šablony příjemce ale zprostředkovatelem šablony. Například může potřebovat nesplňuje požadavek na konkrétní produkt nebo doplňkem produktu (společné pro sdílené svazky clusteru) a vynucování zásad (common pro službu SIs a enterprise IT skupiny). Proměnnou v těchto případech můžete použít k určení, zda prostředek by měl být nasazen.

### <a name="known-configuration-resources-template"></a>Známé konfigurace prostředků šablony
V šabloně hlavní mohou být zpřístupněny parametr umožnit příjemci šablony zadejte požadovanou známé konfiguraci, kterou chcete nasadit. Tato známé konfigurace často používá o přístupu velikost tričko s nastavenou velikostí pevné konfigurace, například izolovaného prostoru, malé, střední a velké.

![Známé konfigurace prostředků](./media/best-practices-resource-manager-design-templates/known-config.png)

**Známé konfigurace prostředků šablony**

Přístup velikost tričko se běžně používá, ale parametry může představovat libovolnou sadu známé konfigurace. Můžete například zadat sadu prostředí pro podniková aplikace například vývoj, testování a produktu. Nebo můžete použít pro cloudové služby představující jednotky různých škálování, verze produktu nebo konfigurace produktu, jako je například komunity, Developer a Enterprise.

Stejně jako u šabloně sdílený prostředek, jsou buď z proměnné předaný šabloně známé konfigurace:

* Koncový uživatel – to znamená, parametry, které jsou odeslána do hlavní šablony.
* Organizace – to znamená, proměnné v šabloně hlavní představující interní požadavky nebo zásady.

### <a name="member-resources-template"></a>Člen prostředků šablony
V rámci známé konfigurace jsou často jeden nebo více typů člen uzlu zahrnuty. Například s Hadoop máte řídicí uzly a datové uzly. Pokud instalujete MongoDB, máte datové uzly a arbiter. Pokud nasazujete DataStax, máte datové uzly a virtuální počítač s OpsCenter nainstalována.

![Členové zdroje](./media/best-practices-resource-manager-design-templates/member-resources.png)

**Člen prostředků šablony**

Každý typ uzlů může mít různé velikosti virtuálních počítačů, počtu připojených disků, skripty, které instalace a nastavení uzly, konfigurace portů pro virtuální počítače, počet instancí a další podrobnosti. Takže každý typ uzlu získá svůj vlastní člen prostředku šablony, která obsahuje podrobné informace o nasazení a konfiguraci infrastruktury a také provádění skriptů pro nasazení a konfigurace softwaru v rámci virtuálního počítače.

U virtuálních počítačů jsou obvykle dva typy skripty použité, široce opakovaně použitelný a vlastní skripty.

### <a name="widely-reusable-scripts"></a>Široce opakovaně použitelné skripty
Široce opakovaně použitelné skripty lze napříč více typů šablon. Jeden z lepší příklady tyto skripty široce opakovaně použitelné nastaví RAID v systému Linux fondu disků a získat větší počet IOPS. Bez ohledu na software instalován ve virtuálním počítači poskytuje tento skript opakované použití osvědčené postupy pro běžné scénáře.

![Skriptů pro opakované použití](./media/best-practices-resource-manager-design-templates/reusable-scripts.png)

**Šablony členských prostředků můžete volat široce opakovaně použitelné skripty**

### <a name="custom-scripts"></a>Vlastní skripty
Šablony běžně volání jeden nebo více skripty, které instalace a konfigurace softwaru v rámci virtuálních počítačů. Běžný vzor se seznámili s velké topologie, kde jsou nasazeny více instancí jedné nebo více typy členů. Instalační skript se zahájí pro každý virtuální počítač, který může běžet paralelně, za nímž následuje instalační skript, která je volána po všech virtuálních počítačů (nebo všech virtuálních počítačů typu daný člen) jsou nasazeny.

![Vlastní skripty](./media/best-practices-resource-manager-design-templates/custom-scripts.png)

**Šablony členských prostředků můžete volat skripty pro specifické účely, například konfigurace virtuálního počítače**

## <a name="capability-scoped-solution-template-example---redis"></a>Příklad šablony funkce obor řešení - Redis
Pokud chcete zobrazit, jak mohou fungovat implementace, podíváme se na praktický příklad vytvoření šablony, která usnadňuje nasazení a konfiguraci Redis v standardní tričko velikosti.  

Pro nasazení jsou sadu sdílené prostředky (virtuální sítě, účet úložiště, skupiny dostupnosti) a prostředek volitelné (jumpbox). Existuje více známé konfigurace vyjádřené tričko velikosti (malé, střední, velký), ale každý do jednoho uzlu zadejte. Existují také dva skripty zaměřené na konkrétní účel (instalace, konfigurace).

### <a name="creating-the-template-files"></a>Vytváření soubory šablon
Vytvoříte hlavní šablonu s názvem azuredeploy.json.

Vytvoření sdílené prostředky šablony s názvem sdílené resources.json

Vytvoření šablony umožňující nasazení jumpbox, s názvem jumpbox_enabled.json aplikace volitelné prostředků

Redis používá jenom jeden uzel typu, takže vytvoříte jeden šablonu prostředků člen s názvem resources.json uzlu.

S Redis budete chtít nainstalovat každého jednotlivého uzlu a pak nastavte cluster.  Máte skripty k přizpůsobení instalace a nastavení, redis-cluster-install.sh a redis-cluster-setup.sh.

### <a name="linking-the-templates"></a>Propojování šablony
Pomocí šablony propojení na hlavní šablonu odkazy na šablony sdílené prostředky, která vytvoří virtuální síť.

V rámci hlavní šablony k povolení příjemcům šablony k určení, zda mají být nasazeny jumpbox je přidat logiku. *Povoleno* hodnota *EnableJumpbox* parametr označuje, že zákazník chce nasadit jumpbox. Pokud je tato hodnota je zadaný, šablony zřetězí *_enabled* jako přípona názvu základní šablony pro funkci jumpbox.

Hlavní šablona platí *velké* hodnota parametru jako příponu na název základní šablony tričko velikosti a pak používá tuto hodnotu v šabloně odkaz na *technology_on_os_large.json*.

Topologie by vypadat podobně jako na obrázku.

![Redis šablony](./media/best-practices-resource-manager-design-templates/redis-template.png)

**Struktura šablony pro šablonu Redis**

### <a name="configuring-state"></a>Konfigurace stavu
Pro uzly v clusteru jsou dva kroky ke konfiguraci stavu, obě reprezentována skriptů pro konkrétní účel.  nainstaluje Redis "redis-cluster-install.sh" a "redis-cluster-setup.sh" nastavuje clusteru.

### <a name="supporting-different-size-deployments"></a>Podpora nasazení jinou velikost
Uvnitř proměnné, šablona velikost tričko určuje počet uzlů každého typu nasazení pro zadané velikosti (*velké*). Se pak nasadí počet instancí virtuálního počítače pomocí smyčky prostředků, poskytuje jedinečné názvy k prostředkům připojením název uzlu s číslem číselného pořadí z *copyIndex()*. Provede tyto kroky úrovněmi horkého a záložním zóny virtuálních počítačů, jak jsou definovány v šabloně tričko název

## <a name="decomposition-and-end-to-end-solution-scoped-templates"></a>Rozložením a začátku do konce řešení obor šablony
Šablona řešení s oborem začátku do konce řešení se zaměřuje na-komplexní řešení.  Tento přístup je obvykle složení více šablon funkce obor s další prostředky, logiku a stavu.

Jak je znázorněno na obrázku níže je stejný model používá pro šablony funkce obor rozšířeno pro šablony s oborem řešení začátku do konce.

Sdílené prostředky šablony a volitelné šablony prostředků slouží stejnou funkci jako kapacity a schopností obor přístupy šablony, ale jsou určené pro komplexní řešení.

Jako obor koncová řešení šablony také můžete obvykle mají i tričko velikosti, známé konfigurace prostředků šablony odráží se požaduje pro danou konfiguraci známé řešení.

Známé konfigurace prostředků šablony odkazy na jeden nebo více schopností obor šablony řešení, které jsou relevantní pro komplexní řešení, a také šablony členských prostředků, které jsou požadovány pro komplexní řešení.

Jako velikost trička řešení může být jiný než jednotlivé funkce obor šablona, proměnné v rámci šablony známé prostředky konfigurace slouží k zadejte odpovídající hodnoty pro podřízené schopností obor řešení šablony Nasaďte odpovídající velikost tričko.

![Klient server](./media/best-practices-resource-manager-design-templates/end-to-end.png)

**Model použitý pro kapacitu nebo schopnosti obor řešení, které šablony lze snadno rozšířit pro komplexní řešení šablony obory**

## <a name="preparing-templates-for-the-marketplace"></a>Příprava šablony pro Marketplace.
Předchozí postup snadno přizpůsobí scénáře, kde podniky, SIs a sdílené svazky clusteru chcete nasazení šablon sami nebo povolit zákazníkům nasadit na své vlastní.

Jiné potřeby, že je scénář nasazení šablony prostřednictvím marketplace.  Tento přístup rozložením funguje pro marketplace taky s menšími změnami.

Jak je uvedeno nahoře, šablony umožňuje nabídnout různých typů pro prodej v marketplace. Typy nasazení DISTINCT může být tričko velikosti (malé, střední, velký), typ produktu nebo cílové skupiny (komunity, developer, enterprise) nebo typ funkce (základní, vysoké dostupnosti).

Jak vidíte níže, existující komplexní řešení nebo šablon funkce obor, můžete snadno použít seznam různých známé konfigurace v marketplace.

Parametry, které chcete hlavní šablony jsou nejprve upravit tak, aby příchozí parametr s názvem tshirtSize odebrat.

Při různých typů mapovat do šablony známé konfigurace prostředků, potřebují také běžných prostředků a konfigurace v šabloně sdílené prostředky a potenciálně těmi ve volitelné prostředku šablony nalezen.

Pokud chcete publikovat šablony na marketplace, můžete vytvořit odlišné kopie vaší hlavní šablony, který nahrazuje parametr dříve k dispozici příchozí tshirtSize proměnné vložených v rámci šablony.

![Marketplace](./media/best-practices-resource-manager-design-templates/marketplace.png)

**Přizpůsobení řešení obor šablonu pro marketplace.**

## <a name="next-steps"></a>Další kroky
* Další informace o stavu sdílení do a z šablony najdete v tématu [sdílení stavu v šablonách Azure Resource Manager](best-practices-resource-manager-state.md).
* Pokyny k tomu, jak můžou podniky používat Resource Manager k efektivní správě předplatných, najdete v části [Základní kostra Azure Enterprise – zásady správného řízení pro předplatná](resource-manager-subscription-governance.md).

