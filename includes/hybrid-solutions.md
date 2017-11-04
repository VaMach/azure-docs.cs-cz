# <a name="azure-service-bus"></a>Azure Service Bus
Ať už aplikace nebo služba běží v cloudu nebo lokálně, často potřebuje komunikovat s jinými aplikacemi nebo službami. Pokud chcete zadat široce vhodný způsob, jak to udělat, Azure nabízí Service Bus. V tom to článku se podíváme na tuto technologii a na to, proč byste ji mohli chtít používat.

## <a name="service-bus-fundamentals"></a>Základy služby Service Bus
Různé situace potřebují různé styly komunikace. Někdy je pro aplikace nejlepším řešením zprávy odesílat a přijímat přes jednoduchou frontu. V jiných situacích běžná fronta nestačí a je lepší použít frontu s publikováním a odběrem. A v některých případech všechno, co je skutečně potřeba je připojení mezi aplikací &#151; fronty nejsou požadované. Service Bus poskytuje všechny tři možnosti, když necháte aplikací pracovat několika různými způsoby.

Service Bus je víceklientská cloudová služba – to znamená, že ji může sdílet několik uživatelů. Každý uživatel, jako třeba vývojář aplikací, vytvoří *obor názvů*, a pak definuje komunikační mechanizmy, které potřebuje v daném oboru názvů. [Obrázek 1](#Fig1) ukazuje, jak to vypadá.

<a name="Fig1"></a>![Diagram Azure Service Bus][svc-bus]

**Obrázek 1: Service Bus poskytuje víceklientské služby pro připojení aplikací přes cloud.**

V oboru názvů můžete použít jednu nebo víc instancí čtyř různých komunikačních mechanizmů, každý z nich spojuje aplikace jiným způsobem. Na výběr jsou:

* *Fronty*, které umožňují jednosměrnou komunikaci. Každá fronta slouží jako prostředník (někdy se tomu říká *zprostředkovatel*), který ukládá odeslané zprávy, dokud nedorazí k příjemci. Každou zprávu přijme jeden příjemce.
* *Témata*, která nabízejí jednosměrnou komunikaci pomocí *odběrů* – jedno téma může mít několik odběrů. Podobně jako u front je téma něco jako zprostředkovatel, ale každý odběr může volitelně používat filtr a přijímat jen zprávy, které odpovídají konkrétním kritériím.
* *Předávání*, které umožňuje jednosměrnou komunikaci. Na rozdíl od front a témat se při předávání neukládají žádné zprávy, takže se nejedná o zprostředkování. Naopak se zprávy jednoduše předají k cílové aplikaci.
* *Event Hubs* – centra událostí, která poskytují vstup pro události a telemetrii do cloudu v obrovském měřítku s nízkou latencí a vysokou spolehlivostí.

Když vytvoříte frontu, téma, předávání nebo Event Hub, musíte ho pojmenovat. Kombinace tohoto názvu a vašeho oboru názvů vytváří jedinečný identifikátor objektu. Aplikace můžou tento název poskytnout službě Service Bus, a pak pomocí této fronty, tématu, předávání nebo Event Hubu mezi sebou komunikovat. 

Pokud aplikace Windows chtějí tyto objekty používat, můžou využít WCF (Windows Communication Foundation). Pro fronty, témata a Event Hubs můžou aplikace Windows použít taky API pro přenos zpráv, které definuje služba Service Bus. Pro snadnější použití těchto objektů z aplikací pro jinou platformu než Windows uvolnil Microsoft sady SDK pro Javu, Node.js a další jazyky. Přístup k frontám, tématům a Event Hubs se může získat i pomocí REST API přes HTTP. 

Je důležité pochopit, že i když služba Service Bus samotná běží v cloudu (to znamená v datových centrech Microsoftu pro Azure), aplikace, které ji využívají, můžou běžet kdekoli. Service Bus můžete použít třeba k připojení aplikací běžících v Azure nebo aplikací běžících ve vašem vlastním datovém centru. Můžete ji použít i k připojení aplikace běžící v Azure nebo jiné cloudové službě k lokální službě nebo k mobilním zařízením,jako jsou tablety a telefony. Dokonce s ní můžete připojit domácí spotřebiče, senzory a jiná zařízení k centrální aplikaci nebo k jiným zařízením. Service Bus je obecný komunikační mechanizmus v cloudu, který je přístupný prakticky odkudkoli. To, jakým způsobem ho budete využívat, záleží jen na tom, co vaše aplikace potřebují dělat.

## <a name="queues"></a>Fronty
Řekněme, že jste se rozhodli spojit dvě aplikace pomocí fronty Service Bus. [Obrázek 2](#Fig2) je taková situace.

<a name="Fig2"></a>![Diagram fronty služby Service Bus][queues]

**Obrázek 2: Fronty Service Bus poskytují jednosměrné asynchronní řízení front zpráv.**

Tento proces je prostý: Odesílatel odešle zprávu do fronty Service Bus a příjemce si ji z fronty vyzvedne později. Fronta může mít jen jednoho příjemce, jako [na obrázku 2](#Fig2) ukazuje nebo více aplikací může číst ze stejné fronty. V této druhé situaci každou zprávu přečte jen jeden příjemce – pokud chcete zprávu zpřístupnit víc příjemcům, je lepší místo fronty použít téma.

každá zpráva má dvě části: skupinu vlastností ve formě dvojic klíčů+hodnot a tělo zprávy v binární podobě. Jejich použití závisí na tom, co se vaše aplikace snaží udělat. Například jedna aplikace odešle zprávu o nedávném prodeji, která může obsahovat třeba tyto údaje: *Seller="Ava"* a *Amount=10000*. Tělo zprávy může obsahovat naskenovaný snímek podepsané smlouvy o prodeji nebo nemusí obsahovat nic a může zůstat prázdné.

Příjemce může zprávu načíst z fronty Service Bus dvěma různými způsoby. První možnost se jmenuje *ReceiveAndDelete* (přijmout a odstranit) – odebere zprávu z fronty a okamžitě ji odstraní. Tento způsob je velice jednoduchý, ale pokud příjemce spadne, než se mu podaří zprávu zpracovat, zprávu ztratí. A protože se už odstranila z fronty, nemůže ji získat ani žádný jiný příjemce. 

Druhá možnost se jmenuje *PeekLock* (uzamknout pro zpracování) a jejím smyslem je vyřešit práv tento problém. Čtení PeekLock jako ReceiveAndDelete, odebere zprávu z fronty. Jenže se při tom neodstraní úplně. Místo toho se zpráva jen uzamkne, aby nebyla vidět pro ostatní příjemce, a pak čeká na jednu ze tří událostí:

* Pokud příjemce zprávu úspěšně zpracuje, zavolá zpátky *Complete* (hotovo) a fronta zprávu odstraní. 
* Pokud se příjemce rozhodne, že nedokáže zprávy úspěšně zpracovat, zavolá *Abandon* (opustit). Fronta pak zruší zámek zprávy a ta je pak znovu dostupná pro ostatní příjemce.
* Pokud příjemce v nastaveném časovém limitu nepošle ani jedno z těchto volání (ve výchozím nastavení je to 60 sekund), fronta předpokládá, že příjemce selhal. V takovém případě se chová jako kdyby příjemce zavolal Abandon, zpřístupní zprávu pro ostatní příjemce.

Všimněte si, co se tu může stát: Stejná zpráva se může dodat dvakrát, třeba i dvěma různým příjemcům. Aplikace, které používají fronty Service Bus, na tuto možnost musí být připravené. Pro usnadnění detekce duplicitních, že každá zpráva má jedinečný MessageID vlastnost, která ve výchozím nastavení zůstává stejný bez ohledu na to, jak tolikrát, kolikrát je pro čtení zprávy z fronty. 

Fronty jsou užitečné v mnoha situacích. Umožňují aplikacím komunikovat, i když nejsou spuštěné ve stejnou dobu – to se hodí především pro dávkové a mobilní aplikace. Fronta s několika příjemci taky poskytuje automatické vyvažování zátěže, protože odeslané zprávy se rozdělují mezi jednotlivé příjemce.

## <a name="topics"></a>Témata
Přestože jsou fronty velice užitečné, nemusí se vždy jednat o to nejlepší řešení. Někdy je lepší použít témata Service Bus. [Obrázek 3](#Fig3) je taková situace.

<a name="Fig3"></a>![Diagram témata služby Service Bus a odběrů][topics-subs]

**Obrázek 3: V závislosti na použitém filtru může odběratelská aplikace přijímat všechny nebo jen některé zprávy odeslané do tématu Service Bus.**

Téma se ve spoustě ohledů podobá frontě. Odesílatelé odesílají zprávy do tématu stejným způsobem jako do fronty a zprávy v tématu vypadají stejně jako zprávy ve frontě. Velký rozdíl je, že témata každá aplikace přijímající vytvořila vlastní odběr definováním *filtru*. Odběratel pak uvidí jen zprávy, které odpovídají použitému filtru. Například [obrázek 3](#Fig3) zobrazuje odesílatele a téma se třemi odběrateli a každý svůj vlastní filtr:

* Odběratel 1 přijímá jen zprávy, které mají určitou vlastnost *Seller="Ava"*.
* Odběratel 2 přijímá zprávy, které mají vlastnost *Seller="Ruby"* a/nebo mají vlastnost *Amount* s hodnotou vyšší než 100 000. Možná je Ruby manažerka prodeje a takže chce vidět svoje prodeje a všechny velké prodeje bez ohledu na to, čí jsou.
* Odběratel 3 má nastavený filtr *True* – to znamená, že přijímá všechny zprávy. Tato aplikace může mít například na starost udržování auditní stopy a proto potřebuje vidět všechny zprávy.

Jako v případě front můžou Odběratelé tématu načítat zprávy způsobem ReceiveAndDelete a PeekLock. Na rozdíl od front se ale jedna zpráva odeslaná do tématu může dostat k víc odběratelům. Tento přístup, označovaného jako *publikování a odběr*, je užitečné v případě, že více aplikací může být zájem o stejné zprávy. Pokud odběratel definuje vhodný filtr, může si z proudu zpráv vytáhnout jen ty, které potřebuje.

## <a name="relays"></a>Předávání
Fronty i témata nabízejí jednosměrnou asynchronní komunikaci přes zprostředkovatele. Zprávy proudí jen jedním směrem a mezi odesílateli a příjemci není žádné přímé spojení. Co když to ale není to, co chcete? Řekněme, že aplikace potřebují odesílat i přijímat zprávy nebo že mezi nimi třeba chcete vytvořit přímé spojení a nepotřebujete zprostředkovatele pro ukládání zpráv. Pro takovou situaci Service Bus nabízí možnost předávání, jako [obrázek 4](#Fig4) zobrazuje.

<a name="Fig4"></a>![Diagram předávání přes Service Bus][relay]

**Obrázek 4: Předávání přes Service Bus nabízí synchronní obousměrnou komunikaci mezi aplikacemi.**

Asi vás napadne otázka: K čemu by se mi tohle hodilo? I když nepotřebuju fronty, proč mám nutit aplikace komunikovat přes cloudovou službu, když chci, aby komunikovaly přímo? Odpověď je taková, že přímá komunikace je někdo mnohem obtížnější, než by se mohlo zdát.

Řekněme, že chcete propojit dvě lokální aplikace, které obě běží ve firemních datových centrech. Každá z těchto aplikací je za firewallem a každé datové centrum pravděpodobně používá překládání adres (NAT). Firewall povoluje data jen na několika málo portech a ostatní porty blokuje, zatímco NAT naznačuje, že ani jeden z počítačů, na kterých aplikace běží, nemá pevnou IP adresu, takže z jiné zóny, než je zóna jejího datového centra, se k ní nedá dostat přímo. Bez pomoci specializovaných nástrojů půjdou tyto aplikace propojit přes internet jen těžko.

A Service Bus je právě takový specializovaný nástroj. Aby aplikace mohly komunikovat obousměrně s předáváním, každá z nich vytvoří odchozí připojení přes TCP se službou Service Bus a udržuje ho otevřené. Veškerá komunikace mezi těmito dvěma aplikacemi bude procházet přes tato spojení. Protože se obě spojení vytvořila ze zóny datového centra, firewall povolí veškerou příchozí komunikaci pro danou aplikaci bez nutnosti otevřít nové porty. Tento přístup také obchází problém s NAT, protože každá z obou aplikací má v cloudu vlastní pevný koncový bod. Výměnou dat přes předávací službu se aplikace můžou vyhnout problémům, které by jinak takovou komunikaci výrazně ztěžovaly. 

Pokud chcete používat předávací služby Service Bus, aplikací závisí na Windows Communication Foundation (WCF). Service Bus poskytuje vazby WCF, které aplikacím Windows ulehčují komunikaci přes předávací službu. Pro aplikace, které už WCF používají, obvykle stačí specifikovat jednu z těchto vazeb a potom můžou komunikovat přes předávací službu. Na rozdíl od front a témat ale komunikace přes předávací službu z aplikací na jiné platformě než Windows vyžaduje určitou míru programátorského úsilí, protože nejsou k dispozici žádné standardizované knihovny.

A na rozdíl od front a témat taky aplikace nevytvářejí explicitně předávací službu. Místo toho se stane, že když aplikace chce dostávat zprávy, vytvoří spojení se službou Service Bus přes TCP a předávací služba se vytvoří automaticky. Při ukončení spojení se předávací služba odstraní. Aby mohli aplikaci najít předávací službu vytvořenou konkrétním posluchačem, Service Bus poskytuje registru, která umožňuje aplikacím najít konkrétní předávací službu podle názvu.

Předávací služby jsou správným řešením v situaci, když potřebujete přímou komunikaci mezi aplikacemi. Řekněme třeba, že systém pro rezervaci letenek běží na lokálním datovém centru, ke kterému musí mít přístup prodejní místa, mobilní zařízení a další počítače. Aplikace, které běží na všech těchto systémech, by mohly ke komunikaci použít předávací služby Service Bus v cloudu, a bylo by jedno, kde běží.

## <a name="event-hubs"></a>Event Hubs
Event Hubs je vysoce škálovatelná služba, která dokáže zpracovat miliony událostí za sekundu a umožňuje vaší aplikaci zpracovávat a analyzovat masivní objemy dat vytvářených zařízeními a aplikacemi připojenými k vaší síti. Službu Event Hubs můžete použít třeba ke sběru výkonnostních ukazatelů motorů vozidel firemního vozového parku v reálném čase. Když Event Hubs shromáždí data, můžete je zpracovat a uložit pomocí libovolného úložného clusteru nebo poskytovatele datové analýzy v reálném čase. Další informace o službě Event Hubs najdete v tématu [Přehled služby Event Hubs][Event Hubs overview].

## <a name="summary"></a>Souhrn
Propojení aplikací vždy patřilo k budování kompletních řešení a množství situací, ve kterých spolu aplikace a služby musí komunikovat, se s rostoucím počtem zařízení připojených k internetu bude dál zvyšovat. Service Bus používá fronty, témata, předávání a služby Event Hubs k tomu, aby tuto základní funkci aplikacím a službám umožnil, usnadnil a více zpřístupnil.

[svc-bus]: ./media/hybrid-solutions/SvcBus_01_architecture.png
[queues]: ./media/hybrid-solutions/SvcBus_02_queues.png
[topics-subs]: ./media/hybrid-solutions/SvcBus_03_topicsandsubscriptions.png
[relay]: ./media/hybrid-solutions/SvcBus_04_relay.png
[Event Hubs overview]: https://msdn.microsoft.com/library/azure/dn836025.aspx
