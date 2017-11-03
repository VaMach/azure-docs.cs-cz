---
title: "Azure Active Directory nejčastější dotazy ke správě zařízení | Microsoft Docs"
description: "Azure Active Directory zařízení nejčastější dotazy ke správě."
services: active-directory
documentationcenter: 
author: MarkusVi
manager: femila
ms.assetid: cdc25576-37f2-4afb-a786-f59ba4c284c2
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/13/2017
ms.author: markvi
ms.reviewer: jairoc
ms.openlocfilehash: 3b37033e0c96e01c73bef15af1b85116f404c354
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/11/2017
---
# <a name="azure-active-directory-device-management-faq"></a>Azure Active Directory nejčastější dotazy ke správě zařízení



**Otázka: jak můžete zaregistrovat zařízení systému macOS?**

**Odpověď:** k registraci zařízení v systému macOS:

1.  [Vytvoření zásady dodržování předpisů](https://docs.microsoft.com/intune/compliance-policy-create-mac-os)
2.  [Definovat zásady podmíněného přístupu pro zařízení v systému macOS](active-directory-conditional-access-azure-portal.md) 

**Poznámky:**

- Uživatelé, které jsou součástí zásady podmíněného přístupu musí [podporovaná verze systému Office pro systému macOS](active-directory-conditional-access-technical-reference.md#client-apps-condition) přístup k prostředkům. 

- Při první pokus o přístup uživatelům se zobrazí výzva k registraci zařízení pomocí portálu společnosti.

---

**Otázka: I nedávno zaregistrovaná zařízení. Proč nevidím zařízení v části Moje informace o uživateli na portálu Azure?**

**Odpověď:** zařízení s Windows 10, které jsou připojené k doméně se automatická registrace zařízení se nezobrazí v části informace o uživateli.
Budete muset použít PowerShell zobrazíte všechna zařízení. 

Následující zařízení jsou uvedené v části informace o uživateli:

- Všechny osobní zařízení, které nejsou připojené k enterprise 
- Všechny bez – Windows 10 nebo Windows Server 2016 
- Všechna zařízení jiný systém než Windows 

---

**Otázka: Proč nevidím všechna zařízení zaregistrované v Azure Active Directory na portálu Azure?** 

**Odpověď:** v současné době neexistuje žádný způsob, jak zobrazit všech registrovaných zařízení na portálu Azure. Prostředí Azure PowerShell můžete použít k vyhledání všech zařízení. Další podrobnosti najdete v tématu [Get-MsolDevice](/powershell/module/msonline/get-msoldevice?view=azureadps-1.0) rutiny.

--- 

**Otázka: Jak poznám, jaký je stav registrace zařízení klienta?**

**Odpověď:** stav registrace zařízení závisí na:

- Co je zařízení
- Jak byl zaregistrován 
- Všechny údaje s ním souvisejí. 
 

---

**Otázka: Proč je zařízení odstraněný na portálu Azure nebo pomocí prostředí Windows PowerShell pořád objevuje as registrované?**

**Odpověď:** toto chování je záměrné. Zařízení nebude mít přístup k prostředkům v cloudu. Pokud chcete zařízení odebrat a znovu zaregistrovat, musí být manuální akce mají být provedeny v zařízení. 

Pro Windows 10 a Windows Server 2016, které jsou místní AD připojené k doméně:

1.  Otevřete příkazový řádek jako správce.

2.  Typ`dsregcmd.exe /debug /leave`

3.  Odhlásit se a přihlaste se k aktivaci naplánovanou úlohu, která znovu zaregistruje zařízení. 

Pro jiné platformy Windows, které jsou místní AD připojené k doméně:

1.  Otevřete příkazový řádek jako správce.
2.  Zadejte `"%programFiles%\Microsoft Workplace Join\autoworkplace.exe /l"`.
3.  Zadejte `"%programFiles%\Microsoft Workplace Join\autoworkplace.exe /j"`.

---

**Otázka: Proč vidí položky duplicitní zařízení na portálu Azure?**

**ODPOVĚĎ:**

-   Pro Windows 10 a Windows Server 2016 Pokud jsou opakovaných pokusech o zrušení služby a znova připojit do stejného zařízení, může být duplicitní položky. 

-   Pokud jste použili přidat pracovní nebo školní účet, bude každý uživatel systému windows, který používá přidat pracovní nebo školní účet vytvořit nový záznam zařízení se stejným názvem zařízení.

-   Jiné platformy Windows, které jsou místní AD připojené k doméně pomocí automatické registrace vytvoří nový záznam zařízení s stejný název zařízení pro každého uživatele domény, který se přihlásí do zařízení. 

-   AADJ počítač, který vymaže, přeinstalovat a znovu připojený se stejným názvem, se zobrazí jako jiný záznam se stejným názvem zařízení.

---

**Otázka: Proč může uživatel i nadále přístup k prostředkům ze zařízení, které I zakázali na portálu Azure?**

**Odpověď:** může trvat až jednu hodinu pro odvolání má být použita.

>[!Note] 
>Pro zařízení, ke ztrátě doporučujeme vymazání obsahu zařízení k zajištění, že uživatelé nemají přístup k zařízení. Další podrobnosti najdete v tématu [registrovat zařízení pro správu v Intune](https://docs.microsoft.com/intune/deploy-use/enroll-devices-in-microsoft-intune). 


---

**Otázka: Proč Moji uživatelé vidí "Tam nedostanete odsud"?**

**Odpověď:** Pokud jste nakonfigurovali určitá pravidla podmíněného přístupu tak, aby vyžadovala stavu konkrétní zařízení a zařízení nesplňuje kritéria, uživatelé se zablokuje a zobrazí tato zpráva. Vyhodnocení pravidla a zajistěte, aby byla zařízení moct splňují kritéria, zobrazí se zpráva.

---


**Otázka: I najdete v části zařízení záznam zařízení v části informace o uživateli na webu Azure portal a můžete zobrazit stav as registrované na straně klienta. Se, že správně nastavit pro použití podmíněného přístupu?**

**Odpověď:** stav na portálu Azure a záznam o zařízení (deviceID) musí odpovídat klienta a splnění libovolného kritéria hodnocení pro podmíněný přístup. Další podrobnosti najdete v tématu [Začínáme s Azure Active Directory Device Registration](active-directory-device-registration.md).

---

**Otázka: Proč se zobrazila zpráva "uživatelské jméno nebo heslo je chybné." pro zařízení, které I právě připojené ke službě Azure AD?**

**Odpověď:** běžných příčin pro tento scénář jsou:

- Pověření uživatele již není platný.

- Počítač je schopen komunikovat s Azure Active Directory. Zkontrolujte všechny problémy se síťovým připojením.

- Azure AD Join požadavky nebyly splněny. Zkontrolujte, že jste provedli kroky pro [rozšíření možností cloudu k zařízení s Windows 10 prostřednictvím Azure Active Directory Join](active-directory-azureadjoin-overview.md).  

- Federované přihlášení vyžaduje federačním serveru pro podporu WS-Trust aktivní koncový bod. 

---

**Otázka: Proč se zobrazuje "Oops... došlo k chybě!" dialogové okno při připojení k počítači?**

**Odpověď:** jedná o výsledek nastavení registrace Azure Active Directory s Intune. Další podrobnosti najdete v tématu [nastavení správy pro zařízení Windows](https://docs.microsoft.com/intune/deploy-use/set-up-windows-device-management-with-microsoft-intune#azure-active-directory-enrollment).  

---

**Otázka: Proč moje pokus o připojení k počítači nezdaří i když se mi nepřišel. veškeré informace o chybě?**

**Odpověď:** pravděpodobnou příčinou je, že je uživatel přihlášen do zařízení pomocí předdefinovaného účtu správce. Před použitím Azure Active Directory Join k dokončení instalace vytvořte jiný místní účet. 

---

**Otázka: kde najdete pokyny pro nastavení automatické registrace zařízení?**

**Odpověď:** podrobné pokyny najdete v tématu [postup konfigurace automatické registrace zařízení se systémem Windows připojených k doméně se službou Azure Active Directory](active-directory-conditional-access-automatic-device-registration-setup.md)

---

**Otázka: kde můžete najít řešení potíží s informace o registraci automatického zařízení?**

**Odpověď:** informace o odstraňování potíží, najdete v tématu:

- [Řešení potíží s automatickou registraci domény k počítačům připojeným k Azure AD – Windows 10 a Windows Server 2016](device-management-troubleshoot-hybrid-join-windows-current.md)

- [Řešení potíží s automatickou registraci domény k počítačům připojeným k Azure AD pro klienty nižší úrovně systému Windows](device-management-troubleshoot-hybrid-join-windows-legacy.md)
 
---

