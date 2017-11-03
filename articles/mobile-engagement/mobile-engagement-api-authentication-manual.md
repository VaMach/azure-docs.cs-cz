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
ms.openlocfilehash: 9d6132e1a01be489b8e8e28a0219cf8a0b50b318
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/11/2017
---
# <a name="authenticate-with-mobile-engagement-rest-apis---manual-setup"></a>Ověření pomocí Mobile Engagement REST API – ruční instalaci
Toto je dokumentaci příloha [ověřit pomocí rozhraní API REST Mobile Engagement](mobile-engagement-api-authentication.md). Zajistěte, aby že si ji nejprve se získat kontext přečíst. Popisuje jiný způsob, jak provést jednorázové nastavení pro nastavení ověřování pro použití portálu Azure Mobile Engagement REST API. 

> [!NOTE]
> Následující pokyny jsou založené na tomto [služby Active Directory průvodce](../azure-resource-manager/resource-group-create-service-principal-portal.md) a přizpůsobit pro co je vyžadován pro ověření pro zapojení mobilních rozhraní API. Proto na ni odkazuje Pokud chcete zjistit, následující postup podrobně. 
> 
> 

1. Přihlášení k účtu Azure prostřednictvím [portálu classic](https://manage.windowsazure.com/).
2. Vyberte **služby Active Directory** v levém podokně.
   
     ![Vyberte možnost Active Directory][1]
3. Vyberte **výchozí služby Active Directory** na portálu Azure. 
   
     ![Vybrat adresář.][2]
   
   > [!IMPORTANT]
   > Tento postup funguje jenom v případě, že pracujete v výchozí služby Active Directory vašeho účtu a nebude fungovat, pokud je to dělají ve službě Active Directory, kterou jste vytvořili ve vašem účtu. 
   > 
   > 
4. Chcete-li zobrazit aplikace ve vašem adresáři, klikněte na **aplikace**.
   
     ![Zobrazit aplikace][3]
5. Klikněte na **přidat**. 
   
     ![Přidání aplikace][4]
6. Klikněte na **přidat aplikaci, kterou vyvíjí Moje organizace**
   
     ![Nová aplikace][5]
7. Zadejte název aplikace a vyberte typ aplikace jako **webové aplikace nebo webové rozhraní API** a klikněte na tlačítko Další.
   
     ![název aplikace][6]
8. Můžete zadat všechny fiktivní adresy URL pro **adresa URL přihlašování** a **identifikátor ID URI aplikace**. Nejsou použity pro náš scénář a adresy URL sami nejsou ověřené.  
   
     ![Vlastnosti aplikace][7]
9. Na konci tohoto budete mít aplikaci AAD pomocí názvu, který jste zadali dříve jako následující. Toto je vaše **AD\_aplikace\_název** a poznamenejte si ho.  
   
     ![Název aplikace.][8]
10. Klikněte na název aplikace a klikněte na **konfigurace**.
    
      ![Konfigurace aplikace][9]
11. Poznamenejte si ID klienta, který se použije jako **klienta\_ID** pro vaše rozhraní API volání. 
    
     ![Konfigurace aplikace][10]
12. Přejděte dolů k položce **klíče** a přidání klíče se nejlépe doba trvání 2 roky (vypršení platnosti) a klikněte na **Uložit**. 
    
     ![Konfigurace aplikace][11]
13. Okamžitě zkopírujte hodnotu, která je pro klíč zobrazen, jak se zobrazují pouze nyní a nejsou uložena, se někdy znovu nezobrazí. Pokud ho ztratíte budete muset vygenerovat nový klíč. To bude **tajný klíč CLIENT_SECRET** pro vaše rozhraní API volání. 
    
     ![Konfigurace aplikace][12]
    
    > [!IMPORTANT]
    > Tento klíč vyprší na konci dobu, která jste zadali, ujistěte se, že ho obnovit, když nastane čas jinak ověřování rozhraní API nebude fungovat. Můžete také odstranit a znovu vytvořit tento klíč, pokud se domníváte, že byl napaden.
    > 
    > 
14. Klikněte na **zobrazit koncové body** tlačítko nyní které se otevře **koncových bodů aplikace** dialogové okno. 
    
    ![][13]
15. Z dialogového okna aplikace koncové body zkopírovat **koncový bod TOKENU OAUTH 2.0**. 
    
    ![][14]
16. Tento koncový bod bude ve formuláři následující, kde je identifikátor GUID v adrese URL vaší **TENANT_ID** , poznamenejte si ji: 
    
        https://login.microsoftonline.com/<GUID>/oauth2/token
17. Nyní budeme pokračovat nakonfigurovat oprávnění v této aplikaci. Pro to budete muset otevře [portál Azure](https://portal.azure.com). 
18. Klikněte na **skupiny prostředků** a najděte **Mobile Engagement** skupinu prostředků.  
    
    ![][15]
19. Klikněte na tlačítko **Mobile Engagement** prostředků skupiny a přejděte do **nastavení** okno sem. 
    
    ![][16]
20. Klikněte na **uživatelé** v okně Nastavení a pak klikněte na **přidat** přidat uživatele. 
    
    ![][17]
21. Klikněte na **vybrat roli**
    
    ![][18]
22. Klikněte na **vlastníka**
    
    ![][19]
23. Vyhledávání pro název vaší aplikace **AD\_aplikace\_název** do vyhledávacího pole. Se nezobrazí ve výchozím nastavení v tomto poli. Po nalezení, vyberte ho a klikněte na **vyberte** v dolní části okna. 
    
    ![][20]
24. Na **přidat přístup** okně se zobrazí jako **1 uživatel, 0 skupiny**. Klikněte na tlačítko **OK** v tomto okně pro potvrzení změny. 
    
    ![][21]

Teď jste dokončili požadované konfigurace AAD a jsou všechny nastavíte na volání rozhraní API. 

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
[11]: ./media/mobile-engagement-api-authentication-manual/client_secret.png
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



