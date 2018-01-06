---
title: "Ověření pomocí Mobile Engagement REST API – ruční instalaci"
description: "Popisuje, jak ručně nastavit ověřování rozhraní REST API Mobile Engagement"
services: mobile-engagement
documentationcenter: mobile
author: piyushjo
manager: erikre
editor: 
ms.assetid: 2e79f9c9-41e4-45ac-b427-3b8338675163
ms.service: mobile-engagement
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: mobile-multiple
ms.workload: mobile
ms.date: 08/19/2016
ms.author: piyushjo
ms.openlocfilehash: 3b678acbae225c28223a2ee76e5be2462a529362
ms.sourcegitcommit: d6984ef8cc057423ff81efb4645af9d0b902f843
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/05/2018
---
# <a name="authenticate-with-mobile-engagement-rest-apis---manual-setup"></a>Ověření pomocí Mobile Engagement REST API – ruční instalaci
Tato dokumentace je dokumentaci příloha [ověřit pomocí rozhraní API REST Mobile Engagement](mobile-engagement-api-authentication.md). Zajistěte, aby že si ji nejprve se získat kontext přečíst.
Popisuje jiný způsob, jak provést jednorázové nastavení pro nastavení ověřování pro rozhraní API REST Mobile Engagement pomocí portálu Azure.

> [!NOTE]
> Následující pokyny jsou založené na tomto [služby Active Directory průvodce](../azure-resource-manager/resource-group-create-service-principal-portal.md) a přizpůsobit pro co je vyžadován pro ověření pro zapojení mobilních rozhraní API. Proto na ni odkazuje Pokud chcete zjistit, následující postup podrobně.

1. Přihlaste se k účtu Azure prostřednictvím [portál Azure](https://portal.azure.com/).
2. Vyberte **služby Active Directory** v levém podokně.

     ![Vyberte možnost Active Directory][1]

3. Chcete-li zobrazit aplikace ve vašem adresáři, klikněte na **registrace aplikace**.

     ![Zobrazit aplikace][3]

4. Klikněte na **nové registrace aplikace**.

     ![Přidání aplikace][4]

5. Zadejte název aplikace a ponechat typ aplikace jako **webové aplikace nebo rozhraní API** a klikněte na tlačítko Další. Můžete zadat všechny fiktivní adresy URL pro **adresa URL přihlašování**: nejsou použity pro tento scénář a adresy URL sami nejsou ověřené.
6. Po dokončení máte aplikaci Azure AD pomocí názvu, který jste zadali. Je vaše **AD\_aplikace\_název**, Zkontrolujte prosím, poznamenejte si ho.

     ![název aplikace][8]

7. Klikněte na název aplikace.
8. Najít **ID aplikace**, poznamenejte si ho, je možné ID klienta, který má být použita jako **klienta\_ID** pro vaše rozhraní API volání.

     ![Konfigurace aplikace][10]

9. Najít **klíče** části na pravé straně.

     ![Konfigurace aplikace][11]

10. Vytvořte nový klíč a okamžitě ho zkopírovat a uložit ji pro použití. Nikdy zobrazí se znovu.

     ![Konfigurace aplikace][12]

    > [!IMPORTANT]
    > Tento klíč vyprší na konci dobu, která jste zadali, ujistěte se, že ho obnovit, když nastane čas jinak ověřování rozhraní API nebude fungovat. Můžete také odstranit a znovu vytvořit tento klíč, pokud se domníváte, že byl napaden.
    >
    >
11. Klikněte na **koncové body** tlačítka v horní části stránky a zkopírujte **koncový bod TOKENU OAUTH 2.0**.

    ![][14]

16. Tento koncový bod bude ve formuláři následující, kde je identifikátor GUID v adrese URL vaší **TENANT_ID** , poznamenejte si ji:`https://login.microsoftonline.com/<GUID>/oauth2/token`
17. Nyní budeme pokračovat nakonfigurovat oprávnění v této aplikaci. Pro to budete muset otevře [portál Azure](https://portal.azure.com). 
18. Klikněte na **skupiny prostředků** a najděte **Mobile Engagement** skupinu prostředků.

    ![][15]

19. Klikněte na tlačítko **Mobile Engagement** prostředků skupiny a přejděte do **nastavení** části v tomto poli.

    ![][16]

20. Klikněte na **uživatelé** v nastavení části a potom klikněte na **přidat** přidat uživatele.

    ![][17]

21. Klikněte na **vyberte roli**.

    ![][18]

22. Klikněte na **vlastníka**.

    ![][19]

23. Vyhledávání pro název vaší aplikace **AD\_aplikace\_název** do vyhledávacího pole. Se nezobrazí ve výchozím nastavení v tomto poli. Po nalezení, vyberte ho a klikněte na **vyberte** v dolní části.

    ![][20]

24. Na **přidat přístup** části, se zobrazí jako **1 uživatel, 0 skupiny**. Klikněte na tlačítko **OK** v této části pro potvrzení změny.

    ![][21]

Teď jste dokončili požadované konfigurace služby Azure AD a jsou nastaveny na volání rozhraní API.

<!-- Images -->
[1]: ./media/mobile-engagement-api-authentication-manual/active-directory.png
[2]: ./media/mobile-engagement-api-authentication-manual/active-directory-details.png
[3]: ./media/mobile-engagement-api-authentication-manual/view-applications.png
[4]: ./media/mobile-engagement-api-authentication-manual/add-icon.png
[5]: ./media/mobile-engagement-api-authentication-manual/what-do-you-want-to-do.png
[6]: ./media/mobile-engagement-api-authentication-manual/tell-us-about-your-application.png
[7]: ./media/mobile-engagement-api-authentication-manual/app-properties.png
[8]: ./media/mobile-engagement-api-authentication-manual/aad-app.png
[9]: ./media/mobile-engagement-api-authentication-manual/configure-menu.png
[10]: ./media/mobile-engagement-api-authentication-manual/client-id.png
[11]: ./media/mobile-engagement-api-authentication-manual/client-secret.png
[12]: ./media/mobile-engagement-api-authentication-manual/keys.png
[13]: ./media/mobile-engagement-api-authentication-manual/view-endpoints.png
[14]: ./media/mobile-engagement-api-authentication-manual/app-endpoints.png
[15]: ./media/mobile-engagement-api-authentication-manual/resource-groups.png
[16]: ./media/mobile-engagement-api-authentication-manual/resource-groups-settings.png
[17]: ./media/mobile-engagement-api-authentication-manual/add-users.png
[18]: ./media/mobile-engagement-api-authentication-manual/add-role.png
[19]: ./media/mobile-engagement-api-authentication-manual/select-role.png
[20]: ./media/mobile-engagement-api-authentication-manual/add-user-select.png
[21]: ./media/mobile-engagement-api-authentication-manual/add-access-final.png



