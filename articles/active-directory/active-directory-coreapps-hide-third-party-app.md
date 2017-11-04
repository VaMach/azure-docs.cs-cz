---
title: "Skrýt aplikace třetích stran z možnosti pro uživatele v Azure Active Directory | Microsoft Docs"
description: "Skrytí aplikace třetích stran z možnosti pro uživatele v Azure Active Directory"
services: active-directory
documentationcenter: 
author: billmath
manager: femila
editor: 
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/11/2017
ms.author: billmath
ms.reviewer: asteen
ms.custom: it-pro
ms.openlocfilehash: 976cbb1341493186b9996d250ebca8f2f3688fdf
ms.sourcegitcommit: f8437edf5de144b40aed00af5c52a20e35d10ba1
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/03/2017
---
# <a name="hide-a-third-party-application-from-users-experience-in-azure-active-directory"></a>Skrýt aplikace třetích stran z možnosti pro uživatele v Azure Active Directory

Pokud máte aplikaci třetí strany (aplikace, které zveřejnil jiné než Microsoft), že nechcete zobrazovat ve panelů přístup uživatele nebo Spouštěč oken Office 365, je k dispozici možnost ke skrytí tuto dlaždici aplikace. Skrytím uživatelům aplikací stále mít oprávnění k aplikaci, ale nezobrazí je zobrazit na jejich Spouštěč aplikace oken. Musí mít příslušná oprávnění ke správě firemní aplikace a musí být globální správce adresáře.

## <a name="hiding-a-third-party-app-from-a-users-experience"></a>Skrytí aplikace třetích stran ze zkušeností uživatele
Použijte následující postup ke skrytí aplikace třetích stran z přístupového panelu a Spouštěč oken aplikace Office 365 uživatele

### <a name="how-do-i-hide-a-third-party-app-from-users-access-panel-and-o365-app-launchers"></a>Jak skrýt aplikace třetích stran z přístupového panelu a Spouštěč oken aplikace O365 uživatele?

1.  Přihlaste se k [portál Azure](https://portal.azure.com) pomocí účtu, který je globální správce adresáře.
2.  Vyberte **další služby**, zadejte **Azure Active Directory** v textovém poli a potom vyberte **Enter**.
3.  Na **Azure Active Directory - *directoryname***  obrazovky (to znamená, Azure AD obrazovky pro adresář spravujete), vyberte **podnikové aplikace, které**.
![Podnikové aplikace](media/active-directory-coreapps-hide-third-party-app/app1.png)
4.  Na **podnikové aplikace, které** obrazovku, vyberte **všechny aplikace**. Zobrazí seznam aplikací, které můžete spravovat.
5.  Na **podnikové aplikace – všechny aplikace** obrazovky, vyberte aplikaci.</br>
![Podnikové aplikace](media/active-directory-coreapps-hide-third-party-app/app2.png)
6.  Na ***appname*** obrazovky (to znamená, že na obrazovce s názvem vybranou aplikaci v názvu) vyberte vlastnosti.
7.  Na  ***appname* -vlastnosti** obrazovku, vyberte **Ano** pro **viditelný pro uživatele?**.
![Podnikové aplikace](media/active-directory-coreapps-hide-third-party-app/app3.png)
8.  Vyberte **Uložit** příkaz.

## <a name="next-steps"></a>Další kroky
* [Zobrazení všech Moje skupin](active-directory-groups-view-azure-portal.md)
* [Přiřazení uživatele nebo skupiny do aplikace enterprise](active-directory-coreapps-assign-user-azure-portal.md)
* [Odebrat uživatele nebo skupinu přiřazení z podnikové aplikace.](active-directory-coreapps-remove-assignment-azure-portal.md)
* [Změna názvu nebo logo aplikace enterprise](active-directory-coreapps-change-app-logo-user-azure-portal.md)
