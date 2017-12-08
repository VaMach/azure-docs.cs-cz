---
title: "Požadavky na přístup k Azure AD reporting rozhraní API | Microsoft Docs"
description: "Další informace o požadavcích pro přístup k Azure AD reporting rozhraní API"
services: active-directory
documentationcenter: 
author: MarkusVi
manager: femila
editor: 
ms.assetid: ada19f69-665c-452a-8452-701029bf4252
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 11/14/2017
ms.author: dhanyahk;markvi
ms.reviewer: dhanyahk
ms.openlocfilehash: 9fbcac4bd6240aed607c3da6c6535d207a782448
ms.sourcegitcommit: a036a565bca3e47187eefcaf3cc54e3b5af5b369
ms.translationtype: HT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/17/2017
---
# <a name="prerequisites-to-access-the-azure-ad-reporting-api"></a>Požadavky na přístup k Azure AD reporting rozhraní API

[Rozhraní API pro generování sestav Azure AD](https://msdn.microsoft.com/library/azure/ad/graph/howto/azure-ad-reports-and-events-preview) poskytují programový přístup k datům prostřednictvím sady založené na REST API. Tato rozhraní API můžete volat z nejrůznějších programovacích jazyků a nástrojů.

Generování sestav používá rozhraní API [OAuth](https://msdn.microsoft.com/library/azure/dn645545.aspx) autorizovat přístup k webovému rozhraní API. 

Chcete-li získat přístup k data pro generování sestav prostřednictvím rozhraní API, musíte mít jeden z následujících role přiřazené:

- Čtečka zabezpečení
- Správce zabezpečení
- Globální správce.


Chcete-li připravit váš přístup k rozhraní API pro vytváření sestav, postupujte takto:

1. Registrace aplikace 
2. Udělení oprávnění 
3. Shromážděte nastavení konfigurace 

Pro dotazy, problémy nebo připomínky, prosím [souboru lístek podpory](https://docs.microsoft.com/en-us/azure/active-directory/active-directory-troubleshooting-support-howto).

## <a name="register-an-azure-active-directory-application"></a>Zaregistrovat aplikaci Azure Active Directory

Je třeba zaregistrovat aplikaci i v případě, že se připojujete ke generování sestav rozhraní API pomocí skriptu. To vám dává **ID aplikace**, což je vyžadováno pro volání autorizace a umožňuje kódu přijímat tokeny.

Ke konfiguraci adresáře pro přístup k Azure AD reporting rozhraní API, musíte se přihlásit k portálu Azure pomocí účtu správce služby Azure, který je taky členem skupiny **globálního správce** role adresáře v klientovi služby Azure AD.

> [!IMPORTANT]
> Aplikace běžící v části přihlašovací údaje s oprávněním "admin", jako to může být velmi výkonné, takže Přesvědčte se, k zabezpečení přihlašovacích údajů ID nebo tajný klíč aplikace.
> 


**Zaregistrovat aplikaci Azure Active Directory:**

1. V [portál Azure](https://portal.azure.com), v levém navigačním podokně klikněte na tlačítko **Azure Active Directory**.
   
    ![Registrace aplikace](./media/active-directory-reporting-api-prerequisites-azure-portal/01.png) 

2. Na **Azure Active Directory** okně klikněte na tlačítko **registrace aplikace**.

    ![Registrace aplikace](./media/active-directory-reporting-api-prerequisites-azure-portal/02.png) 

3. Na **registrace aplikace** , na panelu nástrojů v horní části klikněte na **nové registrace aplikace**.

    ![Registrace aplikace](./media/active-directory-reporting-api-prerequisites-azure-portal/03.png)

4. Na **vytvořit** okno, proveďte následující kroky:

    ![Registrace aplikace](./media/active-directory-reporting-api-prerequisites-azure-portal/04.png)

    a. V **název** textovému poli, typ `Reporting API application`.

    b. Jako **typ aplikace**, vyberte **webovou aplikaci nebo API**.

    c. V **přihlašovací adresa URL** textovému poli, typ `https://localhost`.

    d. Klikněte na možnost **Vytvořit**. 


## <a name="grant-permissions"></a>Udělení oprávnění 

Cílem tohoto kroku je udělit aplikaci **čtení dat adresáře** oprávnění **Windows Azure Active Directory** rozhraní API.

![Registrace aplikace](./media/active-directory-reporting-api-prerequisites-azure-portal/16.png)
 

**Udělení oprávnění vaše aplikace používat rozhraní API:**

1. Na **registrace aplikace** okno, v seznamu aplikací klepněte na tlačítko **aplikace Reporting rozhraní API**.

2. Na **aplikace Reporting rozhraní API** , na panelu nástrojů v horní části klikněte na **nastavení**. 

    ![Registrace aplikace](./media/active-directory-reporting-api-prerequisites-azure-portal/05.png)

3. Na **nastavení** okně klikněte na tlačítko **požadovaná oprávnění**. 

    ![Registrace aplikace](./media/active-directory-reporting-api-prerequisites-azure-portal/06.png)

4. Na **požadovaná oprávnění** okno v **rozhraní API** seznamu, klikněte na tlačítko **Windows Azure Active Directory**. 

    ![Registrace aplikace](./media/active-directory-reporting-api-prerequisites-azure-portal/07.png)

5. Na **povolit přístup** vyberte **čtení dat adresáře**. 

    ![Registrace aplikace](./media/active-directory-reporting-api-prerequisites-azure-portal/08.png)

6. Na panelu nástrojů v horní části klikněte na tlačítko **Uložit**.

    ![Registrace aplikace](./media/active-directory-reporting-api-prerequisites-azure-portal/15.png)

7. Klikněte na tlačítko **udělit oprávnění**a potom klikněte na **Ano**.

    ![Registrace aplikace](./media/active-directory-reporting-api-prerequisites-azure-portal/17.png)


## <a name="gather-configuration-settings"></a>Shromážděte nastavení konfigurace 
V této části se dozvíte, jak získat z adresáře následující nastavení:

* Název domény
* ID klienta
* Tajný klíč klienta

Je nutné tyto hodnoty při konfiguraci volání do rozhraní API pro generování sestav. 

### <a name="get-your-domain-name"></a>Získat název domény

**Pokud chcete získat název domény:**

1. V [portál Azure](https://portal.azure.com), v levém navigačním podokně klikněte na tlačítko **Azure Active Directory**.
   
    ![Registrace aplikace](./media/active-directory-reporting-api-prerequisites-azure-portal/01.png) 

2. Na **Azure Active Directory** okně klikněte na tlačítko **vlastní názvy domén**.

    ![Registrace aplikace](./media/active-directory-reporting-api-prerequisites-azure-portal/09.png) 

3. Zkopírujte názvu domény ze seznamu domén.


### <a name="get-your-applications-client-id"></a>Získejte ID klienta aplikace

**Pokud chcete získat ID klienta aplikace:**

1. V [portál Azure](https://portal.azure.com), v levém navigačním podokně klikněte na tlačítko **Azure Active Directory**.
   
    ![Registrace aplikace](./media/active-directory-reporting-api-prerequisites-azure-portal/01.png) 

2. Na **registrace aplikace** okno, v seznamu aplikací klepněte na tlačítko **aplikace Reporting rozhraní API**.

3. Na **aplikace Reporting rozhraní API** okně na **ID aplikace**, klikněte na tlačítko **kliknutím zkopírujte**.

    ![Registrace aplikace](./media/active-directory-reporting-api-prerequisites-azure-portal/11.png) 



### <a name="get-your-applications-client-secret"></a>Získat sdílený tajný klíč klienta vaší aplikace
Získat sdílený tajný klíč klienta aplikace, musíte vytvořit nový klíč a uložit jeho hodnota při ukládání nového klíče, protože není možné později už načíst tuto hodnotu.

**Pokud chcete získat sdílený tajný klíč klienta aplikace:**

1. V [portál Azure](https://portal.azure.com), v levém navigačním podokně klikněte na tlačítko **Azure Active Directory**.
   
    ![Registrace aplikace](./media/active-directory-reporting-api-prerequisites-azure-portal/01.png) 

2. Na **registrace aplikace** okno, v seznamu aplikací klepněte na tlačítko **aplikace Reporting rozhraní API**.


3. Na **aplikace Reporting rozhraní API** , na panelu nástrojů v horní části klikněte na **nastavení**. 

    ![Registrace aplikace](./media/active-directory-reporting-api-prerequisites-azure-portal/05.png)

4. Na **nastavení** okno v **APIR přístup** klikněte na tlačítko **klíče**. 

    ![Registrace aplikace](./media/active-directory-reporting-api-prerequisites-azure-portal/12.png)


5. Na **klíče** okno, proveďte následující kroky:

    ![Registrace aplikace](./media/active-directory-reporting-api-prerequisites-azure-portal/14.png)

    a. V **popis** textovému poli, typ `Reporting API`.

    b. Jako **Expires**, vyberte **ve 2 roky**.

    c. Klikněte na **Uložit**.

    d. Zkopírujte hodnotu klíče.


## <a name="next-steps"></a>Další kroky
* Chcete pro přístup k datům z Azure AD reporting rozhraní API programové způsobem? Podívejte se na [Začínáme s Azure Active Directory Reporting API](active-directory-reporting-api-getting-started.md).
* Pokud chcete získat další informace o vytváření sestav Azure Active Directory, přečtěte si téma [Azure Active Directory průvodce vytvářením sestav](active-directory-reporting-guide.md).  

