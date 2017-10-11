---
title: "Přehled služby Azure Media Services | Dokumentace Microsoftu"
description: "Toto téma nabízí přehled Azure Media Services."
services: media-services
documentationcenter: 
author: Juliako
manager: cfowler
editor: 
ms.assetid: 7a5e9723-c379-446b-b4d6-d0e41bd7d31f
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: hero-article
ms.date: 07/04/2017
ms.author: juliako;anilmur
ms.openlocfilehash: 2a175aed40b9775d9a4de6877eb3467b43819568
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="azure-media-services-overview"></a>Přehled služby Azure Media Services 

Microsoft Azure Media Services je rozšiřitelná cloudová platforma, která vývojářům umožňuje vytvářet škálovatelné aplikace pro správu a doručování médií. Služba Media Services využívá rozhraní REST API, které vám umožní bezpečně nahrávat, ukládat, kódovat a balit obsah (video nebo zvuk) doručovaný na vyžádání i v živě streamovaný různým klientům (například do televizí, počítačů a mobilních zařízení).

Pomocí Media Services můžete vytvářet pracovní postupy od začátku až do konce. V některých částech pracovního postupu můžete použít komponenty třetích stran. Můžete například kódovat pomocí kodéru třetí strany. Potom obsah nahrajete, zabezpečíte, zabalíte a doručíte pomocí služby Media Services.

Svůj obsah můžete streamovat živě nebo doručovat na vyžádání. Téma obsahuje i odkazy na další související témata.

## <a name="media-services-learning-paths"></a>Mapy kurzů ke službě Media Services
Mapu kurzů k AMS můžete zobrazit tady:

* [Pracovní postup živého streamování AMS](https://azure.microsoft.com/documentation/learning-paths/media-services-streaming-live/)
* [Pracovní postup streamování AMS na vyžádání](https://azure.microsoft.com/documentation/learning-paths/media-services-streaming-on-demand/)

## <a name="prerequisites"></a>Požadavky

Pokud chcete začít používat Azure Media Services, potřebujete následující:

* Účet Azure. Pokud účet nemáte, můžete si během několika minut vytvořit bezplatný zkušební účet. Podrobnosti najdete v článku [Bezplatná zkušební verze Azure](https://azure.microsoft.com).
* Účet Azure Media Services. Další informace najdete v článku o [vytvoření účtu](media-services-portal-create-account.md).
* (Volitelné) Nastavte vývojové prostředí. Jako vývojové prostředí si zvolte .NET nebo REST API. Další informace najdete v článku o [nastavení prostředí](media-services-dotnet-how-to-use.md).

    Seznamte se také s postupem [připojení prostřednictvím kódu programu k rozhraní API pro AMS](media-services-use-aad-auth-to-access-ams-api.md).
* Koncový bod streamování Standard nebo Premium ve spuštěném stavu.  Další informace najdete v článku o [správě koncových bodů streamování](media-services-portal-manage-streaming-endpoints.md).

## <a name="sdks-and-tools"></a>Sady SDK a nástroje

Pokud chcete vytvořit řešení Media Services, můžete použít následující pomůcky:

* [Rozhraní REST API služby Media Services](https://docs.microsoft.com/rest/api/media/operations/azure-media-services-rest-api-reference)
* Jednu z dostupných klientských sad SDK:
    * [Azure Media Services SDK pro .NET](https://github.com/Azure/azure-sdk-for-media-services),
    * [Azure SDK pro Javu](https://github.com/Azure/azure-sdk-for-java),
    * [Azure PHP SDK](https://github.com/Azure/azure-sdk-for-php),
    * [Azure Media Services pro Node.js](https://github.com/michelle-becker/node-ams-sdk/blob/master/lib/request.js) (Jedná se o verzi sady SDK, kterou nevytvořil Microsoft. Spravuje ji komunita a aktuálně nemá 100% pokrytí rozhraní API pro AMS.)
* Existující nástroje:
    * [Azure Portal](https://portal.azure.com/)
    * [Azure-Media-Services-Explorer](https://github.com/Azure/Azure-Media-Services-Explorer) (Azure Media Services Explorer (AMSE) je aplikace napsaná v jazyce Winforms/C# pro Windows.)

## <a name="concepts-and-overview"></a>Koncepty a přehled
Informace o konceptech Azure Media Services najdete v článku [Koncepty](media-services-concepts.md).

Řada návodů, které vás seznámí se všemi hlavními součástmi Azure Media Services, najdete v článku o [podrobných kurzech pro Azure Media Services](https://docs.com/fukushima-shigeyuki/3439/english-azure-media-services-step-by-step-series). Tato řada návodů obsahuje přehled konceptů a využívá nástroj AMSE k předvádění úloh AMS. Nástroj AMSE je nástrojem systému Windows. Tento nástroj podporuje většinu úloh, které můžete provádět prostřednictvím kódu programu se sadami [AMS SDK pro .NET](https://github.com/Azure/azure-sdk-for-media-services), [Azure SDK pro Javu](https://github.com/Azure/azure-sdk-for-java), nebo  [Azure PHP SDK](https://github.com/Azure/azure-sdk-for-php).

## <a name="supported-scenarios-and-availability-of-media-services-across-data-centers"></a>Podporované scénáře a dostupnost služby Media Services v datových centrech

Podrobné informace najdete v tématu [Scénáře a dostupnost funkcí a služeb AMS v datových centrech](scenarios-and-availability.md).

## <a name="service-level-agreement-sla"></a>Smlouva SLA

* V případě služby Media Services Encoding zaručujeme 99,9% dostupnosti transakcí REST API.
* V případě streamování budeme úspěšně obsluhovat požadavky se zárukou 99,9% dostupnosti pro existující mediální obsah, pokud jste zakoupili koncový bod streamování Standard nebo Premium.
* V případě živých kanálů zaručujeme externí konektivitu minimálně po 99,9 % času.
* V případě Content Protection zaručujeme úspěšné plnění klíčových požadavků minimálně po 99,9 % času.
* V případě indexeru po 99,9 % času úspěšně obsluhujeme požadavky úloh indexeru, které zpracovává jednotka rezervovaná pro kódování.

Další informace najdete v článku [Microsoft Azure SLA](https://azure.microsoft.com/support/legal/sla/).

Informace o dostupnosti v datových centrech najdete v části [Dostupnost](scenarios-and-availability.md#availability).

## <a name="support"></a>Podpora

[Podpora Azure](https://azure.microsoft.com/support/options/) nabízí možnosti odborné pomoci pro Azure včetně služby Media Services.

## <a name="provide-feedback"></a>Poskytnutí zpětné vazby

[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]
