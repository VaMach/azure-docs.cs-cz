<properties
    pageTitle=" Začínáme s doručováním obsahu na vyžádání pomocí portálu Azure Classic | Microsoft Azure"
    description="V tomto kurzu vás provede jednotlivými kroky implementace aplikace pro doručování obsahu videa na vyžádání (VoD, Video-on-Demand) pomocí služby Azure Media Services a portálu Azure Classic."
    services="media-services"
    documentationCenter=""
    authors="Juliako"
    manager="erikre"
    editor=""/>

<tags
    ms.service="media-services"
    ms.workload="media"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="06/22/2016"
    ms.author="juliako"/>


# Začínáme s doručováním obsahu na vyžádání pomocí portálu Azure Classic


[AZURE.INCLUDE [media-services-selector-get-started](../../includes/media-services-selector-get-started.md)]


V tomto kurzu vás provede jednotlivými kroky implementace základní aplikace pro doručování obsahu videa na vyžádání pomocí portálu Azure Classic.

> [AZURE.NOTE] K dokončení tohoto kurzu potřebujete mít účet Azure. Podrobnosti najdete v článku [Bezplatná zkušební verze Azure](/pricing/free-trial/?WT.mc_id=A261C142F). 


Tento kurz sestává z následujících úloh:

1.  Vytvoření účtu Azure Media Services
2.  Konfigurace koncového bodu streamování
1.  Nahrání videosouboru
1.  Zakódování zdrojového souboru do sady souborů MP4 s adaptivní přenosovou rychlostí
1.  Publikování assetu a získání adres URL streamování a progresivního stahování  
1.  Přehrání obsahu


## Vytvoření účtu Azure Media Services

1. Na stránce [Portál Azure Classic](https://manage.windowsazure.com/) klikněte na **Nový**, **Media Service** a potom na **Rychle vytvořit**.

    ![Rychlé vytvoření Media Services](./media/media-services-portal-get-started/wams-QuickCreate.png)

2. Do pole **NÁZEV** zadejte název nového účtu. Název účtu Media Services musí obsahovat jenom malá písmena a čísla, nesmí obsahovat mezery a musí mít délku 3 až 24 znaků.

3. V poli **OBLAST** vyberte zeměpisnou oblast, která se bude používat k ukládání záznamů metadat pro váš účet Media Services. V rozevíracím seznamu se vám zobrazí pouze ty oblasti Media Services, které jsou dostupné.

4. V poli **ÚČET ÚLOŽIŠTĚ** vyberte účet úložiště, který bude sloužit jako úložiště objektů blob mediálního obsahu z vašeho účtu Media Services. Můžete vybrat existující účet úložiště ve stejné zeměpisné oblasti jako váš účet Media Services, nebo můžete vytvořit nový účet úložiště. Nový účet úložiště bude vytvořen ve stejné oblasti.

5. Pokud jste vytvořili nový účet úložiště, do pole **NÁZEV NOVÉHO ÚČTU ÚLOŽIŠTĚ** zadejte název pro účet úložiště. Pro názvy účtů úložiště platí stejná pravidla jako pro názvy účtů Media Services.

6. V dolní části formuláře klikněte na tlačítko **Rychle vytvořit**.

    V oblasti zpráv v dolní části okna můžete sledovat stav procesu.

    Po úspěšném vytvoření účtu se stav změní na Aktivní.

    V dolní části stránky se zobrazí tlačítko **SPRÁVA KLÍČŮ**. Po kliknutí na toto tlačítko se zobrazí se dialogové okno s názvem účtu Media Services a primárním a sekundárním klíčem. Název účtu a primární klíč budete potřebovat pro přístup k účtu Media Services prostřednictvím programového kódu.

    ![Stránka Media Services](./media/media-services-portal-get-started/wams-mediaservices-page.png)

    Když dvakrát kliknete na název účtu, zobrazí se ve výchozím nastavení stránka Rychlý start. Tato stránka umožňuje provést některé úlohy správy, které jsou jinak dostupné i na jiných stránkách portálu. Z této stránky můžete například nahrát videosoubor, což lze provést i ze stránky obsahu.


## Konfigurace koncového bodu streamování pomocí portálu

Při práci se službou Azure Media Services je jedním nejběžnější scénářů doručování streamování s adaptivní přenosovou rychlostí vašim klientům. Při adaptivní přenosové rychlosti streamování může klient při přehrávání videa přepínat na datový proud s vyšší nebo nižší přenosovou rychlostí podle aktuálně dostupné šířky pásma sítě, využití procesoru a dalších faktorů. Služba Media Services podporuje následující technologie adaptivní přenosové rychlosti streamování: HTTP Live Streaming (HLS), technologie Smooth Streaming, MPEG DASH a HDS (pouze pro držitele licence Adobe PrimeTime/Access).

Služba Media Services poskytuje dynamické balení, což vám umožní dodávat váš obsah s adaptivní přenosovou rychlostí s kódováním MP4 nebo technologie Smooth Streaming ve formátech streamování podporovaných službou Media Services (MPEG DASH, HLS, technologie Smooth Streaming a HDS) bez nutnosti znovu obsah balit do těchto formátů streamování.

Pokud chcete využít výhod dynamického balení, proveďte následující:

- Zakódovat váš soubor mezzanine (zdrojový soubor) do sady souborů MP4 nebo technologie Smooth Streaming s adaptivní přenosovou rychlostí (postup kódování je ukázán později v tomto kurzu).  
- Získat alespoň jednu jednotku streamování pro *koncový bod streamování*, ze kterého plánujete obsah doručovat.

Při dynamickém balení stačí uložit (a platit) soubory pouze v jednom úložném formátu a služba Media Services bude sestavovat a dodávat vhodný formát streamování v reakci na požadavky klientů.

Pokud chcete změnit počet jednotek rezervovaných pro streaming, postupujte takto:

1. Na stránce [Portál Azure Classic](https://manage.windowsazure.com/) klikněte na **Media Services**. Potom klikněte na název mediální služby.

2. Vyberte stránku KONCOVÉ BODY STREAMOVÁNÍ. Potom klikněte na koncový bod streamování, který chcete upravit.

3. Pokud chcete zadat počet jednotek streamování, vyberte kartu **ŠKÁLOVAT** a posuňte posuvníkem **rezervované kapacity**.

    ![Stránka Škálovat](./media/media-services-portal-get-started/media-services-origin-scale.png)

4. Kliknutím na tlačítko **Uložit** uložte provedené změny.

    Dokončení přidělení jakýchkoli nových jednotek trvá přibližně 20 minut.

    >[AZURE.NOTE] Změna z jakéhokoli kladného počtu jednotek streamování zpět na nulu může v současnosti zcela vypnout streamování až na jednu hodinu.
    >
    > Pro výpočet ceny se používá nejvyšší zadaný počet jednotek v průběhu období 24 hodin. Podrobné informace o cenách najdete v článku [Ceny služby Azure Media Services](http://go.microsoft.com/fwlink/?LinkId=275107).

## Nahrání obsahu


1. Na stránce [Portál Azure Classic](http://go.microsoft.com/fwlink/?LinkID=256666&clcid=0x409) klikněte na **Media Services** a potom klikněte na název účtu Media Services.
2. Vyberte stránku obsahu.
3. Klikněte na tlačítko **Nahrát** na stránce nebo v dolní části portálu.
4. V dialogu okně **Nahrát obsah** přejděte k souboru požadovaného assetu. Klikněte na soubor a potom klikněte na tlačítko **Otevřít** nebo stiskněte klávesu Enter.

    ![UploadContentDialog][uploadcontent]

5. V dialogovém okně **Nahrát obsah** kliknutím na potvrzující tlačítko (ikona zaškrtnutí) odsouhlaste **Soubor** a **Název obsahu**.
6. Spustí se nahrávání. Jeho průběh můžete sledovat v dolní části portálu.  

    ![JobStatus][status]

Po dokončení nahrávání se nový asset zobrazí v seznamu obsahu. Podle konvence pojmenování bude k jejímu názvu připojeno „**-Source**, abyste mohli snadněji sledovat nový obsah, který slouží jako zdrojový obsah pro úlohy kódování.

![ContentPage][contentpage]

Pokud se hodnota velikosti souboru po zastavení procesu nahrávání neaktualizuje, vyberte tlačítko **Synchronizovat metadata**. Tím se synchronizuje velikost souboru assetu se skutečnou velikostí souboru v úložišti a aktualizuje se hodnota na stránce obsahu.


## Kódování obsahu

### Přehled

Aby bylo možné poskytovat digitální video přes internet, je nutné médium komprimovat. Služba Media Services poskytuje kodér médií, která umožňuje určit, jak má být váš obsah kódovaný (například jaké má používat kodeky, jaký má mít formát souboru, rozlišení, přenosovou rychlost atd.)

Při práci se službou Azure Media Services je jedním nejběžnější scénářů doručování streamování s adaptivní přenosovou rychlostí vašim klientům. Při adaptivní přenosové rychlosti streamování může klient při přehrávání videa přepínat na datový proud s vyšší nebo nižší přenosovou rychlostí podle aktuálně dostupné šířky pásma sítě, využití procesoru a dalších faktorů. Služba Media Services podporuje následující technologie adaptivní přenosové rychlosti streamování: HTTP Live Streaming (HLS), technologie Smooth Streaming, MPEG DASH a HDS (pouze pro držitele licence Adobe PrimeTime/Access).

Služba Media Services poskytuje dynamické balení, což vám umožní dodávat váš obsah s adaptivní přenosovou rychlostí s kódováním MP4 nebo technologie Smooth Streaming ve formátech streamování podporovaných službou Media Services (MPEG DASH, HLS, technologie Smooth Streaming a HDS) bez nutnosti znovu obsah balit do těchto formátů streamování.

Pokud chcete využít výhod dynamického balení, proveďte následující:

- Zakódovat váš soubor mezzanine (zdrojový soubor) do sady souborů MP4 nebo technologie Smooth Streaming s adaptivní přenosovou rychlostí (postup kódování je ukázán později v tomto kurzu).
- Získat alespoň jednu jednotku streamingu na vyžádání pro koncový bod streamování, ze kterého plánujete obsah doručovat. Další informace najdete v článku o [škálování jednotek rezervovaných pro streaming na vyžádání](media-services-manage-origins.md#scale_streaming_endpoints/).

Při dynamickém balení stačí uložit (a platit) soubory pouze v jednom úložném formátu a služba Media Services bude sestavovat a dodávat vhodný formát streamování v reakci na požadavky klientů.

Všimněte si, že vedle možnosti použít dynamické balení vám jednotky rezervované pro streaming na vyžádání přináší i rezervovanou kapacitu výchozího přenosu, kterou lze dokupovat v jednotkách po 200 Mb/s. Ve výchozím nastavení je streamování na vyžádání nakonfigurováno s modelem sdílené instance, kdy jsou serverové prostředky (například výpočetní výkon nebo kapacita výchozího přenosu) sdíleny se všemi ostatními uživateli. Pokud chcete zlepšit propustnost streamování na vyžádání, doporučujeme vám zakoupit jednotky rezervované pro streamování na vyžádání.

### Kódování

Tato část popisuje kroky, jak můžete zakódovat svůj obsah pomocí procesoru Media Encoder Standard na portálu Azure Classic.

1.  Vyberte soubor, který chcete kódovat.
    Pokud je podporováno kódování pro tento typ souboru, ve spodní části stránky obsahu bude aktivní tlačítko **ZPRACOVAT**.
4. V dialogovém okně **Zpracovat** vyberte procesor **Media Encoder Standard**.
5. Vyberte jednu z **konfigurací kódování**.

    ![Process2][process2]

    Popis významu jednotlivých přednastavených konfigurací najdete v tématu o [přednastavení úloh pro Media Encoder Standard](https://msdn.microsoft.com/en-US/library/mt269960).  

5. Potom zadejte svůj popisný název výstupu obsahu, případně přijměte nabídnutý výchozí název. Klikněte na tlačítko s ikonou zaškrtnutí. Spustí se operace kódování. Její průběh můžete sledovat v dolní části portálu.
6. Vyberte **OK**.

    Po dokončení kódování bude stránka obsahu obsahovat kódovaný soubor.

    Chcete-li zobrazit průběh úlohy kódování, přepněte na stránku **ÚLOHY**.  

    Pokud se hodnota velikosti souboru po dokončení kódování neaktualizuje, vyberte tlačítko **Synchronizovat metadata**. Tím se synchronizuje velikost výstupního souboru assetu se skutečnou velikostí souboru v úložišti a aktualizuje se hodnota na stránce obsahu.


## Publikování obsahu

### Přehled

Pokud chcete uživateli poskytnout adresu URL, kterou lze použít ke streamování nebo stažení vašeho obsahu, musíte asset nejprve „publikovat“ vytvořením lokátoru. Lokátory zajišťují přístup k souborům, které jsou obsaženy v assetu. Služba Media Services podporuje dva typy lokátorů: lokátor OnDemandOrigin, používaný ke streamování médií (například MPEG DASH, HLS nebo technologie Smooth Streaming), a lokátor s přístupovým podpisem (SAS), používaný ke stahování mediálních souborů.

Když k publikování svého assetu použijete portál Azure Classic, vytvoří se pro vás lokátory a obdržíte adresu URL typu OnDemand (pokud váš asset obsahuje soubor .ism) nebo adresu URL typu SAS.

Adresa URL typu SAS má následující formát.

    {blob container name}/{asset name}/{file name}/{SAS signature}

Adresa URL streamování má následující formát a můžete ji použít k přehrávání mediálních assetů technologie Smooth Streaming.

    {streaming endpoint name-media services account name}.streaming.mediaservices.windows.net/{locator ID}/{filename}.ism/Manifest

Pokud chcete vytvořit adresu URL streamování HLS, připojte na konec adresy (format=m3u8-aapl).

    {streaming endpoint name-media services account name}.streaming.mediaservices.windows.net/{locator ID}/{filename}.ism/Manifest(format=m3u8-aapl)

Pokud chcete vytvořit adresu URL streamování MPEG DASH, připojte na konec adresy (format=mpd-time-csf).

    {streaming endpoint name-media services account name}.streaming.mediaservices.windows.net/{locator ID}/{filename}.ism/Manifest(format=mpd-time-csf)


Lokátory mají datum vypršení platnosti. Při použití portálu k publikování vašich assetů se vytváří lokátory s platností 100 let.

>[AZURE.NOTE] Pokud jste použili portál k vytvoření lokátorů před březnem 2015, byly vytvořeny lokátory s platností dva roky.  

Pokud chcete aktualizovat datum vypršení platnosti lokátoru, použijte rozhraní [REST](http://msdn.microsoft.com/library/azure/hh974308.aspx#update_a_locator ) API nebo [.NET](http://go.microsoft.com/fwlink/?LinkID=533259). Upozorňujeme, že při aktualizaci data vypršení platnosti lokátoru SAS se změní adresa URL.

### Publikování

Postup publikování assetu pomocí portálu:

1. Vyberte asset.
2. Potom klikněte na tlačítko pro publikování.

 ![PublishedContent][publishedcontent]


## Přehrávání obsahu z portálu

Portál Azure Classic nabízí přehrávač obsahu, který můžete použít k testování videa.

Klikněte na požadované video a potom klikněte na tlačítko **Přehrát** v dolní části portálu.

Musí být splněny určité předpoklady:

- Zkontrolujte, že bylo video publikováno.
- Přehrávač **MEDIA SERVICES CONTENT PLAYER** přehrává z výchozího koncového bodu streamování. Pokud chcete přehrávat z jiného než výchozích koncového bodu streamování, použijte jiný přehrávač. Například můžete použít [Azure Media Services Player](http://amsplayer.azurewebsites.net/azuremediaplayer.html).


![AMSPlayer][AMSPlayer]



##Další kroky: Mapy kurzů ke službě Media Services

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##Poskytnutí zpětné vazby

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

## Hledáte něco jiného?

Pokud toto téma neobsahovalo, co jste očekávali, něco mu chybí nebo nějakým způsobem nesplnilo vaše očekávání, pošlete nám svoje připomínky prostřednictvím služby Disqus níže.

### Další zdroje
- <a href="http://channel9.msdn.com/Shows/Azure-Friday/Azure-Media-Services-101-Get-your-video-online-now-">Azure Media Services 101 – Zpřístupněte svá videa online</a>
- <a href="http://channel9.msdn.com/Shows/Azure-Friday/Azure-Media-Services-102-Dynamic-Packaging-and-Mobile-Devices">Azure Media Services 102 – Dynamické balení a mobilní zařízení</a>


<!-- Anchors. -->


<!-- URLs. -->
[Portál Azure Classic]: http://manage.windowsazure.com/


<!-- Images -->
[portaloverview]: ./media/media-services-portal-get-started/media-services-content-page.png
[publishedcontent]: ./media/media-services-portal-get-started/media-services-upload-content-published.png
[uploadcontent]: ./media/media-services-portal-get-started/UploadContent.png
[status]: ./media/media-services-portal-get-started/Status.png
[encoder]: ./media/media-services-manage-content/EncoderDialog2.png
[branding]: ./media/branding-reporting.png
[contentpage]: ./media/media-services-portal-get-started/media-services-content-page.png
[process]: ./media/media-services-manage-content/media-services-process-video.png
[process2]: ./media/media-services-portal-get-started/media-services-process-video2.png
[encrypt]: ./media/media-services-manage-content/media-services-encrypt-content.png
[AMSPlayer]: ./media/media-services-portal-get-started/media-services-portal-player.png



<!---HONumber=Aug16_HO4-->


