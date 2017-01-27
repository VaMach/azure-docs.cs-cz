---
title: "Začínáme s webovými aplikacemi Node.js ve službě Azure App Service | Dokumentace Microsoftu"
description: "Naučte se nasadit aplikaci Node.js do webové aplikace ve službě Azure App Service."
services: app-service\web
documentationcenter: nodejs
author: cephalin
manager: wpickett
editor: 
ms.assetid: fb2b90c8-02b6-4700-929b-5de9a35d67cc
ms.service: app-service-web
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: nodejs
ms.topic: get-started-article
ms.date: 12/16/2016
ms.author: cephalin
translationtype: Human Translation
ms.sourcegitcommit: 88405a9e67eb748acc9564022283004b5ebfcf48
ms.openlocfilehash: 63210a5539d1e5e5b7d1f5a60048d507e53038a5


---
# <a name="get-started-with-nodejs-web-apps-in-azure-app-service"></a>Začínáme s webovými aplikacemi Node.js ve službě Azure App Service
[!INCLUDE [tabs](../../includes/app-service-web-get-started-nav-tabs.md)]

Tento kurz ukazuje, jak vytvořit jednoduchou aplikaci [Node.js] a nasadit ji do služby [Azure App Service] z prostředí příkazového řádku, jako je například cmd.exe nebo bash. Pokyny v tomto kurzu platí pro všechny operační systémy, které podporují Node.js.

[!INCLUDE [app-service-linux](../../includes/app-service-linux.md)]

<a name="prereq"></a>

## <a name="cli-versions-to-complete-the-task"></a>Verze rozhraní příkazového řádku pro dokončení úlohy

K dokončení úlohy můžete využít jednu z následujících verzí rozhraní příkazového řádku:

- [Azure CLI 1.0](app-service-web-nodejs-get-started-cli-nodejs.md) – naše rozhraní příkazového řádku pro klasické modely nasazení a modely nasazení správy prostředků
- [Azure CLI 2.0 (Preview)](app-service-web-nodejs-get-started.md) – naše rozhraní příkazového řádku nové generace pro model nasazení správy prostředků

## <a name="prerequisites"></a>Požadavky
* [Node.js]
* [Bower]
* [Yeoman]
* [Git]
* [Azure CLI 2.0 (Preview)](/cli/azure/install-az-cli2)
* Účet Microsoft Azure. Pokud nemáte účet, můžete se [zaregistrovat k bezplatné zkušební verzi] nebo si [aktivovat výhody předplatitele sady Visual Studio].

> [!NOTE]
> [App Service si můžete vyzkoušet](http://go.microsoft.com/fwlink/?LinkId=523751) bez účtu Azure. Můžete si vytvořit úvodní aplikaci a celou hodinu si s ní hrát, bez platebních karet a bez závazků.
> 
> 

## <a name="create-and-configure-a-simple-nodejs-app-for-azure"></a>Vytvoření a konfigurace jednoduché aplikace Node.js pro Azure
1. Otevřete zvolený terminál příkazového řádku a nainstalujte [Generátor Express pro Yeoman].
   
        npm install -g generator-express

2. `CD`do pracovního adresáře a pomocí následující syntaxe vygenerujte expresní aplikaci:
   
        yo express
   
    Po zobrazení výzvy vyberte následující možnosti:  
   
    `? Would you like to create a new directory for your project?` **Ano**  
    `? Enter directory name`**{název_aplikace}**  
    `? Select a version to install:` **MVC**  
    `? Select a view engine to use:` **Jade**  
    `? Select a css preprocessor to use (Sass Requires Ruby):` **Žádný**  
    `? Select a database to use:` **Žádný**  
    `? Select a build tool to use:` **Grunt**

3. `CD`do kořenového adresáře nové aplikace, spusťte ji a ujistěte se, že běží ve vašem vývojovém prostředí:
   
        npm start
   
    V prohlížeči přejděte na adresu <http://localhost:3000> a ujistěte se, zda se zobrazí domovská stránka Express. Jakmile ověříte, že aplikace běží správně, zastavte ji pomocí `Ctrl-C`.

6. Z kořenového adresáře aplikace otevřete soubor ./config/config.js a změňte produkční port na možnost `process.env.port`. Vlastnost `production` v objektu `config` by měla odpovídat následujícímu příkladu:
   
        production: {
            root: rootPath,
            app: {
                name: 'express1'
            },
            port: process.env.port,
        }
   
    > [!NOTE] 
    > Azure App Service ve výchozím nastavení spustí aplikaci Node.js s proměnnými prostředí `production` (`process.env.NODE_ENV="production"`.
    > Tato konfigurace aplikaci Node.js na Azure umožní reagovat na webové požadavky na výchozím portu, jemuž modul iisnode naslouchá.
    >
    >

7. Otevřete soubor ./package.json a přidejte vlastnost `engines` do části [zadejte požadovanou verzi Node.js](#version).
   
        "engines": {
            "node": "6.9.1"
        }, 

8. Uložte změny a inicializujte úložiště GIT v kořeni aplikace. Potom spusťte kód:
   
        git add .
        git add -f config
        git commit -m "{your commit message}"

## <a name="deploy-your-nodejs-app-to-azure"></a>Nasazení aplikace Node.js do Azure

1. Přihlaste se k Azure (potřebujete [Azure CLI 2.0 Preview](#prereq)):
   
        az login
   
    Postupujte podle výzvy a pokračujte v přihlášení v prohlížeči pomocí účtu Microsoft, který obsahuje předplatné Azure.

3. Nastavte uživatele nasazení pro App Service. Později pomocí těchto přihlašovacích údajů nasadíte kód.
   
        az appservice web deployment user set --user-name <username> --password <password>

3. Vytvořte novou [skupinu prostředků](../azure-resource-manager/resource-group-overview.md). V tomto kurzu k node.js ještě nepotřebujete vědět, co to je.

        az group create --location "<location>" --name my-nodejs-app-group

    K zobrazení možných hodnot, které se dají použít pro `<location>`, použijte příkaz `az appservice list-locations` rozhraní příkazového řádku.

3. Vytvořte nový BEZPLATNÝ [plán služby App Service](../app-service/azure-web-sites-web-hosting-plans-in-depth-overview.md). Pro tento kurz k node.js stačí vědět, že se vám za webové aplikace v tomto plánu nebude nic účtovat.

        az appservice plan create --name my-nodejs-appservice-plan --resource-group my-nodejs-app-group --sku FREE

4. Vytvořte novou webovou aplikaci s jedinečným názvem ve značce `<app_name>`.

        az appservice web create --name <app_name> --resource-group my-nodejs-app-group --plan my-nodejs-appservice-plan

5. Ke konfiguraci místního nasazení Gitu pro webovou aplikaci použijete následující příkaz:

        az appservice web source-control config-local-git --name <app_name> --resource-group my-nodejs-app-group

    Získáte výstup JSON podobný tomuto. To znamená, že vzdálené úložiště Git je nakonfigurované:

        {
        "url": "https://<deployment_user>@<app_name>.scm.azurewebsites.net/<app_name>.git"
        }

6. Přidejte adresu URL v kódu JSON jako vzdálené úložiště Git pro vaše místní úložiště (pro zjednodušení nazvané `azure`).

        git remote add azure https://<deployment_user>@<app_name>.scm.azurewebsites.net/<app_name>.git
   
7. Nasaďte ukázkový kód do vzdáleného úložiště Git s názvem `azure`. Po zobrazení výzvy použijte přihlašovací údaje nasazení, které jste nakonfigurovali dříve.

        git push azure master
   
    Generátor Express již poskytuje soubor .gitignore, takže příkaz `git push` nebude využívat šířku pásma při pokusu o nahrání adresáře node_modules/.

9. Nakonec spusťte živou aplikaci Azure v prohlížeči:
   
        az appservice web browse --name <app_name> --resource-group my-nodejs-app-group
   
    Nyní byste měli vidět živý běh webové aplikace Node.js ve službě Azure App Service.
   
    ![Příklad procházení k nasazené aplikaci.][deployed-express-app]

## <a name="update-your-nodejs-web-app"></a>Aktualizace webové aplikace Node.js
Chcete-li provést aktualizace webové aplikace Node.js spuštěné ve službě App Service, stačí spustit `git add`, `git commit` a `git push` stejně jako při prvním nasazení webové aplikace.

## <a name="how-app-service-deploys-your-nodejs-app"></a>Postup nasazení aplikace Node.js službou App Service
Služba Azure App Service spouští aplikace Node.js pomocí modulu [iisnode]. Rozhraní příkazového řádku CLI 2.0 Preview a modul Kudu (nasazení Git) společně umožňují efektivnější práci uživatelů při vývoji a nasazení aplikací Node.js z příkazového řádku. 

* Můžete vytvořit soubor iisnode.yml v kořenovém adresáři a použít ho k přizpůsobení vlastností iisnode. Všechno konfigurovatelné nastavení je popsáno [zde](https://github.com/tjanczuk/iisnode/blob/master/src/samples/configuration/iisnode.yml).
* Při použití `git push azure master` modul Kudu automatizuje následující úkoly nasazení:
  
  * Pokud se v kořenovém adresáři úložiště nachází soubor package.json, spusťte `npm install --production`.
  * Vygenerujte soubor Web.config pro modul iisnode, který odkazuje na spouštěcí skript v souboru package.json (např. server.js nebo app.js).
  * Přizpůsobením souboru Web.config připravte aplikaci k ladění pomocí nástroje Node-Inspector.

## <a name="use-a-nodejs-framework"></a>Použití rozhraní Node.js
Používáte-li k vývoji aplikací některé z oblíbených rozhraní Node.js, například [Sails.js][SAILSJS] nebo [MEAN.js][MEANJS], můžete je nasadit do služby App Service. Oblíbená rozhraní Node.js mají své specifické zvláštnosti, přičemž závislosti jejich balíčků jsou neustále aktualizovány. Služba App Service však poskytuje protokoly stdout a stderr, díky nimž přesně víte, co se s aplikací děje, a můžete provádět odpovídající změny. Další informace naleznete v tématu [Získání protokolů stdout a stderr z modulu iisnode](#iisnodelog).

Následující kurzy vás seznámí s tím, jak pracovat s konkrétním rozhraním ve službě App Service:

* [Nasazení webové aplikace Sails.js do služby Azure App Service]
* [Vytvoření chatovací aplikace Node.js pomocí Socket.IO ve službě Azure App Service]
* [Použití io.js s aplikacemi Azure App Service Web Apps]

<a name="version"></a>

## <a name="use-a-specific-nodejs-engine"></a>Použití konkrétního modulu Node.js
Při obvyklém pracovním postupu říkáte službě App Service, aby používala konkrétní modul Node.js, a to stejným způsobem, jakým byste běžně postupovali v souboru package.json.
Příklad:

    "engines": {
        "node": "6.9.1"
    }, 

Modul nasazení Kudu určí, který modul Node.js se má použít, a to v následujícím pořadí:

* Nejprve zjišťuje, zda je v souboru iisnode.yml zadána položka `nodeProcessCommandLine`. Pokud ano, použije ji.
* Poté zjišťuje, zda je v souboru package.json zadána položka `"node": "..."` v objektu `engines`. Pokud ano, použije ji.
* Ve výchozím nastavení vybere výchozí Node.js.

Aktualizovaný seznam všech podporovaných verzí Node.js/NPM ve službě Azure App Service najdete na následující adrese URL aplikace:

    https://<app_name>.scm.azurewebsites.net/api/diagnostics/runtime

> [!NOTE]
> Doporučujeme explicitně definovat, který modul Node.js chcete použít. Výchozí verze Node.js se může změnit a ve vaší webové aplikaci může docházet k chybám, protože výchozí verze Node.js není vhodná pro vaši aplikaci.
> 
> 

<a name="iisnodelog"></a>

## <a name="get-stdout-and-stderr-logs-from-iisnode"></a>Získání protokolů stdout a stderr z modulu iisnode
Chcete-li si přečíst protokoly iisnode, postupujte následovně.

> [!NOTE]
> Po dokončení tohoto postupu nemusí soubory protokolů existovat, dokud nedojde k chybě.
> 
> 

1. Otevřete soubor iisnode.yml poskytovaný rozhraním příkazového řádku Azure CLI 2.0 Preview.
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
   
    Tato adresa URL se liší od adresy URL webové aplikace, neboť rozšiřuje název DNS o řetězec „*.scm.*“. . Pokud adresu URL takto nerozšíříte, obdržíte chybu 404.
5. Přejděte do adresáře D:\home\site\wwwroot\iisnode
   
    ![Navigace k umístění souborů protokolů modulu iisnode.][iislog-kudu-console-find]
6. Klikněte na ikonu **Upravit** u protokolu, který chcete číst. Dle potřeby také můžete kliknout na položku **Stáhnout** nebo **Odstranit**.
   
    ![Otevření souboru protokolu modulu iisnode.][iislog-kudu-console-open]
   
    Nyní je zobrazen protokol, který vám pomůže při ladění nasazení ve službě App Service.
   
    ![Prohlížení souboru protokolu modulu iisnode.][iislog-kudu-console-read]

## <a name="debug-your-app-with-node-inspector"></a>Ladění aplikace pomocí nástroje Node-Inspector
Používáte-li k ladění aplikací Node.js nástroj Node-Inspector, můžete jej použít pro živou aplikaci služby App Service. Nástroj Node-Inspector je předinstalován v instalaci modulu iisnode pro službu App Service. A pokud nasazujete prostřednictvím Git, automaticky generovaný soubor Web.config z modulu Kudu již obsahuje veškeré konfigurace nezbytné k povolení nástroje Node-Inspector.

Chcete-li povolit nástroj Node-Inspector, postupujte takto:

1. Otevřete soubor iisnode.yml v kořenovém adresáři úložiště a zadejte následující parametry: 
   
        debuggingEnabled: true
        debuggerExtensionDll: iisnode-inspector.dll
2. Uložte změny a poté tyto změny nuceně vložte (push) do Azure pomocí následujících příkazů Git:
   
        git add .
        git commit -m "{your commit message}"
        git push azure master
3. Nyní jednoduše přejděte k spouštěcímu souboru aplikace, který je určen spouštěcím skriptem v souboru package.json, přičemž příslušnou adresu URL doplňte o /debug. Například:
   
        http://{appname}.azurewebsites.net/server.js/debug
   
    Nebo:
   
        http://{appname}.azurewebsites.net/app.js/debug

## <a name="more-resources"></a>Další zdroje informací
* [Určení verze Node.js v aplikaci Azure](../nodejs-specify-node-version-azure-apps.md)
* [Osvědčené postupy a průvodce odstraňováním potíží pro aplikace Node.js v Azure](app-service-web-nodejs-best-practices-and-troubleshoot-guide.md)
* [Postup ladění webové aplikace Node.js ve službě Azure App Service](web-sites-nodejs-debug.md)
* [Používání modulů Node.js s aplikacemi Azure](../nodejs-use-node-modules-azure-apps.md)
* [Azure App Service Web Apps: Node.js](http://blogs.msdn.com/b/silverlining/archive/2012/06/14/windows-azure-websites-node-js.aspx)
* [Středisko pro vývojáře Node.js](/develop/nodejs/)
* [Začínáme s webovými aplikacemi ve službě Azure App Service](app-service-web-get-started.md)
* [Seznámení se supertajnou konzolou pro ladění modulu Kudu]

<!-- URL List -->

[Azure App Service]: ../app-service/app-service-value-prop-what-is.md
[aktivovat výhody předplatitele sady Visual Studio]: http://go.microsoft.com/fwlink/?LinkId=623901
[Bower]: http://bower.io/
[Vytvoření chatovací aplikace Node.js pomocí Socket.IO ve službě Azure App Service]: ./web-sites-nodejs-chat-app-socketio.md
[Nasazení webové aplikace Sails.js do služby Azure App Service]: ./app-service-web-nodejs-sails.md
[Seznámení se supertajnou konzolou pro ladění modulu Kudu]: /documentation/videos/super-secret-kudu-debug-console-for-azure-web-sites/
[Generátor Express pro Yeoman]: https://github.com/petecoop/generator-express
[Git]: http://www.git-scm.com/downloads
[Použití io.js s aplikacemi Azure App Service Web Apps]: ./web-sites-nodejs-iojs.md
[iisnode]: https://github.com/tjanczuk/iisnode/wiki
[MEANJS]: http://meanjs.org/
[Node.js]: http://nodejs.org
[SAILSJS]: http://sailsjs.org/
[zaregistrovat k bezplatné zkušební verzi]: http://go.microsoft.com/fwlink/?LinkId=623901
[web app]: ./app-service-web-overview.md
[Yeoman]: http://yeoman.io/

<!-- IMG List -->

[deployed-express-app]: ./media/app-service-web-nodejs-get-started/deployed-express-app.png
[iislog-kudu-console-find]: ./media/app-service-web-nodejs-get-started/iislog-kudu-console-navigate.png
[iislog-kudu-console-open]: ./media/app-service-web-nodejs-get-started/iislog-kudu-console-open.png
[iislog-kudu-console-read]: ./media/app-service-web-nodejs-get-started/iislog-kudu-console-read.png



<!--HONumber=Jan17_HO3-->


