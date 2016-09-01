<properties
    pageTitle=" Začínáme s doručováním obsahu na vyžádání pomocí portálu Azure | Microsoft Azure"
    description="V tomto kurzu vás provede jednotlivými kroky implementace základní aplikace pro doručování obsahu videa na vyžádání (VoD, Video-on-Demand) pomocí služby Azure Media Services (AMS) a portálu Azure."
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


# Začínáme s doručováním obsahu na vyžádání pomocí portálu Azure Portal

V tomto kurzu vás provede jednotlivými kroky implementace základní aplikace pro doručování obsahu videa na vyžádání (VoD, Video-on-Demand) pomocí služby Azure Media Services (AMS) a portálu Azure.

Služba Azure Media Services na portálu Azure je aktuálně ve verzi preview. 

> [AZURE.NOTE] K dokončení tohoto kurzu potřebujete mít účet Azure. Podrobnosti najdete v článku [Bezplatná zkušební verze Azure](https://azure.microsoft.com/pricing/free-trial/). 

Tento kurz sestává z následujících úloh:

1.  Vytvoření účtu Azure Media Services
2.  Konfigurace koncového bodu streamování
1.  Nahrání videosouboru
1.  Zakódování zdrojového souboru do sady souborů MP4 s adaptivní přenosovou rychlostí
1.  Publikování assetu a získání adres URL streamování a progresivního stahování  
1.  Přehrání obsahu


## Vytvoření účtu Azure Media Services

Postup v této části ukazuje, jak vytvořit nový účet AMS.

1. Přihlaste se k [portálu Azure](https://portal.azure.com/).
2. Klikněte na **+Nový** > **Média a CDN** > **Media Services**.

    ![Media Services – vytvoření](./media/media-services-portal-vod-get-started/media-services-new1.png)

3. V okně **VYTVOŘIT ÚČET MEDIA SERVICES** zadejte požadované hodnoty.

    ![Media Services – vytvoření](./media/media-services-portal-vod-get-started/media-services-new3.png)
    
    1. Do pole **Název účtu** zadejte název nového účtu AMS. Název účtu Media Services musí obsahovat jenom malá písmena a čísla, nesmí obsahovat mezery a musí mít délku 3 až 24 znaků.
    2. V poli Předplatné si vyberte z různých předplatných Azure, ke kterým máte přístup.
    
    2. V poli **Skupina prostředků** vyberte nový nebo existující prostředek.  Skupina prostředků je kolekce prostředků, které sdílejí stejný životní cyklus, oprávnění a zásady. Další informace najdete [tady](resource-group-overview.md#resource-groups).
    3. V poli **Umístění** vyberte zeměpisnou oblast, která se bude používat k ukládání médií a záznamů metadat pro váš účet Media Services. Tato oblast se bude používat ke zpracování a streamování vašeho média. V rozevíracím seznamu se vám zobrazí pouze ty oblasti Media Services, které jsou dostupné. 
    
    3. V poli **Účet úložiště** vyberte účet úložiště, který bude sloužit jako úložiště objektů blob mediálního obsahu z vašeho účtu Media Services. Můžete vybrat existující účet úložiště ve stejné zeměpisné oblasti jako váš účet Media Services, nebo můžete vytvořit nový účet úložiště. Nový účet úložiště bude vytvořen ve stejné oblasti. Pro názvy účtů úložiště platí stejná pravidla jako pro názvy účtů Media Services.

        Další informace o ukládání a úložištích najdete [tady](storage-introduction.md).

    4. Zaškrtněte **Připnout na řídicí panel**, abyste viděli průběh nasazení účtu.
    
7. Klikněte na tlačítko **Vytvořit** dole na formuláři.

    Po úspěšném vytvoření účtu se stav změní na **Spuštěno**. 

    ![Nastavení Media Services](./media/media-services-portal-vod-get-started/media-services-settings.png)

    Ke správě vašeho účtu AMS (například nahrávání videí, kódování assetů, sledovat průběhu úloh) použijte okno **Nastavení**.

## Správa klíčů

Název účtu a primární klíč budete potřebovat pro přístup k účtu Media Services prostřednictvím programového kódu.

1. Na portálu Azure vyberte svůj účet. 

    Okno **Nastavení** se zobrazí na pravé straně. 

2. V okně **Nastavení** vyberte **Klíče**. 

    Zobrazí se okno **Správa klíčů**, které ukazuje název účtu a primární a sekundární klíče. 
3. Stisknutím tlačítka kopírování zkopírujete hodnoty.
    
    ![Klíče služby Media Services](./media/media-services-portal-vod-get-started/media-services-keys.png)

## Konfigurace koncových bodů streamování

Při práci se službou Azure Media Services je jedním nejběžnější scénářů doručování videa vašim klientům prostřednictvím streamování s adaptivní přenosovou rychlostí. Při adaptivní přenosové rychlosti streamování může klient při přehrávání videa přepínat na datový proud s vyšší nebo nižší přenosovou rychlostí podle aktuálně dostupné šířky pásma sítě, využití procesoru a dalších faktorů. Služba Media Services podporuje následující technologie adaptivní přenosové rychlosti streamování: HTTP Live Streaming (HLS), technologie Smooth Streaming, MPEG DASH a HDS (pouze pro držitele licence Adobe PrimeTime/Access).

Služba Media Services poskytuje dynamické balení, což vám umožní dodávat váš obsah s adaptivní přenosovou rychlostí s kódováním MP4 nebo technologie Smooth Streaming ve formátech streamování podporovaných službou Media Services (MPEG DASH, HLS, technologie Smooth Streaming a HDS) bez nutnosti mít uložené předem zabalené verze pro každý z těchto formátů streamování.

Pokud chcete využít výhod dynamického balení, proveďte následující:

- Zakódovat váš soubor mezzanine (zdrojový soubor) do sady souborů MP4 s adaptivní přenosovou rychlostí (postup kódování je ukázán později v tomto kurzu).  
- Vytvořit alespoň jednu jednotku streamování pro *koncový bod streamování*, ze kterého plánujete obsah doručovat. Následující postup popisuje, jak změnit počet jednotek streamování.

Při dynamickém balení stačí uložit (a platit) soubory pouze v jednom úložném formátu a služba Media Services bude sestavovat a dodávat vhodný formát streamování v reakci na požadavky klientů.

Pokud chcete vytvořit a změnit počet jednotek rezervovaných pro streaming, postupujte takto:


1. V okně **Nastavení** klikněte na **Koncové body streamování**. 

2. Klikněte na výchozí koncový bod streamování. 

    Zobrazí se okno **VÝCHOZÍ KONCOVÝ BOD STREAMOVÁNÍ – PODROBNOSTI**.

3. Pokud chcete zadat počet jednotek streamování, posuňte jezdcem **Jednotky streamování**.

    ![Jednotky streamování](./media/media-services-portal-vod-get-started/media-services-streaming-units.png)

4. Kliknutím na tlačítko **Uložit** uložte provedené změny.

    >[AZURE.NOTE]Přidělení jakýchkoli nových jednotek může trvat až 20 minut.

## Nahrání souborů

Pokud chcete streamovat videa pomocí služby Azure Media Services, musíte nahrát zdrojová videa, zakódovat je do více přenosových rychlostí a výsledek publikovat. První krok pokrývá tato část. 

1. V okně **Nastavení** klikněte na **Assety**.

    ![Nahrání souborů](./media/media-services-portal-vod-get-started/media-services-upload.png)

3. Klikněte na tlačítko **Nahrát**.

    Zobrazí se okno **Nahrát asset videa**.

    >[AZURE.NOTE] Velikost souboru není nijak omezená.
    
4. Přejděte v počítači na požadovaného video, vyberte ho a klikněte na OK.  

    Spustí se nahrávání. Jeho průběh můžete sledovat pod názvem souboru.  

Po dokončení nahrávání se nový prostředek zobrazí v okně **Assety**. 


## Kódování assetů

Při práci se službou Azure Media Services je jedním nejběžnější scénářů doručování streamování s adaptivní přenosovou rychlostí vašim klientům. Služba Media Services podporuje následující technologie adaptivní přenosové rychlosti streamování: HTTP Live Streaming (HLS), technologie Smooth Streaming, MPEG DASH a HDS (pouze pro držitele licence Adobe PrimeTime/Access). Příprava vašich videí pro streamování s adaptivní přenosovou rychlostí spočívá v zakódování zdrojového videa zdroje do souborů s více přenosovými rychlostmi. Ke kódování vašich videí byste měli použít kodér **Media Encoder Standard**.  

Služba Media Services také poskytuje dynamické balení, což vám umožní dodávat vaše soubory MP4 s více přenosovými rychlostmi ve formátech streamování MPEG DASH, HLS, technologie Smooth Streaming nebo HDS, aniž byste je museli znovu zabalit do těchto formátů streamování. Při dynamickém balení stačí uložit (a platit) soubory pouze v jednom úložném formátu a služba Media Services bude sestavovat a dodávat vhodný formát streamování v reakci na požadavky klientů.

Pokud chcete využít výhod dynamického balení, proveďte následující:

- zakódovat váš zdrojový soubor do sady souborů MP4 s více přenosovými rychlostmi (postup kódování je ukázán později v této části)
- získat alespoň jednu jednotku streamování pro koncový bod streamování, ze kterého plánujete obsah doručovat Další informace naleznete v článku o [konfiguraci koncových bodů streamování](media-services-portal-vod-get-started.md#configure-streaming-endpoints). 

### Použití portálu ke kódování

Tato část popisuje kroky, jak můžete zakódovat svůj obsah pomocí procesoru Media Encoder Standard.

1.  V okně **Nastavení** vyberte **Assety**.  
2.  V okně **Assety** vyberte asset, který chcete zakódovat.
3.  Stiskněte tlačítko **Kódovat**.
4.  V okně **Kódovat asset** vyberte procesor „Media Encoder Standard“ a jedno z přednastavení. Pokud například více, že vaše vstupní video má rozlišení 1920 × 1080 pixelů, můžete použít přednastavení „H264 Multiple Bitrate 1080p“. Další informace o přednastaveních naleznete v [tomto](https://msdn.microsoft.com/library/azure/mt269960.aspx) článku – je důležité vybrat přednastavení, která je nejvhodnější pro váš vstup videa. Pokud máte video s nízkým rozlišením (640 x 360), neměli byste používat výchozí přednastavení „H264 Multiple Bitrate 1080p“.
    
    Pro snadnější správu máte možnost upravit název výstupního assetu a název úlohy.
        
    ![Kódování assetů](./media/media-services-portal-vod-get-started/media-services-encode1.png)
5. Stiskněte **Vytvořit**.

### Monitorování průběhu úlohy kódování

Pokud chcete monitorovat průběh úlohy kódování, klikněte na **Nastavení** (v horní části stránky) a pak vyberte **Úlohy**.

![Úlohy](./media/media-services-portal-vod-get-started/media-services-jobs.png)

## Publikování obsahu

Pokud chcete uživateli poskytnout adresu URL, kterou lze použít ke streamování nebo stažení vašeho obsahu, musíte asset nejprve „publikovat“ vytvořením lokátoru. Lokátory zajišťují přístup k souborům, které jsou obsaženy v assetu. Služba Media Services podporuje dva typy lokátorů: 

- Lokátory streamování (OnDemandOrigin), které se používají pro adaptivní streamování (například pro streamování MPEG, HLS nebo technologie Smooth Streaming). Pokud chcete vytvořit lokátor streamování, váš asset musí obsahovat soubor .ism. 
- Progresivní lokátory (SAS), které se používají pro doručení videa přes progresivní stahování.


Adresa URL streamování má následující formát a můžete ji použít k přehrávání mediálních assetů technologie Smooth Streaming.

    {streaming endpoint name-media services account name}.streaming.mediaservices.windows.net/{locator ID}/{filename}.ism/Manifest

Pokud chcete vytvořit adresu URL streamování HLS, připojte na konec adresy (format=m3u8-aapl).

    {streaming endpoint name-media services account name}.streaming.mediaservices.windows.net/{locator ID}/{filename}.ism/Manifest(format=m3u8-aapl)

Pokud chcete vytvořit adresu URL streamování MPEG DASH, připojte na konec adresy (format=mpd-time-csf).

    {streaming endpoint name-media services account name}.streaming.mediaservices.windows.net/{locator ID}/{filename}.ism/Manifest(format=mpd-time-csf)


Adresa URL typu SAS má následující formát.

    {blob container name}/{asset name}/{file name}/{SAS signature}

>[AZURE.NOTE] Pokud jste použili portál k vytvoření lokátorů před březnem 2015, byly vytvořeny lokátory s platností dva roky.  

Pokud chcete aktualizovat datum vypršení platnosti lokátoru, použijte rozhraní [REST](http://msdn.microsoft.com/library/azure/hh974308.aspx#update_a_locator ) API nebo [.NET](http://go.microsoft.com/fwlink/?LinkID=533259). Upozorňujeme, že při aktualizaci data vypršení platnosti lokátoru SAS se změní adresa URL.

### Postup publikování assetu pomocí portálu

Postup publikování assetu pomocí portálu:

1. Vyberte **Nastavení** > **Assety**.
1. Vyberte asset, který chcete publikovat.
1. Potom klikněte na tlačítko **Publikovat**.
1. Vyberte typ lokátoru.
2. Stiskněte **Přidat**.

    ![Publikování](./media/media-services-portal-vod-get-started/media-services-publish1.png)

Adresa URL bude přidána do seznamu **publikovaných adres URL**.

## Přehrávání obsahu z portálu

Portál Azure nabízí přehrávač obsahu, který můžete použít k testování videa.

Klikněte na požadované video a potom klikněte na tlačítko **Přehrát**.

![Publikování](./media/media-services-portal-vod-get-started/media-services-play.png)

Musí být splněny určité předpoklady:

- Zkontrolujte, že bylo video publikováno.
- Tento *Přehrávač médií** přehrává z výchozího koncového bodu streamování. Pokud chcete přehrávat z jiného než výchozího koncového bodu streamování, klikněte na zkopírování adresy URL a použijte jiný přehrávač. Například můžete použít [Azure Media Services Player](http://amsplayer.azurewebsites.net/azuremediaplayer.html).

##Další kroky: Mapy kurzů ke službě Media Services

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##Poskytnutí zpětné vazby

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]





<!---HONumber=Aug16_HO4-->


