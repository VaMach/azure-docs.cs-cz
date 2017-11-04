---
title: "VPN integraci s Azure MFA pomocí serveru NPS rozšíření | Microsoft Docs"
description: "Tento článek popisuje integraci infrastrukturu sítě VPN s Azure MFA pomocí serveru NPS (Network Policy Server) rozšíření pro Microsoft Azure."
services: active-directory
keywords: "Azure MFA integrovat sítě VPN, Azure Active Directory, Network Policy Server rozšíření"
documentationcenter: 
author: kgremban
manager: femila
ms.assetid: 
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/15/2017
ms.author: kgremban
ms.reviewer: jsnow
ms.custom: it-pro
ms.openlocfilehash: 8ac4c5d88e724febf21fe6bcc8ecf939283f361e
ms.sourcegitcommit: f8437edf5de144b40aed00af5c52a20e35d10ba1
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/03/2017
---
# <a name="integrate-your-vpn-infrastructure-with-azure-multi-factor-authentication-mfa-using-the-network-policy-server-nps-extension-for-azure"></a>Integrovat infrastrukturu sítě VPN s Azure Multi-Factor Authentication (MFA) pomocí rozšíření serveru NPS (Network Policy Server) pro Azure.

## <a name="overview"></a>Přehled

Rozšíření serveru NPS (Network Policy Server) pro Azure umožňuje organizacím zabezpečit pomocí cloudového ověřování klientů RADIUS Remote Authentication Dial-In User Service () [Azure Multi-Factor Authentication (MFA)](multi-factor-authentication-get-started-server-rdg.md), který nabízí dvoustupňové ověřování.

Tento článek obsahuje pokyny k integraci s Azure MFA serveru NPS infrastruktury pomocí rozšíření NPS pro Azure. Chcete-li povolit zabezpečené dvoustupňové ověřování pro uživatele, na které se pokouší o připojení k síti pomocí sítě VPN. 

Síťové zásady a přístup služby (NPS) poskytuje organizacím následující možnosti:

* Zadejte centrální umístění pro správu a řízení požadavků sítě k určení, kdo se může připojit, denní jsou připojení povolena, jakou dobu trvání připojení a úroveň zabezpečení, které musí klienti používat pro připojení a tak dále. Místo zadat tyto zásady na každém serveru sítě VPN nebo brány vzdálené plochy (RD), tyto zásady můžete zadat jednou v centrálním umístění. Protokol RADIUS slouží k centralizované ověřování, autorizace a monitorování (AAA). 
* Stanovit a vynutit zásady stavu klienta Network Access Protection (NAP), které určují, jestli zařízení mají neomezeného nebo omezeného přístupu k síťovým prostředkům.
* Zadejte způsob k vynucení ověřování a autorizace pro přístup k protokolu 802. 1 x podporující bezdrátové přístupové body a přepínače sítě Ethernet.    

Další informace najdete v tématu [Server NPS (Network Policy Server)](https://docs.microsoft.com/windows-server/networking/technologies/nps/nps-top). 

Pokud chcete zvýšit zabezpečení a zajistit vysokou úroveň dodržování předpisů, můžete integrovat organizace NPS s Azure MFA a zkontrolujte, jestli uživatelé používají dvoustupňové ověření, abyste mohli připojit virtuální port na serveru VPN. Uživatelům udělen přístup musí zadat jejich kombinace uživatelského jména a hesla s informacemi, které má uživatel v jejich řízení. Tyto informace musí být důvěryhodný a duplikovaná není snadno, například číslo mobilního telefonu, číslo pevné, aplikace na mobilní zařízení a tak dále.

Před dostupnost serveru NPS rozšíření pro Azure zákazníky, kteří si přáli implementovat dvoustupňové ověřování pro integrované prostředí NPS a Azure MFA museli konfigurovat a spravovat samostatný Server MFA v místním prostředí, jak je uvedeno v Vzdálená plocha brány a Server Azure Multi-Factor Authentication pomocí protokolu RADIUS.

Dostupnost serveru NPS rozšíření pro Azure teď poskytuje organizacím rozhodnout pro nasazení do řešení vícefaktorového ověřování pro místní nebo cloudové řešení vícefaktorového ověřování do zabezpečeného ověřování klienta protokolu RADIUS.
 
## <a name="authentication-flow"></a>Tok ověřování
Když se uživatel připojí na virtuální port na serveru VPN, musí je nejdřív ověřit pomocí různých protokolů, které umožňuje použít kombinaci uživatelské jméno a heslo a metody ověřování založené na certifikátu. 

Kromě ověřování a ověření identity, uživatelé musí mít příslušná oprávnění. V jednoduché implementace jsou tato oprávnění dial-in, které umožňují přístup nastavit přímo na objekty uživatele služby Active Directory. 

 ![Vlastnosti uživatele.](./media/nps-extension-vpn/image1.png)

Každý server sítě VPN pro jednoduché implementace, uděluje nebo odmítne přístup na základě zásad, které jsou definované na každém serveru místní VPN.

V implementacích větší a větší škálovatelnost zásady této udělit nebo odepřít přístup k síti VPN jsou centralizované na servery RADIUS. V tomto případě serverem VPN funguje jako server služby přístupu (klienta RADIUS), který předává žádosti o připojení a zprávy účtu na serveru RADIUS. Pro připojení k virtuálnímu portu na serveru VPN, uživatelé musí být ověřeny a splňovat podmínky, které jsou definované centrálně na servery RADIUS. 

Když NPS rozšíření pro Azure je integrována na server NPS, tok úspěšné ověření vypadá takto:

1. Server sítě VPN obdrží požadavek na ověření od uživatele sítě VPN, která obsahuje uživatelské jméno a heslo pro připojení k prostředku, jako je například relaci vzdálené plochy. 
2. Funguje jako klienta protokolu RADIUS, VPN server převede požadavek na zprávu protokolu RADIUS – žádost o přístup a odešle zprávu (heslo je zašifrováno) na server RADIUS (NPS), kde je nainstalován server NPS rozšíření. 
3. Kombinace uživatelského jména a hesla je ověřit ve službě Active Directory. Pokud uživatelské jméno / heslo není správné, odešle RADIUS Server zprávu odepření přístupu. 
4. Pokud jsou splněny všechny podmínky zadané v žádosti o připojení serveru NPS a zásady sítě (například čas, den nebo skupiny omezeními pro členství vyplývajícími), NPS rozšíření aktivuje žádost o sekundárního ověřování s Azure MFA. 
5. Azure MFA komunikuje se službou Azure Active Directory, načte podrobnosti uživatele a provádí sekundární ověřování metodou nakonfigurovaná uživatelem (textová zpráva, mobilní aplikace a tak dále). 
6. Po úspěšné ověřovací test MFA Azure MFA komunikuje výsledek, který má rozšíření serveru NPS.
7. Po ověření i autorizace server NPS, kde je nainstalován rozšíření odešle zprávu RADIUS s potvrzením přístupu k serveru sítě VPN (klienta RADIUS).
8. Uživateli je udělen přístup k virtuálnímu portu na serveru VPN a vytvoří šifrované tunelové propojení VPN.

## <a name="prerequisites"></a>Požadavky
Tato část podrobně předpoklady nezbytné před integrace Azure MFA s služby Brána vzdálené plochy. Než začnete, musí mít následující požadavky na místě.

* Infrastruktura sítě VPN.
* Role Síťové zásady a přístup služby NPS)
* Licence Azure MFA
* Software Windows serveru
* Knihovny
* Azure AD synchronizována s místní AD 
* ID identifikátor GUID služby Azure Active Directory

### <a name="vpn-infrastructure"></a>Infrastruktura sítě VPN.
Tento článek předpokládá, že máte pracovní infrastrukturu sítě VPN pomocí Microsoft Windows Server 2016 na místě a, VPN server není nyní nakonfigurován, aby předával požadavky na RADIUS server připojení. Můžete nakonfigurovat infrastrukturu sítě VPN na použití centrálního serveru RADIUS v této příručce.

Pokud nemáte pracovní infrastruktury na místě, můžete rychle vytvořit této infrastruktury podle následujících pokynů uvedených v množství kurzech instalační program VPN, které můžete najít na společnosti Microsoft a weby třetích stran. 

### <a name="network-policy-and-access-services-nps-role"></a>Role Síťové zásady a přístup služby NPS)

Služba role NPS poskytuje RADIUS server a funkce klienta. Tento článek předpokládá, že jste nainstalovali NPS role na členský server nebo řadič domény ve vašem prostředí. Pro konfiguraci sítě VPN v tomto průvodci můžete nakonfigurovat protokolu RADIUS. Instalace role serveru NPS na serveru _jiných_ než je na serveru VPN.

Informace o instalaci role serveru NPS služby Windows Server 2012 nebo vyšší, najdete v části [nainstalovat Server zásad stavu NAP](https://technet.microsoft.com/library/dd296890.aspx). Zásady NAP (Network Access) je ve Windows serveru 2016 zastaralá. Popis osvědčené postupy pro server NPS, včetně doporučení k instalaci NPS na řadiči domény najdete v části [osvědčené postupy pro server NPS](https://technet.microsoft.com/library/cc771746).

### <a name="azure-mfa-license"></a>Licence Azure MFA

Vyžaduje se licenci pro Azure MFA, která je k dispozici prostřednictvím Azure AD Premium, Enterprise Mobility plus Security (EMS) nebo předplatné vícefaktorového ověřování. Další informace najdete v tématu [jak získat Azure Multi-Factor Authentication](multi-factor-authentication-versions-plans.md). Pro účely testování můžete použít zkušební verzi předplatného.

### <a name="windows-server-software"></a>Software Windows serveru

Rozšíření serveru NPS vyžaduje Windows Server 2008 R2 SP1 nebo novějším s nainstalovanou službou role NPS. Všechny kroky v této příručce se provádí pomocí systému Windows Server 2016.

### <a name="libraries"></a>Knihovny

Tyto knihovny jsou automaticky nainstalovány s příponou.

-   [Distribuovatelné balíčky Visual C++ pro Visual Studio 2013 (X64)](https://www.microsoft.com/download/details.aspx?id=40784)
-   [Microsoft Azure Active Directory modul pro prostředí Windows PowerShell verze 1.1.166.0](https://connect.microsoft.com/site1164/Downloads/DownloadDetails.aspx?DownloadID=59185)

Microsoft Azure Active Directory modul pro prostředí Windows PowerShell je nainstalován, pokud ještě není přítomný, prostřednictvím konfigurační skript, které spustíte jako součást procesu instalace. Není nutné k instalaci tohoto modulu předem, pokud již není nainstalována.

### <a name="azure-active-directory-synched-with-on-premises-active-directory"></a>Azure Active Directory synchronizována s místní služby Active Directory 

Použití rozšíření serveru NPS, musí být místních uživatelů se synchronizují s Azure Active Directory a povolené pro službu Multi-Factor Authentication. Tato příručka předpokládá, že jsou synchronizována místních uživatelů s Azure Active Directory pomocí služby AD Connect. Pokyny k povolení uživatelů pro MFA jsou uvedeny níže.
Informace o Azure AD connect, najdete v části [integraci místních adresářů se službou Azure Active Directory](../active-directory/connect/active-directory-aadconnect.md). 

### <a name="azure-active-directory-guid-id"></a>ID identifikátor GUID služby Azure Active Directory 
Pokud chcete nainstalovat rozšíření serveru NPS, musíte znát identifikátor GUID služby Azure Active Directory. Pokyny pro hledání identifikátor GUID služby Azure Active Directory najdete v další části.

## <a name="configure-radius-for-vpn-connections"></a>Konfigurace protokolu RADIUS pro připojení k síti VPN

Pokud jste nainstalovali roli serveru NPS na členském serveru, musíte nakonfigurovat k ověřování a autorizaci klienta VPN, že žádost o připojení k síti VPN. 

V této části předpokládá, že jste nainstalovali roli serveru zásad sítě, ale nenakonfigurovali ho pro použití ve vaší infrastruktuře.

>[!NOTE]
>Pokud už máte pracovní serveru VPN, který používá centralizovaný server RADIUS pro ověřování, můžete tuto část přeskočit.
>

### <a name="register-server-in-active-directory"></a>Registrace serveru ve službě Active Directory
V tomto scénáři, fungovat správně, je potřeba zaregistrovat ve službě Active Directory serveru NPS.

1. Otevřete správce serveru.
2. Ve Správci serveru klikněte na tlačítko **nástroje**a potom klikněte na **Network Policy Server**. 
3. V konzole serveru NPS klikněte pravým tlačítkem na **server NPS (místní)**a potom klikněte na **server zaregistrovat ve službě Active Directory**. Klikněte na tlačítko **OK** dvakrát.

 ![Server NPS](./media/nps-extension-vpn/image2.png)

4. Ponechte konzole otevřené pro následující postup.

### <a name="use-wizard-to-configure-radius-server"></a>Použití Průvodce konfigurací serveru RADIUS
Můžete použít standardní (založené na průvodci) nebo rozšířené možnosti konfigurace pro konfigurace serveru RADIUS. V této části se předpokládá použití možnosti založené na průvodci standardní konfigurace.

1. V konzole serveru NPS klikněte na **server NPS (místní)**.
2. V části standardní konfiguraci, vyberte **Server RADIUS pro vytáčená nebo VPN připojení**a potom klikněte na **konfigurace sítě VPN nebo telefonické**.

 ![Konfigurace sítě VPN](./media/nps-extension-vpn/image3.png)

3. Na vyberte telefonicky nebo typ síťového připojení virtuální privátní stránce vyberte **připojení k virtuální privátní síti**a klikněte na tlačítko **Další**.

 ![Virtuální privátní síť](./media/nps-extension-vpn/image4.png)

4. Na stránce zadejte vytáčená nebo VPN serveru klikněte na **přidat**.
5. V **klienta RADIUS nové** dialogové okno, zadejte popisný název, zadejte rozlišitelný název nebo IP adresu serveru VPN a zadejte sdílený tajný heslo. Ujistěte se, toto sdílené tajné heslo dlouhá a složitá. Toto heslo si poznamenejte, budete potřebovat pro kroků v další části.

 ![Nový klient protokolu RADIUS](./media/nps-extension-vpn/image5.png)

6. Klikněte na tlačítko **OK**a potom **Další**.
7. Na **konfigurovat metody ověřování** stránky, přijměte výchozí hodnotu (šifrované ověření verze 2 (MS-CHAPv2) nebo zvolte jinou možnost a klikněte na tlačítko **Další**.

  >[!NOTE]
  >Při konfiguraci protokolu EAP (Extensible Authentication), musíte použít MS CHAPv2 nebo protokolem PEAP. Žádné jiné EAP je podporována.
 
8. Na stránce zadat skupiny uživatelů, klikněte na **přidat** a vyberte příslušné skupiny, pokud existuje. V opačném nezadáte výběr udělit přístup všem uživatelům.

 ![Určete skupiny uživatelů](./media/nps-extension-vpn/image7.png)

9. Klikněte na **Další**.
10. Na stránce zadat filtry IP adres klikněte na **Další**.
11. Na stránce zadat nastavení šifrování, přijměte výchozí nastavení a klikněte na **Další**.

 ![Zadejte možnost Encryption](./media/nps-extension-vpn/image8.png)

12. Na zadejte název sféry nechejte prázdné, název, přijměte výchozí nastavení a klikněte na **Další**.

 ![Zadejte název sféry](./media/nps-extension-vpn/image9.png)

13. Na dokončení nové telefonické nebo stránku klienti připojení k virtuální privátní síti a protokolu RADIUS, klikněte na tlačítko **Dokončit**.

 ![Dokončení připojení](./media/nps-extension-vpn/image10.png)

### <a name="verify-radius-configuration"></a>Ověření konfigurace RADIUS
Tato část Podrobnosti o konfiguraci, kterou jste vytvořili pomocí průvodce.

1. Na serveru NPS, v konzole serveru NPS (místní) rozbalte klientů RADIUS a vyberte **klientů RADIUS**.
2. V podokně podrobností klikněte pravým tlačítkem na klienta RADIUS, který jste vytvořili pomocí průvodce a klikněte na tlačítko **vlastnosti**. Vlastnosti pro vašeho klienta protokolu RADIUS (VPN server) by měl být podobné těm, které vidíte níže.

 ![Vlastnosti sítě VPN](./media/nps-extension-vpn/image11.png)

3. Klikněte na tlačítko **zrušit**.
4. Na serveru NPS, v konzole serveru NPS (místní) rozbalte **zásady**a vyberte **zásady vyžádání nového připojení**. Měli byste vidět zásad připojení k síti VPN, která se podobá následující obrázek.

 ![Žádosti o připojení](./media/nps-extension-vpn/image12.png)

5. V části zásady, vyberte **zásady sítě**. Měli byste zásady připojení virtuální privátní sítě (VPN), která se podobá následující obrázek.

 ![Vlastnosti sítě](./media/nps-extension-vpn/image13.png)

## <a name="configure-vpn-server-to-use-radius-authentication"></a>Konfigurace serveru sítě VPN pro použití ověřování pomocí protokolu RADIUS
V této části nakonfigurujete server sítě VPN, aby používal ověřování pomocí protokolu RADIUS. V této části se předpokládá, že máte funkční konfigurace serveru VPN, ale nenakonfigurovali server sítě VPN, aby používal ověřování pomocí protokolu RADIUS. Po dokončení konfigurace serveru VPN, potvrďte, že vaše konfigurace funguje podle očekávání.

>[!NOTE]
>Pokud již máte funkční konfigurace serveru VPN, který používá ověřování pomocí protokolu RADIUS, můžete tuto část přeskočit.
>

### <a name="configure-authentication-provider"></a>Konfigurace zprostředkovatele ověřování
1. Na serveru VPN spusťte Správce serveru.
2. Ve Správci serveru klikněte na tlačítko **nástroje**a potom **směrování a vzdálený přístup**.
3. V konzole Směrování a vzdálený přístup, klikněte pravým tlačítkem na  **\[název serveru\] (místní)**a potom klikněte na **vlastnosti**.

 ![Směrování a vzdálený přístup](./media/nps-extension-vpn/image14.png)
 
4. V **[název serveru} (místní) vlastnosti** dialogové okno, klikněte na tlačítko **zabezpečení** kartě. 
5. Na **zabezpečení** karta, v rámci zprostředkovatele ověřování, klikněte na tlačítko **ověřování pomocí protokolu RADIUS**a potom **konfigurace**.

 ![Ověřování RADIUS](./media/nps-extension-vpn/image15.png)
 
6. V dialogovém okně ověřování pomocí protokolu RADIUS klikněte na **přidat**.
7. Přidat Server RADIUS, v názvu serveru přidejte název nebo adresu IP ze serveru RADIUS, které jste nakonfigurovali v předchozím oddílu.
8. V sdílený tajný klíč, klikněte na **změnu** a přidejte sdílené tajné heslo jste vytvořili a poznamenali dříve.
9. Časový limit (v sekundách), změňte hodnotu na hodnotu mezi **30** a **60**. To je potřeba povolit dostatek času k dokončení druhý ověřovací faktor.
 
 ![Přidat RADIUS Server](./media/nps-extension-vpn/image16.png)
 
10. Klikněte na tlačítko **OK** dokud nedokončíte zavřít všechna dialogová okna.

### <a name="test-vpn-connectivity"></a>Test připojení VPN
V této části potvrďte, že klient VPN ověří a autorizuje serverem RADIUS při pokusu o připojení k VPN virtuální port. V této části se předpokládá, že používáte Windows 10 jako klient VPN. 

>[!NOTE]
>Pokud jste již nakonfigurovali klienta VPN pro připojení k serveru VPN a uložení nastavení, můžete přeskočit kroky týkající se konfigurace a uložení objekt připojení VPN.
>

1. Na klientském počítači sítě VPN, klikněte na tlačítko **spustit**a potom **nastavení** (ozubené kolečko ikona).
2. V okně nastavení klikněte na tlačítko **síť a Internet**.
3. Klikněte na tlačítko **VPN**.
4. Klikněte na tlačítko **přidat připojení VPN**.
5. V přidat připojení VPN, zadejte Windows (předdefinovaná) jako poskytovatele sítě VPN, pak dokončit zbývající pole, podle potřeby a klikněte na tlačítko **Uložit**. 

 ![Přidat připojení VPN](./media/nps-extension-vpn/image17.png)
 
6. Otevřete **Centrum sítí a sdílení** v Ovládacích panelech.
7. Klikněte na tlačítko **změnit nastavení adaptéru**.

 ![Změnit nastavení adaptéru](./media/nps-extension-vpn/image18.png)

8. Klikněte pravým tlačítkem na připojení sítě VPN a klikněte na položku Vlastnosti. 

 ![Vlastnosti sítě VPN](./media/nps-extension-vpn/image19.png)

9. V dialogovém okně Vlastnosti sítě VPN klikněte na **zabezpečení** kartě. 
10. Na kartě zabezpečení zkontrolujte pouze **Microsoft CHAP Version 2 (MS-CHAP v2)** je vybrána a klikněte na tlačítko OK.

 ![Povolit protokoly](./media/nps-extension-vpn/image20.png)

11. Klikněte pravým tlačítkem na připojení k síti VPN a klikněte na tlačítko **Connect**.
12. Na stránce nastavení, klikněte na **Connect**.

Úspěšné připojení zobrazí se v protokolu zabezpečení na serveru RADIUS jako 6272 ID událostí, jak je uvedeno níže.

 ![Vlastnosti události](./media/nps-extension-vpn/image21.png)

## <a name="troubleshoot-guide"></a>Řešení potíží s Průvodce
Předpokládejme, že konfiguraci VPN fungovala předtím, než jste nakonfigurovali server VPN používat centralizovaný server RADIUS pro ověřování a autorizaci. V takovém případě je pravděpodobné, že tento problém může být způsobeno nesprávnou konfiguraci serveru RADIUS nebo použití neplatného uživatelského jména nebo hesla. Například pokud používáte alternativní přípona UPN v uživatelské jméno, pokus o přihlášení může selhat (musí použít stejný název účtu pro dosažení co nejlepších výsledků). 

Chcete-li vyřešit tyto problémy, je ideální místo spuštění Zkontrolujte protokoly událostí zabezpečení na serveru RADIUS. Pokud chcete uložit, čas vyhledat události, můžete na základě rolí síťové zásady a přístup k serveru vlastní zobrazení v prohlížeči událostí, jak je vidět níže. ID události 6273 označuje událostí, kde serveru Network Policy Server odepřen přístup na uživatele. 

 ![Prohlížeč událostí](./media/nps-extension-vpn/image22.png)
 
## <a name="configure-multi-factor-authentication"></a>Konfigurace služby Multi-Factor Authentication
Tato část obsahuje pokyny pro povolení uživatelů pro MFA a pro nastavení účtů pro dvoustupňové ověření. 

### <a name="enable-multi-factor-authentication"></a>Povolení vícefaktorového ověřování
V této části je povolit účty Azure AD pro MFA. Použití **portálu classic** umožníte uživatelům pro MFA. 

1. Otevřete prohlížeč a přejděte na [https://manage.windowsazure.com](https://manage.windowsazure.com). 
2. Přihlaste se jako správce.
3. Na portálu, v levém navigačním panelu, klikněte na tlačítko **služby ACTIVE DIRECTORY**.

 ![Výchozí adresář](./media/nps-extension-vpn/image23.png)

4. Ve sloupci Název klikněte na tlačítko **výchozí adresář** (nebo jiného adresáře, podle potřeby).
5. Na stránce Rychlý Start klikněte na **konfigurace**.

 ![Konfigurace výchozích](./media/nps-extension-vpn/image24.png)

6. Na stránce konfigurace přejděte dolů a v části služby Multi-Factor authentication klikněte na tlačítko **spravovat nastavení služby**.

 ![Spravovat nastavení vícefaktorového ověřování](./media/nps-extension-vpn/image25.png)
 
7. Na stránce služby Multi-Factor authentication Zkontrolujte výchozí nastavení služby a pak klikněte na tlačítko **uživatelé**. 

 ![Uživatelé vícefaktorového ověřování](./media/nps-extension-vpn/image26.png)
 
8. Na stránce uživatele vyberte uživatele, které chcete povolit pro vícefaktorové ověřování a pak klikněte na tlačítko **povolit**.

 ![Vlastnosti](./media/nps-extension-vpn/image27.png)
 
9. Po zobrazení výzvy klikněte na tlačítko **povolit Multi-Factor auth**.

 ![Povolit MFA](./media/nps-extension-vpn/image28.png)
 
10. Klikněte na **Zavřít**. 
11. Aktualizujte stránku. MFA stav se změní na povoleno.

Informace o tom, jak povolit uživatelům služby Multi-Factor Authentication najdete v tématu [Začínáme s Azure Multi-Factor Authentication v cloudu](multi-factor-authentication-get-started-cloud.md). 

### <a name="configure-accounts-for-two-step-verification"></a>Konfigurace účtů pro dvoustupňové ověření
Jakmile účet povolen pro MFA, nebudou se uživatelé moct přihlásit k prostředkům řídí podle zásad vícefaktorového ověřování, dokud úspěšně nakonfigurovali důvěryhodné zařízení pro druhý ověřovací faktor nutnosti použít dvoustupňové ověřování.

V této části nakonfigurujete důvěryhodné zařízení pro použití s dvoustupňové ověřování. Existuje několik možností můžete nakonfigurovat tyto, včetně následujících:

* **Mobilní aplikace**. Nainstalujte aplikaci Microsoft Authenticator na zařízení Windows Phone, Android nebo iOS. V závislosti na zásadách vaší organizace, je nutné, abyste používali aplikaci v jednom ze dvou režimů: přijímání oznámení pro ověření (oznámení vložena do zařízení) nebo použít ověřovací kód (je nutné zadat ověřovací kód, který aktualizuje každých 30 sekund). 
* **Mobilní telefonní hovor nebo textovou**. Můžete buď zobrazí automatizovaného telefonního hovoru nebo textové zprávy. S parametrem telefonní hovor odpovíte na volání a stiskněte klávesu znak # k ověření. S parametrem text může odpovědět na textovou zprávu nebo zadejte ověřovací kód do rozhraní přihlášení.
* **Office telefonní hovor**. Tento proces je stejný jako příkaz, který popisuje pro automatické telefonické hovory výše.

Postupujte podle těchto pokynů pro nastavení zařízení pro mobilní aplikace pro příjem nabízených oznámení pro ověření.

1. Přihlaste se k [https://aka.ms/mfasetup](https://aka.ms/mfasetup) nebo všechny lokality, jako například [https://portal.azure.com](https://portal.azure.com), kde budete muset ověřit pomocí svých přihlašovacích údajů povolit MFA. 
2. Po přihlášení pomocí uživatelského jména a hesla, zobrazí se obrazovka, která vás vyzve k nastavení účtu pro další ověření zabezpečení.

 ![Další zabezpečení](./media/nps-extension-vpn/image29.png)

3. Klikněte na tlačítko **nyní nastavit**.
4. Na stránce další bezpečnostní ověření vyberte typ kontaktu (telefon pro ověření, telefonní číslo do kanceláře nebo mobilní aplikace). Poté vyberte zemi nebo oblast a vyberte metodu. Metoda se liší podle kontaktní typ, který jste vybrali. Například pokud se rozhodnete mobilní aplikace, můžete vybrat zda chcete dostávat oznámení pro ověření nebo použít ověřovací kód. Kroky, které následují předpokládá můžete zvolit **mobilní aplikace** jako typ kontaktu.

 ![Ověření telefonu](./media/nps-extension-vpn/image30.png)

5. Vyberte mobilní aplikace, klikněte na **dostávat oznámení pro ověření**a potom **nastavit**. 

 ![Ověření mobilní aplikace](./media/nps-extension-vpn/image31.png)
 
6. Pokud jste tak ještě neučinili, nainstalujte ověřovací mobilní aplikaci na vašem zařízení. 
7. Postupujte podle pokynů v mobilní aplikaci kontrolovat vidění čárový kód nebo informace zadat ručně a potom klikněte na **provádí**.

 ![Konfigurace mobilních aplikací](./media/nps-extension-vpn/image32.png)

8. Na stránce další bezpečnostní ověření klikněte na **kontaktujte mi** a odpovědět na oznámení na vaše zařízení.
9. Na stránce další bezpečnostní ověření, zadejte číslo kontaktu v případě, že jste ztratili přístup k mobilní aplikaci a klikněte na **Další**.

 ![Číslo mobilního telefonu](./media/nps-extension-vpn/image33.png)
 
10. Na dalšího ověření zabezpečení, klikněte na tlačítko **provádí**.

Zařízení je nyní nakonfigurováno pro poskytování druhé metody ověřování. Informace o nastavení účtů pro dvoustupňové ověření najdete v tématu [nastavit účtu pro dvoustupňové ověření](./end-user/multi-factor-authentication-end-user-first-time.md).

## <a name="install-and-configure-nps-extension"></a>Instalace a konfigurace serveru NPS rozšíření

Tato část obsahuje pokyny ke konfiguraci sítě VPN pro použití Azure MFA pro ověření klienta se serverem VPN.

Po instalaci a konfiguraci serveru NPS rozšíření, všechny ověřování klienta na základě protokolu RADIUS, které zpracovává tento server je potřeba použít Azure MFA. Není-li všichni uživatelé sítě VPN jsou zaregistrované v Azure MFA, můžete nastavit jiný server RADIUS pro ověřování uživatelů, kteří nejsou nakonfigurovaná pro použití vícefaktorového ověřování. Nebo můžete vytvořit položku registru, který umožňuje němuž uživatelům poskytnout druhý ověřovací faktor, pouze v případě, že jsou zaregistrovaná ve službě vícefaktorového ověřování. 

Vytvořte novou řetězcovou hodnotu s názvem _REQUIRE_USER_MATCH v HKLM\SOFTWARE\Microsoft\AzureMfa_a nastavte hodnotu na hodnotu TRUE nebo FALSE. 

 ![Vyžadovat porovnání uživatele](./media/nps-extension-vpn/image34.png)
 
Pokud je hodnota nastavena na hodnotu TRUE nebo není nastavena, všechny požadavky na ověření se vztahují ověřovací test MFA. Pokud je hodnota nastavena na hodnotu FALSE, vícefaktorového ověřování výzvy vystaveny pouze pro uživatele, která jsou registrovaná v MFA. Používejte pouze FALSE nastavení v provozním prostředí nebo při testování během po dobu registrace.

### <a name="acquire-azure-active-directory-guid-id"></a>Získání ID identifikátor GUID služby Azure Active Directory

Jako součást konfigurace serveru NPS rozšíření budete muset zadat přihlašovací údaje správce a Azure Active Directory ID pro vašeho tenanta Azure AD. Následující kroky ukazují, jak získat ID klienta.

1. Přihlaste se k portálu Azure v [https://portal.azure.com](https://portal.azure.com) jako globální správce Azure klienta.
2. V levém navigačním panelu, klikněte **Azure Active Directory** ikonu.
3. Klikněte na **Vlastnosti**.
4. Vaše ID adresáře zkopírovat do schránky, vyberte **kopie** ikonu.
 
 ![ID adresáře](./media/nps-extension-vpn/image35.png)

### <a name="install-the-nps-extension"></a>Nainstalujte rozšíření serveru NPS
Rozšíření serveru NPS musí být nainstalovaný na server, který má síťové zásady a nainstalovanou rolí služby NPS přístup (Server) a která slouží jako server RADIUS při návrhu. Neinstalujte rozšíření serveru NPS na vašem serveru vzdálené plochy.

1. Stáhněte si rozšíření serveru NPS ze [https://aka.ms/npsmfa](https://aka.ms/npsmfa). 
2. Zkopírujte instalační spustitelný soubor (NpsExtnForAzureMfaInstaller.exe) do serveru NPS.
3. Na serveru NPS, dvakrát klikněte na **NpsExtnForAzureMfaInstaller.exe**. Pokud se zobrazí výzva, klikněte na tlačítko **spustit**.
4. V dialogovém okně NPS rozšíření pro Azure MFA nastavení zkontrolovat licenční podmínky pro software, zkontrolujte **souhlasím s licenční podmínky a ujednání**a klikněte na tlačítko **nainstalovat**.

 ![Rozšíření serveru NPS](./media/nps-extension-vpn/image36.png)
 
5. V dialogovém okně NPS rozšíření pro Azure MFA nastavení, klikněte na **Zavřít**.  

 ![Instalační program úspěšně](./media/nps-extension-vpn/image37.png) 
 
### <a name="configure-certificates-for-use-with-the-nps-extension-using-a-powershell-script"></a>Konfigurace certifikátů pro použití s příponou NPS pomocí skriptu prostředí PowerShell
K zajištění zabezpečení komunikace a záruku, musíte nakonfigurovat certifikáty pro použití rozšíření serveru NPS. Součásti serveru NPS zahrnují skript prostředí Windows PowerShell, který konfiguruje certifikát podepsaný svým držitelem pro použití se serverem NPS. 

Skript provede následující akce:

* Vytvoří certifikát podepsaný svým držitelem
* Přidruží veřejný klíč certifikátu instančního objektu v Azure AD
* Ukládá certifikát v úložišti místního počítače
* Uděluje přístup k privátní klíč certifikátu do síťového uživatele
* Restartuje službu serveru zásad sítě

Pokud chcete používat vlastní certifikáty, musíte přidružit veřejný klíč certifikátu do objektu služby v Azure AD a tak dále.
Chcete-li použít skript, poskytnout rozšíření vaše pověření pro správu Azure Active Directory a ID klienta Azure Active Directory, který jste zkopírovali dříve. Spuštění skriptu na každý server NPS, který jste nainstalovali rozšíření serveru NPS.

1. Otevřete řádku prostředí Windows PowerShell pro správu.
2. Zadejte v příkazovém prostředí PowerShell, _cd 'c:\Program Files\Microsoft\AzureMfa\Config'_a stiskněte klávesu **ENTER**.
3. Typ _.\AzureMfsNpsExtnConfigSetup.ps1_a stiskněte klávesu **ENTER**. 
 * Skript zkontroluje, zda je nainstalován modul Azure Active Directory PowerShell. Pokud není nainstalována, skript nainstaluje modul pro vás.
 
 ![PowerShell](./media/nps-extension-vpn/image38.png)
 
4. Po skript ověřuje instalace modulu prostředí PowerShell, se zobrazí dialogové okno modul Azure Active Directory PowerShell. V dialogovém okně zadejte přihlašovací údaje Správce služby Azure AD a heslo a klikněte na tlačítko **přihlášení**. 
 
 ![Přihlášení k prostředí PowerShell](./media/nps-extension-vpn/image39.png)
 
5. Po zobrazení výzvy, vložte ID klienta, který jste zkopírovali dříve do schránky a stiskněte klávesu **ENTER**. 

 ![ID tenanta](./media/nps-extension-vpn/image40.png)

6. Tento skript vytvoří certifikát podepsaný svým držitelem a provede další změny v konfiguraci. Výstup se podobá obrázku vidíte níže.

 ![Vlastní certifikát podepsaný](./media/nps-extension-vpn/image41.png)

7. Restartujte server.
 
### <a name="verify-configuration"></a>Ověření konfigurace
Pokud chcete ověřit konfiguraci, budete muset vytvořit nové připojení VPN se serverem VPN. Po úspěšně zadávat svoje přihlašovací údaje pro primární ověřování, čeká připojení k síti VPN pro sekundární ověření úspěšné před připojení, jak je uvedeno níže. 

 ![Ověření konfigurace](./media/nps-extension-vpn/image42.png)

Pokud jste úspěšně ověřit pomocí metody sekundární ověření, které jste dříve nakonfigurovali v Azure MFA, jste připojení k prostředku. Pokud sekundárního ověřování není úspěšné, ale je odepřen přístup k prostředku. 

V následujícím příkladu se používá ověřovací aplikací na Windows phone pro zajištění sekundární ověřování.

 ![Ověření účtu.](./media/nps-extension-vpn/image43.png)

Jakmile úspěšně jste se ověřili sekundární metodou, je uděleno oprávnění na virtuální port na serveru VPN. Protože však bylo potřeba použít metodu sekundární ověřování pomocí mobilní aplikace na důvěryhodném zařízení, do protokolu v procesu je bezpečnější, než se bude používat jenom uživatelské jméno nebo kombinace hesla.

### <a name="view-event-viewer-logs-for-successful-logon-events"></a>Zobrazit protokoly Prohlížeče událostí pro události úspěšného přihlášení
Zobrazení událostí úspěšného přihlášení v protokolech prohlížeče událostí systému Windows, můžete použít následující příkaz prostředí Windows PowerShell k dotazování protokolu zabezpečení systému Windows na serveru NPS.

Chcete-li prohledávat události úspěšného přihlášení v protokolech prohlížeče událostí zabezpečení, použijte následující příkaz,
* _Get-WinEvent. – název protokolu zabezpečení_ | kde {$_.ID - eq '6272'} | FL 

 ![Prohlížeč událostí zabezpečení](./media/nps-extension-vpn/image44.png)
 
Také můžete zobrazit protokol zabezpečení nebo vlastní zobrazení služby Síťové zásady a přístup, jak je uvedeno níže:

 ![Přístup k síti zásad](./media/nps-extension-vpn/image45.png)

Na serveru, kam jste nainstalovali server NPS rozšíření pro Azure MFA, můžete najít protokoly Prohlížeče událostí aplikace specifické pro rozšíření na **aplikace a služby Logs\Microsoft\AzureMfa**. 

* _Get-WinEvent. – název protokolu zabezpečení_ | kde {$_.ID - eq '6272'} | FL

 ![Počet událostí](./media/nps-extension-vpn/image46.png)

## <a name="troubleshoot-guide"></a>Řešení potíží s Průvodce
Pokud konfigurace nefungují podle očekávání, je vhodné oddělení na zahájení řešení ověřte, že je správce nakonfigurován na použití Azure MFA. Uživatel připojit k [https://portal.azure.com](https://portal.azure.com). Když se zobrazí výzva k sekundárního ověřování a můžete úspěšně ověřit, že můžete eliminovat nesprávnou konfiguraci Azure MFA.

Pokud Azure MFA funguje pro uživatele, přečtěte si relevantní protokoly událostí. Mezi ně patří událostí zabezpečení, brány provozní a Azure MFA protokoly, které jsou popsané v předchozí části. 

Dole je příklad výstupu zobrazuje událost přihlášení se nezdařilo (událost ID 6273) protokolu zabezpečení:

 ![Protokolu zabezpečení](./media/nps-extension-vpn/image47.png)

Zde jsou související události z protokolů AzureMFA:

 ![Protokoly Azure MFA](./media/nps-extension-vpn/image48.png)

K provedení rozšířené možnosti řešení potíží, zkontrolujte soubory protokolů formátu databáze NPS nainstalovanou službu NPS. Tyto soubory protokolu se vytvoří v _%SystemRoot%\System32\Logs_ složky jako soubory text oddělený čárkami. Popis těchto souborů protokolu najdete v tématu [interpretovat NPS formátu soubory protokolů databáze](https://technet.microsoft.com/library/cc771748.aspx). 

Položky v těchto protokolových souborech obtížně interpretovat bez jejich importování do tabulkového procesoru nebo databázi. Můžete získat počet analyzátory služby ověřování v Internetu online vám pomůže při interpretaci souborů protokolu. Níže je výstup jednoho takové ke stažení [shareware aplikace](http://www.deepsoftware.com/iasviewer): 

 ![Shareware aplikace](./media/nps-extension-vpn/image49.png)

Nakonec pro další možnosti řešení potíží, můžete pomocí analyzátoru protokolu například Wireshark nebo [Microsoft Message Analyzer](https://technet.microsoft.com/library/jj649776.aspx). Na následujícím obrázku z Wireshark zobrazí zprávy protokolu RADIUS mezi serverem VPN a serverem NPS.

 ![Microsoft Message Analyzer](./media/nps-extension-vpn/image50.png)

Další informace najdete v tématu [vaší stávající infrastruktury pro server NPS integrovat Azure Multi-Factor Authentication](multi-factor-authentication-nps-extension.md).  

## <a name="next-steps"></a>Další kroky
[Jak získat službu Azure Multi-Factor Authentication](multi-factor-authentication-versions-plans.md)

[Brána vzdálené plochy Azure Multi-Factor Authentication Server pomocí protokolu RADIUS](multi-factor-authentication-get-started-server-rdg.md)

[Integrace místních adresářů se službou Azure Active Directory](../active-directory/connect/active-directory-aadconnect.md)

