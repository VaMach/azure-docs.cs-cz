---
title: "Řešení potíží s hybridní Azure Active Directory nižší úrovně zařízení připojená k | Microsoft Docs"
description: "Řešení potíží s hybridní Azure Active Directory připojené zařízení nižší úrovně."
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
ms.date: 11/08/2017
ms.author: markvi
ms.reviewer: jairoc
ms.openlocfilehash: 8bc07c551b5554a3f5b8697ca77a00d9cd0f470a
ms.sourcegitcommit: adf6a4c89364394931c1d29e4057a50799c90fc0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/09/2017
---
# <a name="troubleshooting-hybrid-azure-active-directory-joined-down-level-devices"></a>Nižší úrovně zařízení připojená k řešení potíží s hybridní Azure Active Directory 

Toto téma se vztahuje pouze na následujících zařízeních: 

- Windows 7 
- Windows 8.1 
- Windows Server 2008 R2 
- Windows Server 2012 
- Windows Server 2012 R2 
 

Pro Windows 10 nebo Windows Server 2016, najdete v části [hybridní Poradce při potížích s Azure Active Directory připojené zařízení s Windows 10 a Windows Server 2016](device-management-troubleshoot-hybrid-join-windows-current.md).

Toto téma předpokládá, že máte [nakonfigurované hybridní Azure Active Directory zařízení připojená k](device-management-hybrid-azuread-joined-devices-setup.md) k podporují následující scénáře:

- Podmíněný přístup využívající zařízení

- [Enterprise cestovní nastavení](active-directory-windows-enterprise-state-roaming-overview.md)

- [Windows Hello pro firmy](active-directory-azureadjoin-passport-deployment.md) 





Toto téma poskytuje pokyny o tom, jak vyřešit potenciální problémy při řešení potíží.  

**Co byste měli vědět:** 

- Maximální počet zařízení na uživatele je zaměřená na zařízení. Například pokud *jdoe* a *jharnett* Přihlaste se do zařízení, samostatné registrace (DeviceID) se vytvoří pro každou z nich **uživatele** informace o kartě.  

- Počáteční registrace / připojení k zařízení, je nakonfigurována se provést k pokusu o přihlášení nebo uzamčení nebo odemčení. Může dojít k 5 minut zpoždění aktivovány úloh služby Plánovač úloh. 

- Přeinstalujte operační systém nebo ruční unregister a zopakujte registraci na Azure AD může vytvořit nové registrace a má za následek více položek na kartě informace uživatele na portálu Azure. 


## <a name="step-1-retrieve-the-registration-status"></a>Krok 1: Načíst stav registrace 

**Chcete-li ověřit stav registrace:**  

1. Otevřete příkazový řádek jako správce 

2. Typ`"%programFiles%\Microsoft Workplace Join\autoworkplace.exe /i"`

Tento příkaz zobrazí dialogové okno, které vám poskytne další informace o stavu připojení.

![Připojení k síti na pracovišti pro Windows](./media/active-directory-device-registration-troubleshoot-windows-legacy/01.png)


## <a name="step-2-evaluate-the-hybrid-azure-ad-join-status"></a>Krok 2: Vyhodnoťte hybridní stav připojení k Azure AD 

Pokud připojení k Azure AD hybridní nebyla úspěšná, dialogové okno vám poskytuje podrobnosti o problému, který došlo k chybě.

**Nejběžnějším problémům, jsou:**

- Konfigurace služby AD FS nebo služby Azure AD

    ![Připojení k síti na pracovišti pro Windows](./media/active-directory-device-registration-troubleshoot-windows-legacy/02.png)

- Nejsou přihlášeni jako uživatel domény

    ![Připojení k síti na pracovišti pro Windows](./media/active-directory-device-registration-troubleshoot-windows-legacy/03.png)

- Bylo dosaženo kvóty

    ![Připojení k síti na pracovišti pro Windows](./media/active-directory-device-registration-troubleshoot-windows-legacy/04.png)

- Služba nereaguje. 

    ![Připojení k síti na pracovišti pro Windows](./media/active-directory-device-registration-troubleshoot-windows-legacy/05.png)

Informace o stavu můžete také najít v protokolu událostí v části **aplikace a služby Log\Microsoft-pracovišti**.
  
**Nejběžnější příčiny selhání hybridní Azure AD join jsou:** 

- Váš počítač není v interní síti organizace nebo síť VPN bez připojení k místní řadič domény AD.

- Jste přihlášeni k počítači pomocí účtu místního počítače. 

- Problémy s konfigurací služby: 

  - Federační server nakonfigurovaný pro podporu **WIAORMULTIAUTHN**. 

  - Neexistuje žádný objekt spojovací bod služby, který odkazuje na název ověřené domény ve službě Azure AD v doménové struktuře AD, pokud je počítač členem.

  - Uživatel byl dosažen maximální počet zařízení. 

## <a name="next-steps"></a>Další kroky

Otázky, najdete v článku [nejčastější dotazy ke správě zařízení](device-management-faq.md)  
