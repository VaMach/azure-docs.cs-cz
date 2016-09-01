<properties
    pageTitle="Začínáme s webovými aplikacemi Node.js ve službě Azure App Service"
    description="Naučte se nasadit aplikaci Node.js do webové aplikace ve službě Azure App Service."
    services="app-service\web"
    documentationCenter="nodejs"
    authors="cephalin"
    manager="wpickett"
    editor=""/>

<tags
    ms.service="app-service-web"
    ms.workload="web"
    ms.tgt_pltfrm="na"
    ms.devlang="nodejs"
    ms.topic="get-started-article"
    ms.date="07/01/2016"
    ms.author="cephalin"/>

# Začínáme s webovými aplikacemi Node.js ve službě Azure App Service

[AZURE.INCLUDE [tabs](../../includes/app-service-web-get-started-nav-tabs.md)]

Tento kurz ukazuje, jak vytvořit jednoduchou aplikaci [Node.js][NODEJS] a nasadit ji do [web app] ve službě [Azure App Service] z prostředí příkazového řádku, jako je například cmd.exe nebo bash. Pokyny v tomto kurzu platí pro všechny operační systémy, které podporují Node.js.

<a name="prereq"></a>
## Požadavky

- **Node.js** ([Nainstalujete kliknutím sem.][NODEJS])
- **Bower** ([Nainstalujete kliknutím sem.][BOWER])
- **Yeoman** ([Nainstalujete kliknutím sem.][YEOMAN])
- **Git** ([Nainstalujete kliknutím sem.][GIT])
- **Azure CLI** ([Nainstalujete kliknutím sem.][Azure CLI])
- Účet Microsoft Azure. Pokud nemáte účet, můžete se [zaregistrovat k bezplatné zkušební verzi] nebo si [aktivovat výhody předplatitele Visual Studio].

## Vytvoření a nasazení jednoduché webové aplikace Node.js

1. Otevřete zvolený terminál příkazového řádku a nainstalujte [Generátor Express pro Yeoman].

        npm install -g generator-express

2. `CD` do pracovního adresáře a pomocí následující syntaxe vygenerujte expresní aplikaci:

        yo express
        
    Po zobrazení výzvy vyberte následující možnosti:  

    `? Would you like to create a new directory for your project?` **Ano**  
    `? Enter directory name` **{názevaplikace}**  
    `? Select a version to install:` **MVC**  
    `? Select a view engine to use:` **Jade**  
    `? Select a css preprocessor to use (Sass Requires Ruby):` **Žádný**  
    `? Select a database to use:` **Žádný**  
    `? Select a build tool to use:` **Grunt**

3. `CD` do kořenového adresáře nové aplikace, spusťte ji a ujistěte se, zda běží ve vašem vývojovém prostředí:

        npm start

    V prohlížeči přejděte na adresu <http://localhost:3000> a ujistěte se, zda se zobrazí domovská stránka Express. Jakmile ověříte, že aplikace běží správně, zastavte ji pomocí `Ctrl-C`.
    
1. Přihlaste se k Azure takto (je k tomu zapotřebí rozhraní příkazového řádku [Azure CLI](#prereq)):

        azure login

    Postupujte podle výzvy a pokračujte v přihlášení v prohlížeči pomocí účtu Microsoft, který obsahuje předplatné Azure.

2. Ujistěte se, zda se stále nacházíte v kořenovém adresáři aplikace, a poté následujícím příkazem vytvořte prostředek aplikace služby App Service v Azure s jedinečným názvem aplikace; například: http://{názevaplikace}.azurewebsites.net

        azure site create --git {appname}

    Po zobrazení výzvy vyberte oblast Azure, do které chcete provést nasazení. Pokud jste u předplatného Azure dosud nenastavili přihlašovací údaje pro nasazení Git/FTP, budete rovněž vyzváni k jejich vytvoření.

3. Z kořenového adresáře aplikace otevřete soubor ./config/config.js a změňte produkční port na možnost `process.env.port`. Vlastnost `production` v objektu `config` by měla odpovídat následujícímu příkladu.

        production: {
            root: rootPath,
            app: {
                name: 'express1'
            },
            port: process.env.port,
        }

    Aplikaci Node.js to umožní reagovat na webové požadavky na výchozím portu, jemuž modul iisnode naslouchá.
    
4. Uložte změny a poté pomocí Git nasaďte aplikaci do Azure:

        git add .
        git commit -m "{your commit message}"
        git push azure master

    Generátor Express již poskytuje soubor .gitignore, takže `git push` nebude využívat šířku pásma k nahrání node_modules / adresáře.

5. Nakonec spusťte živou aplikaci Azure v prohlížeči:

        azure site browse

    Nyní byste měli vidět živý běh webové aplikace Node.js ve službě Azure App Service.
    
    ![Příklad procházení k nasazené aplikaci.][deployed-express-app]

## Aktualizace webové aplikace Node.js

Chcete-li provést aktualizace webové aplikace Node.js spuštěné ve službě App Service, stačí spustit `git add`, `git commit` a `git push` stejně jako při prvním nasazení webové aplikace.
     
## Postup nasazení aplikace Node.js službou App Service

Služba Azure App Service spouští aplikace Node.js pomocí modulu [iisnode]. Rozhraní příkazového řádku Azure CLI a modul Kudu (nasazení Git) společně umožňují efektivnější práci uživatelů při vývoji a nasazení aplikací Node.js z příkazového řádku. 

- `azure site create --git` rozpozná běžný vzor Node.js u server.js nebo app.js a vytvoří v kořenovém adresáři soubor iisnode.yml. Tento soubor můžete použít k přizpůsobení modulu iisnode.
- Při použití `git push azure master` modul Kudu automatizuje následující úkoly nasazení:

    - Pokud se v kořenovém adresáři úložiště nachází soubor package.json, spusťte `npm install --production`.
    - Vygenerujte soubor Web.config pro modul iisnode, který odkazuje na spouštěcí skript v souboru package.json (např. server.js nebo app.js).
    - Přizpůsobením souboru Web.config připravte aplikaci k ladění pomocí nástroje Node-Inspector.
    
## Použití rozhraní Node.js

Používáte-li k vývoji aplikací některé z oblíbených rozhraní Node.js, například [Sails.js][SAILSJS] nebo [MEAN.js][MEANJS], můžete je nasadit do služby App Service. Oblíbená rozhraní Node.js mají své specifické zvláštnosti, přičemž závislosti jejich balíčků jsou neustále aktualizovány. Služba App Service však poskytuje protokoly stdout a stderr, díky nimž přesně víte, co se s aplikací děje, a můžete provádět odpovídající změny. Další informace naleznete v tématu [Získání protokolů stdout a stderr z modulu iisnode](#iisnodelog).

Následující kurzy vás seznámí s tím, jak pracovat s konkrétním rozhraním ve službě App Service:

- [Nasazení webové aplikace Sails.js do služby Azure App Service]
- [Vytvoření chatovací aplikace Node.js pomocí Socket.IO ve službě Azure App Service]
- [Použití io.js s aplikacemi Azure App Service Web Apps]

## Použití konkrétního modulu Node.js

Při obvyklém pracovním postupu můžete službě App Service říci, aby používala konkrétní modul Node.js, a to stejným způsobem, jakým byste běžně postupovali v package.json.
Příklad:

    "engines": {
        "node": "5.5.0"
    }, 

Modul nasazení Kudu určí, který modul Node.js se má použít, a to v následujícím pořadí:

- Nejprve zjišťuje, zda je v souboru iisnode.yml zadána položka `nodeProcessCommandLine`. Pokud ano, použije ji.
- Poté zjišťuje, zda je v souboru package.json zadána položka `"node": "..."` v objektu `engines`. Pokud ano, použije ji.
- Ve výchozím nastavení vybere výchozí Node.js.

<a name="iisnodelog"></a>
## Získání protokolů stdout a stderr z modulu iisnode

Chcete-li číst protokoly modulu iisnode, použijte následující postup.

> [AZURE.NOTE] Po dokončení tohoto postupu nemusí soubory protokolů existovat, dokud nedojde k chybě.

1. Otevřete soubor iisnode.yml poskytovaný rozhraním příkazového řádku Azure CLI.

2. Nastavte následující dva parametry: 

        loggingEnabled: true
        logDirectory: iisnode
    
    Tyto parametry společně dávají modulu iisnode ve službě App Service pokyn, aby ukládal výstupy stdout a stderror do adresáře D:\home\site\wwwroot\**iisnode**.

3. Uložte změny a poté tyto změny nuceně vložte (push) do Azure pomocí následujících příkazů Git:

        git add .
        git commit -m "{your commit message}"
        git push azure master
   
   Modul iisnode je nyní nakonfigurován. Následující kroky ukazují, jak k těmto protokolům získat přístup.
     
4. Otevřete v prohlížeči konzolu pro ladění Kudu pro aplikaci, která se nachází na adrese:

        https://{appname}.scm.azurewebsites.net/DebugConsole 

    Upozorňujeme, že tato adresa URL se liší od adresy URL webové aplikace, neboť rozšiřuje název DNS o řetězec „*.scm“.* . Pokud adresu URL takto nerozšíříte, obdržíte chybu 404.

5. Přejděte do adresáře D:\home\site\wwwroot\iisnode

    ![Navigace k umístění souborů protokolů modulu iisnode.][iislog-kudu-console-find]

6. Klikněte na ikonu **Upravit** u protokolu, který chcete číst. Dle potřeby také můžete kliknout na položku **Stáhnout** nebo **Odstranit**.

    ![Otevření souboru protokolu modulu iisnode.][iislog-kudu-console-open]

    Nyní je zobrazen protokol, který vám pomůže při ladění nasazení ve službě App Service.
    
    ![Prohlížení souboru protokolu modulu iisnode.][iislog-kudu-console-read]

## Ladění aplikace pomocí nástroje Node-Inspector

Používáte-li k ladění aplikací Node.js nástroj Node-Inspector, můžete jej použít pro živou aplikaci služby App Service. Nástroj Node-Inspector je předinstalován v instalaci modulu iisnode pro službu App Service. A pokud nasazujete prostřednictvím Git, automaticky generovaný soubor Web.config z modulu Kudu již obsahuje veškeré konfigurace nezbytné k povolení nástroje Node-Inspector.

Chcete-li povolit nástroj Node-Inspector, postupujte takto:

1. Otevřete soubor iisnode.yml v kořenovém adresáři úložiště a zadejte následující parametry: 

        debuggingEnabled: true
        debuggerExtensionDll: iisnode-inspector.dll

3. Uložte změny a poté tyto změny nuceně vložte (push) do Azure pomocí následujících příkazů Git:

        git add .
        git commit -m "{your commit message}"
        git push azure master
   
4. Nyní jednoduše přejděte k spouštěcímu souboru aplikace, který je určen spouštěcím skriptem v souboru package.json, přičemž příslušnou adresu URL doplňte o /debug. Například:

        http://{appname}.azurewebsites.net/server.js/debug
    
    Nebo:
    
        http://{appname}.azurewebsites.net/app.js/debug

## Další zdroje informací

- [Určení verze Node.js v aplikaci Azure](../nodejs-specify-node-version-azure-apps.md)
- [Osvědčené postupy a průvodci odstraňováním potíží aplikace Node.js v Azure](app-service-web-nodejs-best-practices-and-troubleshoot-guide.md)
- [Postup ladění webové aplikace Node.js ve službě Azure App Service](web-sites-nodejs-debug.md)
- [Používání modulů Node.js s aplikacemi Azure](../nodejs-use-node-modules-azure-apps.md)
- [Azure App Service Web Apps: Node.js](http://blogs.msdn.com/b/silverlining/archive/2012/06/14/windows-azure-websites-node-js.aspx)
- [Středisko pro vývojáře Node.js](/develop/nodejs/)
- [Začínáme s webovými aplikacemi ve službě Azure App Service](app-service-web-get-started.md)
- [Seznámení se super tajnou konzolou pro ladění modulu Kudu]

<!-- URL List -->

[Azure CLI]: ../xplat-cli-install.md
[Azure App Service]: ../app-service/app-service-value-prop-what-is.md
[aktivovat výhody předplatitele Visual Studio]: http://go.microsoft.com/fwlink/?LinkId=623901
[BOWER]: http://bower.io/
[Vytvoření chatovací aplikace Node.js pomocí Socket.IO ve službě Azure App Service]: ./web-sites-nodejs-chat-app-socketio.md
[Nasazení webové aplikace Sails.js do služby Azure App Service]: ./app-service-web-nodejs-sails.md
[Seznámení se super tajnou konzolou pro ladění modulu Kudu]: /documentation/videos/super-secret-kudu-debug-console-for-azure-web-sites/
[Generátor Express pro Yeoman]: https://github.com/petecoop/generator-express
[GIT]: http://www.git-scm.com/downloads
[Použití io.js s aplikacemi Azure App Service Web Apps]: ./web-sites-nodejs-iojs.md
[iisnode]: https://github.com/tjanczuk/iisnode/wiki
[MEANJS]: http://meanjs.org/
[NODEJS]: http://nodejs.org
[SAILSJS]: http://sailsjs.org/
[zaregistrovat k bezplatné zkušební verzi]: http://go.microsoft.com/fwlink/?LinkId=623901
[web app]: ./app-service-web-overview.md
[YEOMAN]: http://yeoman.io/

<!-- IMG List -->

[deployed-express-app]: ./media/app-service-web-nodejs-get-started/deployed-express-app.png
[iislog-kudu-console-find]: ./media/app-service-web-nodejs-get-started/iislog-kudu-console-navigate.png
[iislog-kudu-console-open]: ./media/app-service-web-nodejs-get-started/iislog-kudu-console-open.png
[iislog-kudu-console-read]: ./media/app-service-web-nodejs-get-started/iislog-kudu-console-read.png



<!---HONumber=Aug16_HO4-->


