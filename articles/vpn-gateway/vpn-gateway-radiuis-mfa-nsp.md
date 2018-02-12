---
title: "Zabezpečené Azure VPN gateway ověřování RADIUS se serverem NPS pro službu Multi-Factor Authentication | Microsoft Docs"
description: "Popisuje integrovat Azure brány ověřování pomocí protokolu RADIUS serveru NPS pro službu Multi-Factor Authentication."
services: vpn-gateway
documentationcenter: na
author: genlin
manager: willchen
editor: 
tags: azure-resource-manager
ms.assetid: 
ms.service: vpn-gateway
ms.devlang: na
ms.topic: 
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/09/2018
ms.author: genli
ms.openlocfilehash: 0754c6cab9de2f93e9a57e202227a81c51bedf4c
ms.sourcegitcommit: 4723859f545bccc38a515192cf86dcf7ba0c0a67
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 02/11/2018
---
# <a name="integrate-azure-vpn-gateway-radius-authentication-with-nps-server-for-multi-factor-authentication"></a>Ověřování RADIUS brány Azure VPN integrovat NPS server pro službu Multi-Factor Authentication 

Článek popisuje, jak integrovat Server NPS (Network Policy Server) serveru s ověřováním protokolu RADIUS brány Azure VPN k poskytování služby Multi-Factor Authentication (MFA) pro připojení VPN typu Point-to-site. 

## <a name="prerequisite"></a>Požadavek

Jak zapnout MFA, uživatelé musí být v Azure Active Directory (synchronizovaná z místní nebo cloudové pouze) a uživatel musí již dokončení ověření si proces pro MFA.  Uživatele můžete dokončit proces ověření pomocí https://myapps.microsoft.com.

## <a name="detailed-steps"></a>Podrobné kroky

### <a name="step-1-create-virtual-network-gateway"></a>Krok 1 vytvořit bránu virtuální sítě

1. Přihlaste se k portálu [Azure Portal](https://portal.azure.com).
2. Ve virtuální síti, který bude hostitelem bránu virtuální sítě, vyberte **podsítě**a potom vyberte **podsíť brány** vytvořit podsíť. 

    ![Obrázek o tom, jak přidat podsíť brány](./media/vpn-gateway-radiuis-mfa-nsp/gateway-subnet.png)
3. Vytvoření brány virtuální sítě s následujícím nastavením:

    - **Typ brány**: vyberte **VPN**.
    - **Typ sítě VPN**: vyberte **založené na trasách**.
    - **Skladová položka**: Vyberte typ SKU podle svých požadavků.
    - **Virtuální síť**: vyberte virtuální síť, který jste vytvořili podsíť brány.

        ![Obrázek o nastavení brány virtuální sítě](./media/vpn-gateway-radiuis-mfa-nsp/create-vpn-gateway.png)


 
### <a name="step-2-configure-the-nps-for-azure-mfa"></a>Krok 2 konfigurace na server NPS pro Azure MFA

1. Na serveru NPS [instalace serveru NPS rozšíření pro Azure MFA](../multi-factor-authentication/multi-factor-authentication-nps-extension.md#install-the-nps-extension).
2. Otevřete konzolu NSP, klikněte pravým tlačítkem na **RADUIS klienti**, vyberte **nový**. Vytvoření klienta RADUIS s následujícím nastavením:

    - **Popisný název**: napište libovolný název.
    - **Adresa (IP adresu nebo DNS)**: Zadejte podsíť brány, který jste vytvořili v kroku 1.
    - **Sdílený tajný klíč**: Zadejte jakékoli tajný klíč a zapamatovat si ho. Použijeme ji později.

    ![Obrázek o nastavení klienta RADUIS](./media/vpn-gateway-radiuis-mfa-nsp/create-radius-client1.png)

 
3.  V **Upřesnit** kartě, název dodavatele nastavte **RADIUS standardní** a ujistěte se, že **další možnosti** nejsou vybrané.

    ![Bitovou kopii RADUIS klienta rozšířené možnosti nastavení](./media/vpn-gateway-radiuis-mfa-nsp/create-radius-client2.png)

4. Přejděte na **zásady** > **zásady sítě**, dvakrát klikněte na **připojení k serveru Microsoft Routing a vzdálený přístup** zásad, vyberte  **Udělení přístupu**a potom klikněte na **OK**.

### <a name="step-3-configure-the-virtual-network-gateway"></a>Krok 3 konfigurovat bránu virtuální sítě

1. Přihlaste se k portálu [Azure Portal](https://portal.azure.com).
2. Otevřete bránu virtuální sítě, který jste vytvořili, ujistěte se, že typ brány je **VPN** a typ sítě VPN je **založené na trasách**.
3. Klikněte na tlačítko **přejděte na konfigurace lokality** > **nyní nakonfigurovat**a poté přidejte následující nastavení:

    - **Fond adres**: Zadejte podsíť brány, který jste vytvořili v kroku 1.
    - **Typ ověřování**: vyberte **ověřování pomocí protokolu RADIUS**.
    - **IP adresa serveru**: Zadejte IP adresu serveru NPS.

    ![Obrázek o, přejděte na nastavení lokality](./media/vpn-gateway-radiuis-mfa-nsp/configure-p2s.png)

## <a name="next-steps"></a>Další postup

- [Azure Multi-Factor Authentication](../multi-factor-authentication/multi-factor-authentication.md)
- [Integrace vaší stávající infrastruktury NPS se službou Multi-Factor Authentication](../multi-factor-authentication/multi-factor-authentication-nps-extension.md)
