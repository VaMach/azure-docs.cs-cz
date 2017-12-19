---
title: "Škálování zpracování média pomocí portálu Azure | Microsoft Docs"
description: "V tomto kurzu vás provede jednotlivými kroky škálování média zpracování pomocí portálu Azure."
services: media-services
documentationcenter: 
author: Juliako
manager: cfowler
editor: 
ms.assetid: e500f733-68aa-450c-b212-cf717c0d15da
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/04/2017
ms.author: juliako
ms.openlocfilehash: d2312803a4471e207d3696ca8350a86e3c4761e6
ms.sourcegitcommit: 68aec76e471d677fd9a6333dc60ed098d1072cfc
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 12/18/2017
---
# <a name="change-the-reserved-unit-type"></a>Změna typu rezervované jednotky
> [!div class="op_single_selector"]
> * [.NET](media-services-dotnet-encoding-units.md)
> * [Azure Portal](media-services-portal-scale-media-processing.md)
> * [REST](https://docs.microsoft.com/rest/api/media/operations/encodingreservedunittype)
> * [Java](https://github.com/southworkscom/azure-sdk-for-media-services-java-samples)
> * [PHP](https://github.com/Azure/azure-sdk-for-php/tree/master/examples/MediaServices)
> 
> 

> [!NOTE]
> Získat nejnovější verzi sady Java SDK a začít s vývojem v jazyce Java, najdete v tématu [Začínáme s klientem nástroje Java SDK pro Media Services](https://docs.microsoft.com/azure/media-services/media-services-java-how-to-use). <br/>
> Chcete-li stáhnout nejnovější PHP SDK pro Media Services, vyhledejte verze 0.5.7 balíčku Microsoft/WindowAzure v [Packagist úložiště](https://packagist.org/packages/microsoft/windowsazure#v0.5.7).  

## <a name="overview"></a>Přehled

Účet Media Services je přidružený k typu rezervované jednotky, který určuje rychlost zpracování vašich úloh zpracování médií. Můžete si vybrat mezi následujícími typy rezervovaných jednotek: **S1**, **S2** nebo **S3**. Například stejná úloha kódování bude rychlejší, když použijete typ rezervované jednotky **S2**, než kdybyste použili typ **S1**.

Kromě určení typu rezervované jednotky můžete určit, že chcete účet zřídit s **rezervovanými jednotkami** (RU). Počet zřízených RU určuje počet úloh médií, které je možné v daném účtu zpracovávat současně.

>[!NOTE]
>RU fungují pro paralelní provádění veškerého zpracování médií, včetně úloh indexování pomocí Azure Media Indexeru. Ale na rozdíl od kódování se úlohy indexování s rychlejšími rezervovanými jednotkami nezpracovávají rychleji.

> [!IMPORTANT]
> Projděte si [přehled](media-services-scale-media-processing-overview.md) tématu, které chcete získat další informace o škálování média zpracování tématu.
> 
> 

## <a name="scale-media-processing"></a>Škálování zpracování médií
Chcete-li změnit typ jednotku rezervovanou a počet jednotek rezervovaných, postupujte takto:

1. Na webu [Azure Portal](https://portal.azure.com/) zvolte účet Azure Media Services.
2. V **nastavení** vyberte **jednotky rezervované pro média**.
   
    Chcete-li změnit počet jednotek rezervovaných pro vybranou jednotku rezervovanou typ, použijte **obsluhovat jednotky média** posuvníku.
   
    Chcete-li změnit **vyhrazený typ jednotky**, stiskněte klávesu S1, S2 nebo S3.
   
    ![Stránka procesorů](./media/media-services-portal-scale-media-processing/media-services-scale-media-processing.png)
3. Stisknutím tlačítka ULOŽIT uložte provedené změny.
   
    Nové vyhrazené jednotky jsou přiděleny po stisknutí klávesy uložit.

## <a name="next-steps"></a>Další kroky
Prohlédněte si mapy kurzů k Media Services.

[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Poskytnutí zpětné vazby
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

