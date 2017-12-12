---
title: "Azure Active Directory podmíněný přístup pro připojení VPN (preview) | Microsoft Docs"
description: "Zjistěte, jak funguje Azure Active Directory podmíněný přístup pro připojení k síti VPN. "
services: active-directory
documentationcenter: 
author: MarkusVi
manager: mtillman
ms.assetid: 51a1ee61-3ffe-4f65-b8de-ff21903e1e74
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/01/2017
ms.author: markvi
ms.reviewer: jairoc
ms.openlocfilehash: 8941e631976eb11966c1f9ddd207af816df5dadf
ms.sourcegitcommit: e266df9f97d04acfc4a843770fadfd8edf4fa2b7
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 12/11/2017
---
# <a name="azure-active-directory-conditional-access-for-vpn-connectivity-preview"></a>Azure Active Directory podmíněný přístup pro připojení VPN (preview)

S [podmíněného přístupu Azure Active Directory (Azure AD)](active-directory-conditional-access-azure-portal.md), můžete upřesnit jak oprávněným uživatelům přístup k prostředkům. S Azure AD podmíněný přístup pro připojení k virtuální privátní síti (VPN) můžete pomoct chránit vaše připojení k síti VPN.


Konfigurace podmíněného přístupu pro připojení k síti VPN, je třeba provést následující kroky: 

1.  Nakonfigurujte server sítě VPN.
2.  Konfigurace klienta VPN.
3.  Nakonfigurujte zásady podmíněného přístupu.


## <a name="before-you-begin"></a>Než začnete

Toto téma předpokládá, že jste obeznámeni s následujícími tématy:

- [Podmíněný přístup v Azure Active Directory](active-directory-conditional-access-azure-portal.md)
- [Připojení VPN a podmíněného přístupu](https://docs.microsoft.com/windows/access-protection/vpn/vpn-conditional-access)

A získáte přehled o tom, jak Microsoft implementuje tuto funkci, najdete v části [rozšíření vzdáleného přístupu ve Windows 10 s profilem sítě VPN automatické](https://www.microsoft.com/itshowcase/Article/Content/894/Enhancing-remote-access-in-Windows-10-with-an-automatic-VPN-profile).   


## <a name="prerequisites"></a>Požadavky

Konfigurace podmíněného přístupu Azure Active Directory pro připojení k síti VPN, musíte mít server VPN nakonfigurován. 



## <a name="step-1-configure-your-vpn-server"></a>Krok 1: Konfigurace serveru VPN 

Tento krok nakonfiguruje kořenové certifikáty pro ověřování sítě VPN s Azure AD. Chcete-li nakonfigurovat podmíněný přístup pro připojení VPN, je potřeba:

1. Vytvořte certifikát sítě VPN na portálu Azure.
2. Stáhněte si certifikát sítě VPN.
2. Nasaďte certifikát k serveru VPN.

Azure AD používá certifikát sítě VPN k podepisování certifikátů, které jsou vystavené klientům Windows 10 při ověřování do služby Azure AD pro připojení k síti VPN. Token, který vyžaduje klienta Windows 10 je certifikát, který pak představuje aplikaci, která v tomto případě je na serveru VPN.

![Stáhněte si certifikát pro podmíněný přístup](./media/active-directory-conditional-access-vpn-connectivity-windows10/06.png)

Na portálu Azure můžete vytvořit dva certifikáty ke správě přechodu, když jeden certifikát již brzy vyprší. Když vytvoříte certifikát, je možné, zda se jedná o primární certifikát, který se používá při ověřování k podepsání certifikátu pro připojení.

Chcete-li vytvořit certifikát sítě VPN:

1. Přihlaste se k vaší [portál Azure](https://portal.azure.com) jako globální správce.

2. V nabídce vlevo klikněte na tlačítko **Azure Active Directory**. 

    ![Vyberte Azure Active Directory.](./media/active-directory-conditional-access-vpn-connectivity-windows10/01.png)

3. Na **Azure Active Directory** stránky v **spravovat** klikněte na tlačítko **podmíněného přístupu**.

    ![Vyberte podmíněného přístupu](./media/active-directory-conditional-access-azure-portal-get-started/02.png)

4. Na **podmíněného přístupu** stránky v **spravovat** klikněte na tlačítko **připojení VPN (preview)**.

    ![Vyberte připojení VPN](./media/active-directory-conditional-access-vpn-connectivity-windows10/03.png)

5. Na **připojení VPN** klikněte na tlačítko **nový certifikát**.

    ![Vyberte nový certifikát](./media/active-directory-conditional-access-vpn-connectivity-windows10/04.png)

6. Na **nový** proveďte následující kroky:

    ![Vyberte dobu trvání a primární](./media/active-directory-conditional-access-vpn-connectivity-windows10/05.png)

    a. Pro **vyberte dobu trvání**, vyberte **1 rok**.

    b. Pro **primární**, vyberte **Ano**.

    c. Klikněte na možnost **Vytvořit**.

7. Na stránce připojení sítě VPN klikněte na tlačítko **stažení certifikátu**.


Teď jste připravení nasadit nově vytvořený certifikát k serveru VPN. Na serveru VPN, přidejte stažený certifikát jako *důvěryhodná kořenová certifikační Autorita pro ověřování sítě VPN*.

Pro nasazení na základě RRAS systému Windows na serveru NPS, přidat kořenový certifikát do *Enterprise NTauth* uložit tak, že spustíte následující příkazy:

1. `certutil -dspublish <CACERT> RootCA`
2. `certutil -dspublish <CACERT> NtAuthCA`



## <a name="step-2-configure-your-vpn-client"></a>Krok 2: Konfigurace klienta VPN 

V tomto kroku nakonfigurujete váš profil připojení klienta VPN jak je uvedeno v [VPN a podmíněného přístupu](https://docs.microsoft.com/windows/access-protection/vpn/vpn-conditional-access).


## <a name="step-3-configure-your-conditional-access-policy"></a>Krok 3: Konfigurace zásady podmíněného přístupu

Tato část obsahuje pokyny pro konfiguraci zásady podmíněného přístupu pro připojení k síti VPN.


1. Na **podmíněného přístupu** stránky, na panelu nástrojů v horní části klikněte na tlačítko **přidat**.

    ![Vyberte Přidat na stránky podmíněného přístupu](./media/active-directory-conditional-access-vpn-connectivity-windows10/07.png)

2. Na **nový** stránky v **název** zadejte název vaší zásady. Můžete například zadat **zásady VPN**.

    ![Přidejte název zásady na stránce podmíněného přístupu](./media/active-directory-conditional-access-vpn-connectivity-windows10/08.png)

5. V **přiřazení** klikněte na tlačítko **uživatelů a skupin**.

    ![Vyberte uživatele a skupiny](./media/active-directory-conditional-access-vpn-connectivity-windows10/09.png)

6. Na **uživatelů a skupin** proveďte následující kroky:

    ![Vyberte testovacího uživatele](./media/active-directory-conditional-access-vpn-connectivity-windows10/10.png)

    a. Klikněte na tlačítko **vyberte uživatele a skupiny**.

    b. Klikněte na **Vybrat**.

    c. Na **vyberte** vyberte svého testovacího uživatele a pak klikněte na tlačítko **vyberte**.

    d. Na **uživatelů a skupin** klikněte na tlačítko **provést**.

7. Na **nový** proveďte následující kroky:

    ![Vyberte cloudové aplikace](./media/active-directory-conditional-access-vpn-connectivity-windows10/11.png)

    a. V **přiřazení** klikněte na tlačítko **cloudových aplikací**.

    b. Na **cloudových aplikací** klikněte na tlačítko **vybrat aplikace,**a pak klikněte na **vyberte**.

    c. Na **vyberte** stránky v **aplikace** zadejte **vpn**.

    d. Vyberte **Server sítě VPN**.

    e. Klikněte na **Vybrat**.


13. Na **nový** stránky, otevřete **Grant** stránky v **ovládací prvky** klikněte na tlačítko **Grant**.

    ![Vyberte grant](./media/active-directory-conditional-access-azure-portal-get-started/13.png)

14. Na **Grant** proveďte následující kroky:

    ![Vyberte vyžadovat vícefaktorové ověřování](./media/active-directory-conditional-access-azure-portal-get-started/14.png)

    a. Vyberte **vyžadovat vícefaktorové ověřování**.

    b. Klikněte na **Vybrat**.

15. Na **nový** v části **zapnout zásady**, klikněte na tlačítko **na**.

    ![Povolit zásady](./media/active-directory-conditional-access-azure-portal-get-started/15.png)

16. Na **nový** klikněte na tlačítko **vytvořit**.



## <a name="next-steps"></a>Další kroky

A získáte přehled o tom, jak Microsoft implementuje tuto funkci, najdete v části [rozšíření vzdáleného přístupu ve Windows 10 s profilem sítě VPN automatické](https://www.microsoft.com/itshowcase/Article/Content/894/Enhancing-remote-access-in-Windows-10-with-an-automatic-VPN-profile).    

