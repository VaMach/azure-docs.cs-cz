---
title: "Azure Service Bus ověřování a autorizace | Microsoft Docs"
description: "Ověření aplikací k Service Bus pomocí sdíleného přístupového podpisu (SAS) ověřování."
services: service-bus-messaging
documentationcenter: na
author: sethmanheim
manager: timlt
editor: 
ms.assetid: 18bad0ed-1cee-4a5c-a377-facc4785c8c9
ms.service: service-bus-messaging
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/09/2017
ms.author: sethm
ms.openlocfilehash: b4b9d5d272bdb172f1d40db379a519a4f617550a
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/11/2017
---
# <a name="service-bus-authentication-and-authorization"></a>Ověřování a autorizace Service Bus

Aplikace získáte přístup k funkcím Azure Service Bus pomocí sdíleného přístupového podpisu (SAS) tokenu ověřování. S tokenem SAS, aplikace prezentovat token Service Bus, která byla podepsána pomocí symetrický klíč známé na vydavatel tokenu a Service Bus ("sdílené") a tento klíč je přímo přidružený k pravidlu přidělení konkrétní přístupová práva, jako jsou oprávnění k příjmu / naslouchání nebo odesílání zpráv. SAS pravidla jsou že buď konfigurovat na obor názvů, nebo přímo na entity jako fronta nebo téma, povolení pro řízení přístupu podrobné.

Tokeny SAS může být generována buď klientem služby Service Bus přímo, nebo může být generována některé zprostředkující token vydání koncový bod, který klient komunikuje se službou. Například systém může vyžadovat klientovi umožní zavolat Active Directory autorizace chráněné koncový bod webové služby k prokázání své identity a systému přístupová práva, a webovou službu pak vrátí odpovídající token Service Bus. Tento token SAS lze snadno vygenerovat pomocí zprostředkovateli tokenu služby Service Bus, který je zahrnutý v sadě SDK. 

> [!IMPORTANT]
> Pokud používáte Azure Active Directory řízení přístupu (také označované jako služby Řízení přístupu nebo služby ACS) ve spojení se Service Bus, Všimněte si, podporu pro tato metoda je nyní omezené a je potřeba migrovat aplikace pomocí SAS. Další informace najdete v tématu [tomto příspěvku na blogu](https://blogs.msdn.microsoft.com/servicebus/2017/06/01/upcoming-changes-to-acs-enabled-namespaces/).

## <a name="shared-access-signature-authentication"></a>Ověření pomocí sdíleného přístupového podpisu

[Ověřování SAS](service-bus-sas.md) vám umožní udělit přístup uživatelů k prostředkům služby Service Bus pomocí konkrétní práva. Ověřování SAS v Service Bus zahrnuje konfiguraci kryptografického klíče s přidružená práva na prostředku služby Service Bus. Klienti mohou získat přístup k danému prostředku pak prezentací token SAS, který se skládá z identifikátoru URI přistupuje prostředku a vypršení platnosti podepsaný pomocí nakonfigurovaný klíč.

Klíče pro SAS můžete konfigurovat v oboru názvů Service Bus. Klíč se vztahuje na všechny entity zasílání zpráv v daném oboru názvů. Můžete také nakonfigurovat klíče na témata a fronty Service Bus. SAS je podporováno také v [předávání přes Azure](../service-bus-relay/relay-authentication-and-authorization.md).

Chcete-li použít SAS, můžete konfigurovat [SharedAccessAuthorizationRule](/dotnet/api/microsoft.servicebus.messaging.sharedaccessauthorizationrule) objekt v oboru názvů, fronta nebo téma. Toto pravidlo se skládá z následujících elementů:

* *KeyName* identifikující pravidlo.
* *PrimaryKey* je kryptografický klíč používaný k přihlášení nebo ověření tokeny SAS.
* *Sekundární klíč* je kryptografický klíč používaný k přihlášení nebo ověření tokeny SAS.
* *Práva* představující kolekci naslouchání, odesílat, nebo spravovat udělena oprávnění.

Autorizační pravidla nakonfigurována na úrovni oboru názvů můžete udělit přístup k všechny entity v oboru názvů pro klienty s tokenů podepsaných pomocí odpovídající klíče. Až 12 toto povolení pravidla lze nastavit v oboru názvů Service Bus, fronta nebo téma. Ve výchozím nastavení [SharedAccessAuthorizationRule](/dotnet/api/microsoft.servicebus.messaging.sharedaccessauthorizationrule) se všemi právy je nakonfigurován pro každý obor názvů při první je zřízený.

Pro přístup k entity, klient vyžaduje token SAS, který je generována pomocí konkrétní [SharedAccessAuthorizationRule](/dotnet/api/microsoft.servicebus.messaging.sharedaccessauthorizationrule). SAS token je generována pomocí SHA256 HMAC řetězec prostředku, který se skládá z identifikátoru URI prostředku, na kterou je požadována přístupu a vypršení platnosti se kryptografický klíč přidružený k pravidlu autorizace.

Podpora ověřování SAS pro Service Bus je zahrnutý ve verzích sady Azure .NET SDK 2.0 nebo novější. Zahrnuje podporu pro SAS [SharedAccessAuthorizationRule](https://docs.microsoft.com/dotnet/api/microsoft.servicebus.messaging.sharedaccessauthorizationrule). Všechna rozhraní API, které přijímají připojovací řetězec jako parametr zahrnují podporu pro SAS připojovací řetězce.

## <a name="next-steps"></a>Další kroky

- Materiály [ověření sběrnice s podpisy sdíleného přístupu](service-bus-sas.md) další podrobnosti o tokenu SAS.
- [Obory názvů změny chcete-li povolena služba ACS.](https://blogs.msdn.microsoft.com/servicebus/2017/06/01/upcoming-changes-to-acs-enabled-namespaces/)
- Odpovídající informace o předávání přes Azure ověřování a autorizace najdete v tématu [Azure předávací ověřování a autorizace](../service-bus-relay/relay-authentication-and-authorization.md). 

