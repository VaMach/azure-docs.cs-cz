<properties 
    pageTitle="Zabezpečení cloudových a lokálních prostředků pomocí Azure MFA Serveru s Windows Serverem 2012 R2 AD FS" 
    description="Toto je stránka vícefaktorového ověřování Azure, která popisuje, jak začít s vícefaktorovým ověřováním Azure a službami AD FS na Windows Serveru 2012 R2." 
    services="multi-factor-authentication" 
    documentationCenter="" 
    authors="billmath" 
    manager="stevenpo" 
    editor="curtland"/>

<tags 
    ms.service="multi-factor-authentication" 
    ms.workload="identity" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="get-started-article" 
    ms.date="05/12/2016" 
    ms.author="billmath"/>


# Zabezpečení cloudových a lokálních prostředků pomocí Microsoft Azure Multi-Factor Authentication Serveru s Windows Serverem 2012 R2 AD FS 2012

Pokud je vaše organizace federovaná pomocí Azure AD a chcete zabezpečit některé lokální nebo cloudové prostředky, můžete k tomu použít Azure Multi-Factor Authentication Sever a nastavit ho na spolupráci s AD FS, aby se pro koncové body vysoké hodnoty spouštělo vícefaktorové ověření.

Tato dokumentace popisuje používání Azure Mul-Factor Authentication Serveru s AD FS ve Windows Serveru 2012 R2.  Informace o používání ověřování Azure Multi-Factor Authentication s AD FS 2.0 najdete v tématu [Zabezpečený cloud a místní prostředky pomocí ověřování Azure Multi-Factor Authentication Server s AD FS 2.0](multi-factor-authentication-get-started-adfs-adfs2.md).

## Zabezpečení Windows Serveru 2012 R2 AD FS pomocí Azure Multi-Factor Authentication Serveru

Při instalaci Azure Multi-Factor Authentication Serveru máte tyto dvě možnosti:

- Nainstalovat Azure Multi-Factor Authentication Server lokálně na stejném serveru jako AD FS 
- Nainstalovat Azure Multi-Factor Authentication Adapter lokálně na AD FS Serveru a nainstalovat MFA Server na jiný počítač

Před zahájením se ujistěte, že znáte následující informace:

- Azure Multi-Factor Authentication Server nemusí být nainstalovaný na vašem federačním serveru AD FS, ale Multi-Factor Authentication Adapter pro AD FS musí být nainstalovaný na Windows Serveru 2012 R2, na kterém běží AD FS. Server můžete nainstalovat na jiný počítač, pokud se jedná o podporovanou verzi, a adaptér AD FS můžete nainstalovat samostatně na federační server AD FS. Pokyny pro instalaci adaptéru samostatně najdete v postupu dole.
- Při vytváření Adapteru AD FS pro Multi-Factor Authentication Server se předpokládalo, že AD FS může předávat název předávající strany do adaptéru, který by se použil jako název aplikace.  Ukázalo se ale, že to tak není.  Pokud používáte metody ověření pomocí textové zprávy nebo mobilní aplikace, řetězce definované v Nastavení společnosti obsahují zástupný text „<$application_name$>“.  Při použití AD FS Adapteru se tento zástupný text nenahradí.  Proto se při zabezpečování AD FS doporučuje tento zástupný text odebrat z příslušných řetězců.

- Přihlášený účet musí mít oprávnění vytvářet skupiny zabezpečení v Active Directory.

- Průvodce instalací Multi-Factor Authentication AD FS Adapteru ve vašem Active Directory vytvoří skupinu zabezpečení s názvem PhoneFactor Admins, a pak do této skupiny přidá účet služby AD FS vaší federační služby. Doporučuje se, abyste na svém ovladači domény ověřili, že se skupina PhoneFactor Admins opravdu vytvořila a že je účet služby AD FS členem této skupiny. Pokud to bude potřeba, přidejte účet služby AD FS do skupiny PhoneFactor Admins ve vašem řadiči domény ručně.
- Informace o instalaci sady SDK webové služby pomocí uživatelského portálu najdete v tématu [Nasazení uživatelského portálu pro Azure Multi-Factor Authentication Server.](multi-factor-authentication-get-started-portal.md)
  

### Instalace Azure Multi-Factor Authentication Server lokálně na stejném serveru jako AD FS

1. Stáhněte a nainstalujte Azure Multi-Factor Authentication Server lokálně na federační server AD FS. Další informace o instalaci serveru Azure Multi-Factor Authentication najdete v článku [Začínáme s ověřováním Azure Multi-Factor Authentication Serverem](multi-factor-authentication-get-started-server.md).
2. V uživatelském prostředí Azure Multi-Factor Authentication Serveru vyberte ikonu AD FS a vyberte možnosti pro Povolit zápis uživatele a Povolit uživatelům výběr metody.
3. Vyberte jakékoli další možnosti.
4. Klikněte na Instalovat AD FS Adapter.
<center>![Cloud](./media/multi-factor-authentication-get-started-adfs-w2k12/server.png)</center>
5. Pokud je počítač připojený k doméně a konfigurace Active Directory pro zabezpečení komunikace mezi AD FS Adapterem a ověřovací službou Multi-Factor Authentication není kompletní, zobrazí se krok Active Directory.  Kliknutím na tlačítko Další můžete tuto konfiguraci automaticky dokončit, jinak můžete označit zatržítko Vynechat automatickou konfiguraci služby Active Directory a konfigurovat nastavení ručně a pak kliknout na tlačítko Další.
6. Pokud počítač není připojený k doméně a konfigurace Místní skupiny pro zabezpečení komunikace mezi AD FS Adapterem a ověřovací službou Multi-Factor Authentication není kompletní, zobrazí se krok Místní skupiny.  Kliknutím na tlačítko Další můžete tuto konfiguraci automaticky dokončit, jinak můžete označit zatržítko Vynechat automatickou konfiguraci místní skupiny a konfigurovat nastavení ručně a pak kliknout na tlačítko Další.
7. Otevře se průvodce instalací, klikněte na Další, tím umožníte Multi-Factor Authentication Serveru vytvořit skupinu PhoneFactor Admins a přidat účet služby AD FS do skupiny PhoneFactor Admins.
<center>![Cloud](./media/multi-factor-authentication-get-started-adfs-w2k12/adapter.png)</center>
8. V kroku Spustit instalační program klikněte na Další.
9. V instalačním programu Multi-Factor Authentication AD FS Adapteru klikněte na Další.
10. Až instalace skončí, klikněte na Zavřít.
11. Když je teď adaptér nainstalovaný, musí se zaregistrovat do AD FS. Otevřete Windows PowerShell a spusťte tento příkaz: C:\Program Files\Multi-Factor Authentication Server\Register-MultiFactorAuthenticationAdfsAdapter.ps1 <center>![Cloud](./media/multi-factor-authentication-get-started-adfs-w2k12/pshell.png)</center>
12. Teď musíme upravit Globální zásady ověřování v AD FS, abychom mohli náš nově zaregistrovaný adaptér použít. V Konzole pro správu AD FS přejděte k uzlu Zásady ověřování a v části Vícefaktorové ověřování klikněte na odkaz Upravit vedle dílčí části Globální nastavení. V okně Upravit globální zásady ověření vyberte jako další metodu ověření Multi-Factor Authentication a klikněte na OK. Adaptér je zaregistrovaný jako WindowsAzureMultiFactorAuthentication.  Aby se registrace projevila, je potřeba restartovat službu AD FS.

<center>![Cloud](./media/multi-factor-authentication-get-started-adfs-w2k12/global.png)</center>

V tuto chvíli je Multi-Factor Authentication Server nastavený jako dodatečný poskytovatel ověření pro použití s AD FS.

## Samostatná instalace AD FS Adapteru pomocí sady SDK webové služby
1. Nainstalujte sadu SDK webové služby na server, na kterém běží Multi-Factor Authentication Server.
2. Zkopírujte soubory MultiFactorAuthenticationAdfsAdapterSetup64.msi, Register-MultiFactorAuthenticationAdfsAdapter.ps1, Unregister-MultiFactorAuthenticationAdfsAdapter.ps1, and MultiFactorAuthenticationAdfsAdapter.config z adresáře \Program Files\Multi-Factor Authentication Server na server, na kterém chcete nainstalovat AD FS Adapter.
3. Spusťte soubor MultiFactorAuthenticationAdfsAdapterSetup64.msi.
4. V instalačním programu Multi-Factor Authentication AD FS Adapteru klikněte na Další a provede se instalace.
5. Až instalace skončí, klikněte na tlačítko Zavřít.
6. V souboru MultiFactorAuthenticationAdfsAdapter.config udělejte tyto změny:

MultiFactorAuthenticationAdfsAdapter.config
Krok| Dílčí krok
:------------- | :------------- |
Nastavte uzel UseWebServiceSdk na true.||
Nastavte WebServiceSdkUrl na URL sady SDK webové služby pro Multi-Factor Authentication.||
Možnost 1 - Konfigurace sady SDK webové služby pomocí uživatelského jména a hesla.|<ol><li>Nastavte WebServiceSdkUsername na účet, který je členem skupiny zabezpečení PhoneFactor Admins.  Použijte formát <domain>\<uživatelské jméno>.<li>Nastavte WebServiceSdkPassword na odpovídající heslo k účtu.</li></ol>
Možnost 2 - Konfigurace sady SDK webové služby pomocí klientského certifikátu.|<ol><li>Získejte klientský certifikát od certifikační autority pro server, na kterém běží sada SDK webové služby.  Informace o získání certifikátu najdete v tématu [Získání klientského certifikátu](https://technet.microsoft.com/library/cc770328(v=ws.10).aspx).</li><li>Importujte klientský certifikát do Osobního úložiště certifikátů na místním serveru, na kterém běží sada SDK webové služby.  Poznámka:  Ujistěte se, že je veřejný certifikát certifikační autority v Důvěryhodných kořenových certifikátech.</li><li>Exportujte veřejný a privátní klíč klientského certifikátu do souboru .pfx.</li><li>Exportujte veřejný klíč ve formátu base-64 do souboru .cer.</li><li>Ve Správci serveru zkontrolujte, že je nainstalovaná funkce Web Server (IIS)\Web Server\Security\IIS Client Certificate Mapping Authentication.</li><li>Pokud nainstalovaná není, přidejte tuto funkci kliknutím na Přidat role a Funkce.</li><li>Ve Správci IIS poklikejte na Editor konfigurace ve webové stránce, která obsahuje virtuální adresář sady SDK webové služby.  Poznámka: Je velmi důležité, abyste to provedli na úrovni webové stránky a ne na úrovni virtuálního adresáře.</li><li>Přejděte do části system.webServer/security/authentication/iisClientCertificateMappingAuthentication.</li><li>Nastavte enabled na true.</li><li>Nastavte oneToOneCertificateMappingsEnabled na true.</li><li>Klikněte na tlačítko ... vedle oneToOneMappings.</li><li>Klikněte na odkaz Přidat.</li><li>Otevřete soubor .cer ve formátu base-64 , který jste předtím exportovali.  Odstraňte -----BEGIN CERTIFICATE-----, -----END CERTIFICATE----- a všechna zalomení řádků.  Výsledný řetězec zkopírujte.</li><li>Nastavte certifikát na řetězec zkopírovaný v předchozím kroku.</li><li>Nastavte enabled na true.</li><li>Nastavte userName na účet, který je členem skupiny zabezpečení PhoneFactor Admins.  Použijte formát <domain>\<uživatelské jméno>.</li><li>Nastavte password na odpovídající heslo k účtu.</li><li>Zavřete Editor kolekce.</li><li>Klikněte na odkaz Použít.</li><li>Přejděte do virtuálního adresáře sady SDK webové služby.</li><li>Poklikejte na Ověření.</li><li>Zkontrolujte, že jsou Zosobnění technologie ASP.NET a Základní ověření Zapnuté a všechny ostatní položky Vypnuté.</li><li>Znovu přejděte do virtuálního adresáře sady SDK webové služby.</li><li>Poklikejte na Nastavení SSL.</li><li>Nastavte Klientské certifikáty na Přijmout a klikněte na Použít.</li><li>Soubor .pfx, který jste předtím vyexportovali, zkopírujte do serveru, na kterém běží AD FS Adapter.</li><li>Soubor .pfx importujte do Osobního úložiště certifikátů na místním počítači.</li><li>Po kliknutí pravým tlačítkem se objeví nabídka, ve které vyberte Spravovat privátní klíče a účtu přihlášenému do služby Active Directory Federation Services udělte přístup pro čtení.</li><li>Otevřete klientský certifikát a zkopírujte kryptografický otisk z karty Podrobnosti.</li><li>V souboru MultiFactorAuthenticationAdfsAdapter.config nastavte WebServiceSdkCertificateThumbprint na řetězec zkopírovaný v předchozím kroku.</li></ol>
Upravte skript Register-MultiFactorAuthenticationAdfsAdapter.ps1 tak, že na konec příkazu Register-AdfsAuthenticationProvider přidáte -ConfigurationFilePath <path>, kde <path> je úplná cesta k souboru MultiFactorAuthenticationAdfsAdapter.config.|


Teď v PowerShellu spusťte skript \Program Files\Multi-Factor Authentication Server\Register-MultiFactorAuthenticationAdfsAdapter.ps1, kterým adaptér zaregistrujete.  Adaptér je zaregistrovaný jako WindowsAzureMultiFactorAuthentication.  Aby se registrace projevila, je potřeba restartovat službu AD FS. 




























 

 


 

 


 





 


 

























































































 


 

 






 


<!--HONumber=Jun16_HO2-->


