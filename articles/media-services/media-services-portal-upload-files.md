---
title: " Odeslání souborů do účtu Azure Media Services pomocí webu Azure Portal | Dokumentace Microsoftu"
description: "Tento kurz vás provede kroky pro odeslání souborů do účtu služby Azure Media Services pomocí webu Azure Portal."
services: media-services
documentationcenter: 
author: Juliako
manager: SyntaxC4
editor: 
ms.assetid: 3ad3dcea-95be-4711-9aae-a455a32434f6
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 08/07/2017
ms.author: juliako
ms.translationtype: HT
ms.sourcegitcommit: f5c887487ab74934cb65f9f3fa512baeb5dcaf2f
ms.openlocfilehash: 8b9a7a266259077d49a38d51d7f801f420cc6315
ms.contentlocale: cs-cz
ms.lasthandoff: 08/08/2017

---
# <a name="upload-files-into-a-media-services-account-using-the-azure-portal"></a>Odeslání souborů do účtu Azure Media Services pomocí webu Azure Portal
> [!div class="op_single_selector"]
> * [Azure Portal](media-services-portal-upload-files.md)
> * [.NET](media-services-dotnet-upload-files.md)
> * [REST](media-services-rest-upload-files.md)
> 
> [!NOTE]
> K dokončení tohoto kurzu potřebujete mít účet Azure. Podrobnosti najdete v článku [Bezplatná zkušební verze Azure](https://azure.microsoft.com/pricing/free-trial/). 
> 


Ve službě Media Services můžete digitální soubory nahrát do assetu. Asset může obsahovat video, zvuk, obrázky, kolekci miniatur, textové stopy a soubory titulků (a metadata o těchto souborech.) Jakmile soubory odešlete, bude váš obsah bezpečně uložen v cloudu pro další zpracování a streamování.


## <a name="upload-files"></a>Nahrání souborů

>[!NOTE]
>Maximální velikost souboru podporovaná při zpracování ve službě Media Services je omezená. Podrobnosti o omezení velikosti souboru najdete [tady](media-services-quotas-and-limitations.md).
>

1. Na webu [Azure Portal](https://portal.azure.com/) zvolte účet Azure Media Services.
2. V okně **Nastavení** klikněte na **Assety**.
   
    ![Nahrání souborů](./media/media-services-portal-vod-get-started/media-services-upload.png)
3. Klikněte na tlačítko **Nahrát**.
   
    Zobrazí se okno **Nahrát asset videa**.
   
   > [!NOTE]
   > Velikost souboru není nijak omezená.
   > 
   > 
4. Přejděte v počítači na požadovaného video, vyberte ho a klikněte na OK.  
   
    Spustí se nahrávání. Jeho průběh můžete sledovat pod názvem souboru.  

Po dokončení nahrávání se nový prostředek zobrazí v okně **Assety**. 

## <a name="next-steps"></a>Další kroky
Nyní můžete kódovat nahrané assety. Další informace najdete v tématu [Kódování assetů](media-services-portal-encode.md).

Můžete také použít službu Azure Functions k aktivaci úlohy kódování při příchodu souboru do nakonfigurovaného kontejneru. Další informace najdete v [této ukázce](https://azure.microsoft.com/resources/samples/media-services-dotnet-functions-integration/ ).

## <a name="media-services-learning-paths"></a>Mapy kurzů ke službě Media Services
[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Poskytnutí zpětné vazby
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]


