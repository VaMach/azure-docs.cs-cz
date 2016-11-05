---
title: Azure AD Connect v Microsoft Cloudu Německo
description: Azure AD Connect integruje vaše místní adresáře do služby Azure Active Directory. To umožní poskytovat společnou identitu pro aplikace Office 365, Azure a SaaS integrované s Azure AD.
keywords: Úvod do služby Azure AD Connect, přehled služby Azure AD Connect, co je Azure AD Connect, instalace služby Active Directory, Německo, Černý les
services: active-directory
documentationcenter: ''
author: billmath
manager: femila
editor: ''

ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 09/08/2016
ms.author: billmath

---
# Azure AD Connect v Microsoft Cloudu Německo – verze Public Preview
## Úvod
Azure AD Connect poskytuje synchronizaci mezi vaší místní službou Active Directory a službou Azure Active Directory.
V současné době je nutné, aby mnoho scénářů v [Microsoft Cloudu Německo](https://www.microsoft.com/de-de/cloud/deutschland/default.aspx) prováděl operátor. Při používání Microsoft Cloudu Německo je třeba brát v úvahu následující:

* Aby mohlo dojít k úspěšné synchronizaci, je nutné, aby na proxy serveru byly otevřené následující adresy URL:
  
  * *.microsoftonline.de
  * *.windows.net
  * * Seznamy odvolaných certifikátů
* Při přihlášení do adresáře služby Azure AD je nutné použít účet v doméně onmicrosoft.de.
* Následující funkce nejsou k dispozici:
  * Azure AD Connect Health
  * Automatické aktualizace
  * Zpětný zápis hesla

## Stáhnout
Službu Azure AD Connect lze stáhnout z okna Azure AD Connect v rámci portálu.  Pomocí pokynů níže vyhledejte okno Azure AD Connect.

### Okno Azure AD Connect
Po přihlášení na webu Azure Portal postupujte následovně:

1. Přejděte do Procházet.
2. Vyberte Azure Active Directory.
3. Poté vyberte Azure AD Connect.

Měli byste vidět následující:

![Okno Azure AD Connect](media\\active-directory-aadconnect-germany\\germany1.png)

Následující tabulka popisuje funkce zobrazené v okně.

| Název | Popis |
| --- | --- |
| STAV SYNCHRONIZACE |Uvádí, zda je synchronizace povolená nebo zakázaná. |
| POSLEDNÍ SYNCHRONIZACE |Čas dokončení poslední úspěšné synchronizace. |
| FEDEROVANÉ DOMÉNY |Zobrazuje počet aktuálně nakonfigurovaných federovaných domén. |

## Instalace
Chcete-li nainstalovat službu Azure AD Connect, můžete využít dokumentaci, kterou najdete [tady](active-directory-aadconnect.md#install-azure-ad-connect).

## Pokročilé funkce a další informace
Hledáte-li další informace a doprovodné materiály k vlastním nastavením nebo pokročilým konfiguracím, začněte s tématem [Integrování místních identit do služby Azure Active Directory](active-directory-aadconnect.md).  Tato stránka poskytuje informace a odkazy na další doprovodné materiály.

<!--HONumber=Sep16_HO3-->


