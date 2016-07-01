<properties
    pageTitle="Povolení proxy aplikace u služby Azure AD | Microsoft Azure"
    description="Zapněte proxy aplikace v portálu Azure Classic a nainstalujte konektory pro reverzní proxy."
    services="active-directory"
    documentationCenter=""
    authors="kgremban"
    manager="StevenPo"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="06/01/2016"
    ms.author="kgremban"/>

# Povolení proxy aplikace v portálu Azure

Tento článek vám ukáže, jak povolit proxy aplikace u cloudového adresáře služby Microsoft Azure AD. To zahrnuje instalaci konektoru proxy aplikace, který udržuje propojení vaší sítě se službou proxy, ve vaší privátní síti. Konektor si také zaregistrujete k předplatnému klienta služby Microsoft Azure AD. Pokud netušíte, čím vám může proxy aplikace prospět, přečtěte si článek o tom, [jak poskytnout zabezpečený vzdálený přístup k místním aplikacím](active-directory-application-proxy-get-started.md).

Po dokončení tohoto návodu budete mít povolenou proxy aplikace služby Azure AD a budete moci publikovat své místní aplikace pro vzdálený přístup.

> [AZURE.NOTE] Proxy aplikace je funkce, která je dostupná jenom v případě, pokud jste upgradovali na edici Premium nebo Basic služby Azure Active Directory. Další informace najdete v článku [Edice služby Azure Active Directory](active-directory-editions.md).

## Požadavky na proxy aplikace
Předtím, než budete moct povolit a používat služby proxy aplikace, musíte mít:

- [Základní nebo prémiové předplatné služby Microsoft Azure AD](active-directory-editions.md) a adresář služby Azure AD, u kterého jste globální správce.
- Server se systémem Windows Server 2012 R2 nebo Windows 8.1 či novější, na který nainstalujete konektor proxy aplikace. Server bude odesílat požadavky službám proxy aplikace na cloudu pomocí protokolu HTTPS a musí mít připojení protokolu HTTPS k aplikacím, které chcete publikovat.
- Pokud používáte bránu firewall, zajistěte, aby byla otevřená a konektor mohl proxy aplikace posílat požadavky prostřednictvím protokolu HTTPS (TCP). Konektor používá následující porty společně se subdoménami, které jsou součástí domén nejvyšších úrovní: *msappproxy.net* a *servicebus.windows.net*. Zajistěte, aby byly otevřeny **všechny** následující porty pro **odchozí** přenos:

  	| Číslo portu | Popis |
  	| --- | --- |
  	| 80 | Slouží k povolení odchozího přenosu protokolu HTTP pro bezpečnostní ověření. |
  	| 443 | Slouží k povolení ověřování uživatele pomocí služby Azure AD (vyžadováno pouze u registračního procesu konektoru) |
  	| 10100 - 10120 | Slouží k povolení odesílání odpovědí LOB HTTP zpět na proxy |
  	| 9352, 5671 | Slouží k povolení komunikace mezi konektorem a službou Azure v rámci příchozích požadavků |
  	| 9350 | Volitelné, umožňuje lepší příjem příchozích požadavků |
  	| 8080 | Slouží k povolení sekvence bootstrap konektoru a automatické aktualizaci konektoru |
  	| 9090 | Slouží k povolení registrace konektoru (vyžadováno pouze u registračního procesu konektoru) |
  	| 9091 | Slouží k povolení automatického obnovení certifikátu důvěryhodnosti konektoru |

Pokud brána firewall vynucuje přenos v závislosti na zdroji uživatelů, otevřete tyto porty pro přenos ze služeb systému Windows, které běží jako síťové služby. Zajistěte také, aby byl port číslo 8080 povolen pro NT Authority\System.


## Krok 1: Povolení proxy aplikace ve službě Azure AD
1. Přihlaste se jako správce do [portálu Azure Classic](https://manage.windowsazure.com/).
2. Přejděte do služby Active Directory a vyberte adresář, u kterého chcete povolit proxy aplikace.

    ![Active Directory – ikona](./media/active-directory-application-proxy-enable/ad_icon.png)

3. Na stránce adresáře vyberte **Konfigurovat** a přejděte dolů k **Proxy aplikace**.
4. Přepněte možnost **Povolit pro tento adresář služby proxy aplikace** na **Povoleno**.

    ![Povolení proxy aplikace](./media/active-directory-application-proxy-enable/app_proxy_enable.png)

5. Vyberte **Stáhnout**. Tím přejdete ke **Stažení konektoru proxy aplikace služby Azure AD**. Přečtěte si a přijměte licenční podmínky, klikněte na **Stáhnout** a uložte instalační soubor (.exe) konektoru proxy aplikace.

## Krok 2: Instalace a registrace konektoru
1. Spusťte aplikaci *AADApplicationProxyConnectorInstaller.exe* na serveru, který jste připravili v souladu s výše uvedenými požadavky.
2. Nainstalujte podle pokynů instalačního průvodce.
3. Během instalace budete vyzváni k registraci konektoru k proxy aplikace vašeho klienta služby Azure AD.

  - Zadejte vaše přihlašovací údaje globálního správce pro službu Azure AD. Klient globálního správce se může lišit od vašich přihlašovacích údajů ke službě Microsoft Azure.
  - Ujistěte se, že je správce, který registruje konektor, ve stejném adresáři, kde jste povolili službu proxy aplikace: Pokud je například doména klienta „contoso.com“, měl by být správce „admin@contoso.com“, případně jiný alias v této doméně.
  - Pokud je **Konfigurace rozšířeného zabezpečení aplikace Internet Explorer** na serveru, kam instalujete konektor služby Azure AD, nastavena na **Zapnuto**, může být registrační obrazovka zablokovaná. V takovém případě postupujte podle pokynů v chybové zprávě, které vás dovedou k povolení přístupu. Ujistěte se, že je rozšířené zabezpečení aplikace Internet Explorer vypnuto.
  - Pokud registrace konektoru selže, podívejte se do článku [Poradce při potížích s proxy aplikace](active-directory-application-proxy-troubleshoot.md).  

4. Po dokončení instalace se k serveru přidají dvě nové služby, jak můžete vidět níže.

    - **Microsoft AAD Application Proxy Connector** umožňuje propojení
    - **Microsoft AAD Application Proxy Connector Updater** je služba pro automatickou aktualizaci, která pravidelně kontroluje nové verze konektoru a v případě potřeby ho aktualizuje.

    ![Služby konektoru proxy aplikace – snímek obrazovky](./media/active-directory-application-proxy-enable/app_proxy_services.png)

5. V okně instalace klikněte na tlačítko **Dokončit**, a instalace se dokončí.

Nyní jste připraveni k [publikování aplikací pomocí proxy aplikace](active-directory-application-proxy-publish.md).

V zájmu vyšší dostupnosti byste měli nasadit alespoň jeden další konektor. Pokud chcete nasadit více konektorů, opakujte výše popsané kroky 2 a 3. Každý konektor musí být zaregistrován samostatně.

Pokud chcete odinstalovat konektor, odinstalujte službu konektoru i aktualizační službu a restartujte počítač, aby se služba úplně odebrala.


## Další kroky

- [Publikování aplikací pomocí proxy aplikace](active-directory-application-proxy-publish.md)
- [Publikování aplikací s použitím vlastního názvu domény](active-directory-application-proxy-custom-domains.md)
- [Povolení jednoduchého přihlášení](active-directory-application-proxy-sso-using-kcd.md)
- [Řešení potíží s proxy aplikace](active-directory-application-proxy-troubleshoot.md)

Nejnovější novinky a aktualizace naleznete na [blogu proxy aplikace](http://blogs.technet.com/b/applicationproxyblog/)



<!--HONumber=Jun16_HO2-->


