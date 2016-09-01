<properties
    pageTitle="Povolení proxy aplikace u služby Azure AD | Microsoft Azure"
    description="Zapněte proxy aplikace v portálu Azure Classic a nainstalujte konektory pro reverzní proxy."
    services="active-directory"
    documentationCenter=""
    authors="kgremban"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="07/19/2016"
    ms.author="kgremban"/>

# Povolení proxy aplikace v portálu Azure

Tento článek vás provede postupem, který umožňuje povolit proxy aplikace u cloudového adresáře služby Microsoft Azure AD.

Pokud netušíte, čím vám může proxy aplikace prospět, přečtěte si článek o tom, [jak poskytnout zabezpečený vzdálený přístup k místním aplikacím](active-directory-application-proxy-get-started.md).

## Požadavky na proxy aplikace
Předtím, než budete moct povolit a používat služby proxy aplikace, musíte mít:

- [Základní nebo prémiové předplatné služby Microsoft Azure AD](active-directory-editions.md) a adresář služby Azure AD, u kterého jste globální správce.
- Server se systémem Windows Server 2012 R2 nebo Windows 8.1 či novější, na který nainstalujete konektor proxy aplikace. Server odesílá požadavky službám proxy aplikací v cloudu pomocí protokolu HTTPS a musí mít připojení protokolu HTTP nebo HTTPS k aplikacím, které publikujete.

    - Pokud chcete v publikovaných aplikacích používat jednotné přihlašování, měl by být tento počítač připojený ke stejné doméně služby AD jako aplikace, které publikujete.

- Pokud stojí v cestě brána firewall, zajistěte, aby byla otevřená a konektor mohl proxy aplikace posílat požadavky prostřednictvím protokolu HTTPS (TCP). Konektor používá následující porty společně se subdoménami, které jsou součástí domén nejvyšších úrovní: msappproxy.net a servicebus.windows.net. Zajistěte, aby byly otevřené všechny následující porty pro **odchozí** přenos:

  	| Číslo portu | Popis |
  	| --- | --- |
  	| 80 | Umožňuje odchozí přenosy pomocí protokolu HTTP pro bezpečnostní ověření. |
  	| 443 | Umožňuje ověření uživatele ve službě Azure AD (potřeba jenom při registračním procesu konektoru). |
  	| 10100–10120 | Umožňuje odesílání odpovědí LOB HTTP zpátky na proxy. |
  	| 9352, 5671 | Umožňuje komunikaci mezi konektorem a službou Azure v rámci příchozích požadavků. |
  	| 9350 | Volitelné, umožňuje lepší příjem příchozích požadavků |
  	| 8080 | Umožňuje sekvenci bootstrap konektoru a automatickou aktualizaci konektoru. |
  	| 9090 | Umožňuje registraci konektoru (potřeba jenom při registračním procesu konektoru). |
  	| 9091 | Umožňuje automatické obnovení certifikátu důvěryhodnosti konektoru. |

    Pokud brána firewall vynucuje přenos v závislosti na zdroji uživatelů, otevřete tyto porty pro přenos ze služeb systému Windows, které běží jako síťové služby. Zajistěte také, aby byl port číslo 8080 povolen pro NT Authority\System.

- Pokud se vaše organizace připojuje k internetu pomocí proxy serverů, podívejte se na příspěvek na blogu [Working with existing on-premises proxy servers](https://blogs.technet.microsoft.com/applicationproxyblog/2016/03/07/working-with-existing-on-prem-proxy-servers-configuration-considerations-for-your-connectors/) (Práce s existujícími místními proxy servery) a přečtěte si podrobnosti o jejich konfiguraci.

## Krok 1: Povolení proxy aplikace ve službě Azure AD
1. Přihlaste se jako správce do [portálu Azure Classic](https://manage.windowsazure.com/).
2. Přejděte do služby Active Directory a vyberte adresář, u kterého chcete povolit proxy aplikace.

    ![Active Directory – ikona](./media/active-directory-application-proxy-enable/ad_icon.png)

3. Na stránce adresáře vyberte **Konfigurovat** a přejděte dolů k **Proxy aplikace**.
4. Přepněte možnost **Povolit pro tento adresář služby proxy aplikace** na **Povoleno**.

    ![Povolení proxy aplikace](./media/active-directory-application-proxy-enable/app_proxy_enable.png)

5. Vyberte **Stáhnout**. Tím přejdete na stránku pro **stažení konektoru proxy aplikace služby Azure AD**. Přečtěte si a přijměte licenční podmínky, klikněte na **Stáhnout** a uložte soubor Instalační služby Windows (.exe) konektoru.

## Krok 2: Instalace a registrace konektoru
1. Spusťte aplikaci **AADApplicationProxyConnectorInstaller.exe** na serveru, který jste připravili podle uvedených požadavků.
2. Nainstalujte podle pokynů instalačního průvodce.
3. Během instalace se zobrazí výzva k registraci konektoru v proxy aplikace vašeho tenanta služby Azure AD.

  - Zadejte vaše přihlašovací údaje globálního správce pro službu Azure AD. Klient globálního správce se může lišit od vašich přihlašovacích údajů ke službě Microsoft Azure.
  - Ujistěte se, že správce, který zaregistruje konektor, je ve stejném adresáři, ve kterým jste povolili službu proxy aplikací. Pokud je třeba doména tenanta contoso.com, správce by měl být admin@contoso.com nebo jakýkoli jiný alias v této doméně.
  - Pokud je **Konfigurace rozšířeného zabezpečení aplikace Internet Explorer** na serveru, kam instalujete konektor, nastavená na **Zapnuto**, může být registrační obrazovka zablokovaná. V tom případě postupujte podle pokynů v chybové zprávě, které vás dovedou k povolení přístupu. Ujistěte se, že je rozšířené zabezpečení aplikace Internet Explorer vypnuto.
  - Pokud registrace konektoru selže, podívejte se do článku [Poradce při potížích s proxy aplikace](active-directory-application-proxy-troubleshoot.md).  

4. Po dokončení instalace se k serveru přidají dvě nové služby:

    - **Microsoft AAD Application Proxy Connector** umožňuje propojení
    - **Microsoft AAD Application Proxy Connector Updater** je služba pro automatickou aktualizaci, která pravidelně kontroluje nové verze konektoru a v případě potřeby ho aktualizuje.

    ![Služby konektoru proxy aplikace – snímek obrazovky](./media/active-directory-application-proxy-enable/app_proxy_services.png)

5. V okně instalace klikněte na **Dokončit**.

V zájmu vyšší dostupnosti byste měli nasadit aspoň dva konektory. Pokud chcete nasadit víc konektorů, opakujte výše popsané kroky 2 a 3. Každý konektor musí být zaregistrovaný samostatně.

Pokud chcete konektor odinstalovat, odinstalujte službu Connector i službu Updater. Restartujte počítač, aby se službě úplně odebrala.


## Další kroky

Nyní jste připraveni k [publikování aplikací pomocí proxy aplikace](active-directory-application-proxy-publish.md).

Pokud máte aplikace, které jsou v různých sítích nebo na různých místech, můžete pomocí skupin konektorů uspořádat různé konektory do logických jednotek. Další informace získáte v článku [Práce s konektory proxy aplikací](active-directory-application-proxy-connectors.md).



<!---HONumber=Aug16_HO4-->


