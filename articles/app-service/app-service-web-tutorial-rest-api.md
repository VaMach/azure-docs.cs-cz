---
title: Aplikace Node.js API v Azure App Service | Dokumentace Microsoftu
description: "Podívejte se, jak vytvořit rozhraní Node.js RESTful API a nasadit ho do aplikace API v Azure App Service."
services: app-service\api
documentationcenter: node
author: bradygaster
manager: erikre
editor: 
ms.assetid: a820e400-06af-4852-8627-12b3db4a8e70
ms.service: app-service-api
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: nodejs
ms.topic: tutorial
ms.date: 06/13/2017
ms.author: rachelap
ms.custom: mvc, devcenter
ms.openlocfilehash: 2170ac7df3b894c8d19b432abdcfef5c7fd75ff4
ms.sourcegitcommit: 0e4491b7fdd9ca4408d5f2d41be42a09164db775
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 12/14/2017
---
# <a name="build-a-nodejs-restful-api-and-deploy-it-to-an-api-app-in-azure"></a>Sestavení rozhraní Node.js RESTful API a jeho nasazení do aplikace API v Azure

V rámci tohoto rychlého startu se dozvíte, jak vytvořit rozhraní REST API napsané v Node.js [Express](http://expressjs.com/) s použitím definice [Swaggeru](http://swagger.io/) a nasadit ho do Azure. Vytvoříte aplikaci pomocí nástrojů pro příkazový řádek, nakonfigurujete prostředky pomocí [Azure CLI](https://docs.microsoft.com/cli/azure/get-started-with-azure-cli) a pomocí Gitu aplikaci nasadíte.  Po dokončení budete mít funkční ukázkové rozhraní REST API, které běží na Azure.

## <a name="prerequisites"></a>Požadavky

* [Git](https://git-scm.com/)
* [Node.js a NPM](https://nodejs.org/)

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

Pokud se rozhodnete nainstalovat a používat rozhraní příkazového řádku (CLI) místně, musíte mít spuštěnou verzi Azure CLI 2.0 nebo novější. Verzi zjistíte spuštěním příkazu `az --version`. Pokud potřebujete instalaci nebo upgrade, přečtěte si téma [Instalace Azure CLI 2.0]( /cli/azure/install-azure-cli). 

## <a name="prepare-your-environment"></a>Příprava prostředí

1. V okně terminálu naklonujte spuštěním následujícího příkazu úložiště ukázku do místního počítače.

    ```bash
    git clone https://github.com/Azure-Samples/app-service-api-node-contact-list
    ```

2. Přejděte do adresáře, který obsahuje vzorový kód.

    ```bash
    cd app-service-api-node-contact-list
    ```

3. Na místní počítač nainstalujte [Swaggerize](https://www.npmjs.com/package/swaggerize-express). Swaggerize je nástroj, který generuje kód Node.js pro REST API z definice Swaggeru.

    ```bash
    npm install -g yo
    npm install -g generator-swaggerize
    ```

## <a name="generate-nodejs-code"></a>Generování kódu Node.js 

V této části kurzu modelujeme pracovní postup vývoje rozhraní API, při kterém nejprve vytvoříte metadata Swagger a pak pomocí nich automaticky vygenerujete (scaffold) serverový kód pro rozhraní API. 

Změňte adresář na *spouštěcí* složku a potom spusťte `yo swaggerize`. Swaggerize vytvoří projekt Node.js pro vaše rozhraní API z definice Swaggeru v *api.json*.

```bash
cd start
yo swaggerize --apiPath api.json --framework express
```

Pokud se Swaggerize zeptá na název projektu, použijte *ContactList*.
   
   ```bash
   Swaggerize Generator
   Tell us a bit about your application
   ? What would you like to call this project: ContactList
   ? Your name: Francis Totten
   ? Your github user name: fabfrank
   ? Your email: frank@fabrikam.net
   ```
   
## <a name="customize-the-project-code"></a>Přizpůsobení kódu projektu

1. Zkopírujte složku *lib* do složky *ContactList*, kterou vytvořil příkaz `yo swaggerize`, a potom změňte adresář na *ContactList*.

    ```bash
    cp -r lib/ ContactList/
    cd ContactList
    ```

2. Nainstalujte moduly NPM `jsonpath` a `swaggerize-ui`. 

    ```bash
    npm install --save jsonpath swaggerize-ui
    ```

3. Nahraďte kód v souboru *handlers/contacts.js* následujícím kódem: 
    ```javascript
    'use strict';

    var repository = require('../lib/contactRepository');

    module.exports = {
        get: function contacts_get(req, res) {
            res.json(repository.all())
        }
    };
    ```
    Tento kód používá data JSON uložená v souboru *lib/contacts.json*, který poskytuje *lib/contactRepository.js*. Nový kód *contacts.js* vrátí všechny kontakty v úložišti jako datovou část JSON. 

4. Nahraďte kód v souboru **handlers/contacts/{id}.js** následujícím kódem:

    ```javascript
    'use strict';

    var repository = require('../../lib/contactRepository');

    module.exports = {
        get: function contacts_get(req, res) {
            res.json(repository.get(req.params['id']));
        }    
    };
    ```

    Díky tomuto kódu si můžete pomocí proměnné cesty nechat vrátit pouze kontakt s daným ID.

5. Nahraďte kód v souboru **server.js** tímto kódem:

    ```javascript
    'use strict';

    var port = process.env.PORT || 8000; 

    var http = require('http');
    var express = require('express');
    var bodyParser = require('body-parser');
    var swaggerize = require('swaggerize-express');
    var swaggerUi = require('swaggerize-ui'); 
    var path = require('path');
    var fs = require("fs");
    
    fs.existsSync = fs.existsSync || require('path').existsSync;

    var app = express();

    var server = http.createServer(app);

    app.use(bodyParser.json());

    app.use(swaggerize({
        api: path.resolve('./config/swagger.json'),
        handlers: path.resolve('./handlers'),
        docspath: '/swagger' 
    }));

    // change four
    app.use('/docs', swaggerUi({
        docs: '/swagger'  
    }));

    server.listen(port, function () { 
    });
    ```   

    Tento kód provede několik malých změn, aby bylo možné pracovat se službou Azure App Service, a publikuje interaktivní webové rozhraní pro vaše rozhraní API.

### <a name="test-the-api-locally"></a>Místní otestování rozhraní API

1. Spusťte aplikaci Node.js.
    ```bash
    npm start
    ```
    
2. Přejděte na http://localhost:8000/contacts a zobrazte JSON pro celý seznam kontaktů.
   
   ```json
    {
        "id": 1,
        "name": "Barney Poland",
        "email": "barney@contoso.com"
    },
    {
        "id": 2,
        "name": "Lacy Barrera",
        "email": "lacy@contoso.com"
    },
    {
        "id": 3,
        "name": "Lora Riggs",
        "email": "lora@contoso.com"
    }
   ```

3. Přejděte na http://localhost:8000/contacts/2 a zobrazte kontakt s `id` dvě.
   
    ```json
    { 
        "id": 2,
        "name": "Lacy Barrera",
        "email": "lacy@contoso.com"
    }
    ```

4. Otestujte rozhraní API pomocí webového rozhraní Swaggeru na http://localhost:8000/docs.
   
    ![Webové rozhraní Swaggeru](media/app-service-web-tutorial-rest-api/swagger-ui.png)

## <a id="createapiapp"></a> Vytvoření aplikace API

V této části pomocí Azure CLI 2.0 vytvoříte prostředky, které budou hostovat rozhraní API ve službě Azure App Service. 

1.  Přihlaste se k předplatnému Azure pomocí příkazu [az login](/cli/azure/?view=azure-cli-latest#az_login) a postupujte podle pokynů na obrazovce.

    ```azurecli-interactive
    az login
    ```

2. Pokud máte více předplatných Azure, změňte výchozí předplatné na to, které chcete použít.

    ````azurecli-interactive
    az account set --subscription <name or id>
    ````

3. [!INCLUDE [Create resource group](../../includes/app-service-api-create-resource-group.md)] 

4. [!INCLUDE [Create app service plan](../../includes/app-service-api-create-app-service-plan.md)]

5. [!INCLUDE [Create API app](../../includes/app-service-api-create-api-app.md)] 


## <a name="deploy-the-api-with-git"></a>Nasazení rozhraní API pomocí Gitu

Nasaďte kód do aplikace API vynuceným doručením (push) potvrzených změn z místního úložiště Git do služby Azure App Service.

1. [!INCLUDE [Configure your deployment credentials](../../includes/configure-deployment-user-no-h.md)] 

2. Inicializujte nové úložiště v adresáři *ContactList*. 

    ```bash
    git init .
    ```

3. Vylučte z Gitu adresář *node_modules*, který vytvořil nástroj npm v dřívějším kroku v tomto kurzu. V aktuálním adresáři vytvořte nový soubor `.gitignore` a kdekoli v souboru přidejte nový řádek s následujícím textem.

    ```
    node_modules/
    ```
    Potvrďte, že `git status` ignoruje složku `node_modules`.

4. Potvrďte změny do úložiště.
    ```bash
    git add .
    git commit -m "initial version"
    ```

5. [!INCLUDE [Push to Azure](../../includes/app-service-api-git-push-to-azure.md)]  
 
## <a name="test-the-api--in-azure"></a>Otestování rozhraní API v Azure

1. V prohlížeči otevřete http://app_name.azurewebsites.net/contacts. Uvidíte, že se vrátil stejný JSON, jako když jste poslali žádost místně dříve v tomto kurzu.

   ```json
   {
       "id": 1,
       "name": "Barney Poland",
       "email": "barney@contoso.com"
   },
   {
       "id": 2,
       "name": "Lacy Barrera",
       "email": "lacy@contoso.com"
   },
   {
       "id": 3,
       "name": "Lora Riggs",
       "email": "lora@contoso.com"
   }
   ```

2. V prohlížeči přejděte na koncový bod `http://app_name.azurewebsites.net/docs` a vyzkoušejte spuštění uživatelského rozhraní Swaggeru v Azure.

    ![Uživatelské rozhraní Swaggeru](media/app-service-web-tutorial-rest-api/swagger-azure-ui.png)

    Teď můžete nasadit aktualizace ukázkového rozhraní API do Azure jednoduše tak, že vynuceně doručíte (push) potvrzené změny do úložiště Azure Git.

## <a name="clean-up"></a>Vyčištění

Pokud chcete vyčistit prostředky vytvořené v rámci tohoto rychlého startu, spusťte následující příkaz rozhraní příkazového řádku Azure:

```azurecli-interactive
az group delete --name myResourceGroup
```

## <a name="next-step"></a>Další krok 
> [!div class="nextstepaction"]
> [Mapování existujícího vlastního názvu DNS na Azure Web Apps](app-service-web-tutorial-custom-domain.md)

