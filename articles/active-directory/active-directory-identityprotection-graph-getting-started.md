---
title: "Začínáme s Azure Active Directory Identity Protection a Microsoft Graph | Microsoft Docs"
description: "Poskytuje úvod do dotazů Microsoft Graph seznam rizikových událostech a přidružené informace ze služby Azure Active Directory."
services: active-directory
keywords: "ochrany identit Azure active directory, riziko událostí, ohrožení zabezpečení, zásady zabezpečení, Microsoft Graph"
documentationcenter: 
author: MarkusVi
manager: femila
ms.assetid: fa109ba7-a914-437b-821d-2bd98e681386
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/17/2017
ms.author: markvi
ms.reviewer: nigu
ms.openlocfilehash: 9b01ff86da6a1fd4a439a6ba59ea15ed6480cdad
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/18/2017
---
# <a name="get-started-with-azure-active-directory-identity-protection-and-microsoft-graph"></a>Začínáme s Azure Active Directory Identity Protection a Microsoft Graph
Microsoft Graph je Microsoft unified koncový bod rozhraní API a součástí [Azure Active Directory Identity Protection](active-directory-identityprotection.md) rozhraní API. Rozhraní API first **identityRiskEvents**, umožňuje dotazování Microsoft Graph seznam [rizik události](active-directory-identityprotection-risk-events-types.md) a související informace. Tento článek vám pomůže začít dotazování toto rozhraní API. Podrobný úvod, úplnou dokumentaci a přístup do Průzkumníka grafu naleznete v tématu [Microsoft Graph lokality](https://graph.microsoft.io/).

> [!IMPORTANT]
> Společnost Microsoft doporučuje při správě služby Azure AD používat [centrum pro správu Azure AD](https://aad.portal.azure.com) na webu Azure Portal namísto používání portálu Azure Classic, na který odkazuje tento článek.

Přístup k datům ochrany identit pomocí Microsoft Graph tři kroky:

1. Přidáte aplikaci s tajný klíč klienta. 
2. Tento tajný klíč a několik další požadované informace můžete použijte k ověření do aplikace Microsoft Graph, kterou budete dostávat ověřovací token. 
3. Pomocí tohoto tokenu provádět požadavky na koncový bod rozhraní API a vrátit data pro ochranu Identity.

Než začnete, budete potřebovat:

* Oprávnění správce k vytvoření aplikace ve službě Azure AD
* Název domény vašeho klienta (například contoso.onmicrosoft.com)

## <a name="add-an-application-with-a-client-secret"></a>Přidat aplikaci s tajný klíč klienta
1. [Přihlaste se](https://manage.windowsazure.com) na portálu Azure classic jako správce. 
2. V klikněte v levém navigačním podokně na **služby Active Directory**. 
   
    ![Vytváření aplikací](./media/active-directory-identityprotection-graph-getting-started/tutorial_general_01.png)
3. Z **Directory** seznamu, vyberte adresář, pro který chcete povolit integraci adresáře.
4. V nabídce v horní části, klikněte na tlačítko **aplikace**.
   
    ![Vytváření aplikací](./media/active-directory-identityprotection-graph-getting-started/tutorial_general_02.png)
5. Klikněte na tlačítko **přidat** v dolní části stránky.
   
    ![Vytváření aplikací](./media/active-directory-identityprotection-graph-getting-started/tutorial_general_03.png)
6. Na **co chcete udělat** dialogové okno, klikněte na tlačítko **přidat aplikaci, kterou vyvíjí Moje organizace**.
   
    ![Vytváření aplikací](./media/active-directory-identityprotection-graph-getting-started/tutorial_general_04.png)
7. Na **Řekněte nám o své aplikaci** dialogové okno, proveďte následující kroky:
   
    ![Vytváření aplikací](./media/active-directory-identityprotection-graph-getting-started/tutorial_general_05.png)
   
    a. V **název** textovému poli, zadejte název vaší aplikace (např: aplikace AADIP riziko událostí rozhraní API).
   
    b. Jako **typ**, vyberte **webové aplikace nebo webové rozhraní API**.
   
    c. Klikněte na **Další**.
8. Na **vlastností aplikace** dialogové okno, proveďte následující kroky:
   
    ![Vytváření aplikací](./media/active-directory-identityprotection-graph-getting-started/tutorial_general_06.png)
   
    a. V **přihlašovací adresa URL** textovému poli, typ `http://localhost`.
   
    b. V **identifikátor ID URI aplikace** textovému poli, typ `http://localhost`.
   
    c. Klikněte na **Dokončit**.

Můžete teď konfigurovat vaší aplikace.

![Vytváření aplikací](./media/active-directory-identityprotection-graph-getting-started/tutorial_general_07.png)

## <a name="grant-your-application-permission-to-use-the-api"></a>Udělit oprávnění vaše aplikace k používání rozhraní API
1. Na stránce vaší aplikace, v nabídce v horní části klikněte na **konfigurace**. 
   
    ![Vytváření aplikací](./media/active-directory-identityprotection-graph-getting-started/tutorial_general_08.png)
2. V **oprávnění k ostatním aplikacím** klikněte na tlačítko **přidat aplikaci**.
   
    ![Vytváření aplikací](./media/active-directory-identityprotection-graph-getting-started/tutorial_general_09.png)
3. Na **oprávnění k ostatním aplikacím** dialogové okno, proveďte následující kroky:
   
    ![Vytváření aplikací](./media/active-directory-identityprotection-graph-getting-started/tutorial_general_10.png)
   
    a. Vyberte **Microsoft Graph**.
   
    b. Klikněte na **Dokončit**.
4. Klikněte na tlačítko **oprávnění aplikací: 0**a potom vyberte **čtení informací o události riziko všechny identity**.
   
    ![Vytváření aplikací](./media/active-directory-identityprotection-graph-getting-started/tutorial_general_11.png)
5. V dolní části stránky klikněte na **Uložit**.
   
    ![Vytváření aplikací](./media/active-directory-identityprotection-graph-getting-started/tutorial_general_12.png)

## <a name="get-an-access-key"></a>Získání přístupového klíče
1. Na stránce vaší aplikace v **klíče** vyberte 1 rok jako doba trvání.
   
    ![Vytváření aplikací](./media/active-directory-identityprotection-graph-getting-started/tutorial_general_13.png)
2. V dolní části stránky klikněte na **Uložit**.
   
    ![Vytváření aplikací](./media/active-directory-identityprotection-graph-getting-started/tutorial_general_12.png)
3. v části klíče hodnotu nově vytvořený klíč zkopírujte a vložte jej do bezpečného umístění.
   
    ![Vytváření aplikací](./media/active-directory-identityprotection-graph-getting-started/tutorial_general_14.png)
   
   > [!NOTE]
   > Pokud tento klíč ztratíte, budete muset vraťte do této části a vytvořte nový klíč. Zachovat tento klíč tajný klíč: každý, kdo je přístup ke svým datům.
   > 
   > 
4. V **vlastnosti** část, zkopírujte **ID klienta**a pak ji vložit do bezpečného umístění. 

## <a name="authenticate-to-microsoft-graph-and-query-the-identity-risk-events-api"></a>Ověřování do aplikace Microsoft Graph a dotaz rozhraní API Identity riziko události
V tomto okamžiku byste měli mít:

* ID klienta, které jste zkopírovali výše
* Klíč, který jste zkopírovali výše
* Název domény vašeho klienta

K ověření, odeslat požadavek post do `https://login.microsoft.com` s následujícími parametry v textu:

* grant_type: "**client_credentials**"
* prostředek: "**https://graph.microsoft.com**"
* client_id:<your client ID>
* tajný klíč client_secret:<your key>

> [!NOTE]
> Je nutné zadat hodnoty **client_id** a **tajný klíč client_secret** parametr.
> 
> 

V případě úspěchu vrátí ověřovací token.  
Pro volání rozhraní API, vytvoří hlavičku s následující parametr:

    `Authorization`=”<token_type> <access_token>"


Při ověřování, můžete najít typ tokenu a přístupový token v vrácený tokenu.

Odeslat tuto hlavičku jako požadavek na následující adresu URL rozhraní API:`https://graph.microsoft.com/beta/identityRiskEvents`

Odpověď, a to v případě úspěchu je kolekce identity rizikových událostech a přidružená data ve formátu OData JSON, který lze analyzovat a zpracovávají podle svých potřeb.

Tady je ukázkový kód pro ověřování a volání rozhraní API pomocí prostředí Powershell.  
Stačí přidat vaše ID klienta, klíče a klienta domény.

    $ClientID       = "<your client ID here>"        # Should be a ~36 hex character string; insert your info here
    $ClientSecret   = "<your client secret here>"    # Should be a ~44 character string; insert your info here
    $tenantdomain   = "<your tenant domain here>"    # For example, contoso.onmicrosoft.com

    $loginURL       = "https://login.microsoft.com"
    $resource       = "https://graph.microsoft.com"

    $body       = @{grant_type="client_credentials";resource=$resource;client_id=$ClientID;client_secret=$ClientSecret}
    $oauth      = Invoke-RestMethod -Method Post -Uri $loginURL/$tenantdomain/oauth2/token?api-version=1.0 -Body $body

    Write-Output $oauth

    if ($oauth.access_token -ne $null) {
        $headerParams = @{'Authorization'="$($oauth.token_type) $($oauth.access_token)"}

        $url = "https://graph.microsoft.com/beta/identityRiskEvents"
        Write-Output $url

        $myReport = (Invoke-WebRequest -UseBasicParsing -Headers $headerParams -Uri $url)

        foreach ($event in ($myReport.Content | ConvertFrom-Json).value) {
            Write-Output $event
        }

    } else {
        Write-Host "ERROR: No Access Token"
    } 


## <a name="next-steps"></a>Další kroky
Blahopřejeme, jste právě provedli vaše první volání Microsoft Graph.  
Nyní můžete dotaz identity rizikových událostech a použije data, ale svých potřeb.

Další informace o Microsoft Graph a jak vytvářet aplikace, které používají rozhraní Graph API, podívejte se [dokumentace](https://graph.microsoft.io/docs) zdaleka na [Microsoft Graph lokality](https://graph.microsoft.io/). Také si nezapomeňte bookmark [Azure AD Identity Protection API](https://graph.microsoft.io/docs/api-reference/beta/resources/identityprotection_root) stránky, která obsahuje seznam všech dostupných v grafu Identity ochrany pro rozhraní API. Jak jsme přidat nové způsoby, jak pracovat s ochranou Identity prostřednictvím rozhraní API, uvidíte je na této stránce.

## <a name="additional-resources"></a>Další zdroje
* [Ochrany identit Azure Active Directory](active-directory-identityprotection.md)
* [Typy rizikových událostí detekovaných službou Azure Active Directory Identity Protection](active-directory-identityprotection-risk-events-types.md)
* [Microsoft Graph](https://graph.microsoft.io/)
* [Přehled Microsoft Graph](https://graph.microsoft.io/docs)
* [Kořenovém adresáři služby Azure AD Identity Protection](https://graph.microsoft.io/docs/api-reference/beta/resources/identityprotection_root)

