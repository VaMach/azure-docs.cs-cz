---
title: "Co je Azure Event Hubs a proč tuto službu používat | Microsoft Docs"
description: "Přehled a úvod do služby Azure Event Hubs – ingestování telemetrických údajů z webů, aplikací a zařízení v cloudovém měřítku"
services: event-hubs
documentationcenter: .net
author: banisadr
ms.assetid: 
ms.service: event-hubs
ms.topic: get-started-article
ms.date: 11/29/2016
ms.author: sethm; babanisa
translationtype: Human Translation
ms.sourcegitcommit: aa7244849f6286e8ef9f9785c133b4c326193c12
ms.openlocfilehash: 62eefb7a4591c712c5389d3ed7e5ff9675a80042


---
# <a name="what-is-azure-event-hubs"></a>Co je služba Azure Event Hubs?
Event Hubs je vysoce škálovatelná platforma pro streamování dat, která je schopná miliony událostí za sekundu. Data odeslaná do centra událostí je možné transformovat a uložit pomocí libovolného poskytovatele analýz v reálném čase nebo adaptérů pro dávkové zpracování a ukládání. Díky možnosti publikování a odebírání dat s nízkou latencí a v masivním měřítku slouží služba Event Hubs jako vstupní brána k velkým objemům dat.

## <a name="why-use-event-hubs"></a>Proč používat Event Hubs
Možnosti zpracování telemetrických údajů a událostí, které služba Event Hubs nabízí, jsou zvláště užitečné pro:

* instrumentaci aplikací
* zpracování činnosti nebo pracovního postupu koncového uživatele
* scénáře související s internetem věcí (IoT)

Mezi další možnosti služby Event Hubs patří sledování chování v mobilních aplikacích, informace o datových přenosech z webových farem, zachycení událostí v hrách na konzolách nebo shromažďování telemetrických údajů z průmyslových strojů či připojených vozidel.

## <a name="azure-event-hubs-overview"></a>Přehled služby Azure Event Hubs
V architekturách řešení hraje služba Event Hubs běžně roli „předních dveří“ pro kanál událostí, který se často nazývá *přijímač událostí*. Přijímač událostí je komponenta nebo služba, která se nachází mezi zdroji událostí a příjemci událostí a slouží k oddělení produkce datového proudu událostí od spotřeby těchto událostí.

![Event Hubs](./media/event-hubs-what-is-event-hubs/event_hubs_full_pipeline.png)

Služba Event Hubs zpracovává události a telemetrii v cloudovém měřítku, s nízkou latencí a vysokou spolehlivostí. Služba Event Hubs poskytuje možnost zpracovávat datové proudy zpráv. Její charakteristiky ji značně odlišují od tradičních podnikových způsobů zasílání zpráv. Možnosti služby Event Hubs jsou vycházejí ze scénářů zpracování událostí a vysoké propustnosti. V důsledku toho služba Event Hubs neimplementuje některé možnosti zasílání zpráv, které jsou dostupné pro entity přenosu zpráv, jako jsou třeba témata.

Centrum událostí se vytváří na úrovni oboru názvů a jako svoje primární rozhraní API využívá AMQP a HTTP.

## <a name="event-publishers"></a>Zdroje událostí
Každá entita, která odesílá data do centra událostí, je *zdroj události*. Zdroje událostí mohou publikovat události pomocí protokolu HTTPS nebo AMQP 1.0. Zdroje událostí se v centru událostí identifikují pomocí tokenu sdíleného přístupového podpisu (SAS) a mohou mít jedinečnou identitu, nebo mohou používat společný token SAS.

### <a name="publishing-an-event"></a>Publikování události
Událost můžete publikovat prostřednictvím protokolu AMQP 1.0 nebo HTTPS. Služba Service Bus poskytuje třídu [EventHubClient](https://docs.microsoft.com/dotnet/api/microsoft.servicebus.messaging.eventhubclient), která slouží k publikování událostí od klientů .NET do centra událostí. Pro jiné moduly runtime a platformy můžete použít libovolného klienta protokolu AMQP 1.0, například [Apache Qpid](http://qpid.apache.org/). Události můžete publikovat samostatně nebo v dávce. Jedna publikace (instance dat události) je omezena limitem 256 KB – bez ohledu na to, jestli se jedná o jedinou událost nebo dávku (batch). Pokus publikovat události, které tento limit přesahují, skončí chybou. Nejvhodnějším postupem pro zdroje je nezajímat se o oddíly v centru událostí a specifikovat pouze *klíč oddílu* (představíme v další části) nebo svoji identitu prostřednictvím tokenu SAS.

Volba, jestli se použije protokol AMQP nebo HTTPS, závisí na konkrétním scénáři použití. Protokol AMQP vyžaduje nejen protokol TLS (Transport Level Security) nebo SSL/TLS, ale i vytvoření trvalého obousměrného soketu. AMQP má vyšší náklady na síť při inicializaci relace, ale HTTPS pro každý požadavek vyžaduje další režii SSL. AMQP má pro často používané zdroje vyšší výkon.

![Event Hubs](./media/event-hubs-what-is-event-hubs/partition_keys.png)

Služba Event Hubs zajišťuje, aby se všechny události, které sdílejí stejnou hodnotu klíče oddílu, v pořádku doručily do stejného oddílu. Pokud se klíče oddílů používají společně se zásadami zdroje, musí si identita zdroje a hodnota klíče oddílu odpovídat. V opačném případě dojde k chybě.

### <a name="publisher-policy"></a>Zásady zdroje
Služba Event Hubs umožňuje podrobnou kontrolu nad zdroji událostí prostřednictvím *zásad zdroje*. Zásady zdroje jsou běhové funkce, které byly navržené pro usnadnění kontroly nad velkým množstvím nezávislých zdrojů událostí. Zásady zdroje poskytují s použitím následujícího mechanismu každému zdroji vlastní identifikátor, který se používá při publikování událostí do centra událostí.

```
//[my namespace].servicebus.windows.net/[event hub name]/publishers/[my publisher name]
```

Názvy zdrojů není potřeba vytvářet dopředu, při publikování události ale musí odpovídat tokenu SAS, aby se zajistilo, že každý zdroj bude mít nezávislou identitu. Při použití zásad zdroje se každému názvu zdroje nastaví hodnota **PartitionKey** (Klíč oddílu). Aby vše správně fungovalo, musí tyto hodnoty odpovídat.

## <a name="partitions"></a>Oddíly
Služba Event Hubs poskytuje datový proud zpráv pomocí schématu rozdělujícího datový proud na oddíly pro jednotlivé příjemce. To zajišťuje, aby každý příjemce četl jenom konkrétní podmnožinu nebo oddíl datového proudu zpráv. Toto schéma umožňuje vodorovné škálování zpracování událostí a poskytuje další funkce zaměřené na datový proud, které nejsou ve frontách a tématech k dispozici.

Oddíl je seřazená posloupnost událostí, která se nachází v centru událostí. Události, které nově přichází, se zařazují na konec této posloupnosti. Oddíl si lze představit jako „protokol transakcí“.

![Event Hubs](./media/event-hubs-what-is-event-hubs/partition.png)

Služba Event Hubs uchovává data po nakonfigurovanou dobu uchování, která se vztahuje na všechny oddíly centra událostí. Události mizí na základě času, nemůžete je explicitně odstranit. Vzhledem k tomu, že oddíly jsou nezávislé a obsahují vlastní posloupnost dat, často se jejich velikost mění různým tempem.

![Event Hubs](./media/event-hubs-what-is-event-hubs/multiple_partitions.png)

Počet oddílů je určený při vytvoření a musí být v rozsahu 2 až 32. Počet oddílů není možné měnit. Proto je při nastavování počtu oddílů potřeba uvažovat z dlouhodobého hlediska. Oddíly slouží jako mechanismus pro organizaci dat a souvisí se stupněm paralelismu příjmu dat, který vyžadují přijímací aplikace. Počet oddílů v centru událostí přímo souvisí s počtem souběžných čtenářů, které plánujete mít. Pokud chcete použít vyšší počet oddílů než 32, kontaktujte tým služby Event Hubs.

I když lze oddíly identifikovat a je možné posílat jim data přímo, odesílání dat na konkrétní oddíly se nedoporučuje. Místo toho můžete použít konstrukce vyšší úrovně, které představujeme v tématech věnovaných [zdroji událostí](#event-publishers) a [kapacitě](#capacity).

Oddíly jsou naplněné posloupností dat událostí, která obsahují tělo události, uživatelem definovaný kontejner objektů a dat a různá metadata události, jako je třeba posun v rámci oddílu a pořadí v posloupnosti datového proudu.

### <a name="partition-key"></a>Klíč oddílu
Klíč oddílu můžete použít k mapování příchozích dat událostí do konkrétních oddílů pro účely organizace dat. Klíč oddílu je hodnota zadaná odesílatelem, která byla předaná do centra událostí. Zpracovává se pomocí statické hashovací funkce, která vytvoří přiřazení k oddílu. Pokud při publikování události nezadáte klíč oddílu, použije se přiřazení metodou kruhového dotazování.

Zdroj události zná jenom svůj klíč oddílu, a ne oddíl, do kterého se události publikují. Díky tomuto oddělení klíče a oddílu odesílatel toho nepotřebuje vědět o zpracování příjmu dat příliš mnoho. Vhodným klíčem oddílu je jedinečná identita uživatele nebo zařízení, ale k seskupení souvisejících událostí do jednoho oddílu je možné použít i další atributy, například geografickou polohu.

## <a name="sas-tokens"></a>Tokeny SAS
Event Hubs využívá *sdílené přístupové podpisy*, které jsou dostupné na úrovni oboru názvů a centra událostí. Token SAS, který se generuje z klíče SAS, je adresa URL, která je zašifrovaná pomocí hašovacího algoritmu SHA a zakódovaná ve specifickém formátu. Služba Event Hubs může znovu vygenerovat hash kód pomocí názvu klíče (zásady) a tokenu, a ověřit tak odesílatele. Za normálních okolností se tokeny SAS pro zdroje událostí vytváří jen s oprávněními pro **odesílání** na konkrétní centrum událostí. Tento mechanismus adresy URL a tokenu SAS slouží jako základ pro identifikaci zdroje, kterou představíme v části o zásadách zdroje. Další informace o práci s tokenem SAS naleznete v tématu o [ověřování u služby Service Bus pomocí sdíleného přístupového podpisu](../service-bus-messaging/service-bus-shared-access-signature-authentication.md).

## <a name="event-consumers"></a>Příjemci událostí
*Příjemcem událostí* je každá entita, která čte data událostí z centra událostí. Všichni příjemci se ve službě Event Hubs připojují pomocí relace protokolu AMQP 1.0 a události se doručují tak, jak jsou postupně dostupné. Klient se na dostupnost dat nemusí dotazovat.

### <a name="consumer-groups"></a>Skupiny příjemců
Mechanismus publikování/odebírání ve službě Event Hubs umožňují *skupiny příjemců*. Skupina příjemců je zobrazení (stavu, pozice nebo posunu) celého centra událostí. Skupiny příjemců poskytují různým přijímajícím aplikacím oddělená zobrazení datového proudu událostí a umožňují jim nezávisle číst datový proud vlastním tempem a s použitím vlastních posunů.

V architektuře zpracování datového proudu se každá aplikace pro příjem dat rovná skupině příjemců. Pokud chcete zapisovat data událostí do dlouhodobého úložiště, pak je aplikace, která do úložiště zapisuje, skupinou příjemců. Komplexní zpracování událostí zase může provádět jiná, samostatná skupina příjemců. K oddílům můžete přistupovat pouze prostřednictvím skupiny příjemců. Každý oddíl může mít **z konkrétní skupiny příjemců** vždycky jenom jednoho aktivního čtenáře. V centru událostí je vždy jedna výchozí skupina příjemců. Na standardní úrovni centra událostí můžete vytvořit až dvacet skupin příjemců.

Následují příklady konvenčního zápisu identifikátoru URI skupiny příjemců:

```
//[my namespace].servicebus.windows.net/[event hub name]/[Consumer Group #1]
//[my namespace].servicebus.windows.net/[event hub name]/[Consumer Group #2]
```

![Event Hubs](./media/event-hubs-what-is-event-hubs/event_hubs_architecture.png)

### <a name="stream-offsets"></a>Posuny datového proudu
*Posun* je pozice události v rámci oddílu. Posun si můžete představit jako kurzor na straně klienta. Posun je číslo bajtu události. To umožňuje příjemci události (čtenáři) určit bod v datovém proudu událostí, od kterého chce události začít číst. Posun můžete zadat v podobě časového razítka nebo hodnoty posunu. Příjemci si sami zodpovídají za uložení svých hodnot posunu mimo službu Event Hubs. Každá událost v rámci oddílu zahrnuje posun.

![Event Hubs](./media/event-hubs-what-is-event-hubs/partition_offset.png)

### <a name="checkpointing"></a>Vytváření kontrolních bodů
*Vytváření kontrolních bodů* je proces, pomocí kterého čtenáři označují nebo potvrzují svou pozici v rámci posloupnosti událostí v oddílu. Za vytváření kontrolních bodů zodpovídá příjemce. Proces probíhá na bázi oddílů ve skupinách příjemců. To znamená, že si každý čtenář oddílu v každé skupině příjemců musí udržovat přehled o své aktuální pozici v datovém proudu událostí a může informovat službu, když bude považovat datový proud za dokončený.

Pokud se čtenář z oddílu odpojí, začne při opětovném připojení číst od kontrolního bodu, který dříve zaslal poslední čtenář daného oddílu z této skupiny příjemců. Když se čtenář připojí, předá tento posun do centra událostí, a určí tak umístění, od kterého začne číst. Takto můžete vytváření kontrolních bodů použít jak k označování událostí jako „dokončených“, tak k zajištění ochrany pro případ, že nastane selhání u čtenářů spuštěných na různých strojích. Ke starším datům se je možné vrátit tak, že určíte nižší posun od tohoto kontrolního bodu. Díky tomuto mechanismu umožňuje vytváření kontrolních bodů nejen obnovu při selhání, ale i opakované přehrání datového proudu.

### <a name="common-consumer-tasks"></a>Běžné úlohy příjemce
Všichni příjemci se ve službě Event Hubs připojují prostřednictvím obousměrného komunikačního kanálu AMQP 1.0., který rozpoznává relaci a stav. Každý oddíl má relaci AMQP 1.0, která usnadňuje transport událostí rozdělených do oddílů.

#### <a name="connect-to-a-partition"></a>Připojení k oddílu
Při přímém připojení k oddílům se obvykle používá mechanismus „pronájmu“, aby se připojení čtenářů ke konkrétním oddílům koordinovala. Díky tomu je možné mít u každého oddílu ve skupině příjemců pouze jednoho aktivního čtenáře. Použití kontrolních bodů, pronájem a správu čtenářů zjednodušuje použití třídy [EventProcessorHost](https://docs.microsoft.com/dotnet/api/microsoft.servicebus.messaging.eventprocessorhost) pro klienty .NET. [EventProcessorHost](https://docs.microsoft.com/dotnet/api/microsoft.servicebus.messaging.eventprocessorhost) je inteligentní agent příjemce.

#### <a name="read-events"></a>Čtení událostí
Po otevření připojení a relace AMQP 1.0 u konkrétního oddílu služba Event Hubs doručí události do klienta protokolu AMQP 1.0. Tento mechanismus doručení umožňuje vyšší propustnost a nižší latenci než mechanismy založené na operaci Pull, jako je například metoda GET protokolu HTTP. Události se posílají klientovi a každá instance dat události obsahuje důležitá metadata, jako je posun a číslo posloupnosti, která se používají k zjednodušení vytváření kontrolních bodů v posloupnosti událostí.

Data události:
* Posun
* Pořadové číslo
* Tělo
* Uživatelské vlastnosti
* Systémové vlastnosti

Správa posunu je na vás.

## <a name="capacity"></a>Kapacita
Služba Event Hubs využívá vysoce škálovatelnou paralelní architekturu a při nastavování velikosti a škálování je potřeba zvážit několik klíčových aspektů.

### <a name="throughput-units"></a>Jednotky propustnosti
Kapacita propustnosti je ve službě Event Hubs řízená prostřednictvím *jednotek propustnosti*. Jednotky propustnosti jsou předem zakoupené jednotky kapacity. Jedna jednotka propustnosti zahrnuje následující:

* Příchozí data: Až 1 MB za sekundu nebo 1000 událostí za sekundu (podle toho, co nastane dříve)
* Odchozí data: Až 2 MB za sekundu

Nad rámec kapacity zakoupených jednotek propustnosti je příjem příchozích dat omezen a vrátí se výjimka [ServerBusyException](https://docs.microsoft.com/dotnet/api/microsoft.azure.eventhubs.serverbusyexception). Odchozí data nezpůsobují takové výjimky, ale jsou omezená na objem přenosu dat, který poskytují zakoupené jednotky propustnosti. Pokud se vám objevují výjimky související s frekvencí publikování nebo v budoucnu očekáváte větší objem odchozích dat, zkontrolujte, kolik jednotek propustnosti jste pro konkrétní obor názvů zakoupili. Ke správě jednotek propustnosti můžete využít okno **Škálování** oboru názvů na webu [Azure Portal][Azure portal]. Stejného účelu můžete také dosáhnout programově, a to využitím rozhraní Azure API.

Jednotky propustnosti se kupují předem a účtují se po hodinách. Zakoupené jednotky propustnosti se účtují minimálně za jednu hodinu. Pro obor názvů služby Event Hubs můžete zakoupit až 20 jednotek propustnosti, které se sdílejí napříč všemi centry událostí v tomto oboru názvů.

Další jednotky propustnosti je možné zakoupit v blocích po 20 jednotkách propustnosti (maximálně 100) po domluvě s pracovníky podpory Azure. Navíc si můžete taky zakoupit bloky, které obsahují sto jednotek propustnosti.

Doporučujeme, abyste jednotky propustnosti a oddíly vyvážili. Jedině tak dosáhnete optimálního škálování. Škálování jednoho oddílu dovoluje maximálně jednu jednotku propustnosti. Počet jednotek propustnosti by měl být menší nebo roven počtu oddílů v centru událostí.

Podrobné informace o cenách naleznete v článku o [cenách služby Event Hubs](https://azure.microsoft.com/pricing/details/event-hubs/).

## <a name="next-steps"></a>Další kroky

* Úvodní [kurz služby Event Hubs][Event Hubs tutorial]
* Úplná [ukázková aplikace, která používá službu Event Hubs]
* [Průvodce programováním pro službu Event Hubs](event-hubs-programming-guide.md)
* [Nejčastější dotazy k Event Hubs](event-hubs-faq.md)

[Event Hubs tutorial]: event-hubs-csharp-ephcs-getstarted.md
[ukázková aplikace, která používá službu Event Hubs]: https://code.msdn.microsoft.com/Service-Bus-Event-Hub-286fd097
[Azure portal]: https://portal.azure.com



<!--HONumber=Feb17_HO1-->


