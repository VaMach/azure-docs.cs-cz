---
title: "Vytváření sestav zabezpečení v edicích Free a Basic služby Azure Active Directory – Preview | dokumentace Microsoftu"
description: "Obsahuje seznam různých dostupných sestav pro Azure Active Directory ve verzi Preview."
services: active-directory
author: MarkusVi
manager: femila
ms.assetid: 6141a333-38db-478a-927e-526f1e7614f4
ms.service: active-directory
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 01/19/2017
ms.author: markvi
translationtype: Human Translation
ms.sourcegitcommit: fa5a6dab1e76d62956cbfdd9b8b0f64c014696db
ms.openlocfilehash: c1a3953c79cfac9d6f55b38971ee2b79ff4244eb


---
# <a name="security-reporting-in-the-azure-active-directory-free-and-basic-edition---preview"></a>Vytváření sestav zabezpečení v edicích Free a Basic služby Azure Active Directory – Preview

Sestavy zabezpečení ve verzi [Preview](active-directory-preview-explainer.md) služby Azure Active Directory umožňují získat přehled o pravděpodobnosti ohrožení uživatelských účtů ve vašem prostředí. 

Azure Active Directory detekuje podezřelé akce, které souvisejí s vašimi uživatelskými účty. Pro každou zjištěnou akci se vytvoří záznam nazvaný *riziková událost*. Další podrobnosti najdete v tématu věnovaném [rizikovým událostem služby Azure Active Directory](active-directory-identity-protection-risk-events.md). 

Zjištěné rizikové události se použijí k výpočtu těchto údajů:

- **Riziková přihlášení** –Rizikové přihlášení je indikátorem pokusu o přihlášení, který mohl provést někdo, kdo není legitimním vlastníkem uživatelského účtu. Další podrobnosti najdete v tématu [Riziková přihlášení](active-directory-identityprotection.md#risky-sign-ins). 

- **Uživatelé označení příznakem rizika** – Rizikový uživatel je indikátorem uživatelského účtu, který mohl být ohrožený. Další podrobnosti najdete v tématu [Uživatelé označení příznakem rizika](active-directory-identityprotection.md#users-flagged-for-risk).  


## <a name="risky-sign-ins-report"></a>Sestava rizikových přihlášení

Edice Free a Basic služby Azure Active Directory poskytují seznam rizikových přihlášení, která byla zjištěná pro vaše uživatele. Sestava rizikových událostí obsahuje tyto údaje:

- **Uživatel** – Jméno uživatele použité během přihlašovací operace
- **IP** – IP adresa zařízení použitá pro připojení k Azure Active Directory
- **Umístění** – Umístění použité pro připojení k Azure Active Directory
- **Čas přihlášení** – Čas, kdy k přihlášení došlo
- **Stav** – Stav přihlášení

Tato sestava poskytuje možnost stažení příslušných dat.

![Vytváření sestav](./media/active-directory-reporting-security-azure-portal-free-basic/01.png)

Na základě prošetření rizikového přihlášení můžete službě Azure Active Directory poskytnout zpětnou vazbu ve formě následujících akcí:

- Vyřešit
- Označit jako falešně pozitivní
- Ignorovat
- Znovu aktivovat

![Vytváření sestav](./media/active-directory-reporting-security-azure-portal-free-basic/21.png)

další informace najdete v tématu věnovaném [ručnímu zavření rizikových událostí](active-directory-identityprotection.md#closing-risk-events-manually).


## <a name="users-at-risk-report"></a>Sestava ohrožených uživatelů

Edice Free služby Azure Active Directory poskytuje seznam uživatelských účtů, které jsou možná ohrožené. 


![Vytváření sestav](./media/active-directory-reporting-security-azure-portal-free-basic/03.png)

Kliknutím na uživatele v seznamu otevřete okno se souvisejícími uživatelskými daty.
U ohrožených uživatelů zkontrolujte historii jejich přihlášení a v případě potřeby resetujte heslo.

![Vytváření sestav](./media/active-directory-reporting-security-azure-portal-free-basic/46.png)



## <a name="next-steps"></a>Další kroky

- Další informace o vytváření sestav Azure Active Directory najdete v tématu [Příručka generování sestav v Azure Active Directory](active-directory-reporting-guide.md).
- Další informace o Azure Active Directory Identity Protection najdete v tématu [Azure Active Directory Identity Protection](active-directory-identityprotection.md).




<!--HONumber=Jan17_HO3-->


