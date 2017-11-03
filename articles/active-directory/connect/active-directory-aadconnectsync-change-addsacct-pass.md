---
title: "Synchronizace Azure AD Connect: Změna hesla účtu služby AD DS | Microsoft Docs"
description: "Tento dokument téma popisuje postup aktualizace Azure AD Connect po změně hesla účtu služby AD DS."
services: active-directory
keywords: "AD DS účet, heslo a účet služby Active Directory"
documentationcenter: 
author: cychua
manager: femila
editor: 
ms.assetid: 76b19162-8b16-4960-9e22-bd64e6675ecc
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/12/2017
ms.author: billmath
ms.openlocfilehash: 14e16a238e60ecfeeb3cbf88c3922a79349dcc75
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/11/2017
---
# <a name="changing-the-ad-ds-account-password"></a>Změna hesla účtu služby AD DS
Účet služby AD DS odkazuje na uživatelský účet používaný ke komunikaci s místní služby Active Directory přes Azure AD Connect. Pokud změníte heslo účtu služby AD DS, je třeba aktualizovat službu Azure AD Connect synchronizace pomocí nového hesla. Jinak synchronizace mohou už správně synchronizovat s místní služby Active Directory a bude dojde k následujícím chybám:

* V operaci Synchronization Service Manager, všechny import nebo export s místní AD selže s **bez start pověření** chyby.

* V prohlížeči událostí systému Windows v protokolu událostí aplikace obsahuje chybu s **6000 ID události** a zpráva **, agent pro správu "contoso.com" se nepodařilo spustit, protože nebyly platné přihlašovací údaje,**.


## <a name="how-to-update-the-synchronization-service-with-new-password-for-ad-ds-account"></a>Jak aktualizovat synchronizační službu s nové heslo pro účet služby AD DS
Chcete-li aktualizovat synchronizační služby pomocí nového hesla:

1. Spusťte Synchronization Service Manager (Služba synchronizace → START).
</br>![Správce synchronizace služby](./media/active-directory-aadconnectsync-service-manager-ui/startmenu.png)  

2. Přejděte na **konektory** kartě.

3. Vyberte **AD Connector.** odpovídající účet služby AD DS, pro kterou byl změněn jeho heslo.

4. V části **akce**, vyberte **vlastnosti**.

5. V dialogovém okně automaticky otevírané okno Vyberte **připojit k doménové struktuře služby Active Directory**:

6. Zadejte nové heslo účtu služby AD DS v **heslo** textové pole.

7. Klikněte na tlačítko **OK** nové heslo uložte a zavřete dialogové okno automaticky otevírané okno.

8. Restartování Azure AD Connect synchronizační služba v modulu snap-in Správce řízení služeb systému Windows. To je potřeba zajistit, že všechny odkazy na staré heslo je odebraných z mezipaměti v paměti.

## <a name="next-steps"></a>Další kroky
**Témata s přehledem**

* [Synchronizace Azure AD Connect: pochopení a přizpůsobení synchronizace](active-directory-aadconnectsync-whatis.md)

* [Integrování místních identit do služby Azure Active Directory](active-directory-aadconnect.md)
