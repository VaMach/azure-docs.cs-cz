<properties 
    pageTitle="Jak provést živé streamování pomocí místních kodérů na portálu Azure | Microsoft Azure" 
    description="Tento kurz vás provede kroky pro vytvoření Kanálu, který je nakonfigurován pro průchozí doručování." 
    services="media-services" 
    documentationCenter="" 
    authors="juliako" 
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


#Jak provést živé streamování pomocí místních kodérů na portálu Azure

Tento kurz vás provede kroky pro vytvoření **Kanálu**, který je nakonfigurován pro průchozí doručování. 

Služba Azure Media Services na portálu Azure je aktuálně ve verzi preview.   


##Požadavky

K dokončení kurzu potřebujete následující:

- Účet Azure. Podrobnosti najdete v článku [Bezplatná zkušební verze Azure](https://azure.microsoft.com/pricing/free-trial/). 
- Účet Media Services. Pokud chcete vytvořit účet Media Services, přečtěte si článek [Jak vytvořit účet Media Services](media-services-create-account.md).
- Webová kamera. Například [kodér Telestream Wirecast](http://www.telestream.net/wirecast/overview.htm).

Důrazně doporučujeme přečtení následujících článků:

- [Podpora RTMP ve službě Azure Media Services a kodéry pro kódování v reálném čase.](https://azure.microsoft.com/blog/2014/09/18/azure-media-services-rtmp-support-and-live-encoders/)
- [Přehled pro živé streamování pomocí služby Azure Media Services](media-services-manage-channels-overview.md)
- [Živé streamování s místními kodéry, které vytvářejí datové proudy s více přenosovými rychlostmi.](media-services-live-streaming-with-onprem-encoders.md)


##<a id="scenario"></a>Běžný scénář živého streamování

Následující kroky popisují úlohy, které jsou běžně součástí procesu vytváření aplikací pro živé streamování, které používají kanály, nakonfigurované pro průchozí doručování. Tento kurz ukazuje, jak vytvořit a spravovat průchozí kanál a živé události.

1. Připojte k počítači videokameru. Spusťte a nakonfigurujte místní kodér pro kódování v reálném čase, který produkuje RTMP s více přenosovými rychlostmi nebo fragmentovaný proud MP4. Další informace najdete v článku [Podpora RTMP ve službě Azure Media Services a kodéry pro kódování v reálném čase](http://go.microsoft.com/fwlink/?LinkId=532824).
    
    Tento krok můžete provést i po vytvoření kanálu.

1. Vytvořit a spustit průchozí kanál.
1. Načtěte adresu URL ingestování kanálu. 

    Adresu URL ingestování používá kodér po kódování v reálném čase k odesílání datového proudu do kanálu.
1. Načtěte adresu URL náhledu kanálu. 

    Tuto adresu URL můžete použít, když chcete ověřit, jestli kanál správně přijímá proud živého vysílání.

3. Vytvořte živou událost nebo program. 

    Na portálu Azure se při vytváření živé události vytvoří také asset. 
      
    >[AZURE.NOTE]Zajistěte, abyste na koncovém bodu streamování (ze kterého chcete streamovat obsah) měli aspoň jednu jednotku rezervovanou pro streaming.
1. Jakmile budete připraveni začít streamovat a archivovat, spusťte událost nebo program.
2. Volitelně můžete dát kodéru pro kódování v reálném čase signál, aby spustil reklamu. Reklama bude vložena do výstupního proudu.
1. Kdykoli budete chtít zastavit streamování a archivaci události, zastavte událost nebo program.
1. Odstraňte událost nebo program (volitelně můžete odstranit i asset).     

>[AZURE.IMPORTANT] Zkontrolujte [Živé streamování s místními kodéry, které vytváří datové proudy s více přenosovými rychlostmi](media-services-live-streaming-with-onprem-encoders.md) Další informace o konceptech a důležité informace, týkající se živého streamování s místními kodéry a průchozími kanály.

##Zobrazení upozornění a chyb

Pokud chcete zobrazit upozornění a chyby, vytvořené na portálu Azure, klikněte na ikonu oznámení.

![Oznámení](./media/media-services-portal-passthrough-get-started/media-services-notifications.png)

##Konfigurace koncových bodů streamování 

Služba Media Services poskytuje dynamické balení, což vám umožní dodávat vaše soubory MP4 s více přenosovými rychlostmi ve formátech streamování MPEG DASH, HLS, Smooth Streaming nebo HDS, aniž byste je museli znovu zabalit do těchto formátů streamování. Při dynamickém balení stačí uložit (a platit) soubory pouze v jednom úložném formátu a služba Media Services bude sestavovat a dodávat vhodný formát streamování v reakci na požadavky klientů.

Pokud chcete využít výhody dynamického balení, získejte alespoň jednu jednotku streamování pro koncový bod streamování, ze kterého plánujete obsah doručovat.  

Pokud chcete vytvořit a změnit počet jednotek rezervovaných pro streaming, postupujte takto:

1. V okně **Nastavení** klikněte na **Koncové body streamování**. 

2. Klikněte na výchozí koncový bod streamování. 

    Zobrazí se okno **VÝCHOZÍ KONCOVÝ BOD STREAMOVÁNÍ – PODROBNOSTI**.

3. Pokud chcete zadat počet jednotek streamování, posuňte jezdcem **Jednotky streamování**.

    ![Jednotky streamování](./media/media-services-portal-vod-get-started/media-services-streaming-units.png)

4. Kliknutím na tlačítko **Uložit** uložte provedené změny.

    >[AZURE.NOTE]Přidělení jakýchkoli nových jednotek může trvat až 20 minut.
    
##Vytvoření a spuštění průchozího kanálu.

Kanál je přidružený k událostem a programům, které vám umožňují řídit publikování a ukládání segmentů v živém datovém proudu. Kanály spravují události. 
    
Nastavením délky **archivačního okna** můžete určit počet hodin, po které chcete uchovávat zaznamenaný obsah programu. Tuto hodnotu můžete nastavit v rozmezí od 5 minut po 25 hodin. Délka archivačního okna také určuje maximální časový úsek, který můžou klienti prohledávat od aktuální živé pozice směrem zpět v čase. Programy můžou běžet po určenou dobu a obsah, který se do délky okna nevejde, bude vždy zahozen. Hodnota této vlastnosti také určuje, jak dlouho můžou růst manifesty klientů.

Každá událost je přidružena k assetu. Pokud chcete publikovat událost, musíte pro přidružený asset vytvořit lokátor OnDemand. Tento lokátor vám umožní sestavit adresu URL pro streamování, kterou potom poskytnete svým klientům.

Kanál podporuje až tři současně spuštěné programy, takže si můžete vytvořit několik archivů stejného příchozího datového proudu. To vám umožní podle potřeby publikovat a archivovat různé části události. Požadavek vaší firmy může být například takový, že chcete archivovat 6 hodin programu, ale vysílat jenom posledních 10 minut. K tomu potřebujete vytvořit dva současně spuštěné programy. Jeden program nastavíte, aby archivoval 6 hodin události, ale tento program nebudete publikovat. Druhý program nastavíte, aby archivoval 10 minut a tento program budete publikovat.

Neměli byste znovu používat existující živé události. Místo toho vytvořte a spusťte novou událost pro každou jednotlivou událost.

Jakmile budete připraveni začít streamovat a archivovat, spusťte událost. Kdykoli budete chtít zastavit streamování a archivaci události, zastavte program. 

Pokud chcete archivovaný obsah odstranit, zastavte a odstraňte událost a potom odstraňte přidružený asset. Asset nemůžete odstranit, pokud ho událost používá. Nejdřív odstraňte událost. 

I po zastavení a odstranění události můžou uživatelé streamovat archivovaný obsah jako video na vyžádání, a to tak dlouho, dokud asset neodstraníte.

Pokud chcete archivovaný obsah zachovat, ale nechcete ho zpřístupňovat pro streamování, odstraňte lokátor streamování.

###Použití portálu k vytvoření kanálu 

Tato část ukazuje způsob použití funkce **Rychle vytvořit** k vytvoření průchozího kanálu.

Další podrobnosti o průchozích kanálech v [Živé streamování s místními kodéry, které vytvářejí proudy s více přenosovými rychlostmi.](media-services-live-streaming-with-onprem-encoders.md).

1. V okně **Nastavení** klikněte na **Živé streamování**. 

    ![Začínáme](./media/media-services-portal-passthrough-get-started/media-services-getting-started.png)
    
    Zobrazí se okno **Živé vysílání datového proudu**.

3. Klikněte na **Rychle vytvořit** a vytvořte průchozí kanál s protokolem ingestování RTMP.

    Zobrazí se okno **VYTVOŘIT NOVÝ KANÁL**.
4. Zadejte název nového kanálu a klikněte na **Vytvořit**. 

    Tím vytvoříte průchozí kanál s protokolem ingestování RTMP.

    Kanál také přidá, spustí a publikuje výchozí živou událost nebo program. Tato událost má nakonfigurovanou délku archivačního okna 8 hodin. 

    Když chcete přidat další události, stiskněte tlačítko **Živá událost**.

##Získání ingestovaných adres URL

Po vytvoření kanálu můžete získat ingestované adresy URL, které poskytnete kodéru pro kódování v reálném čase. Kodér tyto adresy URL používá ke vkládání živého proudu.

![Vytvořeno](./media/media-services-portal-passthrough-get-started/media-services-channel-created.png)

##Sledování události

Pokud chcete sledovat událost, klikněte na tlačítko **Sledovat** na portálu Azure nebo zkopírujte adresu URL streamování a použijte přehrávač dle svého výběru. 
 
![Vytvořeno](./media/media-services-portal-passthrough-get-started/media-services-default-event.png)

Při zastavení se živá událost automaticky převede na obsah na vyžádání.

##Vyčištění

Další podrobnosti o průchozích kanálech v [Živé streamování s místními kodéry, které vytvářejí proudy s více přenosovými rychlostmi.](media-services-live-streaming-with-onprem-encoders.md).

- Kanál se dá zastavit jenom v případě, že byly zastaveny všechny jeho události nebo programy.  Zastavený kanál zastaví narůstání poplatků. Když bude potřeba kanál znovu spustit, bude mít stejnou ingestovanou adresu URL, takže nebude nutné kodér znovu konfigurovat.
- Kanál se dá odstranit jenom v případě, že byly odstraněny všechny jeho živé události.

##Zobrazení archivovaného obsahu

I po zastavení a odstranění události můžou uživatelé streamovat archivovaný obsah jako video na vyžádání, a to tak dlouho, dokud asset neodstraníte. Asset nemůžete odstranit, pokud ho událost používá. Nejdřív odstraňte událost. 

Ke správě zvolených prostředků vyberte **Nastavení** a klikněte na tlačítko **Prostředky**.

![Prostředky](./media/media-services-portal-passthrough-get-started/media-services-assets.png)

##Mapy kurzů k Media Services

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##Poskytnutí zpětné vazby

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]



<!--HONumber=Aug16_HO4-->


