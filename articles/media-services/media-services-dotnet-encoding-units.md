---
title: "Škálování média zpracování přidáním kódování jednotky – Azure |  Microsoft Docs"
description: "Další postup přidání kódování jednotky s rozhraním .NET"
services: media-services
documentationcenter: 
author: juliako
manager: cfowler
editor: 
ms.assetid: 33f7625a-966a-4f06-bc09-bccd6e2a42b5
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/16/2017
ms.author: juliako;milangada;
ms.openlocfilehash: 4a1bf91c3b3e9dd4d42d84f205a9bc07a71525c2
ms.sourcegitcommit: b5c6197f997aa6858f420302d375896360dd7ceb
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 12/21/2017
---
# <a name="how-to-scale-encoding-with-net-sdk"></a>Jak škálovat kódování pomocí sady .NET SDK
> [!div class="op_single_selector"]
> * [Azure Portal](media-services-portal-scale-media-processing.md)
> * [.NET](media-services-dotnet-encoding-units.md)
> * [REST](https://docs.microsoft.com/rest/api/media/operations/encodingreservedunittype)
> * [Java](https://github.com/southworkscom/azure-sdk-for-media-services-java-samples)
> * [PHP](https://github.com/Azure/azure-sdk-for-php/tree/master/examples/MediaServices)
> 
> 

> [!NOTE]
> Pokud chcete získat nejnovější verzi sady Java SDK a začít s vývojem v jazyce Java, přečtěte si článek [Začínáme s klientskou sadou Java SDK pro Media Services](https://docs.microsoft.com/azure/media-services/media-services-java-how-to-use). <br/>
> Pokud si chcete stáhnout nejnovější sadu PHP SDK pro službu Media Services, najděte si v [úložišti Packagist](https://packagist.org/packages/microsoft/windowsazure#v0.5.7) balíček Microsoft/WindowsAzure verze 0.5.7.  

## <a name="overview"></a>Přehled
> [!IMPORTANT]
> Projděte si [přehled](media-services-scale-media-processing-overview.md) tématu, které chcete získat další informace o škálování média zpracování tématu.
> 
> 

Chcete-li změnit typ jednotku rezervovanou a počet jednotky rezervované pro kódování pomocí sady .NET SDK, postupujte takto:

    IEncodingReservedUnit encodingS1ReservedUnit = _context.EncodingReservedUnits.FirstOrDefault();
    encodingS1ReservedUnit.ReservedUnitType = ReservedUnitType.Basic; // Corresponds to S1
    encodingS1ReservedUnit.Update();
    Console.WriteLine("Reserved Unit Type: {0}", encodingS1ReservedUnit.ReservedUnitType);

    encodingS1ReservedUnit.CurrentReservedUnits = 2;
    encodingS1ReservedUnit.Update();

    Console.WriteLine("Number of reserved units: {0}", encodingS1ReservedUnit.CurrentReservedUnits);

## <a name="opening-a-support-ticket"></a>Otevření lístku podpory
Ve výchozím nastavení každých účtu Media Services můžete škálovat až 25 kódování a 5 na vyžádání jednotek rezervovaných pro streamování. Vyšší limit můžete požádat tak, že otevřete lístek podpory.

### <a name="open-a-support-ticket"></a>Otevřete lístek podpory
Chcete-li otevřít podporu lístku takto:

1. Klikněte na tlačítko [získat podporu](https://manage.windowsazure.com/?getsupport=true). Pokud nejste přihlášeni, budete vyzváni k zadání pověření.
2. Vyberte své předplatné.
3. V rámci podpory typu vyberte "Technical".
4. Kliknutím na tlačítko "Vytvoření lístku".
5. Vyberte "Azure Media Services" v seznamu produktu uvedené na další stránku.
6. Vyberte typ problému"", je vhodné pro váš problém.
7. Klikněte na tlačítko Pokračovat.
8. Postupujte podle pokynů na další stránce a potom zadejte podrobnosti o problému.
9. Klikněte na tlačítko Odeslat otevřete lístek.

## <a name="media-services-learning-paths"></a>Mapy kurzů ke službě Media Services
[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Poskytnutí zpětné vazby
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

