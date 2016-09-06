<properties
    pageTitle=" Vytvoření účtu Azure Media Services pomocí webu Azure Portal | Microsoft Azure"
    description="Tento kurz vás provede kroky pro vytvoření účtu služby Azure Media Services pomocí webu Azure Portal."
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
    ms.date="08/29/2016"
    ms.author="juliako"/>


# Vytvoření účtu Azure Media Services pomocí webu Azure Portal

> [AZURE.NOTE] K dokončení tohoto kurzu potřebujete mít účet Azure. Podrobnosti najdete v článku [Bezplatná zkušební verze Azure](https://azure.microsoft.com/pricing/free-trial/). 

Azure Portal nabízí rychlou možnost vytvoření účtu Azure Media Services (AMS). Účet můžete použít pro přístup ke službě Media Services, která vám umožní ukládat, šifrovat, kódovat, spravovat a streamovat mediální obsah v Azure. V okamžiku vytvoření účtu Media Services si současně vytvoříte i přidružený účet úložiště (nebo použijete existující) ve stejné zeměpisné oblasti jako účet Media Services.

Tento článek popisuje některé obvyklé koncepty a ukazuje jak vytvořit účet Media Services pomocí webu Azure Portal.

## Koncepty

Přístup ke službě Media Services vyžaduje dva přidružené účty:

- Účet Media Services. Váš účet umožňuje přístup k sadě cloudových služeb Media Services, které jsou dostupné v Azure. Účet Media Services neukládá samotný mediální obsah. Místo toho na vašem účtu ukládají metadata o mediálním obsahu a úlohách zpracování médií v účtu. Při vytváření účtu vybíráte dostupnou oblast služby Media Services. Oblast, kterou vyberete, je datové centrum, které ukládá záznamy metadat vašeho účtu.

    Dostupné oblasti služby Media Services (AMS) zahrnují následující: Severní Evropa, Západní Evropa, Západ USA, Východ USA, Jihovýchodní Asie, Východní Asie, Japonsko – západ, Japonsko – východ. Služba Media Services nepoužívá skupiny vztahů.
    
    AMS je nyní dostupná také v následujících datových centrech: Brazílie – jih, Indie – západ, Indie – jih a Indie – střed. V současnosti můžete web Azure Portal používat k vytvoření účtů Media Service a k provádění různých úloh popsaných tady. Funkce Live Encoding ale v těchto datových center není povolená. Kromě toho nejsou v těchto datových centrech dostupné všechny typy jednotek rezervovaných pro kódování.
    
    - Brazílie – jih: Dostupné jsou jenom jednotky rezervované pro kódování typu Standard a Basic.
    - Indie – západ, Indie – jih, pro mediální soubory a musí být umístěný ve stejné zeměpisné oblasti jako účet Media Services. Při vytváření účtu Media Services můžete zvolit buď stávající účet úložiště ve stejné oblasti, nebo můžete vytvořit nový účet úložiště ve stejné oblasti. Pokud odstraníte účet Media Services, objekty blob v souvisejícím účtu úložiště odstraněny nebudou.

## Vytvoření účtu AMS

Postup v této části ukazuje, jak vytvořit účet AMS.

1. Přihlaste se na [Azure Portal](https://portal.azure.com/).
2. Klikněte na **+Nový** > **Média a CDN** > **Media Services**.

    ![Media Services – vytvoření](./media/media-services-portal-vod-get-started/media-services-new1.png)

3. V okně **VYTVOŘIT ÚČET MEDIA SERVICES** zadejte požadované hodnoty.

    ![Media Services – vytvoření](./media/media-services-portal-vod-get-started/media-services-new3.png)
    
    1. Do pole **Název účtu** zadejte název nového účtu AMS. Název účtu Media Services musí obsahovat jenom malá písmena a čísla, nesmí obsahovat mezery a musí mít délku 3 až 24 znaků.
    2. V poli Předplatné si vyberte z různých předplatných Azure, ke kterým máte přístup.
    
    2. V poli **Skupina prostředků** vyberte nový nebo existující prostředek.  Skupina prostředků je kolekce prostředků, které sdílejí životní cyklus, oprávnění a zásady. Další informace najdete [tady](resource-group-overview.md#resource-groups).
    3. V poli **Umístění** vyberte zeměpisnou oblast, která se bude používat k ukládání médií a záznamů metadat pro váš účet Media Services. Tato oblast se bude používat ke zpracování a streamování vašeho média. V rozevíracím seznamu se vám zobrazí pouze ty oblasti Media Services, které jsou dostupné. 
    
    3. V poli **Účet úložiště** vyberte účet úložiště, který bude sloužit jako úložiště objektů blob mediálního obsahu z vašeho účtu Media Services. Můžete vybrat existující účet úložiště ve stejné zeměpisné oblasti jako váš účet Media Services, nebo můžete vytvořit účet úložiště. Nový účet úložiště bude vytvořen ve stejné oblasti. Pro názvy účtů úložiště platí stejná pravidla jako pro názvy účtů Media Services.

        Další informace o ukládání a úložištích najdete [tady](storage-introduction.md).

    4. Zaškrtněte **Připnout na řídicí panel**, abyste viděli průběh nasazení účtu.
    
7. Klikněte na tlačítko **Vytvořit** dole na formuláři.

    Po úspěšném vytvoření účtu se stav změní na **Spuštěno**. 

    ![Nastavení Media Services](./media/media-services-portal-vod-get-started/media-services-settings.png)

    Ke správě vašeho účtu AMS (například nahrávání videí, kódování assetů, sledovat průběhu úloh) použijte okno **Nastavení**.

## Správa klíčů

Název účtu a primární klíč budete potřebovat pro přístup k účtu Media Services prostřednictvím programového kódu.

1. Na webu Azure Portal vyberte svůj účet. 

    Okno **Nastavení** se zobrazí na pravé straně. 

2. V okně **Nastavení** vyberte **Klíče**. 

    Zobrazí se okno **Správa klíčů**, které ukazuje název účtu a primární a sekundární klíče. 
3. Stisknutím tlačítka kopírování zkopírujete hodnoty.
    
    ![Klíče služby Media Services](./media/media-services-portal-vod-get-started/media-services-keys.png)

## Další kroky

Soubory teď můžete nahrát do účtu AMS. Další informace najdete v tématu [Nahrání souborů](media-services-portal-upload-files.md).

## Mapy kurzů ke službě Media Services

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##Poskytnutí zpětné vazby

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]





<!--HONumber=ago16_HO5-->


