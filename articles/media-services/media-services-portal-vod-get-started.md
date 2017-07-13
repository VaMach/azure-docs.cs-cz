---
title: "Začínáme s doručováním videa na vyžádání (VoD) pomocí webu Azure Portal | Dokumentace Microsoftu"
description: "V tomto kurzu vás provede jednotlivými kroky implementace základní aplikace pro doručování obsahu videa na vyžádání (VoD, Video-on-Demand) pomocí služby Azure Media Services (AMS) a webu Azure Portal."
services: media-services
documentationcenter: 
author: Juliako
manager: erikre
editor: 
ms.assetid: 6c98fcfa-39e6-43a5-83a5-d4954788f8a4
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 07/12/2017
ms.author: juliako
ms.translationtype: Human Translation
ms.sourcegitcommit: 555e0b6340d09517bfd87efe209f0304f3266788
ms.openlocfilehash: 76fd245f91e1bfab3df68120859c69e459283e5b
ms.contentlocale: cs-cz
ms.lasthandoff: 01/27/2017

---
# Začínáme s doručováním obsahu na vyžádání pomocí webu Azure Portal
<a id="get-started-with-delivering-content-on-demand-using-the-azure-portal" class="xliff"></a>
[!INCLUDE [media-services-selector-get-started](../../includes/media-services-selector-get-started.md)]

V tomto kurzu vás provede jednotlivými kroky implementace základní aplikace pro doručování obsahu videa na vyžádání (VoD, Video-on-Demand) pomocí služby Azure Media Services (AMS) a webu Azure Portal.

## Požadavky
<a id="prerequisites" class="xliff"></a>
K dokončení kurzu potřebujete následující:

* Účet Azure. Podrobnosti najdete v článku [Bezplatná zkušební verze Azure](https://azure.microsoft.com/pricing/free-trial/). 
* Účet Media Services. Pokud chcete vytvořit účet Media Services, přečtěte si článek [Jak vytvořit účet Media Services](media-services-portal-create-account.md).

Tento kurz sestává z následujících úloh:

1. Spusťte koncový bod streamování.
2. Nahrání videosouboru
3. Zakódování zdrojového souboru do sady souborů MP4 s adaptivní přenosovou rychlostí
4. Publikování assetu a získání adres URL streamování a progresivního stahování  
5. Přehrání obsahu

## Spusťte koncové body streamování.
<a id="start-streaming-endpoints" class="xliff"></a> 

Při práci se službou Azure Media Services je jedním z nejběžnější scénářů doručování videa prostřednictvím streamování s adaptivní přenosovou rychlostí. Služba Media Services poskytuje dynamické balení, které umožňuje doručovat obsah s adaptivní přenosovou rychlostí s kódováním MP4 ve formátech streamování podporovaných službou Media Services (MPEG DASH, HLS, technologie Smooth Streaming). není přitom potřeba ukládat předem zabalené verze pro každý z těchto formátů streamování.

>[!NOTE]
>Po vytvoření účtu AMS se do vašeho účtu přidá **výchozí** koncový bod streamování ve stavu **Zastaveno**. Pokud chcete spustit streamování vašeho obsahu a využít výhod dynamického balení a dynamického šifrování, musí koncový bod streamování, ze kterého chcete streamovat obsah, být ve stavu **Spuštěno**. 

Pokud chcete spustit koncový bod streamování, postupujte takto:

1. Přihlaste se na [Azure Portal](https://portal.azure.com/).
2. V okně Nastavení klikněte na Koncové body streamování. 
3. Klikněte na výchozí koncový bod streamování. 

    Zobrazí se okno VÝCHOZÍ KONCOVÝ BOD STREAMOVÁNÍ – PODROBNOSTI.

4. Klikněte na ikonu Spustit.
5. Kliknutím na tlačítko Uložit uložte provedené změny.

## Nahrání souborů
<a id="upload-files" class="xliff"></a>
Pokud chcete streamovat videa pomocí služby Azure Media Services, musíte nahrát zdrojová videa, zakódovat je do více přenosových rychlostí a výsledek publikovat. První krok pokrývá tato část. 

1. V okně **Nastavení** klikněte na **Assety**.
   
    ![Nahrání souborů](./media/media-services-portal-vod-get-started/media-services-upload.png)
2. Klikněte na tlačítko **Nahrát**.
   
    Zobrazí se okno **Nahrát asset videa**.
   
   > [!NOTE]
   > Velikost souboru není nijak omezená.
   > 
   > 
3. Přejděte v počítači na požadovaného video, vyberte ho a klikněte na OK.  
   
    Spustí se nahrávání. Jeho průběh můžete sledovat pod názvem souboru.  

Po dokončení nahrávání se nový prostředek zobrazí v okně **Assety**. 

## Kódování assetů
<a id="encode-assets" class="xliff"></a>
Při práci se službou Azure Media Services je jedním nejběžnější scénářů doručování streamování s adaptivní přenosovou rychlostí vašim klientům. Služba Media Services podporuje následující technologie streamování s adaptivní přenosovou rychlostí: HTTP Live Streaming (HLS), technologie Smooth Streaming, MPEG DASH. Příprava vašich videí pro streamování s adaptivní přenosovou rychlostí spočívá v zakódování zdrojového videa zdroje do souborů s více přenosovými rychlostmi. Ke kódování vašich videí byste měli použít kodér **Media Encoder Standard**.  

Služba Media Services také poskytuje dynamické balení, což vám umožní dodávat vaše soubory MP4 s více přenosovými rychlostmi ve formátech streamování MPEG DASH, HLS nebo technologie Smooth Streaming, aniž byste je museli znovu zabalit do těchto formátů streamování. Při dynamickém balení stačí uložit (a platit) soubory pouze v jednom úložném formátu a služba Media Services sestaví a dodá vhodný formát streamování v reakci na požadavky klientů.

Pokud chcete využít výhod dynamického balení, musíte zdrojový soubor zakódovat do sady souborů MP4 s více přenosovými rychlostmi (postup kódování je uvedený dále v této části).

### Použití portálu ke kódování
<a id="to-use-the-portal-to-encode" class="xliff"></a>
Tato část popisuje kroky, jak můžete zakódovat svůj obsah pomocí procesoru Media Encoder Standard.

1. V okně **Nastavení** vyberte **Assety**.  
2. V okně **Assety** vyberte asset, který chcete zakódovat.
3. Stiskněte tlačítko **Kódovat**.
4. V okně **Kódovat asset** vyberte procesor „Media Encoder Standard“ a jedno z přednastavení. Pokud například více, že vaše vstupní video má rozlišení 1920 × 1080 pixelů, můžete použít přednastavení „H264 Multiple Bitrate 1080p“. Další informace o přednastaveních naleznete v [tomto](media-services-mes-presets-overview.md) článku – je důležité vybrat přednastavení, která je nejvhodnější pro váš vstup videa. Pokud máte video s nízkým rozlišením (640 x 360), neměli byste používat výchozí přednastavení „H264 Multiple Bitrate 1080p“.
   
   Pro snadnější správu máte možnost upravit název výstupního assetu a název úlohy.
   
   ![Kódování assetů](./media/media-services-portal-vod-get-started/media-services-encode1.png)
5. Stiskněte **Vytvořit**.

### Monitorování průběhu úlohy kódování
<a id="monitor-encoding-job-progress" class="xliff"></a>
Pokud chcete monitorovat průběh úlohy kódování, klikněte na **Nastavení** (v horní části stránky) a pak vyberte **Úlohy**.

![Úlohy](./media/media-services-portal-vod-get-started/media-services-jobs.png)

## Publikování obsahu
<a id="publish-content" class="xliff"></a>
Pokud chcete uživateli poskytnout adresu URL, kterou lze použít ke streamování nebo stažení vašeho obsahu, musíte asset nejprve „publikovat“ vytvořením lokátoru. Lokátory zajišťují přístup k souborům, které jsou obsaženy v assetu. Služba Media Services podporuje dva typy lokátorů: 

* Lokátory streamování (OnDemandOrigin), které se používají pro adaptivní streamování (například pro streamování MPEG, HLS nebo technologie Smooth Streaming). Pokud chcete vytvořit lokátor streamování, váš asset musí obsahovat soubor .ism. 
* Progresivní lokátory (SAS), které se používají pro doručení videa přes progresivní stahování.

Adresa URL streamování má následující formát a můžete ji použít k přehrávání mediálních assetů technologie Smooth Streaming.

    {streaming endpoint name-media services account name}.streaming.mediaservices.windows.net/{locator ID}/{filename}.ism/Manifest

Pokud chcete vytvořit adresu URL streamování HLS, připojte na konec adresy (format=m3u8-aapl).

    {streaming endpoint name-media services account name}.streaming.mediaservices.windows.net/{locator ID}/{filename}.ism/Manifest(format=m3u8-aapl)

Pokud chcete vytvořit adresu URL streamování MPEG DASH, připojte na konec adresy (format=mpd-time-csf).

    {streaming endpoint name-media services account name}.streaming.mediaservices.windows.net/{locator ID}/{filename}.ism/Manifest(format=mpd-time-csf)


Adresa URL typu SAS má následující formát.

    {blob container name}/{asset name}/{file name}/{SAS signature}

> [!NOTE]
> Pokud jste použili portál k vytvoření lokátorů před březnem 2015, byly vytvořeny lokátory s platností dva roky.  
> 
> 

Pokud chcete aktualizovat datum vypršení platnosti lokátoru, použijte rozhraní [REST](https://docs.microsoft.com/rest/api/media/operations/locator#update_a_locator) API nebo [.NET](http://go.microsoft.com/fwlink/?LinkID=533259). Při aktualizaci data vypršení platnosti lokátoru SAS se změní adresa URL.

### Postup publikování assetu pomocí portálu
<a id="to-use-the-portal-to-publish-an-asset" class="xliff"></a>
Postup publikování assetu pomocí portálu:

1. Vyberte **Nastavení** > **Assety**.
2. Vyberte asset, který chcete publikovat.
3. Potom klikněte na tlačítko **Publikovat**.
4. Vyberte typ lokátoru.
5. Stiskněte **Přidat**.
   
    ![Publikování](./media/media-services-portal-vod-get-started/media-services-publish1.png)

Adresa URL se přidá do seznamu **publikovaných adres URL**.

## Přehrávání obsahu z portálu
<a id="play-content-from-the-portal" class="xliff"></a>
Azure Portal nabízí přehrávač obsahu, který můžete použít k testování videa.

Klikněte na požadované video a potom klikněte na tlačítko **Přehrát**.

![Publikování](./media/media-services-portal-vod-get-started/media-services-play.png)

Musí být splněny určité předpoklady:

* Zkontrolujte, že bylo video publikováno.
* Tento **Přehrávač médií** přehrává z výchozího koncového bodu streamování. Pokud chcete přehrávat z jiného než výchozího koncového bodu streamování, klikněte na zkopírování adresy URL a použijte jiný přehrávač. Například můžete použít [Azure Media Services Player](http://amsplayer.azurewebsites.net/azuremediaplayer.html).

## Další kroky
<a id="next-steps" class="xliff"></a>
Prohlédněte si mapy kurzů k Media Services.

[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## Poskytnutí zpětné vazby
<a id="provide-feedback" class="xliff"></a>
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]


