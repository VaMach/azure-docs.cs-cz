<properties 
    pageTitle="Výuka Node.js – kurz k DocumentDB Node.js | Microsoft Azure" 
    description="Naučte se pracovat s Node.js! Tento kurz popisuje, jak pomocí Microsoft Azure DocumentDB ukládat data a přistupovat k nim z webové aplikace Node.js Express hostované na Webech Azure." 
    keywords="Vývoj aplikací, databázový kurz, naučte se pracovat s node.js, kurz k node.js, documentdb, azure, Microsoft azure"
    services="documentdb" 
    documentationCenter="nodejs" 
    authors="AndrewHoh" 
    manager="jhubbard" 
    editor="cgronlun"/>

<tags 
    ms.service="documentdb" 
    ms.workload="data-services" 
    ms.tgt_pltfrm="na" 
    ms.devlang="nodejs" 
    ms.topic="hero-article" 
    ms.date="08/15/2016" 
    ms.author="anhoh"/>

# <a name="_Toc395783175"></a>Sestavení webové aplikace Node.js pomocí DocumentDB

> [AZURE.SELECTOR]
- [.NET](documentdb-dotnet-application.md)
- [Node.js](documentdb-nodejs-application.md)
- [Java](documentdb-java-application.md)
- [Python](documentdb-python-application.md)

Tento kurz Node.js popisuje, jak pomocí služby Azure DocumentDB ukládat data a přistupovat k nim z aplikace Node.js Express hostované na Webech Azure.

Doporučujeme začít následujícím videem, kde se naučíte zřizovat databázový účet Azure DocumentDB a ukládat dokumenty JSON v aplikaci Node.js. 

> [AZURE.VIDEO azure-demo-getting-started-with-azure-documentdb-on-nodejs-in-linux]

Pak se vraťte zpět k tomuto kurzu Node.js, kde se dozvíte odpovědi na následující otázky:

- Jak pracovat s DocumentDB pomocí modulu documentdb npm?
- Jak nasadit webovou aplikaci na Weby Azure?

Postupem v tomto kurzu vytvoříte jednoduchou webovou aplikaci pro správu úkolů, která umožní vytvářet, získávat a dokončovat úkoly. Úkoly se budou ukládat jako dokumenty JSON do Azure DocumentDB.

![Snímek obrazovky aplikace pro seznam úkolů vytvořené v tomto kurzu k Node.js](./media/documentdb-nodejs-application/image1.png)

Nemáte čas absolvovat celý tento kurz a chcete jen získat celé řešení? To není problém, celé ukázkové řešení si můžete stáhnout z [GitHubu][].

## <a name="_Toc395783176"></a>Požadavky

> [AZURE.TIP] V tomto kurzu Node.js se předpokládá, že již máte zkušenosti s používáním Node.js a Webů Azure.

Než budete postupovat podle pokynů tohoto článku, měli byste se ujistit, že máte následující:

- Aktivní účet Azure. Pokud účet nemáte, můžete si během několika minut vytvořit bezplatný zkušební účet. Podrobnosti najdete v článku [Bezplatná zkušební verze Azure](https://azure.microsoft.com/pricing/free-trial/).
- [Node.js][] verze 0.10.29 nebo vyšší
- [Generátor Express](http://www.expressjs.com/starter/generator.html) (lze nainstalovat prostřednictvím `npm install express-generator -g`)
- [Git][]

## <a name="_Toc395637761"></a>Krok 1: Vytvoření databázového účtu DocumentDB

Začněme vytvořením účtu DocumentDB. Pokud již účet máte, můžete přeskočit na [Krok 2: Vytvoření nové aplikace Node.js](#_Toc395783178).

[AZURE.INCLUDE [documentdb-create-dbaccount](../../includes/documentdb-create-dbaccount.md)]

[AZURE.INCLUDE [documentdb-keys](../../includes/documentdb-keys.md)]

## <a name="_Toc395783178"></a>Krok 2: Postup vytvoření nové aplikace Node.js

Nyní se naučíme, jak vytvořit základní projekt Node.js Hello World pomocí rozhraní [Express](http://expressjs.com/).

1. Otevřete svůj oblíbený terminál.

2. Pomocí generátoru Express vygenerujte novou aplikaci s názvem **todo**.

        express todo

3. Otevřete nový adresář **todo** a nainstalujte závislosti.

        cd todo
        npm install

4. Spusťte novou aplikaci.

        npm start

5. Pokud si chcete zobrazit novou aplikaci, přejděte v prohlížeči na adresu [http://localhost:3000](http://localhost:3000).

    ![Výuka Node.js – snímek obrazovky aplikace Hello World v okně prohlížeče](./media/documentdb-nodejs-application/image12.png)

## <a name="_Toc395783179"></a>Krok 3: Instalace dalších modulů

Soubor **package.json** je jedním ze souborů vytvořených v kořenu projektu. Tento soubor obsahuje seznam dalších modulů, které aplikace Node.js vyžaduje. Později, až budete tuto aplikaci nasazovat na Weby Azure, se tento soubor použije k vyhodnocení, které moduly musí být v Azure pro podporu vaší aplikace nainstalovány. Pro tento kurz je ještě nutné nainstalovat další dva balíčky.

1. Po návratu na terminál nainstalujte pomocí npm modul **async**.

        npm install async --save

1. Přes npm nainstalujte modul **documentdb**. V tomto modulu se dějí všechna kouzla DocumentDB.

        npm install documentdb --save

3. Rychlý pohled do souboru **package.json** aplikace by měl odhalit další moduly. Tento soubor informuje Azure, které balíčky stáhnout a nainstalovat při spuštění aplikace. Měl by vypadat přibližně takto:

    ![Snímek obrazovky karty package.json](./media/documentdb-nodejs-application/image17.png)

       This tells Node (and Azure later) that your application depends on these additional modules.

## <a name="_Toc395783180"></a>Krok 4: Využití služby DocumentDB v aplikaci Node

Tím je hotovo veškeré počáteční nastavování a konfigurace – nyní se budeme zabývat tím, proč tu jsme, tedy psaním kódu s využitím Azure DocumentDB.

### Vytvoření modelu

1. V adresáři projektu vytvořte nový adresář s názvem **models**.
2. V adresáři **models** vytvořte nový soubor s názvem **taskDao.js**. Tento soubor bude obsahovat model pro úkoly vytvořené v aplikaci.
3. Ve stejném adresáři **models** vytvořte jiný soubor s názvem **docdbUtils.js**. V tomto souboru bude obsažen užitečný, opakovaně použitelný kód, který budeme používat v celé aplikaci. 
4. Do **docdbUtils.js** zkopírujte následující kód.

        var DocumentDBClient = require('documentdb').DocumentClient;
            
        var DocDBUtils = {
            getOrCreateDatabase: function (client, databaseId, callback) {
                var querySpec = {
                    query: 'SELECT * FROM root r WHERE r.id= @id',
                    parameters: [{
                        name: '@id',
                        value: databaseId
                    }]
                };
        
                client.queryDatabases(querySpec).toArray(function (err, results) {
                    if (err) {
                        callback(err);
        
                    } else {
                        if (results.length === 0) {
                            var databaseSpec = {
                                id: databaseId
                            };
        
                            client.createDatabase(databaseSpec, function (err, created) {
                                callback(null, created);
                            });
        
                        } else {
                            callback(null, results[0]);
                        }
                    }
                });
            },
        
            getOrCreateCollection: function (client, databaseLink, collectionId, callback) {
                var querySpec = {
                    query: 'SELECT * FROM root r WHERE r.id=@id',
                    parameters: [{
                        name: '@id',
                        value: collectionId
                    }]
                };             
                
                client.queryCollections(databaseLink, querySpec).toArray(function (err, results) {
                    if (err) {
                        callback(err);
        
                    } else {        
                        if (results.length === 0) {
                            var collectionSpec = {
                                id: collectionId
                            };
                            
                            client.createCollection(databaseLink, collectionSpec, function (err, created) {
                                callback(null, created);
                            });
        
                        } else {
                            callback(null, results[0]);
                        }
                    }
                });
            }
        };
                
        module.exports = DocDBUtils;

    > [AZURE.TIP] createCollection přijímá volitelný parametr requestOptions, který je možné použít k určení typu nabídky kolekce. Pokud se nezadá žádná hodnota requestOptions.offerType, kolekce se vytvoří pomocí výchozího typu nabídky.
    >
    > Další informace o typech nabídek DocumentDB najdete v tématu [Úrovně výkonu v DocumentDB](documentdb-performance-levels.md). 
        
3. Uložte a zavřete soubor **docdbUtils.js**.

4. Na začátek souboru **taskDao.js** přidejte následující kód, který odkazuje na **DocumentDBClient** a **docdbUtils.js**, které jsme vytvořili výše:

        var DocumentDBClient = require('documentdb').DocumentClient;
        var docdbUtils = require('./docdbUtils');

4. Dále přidáte kód, který definuje export objektu Task. Ten má na starosti inicializaci objektu Task a nastavení databáze a kolekce dokumentů, které budeme používat.

        function TaskDao(documentDBClient, databaseId, collectionId) {
          this.client = documentDBClient;
          this.databaseId = databaseId;
          this.collectionId = collectionId;
        
          this.database = null;
          this.collection = null;
        }
        
        module.exports = TaskDao;

5. Dále přidejte následující kód, který definuje další metody objektu Task. Ty umožňují pracovat s daty uloženými v DocumentDB.

        TaskDao.prototype = {
            init: function (callback) {
                var self = this;
        
                docdbUtils.getOrCreateDatabase(self.client, self.databaseId, function (err, db) {
                    if (err) {
                        callback(err);
                    } else {
                        self.database = db;
                        docdbUtils.getOrCreateCollection(self.client, self.database._self, self.collectionId, function (err, coll) {
                            if (err) {
                                callback(err);
        
                            } else {
                                self.collection = coll;
                            }
                        });
                    }
                });
            },
        
            find: function (querySpec, callback) {
                var self = this;
        
                self.client.queryDocuments(self.collection._self, querySpec).toArray(function (err, results) {
                    if (err) {
                        callback(err);
        
                    } else {
                        callback(null, results);
                    }
                });
            },
        
            addItem: function (item, callback) {
                var self = this;
        
                item.date = Date.now();
                item.completed = false;
        
                self.client.createDocument(self.collection._self, item, function (err, doc) {
                    if (err) {
                        callback(err);
        
                    } else {
                        callback(null, doc);
                    }
                });
            },
        
            updateItem: function (itemId, callback) {
                var self = this;
        
                self.getItem(itemId, function (err, doc) {
                    if (err) {
                        callback(err);
        
                    } else {
                        doc.completed = true;
        
                        self.client.replaceDocument(doc._self, doc, function (err, replaced) {
                            if (err) {
                                callback(err);
        
                            } else {
                                callback(null, replaced);
                            }
                        });
                    }
                });
            },
        
            getItem: function (itemId, callback) {
                var self = this;
        
                var querySpec = {
                    query: 'SELECT * FROM root r WHERE r.id = @id',
                    parameters: [{
                        name: '@id',
                        value: itemId
                    }]
                };
        
                self.client.queryDocuments(self.collection._self, querySpec).toArray(function (err, results) {
                    if (err) {
                        callback(err);
        
                    } else {
                        callback(null, results[0]);
                    }
                });
            }
        };

6. Uložte a zavřete soubor **taskDao.js**. 

### Vytvoření kontroleru

1. V adresáři **routes** projektu vytvořte nový soubor s názvem **tasklist.js**. 
2. Do souboru **tasklist.js** přidejte následující kód: Tento kód načte moduly DocumentDBClient a async, které budou používány v souboru **tasklist.js**. Také v něm je definována funkce **TaskList**, která přijímá instanci objektu **Task** definovaného dříve:

        var DocumentDBClient = require('documentdb').DocumentClient;
        var async = require('async');
        
        function TaskList(taskDao) {
          this.taskDao = taskDao;
        }
        
        module.exports = TaskList;

3. Dále do souboru **tasklist.js** přidejte metody **showTasks, addTask** a **completeTask**:
        
        TaskList.prototype = {
            showTasks: function (req, res) {
                var self = this;
        
                var querySpec = {
                    query: 'SELECT * FROM root r WHERE r.completed=@completed',
                    parameters: [{
                        name: '@completed',
                        value: false
                    }]
                };
        
                self.taskDao.find(querySpec, function (err, items) {
                    if (err) {
                        throw (err);
                    }
        
                    res.render('index', {
                        title: 'My ToDo List ',
                        tasks: items
                    });
                });
            },
        
            addTask: function (req, res) {
                var self = this;
                var item = req.body;
        
                self.taskDao.addItem(item, function (err) {
                    if (err) {
                        throw (err);
                    }
        
                    res.redirect('/');
                });
            },
        
            completeTask: function (req, res) {
                var self = this;
                var completedTasks = Object.keys(req.body);
        
                async.forEach(completedTasks, function taskIterator(completedTask, callback) {
                    self.taskDao.updateItem(completedTask, function (err) {
                        if (err) {
                            callback(err);
                        } else {
                            callback(null);
                        }
                    });
                }, function goHome(err) {
                    if (err) {
                        throw err;
                    } else {
                        res.redirect('/');
                    }
                });
            }
        };


4. Uložte a zavřete soubor **tasklist.js**.
 
### Přidání souboru config.js

1. V adresáři projektu vytvořte nový soubor s názvem **config.js**.
2. Do souboru **config.js** přidejte následující: Tento kód definuje nastavení konfigurace a hodnoty, které aplikace potřebuje.

        var config = {}
        
        config.host = process.env.HOST || "[the URI value from the DocumentDB Keys blade on http://portal.azure.com]";
        config.authKey = process.env.AUTH_KEY || "[the PRIMARY KEY value from the DocumentDB Keys blade on http://portal.azure.com]";
        config.databaseId = "ToDoList";
        config.collectionId = "Items";
        
        module.exports = config;

3. Aktualizujte v souboru **config.js** hodnoty HOST a AUTH_KEY hodnotami, které získáte v okně Klíče účtu DocumentDB na [Portálu Microsoft Azure](https://portal.azure.com):

4. Uložte a zavřete soubor **config.js**.
 
### Úprava souboru app.js

1. V adresáři projektu otevřete soubor **app.js**. Tento soubor byl vytvořen již dříve, při vytváření webové aplikace Express.
2. Do horní části souboru **app.js** přidejte následující kód.
    
        var DocumentDBClient = require('documentdb').DocumentClient;
        var config = require('./config');
        var TaskList = require('./routes/tasklist');
        var TaskDao = require('./models/taskDao');

3. Tento kód definuje konfigurační soubor, který bude použit, a načte z něj do proměnných hodnoty, které brzy využijeme.
4. Nahraďte v souboru **app.js** následující dva řádky:

        app.use('/', routes);
        app.use('/users', users); 

      následujícím fragmentem kódu:

        var docDbClient = new DocumentDBClient(config.host, {
            masterKey: config.authKey
        });
        var taskDao = new TaskDao(docDbClient, config.databaseId, config.collectionId);
        var taskList = new TaskList(taskDao);
        taskDao.init();
        
        app.get('/', taskList.showTasks.bind(taskList));
        app.post('/addtask', taskList.addTask.bind(taskList));
        app.post('/completetask', taskList.completeTask.bind(taskList));
        app.set('view engine', 'jade');



6. Tyto řádky definují novou instanci objektu **TaskDao** s novým připojením k DocumentDB (pomocí hodnot přečtených ze souboru **config.js**), inicializují objekt úkolů a svážou akce formuláře s metodami kontroleru **TaskList**. 

7. Nakonec soubor **app.js** uložte a zavřete a jsme téměř hotovi.
 
## <a name="_Toc395783181"></a>Krok 5: Vytvoření uživatelského rozhraní

Nyní se zaměřme na vytvoření uživatelského rozhraní, aby uživatelé mohli s aplikací pracovat. Aplikace Express, kterou jsme vytvořili, používá jako zobrazovací modul **Jade**. Další informace o Jade najdete na stránce [http://jade-lang.com/](http://jade-lang.com/).

1. Soubor **layout.jade** v adresáři **views** slouží jako globální šablona pro ostatní soubory **.jade**. V tomto kroku jej upravíte tak, aby používal [Twitter Bootstrap](https://github.com/twbs/bootstrap). To je sada nástrojů, které usnadňují návrh pěkných webů. 
2. Otevřete soubor **layout.jade** umístěný v adresáři **views** a nahraďte jeho obsah následujícím kódem:
    
        doctype html
        html
          head
            title= title
            link(rel='stylesheet', href='//ajax.aspnetcdn.com/ajax/bootstrap/3.3.2/css/bootstrap.min.css')
            link(rel='stylesheet', href='/stylesheets/style.css')
          body
            nav.navbar.navbar-inverse.navbar-fixed-top
              div.navbar-header
                a.navbar-brand(href='#') My Tasks
            block content
            script(src='//ajax.aspnetcdn.com/ajax/jQuery/jquery-1.11.2.min.js')
            script(src='//ajax.aspnetcdn.com/ajax/bootstrap/3.3.2/bootstrap.min.js')



    Tímto modul **Jade** dostává instrukci vykreslovat pro naši aplikaci některý kód HTML a vytvoří **block** s názvem **content**, do kterého můžeme zadat rozložení stránek s obsahem.
    Uložte a zavřete tento soubor **layout.jade**.

4. Nyní otevřete soubor **index.jade**, který představuje zobrazení používané v naší aplikaci a nahraďte jeho obsah následujícím kódem:

        extends layout
        
        block content
          h1 #{title}
          br
        
          form(action="/completetask", method="post")
            table.table.table-striped.table-bordered
              tr
                td Name
                td Category
                td Date
                td Complete
              if (typeof tasks === "undefined")
                tr
                  td
              else
                each task in tasks
                  tr
                    td #{task.name}
                    td #{task.category}
                    - var date  = new Date(task.date);
                    - var day   = date.getDate();
                    - var month = date.getMonth() + 1;
                    - var year  = date.getFullYear();
                    td #{month + "/" + day + "/" + year}
                    td
                      input(type="checkbox", name="#{task.id}", value="#{!task.completed}", checked=task.completed)
            button.btn(type="submit") Update tasks
          hr
          form.well(action="/addtask", method="post")
            label Item Name:
            input(name="name", type="textbox")
            label Item Category:
            input(name="category", type="textbox")
            br
            button.btn(type="submit") Add item

    Tento kód rozšiřuje rozložení a poskytuje obsah pro zástupný symbol **content**, který jsme viděli v souboru **layout.jade** výše.
    
    V tomto rozložení jsme vytvořili dva formuláře HTML. 
    První formulář obsahuje tabulku našich dat a tlačítko, které umožňuje aktualizovat položky odesláním informací metodě kontroleru **/completetask**.
    Druhý formulář obsahuje dvě vstupní pole a tlačítko, které umožňuje vytvořit novou položku odesláním informací metodě kontroleru **/addtask**.
    
    To by mělo být vše, co potřebujeme ke zprovoznění aplikace.

5. Otevřete soubor **style.css** v adresáři **public\stylesheets** a nahraďte kód tímto:

        body {
          padding: 50px;
          font: 14px "Lucida Grande", Helvetica, Arial, sans-serif;
        }
        a {
          color: #00B7FF;
        }
        .well label {
          display: block;
        }
        .well input {
          margin-bottom: 5px;
        }
        .btn {
          margin-top: 5px;
          border: outset 1px #C8C8C8;
        }

    Uložte a zavřete tento soubor **style.css**.

## <a name="_Toc395783181"></a>Krok 6: Místní spuštění aplikace

1. Pokud chcete aplikaci otestovat na místním počítači, spusťte v terminálu `npm start`, aby se spustila aplikace a prohlížeč se stránkou, která vypadá obdobně jako obrázek níže:

    ![Snímek obrazovky aplikace Seznam úkolů v okně prohlížeče](./media/documentdb-nodejs-application/image18.png)


2. Do polí Item (Položka), Item Name (Název položky) a Category (Kategorie) zadejte informace a klikněte na **Add Item** (Přidat položku).

3. Stránka by se měla aktualizovat, aby se v seznamu úkolů zobrazila nově vytvořená položka.

    ![Snímek obrazovky aplikace s novou položkou v seznamu úkolů](./media/documentdb-nodejs-application/image19.png)

4. Pokud chcete dokončit úkol, stačí zaškrtnout políčko ve sloupci Complete (Dokončeno) a kliknout na **Update tasks** (Aktualizovat úkoly).

## <a name="_Toc395783182"></a>Krok 7: Nasazení vývojového projektu aplikace na Weby Azure

1. Pokud jste tak ještě neučinili, povolte úložiště Git pro Weby Azure. Pokyny, jak máte postupovat, najdete v tématu [Místní nasazení přes Git do Azure App Service](../app-service-web/app-service-deploy-local-git.md).

2. Přidejte svůj web Azure jako vzdálené úložiště Git.

        git remote add azure https://username@your-azure-website.scm.azurewebsites.net:443/your-azure-website.git

3. Nasazení provedete odesláním na vzdálené úložiště.

        git push azure master

4. Za několik sekund Git dokončí publikování webové aplikace a spustí prohlížeč, kde se můžete podívat, jak vaše práce běží v Azure!

## <a name="_Toc395637775"></a>Další kroky

Blahopřejeme! Právě jste vytvořili svou první webovou aplikaci Node.js Express, která používá Azure DocumentDB, a publikovali jste ji na Weby Azure.

Zdrojový kód úplné referenční aplikace je možné stáhnout z [GitHubu][].

Další informace najdete ve [Středisku pro vývojáře Node.js](https://azure.microsoft.com/develop/nodejs/).

[Node.js]: http://nodejs.org/
[Git]: http://git-scm.com/
[GitHub]: https://github.com/Azure-Samples/documentdb-node-todo-app
 



<!---HONumber=Aug16_HO4-->


