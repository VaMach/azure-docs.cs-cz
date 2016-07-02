<properties
    pageTitle="Aplikace Node.js API v Azure App Service | Microsoft Azure"
    description="Podívejte se, jak vytvořit rozhraní Node.js RESTful API a nasadit ho do aplikace API v Azure App Service."
    services="app-service\api"
    documentationCenter="node"
    authors="bradygaster"
    manager="wpickett"
    editor=""/>

<tags
    ms.service="app-service-api"
    ms.workload="web"
    ms.tgt_pltfrm="na"
    ms.devlang="node"
    ms.topic="get-started-article"
    ms.date="05/26/2016"
    ms.author="bradygaster"/>

# Sestavení rozhraní Node.js RESTful API a jeho nasazení do aplikace API v Azure

[AZURE.INCLUDE [app-service-api-get-started-selector](../../includes/app-service-api-get-started-selector.md)]

Tento kurz ukazuje, jak vytvořit jednoduché rozhraní [Node.js](http://nodejs.org) API a jak ho nasadit do [aplikace API](app-service-api-apps-why-best-platform.md) v [Azure App Service](../app-service/app-service-value-prop-what-is.md) pomocí systému [Git](http://git-scm.com). Můžete použít libovolný operační systém, na kterém běží Node.js, a všechnu práci provedete pomocí nástrojů příkazového řádku, jako je cmd.exe nebo bash.

## Předpoklady

1. Účet Microsoft Azure ([zde si můžete otevřít bezplatný účet](https://azure.microsoft.com/pricing/free-trial/))
1. Nainstalovaný [Node.js](http://nodejs.org) (V tomto příkladu se předpokládá, že máte Node.js verze 4.2.2.)
2. Nainstalovaný [Git](https://git-scm.com/)
1. Účet [GitHub](https://github.com/)

Ačkoli App Service podporuje mnoho způsobů nasazení kódu do aplikace API, v tomto kurzu vám předvedeme metodu Git, přičemž se předpokládá, že máte základní znalosti o tom, jak se systémem Git pracovat. Informace o dalších metodách nasazení naleznete v tématu [Nasazení vaší aplikace do Azure App Service](../app-service-web/web-sites-deploy.md).

## Získání ukázkového kódu

1. Otevřete rozhraní příkazového řádku, ve kterém se dají spouštět příkazy pro Node.js a Git.

1. Přejděte do složky, které můžete používat jako místní úložiště Git, a naklonujte [úložiště GitHub obsahující ukázkový kód](https://github.com/Azure-Samples/app-service-api-node-contact-list).

        git clone https://github.com/Azure-Samples/app-service-api-node-contact-list.git

    Ukázkové rozhraní API poskytuje dva koncové body: Požadavek Get na `/contacts` vrací seznam jmen a e-mailových adres ve formátu JSON, zatímco `/contacts/{id}` vrací pouze vybraný kontakt.

## Automatické generování (scaffold) kódu Node.js na základě metadat Swagger

[Swagger](http://swagger.io/) je formát souboru pro metadata, která popisují rozhraní RESTful API. Azure App Service nabízí [integrovanou podporu pro metadata Swagger](app-service-api-metadata.md). V této části kurzu modelujeme pracovní postup vývoje rozhraní API, při kterém nejprve vytvoříte metadata Swagger a pak pomocí nich automaticky vygenerujete (scaffold) serverový kód pro rozhraní API. 

>[AZURE.NOTE] Pokud nepotřebujete vědět, jak automaticky vygenerovat kód Node.js ze souboru metadat Swagger, můžete tuto část přeskočit. Pokud chcete jen nasadit ukázkový kód do nové aplikace API, přejděte přímo k části [Vytvoření aplikace API v Azure](#createapiapp).

### Instalace a spuštění nástroje Swaggerize

1. Spuštěním následujících příkazů globálně nainstalujte moduly NPM **yo** a **generator-swaggerize**.

        npm install -g yo
        npm install -g generator-swaggerize

    Swaggerize je nástroj, který generuje serverový kód pro rozhraní API popsané v souboru metadat Swagger. Použijete soubor Swagger s názvem *api.json*, který se nachází ve složce *start* úložiště, které jste naklonovali.

2. Přejděte do složky *start* a potom spusťte příkaz `yo swaggerize`. Swaggerize vám položí řadu otázek.  Jako **název projektu** zadejte „contactlist“, jako **cestu k dokumentu swagger** zadejte „api.json“ a u volby **Express, Hapi nebo Restify** zadejte „express“.

        yo swaggerize

    ![Příkazový řádek nástroje Swaggerize](media/app-service-api-nodejs-api-app/swaggerize-command-line.png)
    
    **Poznámka:**: Pokud se vyskytne chyba v tomto kroku, dozvíte se v dalším kroku, jak ji odstranit.

    Swaggerize vytvoří služby aplikace, obslužné rutiny scaffold a konfigurační soubory a vygeneruje soubor **package.json**. Modul expresního zobrazení slouží ke generování stránky nápovědy pro Swagger.  

3. Pokud příkaz `swaggerize` selže kvůli chybě „neočekávaný token“ nebo „neplatná řídicí sekvence“, opravte příčinu chyby úpravou vygenerovaného souboru *package.json*. Na řádku `regenerate` pod položkou `scripts` změňte zpětné lomítko, které předchází *api.json*, na dopředné lomítko, aby řádek vypadal jako v následujícím příkladu:

        "regenerate": "yo swaggerize --only=handlers,models,tests --framework express --apiPath config/api.json"

1. Přejděte do složky, která obsahuje automaticky generovaný kód (v tomto případě podsložka *ContactList*).

1. Spusťte `npm install`.
    
        npm install
        
2. Nainstalujte modul NPM **jsonpath**. 

        npm install --save jsonpath
        
    ![Instalace jsonpath](media/app-service-api-nodejs-api-app/jsonpath-install.png)

1. Nainstalujte modul NPM **swaggerize-ui**. 

        npm install --save swaggerize-ui
        
    ![Instalace uživatelského rozhraní nástroje Swaggerize](media/app-service-api-nodejs-api-app/swaggerize-ui-install.png)

### Přizpůsobení automaticky generovaného kódu

1. Zkopírujte složku **lib** ze složky **start** do složky **ContactList** vytvořené modulem scaffolder. 

1. Nahraďte kód v souboru **handlers/contacts.js** následujícím kódem. 

    Tento kód používá JSON data uložená v souboru **lib/contacts.json**, který poskytuje **lib/contactRepository.js**. Nový kód contacts.js odpovídá na požadavky HTTP, aby bylo možné získat všechny kontakty a mohli je vrátit jako data JSON. 

        'use strict';
        
        var repository = require('../lib/contactRepository');
        
        module.exports = {
            get: function contacts_get(req, res) {
                res.json(repository.all())
            }
        };

1. Nahraďte kód v souboru **handlers/contacts/{id}.js** následujícím kódem. 

        'use strict';

        var repository = require('../../lib/contactRepository');
        
        module.exports = {
            get: function contacts_get(req, res) {
                res.json(repository.get(req.params['id']));
            }    
        };

1. Nahraďte kód v **server.js** následujícím kódem. 

    Změny provedené v souboru server.js jsou vyznačeny pomocí komentářů, abyste viděli prováděné změny. 

        'use strict';

        var port = process.env.PORT || 8000; // first change

        var http = require('http');
        var express = require('express');
        var bodyParser = require('body-parser');
        var swaggerize = require('swaggerize-express');
        var swaggerUi = require('swaggerize-ui'); // second change
        var path = require('path');

        var app = express();

        var server = http.createServer(app);

        app.use(bodyParser.json());

        app.use(swaggerize({
            api: path.resolve('./config/api.json'), // third change
            handlers: path.resolve('./handlers'),
            docspath: '/swagger' // fourth change
        }));

        // change four
        app.use('/docs', swaggerUi({
          docs: '/swagger'  
        }));

        server.listen(port, function () { // fifth and final change
        });

### Testování s místně spuštěným rozhraním API 

1. Aktivujte server pomocí spustitelného souboru příkazového řádku Node.js. 

        node server.js

1. Když přejdete na **http://localhost:8000/contacts**, uvidíte výstup JSON ze seznamu kontaktů (nebo se zobrazí výzva k jeho stažení, v závislosti na prohlížeči). 

    ![Volání rozhraní API pro všechny kontakty](media/app-service-api-nodejs-api-app/all-contacts-api-call.png)

1. Když přejdete na **http://localhost:8000/contacts/2**, uvidíte kontakt odpovídající této hodnotě ID.

    ![Volání rozhraní API pro konkrétní kontakt](media/app-service-api-nodejs-api-app/specific-contact-api-call.png)

1. Data JSON pro Swagger se předávají prostřednictvím koncového bodu **/swagger**:

    ![Kontakty jako data JSON pro Swagger](media/app-service-api-nodejs-api-app/contacts-swagger-json.png)

1. Uživatelské rozhraní Swagger se poskytuje prostřednictvím koncového bodu **/docs**. V uživatelském rozhraní Swagger si můžete rozhraní API otestovat díky bohaté nabídce funkcí pro klienta HTML.

    ![Uživatelské rozhraní Swagger](media/app-service-api-nodejs-api-app/swagger-ui.png)

## <a id="createapiapp"></a> Vytvoření nové aplikace API

V této části využijete Portál Azure k vytvoření nové aplikace API v Azure. Tato aplikace API představuje výpočetní prostředky, které Azure poskytuje pro spouštění vašeho kódu. V pozdějších částech kód nasadíte do nové aplikace API.

1. Přejděte na [Portál Azure](https://portal.azure.com/). 

1. Klikněte na **Nové > Web + mobilní zařízení > Aplikace API**. 

    ![Nová aplikace API na portálu](media/app-service-api-nodejs-api-app/new-api-app-portal.png)

4. Zadejte **název aplikace**, který bude jedinečný v doméně *azurewebsites.net*, například NodejsAPIApp plus číslo pro zajištění jeho jedinečnosti. 

    Pokud je název například `NodejsAPIApp`, adresa URL bude `nodejsapiapp.azurewebsites.net`.

    Pokud zadáte název, který již použil někdo jiný, zobrazí se vpravo červený vykřičník.

6. V rozevíracím seznamu **Skupina prostředků**, klikněte na **Nový** a potom do pole **Nový název skupiny prostředků** zadejte NodejsAPIAppGroup nebo jiný název, který preferujete. 

    [Skupina prostředků](../azure-portal/resource-group-portal.md) je kolekce prostředků Azure, jako jsou aplikace API, databáze a virtuální počítače. V rámci tohoto kurzu bude nejlepší vytvořit novou skupinu prostředků, protože pak bude možné v jednom kroku odstranit všechny prostředky Azure, které při kurzu vytvoříte.

4. Klikněte na **Plán nebo umístění služby App Service**, a potom na tlačítko **Vytvořit nové**.

    ![Vytvoření plánu služby App Service](./media/app-service-api-nodejs-api-app/newappserviceplan.png)

    V následujícím postupu vytvoříte plán služby App Service pro novou skupinu prostředků. Plán služby App Service určuje výpočetní prostředky, na kterých běží vaše aplikace API. Pokud se například rozhodnete pro úroveň Free, vaše aplikace API poběží na sdílených virtuálních počítačích, zatímco u některých placených úrovní poběží na vyhrazených virtuálních počítačích. Informace o plánech služby App Service naleznete v části [Přehled plánů služby App Service](../app-service/azure-web-sites-web-hosting-plans-in-depth-overview.md).

5. V okně **Plán služby App Service** zadejte NodejsAPIAppPlan nebo jiný preferovaný název.

5. V rozevíracím seznamu **Umístění** vyberte umístění, které je k vám nejblíže.

    Toto nastavení určuje, ve kterém datacentru Azure vaše aplikace poběží. V rámci tohoto kurzu můžete vybrat libovolnou oblast – rozdíl bude nepatrný. Avšak u produkční aplikace bude ve vašem zájmu, aby server byl co nejblíže ke klientům, kteří k němu přistupují, aby se minimalizovala [latence](http://www.bing.com/search?q=web%20latency%20introduction&qs=n&form=QBRE&pq=web%20latency%20introduction&sc=1-24&sp=-1&sk=&cvid=eefff99dfc864d25a75a83740f1e0090).

5. Klikněte na **Cenová úroveň > Zobrazit vše > F1 Free**.

    V rámci tohoto kurzu zajistí cenová úroveň Free dostatečný výkon.

    ![Výběr cenové úrovně Free](./media/app-service-api-nodejs-api-app/selectfreetier.png)

6. V okně **Plán služby App Service** klikněte na **OK**.

7. V okně **Aplikace API** klikněte na **Vytvořit**.

## Nastavení nové aplikace API pro nasazení přes Git

Kód do aplikace API nasadíte vynuceným doručením (push) potvrzených změn do úložiště Git v Azure App Service. V této části kurzu vytvoříte přihlašovací údaje a úložiště Git v Azure, který pro toto nasazení použijete.  

1. Po vytvoření aplikace API klikněte na **App Services > {vaše aplikace API}** na domovské stránce portálu. 

    Na portálu se zobrazí okna **Aplikace API** a **Nastavení**.

    ![Okno Aplikace API a Nastavení na portálu](media/app-service-api-nodejs-api-app/portalapiappblade.png)

1. V okně **Nastavení** přejděte dolů k části **Publikování** a potom klikněte na **Přihlašovací údaje pro nasazení**.
 
3. V okně **Přihlašovací údaje pro nasazení** zadejte uživatelské jméno a heslo a potom klikněte na **Uložit**.

    Tyto přihlašovací údaje použijete pro publikování kódu Node.js do aplikace API. 

    ![Přihlašovací údaje pro nasazení](media/app-service-api-nodejs-api-app/deployment-credentials.png)

1. V okně **Nastavení** klikněte na **Zdroj nasazení > Vybrat zdroj > Místní úložiště Git** a klikněte na **OK**.

    ![Vytvoření úložiště Git](media/app-service-api-nodejs-api-app/create-git-repo.png)

1. Po vytvoření úložiště Git se okno změní tak, aby zobrazovalo aktivní nasazení. Vzhledem k tomu, že úložiště je nové, nemáte v seznamu žádná aktivní nasazení. 

    ![Žádná aktivní nasazení](media/app-service-api-nodejs-api-app/no-active-deployments.png)

1. Zkopírujte adresu URL úložiště Git. Uděláte to tak, že přejdete do okna pro novou aplikaci API a podíváte se do části **Základy**. Všimněte si položky **Adresa URL pro git clone** v části **Základy**. Po přesunutí ukazatele myši na tuto adresu URL se na pravé straně zobrazí ikona, která umožňuje zkopírování adresy URL do schránky. Kliknutím na tuto ikonu zkopírujte adresu URL.

    ![Získání adresy URL pro Git z portálu](media/app-service-api-nodejs-api-app/get-the-git-url-from-the-portal.png)

    **Poznámka:** Adresu URL klonu Git budete potřebovat v další části, a proto si ji na chvíli někam uložte.

Nyní již máte aplikaci API s podpůrným úložištěm Git, takže můžete do úložiště nuceně doručit (push) kód, čímž nasadíte kód do aplikace API. 

## Nasazení kódu rozhraní API do Azure

V této části vytvoříte místní úložiště Git, které bude obsahovat serverový kód pro vaše rozhraní API, a potom nuceně doručíte (push) kód z tohoto úložiště do úložiště v Azure, které jste vytvořili dříve.

1. Zkopírujte složku `ContactList` do umístění, které můžete použít pro nové místní úložiště Git. Pokud jste to absolvovali první část kurzu, zkopírujte `ContactList` ze složky `start`; jinak zkopírujte `ContactList` ze složky `end`.

1. V nástroji příkazového řádku přejděte do nové složky a potom spusťte následující příkaz, čímž vytvoříte nové místní úložiště Git. 

        git init

     ![Nové místní úložiště Git](media/app-service-api-nodejs-api-app/new-local-git-repo.png)

1. Spusťte následující příkaz, kterým přidáte vzdálené řízení Git pro úložiště vaší aplikace API. 

        git remote add azure YOUR_GIT_CLONE_URL_HERE

    **Poznámka:**: Řetězec YOUR_GIT_CLONE_URL_HERE nahraďte vlastní adresou URL pro klon Git, kterou jste si zkopírovali dříve. 

1. Spusťte následující příkazy, čímž vytvoření potvrzení změn, které bude obsahovat veškerý kód. 

        git add .
        git commit -m "initial revision"

    ![Výstup ve formě potvrzení změn ze systému Git ](media/app-service-api-nodejs-api-app/git-commit-output.png)

1. Spuštěním příkazu nuceně vložte (push) kód do Azure. Když se zobrazí výzva k zadání hesla, zadejte to, které jste si dříve nastavili na Portálu Azure.

        git push azure master

    Tím se spustí nasazování do vaší aplikace API.  

1. V prohlížeči se vraťte do okna **Nasazení** pro vaši aplikaci API, kde uvidíte, že nasazování probíhá. 

    ![Probíhající nasazení](media/app-service-api-nodejs-api-app/deployment-happening.png)

    Simultánně se postup probíhajícího nasazování odráží v rozhraní příkazového řádku. 

    ![Probíhající nasazení Node.js](media/app-service-api-nodejs-api-app/node-js-deployment-happening.png)

    Po dokončení operace bude okno **Nasazení** odrážet úspěšné nasazení změn kódu do vaší aplikace API. 

## Testování s rozhraním API spuštěným v Azure
 
3. Zkopírujte **adresu URL** v části **Základy** v okně vaší aplikace API. 

    ![Nasazení dokončeno](media/app-service-api-nodejs-api-app/deployment-completed.png)

1. Pomocí klienta pro REST API, jako je Postman nebo Fiddler, (nebo ve webovém prohlížeči) zadejte adresu URL rozhraní API pro vyvolání kontaktů, což je koncový bod `/contacts` vaší aplikace API. Adresa URL bude `https://{your API app name}.azurewebsites.net/contacts`

    Pokud vydáte požadavek GET na tento koncový bod, získáte výstup JSON vaší aplikace API.

    ![Spuštění rozhraní API v nástroji Postman](media/app-service-api-nodejs-api-app/postman-hitting-api.png)

2. V prohlížeči přejděte ke koncovému bodu `/docs` a vyzkoušejte spuštění Swagger UI v Azure.

Nyní když máte nastavené průběžné doručování, můžete provádět změny kódu a nasazovat je do Azure jednoduchým nuceným doručením (push) potvrzených změn do úložiště Git v Azure.

## Další kroky

V tomto okamžiku jste úspěšně vytvořili aplikaci API a nasadili do ní kód Node.js API. V dalším kurzu se dozvíte, jak [využívat aplikace API z klientů JavaScript pomocí CORS](app-service-api-cors-consume-javascript.md).



<!--HONumber=Jun16_HO2-->


