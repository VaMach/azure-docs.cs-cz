<properties 
    pageTitle="Postup živého streamování využívajícího službu Azure Media Services k vytvoření datových proudů s více přenosovými rychlostmi pomocí portálu Azure Classic" 
    description="Tento kurz vás provede kroky k vytvoření kanálu, který přijímá datový proud s jednou přenosovou rychlostí a kóduje ho do datového proudu s více přenosovými rychlostmi pomocí portálu Azure Classic." 
    services="media-services" 
    documentationCenter="" 
    authors="juliako,anilmur" 
    manager="erikre" 
    editor=""/>

<tags 
    ms.service="media-services" 
    ms.workload="media" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="get-started-article"
    ms.date="05/05/2016" 
    ms.author="juliako"/>


#Postup živého streamování využívajícího službu Azure Media Services k vytvoření datových proudů s více přenosovými rychlostmi pomocí portálu Azure Classic

> [AZURE.SELECTOR]
- [Portál](media-services-portal-creating-live-encoder-enabled-channel.md)
- [.NET](media-services-dotnet-creating-live-encoder-enabled-channel.md)
- [REST API](https://msdn.microsoft.com/library/azure/dn783458.aspx)

Tento kurz vás provede kroky k vytvoření **kanálu**, který přijímá datový proud s jednou přenosovou rychlostí a kóduje ho do datového proudu s více přenosovými rychlostmi.

>[AZURE.NOTE]Další koncepční informace o kanálech s povoleným kódováním v reálném čase najdete v článku [Živé streamování využívající Azure Media Services k vytváření datových proudů s více přenosovými rychlostmi](media-services-manage-live-encoder-enabled-channels.md).

##Běžný scénář živého streamování

Následující část představuje obecné kroky, které jsou součástí procesu vytváření běžných aplikací pro živé streamování.

>[AZURE.NOTE] V současné době doporučujeme maximální dobu trvání živé události v délce 8 hodin. Pokud potřebujete, aby kanál běžel delší dobu, kontaktujte nás prosím na adrese amslived@microsoft.com.

1. Připojte k počítači videokameru. Spusťte a nakonfigurujte místní kodér pro kódování v reálném čase, který umí produkovat datový proud s jednou přenosovou rychlostí v jednom z následujících protokolů: RTMP, technologie Smooth Streaming nebo RTP (MPEG-TS). Další informace najdete v článku [Podpora RTMP ve službě Azure Media Services a kodéry pro kódování v reálném čase](http://go.microsoft.com/fwlink/?LinkId=532824).
    
    Tento krok můžete provést i po vytvoření kanálu.

1. Vytvořte a spusťte kanál. 

1. Načtěte adresu URL ingestování kanálu. 

    Adresu URL ingestování používá kodér po kódování v reálném čase k odesílání datového proudu do kanálu.
1. Načtěte adresu URL náhledu kanálu. 

    Tuto adresu můžete použít, když chcete ověřit, jestli kanál správně přijímá živý datový proud.

3. Vytvořte program (tím se vytvoří také asset). 
1. Publikujte program (tím se vytvoří lokátor OnDemand pro přidružený asset).  

    Zajistěte, abyste na koncovém bodu streamování (ze kterého chcete streamovat obsah) měli aspoň jednu jednotku rezervovanou pro streaming.
1. Jakmile budete připraveni začít streamovat a archivovat, spusťte program.
2. Volitelně můžete dát kodéru pro kódování v reálném čase signál, aby spustil reklamu. Reklama bude vložena do výstupního datového proudu.
1. Kdykoli budete chtít zastavit streamování a archivaci události, zastavte program.
1. Odstraňte program (a volitelně můžete odstranit i asset).   

##V tomto kurzu

V tomto kurzu budeme portál Azure Classic používat k provádění následujících úloh: 

2.  Konfigurace koncových bodů streamování.
3.  Vytvoření kanálu, který má povolené kódování v reálném čase.
1.  Získání ingestované adresy URL, která bude dodána kodéru pro kódování v reálném čase. Kodér pro kódování v reálném čase tuto adresu URL použije k ingestování datového proudu do kanálu. .
1.  Vytvoření programu (a assetu)
1.  Publikování assetu a získání adresy URL pro streamování  
1.  Přehrání obsahu 
2.  Čištění

##Požadavky
K dokončení kurzu potřebujete následující:

- K dokončení tohoto kurzu potřebujete mít účet Azure. Pokud účet nemáte, můžete si během několika minut vytvořit bezplatný zkušební účet. Podrobnosti najdete v článku [Bezplatná zkušební verze Azure](https://azure.microsoft.com/pricing/free-trial/).
- Účet Media Services. Pokud chcete vytvořit účet Media Services, přečtěte si článek [Vytvoření účtu](media-services-create-account.md).
- Webová kamera a kodér, který dokáže odesílat živý datový proud s jednou přenosovou rychlostí.

##Konfigurace koncového bodu streamování pomocí portálu

Při práci se službou Azure Media Services je jedním nejběžnějších scénářů doručování streamování s adaptivní přenosovou rychlostí vašim klientům. Při adaptivní přenosové rychlosti streamování může klient při přehrávání videa přepínat na datový proud s vyšší nebo nižší přenosovou rychlostí podle aktuálně dostupné šířky pásma sítě, využití procesoru a dalších faktorů. Služba Media Services podporuje následující technologie adaptivní přenosové rychlosti streamování: HTTP Live Streaming (HLS), technologie Smooth Streaming, MPEG DASH a HDS (pouze pro držitele licence Adobe PrimeTime/Access). 

Při práci s živým streamováním místní kodér pro kódování v reálném čase (v našem případě Wirecast) ingestuje živý datový proud s více přenosovými rychlostmi do vašeho kanálu. Když si uživatel vyžádá datový proud, služba Media Services použije dynamické balení a znovu zdrojový datový proud zabalí do požadovaného datového proudu s adaptivní přenosovou rychlostí (HLS, DASH nebo Smooth). 

Pokud chcete využít výhody dynamického balení, získejte alespoň jednu jednotku streamování pro **koncový bod streamování**, ze kterého plánujete obsah doručovat.

Pokud chcete změnit počet jednotek rezervovaných pro streaming, postupujte takto:

1. Na stránce [Portál Azure Classic](https://manage.windowsazure.com/) klikněte na **Media Services**. Potom klikněte na název mediální služby.

2. Vyberte stránku KONCOVÉ BODY STREAMOVÁNÍ. Potom klikněte na koncový bod streamování, který chcete upravit.

3. Pokud chcete zadat počet jednotek streamování, vyberte kartu ŠKÁLOVAT a posuňte posuvníkem **rezervované kapacity**.

    ![Stránka Škálovat](./media/media-services-portal-creating-live-encoder-enabled-channel/media-services-origin-scale.png)

4. Stisknutím tlačítka ULOŽIT uložte provedené změny.

    Dokončení přidělení jakýchkoli nových jednotek trvá přibližně 20 minut. 

     
    >[AZURE.NOTE] Změna z jakéhokoli kladného počtu jednotek streamování zpět na nulu může v současnosti zcela vypnout streamování až na jednu hodinu.
    >
    > Pro výpočet ceny se používá nejvyšší zadaný počet jednotek v průběhu období 24 hodin. Podrobné informace o cenách najdete v článku [Ceny služby Azure Media Services](http://go.microsoft.com/fwlink/?LinkId=275107).

 
##Vytvoření KANÁLU

1.  Na stránce [Portál Azure Classic](http://manage.windowsazure.com/) klikněte na Media Services a potom klikněte na název účtu Media Services.
2.  Vyberte stránku KANÁLY.
3.  Výběrem Přidat přidejte nový kanál.

Zvolte typ kódování **Standard**. Tento typ určuje, že chcete vytvořit kanál, který má povolené kódování v reálném čase. To znamená, že příchozí datový proud s jednou přenosovou rychlostí se odesílá do kanálu a překóduje se na datový proud s více přenosovými rychlostmi pomocí určených nastavení kodéru pro kódování v reálném čase. Další informace najdete v článku [Živé streamování využívající službu Azure Media Services k vytvoření datových proudů s více přenosovými rychlostmi](media-services-manage-live-encoder-enabled-channels.md).

![standard0][standard0]

V případě typu kódování **Standard** jsou platné tyto možnosti protokolu ingestování:

- Fragmentovaný soubor MP4 s jednou přenosovou rychlostí (technologie Smooth Streaming)
- RTMP s jednou přenosovou rychlostí
- RTP (MPEG-TS): přenosový stream MPEG-2 využívající RTP.

Podrobné vysvětlení jednotlivých protokolů najdete v článku [Živé streamování využívající službu Azure Media Services k vytvoření datových proudů s více přenosovými rychlostmi](media-services-manage-live-encoder-enabled-channels.md).

![standard1][standard1]

Vstupní protokol nemůžete změnit, když kanál nebo jeho přidružené programy běží. Pokud požadujete různé protokoly, vytvořte samostatné kanály pro každý vstupní protokol.  

Na stránce **Konfigurace reklamy** můžete určit zdroj signálů reklamních značek. Když používáte portál, můžete vybrat jenom to rozhraní API, které označuje, že kodér pro kódování v reálném čase by měl v rámci kanálu naslouchat asynchronnímu rozhraní API reklamní značky. Když používáte portál, můžete vybrat jenom rozhraní API.

Další informace najdete v článku [Živé streamování využívající službu Azure Media Services k vytvoření datových proudů s více přenosovými rychlostmi](media-services-manage-live-encoder-enabled-channels.md).

![standard2][standard2]

Na stránce **Přednastavení kódování** můžete vybrat přednastavení systému. Momentálně je dostupná jenom jedna možnost: **Výchozí 720 p**.

![standard3][standard3]

Na stránce **Vytvoření kanálu** můžete definovat IP adresy, které mají v tomto kanálu povoleno publikování videa. Povolené IP adresy můžete zadat buď jako jednu IP adresu (např. 10.0.0.1), rozsah IP adres pomocí IP adresy a masky podsítě CIDR (např. 10.0.0.1/22), nebo jako rozsah IP adres pomocí IP adresy a masky podsítě s tečkou (např. 10.0.0.1(255.255.252.0)).

Pokud žádné IP adresy nezadáte a nedefinujete žádné pravidlo, nebude povolená žádná IP adresa. Pokud chcete povolit libovolnou IP adresy, vytvořte pravidlo a nastavte 0.0.0.0/0.


![standard4][standard4]

>[AZURE.NOTE] V současné době může spuštění kanálu trvat až 30 minut. Resetování kanálu může trvat až 5 minut.

Po vytvoření kanálu můžete vybrat kartu **KODÉR**, na které uvidíte konfigurace vašeho kanálů. Můžete také spravovat reklamy a slaty. 

![standard5][standard5]

Další informace najdete v článku [Živé streamování využívající službu Azure Media Services k vytvoření datových proudů s více přenosovými rychlostmi](media-services-manage-live-encoder-enabled-channels.md).


##Získání ingestovaných adres URL

Po vytvoření kanálu můžete získat ingestované adresy URL, které poskytnete kodéru pro kódování v reálném čase. Kodér tyto adresy URL používá ke vkládání živého datového proudu.

![readychannel](./media/media-services-portal-creating-live-encoder-enabled-channel/media-services-ready-channel.png)


![ingesturls](./media/media-services-portal-creating-live-encoder-enabled-channel/media-services-ingest-urls.png)


##Vytvoření a správa programu

###Přehled

Kanál je přidružený k programům, které vám umožňují řídit publikování a ukládání segmentů v živém datovém proudu. Kanály spravují programy. Vztah kanálů a programů se velmi podobná tradičním médiím, kde kanál obsahuje nepřetržitý datový proud obsahu a program je vymezen na určité načasované události v tomto kanálu.

Nastavením délky **archivačního okna** můžete určit počet hodin, po které chcete uchovávat zaznamenaný obsah programu. Tuto hodnotu můžete nastavit v rozmezí od 5 minut po 25 hodin. Délka archivačního okna také určuje maximální časový úsek, který můžou klienti prohledávat od aktuální živé pozice směrem zpět v čase. Programy můžou běžet po určenou dobu a obsah, který se do délky okna nevejde, bude vždy zahozen. Hodnota této vlastnosti také určuje, jak dlouho můžou růst manifesty klientů.

Každý program je přidružený k assetu. Pokud chcete program publikovat, musíte pro přidružený asset vytvořit lokátor OnDemand. Tento lokátor vám umožní sestavit adresu URL pro streamování, kterou potom poskytnete svým klientům.

Kanál podporuje až tři současně spuštěné programy, takže si můžete vytvořit několik archivů stejného příchozího datového proudu. To vám umožní podle potřeby publikovat a archivovat různé části události. Požadavek vaší firmy může být například takový, že chcete archivovat 6 hodin programu, ale vysílat jenom posledních 10 minut. K tomu potřebujete vytvořit dva současně spuštěné programy. Jeden program nastavíte, aby archivoval 6 hodin události, ale tento program nebudete publikovat. Druhý program nastavíte, aby archivoval 10 minut a tento program budete publikovat.

Existující programy nepoužívejte znovu pro nové události. Místo toho vytvořte a spusťte nový program pro každou jednotlivou událost.

Jakmile budete připraveni začít streamovat a archivovat, spusťte program. Kdykoli budete chtít zastavit streamování a archivaci události, zastavte program. 

Pokud chcete archivovaný obsah odstranit, zastavte a odstraňte program a potom odstraňte přidružený asset. Asset nemůžete odstranit, pokud ho program používá. Nejdřív odstraňte program. 

I po zastavení a odstranění programu můžou uživatelé streamovat archivovaný obsah jako video na vyžádání, a to tak dlouho, dokud neodstraníte asset.

Pokud chcete archivovaný obsah zachovat, ale nechcete ho zpřístupňovat pro streamování, odstraňte lokátor streamování.

###Vytvoření, spuštění a zastavení programů

Jakmile datový proud plyne do kanálu, můžete událost streamování zahájit tím, že vytvoříte asset, program a lokátor streamování. Datový proud se tak archivuje a zpřístupní se divákům prostřednictvím koncového bodu streamování. 

Událost můžete spustit dvěma způsoby: 

1. Na stránce **KANÁL** stiskněte **PŘIDAT** a přidejte nový program.

    Zadejte: název aplikace, název assetu, archivační okno a možnost šifrování.
    
    ![createprogram](./media/media-services-portal-creating-live-encoder-enabled-channel/media-services-create-program.png)
    
    Pokud jste nechali zaškrtnuté políčko **Publikovat program**, budou vytvořeny ADRESY URL PRO PUBLIKOVÁNÍ programu.
    
    Kdykoli budete připraveni program streamovat, stiskněte **SPUSTIT**.

    Po spuštění programu můžete stisknout tlačítko PŘEHRÁT a spustit přehrávání obsahu.


    ![createdprogram](./media/media-services-portal-creating-live-encoder-enabled-channel/media-services-created-program.png)

2. Alternativně můžete použít zástupce a na stránce **KANÁL** můžete stisknout tlačítko **SPUSTIT STREAMOVÁNÍ**. Tím vytvoříte asset, program a lokátor streamování.

    Program získá název DefaultProgram a archivační okno bude nastaveno na 1 hodinu.

    Publikovaný program můžete přehrát ze stránky KANÁL. 

    ![channelpublish](./media/media-services-portal-creating-live-encoder-enabled-channel/media-services-channel-play.png)


Pokud na stránce **KANÁL** kliknete na tlačítko **ZASTAVIT STREAMOVÁNÍ**, výchozí program se zastaví a odstraní. Asset bude stále existovat a jeho publikování můžete provést nebo zrušit na stránce **OBSAH**.

Pokud přejdete na stránku **OBSAH**, uvidíte assety, které byly vytvořeny pro vaše programy.

![contentasset](./media/media-services-portal-creating-live-encoder-enabled-channel/media-services-content-assets.png)


##Přehrávání obsahu

Pokud chcete svým uživatelům poskytnout adresu URL, kterou můžou použít ke streamování vašeho obsahu, musíte asset nejdřív „publikovat“ (jak je popsáno v předchozí části) tak, že vytvoříte lokátor (když asset publikujete pomocí portálu, vytvoří se lokátory automaticky). Lokátory zajišťují přístup k souborům, které jsou obsaženy v assetu. 

V závislosti na tom, jaký protokol pro streamování chcete k přehrávání obsahu použít, může být potřeba změnit adresu URL, kterou můžete získat z odkazu **PUBLIKOVAT ADRESU URL**, který najdete v kanálu nebo programu.

Dynamické balení se postará o zabalení živého datového proudu do určeného protokolu. 

Ve výchozím nastavení má adresa URL pro streamování následující formát a můžete ji použít k přehrávání assetů technologie Smooth Streaming:

    {streaming endpoint name-media services account name}.streaming.mediaservices.windows.net/{locator ID}/{filename}.ism/Manifest

Pokud chcete vytvořit adresu URL streamování HLS, připojte na konec adresy (format=m3u8-aapl).

    {streaming endpoint name-media services account name}.streaming.mediaservices.windows.net/{locator ID}/{filename}.ism/Manifest(format=m3u8-aapl)

Pokud chcete vytvořit adresu URL streamování MPEG DASH, připojte na konec adresy (format=mpd-time-csf).

    {streaming endpoint name-media services account name}.streaming.mediaservices.windows.net/{locator ID}/{filename}.ism/Manifest(format=mpd-time-csf)

Další informace o doručování obsahu najdete v článku [Doručení obsahu](media-services-deliver-content-overview.md).

Datový proud Smooth můžete přehrávat pomocí [přehrávače AMS](http://amsplayer.azurewebsites.net/azuremediaplayer.html), popřípadě k přehrávání HLS verze 3 použijte zařízení iOS nebo Android.

##Vyčištění

Pokud jste dokončili streamování událostí a chcete dříve zřízené prostředky vyčistit, postupujte podle následujícího návodu.

- Zastavte odesílání datového proudu z kodéru.
- Zastavte kanál. Zastavený kanál zastaví narůstání poplatků. Když ho budete potřebovat znovu spustit, budete mít stejnou ingestovanou adresu URL, takže nebude nutné kodér znovu konfigurovat.
- Pokud nechcete pokračovat v poskytování archivu živé události ve formě datového proudu na vyžádání, můžete koncový bod streamování zastavit. Pokud je kanál v zastaveném stavu, nebudou vám narůstat poplatky.
  

##Požadavky

- V současné době doporučujeme maximální dobu trvání živé události v délce 8 hodin. Pokud potřebujete, aby kanál běžel delší dobu, kontaktujte nás prosím na adrese amslived@microsoft.com.
- Zajistěte, abyste na koncovém bodu streamování (ze kterého chcete streamovat obsah) měli aspoň jednu jednotku rezervovanou pro streaming.


##Mapy kurzů ke službě Media Services

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##Poskytnutí zpětné vazby

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]



[standard0]: ./media/media-services-portal-creating-live-encoder-enabled-channel/media-services-create-channel-standard0.png
[standard1]: ./media/media-services-portal-creating-live-encoder-enabled-channel/media-services-create-channel-standard1.png
[standard2]: ./media/media-services-portal-creating-live-encoder-enabled-channel/media-services-create-channel-standard2.png
[standard3]: ./media/media-services-portal-creating-live-encoder-enabled-channel/media-services-create-channel-standard3.png
[standard4]: ./media/media-services-portal-creating-live-encoder-enabled-channel/media-services-create-channel-standard4.png
[standard5]: ./media/media-services-portal-creating-live-encoder-enabled-channel/media-services-create-channel-standard_encode.png 


<!--HONumber=Jun16_HO2-->


