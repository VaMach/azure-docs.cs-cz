---
title: "Vytvořit a registrovat SOAP konektory - Azure Logic Apps | Microsoft Docs"
description: "Nastavení protokolu SOAP konektory pro použití v Azure Logic Apps"
author: divyaswarnkar
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
ms.author: LADocs; divswa
ms.openlocfilehash: 2d4d009dfc4d43ccc3c69bb3da15ca2c478b9efe
ms.sourcegitcommit: e5355615d11d69fc8d3101ca97067b3ebb3a45ef
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/31/2017
---
# <a name="create-and-register-soap-connectors-in-azure-logic-apps"></a>Vytvořit a registrovat SOAP konektory v Azure Logic Apps

K integraci služby SOAP ve svých pracovních postupech aplikace logiky, můžete vytvořit a registrovat vlastní konektor objekt přístup protokolu SOAP (Simple) pomocí webové služby popis Language (WSDL) popisující služby SOAP. Konektory SOAP fungují jako předem konektory, abyste je mohli používat stejným způsobem jako ostatní konektory ve vašich logic apps.


## <a name="prerequisites"></a>Požadavky

Chcete-li zaregistrovat vaše SOAP konektoru, je třeba tyto položky:

* Předplatné Azure. Pokud nemáte předplatné, můžete začít s [bezplatný účet Azure](https://azure.microsoft.com/free/). Jinak, zaregistrujte si [předplatné s průběžnými platbami](https://azure.microsoft.com/pricing/purchase-options/).

* Libovolnou položku zde:
  * Adresu URL WSDL, který definuje služby protokolu SOAP a rozhraní API
  * Soubor WSDL, který definuje služby protokolu SOAP a rozhraní API

  V tomto kurzu použijete našem příkladu [objednávky SOAP služby](http://fazioapisoap.azurewebsites.net/FazioService.svc?singleWsdl).

* Volitelné: Obrázek, který chcete použít jako ikona pro vaše vlastní konektor


## <a name="1-create-your-connector"></a>1. Vytvoření vašeho konektoru

1. Na portálu Azure v hlavní nabídce Azure, zvolte **nový**. Do vyhledávacího pole zadejte "konektor aplikace logiky" jako filtr a stiskněte klávesu Enter.

   ![Nové, vyhledejte "konektor aplikace logiky"](./media/logic-apps-soap-connector-create-register/create-logic-apps-connector.png)

2. V seznamu výsledků vyberte **konektor aplikace logiky** > **vytvořit**.

   ![Vytvoření konektoru Logic Apps](./media/logic-apps-soap-connector-create-register/choose-logic-apps-connector.png)

3. Zadejte podrobnosti pro registraci vaší konektor, jak je popsáno v tabulce. Až budete hotoví, zvolte **připnout na řídicí panel** > **vytvořit**.

   ![Podrobnosti o vlastní konektor aplikaci logiky](./media/logic-apps-soap-connector-create-register/logic-apps-soap-connector-details.png)

   | Vlastnost | Navrhovaná hodnota | Popis | 
   | -------- | --------------- | ----------- | 
   | **Název** | *název konektoru protokolu SOAP* | Zadejte název vašeho konektoru. | 
   | **Předplatné** | *Název předplatného Azure* | Vyberte předplatné Azure. | 
   | **Skupina prostředků** | *Azure--název skupiny prostředků –* | Vytvořte nebo vyberte skupiny Azure pro uspořádání prostředků Azure. | 
   | **Umístění** | *nasazení oblast* | Vyberte oblast nasazení pro vaše konektor. | 
   |||| 

   Po Azure nasadí vašeho konektoru, v nabídce konektor logiku aplikace se automaticky otevře. 
   V opačném případě vyberte konektor vaše protokolu soap z řídicího panelu Azure.

## <a name="2-define-your-connector"></a>2. Zadejte vaše konektoru

Nyní zadejte WSDL soubor nebo adresa URL pro vytvoření vašeho konektoru, ověřování, které vaše konektor používá, a akce a aktivačních událostí, které poskytuje vaší konektor protokolu soap


### <a name="2a-specify-the-wsdl-file-or-url-for-your-connector"></a>2a. Zadejte soubor WSDL nebo adresa URL pro váš konektor

1. V nabídce vašeho konektoru, pokud již není vybrána, vyberte **konektor aplikace logiky**. Na panelu nástrojů vyberte **upravit**.

   ![Upravit vlastní konektor](./media/logic-apps-soap-connector-create-register/edit-soap-connector.png)

2. Zvolte **Obecné** tak, aby vytváření můžete zadat podrobnosti v těchto tabulkách, zabezpečení a definování akcí a aktivačních událostí pro vaše konektor protokolu SOAP.

   1. Pro **vlastní konektory**, vyberte **SOAP** pro vaše **koncový bod rozhraní API** můžou poskytovat WSDL soubor, který popisuje rozhraní API.

      ![Zadejte soubor WSDL pro vaše rozhraní API](./media/logic-apps-soap-connector-create-register/provide-wsdl-file.png)

      | Možnost | Formát |Popis | 
      | ------ | ------ | ----------- | 
      | **Nahrát WSDL ze souboru** | *Soubor WSDL* | Přejděte do umístění souboru WSDL a vyberte tento soubor. | 
      | **Nahrát WSDL z adresy URL** | http://*cestu do wsdl souboru* | Zadejte adresu URL k souboru WSDL vaší služby. | 
      | **SOAP ostatními** |   | Transformuje rozhraní API v protokolu SOAP služby do rozhraní REST API. | 
      |||| 

   2. Pro **obecné informace**, nahrajte ikonu pro vaše konektor. 
   Obvykle **popis**, **hostitele**, a **základní adresa URL** pole se vyplní automaticky ze souboru WSDL. 
   Ale pokud nejsou, přidejte tyto informace, jak je popsáno v tabulce a zvolte **pokračovat**. 

      ![Podrobnosti o konektoru](./media/logic-apps-soap-connector-create-register/add-general-details.png)

      | Možnost nebo nastavení | Formát | Popis | 
      | ----------------- | ------ | ----------- | 
      | **Nahrajte ikonu** | *PNG-or-JPG-File-under-1-MB* | Ikona, která představuje váš konektoru <p>Barva: Pokud možno bílé logo na barvu pozadí. <p>Dimenze: Logo ~ 160 pixelů uvnitř čtverce 230 pixelů | 
      | **Barva pozadí ikonu** | *Ikona brand – barva hexadecimální kódu* | <p>Barva za vaše ikonu, která odpovídá barvu pozadí v souboru ikonu. <p>Formát: šestnáctkové. Například #007ee5 představuje modrou barvu. | 
      | **Popis** | *Popis konektoru* | Zadejte krátký popis pro vaše konektor. | 
      | **Hostitele** | *konektor hostitele* | Zadejte domény hostitele služby SOAP. | 
      | **Základní adresu URL** | *konektor – základní – adresa URL* | Zadejte základní adresu URL služby SOAP. | 
      |||| 

### <a name="2b-describe-the-authentication-that-your-connector-uses"></a>2b. Popisují ověřování, které používá vaše konektoru

1. Teď zvolte **zabezpečení** , můžete zkontrolovat nebo popisují ověřování, které používá vaše konektor. Ověřování je zajištěno správně toku identity vašich uživatelů mezi službou a všechny klienty.

   Ve výchozím vaše konektoru na **typ ověřování** je nastaven na **bez ověřování**.
   
   ![Typ ověřování](./media/logic-apps-soap-connector-create-register/security-authentication-options.png)

   Chcete-li změnit typ ověřování, zvolte **upravit**. Můžete vybrat **základní ověřování**. Pokud chcete použít parametr popisky než výchozí hodnoty, aktualizovat je v části **popisek parametru**.

   ![Základní ověřování](./media/logic-apps-soap-connector-create-register/security.png)

   
2. Chcete-li uložit vaše konektoru po zadání informace o zabezpečení v horní části stránky, zvolte **aktualizovat konektor**, zvolte **pokračovat**. 

### <a name="2c-review-update-or-define-actions-and-triggers-for-your-connector"></a>2c. Zkontrolujte, aktualizovat nebo zadejte akce a aktivačních událostí pro vaše konektoru

1. Teď zvolte **definice** , můžete zkontrolovat, upravit nebo definovat nové akce a aktivační události, které uživatelé mohou přidávat do svých pracovních postupů.

   Akce a aktivační události jsou založené na operace definované v souboru WSDL, které automaticky vyplnit **definice** stránky a zahrnout hodnoty požadavku a odpovědi. Takže pokud potřebných operací se již v tomto poli, můžete přejít na další krok v procesu registrace bez provedení změn na této stránce.

   ![Definice konektoru](./media/logic-apps-soap-connector-create-register/definition.png)

2. Případně, pokud chcete upravit existující akce a aktivační události nebo přidat nové, [pokračovat v těchto krocích](logic-apps-custom-connector-register.md#add-action-or-trigger).


## <a name="3-finish-creating-your-connector"></a>3. Vytvoření vašeho konektoru

Až budete připraveni, zvolte **aktualizace konektor** abyste mohli nasadit vaší konektor. 

Blahopřejeme! Teď při vytváření aplikace logiky můžete najít vaše konektor v návrháři aplikace logiky a přidejte tento konektor do aplikace logiky.

![V návrháři aplikace logiky najít váš konektoru](./media/logic-apps-soap-connector-create-register/soap-connector-created.png)

## <a name="share-your-connector-with-other-logic-apps-users"></a>Sdílet vaše konektor s jinými uživateli Logic Apps

Registrovaná, ale necertifikované vlastní konektory fungovat jako konektorů spravovaných společností Microsoft, ale jsou viditelné a dostupné *pouze* konektoru autora a uživatelé, kteří mají stejné klienta Azure Active Directory a předplatné Azure pro logic apps v oblasti, kde jsou tyto aplikace nasazené. I když sdílení je volitelný a může mít scénáře, ve které chcete sdílet vaše konektory s dalšími uživateli. 

> [!IMPORTANT]
> Pokud sdílíte konektor, jiné můžou začít závisí na konektor. 
> ***Odstraněním vašeho konektoru odstraníte všechna připojení pro tento konektor.***
 
Sdílet vaše konektor s externími uživateli mimo tyto hranice, například se všemi uživateli Logic Apps, [odeslání vašeho konektoru pro certifikaci Microsoft](../logic-apps/custom-connector-submit-certification.md).

## <a name="faq"></a>Nejčastější dotazy

**Otázka:** je nějak omezený pro vlastní konektory? </br>
**Odpověď:** Ano najdete v tématu [vlastní konektor omezuje zde](../logic-apps/logic-apps-limits-and-config.md#custom-connector-limits).

## <a name="get-support"></a>Získat podporu

* Obraťte se na podporu vývoje a registrace nebo na žádost o funkce, které nejsou k dispozici v Průvodci registrací [ condevhelp@microsoft.com ](mailto:condevhelp@microsoft.com). Microsoft sleduje tento účet pro vývojáře otázky a problémy a směruje je do příslušné team.

* Požádejte nebo odpovědi na otázky, nebo najdete, co dělají jiných uživatelů Azure Logic Apps, naleznete [fórum Azure Logic Apps](https://social.msdn.microsoft.com/Forums/en-US/home?forum=azurelogicapps).

* K vylepšení Logic Apps, hlasovat o nebo odeslání nápadů na [web pro zasílání názorů uživatele Logic Apps](http://aka.ms/logicapps-wish). 

## <a name="next-steps"></a>Další kroky

* [Volitelné: Certifikovat vašeho konektoru](../logic-apps/custom-connector-submit-certification.md)
* [Vlastní konektor – nejčastější dotazy](../logic-apps/custom-connector-faq.md)