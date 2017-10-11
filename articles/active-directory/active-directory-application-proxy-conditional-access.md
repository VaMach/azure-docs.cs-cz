---
title: "Podmíněný přístup k místní aplikace – Azure AD | Microsoft Docs"
description: "Popisuje postup nastavení podmíněného přístupu pro aplikace, které publikujete nelze přistupovat vzdáleně přes Azure AD Application Proxy."
services: active-directory
documentationcenter: 
author: kgremban
manager: femila
ms.assetid: 2e97722b-eb4e-4078-b607-9fed210d8a0f
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/23/2017
ms.author: kgremban
ms.reviewer: harshja
ms.custom: it-pro; oldportal
ms.openlocfilehash: 463946256f9e335fa6d98fc904835e5c3dc2725e
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="working-with-conditional-access-in-azure-ad-application-proxy"></a>Práce s podmíněného přístupu v Azure AD Application Proxy

>[!NOTE]
>Tento článek se týká na portál Azure classic, které se postupně vyřazuje z provozu. Doporučujeme vám, že používáte [portál Azure](https://portal.azure.com). Na portálu Azure aplikace Proxy aplikací mít stejné funkce podmíněného přístupu, jako kterákoli jiná aplikace SaaS. Další informace o podmíněném přístupu najdete v tématu [Začínáme s podmíněným přístupem v Azure Active Directory](active-directory-conditional-access-azure-portal-get-started.md).

Můžete nakonfigurovat pravidla přístupu k udělení podmíněný přístup k aplikacím publikovat pomocí Proxy aplikací. To umožňuje:

* Požadovat použití vícefaktorového ověřování na aplikaci.
* Vyžadovat víceúrovňové ověřování jenom v případě, že uživatelé nejsou v práci
* Zabránění uživatelům v přístupu k aplikaci, pokud nejsou v práci

Tato pravidla můžete použít pro všechny uživatele a skupiny nebo jenom u konkrétních uživatelů a skupin. Ve výchozím nastavení toto pravidlo se vztahuje na všechny uživatele, kteří mají přístup k aplikaci. Ale pravidla lze také omezen na uživatele, kteří jsou členy zadané skupiny zabezpečení.  

Pravidla přístupu se vyhodnocují, když uživatel získá přístup k federované aplikaci, která používá OAuth 2.0, OpenID Connect, SAML nebo WS-Federation. Kromě toho pravidla přístupu k vyhodnocují s OAuth 2.0 a OpenID Connect, pokud obnovovací token se používá k získání tokenu přístupu.

## <a name="conditional-access-prerequisites"></a>Požadavky podmíněného přístupu
* Předplatné služby Azure Active Directory Premium
* Federovaný nebo spravovaný klienta Azure Active Directory
* Federovaných klientů vyžadovat vícefaktorové ověřování (MFA)  
    ![Konfigurace pravidel přístupu – vyžadovat vícefaktorové ověřování](./media/active-directory-application-proxy-conditional-access/application-proxy-conditional-access.png)

## <a name="configure-per-application-multi-factor-authentication"></a>Nakonfigurovat každou aplikaci služby Multi-Factor authentication
1. Přihlaste se jako správce na portálu Azure classic.
2. Přejděte do služby Active Directory a vyberte adresář, u kterého chcete povolit proxy aplikace.
3. Klikněte na tlačítko **aplikace** a přejděte dolů k položce **pravidla přístupu k** části. V části pravidla přístupu se zobrazí pouze pro aplikace publikované pomocí Proxy aplikací, které používají federovaného ověřování.
4. Povolit pravidlo výběrem **povolit pravidla přístupu k** k **na**.
5. Zadejte uživatele a skupiny, na které se vztahují pravidla. Použití **přidat skupinu** tlačítko vyberte jeden nebo více skupin, na které se vztahuje pravidlo přístupu. Tento dialog můžete použít také k odebrání vybrané skupiny.  Pravidla je vybrána pro použití na skupiny, se vynucují pravidla přístupu pouze pro uživatele, kteří patřit do jedné ze zadaných skupin zabezpečení.  

   * Abyste výslovně vyloučili skupin zabezpečení z tohoto pravidla, zkontrolujte **s výjimkou** a zadejte jednu nebo více skupin. Uživatelé, kteří jsou členy skupiny v seznamu Except není nutné provést službu Multi-Factor authentication.  
   * Pokud uživatel byl nakonfigurován pomocí funkce služby Multi-Factor authentication na uživatele, toto nastavení má přednost před pravidly vícefaktorového ověřování aplikace. Uživatel, který byl nakonfigurován pro jednotlivé uživatele služby Multi-Factor authentication je potřeba provést službu Multi-Factor authentication, i když mají byly vyloučené z pravidel aplikace služby Multi-Factor authentication. Další informace o [vícefaktorového ověřování a uživatelská nastavení](../multi-factor-authentication/multi-factor-authentication.md).
6. Vyberte pravidlo přístupu, kterou chcete nastavit:

   * **Vyžadovat vícefaktorové ověřování**: uživatelé, na které se vztahují pravidla přístupu k nutných k dokončení služby Multi-Factor authentication před přístupem k aplikaci, na které se pravidlo vztahuje.
   * **Vyžadovat vícefaktorové ověřování, když není v práci**: uživatelé pokusu o přístup k aplikacím z důvěryhodné adresy IP, nebudou muset provést službu Multi-Factor authentication. Na stránce nastavení služby Multi-Factor authentication lze nakonfigurovat důvěryhodné rozsahy IP adres.
   * **Blokování přístupu, když není v práci**: uživatelé pokusí o přístup k aplikaci z mimo podnikovou síť nebudou mít přístup k aplikaci.

## <a name="configuring-mfa-for-federation-services"></a>Konfigurace vícefaktorového ověřování pro služby federation
U federovaných klientů, může provést službu Multi-Factor authentication (MFA), Azure Active Directory nebo místní server AD FS. Ve výchozím nastavení provádí MFA na libovolné stránce hostované službou Azure Active Directory. Pokud chcete nakonfigurovat MFA na místě, spusťte prostředí Windows PowerShell a pomocí vlastnosti – SupportsMFA nastavení modulu Azure AD.

Následující příklad ukazuje, jak zapnout MFA místně pomocí [rutiny Set-MsolDomainFederationSettings](https://msdn.microsoft.com/library/azure/dn194088.aspx) u contoso.com klienta:`Set-MsolDomainFederationSettings -DomainName contoso.com -SupportsMFA $true `

Kromě nastavení tento příznak, je třeba instanci federovaného klienta AD FS provést službu Multi-Factor authentication. Postupujte podle pokynů pro [nasazení Microsoft Azure Multi-Factor authentication na místě](../multi-factor-authentication/multi-factor-authentication-get-started-server.md).

## <a name="see-also"></a>Viz také
* [Práce s aplikacemi využívajícími deklarace](active-directory-application-proxy-claims-aware-apps.md)
* [Publikování aplikací pomocí Proxy aplikace](active-directory-application-proxy-publish.md)
* [Povolení jednoduchého přihlášení](active-directory-application-proxy-sso-using-kcd.md)
* [Publikování aplikací s použitím vlastního názvu domény](active-directory-application-proxy-custom-domains.md)

Nejnovější novinky a aktualizace naleznete na [blogu proxy aplikace](http://blogs.technet.com/b/applicationproxyblog/)
