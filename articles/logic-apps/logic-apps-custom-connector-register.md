---
title: "Zaregistrovat vlastní konektory - Azure Logic Apps | Microsoft Docs"
description: "Nastavit vlastní konektory pro použití v Azure Logic Apps"
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
ms.openlocfilehash: 9e3d88abe751b37700590cc68c458f208d5868d2
ms.sourcegitcommit: e5355615d11d69fc8d3101ca97067b3ebb3a45ef
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/31/2017
---
# <a name="register-custom-connectors-in-azure-logic-apps"></a>Zaregistrovat vlastní konektory v Azure Logic Apps

Po sestavení REST API, nastavení ověřování a získání souboru definice OpenAPI nebo kolekci Postman, jste připraveni k registraci vašeho konektoru. 

## <a name="prerequisites"></a>Požadavky

Chcete-li zaregistrovat vlastní konektor, je třeba tyto položky:

* Podrobnosti o registraci vašeho konektoru v Azure, jako je například název, předplatné, skupinu prostředků Azure a umístění, které chcete použít

* Soubor OpenAPI (Swagger) nebo Postman kolekce, které popisují vaše rozhraní API

  V tomto kurzu můžete použít [ukázkový soubor OpenAPI správce prostředků Azure](http://pwrappssamples.blob.core.windows.net/samples/AzureResourceManager.json).

* Ikona, která představuje váš konektoru

* Krátký popis pro vaše konektor

* Umístění hostitele pro vaše rozhraní API

## <a name="1-create-your-connector"></a>1. Vytvoření vašeho konektoru

1. Na portálu Azure v hlavní nabídce Azure, zvolte **nový**. Do vyhledávacího pole zadejte "konektor aplikace logiky" jako filtr a stiskněte klávesu Enter.

   ![Nové, vyhledejte "konektor aplikace logiky"](./media/logic-apps-custom-connector-register/create-logic-apps-connector.png)

2. V seznamu výsledků vyberte **konektor aplikace logiky** > **vytvořit**.

   ![Vytvoření konektoru Logic Apps](./media/logic-apps-custom-connector-register/choose-logic-apps-connector.png)

3. Zadejte podrobnosti pro registraci vaší konektor, jak je popsáno v tabulce. Až budete hotoví, zvolte **připnout na řídicí panel** > **vytvořit**.

   ![Podrobnosti o vlastní konektor aplikaci logiky](./media/logic-apps-custom-connector-register/logic-apps-connector-details.png)

   | Vlastnost | Navrhovaná hodnota | Popis | 
   | -------- | --------------- | ----------- | 
   | **Název** | *vlastní název konektoru* | Zadejte název vašeho konektoru. | 
   | **Předplatné** | *Název předplatného Azure* | Vyberte předplatné Azure. | 
   | **Skupina prostředků** | *Azure--název skupiny prostředků –* | Vytvořte nebo vyberte skupiny Azure pro uspořádání prostředků Azure. | 
   | **Umístění** | *nasazení oblast* | Vyberte oblast nasazení pro vaše konektor. | 
   |||| 

   Po Azure nasadí vašeho konektoru, v nabídce vlastní konektor se automaticky otevře. 
   Pokud ne, zvolte vlastní konektor na řídicím panelu Azure.

## <a name="2-define-your-connector"></a>2. Zadejte vaše konektoru

Teď určete soubor OpenAPI nebo Postman kolekce k vytvoření vašeho konektoru, ověřování, které používá vaše konektor, akce a aktivačních událostí, které poskytuje vlastní konektor a parametry, které můžete použít akce a aktivační události.

### <a name="2a-specify-the-openapi-file-or-postman-collection-for-your-connector"></a>2a. Zadejte soubor OpenAPI nebo Postman kolekce pro vaše konektor

1. V nabídce vašeho konektoru, pokud již není vybrána, vyberte **konektor aplikace logiky**. Na panelu nástrojů vyberte **upravit**.

   ![Upravit vlastní konektor](./media/logic-apps-custom-connector-register/edit-custom-connector.png)

2. Zvolte **Obecné** tak, aby vytváření můžete zadat podrobnosti v těchto tabulkách, zabezpečení a definování akcí a aktivačních událostí pro vlastní konektor.

   1. Pro **vlastní konektory**, vyberte možnost, můžete zadat soubor OpenAPI (Swagger), které popisují vaše rozhraní API.

      ![Zadejte soubor OpenAPI pro vaše rozhraní API](./media/logic-apps-custom-connector-register/provide-openapi-file.png)

      | Možnost | Formát |Popis | 
      | ------ | ------ | ----------- | 
      | **Nahrát soubor OpenAPI** | *OpenAPI (Swagger) - json – soubor* | Přejděte do umístění souboru OpenAPI a vyberte tento soubor. | 
      | **Použijte adresu URL OpenAPI** | http://*cesta na--json souboru swagger* | Zadejte adresu URL pro vaše rozhraní API OpenAPI soubor. | 
      | **Nahrát Postman kolekce V1** | *Export Postman--V1-soubor kolekce* | Přejděte do umístění pro exportované kolekce Postman ve formátu V1. | 
      |||| 

   2. Pro **obecné informace**, nahrajte ikonu pro vaše konektor. 
   Obvykle **popis**, **hostitele**, a **základní adresa URL** pole se vyplní automaticky ze souboru OpenAPI. 
   Ale pokud nejsou, přidejte tyto informace, jak je popsáno v tabulce a zvolte **pokračovat**. 

      ![Podrobnosti o konektoru](./media/logic-apps-custom-connector-register/add-connector-details.png)

      | Možnost nebo nastavení | Formát | Popis | 
      | ----------------- | ------ | ----------- | 
      | **Nahrajte ikonu** | *PNG-or-JPG-File-under-1-MB* | Ikona, která představuje váš konektoru <p>Barva: Pokud možno bílé logo na barvu pozadí. <p>Dimenze: Logo ~ 160 pixelů uvnitř čtverce 230 pixelů | 
      | **Barva pozadí ikonu** | *Ikona brand – barva hexadecimální kódu* | <p>Barva za vaše ikonu, která odpovídá barvu pozadí v souboru ikonu. <p>Formát: šestnáctkové. Například #007ee5 představuje modrou barvu. | 
      | **Popis** | *Popis konektoru* | Zadejte krátký popis pro vaše konektor. | 
      | **Hostitele** | *konektor hostitele* | Zadejte domény hostitele pro Web API. | 
      | **Základní adresu URL** | *konektor – základní – adresa URL* | Zadejte základní adresu URL pro Web API. | 
      |||| 

### <a name="2b-describe-the-authentication-that-your-connector-uses"></a>2b. Popisují ověřování, které používá vaše konektoru

1. Teď zvolte **zabezpečení** , můžete zkontrolovat nebo popisují ověřování, které používá vaše konektor. Ověřování je zajištěno správně toku identity vašich uživatelů mezi službou a všechny klienty.

   ![Typ ověřování](./media/logic-apps-custom-connector-register/security.png)

   * Pokud nahrajete soubor OpenAPI, Průvodce registrací automaticky rozpozná typ ověřování, který používá vaše rozhraní Web API, a automaticky naplní **zabezpečení** části v průvodci na základě `securityDefinitions` objektu v tom, že soubor. Zde je ukázka, oddíl, který určuje pomocí OAuth2.0:

     ``` json
     "securityDefinitions": {
       "AAD": {
       "type": "oauth2",
       "flow": "accessCode",
       "authorizationUrl": "https://login.windows.net/common/oauth2/authorize",
       "tokenUrl": "https://login.windows.net/common/oauth2/token",
       "scopes": {}
       }
     },
     ```

   * Pokud váš soubor OpenAPI nebyla naplnit typ ověřování a vlastnosti, vyberte **upravit** , můžete přidat tyto informace. 
   
     Například pokud jste dříve [nastavení ověřování Azure AD v tomto kurzu](../logic-apps/custom-connector-azure-active-directory-authentication.md), jste vytvořili aplikace Azure AD pro zabezpečení vašeho webového rozhraní API a vaše konektor. 
     Takže teď můžete zadat klíč aplikace ID a klienta, který jste předtím uložili.
    
     | Nastavení | Navrhovaná hodnota | Popis | 
     | ------- | --------------- | ----------- | 
     | **Typ ověřování** | OAuth 2.0 | | 
     | **Poskytovatel identity** | Azure Active Directory | | 
     | **Id klienta** | *application-ID-for-connector-Azure-AD-app* | ID aplikace, které jste dříve uložili pro aplikaci Azure AD vaší konektoru | 
     | **Tajný klíč klienta** | *client-key-for-connector-Azure-AD-app* | Klíč klienta pro aplikaci Azure AD vaší konektoru | 
     | **Adresa URL pro přihlášení** | `https://login.windows.net` | | 
     | **Adresa URL prostředku** | `https://management.core.windows.net/` | Ujistěte se, že přidáte adresu URL právě uvedeného, včetně koncové lomítko. | 
     |||| 

   * Postman kolekce, které automaticky generuje soubor OpenAPI za vás, automaticky naplní typ ověřování *pouze* při použití typy podporované ověřování, jako je například OAuth 2.0 nebo Basic.

2. Chcete-li uložit vaše konektoru po zadání informace o zabezpečení v horní části stránky, zvolte **aktualizovat konektor**, zvolte **pokračovat**. 

### <a name="2c-review-update-or-define-actions-and-triggers-for-your-connector"></a>2c. Zkontrolujte, aktualizovat nebo zadejte akce a aktivačních událostí pro vaše konektoru

1. Teď zvolte **definice** , můžete zkontrolovat, upravit nebo definovat nové akce a aktivační události, které uživatelé mohou přidávat do svých pracovních postupů.

   Akce a aktivační události jsou založené na operace definované v souboru OpenAPI nebo Postman kolekce, které automaticky vyplnit **definice** stránky a zahrnout hodnoty požadavku a odpovědi. Takže pokud potřebných operací se již v tomto poli, můžete přejít na další krok v procesu registrace bez provedení změn na této stránce.

   ![Definice konektoru](./media/logic-apps-custom-connector-register/definition.png)

2. Pokud chcete upravit existující akce a aktivační události nebo přidat nové, pokračujte v případě potřeby tyto kroky.

<a name="add-action-or-trigger"></a> 

#### <a name="edit-or-add-actions-for-your-connector"></a>Upravit nebo přidání akcí pro vaše konektor

1. Chcete-li upravit existující akci, zvolte číslo pro tuto akci. Chcete-li přidat akci, která nebyla v souboru OpenAPI nebo Postman kolekce, v části existují **akce**, zvolte **Nová akce**.

2. V části **Obecné**, zadejte nebo upravte tyto informace pro akci:
   
   | Nastavení | Navrhovaná hodnota | Popis | 
   | ------- | --------------- | ----------- | 
   | **Souhrn** | *Název operace* | Název pro tuto akci | 
   | **Popis** | *Popis operace* | Popis pro tuto akci. <p>**Tip**: Ujistěte se, že vaše popis končí tečkou. |
   | **ID operace** | *identifikátor operace* | Jedinečný název pro identifikaci této akce. Použijte formát camelCase, například: "GetPullRequest" | 
   |**Viditelnost**| **žádný**, **rozšířené**, **interní**, nebo **důležité** | Uživatelsky orientovaný viditelnost pro tuto akci. Další informace najdete v tématu [OpenAPI rozšíření](../logic-apps/custom-connector-openapi-extensions.md#visibility). | 
   |||| 

3. Nyní definujte požadavků pro akce.
  
   1. V **požadavku** zvolte **importovat z ukázkové**. 

   2. Na **importovat z ukázkové** stránky, vložte ukázková žádost. 

      Obvykle jsou k dispozici v dokumentaci k rozhraní API, kde může získat informace o požadavky na ukázky **příkaz**, **adresa URL**, **hlavičky**, a **textu**pole. Například najdete v článku [dokumentaci k rozhraní API Analytics Text](https://westus.dev.cognitive.microsoft.com/docs/services/TextAnalytics.V2.0/operations/56f30ceeeda5650db055a3c7).

      ![Import ukázková žádost](./media/logic-apps-custom-connector-register/import-sample-operation-request.png)

      > [!IMPORTANT]
      > Pokud vytvoříte konektor z kolekce Postman, ujistěte se, že odeberete `Content-type` záhlaví z akce a aktivační události. Služba Logic Apps automaticky přidá hlavičku. Odeberte také hlavičky ověřování, které jste definovali v `Security` části Akce a aktivační události.

      Pokročilé funkce OpenAPI, najdete v části [OpenAPI rozšíření pro vlastní konektory](../logic-apps/custom-connector-openapi-extensions.md).

   3. Chcete-li dokončit definice žádosti, zvolte **Import**.

4. Nyní definujte odpověď pro akci.

   1. V části **odpovědi**, můžete zadat výchozí odpověď. 
   Zvolte **přidat výchozí odpověď**.

   2. Na **importovat z ukázkové** stránky, vložte ukázková odpověď, a potom vyberte **Import**.

5. Nakonec v části **ověření**, zkontrolujte a opravte veškeré zjištěné pro akci potenciální potíže.

#### <a name="edit-or-add-triggers-for-your-connector"></a>Upravit nebo přidat aktivačních událostí pro vaše konektoru

1. Chcete-li upravit existující aktivační událost, vyberte číslo pro tento aktivační události. Chcete-li přidat aktivační události, který nebyl v souboru OpenAPI nebo Postman kolekce neexistuje v části **aktivační události**, zvolte **novou aktivační událost**.

2. V části **Obecné**, zadejte nebo upravte tyto informace pro aktivační událost:

   | Nastavení | Navrhovaná hodnota | Popis | 
   | ------- | --------------- | ----------- | 
   | **Souhrn** | *Název operace* | Název této aktivační události | 
   | **Popis** | *Popis operace* | Popis této aktivační události. <p>**Tip**: Ujistěte se, že vaše popis končí tečkou. | 
   | **ID operace** | *identifikátor operace* | Jedinečný název pro identifikaci této aktivační události. Použijte formát camelCase, například: "TriggerOnGitHubPushEvent" | 
   |**Viditelnost**| **žádný**, **rozšířené**, **interní**, nebo **důležité** | Viditelnost zobrazující se uživatelům této aktivační události. Další informace najdete v tématu [OpenAPI rozšíření](../logic-apps/custom-connector-openapi-extensions.md#visibility). | 
   | **Typ aktivace** | **Webhooku** nebo **dotazování** | Typ této aktivační události. Například aktivační události webhooku čeká na konkrétní událost provést před výbuchem. Cyklického dotazování aktivační událost pravidelně kontroluje koncový bod služby na základě zadaného intervalu a četnost. <p>Další informace o webhooku a cyklického dotazování aktivační událost vzory najdete v tématu [vytvořte vlastní rozhraní API](../logic-apps/logic-apps-create-api-app.md). | 
   |||| 

3. Nyní definujte požadavek, který vytvoří aktivační událost. 

   1. V **požadavku** zvolte **importovat z ukázkové**.

   2. Na **importovat z ukázkové** stránky, vložte ukázková žádost. 

      Obvykle jsou k dispozici v dokumentaci k rozhraní API, kde může získat informace o požadavky na ukázky **příkaz**, **adresa URL**, **hlavičky**, a **textu**pole. Například najdete v článku [dokumentaci k rozhraní API Analytics Text](https://westus.dev.cognitive.microsoft.com/docs/services/TextAnalytics.V2.0/operations/56f30ceeeda5650db055a3c7).

      ![Import ukázková žádost](./media/logic-apps-custom-connector-register/import-sample-operation-request.png)

      > [!IMPORTANT]
      > Pokud vytvoříte konektor z kolekce Postman, ujistěte se, že odeberete `Content-type` záhlaví z akce a aktivační události. Služba Logic Apps automaticky přidá hlavičku. Odeberte také hlavičky ověřování, které jste definovali v `Security` části Akce a aktivační události.

      Pokročilé funkce OpenAPI, najdete v části [OpenAPI rozšíření pro vlastní konektory](../logic-apps/custom-connector-openapi-extensions.md).

   3. Chcete-li dokončit definice žádosti, zvolte **Import**. 

4. Nyní definujte odpověď má aktivační procedura. V **odpovědi** oddílu, na základě typu aktivační události, postupujte takto:

   **Aktivační události Webhooku**
   1. V **Webhooku odpovědi**, zvolte **importovat z ukázkové**. 

   2. Na **importovat z ukázkové** stránky, vložte ukázková odpověď, a potom vyberte **Import**. Odpověď příklad najdete v tématu [reference pro GitHub API pro vytváření webhook, jehož](https://developer.github.com/v3/repos/hooks/#create-a-hook).

   3. V části **konfigurace aktivační události**, vyberte parametr pro žádost o vytvoření webhooku. Služba Logic Apps používá tato hodnota parametru pro sestavování adresu URL zpětné volání služby používá ke komunikaci s aktivační událost.

   **Cyklického dotazování aktivační události**
   1. V části **odpovědi**, můžete zadat výchozí odpověď. 
   Zvolte **přidat výchozí odpověď**.

   2. Na **importovat z ukázkové** stránky, vložte ukázková odpověď, a potom vyberte **Import**.

   3. V části **konfigurace aktivační události**, zadejte parametr dotazu hodnotu předat parametru a *pomocný parametr aktivační událost* určující správné interval dotazování pro další požadavek.

5. Nakonec v části **ověření**zkontrolujte a opravte všechny potenciální problémy identifikovat pro aktivační událost.

#### <a name="review-reference-definitions-for-your-connector"></a>Zkontrolujte odkaz definice pro vaše konektoru

**Odkazy** části automaticky naplní z vaší popis rozhraní API a popisuje všechny parametr pole, která může odkazovat akce a aktivační události. 

1. V části **odkazy**, vyberte číslo pro referenční definice, které chcete zkontrolovat.

2. V části **název**, zkontrolovat nebo aktualizovat definice název odkazu.

3. V části **ověření**, zkontrolujte a opravte veškeré zjištěné pro referenční definice potenciální potíže.

## <a name="3-finish-creating-your-connector"></a>3. Vytvoření vašeho konektoru

Až budete připraveni, zvolte **vytvořit** abyste mohli nasadit vaší konektor. Pokud aktualizujete existující konektor, vyberte **aktualizovat konektor**.

Blahopřejeme! Teď při vytváření aplikace logiky můžete najít vaše konektor v návrháři aplikace logiky a přidejte tento konektor do aplikace logiky.

![V návrháři aplikace logiky najít váš konektoru](./media/logic-apps-custom-connector-register/custom-connector-created.png)

## <a name="share-your-connector-with-other-logic-apps-users"></a>Sdílet vaše konektor s jinými uživateli Logic Apps

Registrovaná, ale necertifikované vlastní konektory fungovat jako konektorů spravovaných společností Microsoft, ale jsou viditelné a dostupné *pouze* konektoru autora a uživatelé, kteří mají stejné klienta Azure Active Directory a předplatné Azure pro logic apps v oblasti, kde jsou tyto aplikace nasazené. I když sdílení je volitelný a může mít scénáře, ve které chcete sdílet vaše konektory s dalšími uživateli. 

> [!IMPORTANT]
> Pokud sdílíte konektor, jiné můžou začít závisí na konektor. 
> ***Odstraněním vašeho konektoru odstraníte všechna připojení pro tento konektor.***
 
Sdílení vašeho konektoru s externími uživateli mimo tyto hranice, například s Logic Apps, toku a PowerApps uživateli, [odeslání vašeho konektoru pro certifikaci Microsoft](../logic-apps/custom-connector-submit-certification.md).

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