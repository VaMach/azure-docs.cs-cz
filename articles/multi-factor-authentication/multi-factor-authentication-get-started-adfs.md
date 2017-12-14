---
title: "Dvoustupňové ověřování a AD FS – Azure MFA | Dokumentace Microsoftu"
description: "Toto je stránka vícefaktorového ověřování Azure, která popisuje, jak začít s vícefaktorovým ověřováním Azure a službami AD FS."
services: multi-factor-authentication
documentationcenter: 
author: MicrosoftGuyJFlo
manager: mtillman
ms.reviewer: richagi
ms.assetid: 44fbba68-6cf9-46c1-a9df-736580b68ae3
ms.service: multi-factor-authentication
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 08/25/2017
ms.author: joflore
ms.openlocfilehash: 7b597f45a13c4f31c662a7832d9571dcc6b25e52
ms.sourcegitcommit: e266df9f97d04acfc4a843770fadfd8edf4fa2b7
ms.translationtype: HT
ms.contentlocale: cs-CZ
ms.lasthandoff: 12/11/2017
---
# <a name="getting-started-with-azure-multi-factor-authentication-and-active-directory-federation-services"></a>Začínáme s ověřováním Azure Multi-Factor Authentication a službami Active Directory Federation Services
<center>![Cloud](./media/multi-factor-authentication-get-started-adfs/adfs.png)</center>

Pokud má vaše organizace federované místní služby Active Directory s Azure Active Directory využívající službu AD FS, jsou k dispozici následující 2 možnosti používání ověřování Azure Multi-Factor Authentication.

* Zabezpečení cloudových prostředků pomocí ověřování Azure Multi-Factor Authentication nebo služeb Active Directory Federation Services
* Zabezpečení cloudu a místních prostředků pomocí ověřování Azure Multi-Factor Authentication Server

Následující tabulka shrnuje zkušenosti s ověřováním mezi zabezpečením prostředků pomocí Azure Multi-Factor Authentication a AD FS

| Zkušenosti s ověřováním – aplikace založené na prohlížeči | Zkušenosti s ověřováním – aplikace nezaložené na prohlížeči |
|:--- |:--- |:--- |
| Zabezpečení prostředků Azure AD pomocí Azure Multi-Factor Authentication |<li>První krok ověření se provádí místně pomocí služby AD FS.</li> <li>Druhý krok využívá telefon a provádí se pomocí cloudového ověřování.</li> |
| Zabezpečení prostředků Azure AD pomocí Active Directory Federation Services |<li>První krok ověření se provádí místně pomocí služby AD FS.</li><li>Druhý krok se provádí místně dodržením deklarace identity.</li> |

Upozornění s hesly aplikací pro federované uživatele:

* Hesla aplikace se ověřují pomocí cloudového ověřování, takže obcházejí federaci. Federace se aktivně používá jenom při nastavování hesla aplikace.
* Místní nastavení služby Access Control klienta nejsou dodržená hesly aplikace.
* U hesel aplikací ztratíte schopnost ověřování přihlašování v místě.
* Zakázání/odstranění účtu může pro synchronizaci adresáře trvat až tři hodiny, přičemž dojde ke zpoždění zakázání/odstranění hesla aplikace v cloudové identitě.

Informace o nastavení ověřování Azure Multi-Factor Authentication nebo Azure Multi-Factor Authentication Serveru se službou AD FS najdete v následujících článcích:

* [Zabezpečené cloudové prostředky používající ověřování Azure Multi-Factor Authentication a AD FS](multi-factor-authentication-get-started-adfs-cloud.md)
* [Zabezpečený cloud a místní prostředky používající ověřování Azure Multi-Factor Authentication Server s Windows Server 2012 R2 AD FS](multi-factor-authentication-get-started-adfs-w2k12.md)
* [Zabezpečené cloudové a lokální prostředky používající Microsoft Azure Multi-Factor Authentication Serveru s AD FS 2.0](multi-factor-authentication-get-started-adfs-adfs2.md)
