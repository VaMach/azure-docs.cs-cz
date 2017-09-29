---
title: "Správa přístupu uživatelů pomocí kontrol přístupu Azure AD | Dokumentace Microsoftu"
description: "Zjistěte, jak spravovat přístup uživatelů jako členství ve skupině nebo přiřazení k aplikaci s využitím kontrol přístupu Azure Active Directory."
services: active-directory
documentationcenter: 
author: markwahl-msft
manager: femila
editor: 
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 09/19/2017
ms.author: billmath
ms.translationtype: HT
ms.sourcegitcommit: 8f9234fe1f33625685b66e1d0e0024469f54f95c
ms.openlocfilehash: 0459eb5cc71939202c8491f6b2714e28bd8e202d
ms.contentlocale: cs-cz
ms.lasthandoff: 09/20/2017

---

# <a name="manage-user-access-with-azure-ad-access-reviews"></a>Správa přístupu uživatelů pomocí kontrol přístupu Azure AD

Azure Active Directory vám umožní snadno zajistit, aby vaši uživatelé měli odpovídající přístup. Požádá uživatele samotné nebo pracovníky s rozhodovací pravomocí, aby se zúčastnili kontroly přístupu a znovu certifikovali (nebo potvrdili) přístup uživatele.  Revidující se mohou vyjádřit k nutnosti dalšího přístupu pro jednotlivé uživatele, a to na základě návrhů z Azure AD. Až kontrola přístupu skončí, můžete provést změny a ukončit přístup uživatelů, kteří ho už nepotřebují.

> [!NOTE]
> Pokud chcete kontrolovat přístup jenom u uživatelů typu host a nekontrolovat všechny typy uživatelského přístupu, přečtěte si téma věnované [správě přístupu uživatelů typu host s využitím kontroly přístupu](active-directory-azure-ad-controls-manage-guest-access-with-access-reviews.md).  A pokud chcete kontrolovat členství uživatelů v rolích pro správu, jako je například globální správce, přečtěte si téma věnované [spuštění kontroly přístupu v Azure AD PIM](active-directory-privileged-identity-management-how-to-start-security-review.md). 
>
>

## <a name="prerequisites"></a>Požadavky 

Kontroly přístupu jsou dostupné v edici Premium P2 služby Azure Active Directory, která je součástí EMS E5. Další informace najdete v článku [Edice služby Azure Active Directory](active-directory-editions.md).  Každý uživatel, který komunikuje s touto funkcí při vytváření kontroly, kontrole přístupu nebo použití kontroly, musí mít licenci.


## <a name="creating-and-performing-an-access-review"></a>Vytvoření a provedení kontroly přístupu

V rámci kontroly přístupu může revidujícími být mít jednoho nebo několik uživatelů jako revidující.  

1. Vyberte skupinu v Azure Active Directory, který má jednoho nebo několik členů, nebo aplikaci připojenou k Azure Active Directory, ke které je přiřazený jeden nebo několik uživatelů. 
2. Rozhodněte, jestli má každý uživatel kontrolovat svůj vlastní přístup, nebo jestli má jeden nebo několik uživatelů kontrolovat přístup všech.
3. Povolte zobrazení kontrol přístupu na přístupových panelech kontrolora.  Jako globální správce přejděte na [stránku kontrol přístupu](https://portal.azure.com/#blade/Microsoft_AAD_ERM/DashboardBlade/). 
4. Začněte kontrolu přístupu. Další informace najdete v tématu věnovaném [vytvoření kontroly přístupu](active-directory-azure-ad-controls-create-access-review.md).
5. Požádejte revidující o informace. Ve výchozím nastavení každý z nich obdrží e-mailu od Azure AD s odkazem na panel přístupu, kde bude [provádět kontrolu přístupu](active-directory-azure-ad-controls-perform-access-review.md).
6. Pokud revidující neposkytli informace, můžete pokládat službu Azure AD, aby jim odeslala připomenutí.  Ve výchozím nastavení Azure AD automaticky pošle připomenutí revidujícím, kteří ještě neodpověděli, po uplynutí poloviny času.
7. Jakmile revidující poskytnou potřebné informace, zastavte kontrolu přístupu a proveďte příslušné změny. Další informace najdete v tématu [Dokončení kontroly přístupu](active-directory-azure-ad-controls-complete-access-review.md).


## <a name="next-steps"></a>Další kroky

- [Vytvoření kontroly přístupu pro členy skupiny nebo přístupu k aplikaci](active-directory-azure-ad-controls-create-access-review.md)





