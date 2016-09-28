<properties 
    pageTitle="Ověření služby IIS a server Azure Multi-Factor Authentication"
    description="Toto je stránka Azure Multi-Factor Authentication, která vám pomůže při nasazení ověření IIS a serveru Azure Multi-Factor Authentication."
    services="multi-factor-authentication"
    documentationCenter=""
    authors="kgremban"
    manager="femila"
    editor="curtand"/>

<tags
    ms.service="multi-factor-authentication"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="08/04/2016"
    ms.author="kgremban"/>


# Ověřování IIS

Část ověření služby IIS serveru Azure Multi-Factor Authentication umožňuje povolit a konfigurovat ověřování služby IIS pro integraci s webovými aplikacemi IIS Microsoft. Server Azure Multi-Factor Authentication nainstaluje modul plug-in, který dokáže filtrovat požadavky prováděné na webovém serveru IIS, aby bylo možné přidat Azure Multi-Factor Authentication. Modul plug-in služby IIS poskytuje podporu pro ověřování na základě formuláře a integrované HTTP ověřování systému Windows. Důvěryhodné IP adresy mohou být také nakonfigurovány k vyloučení interních IP adres z dvoufaktorového ověřování.


![Ověřování IIS](./media/multi-factor-authentication-get-started-server-iis/iis.png)


## Ověřování založené na formulářích služby IIS pomocí serveru Azure Multi-Factor Authentication

Pro zabezpečení webové aplikace služby IIS, která používá ověřování založené na formulářích, nainstalujte server Azure Multi-Factor Authentication na webový server služby IIS a nakonfigurujte server podle následujícího postupu.

1. V Azure Multi-Factor Authentication Serveru klikněte v levé nabídce na ikonu Ověřování IIS.
2. Klikněte na kartu Založené na formulářích.
3. Klikněte na tlačítko Přidat... .
4. K automatické detekci uživatelského jména, hesla a proměnných domény zadejte adresu URL pro přihlášení (např. https://localhost/contoso/auth/login.aspx) v rámci dialogového okna Automatická konfigurace webu založeného na formuláři a klikněte na tlačítko OK.
5. Zaškrtněte políčko Vyžadovat uživatele Multi-Factor Authentication, pokud byli nebo budou všichni uživatelé importováni do serveru a podstoupí Multi-Factor Authentication. Pokud se ještě neimportoval velký počet uživatelů do Serveru a/nebo se vyloučí z vícefaktorového ověřování, nechte pole nezaškrtnuté.
6. Pokud proměnné stránky nelze rozpoznat automaticky, klikněte na tlačítko Zadat ručně... v dialogovém okně Automatická konfigurace webu na základě formuláře.
7. V dialogovém okně Přidat web na základě formuláře zadejte adresu URL do přihlašovací stránky v poli Adresa URL pro odeslání a zadejte název aplikace (volitelný). Název aplikace se zobrazí v sestavách Azure Multi-Factor Authentication a může se zobrazit v rámci SMS zpráv nebo mobilních aplikací ověřování. Další informace o Adrese URL pro odeslání naleznete v souboru nápovědy.
8. Vyberte správný formát požadavku. Tato hodnota je u většiny webových aplikací nastavena na „POST nebo GET“.
9. Zadejte proměnnou Uživatelské jméno, Heslo a Doména (pokud se zobrazí na stránce přihlášení). Možná bude nutné přejít na přihlašovací stránku ve webovém prohlížeči, kliknout pravým tlačítkem na stránku a vybrat možnost „Zdroj zobrazení“ pro vyhledání názvů vstupních polí v rámci stránky.
10. Zaškrtněte políčko Vyžadovat uživatele Azure Multi-Factor Authentication, pokud byli nebo budou všichni uživatelé importováni do serveru a podstoupí Multi-Factor Authentication. Pokud se ještě neimportoval velký počet uživatelů do Serveru a/nebo se vyloučí z vícefaktorového ověřování, nechte pole nezaškrtnuté. Další informace o této funkci najdete v souboru nápovědy.
11.  Klikněte na tlačítko Rozšířené... pro kontrolu upřesňujícího nastavení, včetně schopnosti výběru vlastního soubor  odmítnutí stránky, pro ukládání úspěšných ověření na webu do mezipaměti po dobu používání souborů cookie a vyberte, zda chcete ověřit primární pověření vůči doméně systému Windows, adresáři LDAP nebo serveru RADIUS. Po dokončení klikněte na tlačítko OK se vraťte do dialogového okna Přidat web založený na formuláři. Další informace o rozšířeném nastavení naleznete v souboru nápovědy.
12. Klikněte na tlačítko OK.
13. Po zjištění nebo zadání Adresy URL a proměnných hodnot stránek  se zobrazí data webové stránky v panelu založeném na formulářích.
14. V tématu povolit moduly plug-in IIS pro server Azure Multi-Factor Authentication přímo pod tématem dokončete ověřování konfigurace služby IIS.

## Používání integrovaného ověřování systému Windows pomocí serveru Azure Multi-Factor Authentication Server

Pro zabezpečení webové aplikace služby IIS, která používá integrované HTTP ověřování systému Windows, nainstalujte server Azure Multi-Factor Authentication na webový server služby IIS a nakonfigurujte server podle následujícího postupu.

1. V rámci serveru Azure Multi-Factor Authentication klikněte na ikonu ověření služby IIS v levé nabídce.
2. Klikněte na kartu protokolu HTTP. Klikněte na kartu Založené na formulářích.
3. Klikněte na tlačítko Přidat... .
4. V dialogovém okně Přidat základní adresu URL zadejte adresu URL pro webovou stránku, kde se provádí ověřování HTTP (např. http://localhost/owa) do pole Základní adresa URL pro web a zadejte název aplikace (volitelné). Název aplikace se zobrazí v sestavách Azure Multi-Factor Authentication a může se zobrazit v rámci SMS zpráv nebo mobilních aplikací ověřování.
5. Pokud je výchozí hodnota nedostatečná, upravte časový limit nečinnosti a maximální dobu trvání relace.
6. Zaškrtněte políčko Vyžadovat uživatele Multi-Factor Authentication, pokud byli nebo budou všichni uživatelé importováni do serveru a podstoupí Multi-Factor Authentication. Pokud dosud nebyl importován velký počet uživatelů do serveru a/nebo budou vyloučeni z Multi-Factor Authentication, nechejte pole nezaškrtnuté. Další informace o této funkci najdete v souboru nápovědy.
7. V případě potřeby zaškrtněte políčko mezipaměti souborů cookie.
8. Klikněte na tlačítko OK.
9. V tématu [povolit moduly plug-in IIS pro server Azure Multi-Factor Authentication](#enable-iis-plug-ins-for-azure-multi-factor-authentication-server) přímo pod tématem dokončete ověřování konfigurace služby IIS.


## Povolit moduly plug-in služby IIS a server Azure Multi-Factor Authentication

Po konfiguraci adres URL založených na formulářích nebo adres URL protokolu HTTP ověřování a nastavení musíte vybrat místo, kam by měly být moduly plug-in Azure Multi-Factor Authentication IIS načteny a povoleny ve službě IIS. Použijte následující postup:

1. Pokud používáte službu IIS 6, klikněte na kartu ISAPI a vyberte web, který webová aplikace spouští pod (například výchozí web), chcete-li povolit filtr Azure Multi-Factor Authentication ISAPI modulů plug-in pro tuto lokalitu.
2. Pokud používáte službu IIS 7 nebo vyšší, klikněte na kartu Nativní modul a vyberte server, web nebo aplikaci, pro které chcete povolit modul plug-in služby IIS na požadované úrovni.
3. Klikněte na políčko Povolit IIS ověřování v horní části obrazovky. Azure Multi-Factor Authentication nyní zabezpečuje vybrané aplikace služby IIS. Ověřte, zda byli uživatelé naimportováni do serveru. Pokud chcete vytvořit bílou listinu interních IP adres, aby nebylo třeba dvoufaktorové ověřování při přihlašování k webu z těchto umístění, projděte si část Důvěryhodné IP adresy níže.


## Důvěryhodné IP adresy

Důvěryhodné IP adresy umožňují uživatelům obejít ověřování Azure Multi-Factor Authentication u požadavků webů pocházejících z konkrétní IP adresy nebo podsítě. Například můžete chtít vyloučit uživatele z ověřování Azure Multi-Factor Authentication při přihlašování z kanceláře. V takovém případě zadáte podsíť office jako položku důvěryhodné adresy IP. Konfigurace důvěryhodných IP adres pomocí následujícího postupu:

1. V části ověření služby IIS klikněte na kartu Důvěryhodné IP adresy.
2. Klikněte na tlačítko Přidat... .
3. Jakmile se zobrazí dialogové okno Přidat důvěryhodné IP adresy, vyberte jednu IP adresu, rozsah IP adres nebo přepínač podsítě.
4. Zadejte IP adresu, rozsah IP adres nebo podsíť, které chcete zařadit na seznam povolených adres. Pokud zadáváte podsíť, vyberte příslušnou síťovou masku a klikněte na tlačítko OK. Seznam povolených adres byl přidán.



<!--HONumber=Sep16_HO3-->


