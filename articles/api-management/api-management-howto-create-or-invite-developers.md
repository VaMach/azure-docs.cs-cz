---
title: "Jak spravovat uživatelské účty v Azure API Management | Microsoft Docs"
description: "Naučte se vytvořit nebo pozvat uživatele ve službě Azure API Management"
services: api-management
documentationcenter: 
author: juliako
manager: cfowler
editor: 
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/30/2017
ms.author: apimpm
ms.openlocfilehash: 6f2fd5e4c1a51fe9d1652c9970bcd8d76b25ab60
ms.sourcegitcommit: b854df4fc66c73ba1dd141740a2b348de3e1e028
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 12/04/2017
---
# <a name="how-to-manage-user-accounts-in-azure-api-management"></a>Správa uživatelských účtů ve službě Azure API Management
Vývojáři ve službě API Management, jsou uživatelé rozhraní API, která vystavit použití služby API Management. Tato příručka obsahuje k vytváření a zvaní vývojářů používat rozhraní API a produkty, abyste vytvořili pro ně k dispozici s vaší instance služby API Management. Informace o správě uživatelských účtů prostřednictvím kódu programu, najdete v článku [entitu uživatele](https://msdn.microsoft.com/library/azure/dn776330.aspx) dokumentaci v [rozhraní API služby REST pro správu](https://msdn.microsoft.com/library/azure/dn776326.aspx) odkaz.

## <a name="create-developer"></a>Vytvořit nové vývojáře
Chcete-li vytvořit nové vývojáře, klikněte na tlačítko **portál vydavatele** služby API Management na portálu Azure. Tím přejdete na portál vydavatele služby API Management. Pokud jste instanci služby API Management ještě nevytvořili, přečtěte si téma [vytvoření instance API Management][Create an API Management service instance].

![Portál vydavatele][api-management-management-console]

Klikněte na tlačítko **uživatelé** z **API Management** nabídky na levé straně a pak klikněte na **přidat uživatele**.

![Vytvoření vývojáře][api-management-create-developer]

Zadejte **e-mailu**, **heslo**, a **název** pro nové vývojáře a klikněte na tlačítko **Uložit**.

![Vytvoření vývojáře][api-management-add-new-user]

Ve výchozím nastavení, jsou nově vytvořený vývojářským účtům **Active**a přidružené **vývojáři** skupiny.

![Nové vývojáře][api-management-new-developer]

Vývojářským účtům, které jsou v **active** stavu lze použít pro přístup k veškerému rozhraní API, k němuž mají odběry. Nově vytvořený vývojáře přidružit další skupiny, najdete v tématu [postup přidružení skupin k vývojářům][How to associate groups with developers].

## <a name="invite-developer"></a>Pozvat vývojář
Pozvaným vývojář, klikněte na tlačítko **uživatelé** z **API Management** nabídky na levé straně a pak klikněte na **pozvat uživatele**.

![Pozvěte vývojáře][api-management-invite-developer]

Zadejte jméno a e-mailovou adresu od vývojářů a klikněte na tlačítko **pozvat**.

![Pozvěte vývojáře][api-management-invite-developer-window]

Zobrazí se zpráva s potvrzením, ale nově pozvané vývojáře nejsou uvedené v seznamu až po jejich přijetí pozvánky. 

![Pozvěte potvrzení][api-management-invite-developer-confirmation]

Když je pozvat vývojář, e-mail je odeslán pro vývojáře. Tento e-mail je generována pomocí šablony a přizpůsobit. Další informace najdete v tématu [konfigurovat e-mailových šablon][Configure email templates].

Po přijetí pozvání se stane aktivní účet.

## <a name="block-developer"></a> , Deaktivujte nebo opětovnou aktivací vývojářského účtu
Ve výchozím nastavení, jsou nově vytvořené nebo pozvané vývojářským účtům **Active**. Chcete-li deaktivovat účet pro vývojáře, klikněte na tlačítko **bloku**. Chcete-li znovu aktivovat účet zablokovaný vývojáře, klikněte na tlačítko **aktivovat**. Blokované vývojářského účtu nelze přístup k portálu pro vývojáře nebo volání všechny rozhraní API. Chcete-li odstranit uživatelský účet, klikněte na tlačítko **odstranit**.

![Blok vývojáře][api-management-new-developer]

## <a name="reset-a-user-password"></a>Resetování hesla uživatele
Pokud chcete resetovat heslo pro uživatelský účet, klikněte na název účtu.

![Resetování hesla][api-management-view-developer]

Klikněte na tlačítko **resetovat heslo** poslat odkaz uživateli, aby obnovit své heslo.

![Resetování hesla][api-management-reset-password]

Prostřednictvím kódu programu pracovat s uživatelskými účty, najdete v článku [entitu uživatele](https://msdn.microsoft.com/library/azure/dn776330.aspx) dokumentaci v [rozhraní API služby REST pro správu](https://msdn.microsoft.com/library/azure/dn776326.aspx) odkaz. Pokud chcete resetovat heslo uživatelského účtu na určitou hodnotu, můžete použít [aktualizaci uživatele](https://msdn.microsoft.com/library/azure/dn776330.aspx#UpdateUser) operaci a zadejte požadované heslo.

## <a name="pending-verification"></a>Čeká na ověření
![Čeká na ověření][api-management-pending-verification]

## <a name="next-steps"></a>Další kroky
Jakmile se vytvoří účet pro vývojáře, můžete přiřadit k rolím a přihlášení k odběru produktů a rozhraní API. Další informace najdete v tématu [postup vytvoření a používání skupin][How to create and use groups].

[api-management-management-console]: ./media/api-management-howto-create-or-invite-developers/api-management-management-console.png
[api-management-add-new-user]: ./media/api-management-howto-create-or-invite-developers/api-management-add-new-user.png
[api-management-create-developer]: ./media/api-management-howto-create-or-invite-developers/api-management-create-developer.png
[api-management-invite-developer]: ./media/api-management-howto-create-or-invite-developers/api-management-invite-developer.png
[api-management-new-developer]: ./media/api-management-howto-create-or-invite-developers/api-management-new-developer.png
[api-management-invite-developer-window]: ./media/api-management-howto-create-or-invite-developers/api-management-invite-developer-window.png
[api-management-invite-developer-confirmation]: ./media/api-management-howto-create-or-invite-developers/api-management-invite-developer-confirmation.png
[api-management-pending-verification]: ./media/api-management-howto-create-or-invite-developers/api-management-pending-verification.png
[api-management-view-developer]: ./media/api-management-howto-create-or-invite-developers/api-management-view-developer.png
[api-management-reset-password]: ./media/api-management-howto-create-or-invite-developers/api-management-reset-password.png


[Create a new developer]: #create-developer
[Invite a developer]: #invite-developer
[Deactivate or reactivate a developer account]: #block-developer
[Next steps]: #next-steps
[How to create and use groups]: api-management-howto-create-groups.md
[How to associate groups with developers]: api-management-howto-create-groups.md#associate-group-developer

[Get started with Azure API Management]: get-started-create-service-instance.md
[Create an API Management service instance]: get-started-create-service-instance.md
[Configure email templates]: api-management-howto-configure-notifications.md#email-templates
