---
title: "Podmínky v Azure Active Directory podmíněného přístupu | Microsoft Docs"
description: "Zjistěte, jak používá přiřazení podmíněného přístupu Azure Active Directory k aktivaci zásadu."
services: active-directory
keywords: "podmíněný přístup k aplikacím, podmíněného přístupu s Azure AD, zabezpečený přístup k prostředkům společnosti, zásady podmíněného přístupu"
documentationcenter: 
author: MarkusVi
manager: mtillman
editor: 
ms.assetid: 8c1d978f-e80b-420e-853a-8bbddc4bcdad
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 02/09/2018
ms.author: markvi
ms.reviewer: calebb
ms.openlocfilehash: 2415a2c2c0143b4abeb8ec1ecab379a204456874
ms.sourcegitcommit: b32d6948033e7f85e3362e13347a664c0aaa04c1
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 02/13/2018
---
# <a name="conditions-in-azure-active-directory-conditional-access"></a>Podmínky v Azure Active Directory podmíněného přístupu 

S [podmíněného přístupu Azure Active Directory (Azure AD)](active-directory-conditional-access-azure-portal.md), jak oprávněným uživatelům přístup můžete řídit cloudových aplikací. V zásadách podmíněného přístupu které definujete odpovědi ("to") důvod aktivován vaší zásady ("v takovém případě"). 

![Řízení](./media/active-directory-conditional-access-conditions/10.png)


V kontextu podmíněného přístupu:

- "**v takovém případě**" se nazývá **podmínky**. 
- "**Udělejte to**" se nazývá **přístup k ovládacím prvkům**.

Kombinace vaše podmínky s ovládacími prvky přístup představuje zásady podmíněného přístupu.

![Řízení](./media/active-directory-conditional-access-conditions/61.png)

Tento článek poskytuje přehled podmínky a jak se používají v zásadách podmíněného přístupu. 


## <a name="users-and-groups"></a>Uživatelé a skupiny

Podmínka uživatelů a skupin je povinné v zásadách podmíněného přístupu. V zásadách můžete buď vyberte **všichni uživatelé** nebo vyberte konkrétní uživatele a skupiny.

![Řízení](./media/active-directory-conditional-access-conditions/02.png)

Když vyberete:

- **Všichni uživatelé**, vaše zásada se použije pro všechny uživatele s v adresáři. To zahrnuje uživatele typu Host.

- **Vyberte uživatele a skupiny**, můžete vybrat konkrétní skupiny uživatelů. Například můžete vybrat skupinu, která obsahuje všichni členové oddělení lidských zdrojů, když máte aplikaci HR vybrán jako cloudové aplikace. 

- Skupiny, může být jakýkoli typ skupiny ve službě Azure AD, včetně dynamické nebo přiřazené zabezpečení a distribučních skupin.

Můžete také vyloučit konkrétní uživatele nebo skupiny ze zásady. Jeden běžný případ použití jsou účty služby, pokud vaše zásady vynucují v službu Multi-Factor authentication. 

Cílení na konkrétní skupiny uživatelů je užitečná pro nasazení nových zásad. V nové zásady by měl cíl pouze počáteční sadu uživatelů k ověření zásad chování. 



## <a name="cloud-apps"></a>Cloudové aplikace 

Cloudové aplikace se webové stránky nebo služby. To zahrnuje webů zabezpečených službou Proxy aplikace Azure. Podrobný popis podporované cloudové aplikace, najdete v části [cloudové aplikace přiřazení](active-directory-conditional-access-technical-reference.md#cloud-apps-assignments).    

Podmínka cloudové aplikace je povinné v zásadách podmíněného přístupu. V zásadách můžete buď vyberte **všech cloudových aplikací** nebo vyberte konkrétní aplikace.

![Řízení](./media/active-directory-conditional-access-conditions/03.png)

Můžete vybrat:

- **Všechny cloudových aplikací** směrný plán zásad má být použita k celé organizaci. Běžně používá pro tento výběr je zásada, která vyžaduje vícefaktorové ověřování při přihlašování riziko je zjištěna všech cloudových aplikací.

- Jednotlivých cloudových aplikací k určitým službám cíl zásad. Například může vyžadovat uživatelé měli [zařízení kompatibilní s](active-directory-conditional-access-policy-connected-applications.md) pro přístup k SharePoint Online. Když přistupují k obsahu služby SharePoint, například Microsoft Teams je tato zásada také použitá k jiným službám. 

Můžete také vyloučit konkrétní aplikace z zásady; Tyto aplikace se však stále vztahují zásady uplatněné na službách, které chtějí získat přístup. 



## <a name="sign-in-risk"></a>Riziko přihlášení

Riziko přihlášení je indikátorem pro pravděpodobnost (vysoká, střední nebo nízká), že nebyla provedena pokus o přihlášení, legitimní vlastníkem uživatelského účtu. Azure AD vypočítá úroveň rizika přihlášení během přihlášení uživatele. Úroveň rizika počítané přihlášení můžete použít jako podmínku v zásadách podmíněného přístupu. 

![Podmínky](./media/active-directory-conditional-access-conditions/22.png)

Pokud chcete použít tuto podmínku, musíte mít [Azure Active Directory Identity Protection](active-directory-identityprotection.md) povolena.
 
Běžné případy použití pro tuto podmínku jsou zásady, které:

- Blokujte přístup pro uživatele s vysokou přihlašovací riziko potenciálně nezákonné uživatelům zabránit v přístupu k vaší cloudové aplikace. 
- Požadovat použití vícefaktorového ověřování pro uživatele s střední riziko přihlášení. Vynucování služby Multi-Factor authentication, můžete poskytnout další jistotu, že přihlášení se provádí legitimní vlastníka účtu.

Další informace najdete v tématu popisujícím [riziková přihlášení](active-directory-identityprotection.md#risky-sign-ins).  

## <a name="device-platforms"></a>Platformy zařízení

Platforma je charakterizovaná operačního systému, který běží na vašem zařízení. Azure AD identifikuje platformou pomocí informací uvedených zařízení, jako je například uživatelský agent. Protože tyto informace neověřené, doporučujeme, aby měly všechny platformy zásada použitá, a to buď blokování přístupu, požadováno dodržování zásad Intune nebo nutnosti že zařízení být připojené k doméně. Výchozí hodnota je uplatňovat zásady na všech platformách zařízení. 


![Podmínky](./media/active-directory-conditional-access-conditions/24.png)

Úplný seznam platforem podporovaných zařízení najdete v tématu [podmínku platformy zařízení](active-directory-conditional-access-technical-reference.md#device-platform-condition).


Společné pro tento stav je zásadu, která omezuje přístup na vašich cloudových aplikací pro případ použití [důvěryhodné zařízení](active-directory-conditional-access-policy-connected-applications.md#trusted-devices). Další scénáře včetně podmínku platformy zařízení, najdete v části [podmíněného přístupu na základě aplikace Azure Active Directory](active-directory-conditional-access-mam.md).


## <a name="locations"></a>Umístění

Umístění máte možnost definovat podmínky, které jsou založeny na kde bylo inicializováno pokus o připojení. 
     
![Podmínky](./media/active-directory-conditional-access-conditions/25.png)

Běžné případy použití pro tuto podmínku jsou zásady, které:

- Požadovat použití vícefaktorového ověřování pro uživatele, kteří používají službu, když jsou mimo podnikovou síť.  

- Blokujte přístup pro uživatele, kteří používají službu z konkrétní zemích nebo oblastech. 

Další informace najdete v tématu [podmínky umístění v Azure Active Directory podmíněného přístupu](active-directory-conditional-access-locations.md).


## <a name="client-apps"></a>Klientské aplikace

Podmínka aplikace klienta můžete použít zásady pro různé typy aplikací, jako například:

- Weby a služby
- Mobilní aplikace a aplikací klasické pracovní plochy. 

![Podmínky](./media/active-directory-conditional-access-conditions/04.png)

Aplikace se označuje jako:

- Webové stránky nebo služby, pokud používá webové protokoly jednotné přihlašování, SAML, WS-Fed nebo OpenID Connect pro důvěrné klienta.

- A mobilní aplikace nebo plochy aplikace, pokud mobilní aplikace, OpenID Connect používá pro nativní klienta.

Úplný seznam klientských aplikací v požadované zásady podmíněného přístupu můžete použít, najdete v článku [technické informace o Azure Active Directory podmíněného přístupu](active-directory-conditional-access-technical-reference.md#client-apps-condition).

Běžné případy použití pro tuto podmínku jsou zásady, které:

- Vyžadovat [zařízení kompatibilní s](active-directory-conditional-access-policy-connected-applications.md) mobilních a desktopových aplikací, které stáhnout velké objemy dat a zařízení současně přístup z prohlížeče z libovolného zařízení.

- Blokovat přístup z webových aplikací, ale povolení přístupu z mobilních a desktopových aplikací.

Kromě používat protokoly moderní ověřování a jednotné přihlašování k webu, můžete použít tuto podmínku pro e-mailové aplikace, které používají protokol Exchange ActiveSync, jako je nativní e-mailové aplikace na většině smartphony. V současné době klientské aplikace pomocí starší verze protokolů je potřeba zabezpečit pomocí služby AD FS.

 Další informace naleznete v tématu:

- [Nastavení služby SharePoint Online a Exchange Online pro Azure Active Directory podmíněného přístupu](active-directory-conditional-access-no-modern-authentication.md)
 
- [Azure Active Directory, na základě aplikace podmíněného přístupu](active-directory-conditional-access-mam.md) 








## <a name="next-steps"></a>Další postup

- Pokud chcete vědět, jak konfigurovat zásadu podmíněného přístupu, najdete v článku [Začínáme s podmíněným přístupem v Azure Active Directory](active-directory-conditional-access-azure-portal-get-started.md).

- Pokud jste připraveni ke konfiguraci zásad podmíněného přístupu pro prostředí, najdete v článku [osvědčené postupy pro podmíněný přístup v Azure Active Directory](active-directory-conditional-access-best-practices.md). 

