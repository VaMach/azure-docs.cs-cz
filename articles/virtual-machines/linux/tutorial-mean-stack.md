---
title: "Vytvoření střední hodnotu zásobníku na virtuální počítač s Linuxem v Azure | Microsoft Docs"
description: "Naučte se vytvářet zásobníku MongoDB, Express, AngularJS a Node.js (střední) na virtuální počítač s Linuxem v Azure."
services: virtual-machines-linux
documentationcenter: virtual-machines
author: davidmu1
manager: timlt
editor: tysonn
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: tutorial
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 08/08/2017
ms.author: davidmu
ms.custom: mvc
ms.openlocfilehash: 1d74ead08dfb63276afb08bdcb7f4e3e3db5bfd3
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/11/2017
---
# <a name="create-a-mongodb-express-angularjs-and-nodejs-mean-stack-on-a-linux-vm-in-azure"></a>Na virtuální počítač s Linuxem v Azure vytvořit zásobníku MongoDB, Express, AngularJS a Node.js (střední)

V tomto kurzu se dozvíte, jak implementovat zásobníku MongoDB, Express, AngularJS a Node.js (střední) na virtuální počítač s Linuxem v Azure. Střední zásobníku, který vytvoříte umožňuje přidávání, odstraňování a výpis knihy v databázi. Získáte informace o těchto tématech:

> [!div class="checklist"]
> * Vytvoření virtuálního počítače s Linuxem
> * Instalovat Node.js
> * Nainstalujte MongoDB a nastavení serveru
> * Expresní instalace a nastavení trasy k serveru
> * Přístup k tras se AngularJS
> * Spuštění aplikace

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

Pokud si zvolíte instalaci a použití rozhraní příkazového řádku místně, tento kurz vyžaduje, že používáte Azure CLI verze verze 2.0.4 nebo novější. Verzi zjistíte spuštěním příkazu `az --version`. Pokud potřebujete instalaci nebo upgrade, přečtěte si téma [Instalace Azure CLI 2.0]( /cli/azure/install-azure-cli).


## <a name="create-a-linux-vm"></a>Vytvoření virtuálního počítače s Linuxem

Vytvořte skupinu prostředků s [vytvořit skupinu az](https://docs.microsoft.com/cli/azure/group#az_group_create) příkazů a vytvoření virtuálního počítače s Linuxem pomocí [vytvořit virtuální počítač az](https://docs.microsoft.com/cli/azure/vm#az_vm_create) příkaz. Skupina prostředků Azure je logický kontejner, ve kterém se nasazují a spravují prostředky Azure.

Následující příklad používá rozhraní příkazového řádku Azure k vytvoření skupiny prostředků s názvem *myResourceGroupMEAN* v *eastus* umístění. Virtuální počítač je vytvořen s názvem *Můjvp* s klíči SSH, pokud už neexistují ve výchozím umístění klíče. Pokud chcete použít konkrétní sady klíčů, použijte možnost--ssh klíč hodnota.

```azurecli-interactive
az group create --name myResourceGroupMEAN --location eastus
az vm create \
    --resource-group myResourceGroupMEAN \
    --name myVM \
    --image UbuntuLTS \
    --admin-username azureuser \
    --admin-password 'Azure12345678!' \
    --generate-ssh-keys
az vm open-port --port 3300 --resource-group myResourceGroupMEAN --name myVM
```

Po vytvoření virtuálního počítače Azure CLI uvádí informace podobně jako v následujícím příkladu: 

```azurecli-interactive
{
  "fqdns": "",
  "id": "/subscriptions/{subscription-id}/resourceGroups/myResourceGroupMEAN/providers/Microsoft.Compute/virtualMachines/myVM",
  "location": "eastus",
  "macAddress": "00-0D-3A-23-9A-49",
  "powerState": "VM running",
  "privateIpAddress": "10.0.0.4",
  "publicIpAddress": "13.72.77.9",
  "resourceGroup": "myResourceGroupMEAN"
}
```
Poznamenejte si `publicIpAddress`. Tato adresa se používá pro přístup k virtuálnímu počítači.

Použijte následující příkaz k vytvoření relace SSH s virtuálním Počítačem. Nezapomeňte použít správné veřejnou IP adresu. V našem příkladu výše naše IP adresa byla 13.72.77.9.

```bash
ssh azureuser@13.72.77.9
```

## <a name="install-nodejs"></a>Instalovat Node.js

[Node.js](https://nodejs.org/en/) je prostředí JavaScript runtime, který je založený na JavaScript v8: modul pro Chrome. Node.js se používá v tomto kurzu nastavit Express trasy a AngularJS řadiče.

Ve virtuálním počítači, pomocí prostředí bash, kterou jste otevřeli pomocí protokolu SSH nainstalujte si Node.js.

```bash
sudo apt-get install -y nodejs
```

## <a name="install-mongodb-and-set-up-the-server"></a>Nainstalujte MongoDB a nastavení serveru
[MongoDB](http://www.mongodb.com) ukládá data v dokumentech flexibilní, jako JSON. Pole v databázi se může lišit z dokumentu do dokumentu a struktura dat lze změnit v čase. Pro naše ukázková aplikace přidáváme kniha záznamy MongoDB, které obsahují název adresáře, číslo isbn, autora a počet stránek. 

1. Ve virtuálním počítači, pomocí prostředí bash, kterou jste otevřeli pomocí protokolu SSH nastavte klíč MongoDB.

    ```bash
    sudo apt-key adv --keyserver hkp://keyserver.ubuntu.com:80 --recv 0C49F3730359A14518585931BC711F9BA15703C6
    echo "deb [ arch=amd64 ] http://repo.mongodb.org/apt/ubuntu trusty/mongodb-org/3.4 multiverse" | sudo tee /etc/apt/sources.list.d/mongodb-org-3.4.list
    ```

2. Aktualizujte správce balíčků s klíčem.
  
    ```bash
    sudo apt-get update
    ```

3. Nainstalujte MongoDB.

    ```bash
    sudo apt-get install -y mongodb
    ```

4. Spuštění serveru.

    ```bash
    sudo service mongodb start
    ```

5. Také je potřeba nainstalovat [textu analyzátor](https://www.npmjs.com/package/body-parser-json) balíček a pomoci tak zpracování JSON předané požadavky na server.

    Nainstalujte npm Správce balíčků.

    ```bash
    sudo apt-get install npm
    ```

    Nainstalujte balíček analyzátor textu.
    
    ```bash
    sudo npm install body-parser
    ```

6. Vytvořte složku s názvem *knihy* a přidejte do ní s názvem souboru *server.js* obsahující konfigurace na webovém serveru.

    ```node.js
    var express = require('express');
    var bodyParser = require('body-parser');
    var app = express();
    app.use(express.static(__dirname + '/public'));
    app.use(bodyParser.json());
    require('./apps/routes')(app);
    app.set('port', 3300);
    app.listen(app.get('port'), function() {
        console.log('Server up: http://localhost:' + app.get('port'));
    });
    ```

## <a name="install-express-and-set-up-routes-to-the-server"></a>Expresní instalace a nastavení trasy k serveru

[Express](https://expressjs.com) je minimalistické a flexibilní Node.js webové aplikace rozhraní, které poskytuje funkce pro webové a mobilní aplikace. Express se používá v tomto kurzu předat sešit informací na Internet a z našich databáze MongoDB. [Mongoose](http://mongoosejs.com) poskytuje jednoduché a na základě schématu řešení pro modelování data aplikací. Mongoose se používá v tomto kurzu poskytuje schéma adresáře pro databázi.

1. Expresní instalace a Mongoose.

    ```bash
    sudo npm install express mongoose
    ```

2. V *knihy* složky, vytvořte složku s názvem *aplikace* a přidejte do souboru s názvem *routes.js* s express trasy definované.

    ```node.js
    var Book = require('./models/book');
    module.exports = function(app) {
      app.get('/book', function(req, res) {
        Book.find({}, function(err, result) {
          if ( err ) throw err;
          res.json(result);
        });
      }); 
      app.post('/book', function(req, res) {
        var book = new Book( {
          name:req.body.name,
          isbn:req.body.isbn,
          author:req.body.author,
          pages:req.body.pages
        });
        book.save(function(err, result) {
          if ( err ) throw err;
          res.json( {
            message:"Successfully added book",
            book:result
          });
        });
      });
      app.delete("/book/:isbn", function(req, res) {
        Book.findOneAndRemove(req.query, function(err, result) {
          if ( err ) throw err;
          res.json( {
            message: "Successfully deleted the book",
            book: result
          });
        });
      });
      var path = require('path');
      app.get('*', function(req, res) {
        res.sendfile(path.join(__dirname + '/public', 'index.html'));
      });
    };
    ```

3. V *aplikace* složky, vytvořte složku s názvem *modely* a přidejte do souboru s názvem *book.js* pomocí modelu konfigurace adresáře definované.  

    ```node.js
    var mongoose = require('mongoose');
    var dbHost = 'mongodb://localhost:27017/test';
    mongoose.connect(dbHost);
    mongoose.connection;
    mongoose.set('debug', true);
    var bookSchema = mongoose.Schema( {
      name: String,
      isbn: {type: String, index: true},
      author: String,
      pages: Number
    });
    var Book = mongoose.model('Book', bookSchema);
    module.exports = mongoose.model('Book', bookSchema); 
    ```

## <a name="access-the-routes-with-angularjs"></a>Přístup k tras se AngularJS

[AngularJS](https://angularjs.org) poskytuje webové rozhraní pro vytváření dynamického zobrazení v webových aplikací. V tomto kurzu používáme AngularJS připojit naše webové stránky s Express a provádět akce na databázi našich seznamu.

1. Změna adresáře zpět do *knihy* (`cd ../..`) a pak vytvořte složku s názvem *veřejné* a přidejte do souboru s názvem *script.js* s konfigurací řadiče definované.

    ```node.js
    var app = angular.module('myApp', []);
    app.controller('myCtrl', function($scope, $http) {
      $http( {
        method: 'GET',
        url: '/book'
      }).then(function successCallback(response) {
        $scope.books = response.data;
      }, function errorCallback(response) {
        console.log('Error: ' + response);
      });
      $scope.del_book = function(book) {
        $http( {
          method: 'DELETE',
          url: '/book/:isbn',
          params: {'isbn': book.isbn}
        }).then(function successCallback(response) {
          console.log(response);
        }, function errorCallback(response) {
          console.log('Error: ' + response);
        });
      };
      $scope.add_book = function() {
        var body = '{ "name": "' + $scope.Name + 
        '", "isbn": "' + $scope.Isbn +
        '", "author": "' + $scope.Author + 
        '", "pages": "' + $scope.Pages + '" }';
        $http({
          method: 'POST',
          url: '/book',
          data: body
        }).then(function successCallback(response) {
          console.log(response);
        }, function errorCallback(response) {
          console.log('Error: ' + response);
        });
      };
    });
    ```
    
2. V *veřejné* složky, vytvořte soubor s názvem *index.html* s webovou stránkou definované.

    ```html
    <!doctype html>
    <html ng-app="myApp" ng-controller="myCtrl">
      <head>
        <script src="https://ajax.googleapis.com/ajax/libs/angularjs/1.6.4/angular.min.js"></script>
        <script src="script.js"></script>
      </head>
      <body>
        <div>
          <table>
            <tr>
              <td>Name:</td> 
              <td><input type="text" ng-model="Name"></td>
            </tr>
            <tr>
              <td>Isbn:</td>
              <td><input type="text" ng-model="Isbn"></td>
            </tr>
            <tr>
              <td>Author:</td> 
              <td><input type="text" ng-model="Author"></td>
            </tr>
            <tr>
              <td>Pages:</td>
              <td><input type="number" ng-model="Pages"></td>
            </tr>
          </table>
          <button ng-click="add_book()">Add</button>
        </div>
        <hr>
        <div>
          <table>
            <tr>
              <th>Name</th>
              <th>Isbn</th>
              <th>Author</th>
              <th>Pages</th>
            </tr>
            <tr ng-repeat="book in books">
              <td><input type="button" value="Delete" data-ng-click="del_book(book)"></td>
              <td>{{book.name}}</td>
              <td>{{book.isbn}}</td>
              <td>{{book.author}}</td>
              <td>{{book.pages}}</td>
            </tr>
          </table>
        </div>
      </body>
    </html>
    ```

##  <a name="run-the-application"></a>Spuštění aplikace

1. Změna adresáře zpět do *knihy* (`cd ..`) a spustí se server tak, že spustíte tento příkaz:

    ```bash
    nodejs server.js
    ```

2. Otevřete webový prohlížeč na adrese, které jste si poznamenali pro virtuální počítač. Například *http://13.72.77.9:3300*. Měli byste vidět něco podobného jako na následující stránce:

    ![Záznam adresáře](media/tutorial-mean/meanstack-init.png)

3. Zadejte data do textových polí a klikněte na tlačítko **přidat**. Například:

    ![Přidejte záznam adresáře](media/tutorial-mean/meanstack-add.png)

4. Po aktualizaci stránky, měli byste vidět něco podobného jako na této stránce:

    ![Seznam záznamů adresáře](media/tutorial-mean/meanstack-list.png)

5. Uživatel může klepnout **odstranit** a odebrat záznam adresáře z databáze.

## <a name="next-steps"></a>Další kroky

V tomto kurzu jste vytvořili webovou aplikaci, která uchovává informace o seznamu záznamů pomocí střední hodnotu zásobníku na virtuální počítač s Linuxem. Naučili jste se tyto postupy:

> [!div class="checklist"]
> * Vytvoření virtuálního počítače s Linuxem
> * Instalovat Node.js
> * Nainstalujte MongoDB a nastavení serveru
> * Expresní instalace a nastavení trasy k serveru
> * Přístup k tras se AngularJS
> * Spuštění aplikace

Přechodu na v dalším kurzu se dozvíte, jak zabezpečit webové servery s certifikáty protokolu SSL.

> [!div class="nextstepaction"]
> [Zabezpečení webového serveru pomocí protokolu SSL](tutorial-secure-web-server.md)
