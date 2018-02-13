---
title: "Vytvořit a registrovat SOAP konektory - Azure Logic Apps | Microsoft Docs"
description: "Nastavení konektorů SOAP pro použití v Azure Logic Apps"
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
ms.date: 10/24/2017
ms.author: LADocs; estfan
ms.openlocfilehash: 031762e5639fc52e0b0a6a5bf8d12db25da25e12
ms.sourcegitcommit: eeb5daebf10564ec110a4e83874db0fb9f9f8061
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 02/03/2018
---
# <a name="create-and-register-soap-connectors-in-azure-logic-apps"></a>Vytvořit a registrovat SOAP konektory v Azure Logic Apps

Pokud chcete v pracovních postupech svých aplikací logiky integrovat služby SOAP, můžete vytvořit a zaregistrovat vlastní konektor SOAP (Simple Object Access Protocol) s využitím jazyka WSDL (Web Services Description Language) popisujícího vaši službu SOAP. Konektory SOAP fungují jako předem připravené konektory, takže je ve svých aplikacích logiky můžete používat stejným způsobem jako jiné konektory.


## <a name="prerequisites"></a>Požadavky

K registraci konektoru SOAP potřebujete následující položky:

* Předplatné Azure. Pokud předplatné nemáte, můžete začít s [bezplatným účtem Azure](https://azure.microsoft.com/free/). Jinak si můžete zaregistrovat [předplatné s průběžnými platbami](https://azure.microsoft.com/pricing/purchase-options/).

* Jakoukoli z těchto položek:
  * Adresa URL k souboru WSDL, který definuje vaši službu SOAP a rozhraní API.
  * Soubor WSDL, který definuje vaši službu SOAP a rozhraní API.

  Pro účely tohoto kurzu můžete použít naši ukázkovou [službu SOAP pro objednávky](http://fazioapisoap.azurewebsites.net/FazioService.svc?singleWsdl).

* Volitelné: Obrázek, který se použije jako ikona pro váš vlastní konektor.


## <a name="1-create-your-connector"></a>1. Vytvoření konektoru

1. Na webu Azure Portal v hlavní nabídce Azure zvolte **Nový**. Do vyhledávacího pole zadejte jako filtr „konektor Logic Apps“ a stiskněte Enter.

   ![Nový, hledání řetězce „konektor Logic Apps“](./media/logic-apps-soap-connector-create-register/create-logic-apps-connector.png)

2. Ze seznamu výsledků zvolte **Konektor Logic Apps** > **Vytvořit**.

   ![Vytvoření konektoru Logic Apps](./media/logic-apps-soap-connector-create-register/choose-logic-apps-connector.png)

3. Zadejte podrobnosti pro registraci konektoru, jak je popsáno v této tabulce. Až budete hotovi, zvolte **Připnout na řídicí panel** > **Vytvořit**.

   ![Podrobnosti o vlastním konektoru Logic Apps](./media/logic-apps-soap-connector-create-register/logic-apps-soap-connector-details.png)

   | Vlastnost | Navrhovaná hodnota | Popis | 
   | -------- | --------------- | ----------- | 
   | **Název** | *název_konektoru_SOAP* | Zadejte název vašeho konektoru. | 
   | **Předplatné** | *název_předplatného_Azure* | Vyberte své předplatné Azure. | 
   | **Skupina prostředků** | *název_skupiny_prostředků_Azure* | Vytvořte nebo vyberte skupinu Azure pro uspořádání vašich prostředků Azure. | 
   | **Umístění** | *oblast_nasazení* | Vyberte pro konektor oblast nasazení. | 
   |||| 

   Jakmile Azure nasadí váš konektor, automaticky se otevře nabídka konektoru Logic Apps. 
   Pokud ne, zvolte svůj konektor SOAP z řídicího panelu Azure.

## <a name="2-define-your-connector"></a>2. Definice konektoru

Teď zadejte soubor WSDL nebo adresu URL k souboru WSDL pro vytvoření vašeho konektoru, typ ověřování, který váš konektor používá, a akce a triggery, které váš konektor SOAP poskytuje.


### <a name="2a-specify-the-wsdl-file-or-url-for-your-connector"></a>2a. Zadání souboru WSDL nebo adresy URL k souboru WSDL pro váš konektor

1. V nabídce vašeho konektoru zvolte možnost **Konektor Logic Apps**, pokud ještě není vybraná. Na panelu nástrojů zvolte **Upravit**.

   ![Upravit vlastní konektor](./media/logic-apps-soap-connector-create-register/edit-soap-connector.png)

2. Zvolte **Obecné**, abyste mohli zadat podrobnosti uvedené v následujících tabulkách pro vytvoření, zabezpečení a definici akcí a triggerů pro váš konektor SOAP.

   1. V části **Vlastní konektory** vyberte **SOAP** pro váš **Koncový bod rozhraní API**, abyste mohli zadat soubor WSDL, který popisuje vaše rozhraní API.

      ![Zadání souboru WSDL pro vaše rozhraní API](./media/logic-apps-soap-connector-create-register/provide-wsdl-file.png)

      | Možnost | Formát |Popis | 
      | ------ | ------ | ----------- | 
      | **Nahrání WSDL ze souboru** | *soubor_WSDL* | Přejděte do umístění vašeho souboru WSDL a vyberte tento soubor. | 
      | **Nahrání WSDL z adresy URL** | http://*cesta_k_souboru_WSDL* | Zadejte adresu URL souboru WSDL vaší služby. | 
      | **Ze SOAP do REST** |   | Transformujte rozhraní API ve službě SOAP na rozhraní REST API. | 
      |||| 

   2. V části **Obecné informace** nahrajte ikonu pro váš konektor. 
   Do polí **Popis**, **Hostitel** a **Základní adresa URL** se obvykle automaticky vyplní údaje z vašeho souboru WSDL. 
   Pokud ne, přidejte tyto informace tak, jak je popsáno v tabulce, a zvolte **Pokračovat**. 

      ![Podrobnosti o konektoru](./media/logic-apps-soap-connector-create-register/add-general-details.png)

      | Možnost nebo nastavení | Formát | Popis | 
      | ----------------- | ------ | ----------- | 
      | **Nahrát ikonu** | *soubor_png_nebo_jpg_menší_než_1_MB* | Ikona představující váš konektor. <p>Barva: Pokud možno bílé logo na barevném pozadí. <p>Rozměry: Logo o velikosti přibližně 160 pixelů uvnitř čtverce o velikosti 230 pixelů. | 
      | **Barva pozadí ikony** | *šestnáctkový_kód_barvy_značky_v_ikoně* | <p>Barva pozadí vaší ikony, která odpovídá barvě pozadí v souboru vaší ikony. <p>Formát: Šestnáctkový. Například kód #007ee5 představuje modrou barvu. | 
      | **Popis** | *popis_konektoru* | Zadejte krátký popis vašeho konektoru. | 
      | **Hostitel** | *hostitel_konektoru* | Zadejte doménu hostitele vaší služby SOAP. | 
      | **Základní adresa URL** | *základní_adresa_URL_konektoru* | Zadejte základní adresu URL vaší služby SOAP. | 
      |||| 

### <a name="2b-describe-the-authentication-that-your-connector-uses"></a>2b. Popis ověřování, které váš konektor využívá

1. Teď zvolte **Zabezpečení**, abyste mohli zkontrolovat nebo popsat ověřování, které váš konektor využívá. Ověřování zajišťuje tok identit vašich uživatelů odpovídajícím způsobem mezi vaší službou a všemi klienty.

   Ve výchozím nastavení je **Typ ověřování** vašeho konektoru nastavený na **Bez ověřování**.
   
   ![Typ ověřování](./media/logic-apps-soap-connector-create-register/security-authentication-options.png)

   Pokud chcete změnit typ ověřování, zvolte **Upravit**. Můžete vybrat **Základní ověřování**. Pokud chcete použít jiné popisky parametrů než výchozí hodnoty, aktualizujte je v části **Popisek parametru**.

   ![Základní ověřování](./media/logic-apps-soap-connector-create-register/security.png)

   
2. Pokud chcete po zadání informací o zabezpečení svůj konektor uložit, v horní části stránky zvolte **Aktualizovat konektor** a pak **Pokračovat**. 

### <a name="2c-review-update-or-define-actions-and-triggers-for-your-connector"></a>2c. Kontrola, aktualizace nebo definice akcí a triggerů pro konektor

1. Teď zvolte **Definice**, abyste mohli zkontrolovat, upravit nebo definovat nové akce a triggery, které uživatelé můžou přidávat do svých pracovních postupů.

   Akce a triggery vycházejí z operací definovaných ve vašem souboru WSDL, kterými se automaticky vyplní stránka **Definice**, včetně hodnot požadavků a odpovědí. Takže pokud se už tady zobrazí požadované informace, můžete přejít k dalšímu kroku procesu registrace bez provádění změn na této stránce.

   ![Definice konektoru](./media/logic-apps-soap-connector-create-register/definition.png)

2. Případně, pokud chcete upravit existující akce a aktivační události nebo přidat nové, [pokračovat v těchto krocích](logic-apps-custom-connector-register.md#add-action-or-trigger).


## <a name="3-finish-creating-your-connector"></a>3. Dokončení vytváření konektoru

Až budete připraveni, zvolte **Aktualizovat konektor**, abyste mohli svůj konektor nasadit. 

Blahopřejeme! Při vytváření aplikace logiky teď můžete svůj konektor vyhledat v Návrháři pro Logic Apps a přidat ho do své aplikace logiky.

![Vyhledání konektoru v Návrháři pro Logic Apps](./media/logic-apps-soap-connector-create-register/soap-connector-created.png)

## <a name="share-your-connector-with-other-logic-apps-users"></a>Sdílení konektoru s ostatními uživateli Logic Apps

Zaregistrované, ale necertifikované vlastní konektory fungují podobně jako konektory spravované Microsoftem, ale jsou viditelné a dostupné *pouze* pro autora konektoru a uživatele se stejným tenantem Azure Active Directory a předplatným Azure pro aplikace logiky v oblasti, ve které jsou tyto aplikace nasazené. I když sdílení není povinné, v některých situacích chcete sdílet vaše konektory s dalšími uživateli. 

> [!IMPORTANT]
> Pokud sdílíte konektor, ostatní na něm můžou začít být závislí. 
> ***Odstraněním konektoru dojde k odstranění všech připojení k tomuto konektoru.***
 
Sdílet vaše konektor s externími uživateli mimo tyto hranice, například se všemi uživateli Logic Apps, [odeslání vašeho konektoru pro certifikaci Microsoft](../logic-apps/custom-connector-submit-certification.md).

## <a name="faq"></a>Nejčastější dotazy

**Otázka:** Je konektor SOAP obecně dostupný? </br>
**Odpověď:** Konektor SOAP je ve verzi **Preview** a zatím není obecně dostupnou službou.

**Otázka:** Existují nějaká omezení a známé problémy související s konektorem SOAP? </br>
**Odpověď:** Ano, viz [Omezení a známé problémy související s konektorem SOAP](../api-management/api-management-api-import-restrictions.md#wsdl).

**Otázka:** Existují nějaká omezení pro vlastní konektory? </br>
**Odpověď:** Ano, viz [omezení vlastních konektorů](../logic-apps/logic-apps-limits-and-config.md#custom-connector-limits).

## <a name="get-support"></a>Získat podporu

* Pokud potřebujete podporu k vývoji a registraci nebo pokud chcete požádat o funkce, které nejsou dostupné v průvodci registrací, kontaktujte [condevhelp@microsoft.com](mailto:condevhelp@microsoft.com). Microsoft v tomto účtu monitoruje dotazy a problémy vývojářů a směruje je odpovídajícím týmům.

* Klást otázky, odpovídat na ně a poučit se ze zkušeností jiných uživatelů Azure Logic Apps můžete ve [fóru Azure Logic Apps](https://social.msdn.microsoft.com/Forums/en-US/home?forum=azurelogicapps).

* Pokud chcete pomoci při vylepšování Logic Apps, hlasujte nebo zanechte své nápady na [webu zpětné vazby uživatelů Logic Apps](http://aka.ms/logicapps-wish). 

## <a name="next-steps"></a>Další postup

* [Volitelné: Certifikovat vašeho konektoru](../logic-apps/custom-connector-submit-certification.md)
* [Nejčastější dotazy k vlastním konektorům](../logic-apps/custom-connector-faq.md)