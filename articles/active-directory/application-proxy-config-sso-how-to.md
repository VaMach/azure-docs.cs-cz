---
title: "Postup konfigurace jednotného přihlašování k aplikaci Proxy aplikace | Microsoft Docs"
description: "Jak můžete nakonfigurovat jednotné přihlašování k vaší aplikaci proxy aplikace rychle"
services: active-directory
documentationcenter: 
author: ajamess
manager: femila
ms.assetid: 
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/11/2017
ms.author: asteen
ms.openlocfilehash: ccab427857b1439f37f3d9f193e35a4fc2237014
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="how-to-configure-single-sign-on-to-an-application-proxy-application"></a>Postup konfigurace jednotného přihlašování k aplikaci Proxy aplikace

Jednotné přihlašování (SSO) umožňuje uživatelům přístup k aplikaci bez ověřování vícekrát. Ji umožňuje ověření jednotného proběhnout v cloudu s Azure Active Directory a umožňuje služba nebo konektor zosobnění uživatele dokončit všechny další ověřování výzvy z aplikace.

## <a name="how-to-configure-single-sign-on"></a>Postup konfigurace na jednotné přihlašování
Pokud chcete nakonfigurovat jednotné přihlašování, nejprve ujistěte se, že je vaše aplikace nakonfigurovaná pro předběžné ověření pomocí služby Azure Active Directory. Chcete-li to provést, přejděte na **Azure Active Directory**  - &gt; **podnikové aplikace, které**  - &gt; **všechny aplikace**  - &gt; aplikace  **- &gt; Proxy aplikace**. Na této stránce naleznete v poli "Předběžné ověřování" a ujistěte se, který je nastaven na "Azure Active Directory. 

Další informace o předběžné ověření metody naleznete v tématu čtvrtý krok [publikování dokumentu aplikace](https://docs.microsoft.com/azure/active-directory/application-proxy-publish-azure-portal).

   ![Metoda předběžného ověřování na portálu Azure](./media/application-proxy-config-sso-how-to/app-proxy.png)

## <a name="configuring-single-sign-on-modes-for-application-proxy-applications"></a>Konfigurace režimy přihlašování pro aplikace Proxy aplikace
V dalším jsme nakonfigurujte konkrétní typ jednotného přihlašování. Metody přihlašování jsou klasifikovány podle jaký typ ověřování back-end aplikace používá. Proxy aplikace aplikace podporuje tři typy přihlašování:

-   **Založené na heslech přihlašování**: založené na heslech přihlášení lze použít pro každou aplikaci, která používá pole uživatelské jméno a heslo pro přihlášení. Kroky konfigurace naleznete v našem [heslo SSO konfigurace dokumentaci](https://docs.microsoft.com/azure/active-directory/active-directory-enterprise-apps-whats-new-azure-portal#bring-your-own-password-sso-applications).

-   **Integrované ověřování systému Windows**: pro aplikace pomocí integrovaného ověřování systému Windows (IWA), jednotné přihlašování je povolené prostřednictvím použitím protokolu Kerberos omezené delegování (KCD). To dává oprávnění konektory Proxy aplikace ve službě Active Directory k zosobnění uživatelů a odesílat a přijímat tokeny jejich jménem. Podrobné informace o konfiguraci použitím KCD najdete v [jednotné přihlašování s použitím KCD dokumentaci](https://docs.microsoft.com/azure/active-directory/active-directory-application-proxy-sso-using-kcd).

-   **Na základě záhlaví přihlašování**: na základě záhlaví přihlašování je povolená Díky partnerství a vyžadovat další konfiguraci. Podrobnosti o partnerství a podrobné pokyny pro konfiguraci jednotné přihlašování pro aplikace, která používá hlavičky pro ověřování najdete v tématu [PingAccess dokumentaci k Azure AD](https://docs.microsoft.com/azure/active-directory/application-proxy-ping-access).

Každá z těchto možností najdete tak, že přejdete do vaší aplikace v "Podnikové aplikace" a otevírání **jednotné přihlašování** stránky v levé nabídce. Všimněte si, že pokud vaše aplikace byla vytvořena na původním portálu, se nemusí zobrazit všechny tyto možnosti.

Na této stránce můžete také najdete v jednom další možnost přihlášení: propojené přihlášení. Tato možnost je také podporována službou Proxy aplikací. Uvědomte si však, že tato možnost nepřidá jednotného přihlašování k aplikaci. Ale nutné dodat, že aplikace už můžete mít jednotné přihlašování implementovaná pomocí jiné služby, jako je Active Directory Federation Services. 

Tato možnost umožňuje správce vytvořit odkaz na aplikaci první krajině tohoto uživatele na při přístupu k aplikaci. Například pokud je aplikace, který je nakonfigurovaný k ověřování uživatelů pomocí Active Directory Federation Services 2.0, správce slouží možnost "propojené přihlášení" k vytvoření odkazu na na panel přístupu.

## <a name="next-steps"></a>Další kroky
[Zadejte jednotné přihlašování pro vaše aplikace s Proxy aplikace](active-directory-application-proxy-sso-using-kcd.md)
