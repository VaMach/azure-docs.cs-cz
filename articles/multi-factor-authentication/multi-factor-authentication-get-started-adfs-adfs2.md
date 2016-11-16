---
title: "Použití Azure MFA Serveru s AD FS 2.0 | Dokumentace Microsoftu"
description: "Toto je stránka vícefaktorového ověřování Azure, která popisuje, jak začít s vícefaktorovým ověřováním Azure a službami AD FS 2.0."
services: multi-factor-authentication
documentationcenter: 
author: kgremban
manager: femila
editor: yossib
ms.assetid: 96168849-241a-4499-a224-d829913caa7e
ms.service: multi-factor-authentication
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 10/14/2016
ms.author: kgremban
translationtype: Human Translation
ms.sourcegitcommit: 219dcbfdca145bedb570eb9ef747ee00cc0342eb
ms.openlocfilehash: 639f11856fcb0828db2375e428d9f7efe640b5d7


---
# <a name="secure-cloud-and-onpremises-resources-using-azure-multifactor-authentication-server-with-ad-fs-20"></a>Zabezpečení cloudových a lokálních prostředků pomocí Microsoft Azure Multi-Factor Authentication Serveru s AD FS 2.0
Tento článek je určený pro organizace, které jsou federované se službou Azure Active Directory a chtějí zabezpečit prostředky, které mají uložené místně nebo v cloudu. Chraňte své prostředky pomocí Azure Multi-Factor Authentication Serveru a jeho nakonfigurováním tak, aby fungoval se službou AD FS a bylo možné spouštět dvoustupňové ověření pro koncové body s vysokou hodnotou.

Tato dokumentace popisuje používání Azure Multi-Factor Authentication Serveru s AD FS 2.0  Další informace najdete v tématu [Zabezpečený cloud a místní prostředky používající ověřování Azure Multi-Factor Authentication Server s Windows Server 2012 R2 AD FS](multi-factor-authentication-get-started-adfs-w2k12.md)

## <a name="secure-ad-fs-20-with-a-proxy"></a>Zabezpečení AD FS 2.0 pomocí serveru proxy
Pro zabezpečení AD FS 2.0 pomocí serveru proxy nainstalujte Azure Multi-Factor Authentication Server na server proxy služby ADFS a nakonfigurujte ho.

### <a name="configure-iis-authentication"></a>Konfigurace ověřování IIS
1. V Azure Multi-Factor Authentication Serveru klikněte v levé nabídce na ikonu **ověřování IIS**.
2. Klikněte na kartu **Založené na formulářích**.
3. Klikněte na tlačítko **Přidat...** .
   <center>![Nastavení](./media/multi-factor-authentication-get-started-adfs-adfs2/setup1.png)</center>
4. K automatické detekci uživatelského jména, hesla a proměnných domény zadejte adresu URL pro přihlášení (například https://sso.contoso.com/adfs/ls) v rámci dialogového okna Automatická konfigurace webu založeného na formuláři a klikněte na tlačítko OK.
5. Zaškrtněte políčko **Vyžadovat porovnání uživatele Azure Multi-Factor Authentication**, pokud byli nebo budou všichni uživatelé importováni na server a podstoupí dvoustupňové ověření. Pokud ještě na server nebyl importován velký počet uživatelů nebo budou uživatelé vyloučení z dvoustupňového ověření, nechte toto políčko nezaškrtnuté. Další informace o této funkci najdete v souboru nápovědy.
6. Pokud proměnné stránky nejde rozpoznat automaticky, klikněte na **Zadat ručně...** v dialogovém okně Automatická konfigurace webu na základě formuláře.
7. V dialogovém okně Přidat web na základě formuláře zadejte adresu URL k přihlašovací stránce do pole Adresa URL pro odeslání (např. https://sso.contoso.com/adfs/ls) a zadejte název aplikace (volitelný). Název aplikace se zobrazí v sestavách Azure Multi-Factor Authentication a může se zobrazit v rámci SMS zpráv nebo mobilních aplikací ověřování. Další informace o Adrese URL pro odeslání naleznete v souboru nápovědy.
8. Formát požadavku nastavte na „POST nebo GET“.
9. Zadejte Proměnnou uživatelského jména (ctl00$ContentPlaceHolder1$UsernameTextBox) a Proměnnou hesla (ctl00$ContentPlaceHolder1$PasswordTextBox). Pokud vaše formulářová přihlašovací stránka zobrazí pole pro doménu, zadejte taky Proměnnou domény. Možná bude potřeba přejít na přihlašovací stránku ve webovém prohlížeči, kliknout pravým tlačítkem na stránku a vybrat možnost **Zdroj zobrazení** pro vyhledání názvů vstupních polí v rámci přihlašovací stránky.
10. Zaškrtněte políčko **Vyžadovat porovnání uživatele Azure Multi-Factor Authentication**, pokud byli nebo budou všichni uživatelé importováni na server a podstoupí dvoustupňové ověření. Pokud ještě na server nebyl importován velký počet uživatelů nebo budou uživatelé vyloučení z dvoustupňového ověření, nechte toto políčko nezaškrtnuté.
    <center>![Nastavení](./media/multi-factor-authentication-get-started-adfs-adfs2/manual.png)</center>
11. Klikněte na tlačítko **Upřesnit...** pro kontrolu upřesňujícího nastavení. Nastavení můžete nakonfigurovat včetně schopnosti výběru vlastního souboru odmítnutí stránky, pro ukládání úspěšných ověření na webu do mezipaměti po dobu používání souborů cookie a vybrat, jak chcete ověřit primární přihlašovací údaje.
12. Protože se server proxy ADFS pravděpodobně nepřipojí do domény, můžete použít LDAP pro připojení k řadiči domény pro předběžné ověření a import uživatelů. V dialogovém okně Rozšířená webová stránka s formuláři klikněte na kartu **Primární ověření** a pro typ ověření předběžného ověření vyberte **Vázání protokolu LDAP**.
13. Po dokončení klikněte na tlačítko **OK** pro návrat do dialogového okna Přidat web založený na formuláři. Další informace o rozšířeném nastavení naleznete v souboru nápovědy.
14. Zavřete dialogové okno kliknutím na tlačítko **OK**.
15. Po zjištění nebo zadání adresy URL a proměnných hodnot stránek se data webové stránky zobrazí v panelu založeném na formulářích.
16. Klikněte na kartu **Nativní modul** a vyberte server, web, na kterém je spuštěný ADFS proxy (jako Výchozí web) nebo aplikaci ADFS proxy (jako „ls“ v části „adfs“) pro povolení modulu plug-in IIS na požadované úrovni.
17. Zaškrtněte políčko **Povolit ověřování IIS** v horní části obrazovky.
18. Teď je povolené IIS ověřování

### <a name="configure-directory-integration"></a>Konfigurace integrace adresáře
Povolili jste ověřování IIS, ale abyste mohli provádět předběžné ověřování Active Directory (AD) přes LDAP, musíte připojení LDAP nakonfigurovat na řadič domény.

1. Klikněte na ikonu **Integrace adresáře**.
2. Na kartě Nastavení vyberte přepínač **Použít specifickou konfiguraci LDAP**.
   <center>![Nastavení](./media/multi-factor-authentication-get-started-adfs-adfs2/ldap1.png)</center>
3. Klikněte na tlačítko **Upravit...** .
4. V dialogovém okně Upravit konfiguraci LDAP vyplňte pole pomocí informací požadovaných pro připojení k řadiči domény AD. Popisy polí jsou zahrnuty v následující tabulce. Tyto informace jsou také uvedeny v souboru nápovědy serveru Azure Multi-Factor Authentication.
5. Otestujte připojení LDAP kliknutím na tlačítko **Test**.
   <center>![Nastavení](./media/multi-factor-authentication-get-started-adfs-adfs2/ldap2.png)</center>
6. Pokud byl test připojení LDAP úspěšný, klikněte na tlačítko **OK**.

### <a name="configure-company-settings"></a>Konfigurace nastavení společnosti
1. Dále klikněte na ikonu **Nastavení společnosti** a vyberte kartu **Překlad uživatelského jména**.
2. Označte přepínač **Pro porovnávání uživatelských jmen použít atribut jedinečného identifikátoru LDAP**.
3. Pokud uživatelé zadají své uživatelské jméno ve formátu „doména\uživatelské jméno“, Server musí být schopný při vytváření dotazu LDAP oddělit doménu od uživatelského jména. To můžete udělat nastavením registrů.
4. Na 64bitovém serveru otevřete editor registrů a přejděte na HKEY_LOCAL_MACHINE/SOFTWARE/Wow6432Node/Positive Networks/PhoneFactor. Pokud jste na 32bitovém serveru, odeberte „Wow6432Node“ z cesty. Vytvořte klíč registru typu DWORD s názvem „UsernameCxz_stripPrefixDomain“ a nastavte hodnotu na 1. Azure Multi-Factor Authentication teď zabezpečuje proxy server ADFS.

Ověřte, že se uživatelé naimportovali z Active Directory do Serveru. Pokud chcete vytvořit bílou listinu interních IP adres, aby při přihlašování k webu z těchto umístění nebylo potřeba dvoustupňové ověření, projděte si část [Důvěryhodné IP adresy](#trusted-ips).

<center>![Nastavení](./media/multi-factor-authentication-get-started-adfs-adfs2/reg.png)</center>

## <a name="ad-fs-20-direct-without-a-proxy"></a>AD FS 2.0 Direct bez serveru proxy
AD FS můžete zabezpečit, i když se server proxy AD FS nepoužívá. Nainstalujte Azure Multi-Factor Authentication Server na server služby ADFS a nakonfigurujte ho podle následujícího postupu:

1. V Azure Multi-Factor Authentication Serveru klikněte v levé nabídce na ikonu **ověřování IIS**.
2. Klikněte na kartu **HTTP**.
3. Klikněte na tlačítko **Přidat...** .
4. V dialogovém okně Přidat základní adresu URL zadejte adresu URL pro webovou stránku ADFS, kde se provádí ověřování HTTP (například https://sso.domain.com/adfs/ls/auth/integrated) do pole Základní adresa URL. Potom zadejte název aplikace (volitelné). Název aplikace se zobrazí v sestavách Azure Multi-Factor Authentication a může se zobrazit v rámci SMS zpráv nebo mobilních aplikací ověřování.
5. Podle potřeby upravte časový limit nečinnosti a maximální dobu trvání relace.
6. Zaškrtněte políčko **Vyžadovat porovnání uživatele Azure Multi-Factor Authentication**, pokud byli nebo budou všichni uživatelé importováni na server a podstoupí dvoustupňové ověření. Pokud ještě na server nebyl importován velký počet uživatelů nebo budou uživatelé vyloučení z dvoustupňového ověření, nechte toto políčko nezaškrtnuté. Další informace o této funkci najdete v souboru nápovědy.
7. V případě potřeby zaškrtněte políčko mezipaměti souborů cookie.
   <center>![Nastavení](./media/multi-factor-authentication-get-started-adfs-adfs2/noproxy.png)</center>
8. Klikněte na tlačítko **OK**.
9. Klikněte na kartu **Nativní modul** a vyberte server, web (jako Výchozí web) nebo aplikaci ADFS proxy (jako „ls“ v části „adfs“) pro povolení modulu plug-in IIS na požadované úrovni.
10. Zaškrtněte políčko **Povolit ověřování IIS** v horní části obrazovky. Azure Multi-Factor Authentication teď zabezpečuje ADFS.

Ověřte, že se uživatelé naimportovali z Active Directory do Serveru. Pokud chcete vytvořit bílou listinu interních IP adres, aby při přihlašování k webu z těchto umístění nebylo potřeba dvoustupňové ověření, projděte si část Důvěryhodné IP adresy.

## <a name="trusted-ips"></a>Důvěryhodné IP adresy
Důvěryhodné IP adresy umožňují uživatelům obejít ověřování Azure Multi-Factor Authentication u požadavků webů pocházejících z konkrétní IP adresy nebo podsítě. Můžete třeba chtít vyloučit uživatele z dvoustupňového ověření, když se přihlásí z kanceláře. V takovém případě zadáte podsíť kanceláře jako položku důvěryhodných IP adres.

### <a name="to-configure-trusted-ips"></a>Konfigurace důvěryhodných adres IP
1. V části ověření služby IIS klikněte na kartu **Důvěryhodné IP adresy**.
2. Klikněte na tlačítko **Přidat...** .
3. Jakmile se zobrazí dialogové okno Přidat důvěryhodné IP adresy, vyberte přepínač **Jedna IP adresa**, **Rozsah IP adres** nebo **Podsíť**.
4. Zadejte IP adresu, rozsah IP adres nebo podsíť, které chcete zařadit na seznam povolených adres. Pokud zadáváte podsíť, vyberte příslušnou síťovou masku a klikněte na tlačítko **OK**. Seznam důvěryhodných adres IP se přidal.

<center>![Nastavení](./media/multi-factor-authentication-get-started-adfs-adfs2/trusted.png)</center>



<!--HONumber=Nov16_HO2-->


