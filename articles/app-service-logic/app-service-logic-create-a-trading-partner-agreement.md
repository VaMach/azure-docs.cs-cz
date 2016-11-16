---
title: "Vytvoření smlouvy s obchodním partnerem v Azure App Service | Dokumentace Microsoftu"
description: "Vytváření smluv s obchodními partnery"
services: logic-apps
documentationcenter: .net,nodejs,java
author: rajram
manager: erikre
editor: 
ms.assetid: 319e46fa-fd81-4730-a742-768bf1676972
ms.service: logic-apps
ms.devlang: multiple
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: integration
ms.date: 08/23/2016
ms.author: rajram
translationtype: Human Translation
ms.sourcegitcommit: 219dcbfdca145bedb570eb9ef747ee00cc0342eb
ms.openlocfilehash: e41ac0e91bd66fbc7df08b4397e78377021fcbca


---
# <a name="creating-a-trading-partner-agreement"></a>Vytvoření smlouvy s obchodním partnerem
[!INCLUDE [app-service-logic-version-message](../../includes/app-service-logic-version-message.md)]

Obchodní partneři jsou entity, které se účastní komunikace B2B (business-to-business). Když dva partneři naváží vztah, označuje se jako *smlouva*. Definovaná smlouva se zakládá na komunikaci, které oba partneři chtějí dosáhnout, a je specifická pro protokol nebo přenos. Mezi protokoly a přenosy B2B, které Azure App Service podporuje, patří:

* AS2 (Applicability Statement 2)
* EDIFACT (United Nations/Electronic Data Interchange For Administration, Commerce and Transport (UN/EDIFACT))
* X12 (ASC X12)

### <a name="biztalk-api-apps-that-support-b2b-scenarios"></a>Aplikace API pro BizTalk, které podporují scénáře B2B
Následující aplikace API nabízejí tyto možnosti v bohatém a intuitivním prostředí na webu Azure Portal:

## <a name="biztalk-trading-partner-management-tpm"></a>BizTalk Trading Partner Management (TPM)
* Vytváření a správa partnerů, profilů a identit
* Ukládání a správa schémat EDI
* Ukládání a správa certifikátů (používá se v protokolu AS2)
* Vytváření a správa smluv AS2
* Vytváření a správa smluv EDIFACT (včetně dávkování na straně odesílání)
* Vytváření a správa smluv X12 (včetně dávkování na straně odesílání)

![][1]

## <a name="as2-connector"></a>AS2 Connector
* Vykonává smlouvy AS2 tak, jak jsou definovány v související instanci aplikace API TPM.
* Poskytuje informace o zpracování nebo sledování AS2 pro řešení potíží.

## <a name="biztalk-edifact"></a>BizTalk EDIFACT
* Vykonává smlouvy EDIFACT tak, jak jsou definovány v související instanci aplikace API TPM.
* Poskytuje informace o zpracování nebo sledování EDIFACT pro řešení potíží.
* Poskytuje správu stavu dávek (spouštění a zastavování) tak, jak je definováno ve smlouvě nebo smlouvách EDIFACT v související instanci aplikace API TPM.

## <a name="biztalk-x12"></a>BizTalk X12
* Vykonává smlouvy X12 tak, jak jsou definovány v související instanci aplikace API TPM. 
* Poskytuje informace o zpracování nebo sledování X12 pro řešení potíží.
* Poskytuje správu stavu dávek (spouštění a zastavování) tak, jak je definováno ve smlouvě nebo smlouvách X12 v související instanci aplikace API TPM.

Jak je uvedeno výše, aplikace API pro AS2, X12 a EDIFACT vyžadují ke správné funkci aplikaci API TPM.

## <a name="getting-started"></a>Začínáme
Postup vytvoření smluv s obchodními partnery:

1. Vytvořte instanci konektoru **BizTalk Trading Partner Management**. To pro své fungování vyžaduje prázdnou databázi SQL. Než začnete, ujistěte se, že máte k dispozici prázdnou databázi, kterou můžete použít.
2. Nahrajte schémata a certifikáty podle požadavků vyplývajících ze smluv. Proveďte to tak, že projdete vytvořenou instanci TPM a zanoříte se do části Schéma a/nebo Certifikáty.
3. Přejděte na vytvořenou instanci TPM a zanořte se do části **Partneři**.
4. Vytvořte partnery dle svých potřeb. Také proveďte příslušné úpravy profilů a přidejte požadované identity.
5. Nyní v části **Smlouvy** vytvořte smlouvy. Když vytvoříte smlouvu, musíte vybrat protokol, který se bude používat. Zbývající možnosti konfigurace závisí na protokolu, který jste vybrali.

![][2]

![][3]

<!--Image references-->
[1]: ./media/app-service-logic-create-a-trading-partner-agreement/TPMResourceView.png
[2]: ./media/app-service-logic-create-a-trading-partner-agreement/ProtocolSelection.png
[3]: ./media/app-service-logic-create-a-trading-partner-agreement/X12AgreementCreation.png




<!--HONumber=Nov16_HO2-->


