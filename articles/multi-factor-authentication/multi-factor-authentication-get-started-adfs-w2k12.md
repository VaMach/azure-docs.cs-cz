---
title: MFA Server s AD FS ve Windows Serveru 2012 R2 | Microsoft Docs
description: Tento článek popisuje, jak začít se službami Azure Multi-Factor Authentication a AD FS v systému Windows Server 2012 R2.
services: multi-factor-authentication
documentationcenter: ''
author: kgremban
manager: femila
editor: curtland

ms.service: multi-factor-authentication
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 09/22/2016
ms.author: kgremban

---
# Zabezpečení cloudových a místních prostředků pomocí Azure Multi-Factor Authentication Serveru s AD FS ve Windows Serveru 2012 R2
Pokud v organizaci používáte službu AD FS (Active Directory Federation Services) a chcete zabezpečit cloudové či místní prostředky, můžete nasadit a nakonfigurovat Azure Multi-Factor Authentication Server pro práci se službou AD FS. Tato konfigurace aktivuje u citlivých koncových bodů vícefaktorové ověřování.

V tomto článku se podíváme na to, jak používat Azure Multi-Factor Authentication Server s AD FS v systém Windows Server 2012 R2. Další informace najdete v tématu [Zabezpečení cloudových a lokálních prostředků pomocí Microsoft Azure Multi-Factor Authentication Serveru s AD FS 2.0](multi-factor-authentication-get-started-adfs-adfs2.md).

## Zabezpečení Windows Serveru 2012 R2 AD FS pomocí Azure Multi-Factor Authentication Serveru
Azure Multi-Factor Authentication Server můžete instalovat různě:

* Nainstalujte Azure Multi-Factor Authentication Server lokálně na stejném serveru jako AD FS.
* Nainstalujte adaptér Azure Multi-Factor Authentication místně na server AD FS a Multi-Factor Authentication Server potom nainstalujte na jiný počítač.

Před zahájením se ujistěte, že znáte následující informace:

* Azure Multi-Factor Authentication Server není potřeba instalovat na váš server AD FS. Na Windows Server 2012 R2, kde běží služba AD FS, je pak ale nutné nainstalovat adaptér Multi-Factor Authentication pro AD FS. Server můžete nainstalovat na jiný počítač, pokud se jedná o podporovanou verzi, a adaptér AD FS potom nainstalujete samostatně na federační server AD FS. Samostatně adaptér instalujte podle následujících postupů.
* Když se adaptér AD FS pro Multi-Factor Authentication Server navrhoval, předpokládalo se, že AD FS může adaptéru předat název přijímající strany, který by se pak použil jako název aplikace. Ukázalo se ale, že to tak není. Pokud v organizaci používáte metody ověření pomocí textové zprávy nebo mobilní aplikace, řetězce definované v Nastavení společnosti obsahují zástupný text „<$*application_name*$>“. Ten se při použití adaptéru AD FS nenahradí automaticky. Doporučujeme, abyste při zabezpečování AD FS zástupný text z příslušných řetězců odebrali.
* Účet, se kterým se přihlašujete, musí mít uživatelská práva k vytváření skupin zabezpečení ve službě Active Directory.
* Průvodce instalací adaptéru Multi-Factor Authentication AD FS vytvoří v instanci Active Directory skupinu zabezpečení s názvem PhoneFactor Admins a přidá do ní účet AD FS vaší federační služby. Doporučujeme, abyste v řadiči domény ověřili, že se skupina PhoneFactor Admins opravdu vytvořila a mezi jejími členy je účet služby AD FS. Pokud je to nutné, přidejte v řadiči domény účet služby AD FS do skupiny PhoneFactor Admins ručně.
* Informace o tom, jak instalovat sadu SDK webové služby pomocí uživatelského portálu, najdete v tématu [Nasazení uživatelského portálu pro Azure Multi-Factor Authentication Server.](multi-factor-authentication-get-started-portal.md)

### Instalace Azure Multi-Factor Authentication Server místně na serveru AD FS
1. Stáhněte a nainstalujte Azure Multi-Factor Authentication Server na federační server AD FS. Informace o instalaci se dočtete v tématu [Začínáme s Azure Multi-Factor Authentication Serverem](multi-factor-authentication-get-started-server.md).
2. V konzole pro správu Azure Multi-Factor Authentication Serveru klikněte na ikonu **služby AD FS** a potom vyberte možnosti **Povolit zápis uživatele** a **Povolit uživatelům výběr metody**.
3. Vyberte další možnosti, které byste chtěli pro svou organizaci nastavit.
4. Klikněte na **Instalovat adaptér služby AD FS**.
   <center>![Cloud](./media/multi-factor-authentication-get-started-adfs-w2k12/server.png)</center>
5. Pokud je počítač připojený k doméně, ale konfigurace Active Directory pro zabezpečení komunikace mezi adaptérem služby AD FS a ověřovací službou Multi-Factor Authentication není kompletní, zobrazí se okno**Active Directory**. Kliknutím na možnost **Další** konfiguraci automaticky dokončíte. Nebo můžete zaškrtnout políčko **Vynechat automatickou konfiguraci služby Active Directory a konfigurovat nastavení ručně** a až potom kliknout na **Další**.
6. Pokud počítač není připojený k doméně a konfigurace místní skupiny pro zabezpečení komunikace mezi adaptérem služby AD FS a ověřovací službou Multi-Factor Authentication není kompletní, zobrazí se okno**Místní skupina**. Kliknutím na možnost **Další** konfiguraci automaticky dokončíte. Nebo můžete zaškrtnout políčko **Vynechat automatickou konfiguraci místní skupiny a konfigurovat nastavení ručně** a až potom kliknout na **Další**.
7. V průvodci instalací klikněte na tlačítko **Další**. Azure Multi-Factor Authentication Server vytvoří skupinu PhoneFactor Admins a přidá do ní účet služby AD FS.
   <center>![Cloud](./media/multi-factor-authentication-get-started-adfs-w2k12/adapter.png)</center>
8. V kroku **Spustit instalační program** klikněte na **Další**.
9. V instalačním programu adaptéru Multi-Factor Authentication AD FS klikněte na **Další**.
10. Po dokončení instalace klikněte na **Zavřít**.
11. Nainstalovaný adaptér je třeba zaregistrovat ve službě AD FS. Otevřete Windows PowerShell a spusťte následující příkaz:<br>
    `C:\Program Files\Multi-Factor Authentication Server\Register-MultiFactorAuthenticationAdfsAdapter.ps1`
    <center>![Cloud](./media/multi-factor-authentication-get-started-adfs-w2k12/pshell.png)</center>
12. Upravte globální zásady ověřování ve službě AD FS tak, abyste mohli nově zaregistrovaný adaptér použít. V konzole pro správu služby AD FS přejděte k uzlu **Zásady ověřování**. V oddílu **Multi-Factor Authentication** klikněte na odkaz **Upravit** vedle oddílu **Globální nastavení**. V okně **Upravit globální zásady ověření** vyberte jako další metodu ověření **Multi-Factor Authentication** a klikněte na **OK**. Adaptér je zaregistrovaný jako WindowsAzureMultiFactorAuthentication. Aby se registrace projevila, je potřeba restartovat službu AD FS.

<center>![Cloud](./media/multi-factor-authentication-get-started-adfs-w2k12/global.png)</center>

Multi-Factor Authentication Server máte teď nastavený jako dodatečného poskytovatele ověření vedle služby AD FS.

## Samostatná instalace adaptéru AD FS pomocí sady SDK webové služby
1. Nainstalujte sadu SDK webové služby na server, kde běží Multi-Factor Authentication Server.
2. Z adresáře \Program Files\Multi-Factor Authentication Server zkopírujte na server, kam plánujete nainstalovat adaptér služby AD FS, tyto soubory:
   * MultiFactorAuthenticationAdfsAdapterSetup64.msi
   * Register-MultiFactorAuthenticationAdfsAdapter.ps1
   * Unregister-MultiFactorAuthenticationAdfsAdapter.ps1
   * MultiFactorAuthenticationAdfsAdapter.config
3. Spusťte instalační soubor MultiFactorAuthenticationAdfsAdapterSetup64.msi.
4. V instalačním programu adaptéru Multi-Factor Authentication AD FS spusťte instalaci kliknutím na tlačítko **Další**.
5. Po dokončení instalace klikněte na **Zavřít**.

## Úprava souboru MultiFactorAuthenticationAdfsAdapter.config
Postupujte podle těchto kroků a upravte soubor MultiFactorAuthenticationAdfsAdapter.config:

1. Uzel **UseWebServiceSdk** nastavte na **true**.  
2. Hodnotu **WebServiceSdkUrl** nastavte na URL sady SDK webové služby pro Multi-Factor Authentication. Například: **https://contoso.com/&lt;název_certifikátu&gt;/MultiFactorAuthWebServicesSdk/PfWsSdk.asmx**, kde název_certifikátu je název vašeho certifikátu.  
3. Upravte skript Register-MultiFactorAuthenticationAdfsAdapter.ps1 tak, že na konec příkazu `Register-AdfsAuthenticationProvider` přidáte *-ConfigurationFilePath &lt;cesta&gt;*, kde *&lt;cesta&gt;* je úplná cesta k souboru MultiFactorAuthenticationAdfsAdapter.config.

### Konfigurace sady SDK webové služby pomocí uživatelského jména a hesla
Por konfiguraci sady SDK webové služby existují dvě možnosti. První možností je použití uživatelského jména a hesla, druhou je použití klientského certifikátu. Pro první možnost postupujte podle těchto kroků, nebo je přeskočte, pokud chcete použít druhou možnost.  

1. Hodnotu **WebServiceSdkUsername** nastavte na účet, který je členem skupiny zabezpečení PhoneFactor Admins. Zadávejte ve formátu &lt;doména&gt;&#92;&lt;uživatelské jméno&gt;.  
2. Hodnotu **WebServiceSdkPassword** nastavte na odpovídající heslo k tomuto účtu.

### Konfigurace sady SDK webové služby pomocí klientského certifikátu
Pokud nechcete použít uživatelské jméno a heslo, postupujte podle těchto kroků a nakonfigurujte sadu SDK webové služby pomocí klientského certifikátu.

1. Od certifikační autority získejte klientský certifikát pro server, na kterém běží sada SDK webové služby. Přečtěte si, jak [získat klientský certifikát](https://technet.microsoft.com/library/cc770328.aspx).  
2. Importujte klientský certifikát do osobního úložiště certifikátů na místním serveru, kde běží sada SDK webové služby. Poznámka: Ujistěte se, že se veřejný certifikát této autority nachází v úložišti certifikátů Důvěryhodné kořenové certifikáty.  
3. Exportujte veřejný a privátní klíč klientského certifikátu do souboru .pfx.  
4. Exportujte veřejný klíč ve formátu Base64 do souboru .cer.  
5. Ve Správci serveru zkontrolujte, že je nainstalovaná funkce Web Server (IIS)\Web Server\Security\IIS Client Certificate Mapping Authentication. Pokud nainstalovaná není, přidejte ji kliknutím na **Přidat role a funkce**.  
6. Ve Správci IIS na webové stránce, která obsahuje virtuální adresář sady SDK webové služby, poklikejte na **Editor konfigurace**. Poznámka: Dejte pozor, abyste tenhle krok provedli na úrovni webové stránky, ne na úrovni virtuálního adresáře.  
7. Přejděte do oddílu **system.webServer/security/authentication/iisClientCertificateMappingAuthentication**.  
8. Hodnotu **enabled** nastavte na **true**.  
9. Hodnotu **oneToOneCertificateMappingsEnabled** nastavte také na **true**.  
10. Klikněte na tlačítko **...** vedle položky **oneToOneMappings** a potom klikněte na odkaz **Přidat**.  
11. Otevřete soubor .cer s klíčem ve formátu Base64, který jste exportovali. Odeberte řetězce *-----BEGIN CERTIFICATE-----* a *-----END CERTIFICATE-----* a veškerá zalomení řádků. Výsledný řetězec zkopírujte.  
12. Hodnotu **certificate** nastavte na řetězec zkopírovaný v předchozím kroku.  
13. Hodnotu **enabled** nastavte na **true**.  
14. Nastavte **userName** na účet, který je členem skupiny zabezpečení PhoneFactor Admins. Zadávejte ve formátu &lt;doména&gt;&#92;&lt;uživatelské jméno&gt;.  
15. Heslo nastavte na odpovídající heslo k tomuto účtu a pak Editor konfigurace zavřete.  
16. Klikněte na odkaz **Použít**.  
17. Ve virtuálním adresáři sady SDK webové služby poklikejte na možnost **Ověřování**.  
18. Zkontrolujte, že jsou možnosti **Zosobnění technologie ASP.NET** a **Základní ověřování** **zapnuté** a všechny ostatní položky **vypnuté**.  
19. Ve virtuálním adresáři sady SDK webové služby poklikejte na **Nastavení SSL**.  
20. Nastavte **Klientské certifikáty** na **Přijmout** a klikněte na **Použít**.  
21. Soubor .pfx, který jste exportovali, zkopírujte na server, kde běží adaptér AD FS.  
22. Importujte soubor .pfx do osobního úložiště certifikátů na místním počítači.  
23. Klikněte pravým tlačítkem, vyberte možnost **Spravovat soukromé klíče** a účtu, který jste použili pro přihlášení ke službě AD FS, udělte oprávnění ke čtení.  
24. Otevřete klientský certifikát a zkopírujte kryptografický otisk z karty **Podrobnosti**.  
25. V souboru MultiFactorAuthenticationAdfsAdapter.config nastavte **WebServiceSdkCertificateThumbprint** na řetězec zkopírovaný v předchozím kroku.  

V prostředí PowerShell spusťte skript \Program Files\Multi-Factor Authentication Server\Register-MultiFactorAuthenticationAdfsAdapter.ps1. Adaptér je zaregistrovaný jako WindowsAzureMultiFactorAuthentication. Aby se registrace projevila, je potřeba restartovat službu AD FS.

## Související témata
Pomoc při řešení potíží najdete v tématu [Nejčastější dotazy ke službě Azure Multi-Factor Authentication](multi-factor-authentication-faq.md).

<!--HONumber=Sep16_HO4-->


