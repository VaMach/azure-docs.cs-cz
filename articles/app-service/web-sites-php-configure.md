---
title: "Nakonfigurovat PHP ve službě Azure App Service Web Apps | Microsoft Docs"
description: "Zjistěte, jak nakonfigurovat výchozí instalace PHP nebo přidat vlastní instalace PHP pro webové aplikace v Azure App Service."
services: app-service
documentationcenter: php
author: rmcmurray
manager: erikre
editor: 
ms.assetid: 95c4072b-8570-496b-9c48-ee21a223fb60
ms.service: app-service
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: PHP
ms.topic: article
ms.date: 04/25/2017
ms.author: robmcm
ms.openlocfilehash: 624dd416f37aacdb3d2f6e59afdc2efe646e610b
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/11/2017
---
# <a name="configure-php-in-azure-app-service-web-apps"></a>Nakonfigurovat PHP ve službě Azure App Service Web Apps
## <a name="introduction"></a>Úvod
Tento průvodce vám ukáže, jak konfigurovat předdefinované modulu PHP runtime pro webové aplikace v [Azure App Service](http://go.microsoft.com/fwlink/?LinkId=529714)zadejte vlastní modul runtime PHP a povolit rozšíření. Pokud chcete používat služby App Service, zaregistrujte si [bezplatnou zkušební verzi]. Pokud chcete nejvíce z této příručky, měli nejprve vytvořit webové aplikace PHP ve službě App Service.

[!INCLUDE [app-service-web-to-api-and-mobile](../../includes/app-service-web-to-api-and-mobile.md)]

## <a name="how-to-change-the-built-in-php-version"></a>Postupy: Změna předdefinované verzi PHP
Ve výchozím nastavení PHP 5.5 je nainstalovaná a okamžitě k dispozici pro použití při vytváření webové aplikace služby App Service. Nejlepší způsob, jak zobrazit dostupné verze revize, jeho výchozí konfigurace a povolené rozšíření je pro nasazení skript, který volá [phpinfo()] funkce.

Verze PHP 5.6 a PHP 7.0 jsou také k dispozici, ale není povoleno ve výchozím nastavení. Pokud chcete aktualizovat verzi PHP, postupujte podle jednu z těchto metod:

### <a name="azure-portal"></a>Azure Portal
1. Přejděte do webové aplikace v [portálu Azure](https://portal.azure.com) a klikněte na **nastavení** tlačítko.
   
    ![Nastavení webové aplikace][settings-button]
2. Z **nastavení** okně vyberte **nastavení aplikace** a vyberte novou verzi PHP.
   
    ![Nastavení aplikace][application-settings]
3. Klikněte **Uložit** tlačítka v horní části **webové nastavení aplikace** okno.
   
    ![Uložit nastavení konfigurace][save-button]

### <a name="azure-powershell-windows"></a>Prostředí Azure PowerShell (Windows)
1. Otevřete prostředí Azure PowerShell a přihlásit k účtu:
   
        PS C:\> Login-AzureRmAccount
2. Nastavte verzi PHP pro webovou aplikaci.
   
        PS C:\> Set-AzureWebsite -PhpVersion {5.5 | 5.6 | 7.0} -Name {app-name}
3. Verze PHP je teď nastavená. Můžete potvrdit, tato nastavení:
   
        PS C:\> Get-AzureWebsite -Name {app-name} | findstr PhpVersion

### <a name="azure-command-line-interface-linux-mac-windows"></a>Rozhraní příkazového řádku Azure (Linux, Mac, Windows)
Chcete-li používat rozhraní příkazového řádku Azure, musíte mít **Node.js** v počítači nainstalována.

1. Otevřete terminál a přihlášení k vašemu účtu.
   
        azure login
2. Nastavte verzi PHP pro webovou aplikaci.
   
        azure site set --php-version {5.5 | 5.6 | 7.0} {app-name}

3. Verze PHP je teď nastavená. Můžete potvrdit, tato nastavení:
   
        azure site show {app-name}

> [!NOTE] 
> [Azure CLI 2.0](https://github.com/Azure/azure-cli) jsou příkazy, které jsou ekvivalentem výše:
>
>

    az login
    az appservice web config update --php-version {5.5 | 5.6 | 7.0} -g {resource-group-name} -n {app-name}
    az appservice web config show -g {resource-group-name} -n {app-name}

## <a name="how-to-change-the-built-in-php-configurations"></a>Postupy: Změna předdefinovaných konfigurací PHP
Pro žádné předdefinované modul runtime PHP můžete změnit některé možnosti konfigurace podle následujících kroků. (Informace o souboru php.ini direktivy najdete v tématu [seznam php.ini direktivy].)

### <a name="changing-phpiniuser-phpiniperdir-phpiniall-configuration-settings"></a>Změna PHP\_INI\_uživatele, PHP\_INI\_PERDIR, PHP\_INI\_všechna nastavení konfigurace
1. Přidat [. user.ini] soubor pro kořenový adresář.
2. Přidat nastavení konfigurace `.user.ini` pomocí stejnou syntaxi, kterou použijete v souboru `php.ini` souboru. Například, pokud chcete zapnout `display_errors` nastavení a nastavte `upload_max_filesize` nastavení na 10 MB, vaše `.user.ini` soubor bude obsahovat tento text:
   
        ; Example Settings
        display_errors=On
        upload_max_filesize=10M
   
        ; OPTIONAL: Turn this on to write errors to d:\home\LogFiles\php_errors.log
        ; log_errors=On
3. Nasazení webové aplikace.
4. Restartování webové aplikace. (Restartování je nutné, protože frekvenci, se kterou PHP čte `.user.ini` se řídí soubory `user_ini.cache_ttl` nastavení, což je úrovně nastavení systému a ve výchozím nastavení je 300 sekund (5 minut). Restartování webové aplikace PHP číst nové nastavení vynutí `.user.ini` souboru.)

Jako alternativu k použití `.user.ini` souboru, můžete použít [ini_set()] funkce ve skriptech, chcete-li nastavit možnosti konfigurace, které nejsou direktivy úrovni systému.

### <a name="changing-phpinisystem-configuration-settings"></a>Změna PHP\_INI\_nastavení konfigurace systému
1. Přidat nastavení aplikace do webové aplikace s klíčem `PHP_INI_SCAN_DIR` a hodnotu`d:\home\site\ini`
2. Vytvoření `settings.ini` souboru pomocí konzole Kudu (http://&lt;název lokality&gt;. scm.azurewebsite.net) v `d:\home\site\ini` directory.
3. Přidat nastavení konfigurace `settings.ini` soubor pomocí stejné syntaxe byste použili v souboru php.ini. Například, pokud jste chtěli bodu `curl.cainfo` nastavení `*.crt` souborů a nastavení 'wincache.maxfilesize' na 512 kB, vaše `settings.ini` soubor bude obsahovat tento text:
   
        ; Example Settings
        curl.cainfo="%ProgramFiles(x86)%\Git\bin\curl-ca-bundle.crt"
        wincache.maxfilesize=512
4. Restartování webové aplikace načíst změny.

## <a name="how-to-enable-extensions-in-the-default-php-runtime"></a>Postupy: povolení rozšíření v modulu runtime PHP výchozí
Jak je uvedeno v předchozí části, je nejlepší způsob, jak zobrazit výchozí verze PHP, jeho výchozí konfigurace a povolené rozšíření nasazení skriptu, který volá [phpinfo()]. Povolit další rozšíření, postupujte podle následujících kroků:

### <a name="configure-via-ini-settings"></a>Konfigurovat pomocí nastavení ini
1. Přidat `ext` do adresáře `d:\home\site` adresáře.
2. Uveďte `.dll` soubory s příponou `ext` directory (například `php_xdebug.dll`). Ujistěte se, že rozšíření jsou kompatibilní se výchozí verze PHP a jsou VC9 a kompatibilní bez bezpečného přístupu (nts).
3. Přidat nastavení aplikace do webové aplikace s klíčem `PHP_INI_SCAN_DIR` a hodnotu`d:\home\site\ini`
4. Vytvoření `ini` souboru v `d:\home\site\ini` názvem `extensions.ini`.
5. Přidat nastavení konfigurace `extensions.ini` soubor pomocí stejné syntaxe byste použili v souboru php.ini. Například, pokud chcete povolit rozšíření MongoDB a nástroje XDebug vaše `extensions.ini` soubor bude obsahovat tento text:
   
        ; Enable Extensions
        extension=d:\home\site\ext\php_mongo.dll
        zend_extension=d:\home\site\ext\php_xdebug.dll
6. Restartování webové aplikace načíst změny.

### <a name="configure-via-app-setting"></a>Konfigurovat pomocí nastavení aplikace
1. Přidat `bin` adresář na kořenový adresář.
2. Uveďte `.dll` soubory s příponou `bin` directory (například `php_xdebug.dll`). Ujistěte se, že rozšíření jsou kompatibilní se výchozí verze PHP a jsou VC9 a kompatibilní bez bezpečného přístupu (nts).
3. Nasazení webové aplikace.
4. Přejděte na webovou aplikaci na portálu Azure a klikněte na **nastavení** tlačítko.
   
    ![Nastavení webové aplikace][settings-button]
5. Z **nastavení** okně vyberte **nastavení aplikace** a přejděte k položce **nastavení aplikace** části.
6. V **nastavení aplikace** , vytvořte **PHP_EXTENSIONS** klíč. Hodnota tohoto klíče by být cesta relativní vůči kořenovému adresáři webu: **bin\your přípona souboru**.
   
    ![Povolit rozšíření v nastavení aplikace][php-extensions]
7. Klikněte **Uložit** tlačítka v horní části **webové nastavení aplikace** okno.
   
    ![Uložit nastavení konfigurace][save-button]

Zend rozšíření jsou podporovány také pomocí **PHP_ZENDEXTENSIONS** klíč. Chcete-li několik rozšíření, zahrnují obsahuje čárkami oddělený seznam `.dll` soubory pro hodnotu nastavení aplikace.

## <a name="how-to-use-a-custom-php-runtime"></a>Postupy: použití vlastního modulu PHP runtime
App Service Web Apps můžete místo výchozí modul runtime PHP, použít modul runtime PHP, který zadáte ke spuštění skripty PHP. Dají se nakonfigurovat modul runtime, který zadáte `php.ini` soubor, který je taky zadat. Vlastní modul runtime PHP s webovými aplikacemi, postupujte podle následujících kroků.

1. Získejte není bezpečná pro přístup z více vláken, VC9 nebo VC11 kompatibilní verzi PHP pro systém Windows. Poslední verze PHP pro systém Windows naleznete zde: [http://windows.php.net/download/]. Starší verze najdete v archivu zde: [http://windows.php.net/downloads/releases/archives/].
2. Změnit `php.ini` soubor pro vaše runtime. Všimněte si, že všechna nastavení, které jsou systému úroveň jen direktivy budou ignorovány webové aplikace. (Informace o systému úroveň jen direktivy najdete v tématu [seznam php.ini direktivy]).
3. Volitelně můžete přidat rozšíření do vaší runtime PHP a povolte je do `php.ini` souboru.
4. Přidat `bin` do kořenového adresáře a put adresář, který obsahuje vaše modulu PHP runtime v ní adresáře (například `bin\php`).
5. Nasazení webové aplikace.
6. Přejděte na webovou aplikaci na portálu Azure a klikněte na **nastavení** tlačítko.
   
    ![Nastavení webové aplikace][settings-button]
7. Z **nastavení** okně vyberte **nastavení aplikace** a přejděte k položce **mapování obslužných rutin** části. Přidat `*.php` rozšíření pole a přidejte cestu k `php-cgi.exe` spustitelný soubor. Když vložíte vaší modulu PHP runtime `bin` adresáře v kořenovém adresáři aplikace, bude cesta `D:\home\site\wwwroot\bin\php\php-cgi.exe`.
   
    ![Zadejte rutinu mapování obslužné rutiny][handler-mappings]
8. Klikněte **Uložit** tlačítka v horní části **webové nastavení aplikace** okno.
   
    ![Uložit nastavení konfigurace][save-button]

<a name="composer" />

## <a name="how-to-enable-composer-automation-in-azure"></a>Postupy: povolení autora automation v Azure
Ve výchozím nastavení služby App Service nic se neděje s composer.json, pokud máte ve vašem projektu PHP. Pokud používáte [nasazení Git](app-service-deploy-local-git.md), můžete povolit composer.json zpracování během `git push` povolením rozšíření autora.

> [!NOTE]
> Můžete [hlas podpora prvotřídní autora ve službě App Service zde](https://feedback.azure.com/forums/169385-web-apps-formerly-websites/suggestions/6477437-first-class-support-for-composer-and-pip)!
> 
> 

1. Ve vašem PHP webové okně aplikace v [portál Azure](https://portal.azure.com), klikněte na tlačítko **nástroje** > **rozšíření**.
   
    ![Okno nastavení Azure Portal povolit autora automation v Azure](./media/web-sites-php-configure/composer-extension-settings.png)
2. Klikněte na tlačítko **přidat**, pak klikněte na tlačítko **autora**.
   
    ![Přidání rozšíření autora jak povolit automatizaci autora v Azure](./media/web-sites-php-configure/composer-extension-add.png)
3. Klikněte na tlačítko **OK** přijměte právní podmínky. Klikněte na tlačítko **OK** přidat rozšíření.
   
    **Nainstalovaná rozšíření** okno se nyní zobrazí rozšíření autora.  
    ![Přijměte právní podmínky, jak povolit automatizaci autora v Azure](./media/web-sites-php-configure/composer-extension-view.png)
4. Teď, provádět `git add`, `git commit`, a `git push` jako v předchozí části. Nyní se zobrazí, autora je instalování závislostí, které jsou definované v composer.json.
   
    ![Nasazení Git s autora automation v Azure](./media/web-sites-php-configure/composer-extension-success.png)

## <a name="next-steps"></a>Další kroky
Další informace najdete v tématu [středisku pro vývojáře PHP](/develop/php/).

> [!NOTE]
> Pokud chcete začít používat Azure App Service před registrací účtu Azure, přejděte k [možnosti vyzkoušet si App Service](https://azure.microsoft.com/try/app-service/), kde si můžete hned vytvořit krátkodobou úvodní webovou aplikaci. Nevyžaduje se žádná platební karta a nevzniká žádný závazek.
> 
> 

[bezplatnou zkušební verzi]: https://www.windowsazure.com/pricing/free-trial/
[phpinfo()]: http://php.net/manual/en/function.phpinfo.php
[select-php-version]: ./media/web-sites-php-configure/select-php-version.png
[seznam php.ini direktivy]: http://www.php.net/manual/en/ini.list.php
[. user.ini]: http://www.php.net/manual/en/configuration.file.per-user.php
[ini_set()]: http://www.php.net/manual/en/function.ini-set.php
[application-settings]: ./media/web-sites-php-configure/application-settings.png
[settings-button]: ./media/web-sites-php-configure/settings-button.png
[save-button]: ./media/web-sites-php-configure/save-button.png
[php-extensions]: ./media/web-sites-php-configure/php-extensions.png
[handler-mappings]: ./media/web-sites-php-configure/handler-mappings.png
[http://windows.php.net/download/]: http://windows.php.net/download/
[http://windows.php.net/downloads/releases/archives/]: http://windows.php.net/downloads/releases/archives/
[SETPHPVERCLI]: ./media/web-sites-php-configure/ChangePHPVersion-XPlatCLI.png
[GETPHPVERCLI]: ./media/web-sites-php-configure/ShowPHPVersion-XplatCLI.png
[SETPHPVERPS]: ./media/web-sites-php-configure/ChangePHPVersion-PS.png
[GETPHPVERPS]: ./media/web-sites-php-configure/ShowPHPVersion-PS.png

