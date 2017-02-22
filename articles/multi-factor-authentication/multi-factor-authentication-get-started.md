---
title: "Výběr mezi cloudovým a serverovým Azure MFA | Dokumentace Microsoftu"
description: "Zvolte pro vás ideální řešení zabezpečení Multi-Factor Authentication položením otázky, co se pokoušíte zabezpečit a kde se nachází vaši uživatelé.  Pak vyberte cloud, server MFA nebo AD FS."
services: multi-factor-authentication
documentationcenter: 
author: kgremban
manager: femila
editor: yossib
ms.assetid: ec2270ea-13d7-4ebc-8a00-fa75ce6c746d
ms.service: multi-factor-authentication
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 01/23/2017
ms.author: kgremban
translationtype: Human Translation
ms.sourcegitcommit: 92c552f42f2645281f5039242825cfdaf6b683c9
ms.openlocfilehash: 1a42c137204c2d2f6fff9a005b2b0e885c2002b2


---
# <a name="choose-the-azure-multi-factor-authentication-solution-for-you"></a>Výběr vhodného řešení Azure Multi-Factor Authentication
Vzhledem k tomu, že existuje několik typů Azure Multi-Factor Authentication (MFA), musíme odpovědět na několik otázek, aby bylo možné zjistit, která verze je pro vás nejlepší.  Jsou to tyto otázky:

* [Co se pokouším zabezpečit?](#what-am-i-trying-to-secure)
* [Kde se nacházejí uživatelé?](#where-are-the-users-located)
* [Jaké funkce budu potřebovat?](#what-featured-do-i-need)

V následujících částech najdete tipy k zodpovězení každé z těchto otázek.

## <a name="what-am-i-trying-to-secure"></a>Co se pokouším zabezpečit?
Pro určení správného řešení dvoustupňového ověření si nejdříve musíme odpovědět na otázku, co se pokoušíte zabezpečit pomocí druhé metody ověřování.  Jedná se o aplikaci, která je v Azure?  Nebo systém vzdáleného přístupu?  Tím, že určíme, co se pokoušíme zabezpečit, si můžeme odpovědět na otázku, kde musí být povoleno ověřování Multi-Factor Authentication.  

| Co se pokoušíte zabezpečit | MFA v cloudu | Server MFA |
| --- |:---:|:---:|
| Vlastní aplikace Microsoftu |● |● |
| Aplikace Saas v galerii aplikací |● |● |
| Webové aplikace publikované prostřednictvím proxy aplikace Azure AD |● |● |
| Aplikace služby IIS nepublikované prostřednictvím proxy aplikace Azure AD | |● |
| Vzdálený přístup, jako je například síť VPN, RDG | |● |

## <a name="where-are-the-users-located"></a>Kde se nachází uživatelé
Když se dále podíváme, kde se naši uživatelé nacházejí, pomůže nám to určit správné řešení – jestli je to v cloudu nebo místně pomocí serveru MFA.

| Umístění uživatele | MFA v cloudu | Server MFA |
| --- |:---:|:---:|
| Azure Active Directory |● | |
| Azure AD a místní AD využívající federaci se službou AD FS |● |● |
| Azure AD a místní AD pomocí DirSync Azure AD Sync Azure AD Connect – bez synchronizace hesla |● |● |
| Azure AD a místní AD pomocí DirSync Azure AD Sync Azure AD Connect – se synchronizací hesla |● | |
| Místní služby Active Directory | |● |

## <a name="what-features-do-i-need"></a>Jaké funkce budu potřebovat?
V následující tabulce je uvedeno porovnání funkcí, které jsou dostupné s Multi-Factor Authentication v cloudu a se serverem Multi-Factor Authentication.

| Funkce | MFA v cloudu | Server MFA |
| --- |:---:|:---:|
| Oznámení mobilní aplikace jako druhý faktor | ● | ● |
| Kód ověření mobilní aplikace jako druhý faktor | ● | ● |
| Telefonní hovor jako druhý faktor | ● | ● |
| Jednosměrné služby SMS jako druhý faktor | ● | ● |
| Obousměrné služby SMS jako druhý faktor | | ● |
| Tokeny hardwaru jako druhý faktor | | ● |
| Hesla aplikací pro klienty Office 365, kteří nepodporují MFA | ● | |
| Kontrola správce nad metodami ověřování | ● | ● |
| Režim kódu PIN | | ● |
| Výstraha podvodů |● | ● |
| Sestavy MFA |● | ● |
| Jednorázové potlačení | | ● |
| Vlastní přivítání pro telefonní hovory | ● | ● |
| Přizpůsobitelné ID volajícího pro telefonní hovory | ● | ● |
| Důvěryhodné IP adresy | ● | ● |
| Zapamatovat MFA pro důvěryhodná zařízení | ● | |
| Podmíněný přístup | ● | ● |
| Mezipaměť |  | ● |

Teď, když jsme určili, zda chcete použít cloudové  multi-factor authentication nebo server multi-factor authentication na místě, můžete začít s nastavováním a používáním Azure Multi-Factor Authentication. **Vyberte ikonu, která představuje váš scénář.**

<center>




[![Cloud](./media/multi-factor-authentication-get-started/cloud2.png)](multi-factor-authentication-get-started-cloud.md)  &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;[![Ověření](./media/multi-factor-authentication-get-started/server2.png)](multi-factor-authentication-get-started-server.md) &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;
</center>



<!--HONumber=Jan17_HO4-->


