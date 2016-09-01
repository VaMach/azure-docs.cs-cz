<properties
    pageTitle="Analýza trendů v sadě Visual Studio | Microsoft Azure"
    description="Analyzujte, vizualizujte a zkoumejte trendy v telemetrii služby Application Insights v sadě Visual Studio."
    services="application-insights"
    documentationCenter=".net"
    authors="numberbycolors"
    manager="douge"/>

<tags
    ms.service="application-insights"
    ms.workload="tbd"
    ms.tgt_pltfrm="ibiza"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="07/14/2016"
    ms.author="daviste"/>

# Analýza trendů v sadě Visual Studio

Nástroj Trendy Application Insights vizualizuje průběžné změny důležitých telemetrických událostí v aplikaci. Díky tomu můžete rychle identifikovat problémy a anomálie. Nástroj Trendy vám dodá podrobnější diagnostické informace, abyste mohli zlepšit výkon aplikace, sledovat příčiny výjimek a získat přehledy z vlastních událostí.

![Příklad okna nástroje Trendy](./media/app-insights-trends/app-insights-trends-hero-750.png)

> [AZURE.NOTE] Nástroj Trendy Application Insights je dostupný v sadě Visual Studio 2015 s aktualizací Update 3 a vyšší nebo prostřednictvím [rozšíření Developer Analytics Tools](https://visualstudiogallery.msdn.microsoft.com/82367b81-3f97-4de1-bbf1-eaf52ddc635a) verze 5.209 a novější.

## Otevření nástroje Trendy Application Insights

Okno nástroje Trendy Application Insights můžete otevřít následujícími způsoby:

* Na panelu nástrojů Application Insights zvolte **Prozkoumat trendy telemetrie**.
* V místní nabídce projektu zvolte **Application Insights > Prozkoumat trendy telemetrie**.
* V řádku nabídek sady Visual Studio zvolte **Zobrazení > Ostatní okna > Trendy Application Insights**.

Může se zobrazit výzva k výběru prostředku. Pokud se zobrazí, klikněte na **Vybrat prostředek**, přihlaste se pomocí předplatného Azure a potom klikněte na prostředek Application Insights, u kterého chcete analyzovat trendy telemetrie.

## Výběr analýzy trendů

![Nabídka běžných typů analýz trendů](./media/app-insights-trends/app-insights-trends-1-750.png)

Začněte výběrem jedné z pěti běžných analýz trendů, kde každá analyzuje data z posledních 24 hodin:

* **Prozkoumat problémy s výkonem požadavků serveru**: Požadavky na vaší službu seskupené podle doby odezvy
* **Analyzovat chyby v požadavcích serveru**: Požadavky na vaší službu seskupené podle kódu odpovědi HTTP
* **Prozkoumat výjimky v aplikaci**: Výjimky služby seskupené podle typu výjimky
* **Zkontrolovat výkon závislostí aplikace**: Služby volané vaší službou seskupené podle doby odezvy
* **Zkontrolovat vlastní události**: Vlastní události, které jste pro svoji službu nastavili, seskupené podle typu události

Tyto předdefinované analýzy jsou dostupné i později prostřednictvím tlačítka **Zobrazit běžné typy analýzy telemetrie**, které se nachází v levém horním rohu okna Trendy.

## Vizualizace trendů v aplikaci

Nástroj Trendy Application Insights pracuje s telemetrií vaší aplikace a vizualizuje z ní časové řady. Každá vizualizace časové řady zobrazuje jeden typ telemetrie (seskupený podle jedné vlastnosti takové telemetrie) za konkrétní časové období.

Můžete například zobrazit požadavky serveru za posledních 24 hodin seskupené podle země původu. V tomto příkladu každá bublina na vizualizaci představuje počet požadavků serveru pro určitou zemi během jedné hodiny.

Pomocí ovládacích prvků v horní části okna nastavte typy telemetrie, které chcete zobrazit. Nejdřív vyberte typy telemetrie, které vás zajímají:

* **Typ telemetrie**: Požadavky serveru, výjimky, závislosti nebo vlastní události
* **Časový rozsah**: Jakýkoli od posledních 30 minut po poslední 3 dny
* **Seskupit podle**: Typ výjimky, ID problému, země/oblast a další

Potom klikněte na **Analyzovat telemetrii** a spusťte dotaz.

Pokud chcete přecházet mezi bublinami ve vizualizaci, postupujte následovně:

* Kliknutím vyberte bublinu, která aktualizuje filtry v dolní části okna a shrnuje události, které nastaly během konkrétního časového období.
* Poklikáním na bublinu přejděte do nástroje hledání a zobrazte všechny jednotlivé telemetrické události, které během tohoto časového období nastaly.
* Pokud chcete zrušit výběr bubliny ve vizualizaci, stiskněte klávesu **Ctrl** a potom na bublinu klikněte.

> [AZURE.TIP] Nástroje Trendy a nástroj hledání usnadňují identifikaci telemetrických události, které ve vaší službě způsobují problémy. Pokud si vaši zákazníci například všimnou, že odezva aplikace je jedno odpoledne pomalejší, můžete spustit proces analýzy problému v nástroji Trendy. Analyzujte požadavky na vaši službu za posledních několik hodin, seskupené podle doby odezvy. Zjistěte, jestli se neobjevil neobvykle velký cluster pomalých požadavků. Poklikáním na tuto bublinu přejdete do nástroje hledání, který se filtruje podle těchto událostí žádostí. Pomocí hledání můžete prozkoumat obsah těchto požadavků a najít problematický kód, abyste mohli problém vyřešit.

## Filtrování konkrétních trendů

Pomocí ovládacích prvků filtru v dolní části okna můžete zjistit konkrétnější trendy. Pokud chcete filtr použít, klikněte na jeho název. Mezi různými filtry můžete rychle přepínat a zjišťovat tak trendy, které se můžou skrývat v konkrétní dimenzi vaší telemetrie. Když filtr použijete v jedné dimenzi, například v dimenzi Typ výjimky, můžete na filtry v ostatních dimenzích stále klikat, i když jsou zobrazené šedě. Pokud chcete zrušit používání filtru, klikněte na něj znovu. Stiskněte klávesu **Ctrl** a potom klikněte na několik filtrů ve stejné dimenzi.

![Filtry trendů](./media/app-insights-trends/TrendsFiltering-750.png)

Jak postupovat, když chcete použít několik filtrů.

1. Použijte první filtr.
2. Klikněte na tlačítko **Použít vybrané filtry a znova poslat dotaz** vedle názvu dimenze prvního filtru. Tím bude znovu odeslán dotaz na telemetrii a bude se konkrétně týkat událostí, které odpovídají prvnímu filtru.
3. Použijte druhý filtr.
4. Proces opakujte tak dlouho, až trendy v konkrétní podmnožině telemetrie najdete. Můžete například odeslat dotaz na požadavky serveru s názvem „GET Home/Index“, které přišly z Německa a které získaly stavový kód 500.

Pokud chcete zrušit používání jednoho z těchto filtrů, klikněte u příslušné dimenze na tlačítko **Odstranit vybrané filtry a poslat dotaz znova**.

![Několik filtrů](./media/app-insights-trends/TrendsFiltering2-750.png)

## Nalezení anomálií

Nástroj Trendy může zvýraznit bubliny událostí, které jsou ve srovnání s jinými bublinami ve stejné časové řadě neobvyklé. V rozevírací nabídce **Typ zobrazení** vyberte **Počty v časovém intervalu (zvýraznit anomálie)** nebo **Procenta v časovém intervalu (zvýraznit anomálie)**. Červené bubliny označují anomálie.

Anomálie se definují jako bubliny s poměrem počty/procenta, který je vyšší než 2,1násobek směrodatné odchylky poměru počty/procenta, ke kterým došlo v posledních dvou časových obdobích (například 48 hodin, pokud zobrazujete posledních 24 hodin).

![Barevné tečky označují anomálie.](./media/app-insights-trends/TrendsAnomalies-750.png)

> [AZURE.TIP] Proces zvýraznění anomálií může být zvláště užitečný při hledání mimořádných hodnot v časových řadách malých bublin, u kterých by se jinak mohlo zdát, že mají podobnou velikost.  

## <a name="next"></a>Další kroky


[Práce s Application Insights v sadě Visual Studio](app-insights-visual-studio.md): Vyhledejte telemetrii, viz data ve funkci CodeLens, a nakonfigurujte nástroj Application Insights – vše v rámci sady Visual Studio.

[Přidat další data](app-insights-asp-net-more.md): Sledujte využití, dostupnost, závislosti, výjimky. Integrujte trasování z rozhraní protokolování. Zapisuje vlastní telemetrii.

[Práce s portálem Application Insights](app-insights-dashboards.md): Řídicí panely, výkonné nástroje pro diagnostiku a analýzy, výstrahy, aktivní mapa závislostí vaší aplikace a export telemetrie.



<!--HONumber=Aug16_HO4-->


