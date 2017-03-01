---
title: "Ověřování pomocí protokolu RADIUS a Azure MFA Server | Dokumentace Microsoftu"
description: "Toto je stránka Azure Multi-Factor Authentication, která vám pomůže při nasazení ověření RADIUS a serveru Azure Multi-Factor Authentication."
services: multi-factor-authentication
documentationcenter: 
author: kgremban
manager: femila
editor: yossib
ms.assetid: f4ba0fb2-2be9-477e-9bea-04c7340c8bce
ms.service: multi-factor-authentication
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 02/16/2017
ms.author: kgremban
translationtype: Human Translation
ms.sourcegitcommit: 999361daa2faebe3e88cab0b6085a938d6f40e9d
ms.openlocfilehash: 749267a5bb6e45714daa1fc06cd3aabcac326e76


---
# <a name="radius-authentication-and-azure-multi-factor-authentication-server"></a>Ověření služby RADIUS a server Azure Multi-Factor Authentication
Ověřování pomocí protokolu RADIUS můžete povolit a konfigurovat v části Azure MFA Serveru Ověřování pomocí protokolu RADIUS. RADIUS je standardní protokol pro přijímání požadavků na ověření a zpracování těchto požadavků. Azure Multi-Factor Authentication Server funguje jako server protokolu RADIUS. Vložte jej mezi vašeho klienta protokolu RADIUS (např. zařízení sítě VPN) a cíl ověřování, kterým může být služba Active Directory (AD), adresář LDAP nebo jiný server protokolu RADIUS, aby bylo možné přidat službu Azure Multi-Factor Authentication. Aby služba Azure Multi-Factor Authentication (MFA) fungovala, je nutné Azure MFA Server nakonfigurovat tak, aby komunikoval s klientskými servery i s cílem ověřování. Azure MFA Server přijímá požadavky od klienta protokolu RADIUS, ověřuje přihlašovací údaje proti cíli ověřování, přidává ověřování Azure Multi-Factor Authentication a odesílá odpověď zpět do klienta protokolu RADIUS. Požadavek na ověření bude úspěšný pouze v případě, že uspěje primární ověřování i ověřování Multi-Factor Authentication.

> [!NOTE]
> Server MFA podporuje pouze PAP (protokol ověřování hesla) a protokoly MSCHAPv2 (protokol ověřování Challenge Handshake společnosti Microsoft) RADIUS při fungování jako server RADIUS.  Když server MFA funguje jako proxy server protokolu RADIUS na jiný server protokolu RADIUS, který podporuje tento protokol, je možné použít jiné protokoly, například protokol EAP (Extensible Authentication Protocol).
> 
> V této konfiguraci nejsou funkční jednosměrné tokeny SMS a OATH, protože MFA Server nemůže vytvořit úspěšné odpovědi RADIUS na výzvy k ověření pomocí alternativních protokolů.

![Ověřování Radius](./media/multi-factor-authentication-get-started-server-rdg/radius.png)

## <a name="add-a-radius-client"></a>Přidání klienta protokolu RADIUS
Pro konfiguraci ověřování pomocí protokolu RADIUS nainstalujte server Azure Multi-Factor Authentication na server Windows. Pokud máte prostředí služby Active Directory, server by měl být připojen k doméně uvnitř sítě. Pomocí následujícího postupu nakonfigurujte server Azure Multi-Factor Authentication:

1. V rámci Azure Multi-Factor Authentication Serveru klikněte na ikonu Ověřování pomocí protokolu RADIUS v levé nabídce.
2. Zaškrtněte políčko **Povolit ověřování pomocí protokolu RADIUS**.
3. Na kartě klienti změňte porty Ověřování a Monitorování, pokud služba Azure MFA RADIUS potřebuje naslouchat požadavkům protokolu RADIUS na nestandardních portech.
4. Klikněte na tlačítko **Přidat**.
5. Zadejte IP adresu zařízení/serveru, který se bude ověřovat v Azure Multi-Factor Authentication Serveru, název aplikace (volitelné) a sdílený tajný klíč. 

  Název aplikace se zobrazí v sestavách služby Azure Multi-Factor Authentication a může se zobrazit v rámci SMS zpráv nebo mobilních aplikací ověřování.

  Sdílený tajný klíč musí být stejný jak na Azure Multi-Factor Authentication Serveru, tak i v zařízení/serveru. 

6. Zaškrtněte políčko **Vyžadovat porovnání uživatele u služby Multi-Factor Authentication**, pokud se všichni uživatelé importovali nebo budou importovat na server a budou podléhat vícefaktorovému ověřování. Pokud ještě na server nebyl importován velký počet uživatelů nebo budou uživatelé vyloučení z dvoustupňového ověření, nechte toto políčko nezaškrtnuté. 
7. Zaškrtněte políčko **Povolit záložní token OAUTH**, pokud chcete použít hesla OAUTH z aplikací pro mobilní ověření jako záložní ověřování pro telefonní hovory, SMS zprávy nebo nabízená oznámení mimo pásmo.
8. Klikněte na tlačítko **OK**.

Opakováním kroků 4 až 8 přidejte požadovaný počet dalších klientů protokolu RADIUS.

## <a name="configure-your-radius-client"></a>Konfigurace klienta protokolu RADIUS

1. Klikněte na kartu **Cíl**.
2. Pokud je Azure MFA Server nainstalován na server připojený k doméně v prostředí služby Active Directory, vyberte doménu systému Windows.
3. Pokud musí být uživatelé ověřováni proti adresáři LDAP, vyberte **Vázání protokolu LDAP**. 

  Pokud chcete použít vázání protokolu LDAP, klikněte na ikonu Integrace adresáře a na kartě Nastavení upravte konfiguraci protokolu LDAP tak, aby server mohl vytvořit vazbu k adresáři. Pokyny ke konfiguraci protokolu LDAP najdete v [průvodci konfigurací serveru proxy protokolu LDAP](multi-factor-authentication-get-started-server-ldap.md).

4. Pokud mají být uživatelé ověřování proti jinému serveru RADIUS, vyberte servery RADIUS.
5. Klikněte na **Přidat** a nakonfigurujte server, na který bude Azure MFA Server přes proxy předávat požadavky protokolu RADIUS.
6. V dialogovém okně Přidat server protokolu RADIUS zadejte IP adresu serveru protokolu RADIUS a sdílený tajný klíč. 

  Sdílený tajný klíč musí být stejný jak na Azure Multi-Factor Authentication Serveru, tak i na serveru protokolu RADIUS. Změňte port ověřování a port monitorování účtů, pokud server RADIUS využívá jiné porty.

7. Klikněte na tlačítko **OK**.
8. Přidejte Azure MFA Server jako klienta protokolu RADIUS v druhém serveru protokolu RADIUS, aby mohl zpracovávat požadavky na přístup odeslané z Azure MFA Serveru. Použijte stejný sdílený tajný klíč konfigurovaný na Azure Multi-Factor Authentication Serveru.

Opakováním těchto kroků přidejte další servery protokolu RADIUS a pomocí tlačítek **Přesunout nahoru** a **Přesunout dolů** nakonfigurujte pořadí, ve kterém je Azure MFA Server má volat. 

Dokončení konfigurace serveru Azure Multi-Factor Authentication. Server teď naslouchá na nakonfigurovaných portech požadavkům přístupu protokolu RADIUS z konfigurovaných klientů.   

## <a name="radius-client-configuration"></a>Konfigurace klienta protokolu RADIUS
Chcete-li nakonfigurovat klienta RADIUS, postupujte podle pokynů:

* Nakonfigurujte zařízení/server k ověřování prostřednictvím protokolu RADIUS nIP adrese serveru Azure Multi-Factor Authentication, která bude fungovat jako RADIUS server.
* Použijte stejný sdílený tajný klíč, který byl nakonfigurován dříve.
* Časový limit platnosti protokolu RADIUS nakonfigurujte na 30–60 sekund, aby byl dostatek času na ověření přihlašovacích údajů uživatele, provedení dvoustupňového ověřování, obdržení odpovědi a následnou odpověď na žádost o přístup protokolu RADIUS.




<!--HONumber=Feb17_HO3-->


