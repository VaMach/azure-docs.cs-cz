---
title: "Certifikovat vlastní konektory - Azure Logic Apps | Microsoft Docs"
description: "Zpřístupnit konektory pro všechny uživatele v Azure Logic Apps, Microsoft Flow a Microsoft PowerApps"
author: ecfan
manager: anneta
editor: 
services: logic-apps
documentationcenter: 
ms.assetid: 
ms.service: logic-apps
ms.workload: logic-apps
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/22/2017
ms.author: LADocs; estfan
ms.openlocfilehash: 9bcb138c181df0c12548cc5dc721542fd9c9ba68
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/11/2017
---
# <a name="submit-your-connectors-for-microsoft-certification"></a>Odeslání vašeho konektory pro certifikaci Microsoft

Chcete-li vlastní konektory veřejně dostupné pro všechny uživatele v Azure Logic Apps, Microsoft Flow a Microsoft PowerApps, odeslání vašeho konektor Microsoft ke kontrole, ověření a schválení pro publikování. 

## <a name="certification-criteria"></a>Certifikační kritéria

| Schopnost | Podrobnosti | Povinných nebo doporučených |
|------------|---------|-------------------------|
| Aplikace software jako služba (SaaS) | Splňuje uživatelský scénář, který nejlépe odpovídá s Logic Apps, toku a PowerApps. | Požaduje se |
| Typ ověřování | Rozhraní API musí podporovat OAuth2, klíč rozhraní API nebo základní ověřování. | Požaduje se | 
| Podpora | Je nutné zadat a obraťte se na podporu, aby zákazníci mohli získat pomoc. | Požaduje se | 
| Dostupnost a provozu | Vaše aplikace obsahuje minimálně 99,9 % doby provozu. | Doporučené | 
|||| 

Navíc pokud nejste partnera Microsoftu nebo nezávislého výrobce softwaru (ISV) a chcete certifikovat a veřejně verzí konektor, musí buď vlastníte základní služby nebo existuje explicitní práva na používání rozhraní API.

## <a name="validation-phases"></a>Fáze ověření

Společnost Microsoft používá tyto fáze ověření pro vyhodnocení vašeho konektor:

| Fáze ověření | Popis | 
| ----- | ----------- |
| Funkce | Microsoft testů vašeho konektoru Logic Apps, tok a PowerApps pro tyto chyby: <p>-Neplatná OpenAPI (Swagger) nebo JSON chyby, které se zobrazují v části definice z Průvodce vlastní konektor <p>-Runtime a schémat chyby ověření, které se zobrazují v testovací část z Průvodce vlastní konektor | 
| Obsah | Microsoft ověří, vaše konektor používá popisné názvy a popisy pro každé entity. Ujistěte se, že každou operaci, vstupní parametr a atribut odpovědi v Swagger vaše konektor má tyto prvky: <p>- [Souhrn](../logic-apps/custom-connector-openapi-extensions.md#summary) <br>- [Popis](../logic-apps/custom-connector-openapi-extensions.md#description) </br>- [Viditelnost informace](../logic-apps/custom-connector-openapi-extensions.md#visibility) | 
||| 

## <a name="nominate-and-submit-your-connector-to-microsoft-for-certification"></a>Jmenovat a odeslání vašeho konektoru společnosti Microsoft pro certifikaci

Použít pro certifikaci, postupujte takto:

1. **Jmenovat**

   1. [Odeslání vašeho navrženém](https://go.microsoft.com/fwlink/?linkid=848754).

      1. V dolní části stránky, zvolte **kontaktujte nás**.
      2. Vyplnění formuláře a vyberte **dotazy k programu konektor ISV a společné marketing**.
      3. Zvolte **odeslání**.

   2. Podepisování vzájemné Non-zpřístupnění smlouvy a partnerskou smlouvu, kterou jste obdrželi. 

      Microsoft vyžaduje tyto podepsaných smluv, než budete pokračovat. 
      Potom jsme můžete zkontrolovat, jestli vaše konektor splňuje kritéria certifikační. 
   
   3. Pokud je vaše konektor, upozorní vás Microsoft s pokyny pro registraci.
    
2. **Recenze**

   1. Tyto informace poslat kontakt navrženém ke kontrole:

      * OpenAPI soubor, který popisuje rozhraní API
      * Soubor ikony (.png nebo .jpg), který představuje vaše konektor 
      
        Vaše ikonu by měl mít logo ~ 160 pixelů uvnitř čtverce 230 pixelů. 
        Bílé logo na barevném pozadí je upřednostňovaný.
      
      * Vaše ikonu brand barvu v šestnáctkovém formátu, který by měl odpovídat barevná pozadí na soubor ikony

      * Testovací účet pro ověřování
      * Kontaktu podpory

   2. Pokud budeme potřebovat další informace, budeme vás bude kontaktovat.

3. **Publikování**

    Po ověření jsme vaše konektor funkce a obsah, jsme Příprava konektor pro nasazení ve všech produktů a oblastech.
    
    Standardně jsou všechny konektory publikované jako "premium" konektory. 
    Pokud jste vytvořili vaší aplikaci s Azure, můžete použít pro výpis vaší konektor jako "standard" konektor, který je k dispozici všem uživatelům s Office 365 podnikových plánů. 
    Další podrobnosti požádejte navrženém kontakt.

## <a name="next-steps"></a>Další kroky

* [Vlastní konektor – nejčastější dotazy](../logic-apps/custom-connector-faq.md)
* [Vlastní konektor – přehled](../logic-apps/custom-connector-overview.md)