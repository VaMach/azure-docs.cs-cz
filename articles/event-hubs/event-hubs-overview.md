<properties 
    pageTitle="Přehled služby Azure Event Hubs | Microsoft Azure"
    description="Představení a přehled služby Azure Event Hubs."
    services="event-hubs"
    documentationCenter="na"
    authors="sethmanheim"
    manager="timlt"
    editor="" />
<tags 
    ms.service="event-hubs"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.tgt_pltfrm="na"
    ms.workload="na"
    ms.date="08/16/2016"
    ms.author="sethm" />


# <a name="azure-event-hubs-overview"></a>Přehled služby Azure Event Hubs

Mnohá moderní řešení jsou navržená tak, aby poskytovala adaptivní uživatelská prostředí nebo vylepšovala produkty pomocí neustálé zpětné vazby a automatizované telemetrie. Taková řešení se potýkají s otázkou, jak bezpečně a spolehlivě zpracovávat obrovské množství informací od mnoha souběžných zdrojů. Služba Microsoft Azure Event Hubs je spravovaná platforma, která poskytuje základy pro příjem velkých objemů dat u mnoha rozličných scénářů. Mezi příklady takových scénářů patří sledování chování v mobilních aplikacích, informace o datových přenosech z webových farem, zachycení událostí v hrách na konzolách nebo shromažďování telemetrických údajů z průmyslových strojů či připojených vozidel. V architekturách řešení hraje služba Event Hubs běžně roli „předních dveří“ pro kanál událostí, který se často nazývá *přijímač událostí*. Přijímač událostí je komponenta nebo služba, která se nachází mezi zdroji událostí a příjemci událostí a slouží k oddělení produkce datového proudu událostí od spotřeby těchto událostí.

![Event Hubs](./media/event-hubs-overview/IC759856.png)

Služba Azure Event Hubs zpracovává události a zajišťuje příjem příchozích dat událostí a telemetrie do cloudu v masivním měřítku, s nízkou latencí a vysokou spolehlivostí. Tato služba je v kombinaci s dalšími službami pro příjem dat obzvlášť užitečná pro scénáře instrumentace aplikací, zpracování činnosti nebo pracovního postupu koncového uživatele a internetu věcí (IoT). Služba Event Hubs poskytuje možnost zpracovávat datové proudy zpráv. Centra událostí (Event Hubs) jsou entity podobné frontám a tématům, ale mají i charakteristiky, které je značně odlišují od tradičních podnikových způsobů zasílání zpráv. Podnikové scénáře zasílání zpráv běžně vyžadují sofistikované funkce, jako je sekvencování, dead-lettering, podpora transakcí nebo silné záruky doručení, ačkoliv je při přijímání událostí hlavním zájmem zajistit u datových proudů událostí vysokou propustnost a flexibilitu zpracování. Možnosti služby Event Hubs se tudíž liší od témat sběrnice Service Bus tím, že výrazně upřednostňují scénáře vysoké propustnosti a zpracování událostí. V důsledku toho služba Event Hubs neimplementuje některé možnosti zasílání zpráv, které jsou k dispozici pro témata. Pokud tyto funkce potřebujete, témata zůstávají optimální volbou.

Centrum událostí (Event Hub) se podobně jako fronty a témata ve sběrnici Service Bus vytváří na úrovni oboru názvů Event Hubs. Služba Event Hubs využívá jako svá primární rozhraní API protokoly AMQP a HTTP. Následující diagram znázorňuje vztah mezi službami Event Hubs a Service Bus.

![Event Hubs](./media/event-hubs-overview/IC741188.png)

## <a name="conceptual-overview"></a>Koncepční přehled

Služba Event Hubs poskytuje datový proud zpráv pomocí schématu rozdělujícího datový proud na oddíly pro jednotlivé příjemce. Fronty a témata využívají model [Competing Consumer](https://msdn.microsoft.com/library/dn568101.aspx), ve kterém se všichni příjemci pokouší číst ze stejné fronty nebo zdroje. Tato soutěž o zdroje ve výsledku přináší aplikacím, které zpracovávají datové proudy, přílišnou složitost a omezení škálování. Služba Event Hubs využívá schéma oddílů pro jednotlivé příjemce, které zajišťuje, aby každý příjemce četl pouze konkrétní podmnožinu nebo oddíl datového proudu zpráv. Toto schéma umožňuje vodorovné škálování zpracování událostí a poskytuje další funkce zaměřené na datový proud, které nejsou ve frontách a tématech k dispozici.

### <a name="partitions"></a>Oddíly

Oddíl je seřazená posloupnost událostí, která se nachází v centru událostí. Události, které nově přichází, se zařazují na konec této posloupnosti. Oddíl si lze představit jako „protokol transakcí“.

![Event Hubs](./media/event-hubs-overview/IC759857.png)

Oddíly uchovávají data po nakonfigurovanou dobu uchování, která se nastavuje na úrovni centra událostí. Toto nastavení platí pro všechny oddíly v centru událostí. Události mizí na základě času, nemůžete je explicitně odstranit. Každé centrum událostí obsahuje několik oddílů. Všechny oddíly jsou nezávislé a obsahují vlastní posloupnosti dat. Kvůli tomu oddíly často rostou různým tempem.

![Event Hubs](./media/event-hubs-overview/IC759858.png)

Počet oddílů se určí v okamžiku vytvoření centra událostí a musí být mezi 2 a 32 (výchozí hodnota je 4). Oddíly slouží jako mechanismus pro organizaci dat a více souvisí se stupněm paralelismu příjmu dat, který vyžadují přijímací aplikace, než s propustností služby Event Hubs. To znamená, že výběr počtu oddílů v centru událostí přímo souvisí s počtem souběžných čtenářů, které plánujete mít. Jakmile centrum událostí vytvoříte, už počet oddílů nemůžete změnit. Proto je třeba zvážit tento počet s ohledem na dlouhodobě očekávanou škálu. Pokud chcete zvýšit limit 32 oddílů, kontaktujte tým Service Bus.

I když lze oddíly identifikovat a je možné posílat jim data přímo, odesílání dat na konkrétní oddíly se nedoporučuje. Místo toho můžete použít konstrukce vyšší úrovně, které představujeme v částech o [zdroji událostí](#event-publisher) a [zásadách zdroje](#capacity-and-security).

V kontextu služby Event Hubs se zprávy označují jako *data událostí*. Data událostí obsahují tělo události, uživatelem definovaný kontejner objektů a dat a různá metadata události, například její posun v oddílu a pořadí v posloupnosti datového proudu. Oddíly jsou vyplněny posloupností dat událostí.

## <a name="event-publisher"></a>Zdroj události

Každá entita, která odesílá události nebo data do centra událostí, je *zdroj události*. Zdroje událostí můžou publikovat události pomocí protokolu HTTPS nebo AMQP 1.0. Zdroje událostí se v centru událostí identifikují pomocí tokenu sdíleného přístupového podpisu (SAS) a v závislosti na požadavcích scénáře mohou mít jedinečnou identitu nebo používat společný token SAS.

Další informace o práci s tokenem SAS naleznete v tématu o [ověřování u služby Service Bus pomocí sdíleného přístupového podpisu](../service-bus-messaging/service-bus-shared-access-signature-authentication.md).

### <a name="common-publisher-tasks"></a>Běžné úlohy zdroje

Tato část popisuje běžné úlohy zdrojů událostí.

#### <a name="acquire-a-sas-token"></a>Získání tokenu SAS

Sdílený přístupový podpis (SAS) je ověřovací mechanismus služby Event Hubs. Služba Service Bus poskytuje zásady SAS na úrovni oboru názvů a centra událostí. Token SAS, který se generuje z klíče SAS, je adresa URL, která je zašifrovaná pomocí hašovacího algoritmu SHA a zakódovaná ve specifickém formátu. Služba Service Bus může znovu vygenerovat hash kód pomocí názvu klíče (zásady) a tokenu, a ověřit tak odesílatele. Za normálních okolností se tokeny SAS pro zdroje událostí vytváří jen s oprávněními pro **odesílání** na konkrétní centrum událostí. Tento mechanismus adresy URL a tokenu SAS slouží jako základ pro identifikaci zdroje, kterou představíme v části o zásadách zdroje. Další informace o práci s tokenem SAS naleznete v tématu o [ověřování u služby Service Bus pomocí sdíleného přístupového podpisu](../service-bus-messaging/service-bus-shared-access-signature-authentication.md).

#### <a name="publishing-an-event"></a>Publikování události

Událost můžete publikovat prostřednictvím protokolu AMQP 1.0 nebo HTTPS. Služba Service Bus poskytuje třídu [EventHubClient](https://msdn.microsoft.com/library/microsoft.servicebus.messaging.eventhubclient.aspx), která slouží k publikování událostí od klientů .NET do centra událostí. Pro jiné moduly runtime a platformy můžete použít libovolného klienta protokolu AMQP 1.0, například [Apache Qpid](http://qpid.apache.org/). Události můžete publikovat samostatně nebo v dávce. Jedna publikace (instance dat událostí) je omezena limitem 256 KB – bez ohledu na to, jestli se jedná o jedinou událost nebo dávku (batch). Pokus publikovat události, které tento limit přesahují, skončí chybou. Nejvhodnějším postupem pro zdroje je nezajímat se o oddíly v centru událostí a specifikovat pouze *klíč oddílu* (představíme v další části) nebo svoji identitu prostřednictvím tokenu SAS.

Volba, jestli se použije protokol AMQP nebo HTTPS, závisí na konkrétním scénáři použití. Protokol AMQP vyžaduje nejen protokol TLS (Transport Level Security) nebo SSL/TLS, ale i vytvoření trvalého obousměrného soketu. To může být z hlediska síťového provozu nákladná operace, která ale nastává jen při spuštění relace protokolu AMQP. Protokol HTTPS má nižší počáteční režijní náklady, ale vyžaduje další režii v rámci SSL pro každý požadavek. Zdrojům, které často publikují události, nabízí protokol AMQP značné úspory v oblasti výkonu, latence a propustnosti.

### <a name="partition-key"></a>Klíč oddílu

Klíč oddílu je hodnota, která se používá k mapování příchozích dat událostí do konkrétních oddílů pro účely organizace dat. Klíč oddílu je hodnota zadaná odesílatelem, která byla předaná do centra událostí. Zpracovává se pomocí statické hašovací funkce, jejímž výsledkem je, že se vytvoří přiřazení k oddílu. Pokud při publikování události nezadáte klíč oddílu, použije se přiřazení metodou kruhového dotazování. Zdroj události, který použije klíč oddílu, zná pouze svůj klíč oddílu, nikoli oddíl, do kterého se události publikují. Díky tomuto oddělení klíče a oddílu odesílatel nepotřebuje vědět příliš mnoho o zpracování příjmu dat a ukládání událostí. Klíče oddílů jsou důležité pro uspořádání dat při zpracovávání přijímaných dat, ale v zásadě se samotnými oddíly nesouvisí. Vhodným klíčem oddílu je jedinečná identita uživatele nebo zařízení, ale k seskupení souvisejících událostí do jednoho oddílu je možné použít i další atributy, například geografickou polohu. Následující obrázek znázorňuje, jak se odesílatelé událostí připojují k oddílům pomocí klíčů oddílů.

![Event Hubs](./media/event-hubs-overview/IC759859.png)

Služba Event Hubs zajišťuje, aby se všechny události, které sdílejí stejnou hodnotu klíče oddílu, doručily ve správném pořadí a do stejného oddílu. Pokud se klíče oddílů používají společně se zásadami zdroje (popsanými v následující sekci), je důležité, aby odpovídala identita zdroje a hodnota klíče oddílu. V opačném případě dojde k chybě.

### <a name="event-consumer"></a>Příjemce událostí

Každá entita, která čte data událostí z centra událostí, je příjemce událostí. Všichni příjemci událostí čtou datový proud událostí prostřednictvím oddílů ve skupině příjemců. Každý oddíl by měl mít v každé chvíli pouze jednoho aktivního čtenáře. Všichni příjemci se ve službě Event Hubs připojují pomocí relace protokolu AMQP 1.0, v níž se události doručují ve chvíli, kdy se stanou dostupnými. Klient se na dostupnost dat nemusí dotazovat.

#### <a name="consumer-groups"></a>Skupiny příjemců

Mechanismus publikování/odebírání ve službě Event Hubs umožňují skupiny příjemců. Skupina příjemců je zobrazení (stavu, pozice nebo posunu) celého centra událostí. Skupiny příjemců poskytují různým přijímajícím aplikacím oddělená zobrazení datového proudu událostí a umožňují jim nezávisle číst datový proud vlastním tempem a s použitím vlastních posunů. V architektuře zpracování datového proudu se každá aplikace pro příjem dat rovná skupině příjemců. Pokud chcete zapisovat data událostí do dlouhodobého úložiště, pak je aplikace, která do úložiště zapisuje, skupinou příjemců. Komplexní zpracování událostí zase provádí jiná, oddělená skupina příjemců. K oddílům můžete přistupovat pouze prostřednictvím skupiny příjemců. V centru událostí je vždy jedna výchozí skupina příjemců. Na standardní úrovni centra událostí můžete vytvořit až dvacet skupin příjemců.

Následují příklady konvenčního zápisu identifikátoru URI skupiny příjemců:

    //<my namespace>.servicebus.windows.net/<event hub name>/<Consumer Group #1>
    //<my namespace>.servicebus.windows.net/<event hub name>/<Consumer Group #2>

Následující obrázek znázorňuje příjemce událostí ve skupinách příjemců.

![Event Hubs](./media/event-hubs-overview/IC759860.png)

#### <a name="stream-offsets"></a>Posuny datového proudu

Posun je pozice události v rámci oddílu. Posun si můžete představit jako kurzor na straně klienta. Posun je číslo bajtu události. To umožňuje příjemci události (čtenáři) určit bod v datovém proudu událostí, od kterého chce události začít číst. Posun můžete zadat v podobě časového razítka nebo hodnoty posunu. Příjemci si sami zodpovídají za uložení svých hodnot posunu mimo službu Event Hubs.

![Event Hubs](./media/event-hubs-overview/IC759861.png)

Každá událost v rámci oddílu zahrnuje posun. Tento posun používají příjemci k zobrazení umístění v posloupnosti událostí v daném oddílu. Posuny se předávají do centra událostí, když se čtenář připojí, a to buď v podobě čísla, nebo hodnoty časového razítka.

#### <a name="checkpointing"></a>Vytváření kontrolních bodů

*Vytváření kontrolních bodů* je proces, pomocí kterého čtenáři označují nebo potvrzují svou pozici v rámci posloupnosti událostí v oddílu. Za vytváření kontrolních bodů zodpovídá příjemce. Proces probíhá na bázi oddílů ve skupinách příjemců. To znamená, že si každý čtenář oddílu v každé skupině příjemců musí udržovat přehled o své aktuální pozici v datovém proudu událostí a může informovat službu, když bude považovat datový proud za dokončený. Pokud se čtenář z oddílu odpojí, začne při opětovném připojení číst od kontrolního bodu, který dříve zaslal poslední čtenář daného oddílu z této skupiny příjemců. Když se čtenář připojí, předá tento posun do centra událostí, a určí tak umístění, od kterého začne číst. Takto můžete vytváření kontrolních bodů použít jak k označování událostí jako „dokončených“, tak k zajištění ochrany pro případ, že nastane selhání u čtenářů spuštěných na různých strojích. Vzhledem k tomu, že se data událostí uchovávají po dobu určenou při vytváření centra událostí, je možné vrátit se ke starším datům tak, že určíte nižší posun od kontrolního bodu. Díky tomuto mechanismu umožňuje vytváření kontrolních bodů nejen obnovu při selhání, ale i opakované kontrolované přehrání datového proudu.

#### <a name="common-consumer-tasks"></a>Běžné úlohy příjemce

Tato část popisuje běžné úlohy, které provádějí příjemci nebo čtenáři ve službě Event Hubs. Všichni příjemci se ve službě Event Hubs připojují prostřednictvím protokolu AMQP 1.0. AMQP 1.0 je obousměrný komunikační kanál, který rozpoznává relaci a stav. Každý oddíl má relaci připojení AMQP 1.0, která usnadňuje transport událostí oddělených oddílem.

##### <a name="connect-to-a-partition"></a>Připojení k oddílu

Aby bylo možné přijímat události z centra událostí, musí se příjemce připojit k oddílu. Jak již bylo zmíněno dříve, k oddílům vždy přistupujete prostřednictvím skupiny příjemců. Součástí modelu oddělených příjemců je pravidlo, že v jedné chvíli by měl být v oddílu aktivní pouze jediný čtenář z dané skupiny příjemců. Obvykle se při přímém připojení k oddílům používá mechanismus „pronájmu“, aby se připojení čtenářů ke konkrétním oddílům koordinovala. Díky tomu je možné mít u každého oddílu ve skupině příjemců pouze jednoho aktivního čtenáře. Správa pozice čtenáře v posloupnosti je důležitá úloha, která se řeší pomocí vytváření kontrolních bodů. Tuto funkcionalitu zjednodušuje použití třídy [EventProcessorHost](https://msdn.microsoft.com/library/microsoft.servicebus.messaging.eventprocessorhost.aspx) pro klienty .NET. [EventProcessorHost](https://msdn.microsoft.com/library/microsoft.servicebus.messaging.eventprocessorhost.aspx) je inteligentní agent příjemce, který je popsán v následující části.

##### <a name="read-events"></a>Čtení událostí

Po otevření připojení a relace AMQP 1.0 u konkrétního oddílu služba Event Hubs doručí události do klienta protokolu AMQP 1.0. Tento mechanismus doručení umožňuje vyšší propustnost a nižší latenci než mechanismy založené na operaci Pull, jako je například metoda GET protokolu HTTP. Události se posílají klientovi a každá instance dat události obsahuje důležitá metadata, jako je posun a číslo posloupnosti, která se používají k zjednodušení vytváření kontrolních bodů v posloupnosti událostí.

![Event Hubs](./media/event-hubs-overview/IC759862.png)

Je na vás, aby tento posun spravoval způsobem, který umožňuje co nejlépe postupovat při zpracovávání datového proudu.

## <a name="capacity-and-security"></a>Kapacita a zabezpečení

Služba Event Hubs je vysoce škálovatelná paralelní architektura pro příjem příchozích dat z datového proudu. Při nastavování velikosti a škálování řešení založeného na službě Event Hubs je tedy potřeba zvážit několik klíčových aspektů. První z ovládacích prvků kapacity se nazývají *jednotky propustnosti*, které jsou popsány v následující části.

### <a name="throughput-units"></a>Jednotky propustnosti

Kapacita propustnosti je ve službě Event Hubs řízená prostřednictvím jednotek propustnosti. Jednotky propustnosti jsou předem zakoupené jednotky kapacity. Jedna jednotka propustnosti zahrnuje následující:

- Příchozí data: Až 1 MB za sekundu nebo 1000 událostí za sekundu.

- Odchozí data: Až 2 MB za sekundu.

Příjem příchozích dat je omezen množstvím poskytnuté kapacity, které závisí na počtu nakoupených jednotek propustnosti. Odesílání dat ve větším množství způsobí výjimku „quota exceeded“ (překročená kvóta). Toto množství může být 1 MB za sekundu nebo 1000 událostí za sekundu – záleží, co nastane dříve. Odchozí data nezpůsobují takové výjimky, ale jsou omezená na objem přenosu dat, který poskytují zakoupené jednotky propustnosti: 2 MB za sekundu za jednotku propustnosti. Pokud se vám objevují výjimky související s frekvencí publikování nebo v budoucnu očekáváte větší objem odchozích dat, zkontrolujte, kolik jednotek propustnosti jste zakoupili pro obor názvů, ve kterém bylo centrum událostí vytvořené. Pokud chcete získat další jednotky propustnosti, můžete upravit nastavení na stránce **Obory názvů** na kartě **Škálování** v [Portál Azure Classic][]. Toto nastavení taky můžete změnit pomocí rozhraní API služby Azure.

Oddíly jsou koncept organizace dat, zatímco jednotky propustnosti jsou čistě koncept kapacity. Jednotky propustnosti se kupují předem a účtují se po hodinách. Zakoupené jednotky propustnosti se účtují minimálně za jednu hodinu. Pro obor názvů služby Event Hubs můžete zakoupit až 20 jednotek propustnosti. Jeden účet Azure je omezený na 20 jednotek propustnosti. Tyto jednotky propustnosti jsou sdíleny ve všech centrech událostí v daném oboru názvů.

Jednotky propustnosti se zřizují podle toho, jak kapacita systému dovolí, a nemusejí být vždy dostupné k okamžitému zakoupení. Pokud potřebujete konkrétní kapacitu, měli byste si potřebné jednotky propustnosti zakoupit s předstihem. Pokud potřebujete více než 20 jednotek propustnosti, kontaktujte podporu služby Azure, která vám umožní zakoupit si další jednotky propustnosti na základě závazku v blocích po 20 jednotkách – až do první stovky jednotek propustnosti. Navíc si můžete taky zakoupit bloky, které obsahují sto jednotek propustnosti.

Doporučujeme, abyste pečlivě vyvážili jednotky propustnosti a oddíly. Jedině tak dosáhnete ve službě Event Hubs optimálního škálování. Škálování jednoho oddílu dovoluje maximálně jednu jednotku propustnosti. Počet jednotek propustnosti by měl být menší nebo roven počtu oddílů v centru událostí.

Podrobné informace o cenách naleznete v článku o [cenách služby Event Hubs](https://azure.microsoft.com/pricing/details/event-hubs/).

### <a name="publisher-policy"></a>Zásady zdroje

Služba Event Hubs umožňuje podrobnou kontrolu nad zdroji událostí prostřednictvím *zásad zdroje*. Zásady zdroje jsou sadou funkcí runtime, které byly navržené pro usnadnění kontroly nad velkým množstvím nezávislých zdrojů událostí. Zásady zdroje poskytují s použitím následujícího mechanismu každému zdroji vlastní identifikátor, který se používá při publikování událostí do centra událostí.

    //<my namespace>.servicebus.windows.net/<event hub name>/publishers/<my publisher name>

Názvy zdrojů není potřeba vytvářet dopředu, při publikování události ale musí odpovídat tokenu SAS, aby se zajistilo, že každý zdroj bude mít nezávislou identitu. Další informace o tokenu SAS naleznete v tématu o [ověřování u služby Service Bus pomocí sdíleného přístupového podpisu](../service-bus-messaging/service-bus-shared-access-signature-authentication.md). Při použití zásad zdroje se každému názvu zdroje nastaví hodnota **PartitionKey** (Klíč oddílu). Aby vše správně fungovalo, musí tyto hodnoty odpovídat.

## <a name="summary"></a>Souhrn

Azure Event Hubs poskytuje flexibilně škálovatelné služby zpracování telemetrie a událostí, které je možné použít pro běžné monitorování aplikací a pracovního postupu uživatelů v jakémkoli měřítku. Díky možnosti publikování a odebírání dat s nízkou latencí a v masivním měřítku slouží služba Event Hubs jako vstupní brána k velkým objemům dat. Díky seznamům odvolaných a identitě založené na zdroji jsou tyto možnosti rozšířené na běžné scénáře internetu věcí. Další informace o vývoji aplikací pro službu Event Hubs naleznete v článku [Průvodce programováním pro službu Event Hubs](event-hubs-programming-guide.md).

## <a name="next-steps"></a>Další kroky

Nyní, když jste se naučili o konceptech služby Event Hubs, se můžete přesunout k následujícím scénářům:

- úvodní [kurz služby Event Hubs]
- úplná [ukázková aplikace, která používá službu Event Hubs]

[Portál Azure Classic]: http://manage.windowsazure.com
[Kurz služby Event Hubs]: event-hubs-csharp-ephcs-getstarted.md
[ukázková aplikace, která používá službu Event Hubs]: https://code.msdn.microsoft.com/windowsazure/Service-Bus-Event-Hub-286fd097



<!--HONumber=Oct16_HO3-->


