---
title: "Nejčastější dotazy souvisí s využitím rozhraní API | Microsoft Docs"
description: "Seznam zásobník Azure měřidla, porovnání Azure využití rozhraní API, doba využití a hlášené čas kódy chyb."
services: azure-stack
documentationcenter: 
author: brenduns
manager: femila
editor: 
ms.assetid: 847f18b2-49a9-4931-9c09-9374e932a071
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/05/2018
ms.author: alfredop
ms.openlocfilehash: 9ec0b0456a0e7bc5f86f33981d4be6289f5fbc71
ms.sourcegitcommit: 1d423a8954731b0f318240f2fa0262934ff04bd9
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/05/2018
---
# <a name="frequently-asked-questions-in-azure-stack-usage-api"></a>Nejčastější dotazy v zásobníku Azure využití rozhraní API
Tento článek obsahuje odpovědi na některé nejčastější dotazy týkající se rozhraní API služby Azure zásobníku využití.

## <a name="what-meter-ids-can-i-see"></a>Jaké měření ID můžete zobrazit?
Využití je uvedená pro následující zprostředkovatele prostředků.

| **Poskytovatel prostředků** | **ID měření** | **Název měřidla** | **Jednotka** | **Další informace** |
| --- | --- | --- | --- | --- |
| **Síť** |F271A8A388C44D93956A063E1D2FA80B |Použití statických IP adres |IP adresy| Počet IP adres používat |
| |9E2739BA86744796B465F64674B822BA |Použití dynamických IP adres |IP adresy| Počet IP adres používat |
| **Úložiště** |B4438D5D-453B-4EE1-B42A-DC72E377F1E4 |TableCapacity |GB\*hodin |Celková kapacita spotřebovávají tabulky |
| |B5C15376-6C94-4FDD-B655-1A69D138ACA3 |PageBlobCapacity |GB\*hodin |Celková kapacita spotřebovávají objekty BLOB stránky |
| |B03C6AE7-B080-4BFA-84A3-22C800F315C6 |QueueCapacity |GB\*hodin |Celková kapacita spotřebovávají fronty |
| |09F8879E-87E9-4305-A572-4B7BE209F857 |BlockBlobCapacity |GB\*hodin |Celková kapacita spotřebovávají objekty BLOB bloku |
| |B9FF3CD0-28AA-4762-84BB-FF8FBAEA6A90 |TableTransactions |Počet požadavku v 10 000's |Tabulka žádosti o služby (v 10, 000's) |
| |50A1AEAF-8ECA-48A0-8973-A5B3077FEE0D |TableDataTransIn |Příchozí přenos dat v GB |Příchozí data služby Table v GB |
| |1B8C1DEC-EE42-414B-AA36-6229CF199370 |TableDataTransOut |Odchozí v GB |Odchozí data služby Table v GB |
| |43DAF82B-4618-444A-B994-40C23F7CD438 |BlobTransactions |Počet požadavků za 10 000's |Žádosti o služby objektů BLOB (v 10, 000's) |
| |9764F92C-E44A-498E-8DC1-AAD66587A810 |BlobDataTransIn |Příchozí přenos dat v GB |Příchozí data služby objektů BLOB v GB |
| |3023FEF4-ECA5-4D7B-87B3-CFBC061931E8 |BlobDataTransOut |Odchozí v GB |Odchozí data služby objektů BLOB v GB |
| |EB43DD12-1AA6-4C4B-872C-FAF15A6785EA |QueueTransactions |Počet požadavků za 10 000's |Fronty žádostí o služby (v 10, 000's) |
| |E518E809-E369-4A45-9274-2017B29FFF25 |QueueDataTransIn |Příchozí přenos dat v GB |Příchozí data služby fronty v GB |
| |DD0A10BA-A5D6-4CB6-88C0-7D585CEF9FC2 |QueueDataTransOut |Odchozí v GB |Odchozí data fronty služby v GB |
| **Compute** |FAB6EB84-500B-4A09-A8CA-7358F8BBAEA5 |Základní virtuální počítač velikost hodiny |Virtuální základní minut | Počet jader virtuální krát minut, po které byl virtuální počítač spuštěn |
| |9CD92D4C-BAFD-4492-B278-BEDC2DE8232A |Velikost virtuálního počítače s Windows – hodiny |Virtuální základní minut | Počet jader virtuální krát minut, po které byl virtuální počítač spuštěn |
| |6DAB500F-A4FD-49C4-956D-229BB9C8C793 |Virtuální počítač velikost hodiny |Virtuální počítač hodiny |Zaznamená základní a systému Windows virtuálního počítače. Se neupraví jader |
| **Key Vault** |EBF13B9F-B3EA-46FE-BF54-396E93D48AB4 |Transakce Key Vault | Počet požadavku v 10 000's| Počet přijatých Key Vault datové roviny požadavky REST API |
| **Aplikační služby** |190C935E-9ADA-48FF-9AB8-56EA1CF9ADAA  | App Service   | Virtuální základní hodiny  | Počet virtuálních jader používá ke spouštění služby app service |

## <a name="how-do-the-azure-stack-usage-apis-compare-to-the-azure-usage-apihttpsmsdnmicrosoftcomlibraryazure1ea5b323-54bb-423d-916f-190de96c6a3c-currently-in-public-preview"></a>Jak Azure zásobníku využití rozhraní API porovnání [rozhraní API pro Azure využití](https://msdn.microsoft.com/library/azure/1ea5b323-54bb-423d-916f-190de96c6a3c) (momentálně ve verzi public preview)?
* Rozhraní API klienta využití je konzistentní s Azure API, s jednou výjimkou: *showDetails* příznak aktuálně není podporována v zásobníku Azure.
* Rozhraní API poskytovatele využití se vztahuje pouze na Azure zásobníku.
* V současné době [RateCard API](https://msdn.microsoft.com/en-us/library/azure/mt219004.aspx) který je k dispozici v Azure, která není k dispozici v zásobníku Azure.

## <a name="what-is-the-difference-between-usage-time-and-reported-time"></a>Jaký je rozdíl mezi časem využití a vykazovaného časového?
Sestavy využití dat mají dva hlavní časové hodnoty:

* **Hlášený čas**. Čas, kdy k události využití zadáno systému využití
* **Doba využití**. Pokud byl prostředek Azure zásobníku uplynulý čas

Může se zobrazit nesoulad hodnoty pro využití čas a čas hlášené pro konkrétní využití událost. Zpoždění může být stejně dlouho jako několik hodin v jakémkoli prostředí.

V současné době můžete dotazovat pouze systémem *hlášené čas*.

## <a name="what-do-these-usage-api-error-codes-mean"></a>Co znamenají tyto kódy chyb využití rozhraní API?
| **Stavový kód protokolu HTTP** | **Kód chyby** | **Popis** |
| --- | --- | --- |
| / 400 – Chybný požadavek |*NoApiVersion* |*Verze rozhraní api* chybí parametr dotazu. |
| / 400 – Chybný požadavek |*InvalidProperty* |Vlastnost nebyla nalezena nebo má neplatnou hodnotu. Zprávy v kód chyby v textu odpovědi identifikuje chybí vlastnost. |
| / 400 – Chybný požadavek |*RequestEndTimeIsInFuture* |Hodnota *ReportedEndTime* je v budoucnu. Hodnoty v budoucnu nejsou povolené pro tento argument. |
| / 400 – Chybný požadavek |*SubscriberIdIsNotDirectTenant* |Volání API poskytovatele použila ID odběru, který není platný volajícího klienta. |
| / 400 – Chybný požadavek |*SubscriptionIdMissingInRequest* |Chybí ID předplatného volajícího. |
| / 400 – Chybný požadavek |*InvalidAggregationGranularity* |Bylo vyžadováno agregaci neplatný členitosti. Platné hodnoty jsou denní a po hodinách. |
| 503 |*ServiceUnavailable* |Opakovatelná chyba došlo k chybě, protože služba je zaneprázdněná nebo volání je omezené. |

## <a name="next-steps"></a>Další kroky
[Zákazník fakturace a vrácení peněz v Azure zásobníku](azure-stack-billing-and-chargeback.md)

[Využití prostředků poskytovatele rozhraní API](azure-stack-provider-resource-api.md)

[Využití prostředků rozhraní API klienta](azure-stack-tenant-resource-usage-api.md)
