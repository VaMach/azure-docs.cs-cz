---
title: "Azure AD Connect: Povolení zpětného zápisu zařízení | Microsoft Docs"
description: "Tento dokument podrobně popisuje postup povolení zpětného zápisu zařízení pomocí služby Azure AD Connect"
services: active-directory
documentationcenter: 
author: billmath
manager: mtillman
editor: curtand
ms.assetid: c0ff679c-7ed5-4d6e-ac6c-b2b6392e7892
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/26/2017
ms.author: billmath
ms.openlocfilehash: 9c0ff3394dac12bdcac9d618832566ef0d3a6609
ms.sourcegitcommit: e266df9f97d04acfc4a843770fadfd8edf4fa2b7
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 12/11/2017
---
# <a name="azure-ad-connect-enabling-device-writeback"></a>Azure AD Connect: Povolení zpětného zápisu zařízení
> [!NOTE]
> Předplatné služby Azure AD Premium je vyžadována pro zpětný zápis zařízení.
> 
> 

Následující dokumentace obsahuje informace o tom, jak povolit funkci zpětného zápisu zařízení v Azure AD Connect. Zpětný zápis zařízení se používá v následujících scénářích:

* Povolení podmíněného přístupu podle zařízení do služby AD FS (2012 R2 nebo vyšší) chráněné aplikace (vztahy důvěryhodnosti předávající strany).

To poskytuje dodatečné zabezpečení a záruku, že je udělen přístup k aplikacím jenom na důvěryhodné zařízení. Další informace o podmíněného přístupu najdete v tématu [řízením rizik pomocí podmíněného přístupu](../active-directory-conditional-access-azure-portal.md) a [nastavení místního podmíněného přístupu pomocí Azure Active Directory Device Registration](../active-directory-conditional-access-automatic-device-registration-setup.md).

> [!IMPORTANT]
> <li>Zařízení se musí nacházet ve stejné doménové struktuře jako uživatelé. Vzhledem k tomu, že zařízení je možné zpětně zapsat do jedné doménové struktury, tato funkce nepodporuje aktuálně nasazení s více doménovými strukturami uživatele.</li>
> <li>Objekt konfigurace pouze jedno zařízení registrace lze přidat do místní doménové struktury služby Active Directory. Tato funkce není kompatibilní s topologii, kde je místní služby Active Directory synchronizovat do více adresářů Azure AD.</li>> 

## <a name="part-1-install-azure-ad-connect"></a>Část 1: Instalace služby Azure AD Connect
1. Nainstalujte Azure AD Connect s použitím vlastní nebo expresní nastavení. Společnost Microsoft doporučuje spustit s všichni uživatelé a skupiny úspěšně synchronizováno před povolením zpětný zápis zařízení.

## <a name="part-2-prepare-active-directory"></a>Část 2: Příprava služby Active Directory
Pomocí následujících kroků připravte pomocí zpětný zápis zařízení.

1. Z počítače, kde je nainstalován Azure AD Connect spusťte prostředí PowerShell v režimu zvýšených oprávnění.
2. Pokud modul Active Directory PowerShell nainstalovaná není, nainstalujte nástrojů vzdálenou správu serveru, který obsahuje modul AD PowerShell a dsacls.exe která je vyžadována pro spuštění skriptu.  Spusťte následující příkaz:
  
   ``` powershell
   Add-WindowsFeature RSAT-AD-Tools
   ```

3. Pokud modul Powershellu pro Azure Active Directory není nainstalován, pak stáhněte a nainstalujte ji z [Azure Active Directory modul pro prostředí Windows PowerShell (64bitová verze)](http://go.microsoft.com/fwlink/p/?linkid=236297). Tato součást obsahuje závislost na Pomocník pro přihlášení, která se instaluje s Azure AD Connect.  
4. Pomocí přihlašovacích údajů správce podniku spusťte následující příkazy a poté ukončete prostředí PowerShell.
   
   ``` powershell
   Import-Module 'C:\Program Files\Microsoft Azure Active Directory Connect\AdPrep\AdSyncPrep.psm1'
   ```

   ``` powershell
   Initialize-ADSyncDeviceWriteback {Optional:–DomainName [name] Optional:-AdConnectorAccount [account]}
   ```

Vzhledem k tomu, že je nutné provést změny do konfigurace oboru názvů, jsou vyžadována pověření správce podniku. Správce domény, nebude mít dostatečná oprávnění.

![Prostředí PowerShell pro povolení zpětného zápisu zařízení](./media/active-directory-aadconnect-feature-device-writeback/powershell.png) d


Popis:

* Pokud by již neexistuje, vytvoří a nakonfiguruje nové kontejnery a objekty v rámci CN = Device Registration Configuration, CN = Services, CN = Configuration, [doménové struktury rozlišující název].
* Pokud by již neexistuje, vytvoří a nakonfiguruje nové kontejnery a objekty v rámci CN = RegisteredDevices, [rozlišující název domény]. Objekty zařízení budou vytvořeny v tomto kontejneru.
* Nastaví potřebná oprávnění pro účet konektoru služby Azure AD, ke správě zařízení ve vaší služby Active Directory.
* Jenom je potřeba spustit na jednu doménovou strukturu, i v případě, že Azure AD Connect instalujete na několik doménových struktur.

Parametry:

* DomainName: Doména služby Active Directory kde bude vytvořen objekty zařízení. Poznámka: Všechna zařízení v dané doménové struktuře služby Active Directory bude vytvořen v jedné doméně.
* AdConnectorAccount: Účet služby Active Directory, který se použije ke správě objektů v adresáři Azure AD Connect. Toto je účet používaný službou Azure AD Connect sync k připojení do služby AD. Pokud jste nainstalovali pomocí expresního nastavení, je to účet MSOL_ s předponou.

## <a name="part-3-enable-device-writeback-in-azure-ad-connect"></a>Část 3: Zpětný zápis zařízení povolit ve službě Azure AD Connect
Použijte následující postup povolení zpětného zápisu zařízení v Azure AD Connect.

1. Spusťte Průvodce instalací znovu. Vyberte **přizpůsobit možnosti synchronizace** z dalších úloh a klikněte na tlačítko **Další**.
   ![Vlastní instalace přizpůsobit možnosti synchronizace](./media/active-directory-aadconnect-feature-device-writeback/devicewriteback2.png)
2. Na stránce volitelné funkce zpětný zápis zařízení už k dispozici. Prosím Všimněte si, že pokud Azure AD Connect přípravný kroky nejsou zpětný zápis dokončené zařízení k dispozici víc systémů na stránce volitelné funkce. Zaškrtněte políčko pro zpětný zápis zařízení a klikněte na tlačítko **Další**. Pokud zaškrtávací políčko je stále zakázaná, najdete v článku [řešení potíží v](#the-writeback-checkbox-is-still-disabled).
   ![Vlastní instalace volitelných funkcí zpětný zápis zařízení.](./media/active-directory-aadconnect-feature-device-writeback/devicewriteback3.png)
3. Na stránce zpětný zápis se zobrazí zadané domény jako doménové struktury výchozí zařízení zpětný zápis.
   ![Vlastní instalace zařízení zpětný zápis cílové doménové struktuře](./media/active-directory-aadconnect-feature-device-writeback/devicewriteback4.png)
4. Dokončete instalaci průvodce žádné další změny konfigurace. V případě potřeby odkazovat na [vlastní instalace Azure AD Connect.](active-directory-aadconnect-get-started-custom.md)

## <a name="enable-conditional-access"></a>Povolení podmíněného přístupu
Podrobné pokyny, které chcete povolit tento scénář jsou k dispozici v rámci [nastavení místního podmíněného přístupu pomocí Azure Active Directory Device Registration](../active-directory-conditional-access-automatic-device-registration-setup.md).

## <a name="verify-devices-are-synchronized-to-active-directory"></a>Ověřte, že zařízení jsou synchronizovány do služby Active Directory
Zpětný zápis zařízení by měl nyní pracuje správně. Upozorňujeme, že může trvat až 3 hodiny pro objekty zařízení být zapsány zpět do služby AD.  Pokud chcete ověřit, zařízení se se správně synchronizovaný, postupujte takto po dokončení synchronizace pravidla:

1. Spusťte Centrum správy služby Active Directory.
2. Rozbalte RegisteredDevices, v rámci domény, který je právě federovaný.
   ![Centrum správy služby Active Directory zaregistrované zařízení](./media/active-directory-aadconnect-feature-device-writeback/devicewriteback5.png)
3. Aktuální registrovaná zařízení se objeví existuje.
   ![Centrum správy služby Active Directory zaregistrován seznam zařízení](./media/active-directory-aadconnect-feature-device-writeback/devicewriteback6.png)

## <a name="troubleshooting"></a>Řešení potíží
### <a name="the-writeback-checkbox-is-still-disabled"></a>Zpětný zápis zaškrtávací políčko je stále zakázáno.
Pokud zaškrtávací políčko pro zpětný zápis zařízení není povolena, i když jste postupovali podle kroků výše, následující postup vás provede co Průvodce instalací ověřuje předtím, než je povoleno pole.

Nejdůležitější první:

* Ujistěte se, že má aspoň jednu doménovou strukturu Windows Server 2012 R2. Typ objektu zařízení musí být přítomen.
* Pokud Průvodce instalací je již spuštěna, nebudou zjištěna žádné změny. V takovém případě dokončit Průvodce instalací a potom ho spusťte znovu.
* Ujistěte se, že účet, který zadáte ve skriptu inicializace je ve skutečnosti správné uživatelské používá konektor služby Active Directory. Chcete-li to ověřit, postupujte takto:
  * Z nabídky start otevřít **synchronizační služba**.
  * Otevřete **konektory** kartě.
  * Najít konektor s typem Active Directory Domain Services a vyberte jej.
  * V části **akce**, vyberte **vlastnosti**.
  * Přejděte na **připojit k doménové struktuře služby Active Directory**. Zkontrolujte, zda doména a uživatelské jméno zadali na této obrazovce shodu zadaný účet do skriptu.
    ![Účet konektoru ve Správci služby synchronizace](./media/active-directory-aadconnect-feature-device-writeback/connectoraccount.png)

Ověření konfigurace ve službě Active Directory:

* Ověřte, že službu Device Registration Service nachází v umístění níže (CN DeviceRegistrationService, CN = = služby registrace zařízení, CN = Device Registration Configuration, CN = Services, CN = Configuration) v názvovém kontextu konfigurace.

![Řešení potíží s DeviceRegistrationService v oboru názvů configuration](./media/active-directory-aadconnect-feature-device-writeback/troubleshoot1.png)

* Ověřte, že existuje jenom jeden objekt konfigurace tak, že konfigurace oboru názvů. Pokud je více než jedna, odstraňte duplicitní.

![Řešení potíží, vyhledejte je duplicitní](./media/active-directory-aadconnect-feature-device-writeback/troubleshoot2.png)

* Na objekt Device Registration Service Přesvědčte se, zda atribut msDS-DeviceLocation je k dispozici a má hodnotu. Vyhledávání toto umístění a ujistěte se, že je k dispozici s objectType msDS-DeviceContainer.

![Řešení potíží s msDS-DeviceLocation](./media/active-directory-aadconnect-feature-device-writeback/troubleshoot3.png)

![Řešení potíží s RegisteredDevices objektu třídy](./media/active-directory-aadconnect-feature-device-writeback/troubleshoot4.png)

* Ověřte, zda má účet používaný službou konektoru služby Active Directory má požadovaná oprávnění na registrovaná zařízení kontejneru nalezena v předchozím kroku. Toto je očekávané oprávnění pro tento kontejner:

![Řešení potíží, ověřte oprávnění u kontejneru](./media/active-directory-aadconnect-feature-device-writeback/troubleshoot5.png)

* Ověřte účet služby Active Directory má oprávnění pro CN = Device Registration Configuration, CN = Services, CN = objekt konfigurace.

![Řešení potíží, ověřte oprávnění u konfigurace registrace zařízení](./media/active-directory-aadconnect-feature-device-writeback/troubleshoot6.png)

## <a name="additional-information"></a>Další informace
* [Řízení rizik pomocí podmíněného přístupu](../active-directory-conditional-access-azure-portal.md)
* [Nastavení místního podmíněného přístupu pomocí Azure Active Directory Device Registration](../active-directory-device-registration-on-premises-setup.md)

## <a name="next-steps"></a>Další kroky
Přečtěte si další informace o [Integrování místních identit do služby Azure Active Directory](active-directory-aadconnect.md).

