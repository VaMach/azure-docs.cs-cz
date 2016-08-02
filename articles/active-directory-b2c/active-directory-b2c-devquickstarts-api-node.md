<properties
    pageTitle="B2C ve verzi Preview: Zabezpečení webového rozhraní API pomocí Node.js | Microsoft Azure"
    description="Jak sestavit webové rozhraní API Node.js, které přijímá tokeny z klienta B2C"
    services="active-directory-b2c"
    documentationCenter=""
    authors="brandwe"
    manager="msmbaldwin"
    editor=""/>

<tags
    ms.service="active-directory-b2c"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="javascript"
    ms.topic="hero-article"
    ms.date="05/31/2016"
    ms.author="brandwe"/>

# B2C ve verzi Preview: Zabezpečení webového rozhraní API pomocí Node.js

[AZURE.INCLUDE [active-directory-b2c-preview-note](../../includes/active-directory-b2c-preview-note.md)]


> [AZURE.NOTE] Tento článek se nezabývá implementací registrace, přihlašování a správy profilů pomocí Azure AD B2C. Zaměřuje se na volání webových rozhraní API po ověření uživatele. Pokud jste tak ještě neučinili, měli byste začít s [kurzem Začínáme s webovými aplikacemi .NET](active-directory-b2c-devquickstarts-web-dotnet.md), kde naleznete základy Azure Active Directory B2C.


> [AZURE.NOTE]  Tato ukázka byla napsána pro připojení pomocí naší [ukázkové aplikace iOS B2C](active-directory-b2c-devquickstarts-ios.md). Nejprve pokračujte podle tohoto návodu a poté postupujte podle ukázky.

**Passport** je ověřovací middleware pro Node.js. Passport je velmi flexibilní a modulární a lze ho snadno nainstalovat v jakékoli webové aplikaci využívající Express nebo Restify. Komplexní sada strategií podporuje ověřování pomocí uživatelského jména a hesla, Facebooku, Twitteru a dalších. Vyvinuli jsme strategii pro Azure Active Directory (Azure AD). Nainstalujete tento modul a poté přidáte modul plug-in Azure AD `passport-azure-ad`.

Budete muset:

1. Zaregistrovat aplikaci s Azure AD.
2. Nastavit aplikaci pro používání modulu plug-in `azure-ad-passport`.
3. Nakonfigurovat klientskou aplikaci, aby volala webové rozhraní API „seznam úkolů“.

Kód k tomuto kurzu [je udržovaný na GitHubu](https://github.com/AzureADQuickStarts/B2C-WebAPI-nodejs). Chcete-li kód sledovat, můžete si [stáhnout kostru aplikace jako soubor ZIP](https://github.com/AzureADQuickStarts/B2C-WebAPI-nodejs/archive/skeleton.zip) nebo tuto kostru klonovat:

```git clone --branch skeleton https://github.com/AzureADQuickStarts/B2C-WebAPI-nodejs.git```

Hotová aplikace je k dispozici na konci tohoto kurzu.

> [AZURE.WARNING]   Pro B2C ve verzi Preview musíte použít stejné **ID klienta**/**ID aplikace** a zásady jak pro server úloh webového rozhraní API, tak pro klienta, který se k němu připojuje. To samé platí pro naše kurzy o iOS a Androidu. Pokud jste již dříve vytvořili aplikaci v některém z těchto rychlých průvodců, použijte tyto hodnoty, nevytvářejte nové.


## Získání adresáře služby Azure AD B2C

Před použitím Azure AD B2C musíte vytvořit adresář, nebo klienta.  Adresář je kontejner pro všechny vaše uživatele, aplikace, skupiny a další.  Pokud ho ještě nemáte, [vytvořte adresář B2C](active-directory-b2c-get-started.md) předtím, než budete pokračovat.

## Vytvoření aplikace

Dále musíte ve svém adresáři B2C vytvořit aplikaci, která poskytne Azure AD informace potřebné k bezpečné komunikaci s vaší aplikací. V tomto případě budou mít klientská aplikace i webové rozhraní API stejné **ID aplikace**, protože společně tvoří jednu logickou aplikaci. Chcete-li vytvořit aplikaci, postupujte podle [těchto pokynů](active-directory-b2c-app-registration.md). Ujistěte se, že:

- Jste do aplikace zahrnuli **webovou aplikaci nebo webové rozhraní API**.
- Jste do pole **Adresa URL odpovědi** vyplnili `http://localhost/TodoListService`. To je výchozí URL pro tento příklad.
- Vytvořte pro aplikaci **tajný klíč aplikace** a poznamenejte si ho. Budete ho potřebovat později. Budete ho potřebovat za chvíli. Před tím, než tuto hodnotu použijete, musí být [uvozena v XML](https://www.w3.org/TR/2006/REC-xml11-20060816/#dt-escape).
- Poznamenejte si **ID aplikace** přiřazené vaší aplikaci. To také budete potřebovat později.

[AZURE.INCLUDE [active-directory-b2c-devquickstarts-v2-apps](../../includes/active-directory-b2c-devquickstarts-v2-apps.md)]

## Vytvořte svoje zásady

V Azure AD B2C je každé uživatelské rozhraní definováno [zásadou](active-directory-b2c-reference-policies.md). Tato aplikace obsahuje tři činnosti identity: registrace, přihlášení a přihlášení pomocí Facebooku. Pro každý typ rozhraní musíte vytvořit zásadu, jak je popsáno v [článku o zásadách](active-directory-b2c-reference-policies.md#how-to-create-a-sign-up-policy).  Když vytváříte tyto tři zásady, nezapomeňte:

- Zvolit **Zobrazovaný název** a další atributy registrace ve svojí registrační zásadě.
- Zvolit deklarace identity aplikace **Zobrazovaný název** a **ID objektu** v každé zásadě.  Můžete zvolit i další deklarace identity.
- Po vytvoření každé zásady si poznamenejte její **Název**. Měl by mít předponu `b2c_1_`.  Tyto názvy zásad budete potřebovat později.

[AZURE.INCLUDE [active-directory-b2c-devquickstarts-policy](../../includes/active-directory-b2c-devquickstarts-policy.md)]

Po vytvoření těchto tří zásad jste připraveni k sestavení aplikace.

Tento článek nepopisuje používání právě vytvořených zásad. Chcete-li se dozvědět, jak fungují zásady v Azure AD B2C, začněte [kurzem Začínáme s webovými aplikacemi .NET](active-directory-b2c-devquickstarts-web-dotnet.md).

## Stažení Node.js pro vaši platformu

Pro úspěšné fungování této ukázky musíte mít funkční instalací Node.js.

Nainstalujte si Node.js z [nodejs.org](http://nodejs.org).

## Instalace MongoDB pro vaši platformu

Pro úspěšné fungování této ukázky musíte mít také funkční instalací MongoDB. MongoDB budete používat k zajištění trvalosti REST API napříč instancemi serveru.

Nainstalujte MongoDB z [mongodb.org](http://www.mongodb.org).

> [AZURE.NOTE] Tento návod předpokládá, že budete používat výchozí instalaci a koncové body serveru pro MongoDB, které jsou v době psaní tohoto návodu `mongodb://localhost`.

## Instalace modulů Restify ve webovém rozhraní API

Restify použijete k sestavení REST API. Restify je minimalistické a flexibilní rozhraní Node.js odvozené z Express. Obsahuje robustní sadu funkcí pro sestavování rozhraní REST API postavených na protokolu Connect.

### Instalace Restify

V příkazovém řádku přejděte do adresáře `azuread`. Pokud adresář `azuread` neexistuje, vytvořte ho.

`cd azuread` nebo `mkdir azuread;`

Zadejte následující příkaz:

`npm install restify`

Tento příkaz nainstaluje Restify.

#### Obdrželi jste chybu?

Při použití příkazu `npm` můžete v některých operačních systémech obdržet chybu `Error: EPERM, chmod '/usr/local/bin/..'` s žádostí, abyste spustili účet jako správce. Pokud k tomu dojde, použijte příkaz `sudo` ke spuštění příkazu `npm` na vyšší úrovni oprávnění.

#### Obdrželi jste chybu DTrace?

Při instalaci Restify se může zobrazit něco podobného:

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

Restify poskytuje výkonný mechanismus pro trasování volání REST pomocí DTrace. Nicméně, na mnoha operačních systémech není DTrace k dispozici. Tyto chyby můžete bezpečně ignorovat.

Výstup příkazu by měl vypadat přibližně takto:

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

## Instalace Passportu ve vašem webovém rozhraní API

[Passport](http://passportjs.org/) je ověřovací middleware pro Node.js. Passport je velmi flexibilní a modulární a lze ho snadno nainstalovat v jakékoli webové aplikaci využívající Express nebo Restify. Komplexní sada strategií podporuje ověřování pomocí uživatelského jména a hesla, Facebooku, Twitteru a dalších. Vyvinuli jsme strategii pro Azure AD. Nainstalujete tento modul a poté přidáte modul plug-in strategie Azure AD.

V příkazovém řádku přejděte do adresáře `azuread`, pokud v něm ještě nejste.

Zadejte následující příkaz pro instalaci Passportu:

`npm install passport`

Výstup příkazu by měl se měl podobat tomuto:

    passport@0.1.17 node_modules\passport
    ├── pause@0.0.1
    └── pkginfo@0.2.3

## Přidání passport-azuread do webového rozhraní API

Nyní přidejte strategii OAuth pomocí `passport-azuread` – sady strategií, které propojují Azure AD s Passportem. Použijte tuto strategii pro nosné tokeny v ukázce REST API.

> [AZURE.NOTE] Přestože OAuth2 poskytuje rozhraní, ve kterém mohou být vydávané všechny známé typy tokenů, rozšířeného využití se dostalo pouze určitým typům tokenů. Tokeny pro ochranu koncových bodů jsou nosné tokeny. Ty jsou nejčastěji vydávaným typem tokenů v OAuth2. Mnoho implementací předpokládá, že jsou nosné tokeny jediným typem vydávaných tokenů.

V příkazovém řádku přejděte do adresáře `azuread`, pokud v něm ještě nejste.

Zadejte následující příkaz pro instalaci modulu Passport `passport-azure-ad`:

`npm install passport-azure-ad`

Výstup příkazu by měl se měl podobat tomuto:

``
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
``

## Přidání modulů MongoDB do webového rozhraní API

MongoDB budete používat jako úložiště dat. Z tohoto důvodu musíte nainstalovat jak Mongoose, často používaný modul plug-in pro správu modelů a schémat, tak i ovladač databáze, také nazývaný MongoDB.

* `npm install mongoose`
* `npm install mongodb`

## Instalace dalších modulů

Dále nainstalujte zbývající požadované moduly.

V příkazovém řádku přejděte do adresáře `azuread`, pokud v něm ještě nejste:

`cd azuread`

Zadejte následující příkazy pro instalaci modulů do svého adresáře `node_modules`:

* `npm install crypto`
* `npm install assert-plus`
* `npm install posix-getopt`
* `npm install util`
* `npm install path`
* `npm install connect`
* `npm install xml-crypto`
* `npm install xml2js`
* `npm install xmldom`
* `npm install async`
* `npm install request`
* `npm install underscore`
* `npm install grunt-contrib-jshint@0.1.1`
* `npm install grunt-contrib-nodeunit@0.1.2`
* `npm install grunt-contrib-watch@0.2.0`
* `npm install grunt@0.4.1`
* `npm install xtend@2.0.3`
* `npm install bunyan`
* `npm update`


## Vytvoření souboru server.js se závislostmi

Soubor `server.js` bude poskytovat většinu funkčnosti vašeho serveru webového rozhraní API. Do tohoto souboru budete přidávat většinu kódu. Pro produkční účely byste měli funkčnost rozdělit do menších souborů, jako například samostatné trasy a ovladače. Pro účely tohoto kurzu použijte pro tuto funkčnost soubor server.js.

V příkazovém řádku přejděte do adresáře `azuread`, pokud v něm ještě nejste:

`cd azuread`

V editoru vytvořte soubor `server.js`. Přidejte následující informace:

```Javascript
'use strict';
/**
* Module dependencies.
*/
var fs = require('fs');
var path = require('path');
var util = require('util');
var assert = require('assert-plus');
var mongoose = require('mongoose/');
var bunyan = require('bunyan');
var restify = require('restify');
var config = require('./config');
var passport = require('passport');
var OIDCBearerStrategy = require('passport-azure-ad').BearerStrategy;
```

Uložte soubor. Vrátíte se k němu později.

## Vytvoření souboru config.js pro ukládání nastavení Azure AD

Tento soubor s kódem předává parametry konfigurace z portálu Azure AD do souboru `Passport.js`. Tyto hodnoty konfigurace jste vytvořili, když jste přidali webové rozhraní API do portálu v první části tohoto návodu.  Vysvětlíme, co zadat jako hodnoty těchto parametrů, až zkopírujete kód.

V příkazovém řádku přejděte do adresáře `azuread`, pokud v něm ještě nejste:

`cd azuread`

V editoru vytvořte soubor `config.js`. Přidejte následující informace:

```Javascript
// Don't commit this file to your public repos. This config is for first-run
exports.creds = {
mongoose_auth_local: 'mongodb://localhost/tasklist', // Your mongo auth uri goes here
audience: '<your audience URI>',
identityMetadata: 'https://login.microsoftonline.com/common/.well-known/openid-configuration', // For using Microsoft you should never need to change this.
tenantName:'<tenant name>',
policyName:'b2c_1_<sign in policy name>',
};

```

[AZURE.INCLUDE [active-directory-b2c-devquickstarts-tenant-name](../../includes/active-directory-b2c-devquickstarts-tenant-name.md)]

### Požadované hodnoty

`IdentityMetadata`: Tady bude `passport-azure-ad` hledat konfigurační data pro zprostředkovatele identity. Také zde bude hledat klíče pro ověření webových tokenů JSON. Jestliže používáte Azure AD, pravděpodobně tuto hodnotu nechcete měnit.

`audience`: Identifikátor URI (URI) z portálu, který identifikuje vaši službu. Naše ukázka používá `http://localhost/TodoListService`.

`tenantName`: Název vašeho klienta (například **contoso.onmicrosoft.com**).

`policyName`: Zásada, kterou chcete použít k ověřování tokenů přicházejících na váš server. To by měla být stejná zásada jako ta, kterou používáte pro přihlašování na klientské aplikaci.

> [AZURE.NOTE] Pro naše B2C ve verzi Preview použijte stejné zásady v nastavení klienta i serveru. Pokud jste již dokončili návod a tyto zásady jste už vytvořili, nemusíte to dělat znovu. Vzhledem k tomu, že jste návod dokončili, nemělo by být třeba nastavovat nové zásady u návodů pro klienty na této stránce.

## Přidání konfigurace do souboru server.js

Hodnoty z právě vytvořeného souboru `config.js` je třeba číst napříč aplikací. Přidejte soubor `.config` jako požadovaný prostředek vaší aplikace a poté nastavte globální proměnné na hodnoty v dokumentu `config.js`.

V příkazovém řádku přejděte do adresáře `azuread`, pokud v něm ještě nejste:

`cd azuread`

V editoru otevřete soubor `server.js`. Přidejte následující informace:

```Javascript
var config = require('./config');
```
Do souboru `server.js` přidejte nový oddíl s následujícím kódem:

```Javascript
// We pass these options in to the ODICBearerStrategy.
var options = {
// The URL of the metadata document for your app. We will put the keys for token validation from the URL found in the jwks_uri tag of the in the metadata.
    identityMetadata: config.creds.identityMetadata,
    clientID: config.creds.clientID,
    tenantName: config.creds.tenantName,
    policyName: config.creds.policyName,
    validateIssuer: config.creds.validateIssuer,
    audience: config.creds.audience
};
// array to hold logged-in users and the current logged-in user (owner)
var users = [];
var owner = null;
// Our logger
var log = bunyan.createLogger({
name: 'Microsoft Azure Active Directory Sample'
});
```

## Přidání informací o modelu a schématu MongoDB pomocí Mongoose

Předchozí příprava se vyplatí, protože tyto tři soubory spojíte dohromady ve službu REST API.

Pro tento návod použijte k ukládání úkolů MongoDB, jak je uvedeno výše.

V souboru `config.js`, který jste vytvořili, jste nazvali databázi **tasklist**. To jste také vložili na konec adresy připojení URL `mongoose_auth_local`. Tuto databázi nemusíte předem vytvářet v MongoDB. Databáze se vytvoří během prvního spuštění vaší serverové aplikace, pokud ještě neexistuje.

Poté, co sdělíte serveru, kterou databázi MongoDB má použít, budete muset napsat další kód pro vytvoření modelu a schématu pro serverové úlohy.

### Rozšíření modelu

Tento model schématu je velmi jednoduchý. Podle potřeby ho můžete rozšířit.

`name`: Kdo je přiřazen k úloze. Jedná se o typ **řetězec**.

`task`: Vlastní úloha. Jedná se o typ **řetězec**.

`date`: Doba, kdy se má úloha vykonat. Jedná se o typ **datetime**.

`completed`: Zda je úloha dokončená, nebo ne. Jedná se o typ **Boolean**.

### Vytvoření schématu v kódu

V příkazovém řádku přejděte do adresáře `azuread`, pokud v něm ještě nejste:

`cd azuread`

V editoru otevřete soubor `server.js`. Pod položku konfigurace přidejte následující informace:

```Javascript
// MongoDB setup
// Set up some configuration
var serverPort = process.env.PORT || 8080;
var serverURI = (process.env.PORT) ? config.creds.mongoose_auth_mongohq : config.creds.mongoose_auth_local;
// Connect to MongoDB
global.db = mongoose.connect(serverURI);
var Schema = mongoose.Schema;
log.info('MongoDB Schema loaded');
```
To se připojí k serveru MongoDB a předá zpět objekt schématu.

### Použití schématu pro vytvoření modelu v kódu

Pod kód, který jste napsali výše, přidejte následující kód:

```Javascript
// Here we create a schema to store our tasks and users. Pretty simple schema for now.
var TaskSchema = new Schema({
owner: String,
task: String,
completed: Boolean,
date: Date
});
// Use the schema to register a model
mongoose.model('Task', TaskSchema);
var Task = mongoose.model('Task');
```
Nejprve vytvoříte schéma a poté vytvoříte objekt modelu, který použijete k ukládání dat napříč kódem, když definujete svoje **trasy**.

## Přidání tras pro serveru úloh REST API

Teď, když máte model databáze, se kterým můžete pracovat, přidejte trasy, které budete používat pro svůj server REST API.

### O trasách v Restify

Trasy v Restify fungují úplně stejně, jako při použití balíku Express. Trasy se definují pomocí identifikátoru URI, který by měly volat klientské aplikace.  Ve většině případů byste trasy měli definovat v samostatném souboru. Pro účely tohoto kurzu umístíte svoje cesty do souboru `server.js`. Doporučujeme, abyste je pro produkční použití oddělili do samostatného souboru.

Typický vzor trasy Restify je:

```Javascript
function createObject(req, res, next) {
// do work on Object
_object.name = req.params.object; // passed value is in req.params under object
///...
return next(); // keep the server going
}
....
server.post('/service/:add/:object', createObject); // calls createObject on routes that match this.
```

Toto je vzor na naprosto základní úrovni. Restify a Express poskytují mnohem hlubší funkčnost, jako například definování typů aplikací a provádění komplexního trasování napříč různými koncovými body. Pro účely tohoto kurzu ponecháme tyto trasy co nejjednodušší.

#### Přidání výchozích tras na server

Nyní přidáte do základní trasy CRUD – **Vytvořit**, **Načíst**, **Aktualizovat**, a **Odstranit**.

V příkazovém řádku přejděte do adresáře `azuread`, pokud v něm ještě nejste:

`cd azuread`

V editoru otevřete soubor `server.js`. Pod položky databáze, které jste přidali výše, přidejte následující informace:

```Javascript
/**
*
* APIs for our REST task server
*/
// Create a task
function createTask(req, res, next) {
// Restify currently has a bug that doesn't allow you to set default headers
// The headers comply with CORS and allow us to mongodbServer our response to any origin
res.header("Access-Control-Allow-Origin", "*");
res.header("Access-Control-Allow-Headers", "X-Requested-With");
// Create a new task model, fill it up and save it to Mongodb
var _task = new Task();
if (!req.params.task) {
req.log.warn({
params: p
}, 'createTodo: missing task');
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
// Delete a task by name
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
// Delete all tasks
function removeAll(req, res, next) {
Task.remove();
res.send(204);
return next();
}
// Get a specific task based on name
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
// Restify currently has a bug that doesn't allow you to set default headers
// The headers comply with CORS and allow us to mongodbServer our response to any origin
res.header("Access-Control-Allow-Origin", "*");
res.header("Access-Control-Allow-Headers", "X-Requested-With");
log.info("listTasks was called for: ", owner);
Task.find({
owner: owner
}).limit(20).sort('date').exec(function(err, data) {
if (err)
return next(err);
if (data.length > 0) {
log.info(data);
}
if (!data.length) {
log.warn(err, "There is no tasks in the database. Add one!");
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

#### Přidání zpracování chyb pro trasy

Měli byste přidat nějaké zpracování chyb, abyste mohli srozumitelně sdělit klientu jakékoli problémy, na které narazíte.

Pod kód, který jste napsali výše, přidejte následující kód:

```Javascript
///--- Errors for communicating something interesting back to the client
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


## Vytvoření serveru

Nyní jste definovali databázi a přidali trasy. Poslední věcí, kterou musíte udělat, je přidání instance serveru, která bude spravovat vaše volání.

Restify a Express poskytují široké možnosti přizpůsobení serveru REST API, ale zde použijeme to nejzákladnější nastavení.

```Javascript
/**
* Our server
*/
var server = restify.createServer({
name: "Microsoft Azure Active Directory TODO Server",
version: "2.0.1"
});
// Ensure that we don't drop data on uploads
server.pre(restify.pre.pause());
// Clean up sloppy paths like //todo//////1//
server.pre(restify.pre.sanitizePath());
// Handle annoying user agents (curl)
server.pre(restify.pre.userAgentConnection());
// Set a per request bunyan logger (with requestid filled in)
server.use(restify.requestLogger());
// Allow five requests/second by IP, and burst to 10
server.use(restify.throttle({
burst: 10,
rate: 5,
ip: true,
}));
// Use the common stuff you probably want
server.use(restify.acceptParser(server.acceptable));
server.use(restify.dateParser());
server.use(restify.queryParser());
server.use(restify.gzipResponse());
server.use(restify.bodyParser({
mapParams: true
}));
```
## Přidání tras na server (bez ověřování)

```Javascript
/// Now the real handlers. Here we just CRUD
/**
/*
/* Each of these handlers is protected by our OIDCBearerStrategy by invoking 'oidc-bearer'
/* in the passport.authenticate() method. We set 'session: false' as REST is stateless and
/* we don't need to maintain session state. You can experiment with removing API protection
/* by removing the passport.authenticate() method like this:
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
// Register a default '/' handler
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
## Před přidáním podpory OAuth spusťte server

Před přidáním ověřování byste měli otestovat svůj server.

Nejjednodušší způsob je pomocí příkazu `curl` v příkazovém řádku. Ale předtím budete potřebovat jednoduchý nástroj, který vám umožní parsovat výstup jako JSON. Nejprve nainstalujte nástroj JSON.

`$npm install -g jsontool`

Tím se globálně nainstaluje nástroj JSON. Po instalaci nástroje JSON otestujte server:

Ujistěte se, že je spuštěna instance MongoDB.

`$sudo mongodb`

Přejděte do adresáře `azuread` a použijte příkaz `curl`.

`$ cd azuread`
`$ node server.js`

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

Přidejte úkol:

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
Úkoly pro uživatele „Brandon“ můžete zobrazit tímto způsobem:

`$ curl -isS http://127.0.0.1:8080/tasks/brandon/`

Pokud toto funguje, jste připraveni pro přidání OAuth na server REST API.

Nyní máte server REST API s MongoDB.

## Přidání ověřování na server REST API

Když teď máte fungující server REST API, můžete ho využít s Azure AD.

V příkazovém řádku přejděte do adresáře `azuread`, pokud v něm ještě nejste:

`cd azuread`

### Použití OIDCBearerStrategy, která je součástí passport-azure-ad

Zatím jste vytvořili typický server REST se seznamem úkolů bez jakéhokoli druhu ověřování. Nyní můžete začít dávat dohromady ověřování.

Nejprve musíte určit, že chcete použít Passport. Pod ostatní konfigurace serveru přidejte následující:

```Javascript
// Let's start using Passport.js

server.use(passport.initialize()); // Starts Passport
server.use(passport.session()); // Provides session support
```

> [AZURE.TIP]
Při psaní rozhraní API byste vždy měli propojit data s něčím jedinečným z tokenu, co uživatel nemůže zfalšovat. Server ukládá položky úkolů na základě **ID objektu** uživatele v tokenu (zavolaném prostřednictvím token.oid) a ten se ukládá do pole „vlastník“. To zajišťuje, že pouze tento uživatel má přístup ke svým položkám úkolů a že nikdo jiný nemůže přistupovat k vloženým položkám úkolů. V rozhraní API se „vlastník“ nijak neprojevuje, takže externí uživatelé nemohou přistupovat k položkám úkolů jiných uživatelů ani když jsou ověřeni.

Dále použijte nosnou strategii, která je součástí `passport-azure-ad`. (Prozatím si kód pouze prohlédneme.) Po tom, co jste vložili dříve, přidejte následující:

```Javascript
/**
/*
/* Calling the OIDCBearerStrategy and managing users
/*
/* Passport pattern provides the need to manage users and info tokens
/* with a FindorCreate() method that must be provided by the implementer.
/* Here we just autoregister any user and implement a FindById().
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
var oidcStrategy = new OIDCBearerStrategy(options,
function(token, done) {
log.info('verifying the user');
log.info(token, 'was the token retrieved');
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
passport.use(oidcStrategy);
```

Passport používá pro všechny svoje strategie (včetně Twitteru a Facebooku) vzor, který je podobný strategiím, kterými se řídí všichni autoři strategií. Předáváte jí `function()`, která jako parametry přijímá `token` a `done`. Po dokončení veškeré práce se k vám strategie vrátí. Poté byste měli uložit uživatele a token, abyste ho nemuseli znovu vyžadovat.

> [AZURE.IMPORTANT]
Výše uvedený kód přijímá jakéhokoli uživatele, který se na vašem serveru ověří. To se označuje jako automatická registrace. Na produkčních serverech byste neměli chtít vpouštět jakékoli uživatele bez toho, aby předtím prošli registračním procesem, který jste zvolili. To je obvykle vzor, který můžete vidět u uživatelských aplikací, které vám umožňují zaregistrovat se pomocí Facebooku, ale poté vás požádají o vyplnění dodatečných informací. Pokud by v tomto případě nešlo o program v příkazovém řádku, mohli bychom extrahovat e-mail z vráceného objektu tokenu a poté požádat uživatele o vyplnění dodatečných informací. Protože se jedná o testovací server, jednoduše je přidáme do databáze v paměti.

### Ochrana koncových bodů

Koncové body ochráníte zadáním volání `passport.authenticate()` pomocí protokolu, který chcete použít.

Trasu v kódu serveru můžete upravit, aby dělala něco zajímavějšího:

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

## Znovu spusťte serverovou aplikaci, abyste ověřili, že vás odmítne

Můžete znovu použít příkaz `curl`, abyste zjistili, zda jsou již koncové body chráněné pomocí OAuth2. Proveďte to předtím, než proti tomuto koncovému bodu spustíte jakoukoli klientskou sadu SDK. Vrácené hlavičky by vám měly dostatečně napovědět, že jste na správné cestě.

Ujistěte se, že je spuštěna instance MongoDB.

    $sudo mongodb

Přejděte do adresáře a použijte příkaz `curl`:

    $ cd azuread
    $ node server.js

Zkuste základní požadavek POST:

`$ curl -isS -X POST http://127.0.0.1:8080/tasks/brandon/Hello`

```Shell
HTTP/1.1 401 Unauthorized
Connection: close
WWW-Authenticate: Bearer realm="Users"
Date: Tue, 14 Jul 2015 05:45:03 GMT
Transfer-Encoding: chunked
```

Chyba 401 je odpověď, kterou si přejete. Označuje, že se vrstva Passportu pokouší přesměrovat na koncový bod ověření.


## Nyní máte službu REST API, která používá OAuth2

S tímto serverem jste došli nejdál, co to jde bez použití klienta kompatibilního s OAuth2. Pro to budete potřebovat další návod.

Pokud hledáte pouze informace o tom, jak implementovat REST API pomocí Restify a OAuth2, nyní máte dostatečný kód, abyste mohli pokračovat s vývojem svojí služby a abyste mohli na tomto příkladu stavět.

Pro srovnání je hotová ukázka (bez vašich hodnot nastavení) [k dispozici jako soubor .zip](https://github.com/AzureADQuickStarts/B2C-WebAPI-nodejs/archive/complete.zip). Můžete ho také klonovat z GitHubu:

```git clone --branch complete https://github.com/AzureADQuickStarts/B2C-WebAPI-nodejs.git```


## Další kroky

Nyní se můžete přesunout k pokročilejším tématům, jako například:

[Připojení k webovému rozhraní API pomocí iOS s B2C](active-directory-b2c-devquickstarts-ios.md)



<!--HONumber=Jun16_HO2-->


