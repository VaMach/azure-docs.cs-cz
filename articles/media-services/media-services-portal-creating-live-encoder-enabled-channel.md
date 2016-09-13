<properties 
    pageTitle="Postup živého streamování využívajícího službu Azure Media Services k vytvoření datových proudů s více přenosovými rychlostmi pomocí webu Azure Portal | Microsoft Azure" 
    description="Tento kurz vás provede kroky k vytvoření kanálu, který přijímá datový proud s jednou přenosovou rychlostí a kóduje ho do datového proudu s více přenosovými rychlostmi pomocí webu Azure Portal." 
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
    ms.date="09/06/2016"
    ms.author="juliako"/>


#Postup živého streamování využívajícího službu Azure Media Services k vytvoření datových proudů s více přenosovými rychlostmi pomocí webu Azure Portal

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

    Tuto adresu URL můžete použít, když chcete ověřit, jestli kanál správně přijímá proud živého vysílání.

3. Vytvořte událost nebo program (tím se vytvoří také asset). 
1. Publikujte událost (tím se vytvoří lokátor OnDemand pro přidružený asset).  

    Zajistěte, abyste na koncovém bodu streamování (ze kterého chcete streamovat obsah) měli aspoň jednu jednotku rezervovanou pro streaming.
1. Jakmile budete připraveni začít streamovat a archivovat, spusťte událost.
2. Volitelně můžete dát kodéru pro kódování v reálném čase signál, aby spustil reklamu. Reklama bude vložena do výstupního proudu.
1. Kdykoli budete chtít zastavit streamování a archivaci události, zastavte událost.
1. Odstraňte událost (volitelně můžete odstranit i asset).   

##V tomto kurzu

V tomto kurzu budeme Azure Portal používat k provádění následujících úloh: 

2.  Konfigurace koncových bodů streamování.
3.  Vytvoření kanálu, který má povolené kódování v reálném čase.
1.  Získání ingestované adresy URL, která bude dodána kodéru pro kódování v reálném čase. Kodér pro kódování v reálném čase tuto adresu URL použije k ingestování datového proudu do kanálu. .
1.  Vytvoření události nebo programu (a assetu)
1.  Publikování assetu a získání adresy URL pro streamování  
1.  Přehrání obsahu 
2.  Čištění

##Požadavky
K dokončení kurzu potřebujete následující:

- K dokončení tohoto kurzu potřebujete mít účet Azure. Pokud účet nemáte, můžete si během několika minut vytvořit bezplatný zkušební účet. Podrobnosti najdete v článku [Bezplatná zkušební verze Azure](https://azure.microsoft.com/pricing/free-trial/).
- Účet Media Services. Pokud chcete vytvořit účet Media Services, přečtěte si článek [Vytvoření účtu](media-services-create-account.md).
- Webová kamera a kodér, který dokáže odesílat živý datový proud s jednou přenosovou rychlostí.

##Konfigurace koncových bodů streamování 

Služba Media Services poskytuje dynamické balení, což vám umožní dodávat vaše soubory MP4 s více přenosovými rychlostmi ve formátech streamování MPEG DASH, HLS, Smooth Streaming nebo HDS, aniž byste je museli znovu zabalit do těchto formátů streamování. Při dynamickém balení stačí uložit (a platit) soubory pouze v jednom úložném formátu a služba Media Services bude sestavovat a dodávat vhodný formát streamování v reakci na požadavky klientů.

Pokud chcete využít výhody dynamického balení, získejte alespoň jednu jednotku streamování pro koncový bod streamování, ze kterého plánujete obsah doručovat.  

Pokud chcete vytvořit a změnit počet jednotek rezervovaných pro streaming, postupujte takto:

1. Přihlaste se na [Azure Portal](https://portal.azure.com/).
1. V okně **Nastavení** klikněte na **Koncové body streamování**. 

2. Klikněte na výchozí koncový bod streamování. 

    Zobrazí se okno **VÝCHOZÍ KONCOVÝ BOD STREAMOVÁNÍ – PODROBNOSTI**.

3. Pokud chcete zadat počet jednotek streamování, posuňte jezdcem **Jednotky streamování**.

    ![Jednotky streamování](./media/media-services-portal-creating-live-encoder-enabled-channel/media-services-streaming-units.png)

4. Kliknutím na tlačítko **Uložit** uložte provedené změny.

    >[AZURE.NOTE]Přidělení jakýchkoli nových jednotek může trvat až 20 minut.

##Vytvoření KANÁLU

1. Na webu [Azure Portal](https://portal.azure.com/) klikněte na Media Services a poté klikněte na název účtu Media Services.
2. Vyberte **Živé streamování**.
3. Vyberte **Vytvořit vlastní**. Tato možnost vám umožní vytvořit kanál, který má povolené kódování v reálném čase.

    ![Vytvoření kanálu](./media/media-services-portal-creating-live-encoder-enabled-channel/media-services-create-channel.png)
    
4. Klikněte na **Nastavení**.
    
    1.  Jako typ kanálu zvolte **Live Encoding**. Tento typ určuje, že chcete vytvořit kanál, který má povolené kódování v reálném čase. To znamená, že příchozí datový proud s jednou přenosovou rychlostí se odesílá do kanálu a překóduje se na datový proud s více přenosovými rychlostmi pomocí určených nastavení kodéru pro kódování v reálném čase. Další informace najdete v článku [Živé streamování využívající službu Azure Media Services k vytvoření datových proudů s více přenosovými rychlostmi](media-services-manage-live-encoder-enabled-channels.md). Klikněte na tlačítko OK.
    2. Zadejte název kanálu.
    3. V dolní části obrazovky klikněte na OK.
    
5. Vyberte kartu **Ingestovat**.

    1. Na této stránce můžete vybrat protokol streamování. Pro kanál typu **Live Encoding** jsou platné tyto možnosti:
        
        - Fragmentovaný soubor MP4 s jednou přenosovou rychlostí (technologie Smooth Streaming)
        - RTMP s jednou přenosovou rychlostí
        - RTP (MPEG-TS): přenosový stream MPEG-2 využívající RTP.
        
        Podrobné vysvětlení jednotlivých protokolů najdete v článku [Živé streamování využívající službu Azure Media Services k vytvoření datových proudů s více přenosovými rychlostmi](media-services-manage-live-encoder-enabled-channels.md).
    
        Možnost protokolu nelze změnit, pokud kanál nebo jeho přidružené události nebo programy právě běží. Pokud požadujete různé protokoly, vytvořte samostatné kanály pro každý protokol streamování.  

    2. Na ingestování můžete použít omezení IP adres. 
    
        Můžete definovat IP adresy, které mají v tomto kanálu povoleno ingestování videa. Povolené IP adresy můžete zadat buď jako jednu IP adresu (např. '10.0.0.1'), rozsah IP adres pomocí IP adresy a masky podsítě s technologií CIDR (např. '10.0.0.1/22') nebo jako rozsah IP adres pomocí IP adresy a masky podsítě zapsané jako čísla oddělená tečkami (např. '10.0.0.1(255.255.252.0)').

        Pokud žádné IP adresy nezadáte a nedefinujete žádné pravidlo, nebude povolená žádná IP adresa. Pokud chcete povolit libovolnou IP adresy, vytvořte pravidlo a nastavte 0.0.0.0/0.

6. Na kartě **Náhled** použijte na náhled omezení IP adres.
7. Na kartě **Kódování** zadejte předvolbu kódování. 

    Momentálně je dostupná jenom jedna možnost: **Výchozí 720 p**. Chcete-li zadat vlastní předvolbu, otevřete lístek podpory společnosti Microsoft. Poté zadejte název vytvořené předvolby. 

>[AZURE.NOTE] V současné době může spuštění kanálu trvat až 30 minut. Resetování kanálu může trvat až 5 minut.

Po vytvoření kanálu můžete kliknutím na kanál a výběrem **Nastavení** zobrazit konfigurace svých kanálů. 

Další informace najdete v článku [Živé streamování využívající službu Azure Media Services k vytvoření datových proudů s více přenosovými rychlostmi](media-services-manage-live-encoder-enabled-channels.md).


##Získání ingestovaných adres URL

Po vytvoření kanálu můžete získat ingestované adresy URL, které poskytnete kodéru pro kódování v reálném čase. Kodér tyto adresy URL používá ke vkládání živého proudu.

![ingesturls](./media/media-services-portal-creating-live-encoder-enabled-channel/media-services-ingest-urls.png)


##Vytvoření a správa událostí

###Přehled

Kanál je přidružený k událostem a programům, které vám umožňují řídit publikování a ukládání segmentů v živém datovém proudu. Kanály spravují události nebo programy. Vztah kanálů a programů se velmi podobná tradičním médiím, kde kanál obsahuje nepřetržitý datový proud obsahu a program je vymezen na určité načasované události v tomto kanálu.

Nastavením délky **archivačního okna** můžete určit počet hodin, po který chcete uchovávat zaznamenaný obsah události. Tuto hodnotu můžete nastavit v rozmezí od 5 minut po 25 hodin. Délka archivačního okna také určuje maximální časový úsek, který můžou klienti prohledávat od aktuální živé pozice směrem zpět v čase. Programy můžou běžet po určenou dobu a obsah, který se do délky okna nevejde, bude vždy zahozen. Hodnota této vlastnosti také určuje, jak dlouho můžou růst manifesty klientů.

Každá událost je přidružena k assetu. Pokud chcete publikovat událost, musíte pro přidružený asset vytvořit lokátor OnDemand. Tento lokátor vám umožní sestavit adresu URL pro streamování, kterou potom poskytnete svým klientům.

Kanál podporuje až tři současně spuštěné programy, takže si můžete vytvořit několik archivů stejného příchozího datového proudu. To vám umožní podle potřeby publikovat a archivovat různé části události. Požadavek vaší firmy může být například takový, že chcete archivovat 6 hodin události, ale vysílat pouze posledních 10 minut. K tomu potřebujete vytvořit dvě současně spuštěné události. Jednu událost nastavíte, aby archivovala 6 hodin události, ale tento program nebudete publikován. Druhou událost nastavíte, aby archivovala 10 minut a tento program budete publikovat.

Existující programy nepoužívejte znovu pro nové události. Místo toho vytvořte a spusťte nový program pro každou jednotlivou událost.

Jakmile budete připraveni začít streamovat a archivovat, spusťte událost nebo program. Kdykoli budete chtít zastavit streamování a archivaci události, zastavte událost. 

Pokud chcete archivovaný obsah odstranit, zastavte a odstraňte událost a potom odstraňte přidružený asset. Asset nemůžete odstranit, pokud ho událost používá. Nejdříve je nutné odstranit událost. 

I po zastavení a odstranění události můžou uživatelé streamovat archivovaný obsah jako video na vyžádání, a to tak dlouho, dokud asset neodstraníte.

Pokud chcete archivovaný obsah zachovat, ale nechcete ho zpřístupňovat pro streamování, odstraňte lokátor streamování.

###Vytvoření, spuštění a zastavení událostí

Jakmile datový proud plyne do kanálu, můžete událost streamování zahájit tím, že vytvoříte asset, program a lokátor streamování. Datový proud se tak archivuje a zpřístupní se divákům prostřednictvím koncového bodu streamování. 

Událost můžete spustit dvěma způsoby: 

1. Na stránce **Kanál** stisknutím **Živá událost** přidejte novou událost.

    Zadejte název události, název assetu, archivační okno a možnost šifrování.
    
    ![createprogram](./media/media-services-portal-creating-live-encoder-enabled-channel/media-services-create-program.png)
    
    Pokud jste nechali zaškrtnuté políčko **Publikovat událost nyní**, budou vytvořeny ADRESY URL PRO PUBLIKOVÁNÍ události.
    
    Kdykoli budete připraveni událost streamovat, stiskněte **Spustit**.

    Po spuštění události můžete stisknout **Přehrát** a spustit přehrávání obsahu.

2. Případně můžete použít zástupce a na stránce **Kanál** stisknout tlačítko **Spustit streamování**. Tím vytvoříte výchozí asset, program a lokátor streamování.

    Událost získá název **default** a archivační okno bude nastaveno na 8 hodin.

Publikovanou událost můžete sledovat na stránce **Živá událost**. 

Pokud kliknete na tlačítko **Zrušit streamování**, zastaví se všechny živé události. 


##Sledování události

Pokud chcete sledovat událost, klikněte na tlačítko **Sledovat** na portálu Azure nebo zkopírujte adresu URL streamování a použijte přehrávač dle svého výběru. 
 
![Vytvořeno](./media/media-services-portal-creating-live-encoder-enabled-channel/media-services-play-event.png)

Při zastavení se živá událost automaticky převede na obsah na vyžádání.

##Vyčištění

Pokud jste dokončili streamování událostí a chcete dříve zřízené prostředky vyčistit, postupujte podle následujícího návodu.

- Zastavte odesílání datového proudu z kodéru.
- Zastavte kanál. Zastavený kanál zastaví narůstání poplatků. Když ho budete potřebovat znovu spustit, budete mít stejnou ingestovanou adresu URL, takže nebude nutné kodér znovu konfigurovat.
- Pokud nechcete pokračovat v poskytování archivu živé události ve formě datového proudu na vyžádání, můžete koncový bod streamování zastavit. Pokud je kanál v zastaveném stavu, nebudou vám narůstat poplatky.
  
##Zobrazení archivovaného obsahu

I po zastavení a odstranění události můžou uživatelé streamovat archivovaný obsah jako video na vyžádání, a to tak dlouho, dokud asset neodstraníte. Asset nemůžete odstranit, pokud ho událost používá. Nejdřív odstraňte událost. 

Ke správě zvolených prostředků vyberte **Nastavení** a klikněte na tlačítko **Prostředky**.

![Prostředky](./media/media-services-portal-creating-live-encoder-enabled-channel/media-services-assets.png)

##Požadavky

- V současné době doporučujeme maximální dobu trvání živé události v délce 8 hodin. Pokud potřebujete, aby kanál běžel delší dobu, kontaktujte nás prosím na adrese amslived@microsoft.com.
- Zajistěte, abyste na koncovém bodu streamování (ze kterého chcete streamovat obsah) měli aspoň jednu jednotku rezervovanou pro streaming.


##Další krok

Prohlédněte si mapy kurzů k Media Services.

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##Poskytnutí zpětné vazby

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

 


<!--HONumber=sep16_HO1-->


