---
title: "Vlastní domény v Azure AD Application Proxy | Microsoft Docs"
description: "Správa vlastních domén v Azure AD Application Proxy tak, aby adresa URL pro aplikaci je stejný bez ohledu na to, kde uživatelé k němu přístup."
services: active-directory
documentationcenter: 
author: daveba
manager: mtillman
ms.assetid: 2fe9f895-f641-4362-8b27-7a5d08f8600f
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/11/2017
ms.author: daveba
ms.reviewer: harshja
ms.custom: it-pro
ms.openlocfilehash: 6a5b7731cfd98a53f83a9882529a713381b4f848
ms.sourcegitcommit: 3cdc82a5561abe564c318bd12986df63fc980a5a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/05/2018
---
# <a name="working-with-custom-domains-in-azure-ad-application-proxy"></a>Práce s vlastní domény v Azure AD Application Proxy

Při publikování aplikace prostřednictvím Proxy aplikace služby Active Directory Azure můžete vytvořit externí adresu URL pro vaše uživatele, který se má přejít při pracují vzdáleně. Tato adresa URL získá výchozí doménu *yourtenant.msappproxy.net*. Například pokud jste publikovali aplikaci s názvem výdajů a váš klient je s názvem Contoso, pak je externí adresa URL by byl https://expenses-contoso.msappproxy.net. Pokud chcete použít vlastní název domény, nakonfigurujte vlastní doménu pro vaši aplikaci. 

Doporučujeme, abyste nastavili vlastní domény pro vaše aplikace, pokud je to možné. Mezi výhody vlastní domény patří:

- Uživatelé dostanou do aplikace se stejnou adresu URL, jestli pracují uvnitř nebo vně vaší sítě.
- Pokud všechny aplikace mají stejné interní a externí adresy URL, pak odkazy v jedné aplikace, které odkazují na jiný nadále pracovat i mimo podnikovou síť. 
- Řízení branding a vytvoření adresy URL, které chcete. 


## <a name="configure-a-custom-domain"></a>Konfigurace vlastní domény

### <a name="prerequisites"></a>Požadavky

Než začnete konfigurovat vlastní doménu, ujistěte se, abyste měli připravit následující požadavky: 
- A [ověřit doménu přidat do Azure Active Directory](active-directory-domains-add-azure-portal.md).
- Vlastní certifikát pro doménu, ve formátu souboru PFX. 
- Místní aplikace [publikované prostřednictvím Proxy aplikace](application-proxy-publish-azure-portal.md).

### <a name="configure-your-custom-domain"></a>Konfigurace vlastní domény

Až budete mít tyto tři požadavky připraven, nastavit vlastní domény takto:

1. Přihlaste se k webu [Azure Portal](https://portal.azure.com).
2. Přejděte na **Azure Active Directory** > **podnikové aplikace, které** > **všechny aplikace** a vyberte aplikaci, kterou chcete spravovat.
3. Vyberte **Proxy aplikace**. 
4. V poli externí adresu URL pomocí rozevíracího seznamu vyberte vaši vlastní doménu. Pokud nevidíte doménu v seznamu, pak jej nebyla ověřena ještě. 
5. Vyberte **uložit**
5. **Certifikát** nepovolíte pole, která byla zakázána. Toto políčko zaškrtněte. 

   ![Klikněte na tlačítko pro odeslání certifikátu](./media/active-directory-application-proxy-custom-domains/certificate.png)

   Pokud jste již odeslali certifikát pro tuto doménu, pole certifikátu zobrazí informace o certifikátu. 

6. Nahrát na server certifikát PFX a zadejte heslo pro certifikát. 
7. Vyberte **Uložit** uložte provedené změny. 
8. Přidat [záznam DNS](../dns/dns-operations-recordsets-portal.md) , přesměruje nový externí adresu URL k doméně msappproxy.net. 

>[!TIP] 
>Potřebujete jenom nahrát jeden certifikát na vlastní domény. Jakmile nahrajete certifikát, můžete zvolit vlastní domény při publikování nové aplikace a není nutné provést další konfiguraci, s výjimkou záznamu DNS. 

## <a name="manage-certificates"></a>Správa certifikátů

### <a name="certificate-format"></a>Formát certifikátu
Neexistuje žádné omezení v certifikátu podpis metody. Šifrování ECC (Elliptic Curve), alternativní název předmětu (SAN) a další běžné typy certifikátů jsou všechny podporované. 

Certifikát se zástupným znakem můžete použít také zástupného odpovídá požadované externí adresu URL. 

### <a name="changing-the-domain"></a>Změna domény
V rozevíracím seznamu externí adresu URL pro vaši aplikaci se zobrazí všechny ověřené domény. Změnit doménu, právě aktualizujte toto pole pro aplikaci. Pokud chcete doménu není v seznamu [přidejte jej jako ověřené domény](active-directory-domains-add-azure-portal.md). Pokud vyberete doméně, která mají přidružený certifikát ještě, postupujte podle kroků 5 až 7 se přidat certifikát. Ujistěte se, že aktualizujete záznam DNS pro přesměrování z nové externí adresu URL. 

### <a name="certificate-management"></a>Správa certifikátů
Pokud aplikace sdílet externího hostitele, můžete použít stejný certifikát pro více aplikací. 

Se zobrazí upozornění, když vyprší platnost certifikátu, s výzvou k odeslání dalšího certifikátu prostřednictvím portálu. Pokud je certifikát odvolán, může se uživatelům zobrazí upozornění zabezpečení při přístupu k aplikaci. Neprovádějte jsme kontroly odvolání certifikátů.  Chcete-li aktualizovat certifikát pro danou aplikaci, přejděte do aplikace a postupujte podle kroků 5 až 7 pro konfiguraci vlastních domén na publikované aplikace nahrát nový certifikát. Pokud starý certifikát není používán jinými aplikacemi, je automaticky odstraněna. 

Aktuálně všechny správy certifikátů je prostřednictvím stránky jednotlivých aplikací, takže budete muset spravovat certifikáty v kontextu příslušné aplikace. 

## <a name="next-steps"></a>Další postup
* [Povolit jednotné přihlašování](active-directory-application-proxy-sso-using-kcd.md) pro vaše publikované aplikace pomocí ověřování Azure AD.
* [Povolení podmíněného přístupu](application-proxy-enable-remote-access-sharepoint.md) k publikovaným aplikacím.
* [Přidání vlastního názvu domény do Azure AD](active-directory-domains-add-azure-portal.md)


