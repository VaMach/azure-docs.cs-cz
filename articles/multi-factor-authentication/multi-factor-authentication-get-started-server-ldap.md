---
title: "Ověřování pomocí protokolu LDAP a Azure MFA Server | Dokumentace Microsoftu"
description: "Toto je stránka Azure Multi-Factor Authentication, která vám pomůže při nasazení ověření LDAP a serveru Azure Multi-Factor Authentication."
services: multi-factor-authentication
documentationcenter: 
author: kgremban
manager: femila
editor: yossib
ms.assetid: e1a68568-53d1-4365-9e41-50925ad00869
ms.service: multi-factor-authentication
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 01/03/2017
ms.author: kgremban
translationtype: Human Translation
ms.sourcegitcommit: 1c6386dda94a3e0ca6eb340f542d04cb336159c3
ms.openlocfilehash: 0de97050e385e3efb9e63bbf934712157ab0d0af


---
# <a name="ldap-authentication-and-azure-multi-factor-authentication-server"></a>Ověření služby LDAP a server Azure Multi-Factor Authentication
Ve výchozím nastavení je server Azure Multi-Factor Authentication nakonfigurován pro import nebo synchronizaci uživatelů ze služby Active Directory. Můžete ho však navázat na různé adresáře LDAP, například adresář ADAM nebo konkrétní řadič domény služby Active Directory. Když je připojen k adresáři prostřednictvím protokolu LDAP, je možné Azure Multi-Factor Authentication Server nakonfigurovat tak, aby se choval jako proxy protokolu LDAP a prováděl ověřování. Umožňuje také použití vazby protokolu LDAP jako cíle protokolu RADIUS pro předběžné ověřování uživatelů pomocí ověření IIS, nebo pro primární ověřování na portálu Azure MFA User Portal.

Pokud chcete službu Azure Multi-Factor Authentication použít jako proxy protokolu LDAP, vložte Azure Multi-Factor Authentication Server mezi klienta protokolu LDAP (např. zařízení sítě VPN, aplikace) a adresářový server LDAP. Azure Multi-Factor Authentication Server musí být nakonfigurován tak, aby komunikoval s klientskými servery i s adresářem LDAP. V této konfiguraci server Azure Multi-Factor Authentication přijímá požadavky LDAP od klientských serverů a aplikací a předává je cílovému adresářovému serveru LDAP pro ověření primárních pověření. Pokud odpověď z adresáře LDAP ukazuje, že primární přihlašovací údaje jsou platné, provede Azure Multi-Factor Authentication druhé ověření identity a odešle odpověď zpět do klienta protokolu LDAP. Celkové ověření proběhne úspěšně pouze pokud je úspěšné ověření pomocí serveru LDAP i druhý krok ověření.

## <a name="ldap-authentication-configuration"></a>Konfigurace ověření LDAP
Pro konfiguraci ověřování pomocí protokolu LDAP nainstalujte server Azure Multi-Factor Authentication na server Windows. Použijte následující postup:

### <a name="add-an-ldap-client"></a>Přidání klienta protokolu LDAP

1. V Azure Multi-Factor Authentication Serveru vyberte v levé nabídce ikonu Ověřování pomocí protokolu LDAP.
2. Zaškrtněte políčko **Povolit ověřování pomocí protokolu LDAP**.

        ![LDAP Authentication](./media/multi-factor-authentication-get-started-server-ldap/ldap2.png)

3. Na kartě Klienti změňte port TCP a SSL, pokud má služba Azure Multi-Factor Authentication LDAP vytvořit vazbu na nestandardní porty pro naslouchání požadavkům protokolu LDAP.
4. Pokud plánujete používat LDAPS z klienta na server Azure Multi-Factor Authentication, musí být na serveru, na kterém server běží, nainstalován certifikát SSL. Klikněte na **Procházet...** vedle pole certifikátu SSL a vyberte nainstalovaný certifikát, který bude použit pro zabezpečené připojení.
5. Klikněte na **Přidat...**
6. V dialogovém okně Přidat klienta protokolu LDAP zadejte IP adresu zařízení, serveru nebo aplikace, které se budou ověřovat u Serveru, a název aplikace (volitelné). Název aplikace se zobrazí v sestavách Azure Multi-Factor Authentication a může se zobrazit v rámci SMS zpráv nebo mobilních aplikací ověřování.
7. Zaškrtněte políčko **Vyžadovat porovnání uživatele Azure Multi-Factor Authentication**, pokud byli nebo budou všichni uživatelé importováni na server a podstoupí dvoustupňové ověření. Pokud ještě na server nebyl importován velký počet uživatelů nebo budou uživatelé vyloučení z dvoustupňového ověření, nechte toto políčko nezaškrtnuté. Další informace o této funkci najdete v souboru nápovědy.

Opakujte tyto kroky pro přidání dalších klientů protokolu LDAP.

### <a name="configure-the-ldap-directory-connection"></a>Konfigurace připojení k adresáři LDAP

Pokud je Azure Multi-Factor Authentication nakonfigurováno pro příjem ověřování LDAP, musí směrovat proxy těchto ověřování do adresáře protokolu LDAP. Proto karta Cíl pouze zobrazí jednu zašedlou možnost použití cíle LDAP.

1. Pro konfiguraci připojení k adresáři LDAP klikněte na ikonu **Integrace adresáře**.
2. Na kartě Nastavení vyberte přepínač **Použít specifickou konfiguraci LDAP**.
3. Vyberte **Upravit...**
4. V dialogovém okně Upravit konfiguraci LDAP vyplňte pole pomocí informací požadovaných pro připojení k adresáři protokolu LDAP. Popisy těchto polí jsou uvedeny v souboru nápovědy Azure Multi-Factor Authentication Serveru.

    ![Integrace adresáře](./media/multi-factor-authentication-get-started-server-ldap/ldap.png)

5. Otestujte připojení LDAP kliknutím na tlačítko **Test**.
6. Pokud byl test připojení LDAP úspěšný, klikněte na tlačítko **OK**.
7. Klikněte na kartu **Filtry**. Server je předem nakonfigurovaný k načítání kontejnerů, skupin zabezpečení a uživatelů ze služby Active Directory. Pokud provádíte navázání na jiný adresář LDAP, budete pravděpodobně muset upravit zobrazené filtry. Kliknutím na odkaz **Nápověda** zobrazíte další informace o filtrech.
8. Klikněte na kartu **Atributy**. Server je předem nakonfigurovaný k mapování atributů ze služby Active Directory.
9. Pokud provádíte navázání na jiný adresář LDAP nebo chcete změnit předem nakonfigurované mapování atributů, klikněte na **Upravit...**
10. V dialogovém okně Upravit atributy upravte mapování atributů protokolu LDAP pro váš adresář. Názvy atributů můžete zadat nebo vybrat kliknutím na **...** na tlačítko vedle každého pole. Kliknutím na odkaz **Nápověda** zobrazíte další informace o atributech.
11. Klikněte na tlačítko **OK**.
12. Klikněte na ikonu **Nastavení společnosti** a vyberte kartu **Překlad uživatelského jména**.
13. Pokud se připojujete ke službě Active Directory ze serveru připojeného k doméně, ponechejte vybraný přepínač **Pro porovnání uživatelských jmen použít identifikátory zabezpečení systému Windows (SID)**. Jinak vyberte přepínač **Pro porovnávání uživatelských jmen použít atribut jedinečného identifikátoru LDAP**. 

Když je vybraný přepínač **Pro porovnávání uživatelských jmen použít atribut jedinečného identifikátoru LDAP**, Azure Multi-Factor Authentication Server se pokusí každé uživatelské jméno přeložit na jedinečný identifikátor v adresáři LDAP. Provede se vyhledávání protokolu LDAP u atributů uživatelského jména definovaných v části Integrace adresáře na kartě Atributy. Při ověřování uživatele se uživatelské jméno přeloží na jedinečný identifikátor v adresáři LDAP a tento jedinečný identifikátor se použije pro porovnání uživatele v datovém souboru Azure Multi-Factor Authentication. To umožňuje porovnávání bez rozlišování velkých a malých písmen a používání dlouhého i krátkého formátu uživatelského jména.

Po dokončení těchto kroků začne MFA Server naslouchat na nakonfigurovaných portech žádostem o přístup prostřednictvím protokolu LDAP z nakonfigurovaných klientů a je nastaven pro předávání těchto žádostí přes proxy do adresáře LDAP k ověření.

## <a name="ldap-client-configuration"></a>Konfigurace klienta LDAP
Chcete-li nakonfigurovat klienta LDAP, postupujte podle pokynů:

* Nakonfigurujte zařízení, server nebo aplikaci k ověřování prostřednictvím protokolu LDAP na Azure Multi-Factor Authentication Serveru, jako by šlo o váš adresář LDAP. Použijte stejné nastavení, které běžně používáte pro připojení přímo k adresáři LDAP, s výjimkou názvu serveru nebo IP adresy, u kterých použijte hodnoty pro Azure Multi-Factor Authentication Server.
* Časový limit platnosti protokolu LDAP nakonfigurujte na 30–60 sekund, aby byl dostatek času na ověření přihlašovacích údajů uživatele v adresáři LDAP, provedení druhého kroku ověřování, obdržení odpovědi a odeslání odpovědi na žádost o přístup prostřednictvím protokolu LDAP.
* V případě použití LDAPS musí zařízení nebo server provádějící žádosti LDAP důvěřovat certifikátu SSL nainstalovaném na serveru Azure Multi-Factor Authentication.




<!--HONumber=Jan17_HO1-->


