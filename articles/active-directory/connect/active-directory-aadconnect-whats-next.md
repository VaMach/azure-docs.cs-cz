---
title: "Azure AD Connect: Další kroky a postupy pro správu Azure AD Connect | Microsoft Docs"
description: "Zjistěte, jak rozšířit výchozí konfigurace a provozní úlohy pro Azure AD Connect."
services: active-directory
documentationcenter: 
author: billmath
manager: femila
editor: curtand
ms.assetid: c18bee36-aebf-4281-b8fc-3fe14116f1a5
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/12/2017
ms.author: billmath
ms.openlocfilehash: beace24fa00c85a5038a3c39ae8f76af5fd12111
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="next-steps-and-how-to-manage-azure-ad-connect"></a>Další kroky a postupy pro správu Azure AD Connect
Chcete-li přizpůsobit Azure Active Directory (Azure AD) připojit ke splnění potřebám a požadavkům vaší organizace pomocí provozních postupů v tomto článku.  

## <a name="add-additional-sync-admins"></a>Přidat další synchronizaci admins
Ve výchozím nastavení je moct spravovat nainstalované synchronizační modul pouze uživatele, který se instalace a místní správci. Pro další osoby mohly k otvírat a spravovat synchronizační modul vyhledejte skupinu s názvem ADSyncAdmins na místním serveru a přidat je do této skupiny.

## <a name="assign-licenses-to-azure-ad-premium-and-enterprise-mobility-suite-users"></a>Přiřazení licencí uživatelům Azure AD Premium a Enterprise Mobility Suite
Teď, když uživatelé byly synchronizované do cloudu, budete muset přiřadit licenci, můžete začít s cloudové aplikace, jako je například Office 365.

### <a name="to-assign-an-azure-ad-premium-or-enterprise-mobility-suite-license"></a>Chcete-li přiřadit Azure AD Premium nebo Enterprise Mobility Suite licencí

1. Přihlaste se k portálu Azure jako správce.
2. Vlevo vyberte možnost **Active Directory**.
3. Na **služby Active Directory** stránky, dvakrát klikněte na adresář, který obsahuje uživatele, kterou chcete nastavit.
4. V horní části stránky adresáře vyberte možnost **Licence**.
5. Na **licence** vyberte **Active Directory Premium** nebo **Enterprise Mobility Suite**a potom klikněte na **přiřadit**.
6. V dialogovém okně vyberte uživatele, kterým chcete přiřadit licence, a potom změny uložte kliknutím na ikonu zaškrtnutí.

## <a name="verify-the-scheduled-synchronization-task"></a>Ověření úlohy plánované synchronizace
Použití portálu Azure a zkontrolujte stav synchronizace.

### <a name="to-verify-the-scheduled-synchronization-task"></a>Ověření úlohy plánované synchronizace
1. Přihlaste se k portálu Azure jako správce.
2. Vlevo vyberte možnost **Active Directory**.
3. Na **služby Active Directory** stránky, dvakrát klikněte na adresář, který obsahuje uživatele, kterou chcete nastavit.
4. V horní části stránky adresáře vyberte **integrace adresáře**.
5. V části **integraci s místní službou active directory**, Všimněte si, čas poslední synchronizace.

<center>![Čas synchronizace adresáře](./media/active-directory-aadconnect-whats-next/verify.png)</center>

## <a name="start-a-scheduled-synchronization-task"></a>Spustit úlohu plánované synchronizace
Pokud potřebujete spustit úlohu synchronizace, musíte znovu spuštěním prostřednictvím Průvodce službou Azure AD Connect.  Je třeba zadat přihlašovací údaje Azure AD.  V průvodci vyberte **přizpůsobit možnosti synchronizace** úloh a klikněte na tlačítko **Další** přesunout pomocí průvodce. Na konci, ujistěte se, že **zahájit proces synchronizace ihned po dokončení prvotní konfigurace** políčka.

<center>![Spustit synchronizaci.](./media/active-directory-aadconnect-whats-next/startsynch.png)</center>

Další informace o Azure AD Connect sync Scheduler najdete v tématu [Azure AD Connect Scheduler](active-directory-aadconnectsync-feature-scheduler.md).

## <a name="additional-tasks-available-in-azure-ad-connect"></a>Další úlohy, které jsou k dispozici ve službě Azure AD Connect
Po počáteční instalace systému služby Azure AD Connect můžete vždy spustíte průvodce znovu z úvodní stránky nebo plochy zástupce Azure AD Connect.  Si všimnete, že znovu projít průvodce nabízí některé nové možnosti ve formě další úkoly.  

Následující tabulka obsahuje souhrn tyto úlohy a stručný popis jednotlivých úloh.

![Seznam dalších úloh](./media/active-directory-aadconnect-whats-next/addtasks.png)

| Další úlohy | Popis |
| --- | --- |
| **Zobrazení vybraným scénářem** |Zobrazte vaše aktuální řešení Azure AD Connect.  To zahrnuje obecná nastavení, synchronizovat adresáře a nastavení synchronizace. |
| **Přizpůsobit možnosti synchronizace** |Změňte aktuální konfiguraci jako přidání další doménové struktury služby Active Directory ke konfiguraci nebo povolení možnosti synchronizace, jako je například uživatele, skupiny, zařízení nebo zpětný zápis hesla. |
| **Zapnout pracovní režim** |Fáze informace, které není synchronizována okamžitě a není exportovány do služby Azure AD nebo místní služby Active Directory.  Pomocí této funkce můžete zobrazit náhled synchronizace předtím, než k nim dojde. |

## <a name="next-steps"></a>Další kroky
Další informace o [integrace místních identit s Azure Active Directory](active-directory-aadconnect.md).
