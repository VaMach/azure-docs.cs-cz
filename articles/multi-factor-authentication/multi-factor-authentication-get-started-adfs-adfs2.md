<properties 
    pageTitle="Zabezpečení cloudových a lokálních prostředků pomocí Microsoft Azure Multi-Factor Authentication Serveru s AD FS 2.0" 
    description="Toto je stránka vícefaktorového ověřování Azure, která popisuje, jak začít s vícefaktorovým ověřováním Azure a službami AD FS 2.0." 
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
# Zabezpečení cloudových a lokálních prostředků pomocí Microsoft Azure Multi-Factor Authentication Serveru s AD FS 2.0

Pokud je vaše organizace federovaná pomocí Azure Active Directory a chcete zabezpečit některé lokální nebo cloudové prostředky, můžete k tomu použít Azure Multi-Factor Authentication Sever a nastavit ho na spolupráci s AD FS, aby se pro koncové body vysoké hodnoty spouštělo vícefaktorové ověření.

Tato dokumentace popisuje používání Azure Multi-Factor Authentication Serveru s AD FS 2.0  Informace o používání ověřování Azure Multi-Factor Authentication s Windows Serverem 2012 R2 AD FS najdete v tématu [Zabezpečený cloud a místní prostředky pomocí ověřování Azure Multi-Factor Authentication Server s Windows Server 2012 R2 AD FS](multi-factor-authentication-get-started-adfs-w2k12.md).


## Proxy server služby AD FS 2.0
Pro zabezpečení AD FS 2.0 pomocí serveru proxy nainstalujte Azure Multi-Factor Authentication Server na server proxy služby ADFS a nakonfigurujte ho podle následujícího postupu. 

### Zabezpečení AD FS 2.0 pomocí serveru proxy

1. V Azure Multi-Factor Authentication Serveru klikněte v levé nabídce na ikonu Ověřování IIS.
2. Klikněte na kartu Založené na formulářích.
3. Klikněte na tlačítko Přidat... .
<center>![Nastavení](./media/multi-factor-authentication-get-started-adfs-adfs2/setup1.png)</center>
4. K automatické detekci uživatelského jména, hesla a proměnných domény zadejte adresu URL pro přihlášení (např. https://sso.contoso.com/adfs/ls) v rámci dialogového okna Automatická konfigurace webu založeného na formuláři a klikněte na tlačítko OK.
5. Zaškrtněte políčko Vyžadovat uživatele Azure Multi-Factor Authentication, pokud byli nebo budou všichni uživatelé importováni do serveru a podstoupí Multi-Factor Authentication. Pokud se ještě neimportoval velký počet uživatelů do Serveru a/nebo se vyloučí z vícefaktorového ověřování, nechte pole nezaškrtnuté. Další informace o této funkci najdete v souboru nápovědy.
6. Pokud proměnné stránky nelze rozpoznat automaticky, klikněte na tlačítko Zadat ručně… v dialogovém okně Automatická konfigurace webu na základě formuláře.
7. V dialogovém okně Přidat web na základě formuláře zadejte adresu URL k přihlašovací stránce do pole Adresa URL pro odeslání (např. https://sso.contoso.com/adfs/ls) a zadejte název aplikace (volitelný). Název aplikace se zobrazí v sestavách Azure Multi-Factor Authentication a může se zobrazit v rámci SMS zpráv nebo mobilních aplikací ověřování. Další informace o Adrese URL pro odeslání naleznete v souboru nápovědy.
8. Formát požadavku nastavte na „POST nebo GET“.
9. Zadejte Proměnnou uživatelského jména (ctl00$ContentPlaceHolder1$UsernameTextBox) a Proměnnou hesla (ctl00$ContentPlaceHolder1$PasswordTextBox). Pokud vaše formulářová přihlašovací stránka zobrazí pole pro doménu, zadejte taky Proměnnou domény. Možná bude potřeba přejít na přihlašovací stránku ve webovém prohlížeči, kliknout pravým tlačítkem na stránku a vybrat možnost „Zdroj zobrazení“ pro vyhledání názvů vstupních polí v rámci přihlašovací stránky.
10. Zaškrtněte políčko Vyžadovat uživatele Azure Multi-Factor Authentication, pokud byli nebo budou všichni uživatelé importováni do serveru a podstoupí Multi-Factor Authentication. Pokud se ještě neimportoval velký počet uživatelů do Serveru a/nebo se vyloučí z vícefaktorového ověřování, nechte pole nezaškrtnuté.
<center>![Nastavení](./media/multi-factor-authentication-get-started-adfs-adfs2/manual.png)</center>
11. Klikněte na tlačítko Rozšířené... pro kontrolu upřesňujícího nastavení, včetně schopnosti výběru vlastního soubor odmítnutí stránky, pro ukládání úspěšných ověření na webu do mezipaměti po dobu používání souborů cookie a vyberte, jak chcete ověřit primární pověření.
12. Protože se server proxy ADFS pravděpodobně nepřipojí do domény, pravděpodobně použijete LDAP pro připojení k řadiči domény pro předběžné ověření a import uživatelů. V dialogovém okně Rozšířená webová stránka s formuláři klikněte na kartu Primární ověření a pro typ ověření předběžného ověření vyberte „Vázání protokolu LDAP“.
13. Po dokončení klikněte na tlačítko OK a vraťte se do dialogového okna Přidat web založený na formuláři. Další informace o rozšířeném nastavení naleznete v souboru nápovědy.
14. Zavřete dialogové okno kliknutím na tlačítko OK.
15. Po zjištění nebo zadání Adresy URL a proměnných hodnot stránek se zobrazí data webové stránky v panelu založeném na formulářích.
16. Klikněte na kartu Nativní modul a vyberte server, webovou stránku, ve které běží server proxy ADFS (např. „Výchozí webová stránka“) nebo aplikaci proxy ADFS (např. „ls“ v „adfs“), a tím povolíte modul plug-in IIS na požadované úrovni.
17. Klikněte na políčko Povolit IIS ověřování v horní části obrazovky.
18. Teď je povolené IIS ověřování Abyste ale mohli provádět předběžné ověřování pro vaše Active Directory (AD) přes LDAP, musíte připojení LDAP nakonfigurovat na řadič domény. Klikněte proto na ikonu Integrace adresáře.
19. Na kartě Nastavení vyberte přepínač Použít konkrétní konfiguraci LDAP.
<center>![Nastavení](./media/multi-factor-authentication-get-started-adfs-adfs2/ldap1.png)</center>
20. Klikněte na tlačítko Upravit... .
21. V dialogovém okně Upravit konfiguraci LDAP vyplňte pole pomocí informací požadovaných pro připojení k řadiči domény AD. Popisy polí jsou zahrnuté v následující tabulce. Poznámka: Tyto informace jsou taky uvedené v souboru nápovědy Azure Multi-Factor Authentication Serveru.
22. Otestujte připojení LDAP kliknutím na tlačítko Test.
<center>![Nastavení](./media/multi-factor-authentication-get-started-adfs-adfs2/ldap2.png)</center>
23. Pokud byl test připojení LDAP úspěšný, klikněte na tlačítko OK.
24. Dál klikněte na ikonu Nastavení společnosti a vyberte kartu Překlad uživatelského jména.
25. Označte přepínač Pro porovnávání uživatelských jmen použít atribut jedinečného identifikátoru LDAP.
26. Pokud uživatelé zadají své uživatelské jméno do přihlašovacího formuláře proxy serveru ADFS ve formátu „doména\uživatelské jméno“, Server musí být schopný při vytváření dotazu LDAP oddělit doménu od uživatelského jména. To můžete udělat nastavením registrů.
27. Na 64bitovém serveru otevřete editor registrů a přejděte na HKEY_LOCAL_MACHINE/SOFTWARE/Wow6432Node/Positive Networks/PhoneFactor. Pokud jste na 32bitovém serveru, odeberte „Wow6432Node“ z cesty. Vytvořte nový klíč registru typu DWORD s názvem „UsernameCxz_stripPrefixDomain“ a nastavte hodnotu na 1. Azure Multi-Factor Authentication teď zabezpečuje proxy server ADFS. Ověřte, že se uživatelé naimportovali z Active Directory do Serveru. Pokud chcete vytvořit bílou listinu interních IP adres, aby nebylo třeba dvoufaktorové ověřování při přihlašování k webu z těchto umístění, projděte si část Důvěryhodné IP adresy níže.

<center>![Nastavení](./media/multi-factor-authentication-get-started-adfs-adfs2/reg.png)</center>

## AD FS 2.0 Direct bez serveru proxy

Pro zabezpečení AD FS 2.0 bez použití serveru proxy nainstalujte Azure Multi-Factor Authentication Server na server ADFS a nakonfigurujte ho podle následujícího postupu. 

### Zabezpečení AS FS 2.0 bez serveru proxy
1. V Azure Multi-Factor Authentication Serveru klikněte v levé nabídce na ikonu Ověřování IIS.
2. Klikněte na kartu protokolu HTTP.
3. Klikněte na tlačítko Přidat... .
4. V dialogovém okně Přidat základní adresu URL zadejte adresu URL pro webovou stránku ADFS, kde se provádí ověřování HTTP (např. https://sso.domena.com/adfs/ls/auth/integrated) do pole Základní adresa URL a zadejte název aplikace (volitelné). Název aplikace se zobrazí v sestavách Azure Multi-Factor Authentication a může se zobrazit v rámci SMS zpráv nebo mobilních aplikací ověřování.
5. Podle potřeby upravte časový limit nečinnosti a maximální dobu trvání relace.
6. Zaškrtněte políčko Vyžadovat uživatele Azure Multi-Factor Authentication, pokud byli nebo budou všichni uživatelé importováni do serveru a podstoupí Multi-Factor Authentication. Pokud se ještě neimportoval velký počet uživatelů do Serveru a/nebo se vyloučí z vícefaktorového ověřování, nechte pole nezaškrtnuté. Další informace o této funkci najdete v souboru nápovědy.
7. V případě potřeby zaškrtněte políčko mezipaměti souborů cookie.
<center>![Nastavení](./media/multi-factor-authentication-get-started-adfs-adfs2/noproxy.png)</center>
8. Klikněte na tlačítko OK.
9. Klikněte na kartu Nativní modul a vyberte server, webovou stránku, ve které běží ADFS (např. „Výchozí webová stránka“) nebo aplikaci ADFS (např. „ls“ v „adfs“), a tím povolíte modul plug-in IIS na požadované úrovni.
10. Klikněte na políčko Povolit IIS ověřování v horní části obrazovky. Azure Multi-Factor Authentication teď zabezpečuje ADFS. Ověřte, že se uživatelé naimportovali z Active Directory do Serveru. Pokud chcete vytvořit bílou listinu interních IP adres, aby nebylo třeba dvoufaktorové ověřování při přihlašování k webu z těchto umístění, projděte si část Důvěryhodné IP adresy níže.


## Důvěryhodné IP adresy
Důvěryhodné IP adresy umožňují uživatelům obejít ověřování Azure Multi-Factor Authentication u požadavků webů pocházejících z konkrétní IP adresy nebo podsítě. Například můžete chtít vyloučit uživatele z ověřování Azure Multi-Factor Authentication při přihlašování z kanceláře. V takovém případě zadáte podsíť office jako položku důvěryhodné adresy IP. 

### Konfigurace důvěryhodných adres IP


1. V části ověření služby IIS klikněte na kartu Důvěryhodné IP adresy.
1. Klikněte na tlačítko Přidat... .
1. Jakmile se zobrazí dialogové okno Přidat důvěryhodné IP adresy, vyberte jednu IP adresu, rozsah IP adres nebo přepínač podsítě.
1. Zadejte IP adresu, rozsah IP adres nebo podsíť, které chcete zařadit na seznam povolených adres. Pokud zadáváte podsíť, vyberte příslušnou síťovou masku a klikněte na tlačítko OK. Seznam důvěryhodných adres IP se přidal.


<center>![Nastavení](./media/multi-factor-authentication-get-started-adfs-adfs2/trusted.png)</center>

 


<!--HONumber=Jun16_HO2-->


