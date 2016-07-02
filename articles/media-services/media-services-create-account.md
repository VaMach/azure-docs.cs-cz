<properties
    pageTitle="Vytvoření účtu Media Services | Microsoft Azure"
    description="Popisuje postup vytvoření nového účtu Azure Media Services v Azure."
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
    ms.date="04/18/2016"
    ms.author="juliako"/>


# Vytvoření účtu Azure Media Services

> [AZURE.SELECTOR]
- [Portál](media-services-create-account.md)
- [PowerShell](media-services-manage-with-powershell.md)
- [REST](http://msdn.microsoft.com/library/azure/dn194267.aspx)


> [AZURE.NOTE] K dokončení tohoto kurzu potřebujete mít účet Azure. Podrobnosti najdete v článku [Bezplatná zkušební verze Azure](/pricing/free-trial/?WT.mc_id=A261C142F).
 
Portál Azure Classic nabízí rychlou možnost vytvoření účtu Azure Media Services. Účet můžete použít pro přístup ke službě Media Services, která vám umožní ukládat, šifrovat, kódovat, spravovat a streamovat mediální obsah v Azure. V okamžiku vytvoření účtu Media Services si současně vytvoříte i přidružený účet úložiště (nebo použijete existující) ve stejné zeměpisné oblasti jako účet Media Services.

Tento článek vysvětluje použití metody rychlého vytvoření k vytvoření nového účtu Media Services a jeho přidružení k účtu úložiště.

<a id="concepts"></a>
## Koncepty

Přístup ke službě Media Services vyžaduje dva přidružené účty:

-   **Účet Media Services**. Váš účet umožňuje přístup k sadě cloudových služeb Media Services, které jsou dostupné v Azure. Účet Media Services neukládá samotný mediální obsah. Místo toho na vašem účtu ukládají metadata o mediálním obsahu a úlohách zpracování médií v účtu. Při vytváření účtu vybíráte dostupnou oblast služby Media Services. Oblast, kterou vyberete, je datové centrum, které ukládá záznamy metadat vašeho účtu.

    Dostupné oblasti služby Media Services (AMS) zahrnují následující: Severní Evropa, Západní Evropa, Západ USA, Východ USA, Jihovýchodní Asie, Východní Asie, Japonsko – západ, Japonsko – východ. Služba Media Services nepoužívá skupiny vztahů.
    
    AMS je nyní dostupná také v následujících datových centrech: Brazílie – jih, Indie – západ, Indie – jih a Indie – střed. V současnosti můžete portál Azure Classic používat k [vytvoření účtů Media Service](media-services-create-account.md#create-a-media-services-account-using-quick-create) a k provádění různých úloh popsaných [tady](https://azure.microsoft.com/documentation/services/media-services/). Funkce Live Encoding ale v těchto datových center není povolená. Kromě toho nejsou v těchto datových centrech dostupné všechny typy jednotek rezervovaných pro kódování.
    
    - Brazílie – jih: Dostupné jsou jenom jednotky rezervované pro kódování typu Standard a Basic.
    - Indie – západ, Indie – jih, Indie – střed: Dostupné jsou jenom jednotky rezervované pro kódování typu Basic


-   **Přidružený účet úložiště**. Váš účet úložiště je účtem úložiště Azure, který je přidružený k vašemu účtu Media Services. Účet úložiště poskytuje úložiště objektů blob pro mediální soubory a musí být umístěný ve stejné zeměpisné oblasti jako účet Media Services. Při vytváření účtu Media Services můžete zvolit buď stávající účet úložiště ve stejné oblasti, nebo můžete vytvořit nový účet úložiště ve stejné oblasti. Pokud odstraníte účet Media Services, objekty blob v souvisejícím účtu úložiště odstraněny nebudou.

<a id="quick"></a>
## Vytvoření účtu ve službě Media Services pomocí metody rychlého vytvoření

1. Na stránce [Portál Azure Classic][] klikněte na **Nový**, **Media Service** a potom na **Rychlé vytvoření**.

![Rychlé vytvoření Media Services](./media/media-services-create-account/wams-QuickCreate.png)

2. Do pole **NÁZEV** zadejte název nového účtu. Název účtu Media Services musí obsahovat jenom malá písmena a čísla, nesmí obsahovat mezery a musí mít délku 3 až 24 znaků.

3. V poli **OBLAST** vyberte zeměpisnou oblast, která se bude používat k ukládání záznamů metadat pro váš účet Media Services. V rozevíracím seznamu se zobrazí pouze dostupné oblasti služby Media Services.

4. V poli **ÚČET ÚLOŽIŠTĚ** vyberte účet úložiště, který bude sloužit jako úložiště objektů blob mediálního obsahu z vašeho účtu Media Services. Můžete vybrat existující účet úložiště ve stejné zeměpisné oblasti jako váš účet Media Services, nebo můžete vytvořit nový účet úložiště. Nový účet úložiště bude vytvořen ve stejné oblasti.

5. Pokud jste vytvořili nový účet úložiště, do pole **NÁZEV NOVÉHO ÚČTU ÚLOŽIŠTĚ** zadejte název pro účet úložiště. Pro názvy účtů úložiště platí stejná pravidla jako pro názvy účtů Media Services.

6. V dolní části formuláře klikněte na tlačítko **Rychle vytvořit**.

V oblasti zpráv v dolní části okna můžete sledovat stav procesu.

Po úspěšném vytvoření účtu se stav změní na Aktivní. Otevře se stránka **Media Services** a zobrazí nový účet.

V dolní části stránky se zobrazí tlačítko **SPRÁVA KLÍČŮ**. Po kliknutí na toto tlačítko se zobrazí stránka s názvem účtu Media Services a primárním a sekundárním klíčem. Název účtu a primární klíč budete potřebovat pro přístup k účtu Media Services prostřednictvím programového kódu.

![Stránka Media Services](./media/media-services-create-account/wams-mediaservices-page.png)

Když dvakrát kliknete na název účtu, zobrazí se ve výchozím nastavení stránka **Rychlý start**. Tato stránka umožňuje provést některé úlohy správy, které jsou jinak dostupné i na jiných stránkách portálu. Z této stránky můžete například nahrát videosoubor, což lze provést i ze stránky **OBSAH**.

Kromě toho můžete zobrazit kód, který používá sada Azure Media Services SDK k provádění následujících úloh: nahrávání, kódování a publikování videa. Můžete kliknout na jeden z odkazů v části **PSANÍ KÓDU**, zkopírovat kód a použít ho v aplikaci.



##Mapy kurzů ke službě Media Services

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##Poskytnutí zpětné vazby

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]


## Další kroky

- [Začínáme s doručováním obsahu videa na vyžádání (VoD, Video-on-Demand) pomocí sady .NET SDK](media-services-dotnet-get-started.md)

- [Použití sad .NET SDK k vytvoření kanálů, které provádějí kódování v reálném čase z datového proudu s jednou přenosovou rychlostí na datový proud s více přenosovými rychlostmi](media-services-dotnet-creating-live-encoder-enabled-channel.md)

<!-- Reusable paths. -->

<!-- Anchors. -->
  [Koncepty]: #concepts
  [Než začnete]: #begin
  [Postup: Vytvoření účtu ve službě Media Services pomocí metody rychlého vytvoření]: #quick

<!-- URLs. -->
  [Instalační program webové platformy]: http://go.microsoft.com/fwlink/?linkid=255386

  [Portál Azure Classic]: http://manage.windowsazure.com/



<!--HONumber=Jun16_HO2-->


