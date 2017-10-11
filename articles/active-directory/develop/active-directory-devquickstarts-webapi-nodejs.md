---
title: "Začínáme se službou Azure AD Node.js | Microsoft Docs"
description: "Jak sestavit webové Node.js REST API, které se integruje se službou Azure AD pro ověřování."
services: active-directory
documentationcenter: nodejs
author: navyasric
manager: mbaldwin
editor: 
ms.assetid: 7654ab4c-4489-4ea5-aba9-d7cdc256e42a
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: javascript
ms.topic: article
ms.date: 01/07/2017
ms.author: nacanuma
ms.custom: aaddev
ms.openlocfilehash: 4f58177f540c14172d7ece8b4bc8c8a2b9787f8f
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="get-started-with-web-apis-for-nodejs"></a>Začínáme s webových rozhraní API pro Node.js
[!INCLUDE [active-directory-devguide](../../../includes/active-directory-devguide.md)]

*Passport* je ověřovací middleware pro Node.js. Flexibilní a modulární, Passport lze snadno vyřadit k žádnému využívající Express nebo Restify webové aplikace. Komplexní sada strategií podporují ověřování pomocí uživatelského jména a hesla, Facebook, Twitter a další. Vyvinuli jsme strategii pro Microsoft Azure Active Directory (Azure AD). Jsme nainstalujete tento modul a poté přidejte Microsoft Azure Active Directory `passport-azure-ad` modulu plug-in.

Budete muset:

1. Zaregistrovat aplikaci s Azure AD.
2. Nastavit aplikaci pro používání `passport-azure-ad` modulu plug-in.
3. Nakonfigurujte klientskou aplikaci, aby volání seznam úkolů webové rozhraní API.

Kód k tomuto kurzu je udržovaný [na GitHubu](https://github.com/Azure-Samples/active-directory-node-webapi).

> [!NOTE]
> Tento článek nezahrnuje, jak implementovat přihlášení, registrace a správy profilů pomocí Azure AD B2C. Zaměřuje se na volání webových rozhraní API po již byl uživatel ověřen.  Doporučujeme spouštět s [postup při integraci s Azure Active Directory dokumentu](active-directory-how-to-integrate.md) se dozvíte základní informace o službě Azure Active Directory.
>
>

Jsme jste vydaná zdrojového kódu v tomto příkladu spuštěné v Githubu pod licencí MIT, takže Nebojte se klonování (nebo i lépe rozvětvení) a poskytnout zpětnou vazbu a požadavky pro vyžádání obsahu.

## <a name="about-nodejs-modules"></a>O modulů Node.js
V tomto návodu použijeme modulů Node.js. Moduly jsou načíst JavaScript balíčky, které poskytují funkce specifická pro vaši aplikaci. Obvykle nainstalujete moduly pomocí Node.js nástroj příkazového řádku na NPM v instalačním adresáři NPM. Některé moduly, jako je například modul HTTP, jsou však součástí základní Node.js balíček.

Nainstalované moduly jsou uloženy **node_modules** adresář v kořenovém adresáři instalace Node.js. Každý modul v **node_modules** directory udržuje vlastní **node_modules** adresář, který obsahuje všechny moduly, které závisí na. Navíc má každý požadované modulu **node_modules** adresáře. Tato struktura adresáře rekurzivní představuje řetězec závislostí.

Tato struktura řetězu závislostí za následek větší nároků aplikace. Můžete ale také zaručuje, že byly splněny všechny závislosti a že verzi moduly, který se používá v vývoj slouží také v produkčním prostředí. To usnadňuje předvídatelnější chování aplikace produkční a zabrání problémům s verzemi, které mohou ovlivnit uživatele.

## <a name="step-1-register-an-azure-ad-tenant"></a>Krok 1: Registrace klienta Azure AD
Chcete-li tuto ukázku použít, musíte klienta služby Azure Active Directory. Pokud si nejste jisti je co klienta nebo jak získat, najdete v části [jak získat klienta Azure AD](active-directory-howto-tenant.md).

## <a name="step-2-create-an-application"></a>Krok 2: Vytvoření aplikace
Dále vytvoříte aplikaci v adresáři, který dává Azure AD informace potřebné k bezpečné komunikaci s vaší aplikací.  Klientská aplikace i webové rozhraní API jsou reprezentované pomocí jedné **ID aplikace** v tomto případě protože společně tvoří jednu logickou aplikaci.  Chcete-li vytvořit aplikaci, postupujte podle [těchto pokynů](active-directory-how-applications-are-added.md). Pokud vytváříte-obchodní aplikace, [mohou být užitečné tyto další pokyny](../active-directory-applications-guiding-developers-for-lob-applications.md).

Vytvoření aplikace:

1. Přihlaste se k webu [Azure Portal](https://portal.azure.com).

2. V horní nabídce vyberte svůj účet. Potom v části **Directory** vyberte klienta služby Active Directory, kde chcete registrace vaší aplikace.

3. V nabídce na levé straně vyberte **více služeb**a potom vyberte **Azure Active Directory**.

4. Vyberte **registrace aplikace**a potom vyberte **přidat**.

5. Postupujte podle výzev a vytvořte **webové aplikace nebo WebAPI**.

      * **Název** aplikace popisuje vaší aplikace pro koncové uživatele.

      * **Přihlašovací adresa URL** je základní adresu URL aplikace.  Výchozí adresa URL ukázkového kódu je `https://localhost:8080`.

6. Po registraci, Azure AD přiřadí aplikace jedinečné ID aplikace Je třeba tuto hodnotu v další části, zkopírujte jej ze stránky aplikace.

7. Z **nastavení** -> **vlastnosti** stránky pro aplikace, aktualizujte identifikátor ID URI aplikace. **Identifikátor ID URI aplikace** je jedinečný identifikátor pro vaši aplikaci. Konvence, je použít `https://<tenant-domain>/<app-name>`, například: `https://contoso.onmicrosoft.com/my-first-aad-app`.

8. Vytvoření **klíč** pro vaši aplikaci z **nastavení** stránky a pak ji někam zkopírujte. Budete je potřebovat za chvíli.

## <a name="step-3-download-nodejs-for-your-platform"></a>Krok 3: Stažení Node.js pro vaši platformu
Pro úspěšné fungování této ukázky musíte mít funkční instalací Node.js.

Nainstalujte si Node.js z [http://nodejs.org](http://nodejs.org).

## <a name="step-4-install-mongodb-on-your-platform"></a>Krok 4: Instalace MongoDB na vaši platformu
Pro úspěšné fungování této ukázky, musí mít funkční instalací MongoDB. Chcete-li trvalé rozhraní REST API napříč instancemi serveru používáte MongoDB.

Nainstalujte MongoDB z [http://mongodb.org](http://www.mongodb.org).

> [!NOTE]
> Tento návod předpokládá, že používáte výchozí instalaci a server koncové body pro MongoDB, které jsou v době psaní tohoto textu je mongodb://localhost.
>
>

## <a name="step-5-install-the-restify-modules-in-your-web-api"></a>Krok 5: Instalace modulů Restify ve webovém rozhraní API
Restify se používá k vytvoření našem REST API. Restify je minimalistické a flexibilní Node.js aplikace rozhraní, které je odvozené z Express. Obsahuje robustní sadu funkcí pro sestavování rozhraní REST API postavených na protokolu Connect.

### <a name="install-restify"></a>Instalace Restify
1. V příkazovém řádku přejděte do adresáře **azuread** adresáře. Pokud **azuread** adresář neexistuje, vytvořte ho.

        `cd azuread - or- mkdir azuread; cd azuread`

2. Zadejte následující příkaz:

    `npm install restify`

    Tento příkaz nainstaluje Restify.

#### <a name="did-you-get-an-error"></a>Obdrželi jste chybu?
Použijete-li NPM u některých operačních systémů, můžete obdržet chybu, která uvádí, že **Chyba: EPERM chmod '/ usr/místní/bin /..'** a zlepšení zkuste účet Spustit jako správce. Pokud k tomu dojde, pomocí příkazu sudo spustit NPM na vyšší úrovni oprávnění.

#### <a name="did-you-get-an-error-regarding-dtrace"></a>Obdrželi jste chybu týkající se DTRACE?
Při instalaci Restify může zobrazit chyba takto:

```Shell
clang: error: no such file or directory: 'HD/azuread/node_modules/restify/node_modules/dtrace-provider/libusdt'
make: *** [Release/DTraceProviderBindings.node] Error 1
gyp ERR! build error
gyp ERR! stack Error: `make` failed with exit code: 2
gyp ERR! stack     at ChildProcess.onExit (/usr/local/lib/node_modules/npm/node_modules/node-gyp/lib/build.js:267:23)
gyp ERR! stack     at ChildProcess.EventEmitter.emit (events.js:98:17)
gyp ERR! stack     at Process.ChildProcess._handle.onexit (child_process.js:789:12)
gyp ERR! System Darwin 13.1.0
gyp ERR! command "node" "/usr/local/lib/node_modules/npm/node_modules/node-gyp/bin/node-gyp.js" "rebuild"
gyp ERR! cwd /Volumes/Development HD/azuread/node_modules/restify/node_modules/dtrace-provider
gyp ERR! node -v v0.10.11
gyp ERR! node-gyp -v v0.10.0
gyp ERR! not ok
npm WARN optional dep failed, continuing dtrace-provider@0.2.8
```
Restify poskytuje výkonný mechanismus pro trasování volání REST pomocí DTrace. Řada operačních systémů, ale nemají DTrace. Tyto chyby můžete bezpečně ignorovat.

Výstup tohoto příkazu by měl vypadat podobně jako následující výstup:

    restify@2.6.1 node_modules/restify
    ├── assert-plus@0.1.4
    ├── once@1.3.0
    ├── deep-equal@0.0.0
    ├── escape-regexp-component@1.0.2
    ├── qs@0.6.5
    ├── tunnel-agent@0.3.0
    ├── keep-alive-agent@0.0.1
    ├── lru-cache@2.3.1
    ├── node-uuid@1.4.0
    ├── negotiator@0.3.0
    ├── mime@1.2.11
    ├── semver@2.2.1
    ├── spdy@1.14.12
    ├── backoff@2.3.0
    ├── formidable@1.0.14
    ├── verror@1.3.6 (extsprintf@1.0.2)
    ├── csv@0.3.6
    ├── http-signature@0.10.0 (assert-plus@0.1.2, asn1@0.1.11, ctype@0.5.2)
    └── bunyan@0.22.0 (mv@0.0.5)


## <a name="step-6-install-passportjs-in-your-web-api"></a>Krok 6: Instalace Passport.js ve vašem webovém rozhraní API
[Passport](http://passportjs.org/) je ověřovací middleware pro Node.js. Flexibilní a modulární, Passport lze snadno vyřadit k žádnému využívající Express nebo Restify webové aplikace. Komplexní sada strategií podporují ověřování pomocí uživatelského jména a hesla, Facebook, Twitter a další.

Vyvinuli jsme strategii pro Azure Active Directory. Jsme nainstalujete tento modul a poté přidáte modul plug-in strategie Azure Active Directory.

1. V příkazovém řádku přejděte do adresáře **azuread** adresáře.

2. Chcete-li nainstalovat passport.js, zadejte následující příkaz:

    `npm install passport`

    Výstup příkazu by měl vypadat podobně jako následující:

``
        passport@0.1.17 node_modules\passport
        ├── pause@0.0.1
        └── pkginfo@0.2.3
``

## <a name="step-7-add-passport-azure-ad-to-your-web-api"></a>Krok 7: Přidání Passport-Azure-AD do webového rozhraní API
Další přidáme strategii OAuth pomocí `passport-azure-ad`, sada strategií, které propojují Azure Active Directory do služby Passport. Používáme tuto strategii pro nosné tokeny v této ukázce REST API.

> [!NOTE]
> Přestože OAuth2 poskytuje rozhraní, ve kterém můžou být vystavené všechny známé typy tokenů, běžně se používají pouze určitým typům tokenů. Nejčastěji používané tokeny pro ochranu koncových bodů jsou nosné tokeny. Jsou nejčastěji vydávaným typem tokenů v OAuth2. Mnoho implementací předpokládá, že jsou nosné tokeny jediným typem tokeny, které jsou vystavené.
>
>

V příkazovém řádku přejděte do adresáře **azuread** adresáře.

Zadejte následující příkaz k instalaci Passport.js `passport-azure-ad module`:

`npm install passport-azure-ad`

Výstup příkazu by měl vypadat podobně jako následující výstup:


    passport-azure-ad@1.0.0 node_modules/passport-azure-ad
    ├── xtend@4.0.0
    ├── xmldom@0.1.19
    ├── passport-http-bearer@1.0.1 (passport-strategy@1.0.0)
    ├── underscore@1.8.3
    ├── async@1.3.0
    ├── jsonwebtoken@5.0.2
    ├── xml-crypto@0.5.27 (xpath.js@1.0.6)
    ├── ursa@0.8.5 (bindings@1.2.1, nan@1.8.4)
    ├── jws@3.0.0 (jwa@1.0.1, base64url@1.0.4)
    ├── request@2.58.0 (caseless@0.10.0, aws-sign2@0.5.0, forever-agent@0.6.1, stringstream@0.0.4, tunnel-agent@0.4.1, oauth-sign@0.8.0, isstream@0.1.2, extend@2.0.1, json-stringify-safe@5.0.1, node-uuid@1.4.3, qs@3.1.0, combined-stream@1.0.5, mime-types@2.0.14, form-data@1.0.0-rc1, http-signature@0.11.0, bl@0.9.4, tough-cookie@2.0.0, hawk@2.3.1, har-validator@1.8.0)
    └── xml2js@0.4.9 (sax@0.6.1, xmlbuilder@2.6.4)



## <a name="step-8-add-mongodb-modules-to-your-web-api"></a>Krok 8: Přidání modulů MongoDB do webového rozhraní API
MongoDB používáme jako naše úložiště. Z tohoto důvodu je potřeba nainstalovat často používaný modul plug-in volané Mongoose ke správě modelů a schémat. Také je potřeba nainstalovat ovladač databáze pro MongoDB (což je také nazývaný MongoDB).

 `npm install mongoose`

## <a name="step-9-install-additional-modules"></a>Krok 9: Instalace dalších modulů
Jsme dále nainstalujte zbývající požadované moduly.

1. V příkazovém řádku přejděte do adresáře **azuread** složky, pokud si nejste již existuje.

    `cd azuread`

2. Zadejte následující příkazy pro instalaci těchto modulů ve vašem **node_modules** directory:

    * `npm install assert-plus`
    * `npm install bunyan`
    * `npm update`

## <a name="step-10-create-a-serverjs-with-your-dependencies"></a>Krok 10: Vytvoření server.js se závislostmi
V souboru server.js poskytuje většinu funkcí pro naše webového rozhraní API serveru. Do tohoto souboru jsme přidávat většinu kódu. Pro produkční účely doporučujeme, aby vám funkčnost rozdělit do menších souborů, jako je například samostatné trasy a ovladače. V této ukázce používáme server.js pro tuto funkci.

1. V příkazovém řádku přejděte do adresáře **azuread** složky, pokud si nejste již existuje.

    `cd azuread`

2. Vytvoření `server.js` souboru ve svém oblíbeném editoru a poté přidejte následující informace:

    ```Javascript
        'use strict';

        /**
         * Module dependencies.
         */

        var fs = require('fs');
        var path = require('path');
        var util = require('util');
        var assert = require('assert-plus');
        var bunyan = require('bunyan');
        var getopt = require('posix-getopt');
        var mongoose = require('mongoose/');
        var restify = require('restify');
        var passport = require('passport');
      var BearerStrategy = require('passport-azure-ad').BearerStrategy;
    ```

3. Uložte soubor. Se vrátíme k němu za chvíli.

## <a name="step-11-create-a-config-file-to-store-your-azure-ad-settings"></a>Krok 11: Vytvořte konfigurační soubor pro uložení nastavení Azure AD
Tento soubor s kódem předává parametry konfigurace z portálu Azure Active Directory Passport.js. Tyto hodnoty konfigurace jste vytvořili, když jste přidali webové rozhraní API do portálu v první části tohoto návodu. Vysvětlíme, co zadat jako hodnoty těchto parametrů, až zkopírujete kód.

1. V příkazovém řádku přejděte do adresáře **azuread** složky, pokud si nejste již existuje.

    `cd azuread`

2. Vytvoření `config.js` souboru ve svém oblíbeném editoru a poté přidejte následující informace:

    ```Javascript
         exports.creds = {
             mongoose_auth_local: 'mongodb://localhost/tasklist', // Your mongo auth uri goes here
             clientID: 'your client ID',
             audience: 'your application URL',
            // you cannot have users from multiple tenants sign in to your server unless you use the common endpoint
          // example: https://login.microsoftonline.com/common/.well-known/openid-configuration
             identityMetadata: 'https://login.microsoftonline.com/<your tenant id>/.well-known/openid-configuration',
             validateIssuer: true, // if you have validation on, you cannot have users from multiple tenants sign in to your server
             passReqToCallback: false,
             loggingLevel: 'info' // valid are 'info', 'warn', 'error'. Error always goes to stderr in Unix.

         };
    ```
3. Uložte soubor.

## <a name="step-12-add-configuration-values-to-your-serverjs-file"></a>Krok 12: Přidejte hodnoty konfigurace do souboru server.js
Je potřeba tyto hodnoty čtení ze souboru .config, který jste vytvořili v naší aplikaci. K tomuto účelu přidáme souboru .config jako požadovaný prostředek v naší aplikaci. Potom jsme nastavte globální proměnné tak, aby odpovídaly proměnné v dokumentu config.js.

1. V příkazovém řádku přejděte do adresáře **azuread** složky, pokud si nejste již existuje.

    `cd azuread`

2. Otevřete váš `server.js` souboru ve svém oblíbeném editoru a poté přidejte následující informace:

    ```Javascript
    var config = require('./config');
    ```
3. Pak přidejte novou část, která `server.js` následujícím kódem:

    ```Javascript
    var options = {
        // The URL of the metadata document for your app. We will put the keys for token validation from the URL found in the jwks_uri tag of the in the metadata.
        identityMetadata: config.creds.identityMetadata,
        clientID: config.creds.clientID,
        validateIssuer: config.creds.validateIssuer,
        audience: config.creds.audience,
        passReqToCallback: config.creds.passReqToCallback,
        loggingLevel: config.creds.loggingLevel

    };

    // Array to hold logged in users and the current logged in user (owner).
    var users = [];
    var owner = null;

    // Our logger.
    var log = bunyan.createLogger({
        name: 'Azure Active Directory Bearer Sample',
             streams: [
            {
                stream: process.stderr,
                level: "error",
                name: "error"
            },
            {
                stream: process.stdout,
                level: "warn",
                name: "console"
            }, ]
    });

      // If the logging level is specified, switch to it.
      if (config.creds.loggingLevel) { log.levels("console", config.creds.loggingLevel); }

    // MongoDB setup.
    // Set up some configuration.
    var serverPort = process.env.PORT || 8080;
    var serverURI = (process.env.PORT) ? config.creds.mongoose_auth_mongohq : config.creds.mongoose_auth_local;
    ```

4. Uložte soubor.

## <a name="step-13-add-the-mongodb-model-and-schema-information-by-using-mongoose"></a>Krok 13: Přidejte MongoDB modelu a schématu informace pomocí Mongoose
Této přípravy se teď bude spustit platícího, protože jsme sloučit tyto tři soubory ve službu REST API.

V tomto návodu použijeme k uložení naše úlohy popsané v kroku 4 MongoDB.

V `config.js` souboru, že jsme vytvořili v kroku 11, volali jsme naše databáze `tasklist`, protože, který byl co jsme uveďte na konci naše **mogoose_auth_local** adresy URL pro připojení. Tuto databázi nemusíte předem vytvářet v MongoDB. Místo toho MongoDB vytvoří to nám při prvním spuštění aplikace naše server (za předpokladu, že databáze ještě neexistuje).

Teď, když server jsme jste sdělili kterou databázi MongoDB jsme chtěli používat, je potřeba napsat další kód pro vytvoření modelu a schématu pro úlohy naše serveru.

### <a name="discussion-of-the-model"></a>Informace o modelu
Naše model schématu je jednoduché. Rozbalte podle potřeby.

Název: Název osoby, která je přiřazen k úloze. A **řetězec**.

ÚLOHA: Vlastní úloha. A **řetězec**.

DATUM: Datum, tato úloha je kvůli. A **DATA A ČASU**.

DOKONČENO: Pokud je úloha byla dokončena nebo ne. A **BOOLEAN**.

### <a name="creating-the-schema-in-the-code"></a>Vytvoření schématu v kódu
1. V příkazovém řádku přejděte do adresáře **azuread** složky, pokud si nejste již existuje.

    `cd azuread`

2. Otevřete váš `server.js` souboru ve svém oblíbeném editoru a poté přidejte následující informace pod položku konfigurace:

    ```Javascript
    // Connect to MongoDB.
    global.db = mongoose.connect(serverURI);
    var Schema = mongoose.Schema;
    log.info('MongoDB Schema loaded');

    // Here we create a schema to store our tasks and users. It's a fairly simple schema for now.
    var TaskSchema = new Schema({
        owner: String,
        task: String,
        completed: Boolean,
        date: Date
    });

    // Use the schema to register a model.
    mongoose.model('Task', TaskSchema);
    var Task = mongoose.model('Task');
    ```
Jak se dá zjistit z kódu, jsme naše schématu nejprve vytvořit. Poté vytvoříme objekt modelu, který používáme k uložení našich dat napříč kódem, když jsme definovali naše **trasy**.

## <a name="step-14-add-our-routes-for-our-task-rest-api-server"></a>Krok 14: Přidejte naše trasy pro naše serveru úloh REST API
Teď, když máme model databáze pro práci s, přidejme trasy, které budeme používat naše server REST API.

### <a name="about-routes-in-restify"></a>O trasách v Restify
Trasy v Restify fungují stejně tak v balíku Express. Trasy se definují pomocí identifikátoru URI, který by měly volat klientské aplikace.  Trasy se obvykle definovat v samostatném souboru. Pro naše účely jsme do souboru server.js umístit naše trasy. Doporučujeme, abyste je zvážit tyto trasy do své vlastní souboru pro použití v provozním prostředí.

Typický vzor trasy Restify je následující:

```Javascript
function createObject(req, res, next) {

// Do work on object.

 _object.name = req.params.object; // passed value is in req.params under object

 ///...

return next(); // Keep the server going.
}

....

server.post('/service/:add/:object', createObject); // Calls createObject on routes that match this.

```


Toto je vzor na naprosto základní úrovni. Restify (a Express) poskytují mnohem hlubší funkčnost, jako je například definování typů aplikací a zajištění komplexního trasování napříč různými koncovými body. Pro naše účely jsme jsou zachování tyto trasy jednoduché.

### <a name="add-default-routes-to-our-server"></a>Přidání výchozích tras na našem serveru
Jsme teď přidejte do základní trasy CRUD vytvořit, načtení, aktualizace a odstranění.

1. V příkazovém řádku přejděte do adresáře **azuread** složky, pokud si nejste již existuje:

    `cd azuread`

2. Otevřete `server.js` souboru ve svém oblíbeném editoru a poté přidejte níže předchozí položky databáze, které jste provedli následující informace:

```Javascript

/**
 *
 * APIs for our REST Task server.
 */

// Create a task.

function createTask(req, res, next) {

    // Restify currently has a bug which doesn't allow you to set default headers.
    // These headers comply with CORS and allow us to mongodbServer our response to any origin.

    res.header("Access-Control-Allow-Origin", "*");
    res.header("Access-Control-Allow-Headers", "X-Requested-With");

    // Create a new task model, fill it, and save it to Mongodb.
    var _task = new Task();

    if (!req.params.task) {
        req.log.warn('createTodo: missing task');
        next(new MissingTaskError());
        return;
    }

    _task.owner = owner;
    _task.task = req.params.task;
    _task.date = new Date();

    _task.save(function(err) {
        if (err) {
            req.log.warn(err, 'createTask: unable to save');
            next(err);
        } else {
            res.send(201, _task);

        }
    });

    return next();

}


// Delete a task by name.

function removeTask(req, res, next) {

    Task.remove({
        task: req.params.task,
        owner: owner
    }, function(err) {
        if (err) {
            req.log.warn(err,
                'removeTask: unable to delete %s',
                req.params.task);
            next(err);
        } else {
            log.info('Deleted task:', req.params.task);
            res.send(204);
            next();
        }
    });
}

// Delete all tasks.

function removeAll(req, res, next) {
    Task.remove();
    res.send(204);
    return next();
}


// Get a specific task based on name.

function getTask(req, res, next) {

    log.info('getTask was called for: ', owner);
    Task.find({
        owner: owner
    }, function(err, data) {
        if (err) {
            req.log.warn(err, 'get: unable to read %s', owner);
            next(err);
            return;
        }

        res.json(data);
    });

    return next();
}

/// Simple returns the list of TODOs that were loaded.

function listTasks(req, res, next) {
    // Restify currently has a bug which doesn't allow you to set default headers.
    // These headers comply with CORS and allow us to mongodbServer our response to any origin.

    res.header("Access-Control-Allow-Origin", "*");
    res.header("Access-Control-Allow-Headers", "X-Requested-With");

    log.info("listTasks was called for: ", owner);

    Task.find({
        owner: owner
    }).limit(20).sort('date').exec(function(err, data) {

        if (err) {
            return next(err);
        }

        if (data.length > 0) {
            log.info(data);
        }

        if (!data.length) {
            log.warn(err, "There is no tasks in the database. Did you initialize the database as stated in the README?");
        }

        if (!owner) {
            log.warn(err, "You did not pass an owner when listing tasks.");
        } else {

            res.json(data);

        }
    });

    return next();
}

```

### <a name="add-error-handling-in-our-apis"></a>Přidání zpracování chyb v našem rozhraní API
```

///--- Errors for communicating something interesting back to the client.

function MissingTaskError() {
    restify.RestError.call(this, {
        statusCode: 409,
        restCode: 'MissingTask',
        message: '"task" is a required parameter',
        constructorOpt: MissingTaskError
    });

    this.name = 'MissingTaskError';
}
util.inherits(MissingTaskError, restify.RestError);


function TaskExistsError(owner) {
    assert.string(owner, 'owner');

    restify.RestError.call(this, {
        statusCode: 409,
        restCode: 'TaskExists',
        message: owner + ' already exists',
        constructorOpt: TaskExistsError
    });

    this.name = 'TaskExistsError';
}
util.inherits(TaskExistsError, restify.RestError);


function TaskNotFoundError(owner) {
    assert.string(owner, 'owner');

    restify.RestError.call(this, {
        statusCode: 404,
        restCode: 'TaskNotFound',
        message: owner + ' was not found',
        constructorOpt: TaskNotFoundError
    });

    this.name = 'TaskNotFoundError';
}

util.inherits(TaskNotFoundError, restify.RestError);
```


## <a name="step-15-create-your-server"></a>Krok 15: Vytvoření serveru
Jsme definovali naše databáze a naše trasy jsou na místě. Poslední krokem je přidání instance serveru, který spravuje naše volání.

V Restify (a Express) lze provádět mnoho přizpůsobení serveru REST API, ale znovu jsme se chystáte použít nejzákladnější nastavení pro naše účely.

```Javascript
/**
 * Our server.
 */


var server = restify.createServer({
    name: "Azure Active Directroy TODO Server",
    version: "2.0.1"
});

// Ensure we don't drop data on uploads.
server.pre(restify.pre.pause());

// Clean up sloppy paths like //todo//////1//.
server.pre(restify.pre.sanitizePath());

// Handle annoying user agents (curl).
server.pre(restify.pre.userAgentConnection());

// Set a per request bunyan logger (with requestid filled in).
server.use(restify.requestLogger());

// Allow five requests per second by IP, and burst to 10.
server.use(restify.throttle({
    burst: 10,
    rate: 5,
    ip: true,
}));

// Use the common stuff you probably want.
server.use(restify.acceptParser(server.acceptable));
server.use(restify.dateParser());
server.use(restify.queryParser());
server.use(restify.gzipResponse());
server.use(restify.bodyParser({
    mapParams: true
})); // Allow for JSON mapping to REST.
```

## <a name="step-16-add-the-routes-to-the-server-without-authentication-for-now"></a>Krok 16: Přidání tras na server (bez ověřování prozatím)
```Javascript
/// Now the real handlers. Here we just CRUD.
/**
/*
/* Each of these handlers is protected by our OIDCBearerStrategy by invoking 'oidc-bearer'.
/* In the pasport.authenticate() method. We set 'session: false' because REST is stateless and
/* we don't need to maintain session state. You can experiment with removing API protection
/* by removing the passport.authenticate() method as follows:
/*
/* server.get('/tasks', listTasks);
/*
**/
server.get('/tasks', listTasks);
server.get('/tasks', listTasks);
server.get('/tasks/:owner', getTask);
server.head('/tasks/:owner', getTask);
server.post('/tasks/:owner/:task', createTask);
server.post('/tasks', createTask);
server.del('/tasks/:owner/:task', removeTask);
server.del('/tasks/:owner', removeTask);
server.del('/tasks', removeTask);
server.del('/tasks', removeAll, function respond(req, res, next) {
res.send(204);
next();
});
// Register a default '/' handler.
server.get('/', function root(req, res, next) {
var routes = [
'GET /',
'POST /tasks/:owner/:task',
'POST /tasks (for JSON body)',
'GET /tasks',
'PUT /tasks/:owner',
'GET /tasks/:owner',
'DELETE /tasks/:owner/:task'
];
res.send(200, routes);
next();
});
server.listen(serverPort, function() {
var consoleMessage = '\n Microsoft Azure Active Directory Tutorial';
consoleMessage += '\n +++++++++++++++++++++++++++++++++++++++++++++++++++++';
consoleMessage += '\n %s server is listening at %s';
consoleMessage += '\n Open your browser to %s/tasks\n';
consoleMessage += '+++++++++++++++++++++++++++++++++++++++++++++++++++++ \n';
consoleMessage += '\n !!! why not try a $curl -isS %s | json to get some ideas? \n';
consoleMessage += '+++++++++++++++++++++++++++++++++++++++++++++++++++++ \n\n';
});
```

## <a name="step-17-run-the-server-before-adding-oauth-support"></a>Krok 17: Spuštění serveru (před přidáním podpory OAuth)
Otestovat váš server před přidáme ověřování.

Nejjednodušší způsob, jak otestovat svůj server je pomocí curl v příkazovém řádku. Než to, potřebujeme nástroj, který umožňuje parsovat výstup jako JSON.

1. Nainstalujte nástroj následující JSON (Tento nástroj použijte v následujících příkladech):

    `$npm install -g jsontool`

    Tím se globálně nainstaluje nástroj JSON. Teď, když jsme který udělat, budeme přehrání se serverem:

2. Nejprve se ujistěte, že je spuštěna mongoDB instance:

    `$sudo mongod`

3. Pak přejděte do adresáře a spusťte kulmy:

    `$ cd azuread` `$ node server.js`

    `$ curl -isS http://127.0.0.1:8080 | json`

    ```Shell
    HTTP/1.1 200 OK
    Connection: close
    Content-Type: application/json
    Content-Length: 171
    Date: Tue, 14 Jul 2015 05:43:38 GMT
    [
    "GET /",
    "POST /tasks/:owner/:task",
    "POST /tasks (for JSON body)",
    "GET /tasks",
    "PUT /tasks/:owner",
    "GET /tasks/:owner",
    "DELETE /tasks/:owner/:task"
    ]
    ```

4. Potom jsme můžete přidat úloha tímto způsobem:

    `$ curl -isS -X POST http://127.0.0.1:8080/tasks/brandon/Hello`

    Odpověď by měla být:

        ```Shell
        HTTP/1.1 201 Created
        Connection: close
        Access-Control-Allow-Origin: *
        Access-Control-Allow-Headers: X-Requested-With
        Content-Type: application/x-www-form-urlencoded
        Content-Length: 5
        Date: Tue, 04 Feb 2014 01:02:26 GMT
        Hello
        ```
    A zde jsou uvedeny úlohy pro Brandon tímto způsobem:

        `$ curl -isS http://127.0.0.1:8080/tasks/brandon/`

Pokud to vše funguje, jsme připraveni pro přidání OAuth na server REST API.

Máte server REST API s MongoDB!

## <a name="step-18-add-authentication-to-our-rest-api-server"></a>Krok 18: Přidání ověřování do našich server REST API
Teď, když máme spuštěné rozhraní REST API, Začněme jeho užitečnost s Azure AD.

V příkazovém řádku přejděte do adresáře **azuread** složky, pokud si nejste již existuje.

`cd azuread`

### <a name="use-the-oidcbearerstrategy-that-is-included-with-passport-azure-ad"></a>Použití OIDCBearerStrategy, která je součástí passport-azure-ad
Pokud jsme jste vytvořili typický server REST se seznamem úkolů bez jakéhokoli druhu ověřování. Toto je, kde začneme, které připravuje umístění.

1. Nejprve musíme znamenat, že má být použít Passport. Toto právo PUT po ostatní konfigurace serveru:

    ```Javascript
            // Let's start using Passport.js.

            server.use(passport.initialize()); // Starts passport.
            server.use(passport.session()); // Provides session support.
    ```
    > [!TIP]
    > Při psaní rozhraní API, doporučujeme vám, že jste vždy měli propojit data s něčím jedinečným z tokenu, který uživatel nemůže zfalšovat. Pokud tento server ukládá položky úkolů, uloží je na základě Identifikátoru objektu uživatele v tokenu (zavolaném prostřednictvím token.oid), který jsme umístit do pole "vlastník". To zajišťuje, aby pouze tento uživatel může přístup k jejich TODOs. Není nijak neprojevuje v rozhraní API se "vlastník", takže externí uživatel může požádat o TODOs jiných, i když jsou ověřeni.                    

2. Další umožňuje použijte nosnou strategii, která se dodává s `passport-azure-ad`. Podívejte se na kód prozatím a vysvětlíme zbývající za chvíli. To uvedli po vložení výše:

```Javascript
    /**
    /*
    /* Calling the OIDCBearerStrategy and managing users.
    /*
    /* Passport pattern provides the need to manage users and info tokens
    /* with a FindorCreate() method that must be provided by the implementor.
    /* Here we just auto-register any user and implement a FindById().
    /* You'll want to do something smarter.
    **/

    var findById = function(id, fn) {
        for (var i = 0, len = users.length; i < len; i++) {
            var user = users[i];
            if (user.sub === id) {
                log.info('Found user: ', user);
                return fn(null, user);
            }
        }
        return fn(null, null);
    };


    var bearerStrategy = new BearerStrategy(options,
        function(token, done) {
            log.info('verifying the user');
            log.info(token, 'was the token retreived');
            findById(token.sub, function(err, user) {
                if (err) {
                    return done(err);
                }
                if (!user) {
                    // "Auto-registration"
                    log.info('User was added automatically as they were new. Their sub is: ', token.sub);
                    users.push(token);
                    owner = token.sub;
                    return done(null, token);
                }
                owner = token.sub;
                return done(null, user, token);
            });
        }
    );

    passport.use(bearerStrategy);
```

Passport používá podobný Princip pro všechny svoje strategie (Twitteru, Facebooku a tak dále), které řídí všichni autoři strategií k. Prohlížení strategie, uvidíte, že jsme předáváme funkci, která má token a done jako parametry. Strategie vrátí do us po jeho činnosti provede. Po překročení se jsme uložení uživatele a skrytí tokenu, takže jsme nebudete muset požadovat znovu.

> [!IMPORTANT]
> Předchozí kód přijímá jakéhokoli uživatele, které dochází k ověřování na našem server. To se označuje jako Automatická registrace. Na produkčních serverech, které doporučujeme si nechat každý, kdo aniž by bylo nejdříve je přejdete prostřednictvím procesu registrace, který se rozhodnete. To je obvykle vzor, který můžete vidět u uživatelských aplikací, které umožňují registraci pomocí Facebooku, ale poté vás požádají o vyplnění dodatečných informací. Pokud to nebyli příkazového řádku programu, mohli bychom extrahovat e-mail z tokenu objektu, který se vrátí a poté požádat uživatele k vyplnění dodatečných informací. Protože se jedná o testovací server, jednoduše je přidáme do databáze v paměti.
>
>

### <a name="protect-some-endpoints"></a>Ochrana některé koncových bodů
Ochrana koncových bodů tak, že zadáte `passport.authenticate()` volání s protokol, který chcete použít.

Chcete-li naše serverový kód dělala něco zajímavějšího, Pojďme upravit trasy.

```Javascript
server.get('/tasks', passport.authenticate('oauth-bearer', {
session: false
}), listTasks);
server.get('/tasks', passport.authenticate('oauth-bearer', {
session: false
}), listTasks);
server.get('/tasks/:owner', passport.authenticate('oauth-bearer', {
session: false
}), getTask);
server.head('/tasks/:owner', passport.authenticate('oauth-bearer', {
session: false
}), getTask);
server.post('/tasks/:owner/:task', passport.authenticate('oauth-bearer', {
session: false
}), createTask);
server.post('/tasks', passport.authenticate('oauth-bearer', {
session: false
}), createTask);
server.del('/tasks/:owner/:task', passport.authenticate('oauth-bearer', {
session: false
}), removeTask);
server.del('/tasks/:owner', passport.authenticate('oauth-bearer', {
session: false
}), removeTask);
server.del('/tasks', passport.authenticate('oauth-bearer', {
session: false
}), removeTask);
server.del('/tasks', passport.authenticate('oauth-bearer', {
session: false
}), removeAll, function respond(req, res, next) {
res.send(204);
next();
});
```

## <a name="step-19-run-your-server-application-again-and-ensure-it-rejects-you"></a>Krok 19: Spusťte aplikaci server znovu a ujistěte se, že vás odmítne
Použijeme `curl` zjistíte, pokud máme teď chráněné pomocí OAuth2 proti naše koncové body. Provedeme tento test před s některým z klientskou sadu SDK proti tomuto koncovému bodu. Vrácené hlavičky by vám měly dostatečně Řekněte nám, pokud vytvoříme dolů správné cestě.

1. Nejprve se ujistěte, že je spuštěna mongoDB instance:

    `$sudo mongod`

2. Pak přejděte do adresáře a spusťte kulmy.

      `$ cd azuread` `$ node server.js`

3. Zkuste základní požadavek POST.

    `$ curl -isS -X POST http://127.0.0.1:8080/tasks/brandon/Hello`

    ```Shell
    HTTP/1.1 401 Unauthorized
    Connection: close
    WWW-Authenticate: Bearer realm="Users"
    Date: Tue, 14 Jul 2015 05:45:03 GMT
    Transfer-Encoding: chunked
    ```

401 je odpověď, kterou hledáte sem. Tato odpovědi vyplývá, že se vrstva Passportu pokouší přesměrovat na autorizovaný koncový bod, který je právě co chcete použít.

## <a name="next-steps"></a>Další kroky
Jste došli nejdál, co můžete s tímto serverem bez použití klientem kompatibilní OAuth2. Musíte absolvovat další návod.

Teď když jste se naučili jak implementovat REST API pomocí Restify a OAuth2. Máte také víc než dost kód zachovat vývoj služby a naučit, jak stavět na tomto příkladu.

Pokud vás zajímají další kroky v vaší ADAL cesty, tady jsou některé podporované klienty ADAL, doporučujeme, můžete pokračovat v práci s.

Klonovat na počítači pro vývojáře a nakonfigurujte, jak je popsáno v tomto návodu.

[ADAL pro iOS](https://github.com/MSOpenTech/azure-activedirectory-library-for-ios)

[ADAL pro Android](https://github.com/MSOpenTech/azure-activedirectory-library-for-android)

[!INCLUDE [active-directory-devquickstarts-additional-resources](../../../includes/active-directory-devquickstarts-additional-resources.md)]
