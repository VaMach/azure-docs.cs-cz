<properties
    pageTitle="Začínáme s Azure Mobile Services a Sencha"
    description="Chcete-li začít s vývojem pomocí Mobile Services a rámcem mobilních aplikací Sencha HTML5, využijte tento kurz."
    services="mobile-services"
    documentationCenter=""
    authors="ggailey777"
    manager="dwrede"
    editor=""/>

<tags
    ms.service="mobile-services"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-sencha"
    ms.devlang="multiple"
    ms.topic="get-started-article"
    ms.date="07/21/2016"
    ms.author="glenga"/>


# <a name="getting-started"> </a>Začínáme s Mobile Services a Sencha Touch

[AZURE.INCLUDE [mobile-service-note-mobile-apps](../../includes/mobile-services-note-mobile-apps.md)]

&nbsp;


[AZURE.INCLUDE [mobile-services-selector-get-started](../../includes/mobile-services-selector-get-started.md)]
&nbsp;

[AZURE.INCLUDE [mobile-services-hero-slug](../../includes/mobile-services-hero-slug.md)]

##Přehled

Tento kurz ukazuje, jak můžete využít Azure Mobile Services v aplikaci Sencha Touch. Pomocí Sencha Touch vytvoříte jednoduchou aplikaci *Seznam úkolů*, která využívá mobilní služby definované prostřednictvím portálu Azure Classic. Tento kurz je určen pro středně pokročilé až pokročilé vývojáře webových aplikací, kteří mají dobrou znalost jazyka JavaScript a jsou obeznámeni s frameworkem Sencha Touch.

Pokud upřednostňujete video, tento klip postupuje stejně jako tento kurz. Na videu Arthur Kay vysvětluje, jak sestavit aplikaci Sencha Touch pomocí back-end Azure Mobile Services.

> [AZURE.VIDEO getting-started-with-sencha-touch]


Snímek obrazovky dokončené aplikace naleznete níže:

![][0]

##Požadavky

- Stáhněte a nainstalujte [Sencha Touch](http://wwww.sencha.com/products/touch/download" target="_blank").

- Stáhněte a nainstalujte [Sencha Cmd Tool](http://www.sencha.com/products/sencha-cmd/download" target="_blank").

- Běhové prostředí Java Runtime (JRE) nebo Java Development Kit (pokud vytváříte aplikace pro Android)
- Ruby a SASS gem.

## <a name="create-new-service"> </a>Vytvoření nové mobilní služby

[AZURE.INCLUDE [mobile-services-create-new-service](../../includes/mobile-services-create-new-service.md)]

##Vytvoření tabulky Seznam úkolů

Jakmile vytvoříte mobilní službu, můžete snadno využít postup rychlého spuštění portálu Azure Classic k vytvoření nové databázové tabulky pro použití v mobilní službě.

1. Na [portál Azure Classic] klikněte na tlačítko **Mobile Services** a pak klikněte na mobilní službu, kterou jste právě vytvořili.

2. Na kartě Rychlé spuštění klikněte na tlačítko **HTML** pod položkou **Vybrat platformu** a rozbalte možnost **Vytvořit novou aplikaci HTML**.

    ![Mobilní rychlý start html](./media/partner-sencha-mobile-services-get-started/mobile-portal-quickstart-html.png)

    Zobrazí se tří jednoduché kroky k vytvoření a hostování aplikace HTML připojené k mobilní službě.

    ![Mobilní rychlý start html](./media/partner-sencha-mobile-services-get-started/mobile-quickstart-steps-html.png)

3. Klikněte na tlačítko **Vytvořit tabulku TodoItem** a vytvořte tabulku pro ukládání dat aplikací.

    > [AZURE.NOTE] Nestahujte aplikaci HTML z portálu Azure Classic. Místo toho ručně vytvoříme aplikaci Sencha Touch v následující části.


1. Povšimněte si položek **appKey** a **appUrl** na portálu Azure Classic. Použijeme je v dalších částech tohoto kurzu.

    ![klíč aplikace](./media/partner-sencha-mobile-services-get-started/mobile-app-key-portal.png)

1. Na kartě **Konfigurovat** ověřte, zda je `localhost` již uveden v seznamu **Povolit požadavky od názvů hostitelů** v rámci **Sdílení prostředků různých původů (CORS)**. Pokud není, zadejte `localhost` do pole **Název hostitele** a pak klikněte na tlačítko **Uložit**.

    ![Instalační program CORS pro místního hostitele](./media/partner-sencha-mobile-services-get-started/mobile-services-set-cors-localhost.png)

##Vygenerujte svou aplikaci Touch

Generování aplikace šablony Sencha Touch je jednoduchou úlohou využívající Sencha Cmd a představuje skvělý způsob, jak aplikaci velmi rychle zprovoznit.

Z adresáře, kam jste nainstalovali framework Touch vydejte následující příkaz:

    $ sencha generate app Basic /path/to/application

Tento postup vytvoří šablonu Touch s názvem aplikace „Basic“. Ke spuštění aplikace jednoduše přejděte v prohlížeči do adresáře /path/to/application, kde by se měla zobrazit standardní vzorová aplikace Touch.

##Instalace rozšíření Sencha Touch pro Azure.

Rozšíření pro Azure lze nainstalovat ručně nebo jako balíček Sencha. Použitý způsob je zcela na vás.

###Ruční instalace

Ve většině aplikací Touch musíte v případě požadavku na přidání externí knihovny tříd jednoduše stáhnout balíček, rozbalit ho v adresáři vaší aplikace a nakonfigurovat zavaděč Touch podle umístění knihovny.

Rozšíření Azure můžete do aplikace přidat ručně pomocí následujících kroků:

1. Stáhněte balíček rozšíření Azure [odtud](https://market.sencha.com/extensions/sencha-extensions-for-microsoft-azure). (Pro přístup do této oblasti můžete využít ID na fóru Sencha.)

2. Zkopírujte balíček rozšíření Azure z adresáře ke stažení do místa, kde se bude nakonec nacházet a rozbalte ho:

        $ cd /path/to/application
        $ mv /download-location/azure.zip .
        $ unzip azure.zip

    Tím se vytvoří adresář **azure** obsahující celý zdrojový balíček, příklady a dokumentaci. Zdroj se bude nacházet v adresáři **azure/src**.


###Instalace jako balíček Sencha

> [AZURE.NOTE] Tuto metodu můžete také použít v případě vygenerování aplikace pomocí příkazu <code>sencha generate app</code>.

Všechny aplikace generované Sencha Cmd mají složku „packages“ v kořenovém adresáři. Umístění této složky lze konfigurovat, ale bez ohledu na její umístění slouží role složky „packages“ jako úložiště pro všechny balíčky používané aplikací (nebo aplikacemi, pokud jste vytvořili pracovní prostor Sencha).

Jelikož je Ext.Azure „balíček“ Sencha Cmd, zdrojový kód lze snadno nainstalovat a zahrnout do vaší aplikace pomocí Sencha Cmd. (Další informace naleznete v části [Balíčky Sencha Cmd](http://docs.sencha.com/cmd/6.x/cmd_packages/cmd_packages.html)).

Chcete-li stáhnout a nainstalovat balíčky rozšíření Azure z úložiště balíčků Sencha, budete muset přidat název balíčku pro váš soubor **app.json** a sestavit aplikaci:

1. Přidejte balíček Azure do požadované části souboru app.json:

        {
            "name": "Basic",
            "requires": [
                "touch-azure"
            ]
        }

2. Znovu sestavte aplikaci pomocí **sencha cmd** pro načtení a instalaci balíčku:

        $ sencha app build

Jak **sestavení aplikace sencha**, tak i **obnovení aplikace sencha** teď provede kroky potřebné k integraci balíčku do vaší aplikace. Obvykle po změně požadavků balíčku budete muset spustit **obnovení aplikace sencha**, aby byla metadata potřebná k podpoře „režimu vývojáře“ aktuální.

Podle toho, který příkaz spustíte, se Sencha Cmd stáhne a rozbalí balíček do vaší složky „packages“. Následně naleznete v pracovním prostoru složku „packages/touch-azure“.

##Zahrnují a konfigurace služby Azure

**Název souboru**: app.js

Po stažení a instalaci rozšíření Azure do adresáře aplikace je dalším krokem oznámit aplikaci, kde nalezne zdrojové soubory a jak vyžadovat tyto soubory:

1. Konfigurace zavaděče Sencha pomocí umístění zdrojového kódu:

        Ext.Loader.setConfig({
            enabled : true,
            paths   : {
                'Ext'       : 'touch/src',
                'Ext.azure' : '/path-to/azure-for-touch/azure/src'
            }
        });


2. Vyžadování soubory třídy Azure:

        Ext.application({

            requires: [ 'Ext.azure.Azure' ],

            // ...

        });


3. Konfigurace Azure

    Balíček Azure se inicializuje pomocí volání metody **Ext.Azure.init** v oddílu spuštění vaší aplikace. Této metodě se předává objekt konfigurace obsahující pověření mobilní služby, stejně jako další pověření a funkce, které chcete využívat.

    Zatímco můžete předat objekt konfigurace přímo do počáteční metody, doporučujeme vytvoření vlastnosti konfigurace aplikace Sencha nazývané **azure** a umístění příslušných informací. Tuto hodnotu vlastnosti můžete poté předat metodě Ext.Azure.init.

    Když vytvoříte mobilní službu v Azure (viz [Začínáme s Azure](http://senchaazuredocs.azurewebsites.net/#!/guide/getting_started)), přiřadí se do dané služby klíč aplikace a adresa URL. Tyto informace musí být k dispozici balíčku Azure, aby se mohl připojit ke službě.

    Tento příklad ukazuje velmi jednoduchou konfiguraci a inicializaci Azure zadáním pouze klíče aplikace a adresy URL:

        Ext.application({
            name: 'Basic',

            requires: [ 'Ext.azure.Azure' ],

            azure: {
                appKey: 'myazureservice-access-key',
                appUrl: 'myazure-service.azure-mobile.net'
            },

            launch: function() {

                // Call Azure initialization

                Ext.Azure.init(this.config.azure);

           }
        });

    Další informace o možnostech konfigurace Azure najdete v dokumentaci k rozhraní Ext.Azure API.


Blahopřejeme! Vaše aplikace by teď měla mít přístup k mobilní službě.

##Vytvoření aplikace ToDo

Teď, když jsme aplikaci nakonfigurovali pro zahrnutí rozšíření Azure a uvedli své přihlašovací údaje mobilních služeb, se můžeme přesunout k vytvoření aplikace Touch, který využívá vaši mobilní službu pro prohlížení a úpravy dat seznamu ToDo uložených ve službě.

###Konfigurace datové proxy Azure

**Název souboru:** app/model/TodoItem.js

Vaše aplikace Touch bude komunikovat mobilní službou přes datovou proxy. Proxy vykonává všechnu práci týkající se odesílání žádostí a příjmu dat z mobilní služby a do ní. V kombinaci s datovým modelem Touch a úložištěm dojde k odstranění náročné práce zpracovávání vzdálených data a načítání do aplikace a je řešeno samotným modelem Touch.

Modely Sencha Touch poskytují definice datových záznamů, které budete používat v aplikaci. Umožňují vám nejen definovat datová pole, ale také zadat konfiguraci proxy, která bude zpracovávat komunikaci mezi aplikací a mobilními službami Azure.

V kódu níže můžete vidět, že definujeme pole (a jejich typy) pro model a také zadáváme konfiguraci proxy. Při konfigurace proxy musíte přiřadit typ (v tomto případě „azure“), název tabulky mobilní služby (ToDoItem) a další volitelné parametry. V tomto příkladu zapneme proxy stránkování tak, abychom mohli hladce procházet položkami seznamu na stránkách vpřed a zpět.

Azure proxy automaticky nastaví všechny hlavičky protokolu HTTP s příslušnými operacemi CRUD očekávané rozhraním API Azure (včetně pověření pro ověření, pokud existují).

    Ext.define('Basic.model.TodoItem', {
        extend : 'Ext.data.Model',

        requires : [
            'Ext.azure.Proxy'
        ],

        config : {
            idProperty : 'id',
            useCache   : false,

            fields     : [
                {
                    name : 'id',
                    type : 'int'
                },
                {
                    name : 'text',
                    type : 'string'
                },
                {
                    name : 'complete',
                    type : 'boolean'
                }
            ],

            proxy : {
                type               : 'azure',
                tableName          : 'TodoItem',
                enablePagingParams : true
            }
        }
    });


###Uložení položek ToDo

**Název souboru**: app/store/TodoItems.js

Úložiště Sencha Touch se používají k ukládání kolekcí datových záznamů (modelů), které lze použít jako zdroje pro součásti Touch pro zobrazení záznamů mnoha různými způsoby. To může zahrnovat mřížky, grafy, seznamy a další.

Zde jsme definovali úložiště, které bude použito pro uložení všech položek seznamu ToDo, které jsou načteny z mobilní služby Azure. Všimněte si, že konfigurace úložiště obsahuje název typu modelu (Basic.model.TodoItem – definovaný výše). Ten definuje strukturu záznamů, které budou v úložišti obsaženy.

Máme také některé další možnosti konfigurace úložiště, například určení velikosti stránky (8 záznamů) a vzdálené provedení řazení záznamů pro toto úložiště pomocí mobilní služby Azure (žádné řazení se neprovádí místně v rámci samotného úložiště).

    Ext.define('Basic.store.TodoItems', {
        extend : 'Ext.data.Store',

        requires : [
            'Basic.model.TodoItem'
        ],

        config : {
            model        : 'Basic.model.TodoItem',
            pageSize     : 8,
            remoteSort   : true,
            remoteFilter : true
        }
    });


###Zobrazení a úpravy položek ToDo

**Název souboru**: app/view/DataItem.js

Teď, když jsme definovali strukturu každé položky ToDo a vytvořili úložiště pro umístění všech záznamů, bychom se měli zamyslet nad tím, jak chceme zobrazit tyto informace pro uživatele aplikace. Za normálních okolností zobrazujeme uživatelům informace prostřednictvím **Zobrazení**. Zobrazení může představovat libovolný počet součásti Touch, samostatné nebo v kombinaci s dalšími.

Zobrazení níže se skládá z položky ListItem, která definuje, jak se každý záznam zobrazí společně s některými tlačítky, kterým budou přiřazeny akce pro odstranění jednotlivých položek.

    Ext.define('Basic.view.DataItem', {
        extend : 'Ext.dataview.component.ListItem',
        xtype  : 'basic-dataitem',

        requires : [
            'Ext.Button',
            'Ext.layout.HBox',
            'Ext.field.Checkbox'
        ],

        config : {
            checkbox : {
                docked     : 'left',
                xtype      : 'checkboxfield',
                width      : 50,
                labelWidth : 0
            },

            text : {
                flex : 1
            },

            button : {
                docked   : 'right',
                xtype    : 'button',
                ui       : 'plain',
                iconMask : true,
                iconCls  : 'delete',
                style    : 'color: red;'
            },

            dataMap : {
                getText : {
                    setHtml : 'text'
                },

                getCheckbox : {
                    setChecked : 'complete'
                }
            },

            layout : {
                type : 'hbox',
                align: 'stretch'
            }
        },

        applyCheckbox : function(config) {
            return Ext.factory(config, Ext.field.Checkbox, this.getCheckbox());
        },

        updateCheckbox : function (cmp) {
            if (cmp) {
                this.add(cmp);
            }
        },

        applyButton : function(config) {
            return Ext.factory(config, Ext.Button, this.getButton());
        },

        updateButton : function (cmp) {
            if (cmp) {
                this.add(cmp);
            }
        }

    });


###Vytvoření primárního zobrazení

**Název souboru**: app/view/Main.js

Teď, když jsme definovali rozložení jednotlivých položek seznamu ToDo (výše) chceme obtéct úplné uživatelské rozhraní kolem tohoto seznamu, který definuje skutečný seznam položek, název aplikace a tlačítko pro přidání nové úlohy.

    Ext.define('Basic.view.Main', {
        extend : 'Ext.dataview.List',
        xtype  : 'main',

        requires : [
            'Ext.TitleBar',
            'Ext.dataview.List',
            'Ext.data.Store',
            'Ext.plugin.PullRefresh',
            'Ext.plugin.ListPaging',
            'Basic.view.DataItem'
        ],

        config : {
            store : 'TodoItems',

            useSimpleItems : false,
            defaultType    : 'basic-dataitem',

            plugins : [
                {
                    xclass          : 'Ext.plugin.PullRefresh',
                    pullRefreshText : 'Pull down to refresh!'
                },
                {
                    xclass     : 'Ext.plugin.ListPaging',
                    autoPaging : true
                }
            ],

            scrollable : {
                direction     : 'vertical',
                directionLock : true
            },

            items : [
                {
                    docked : 'top',
                    xtype  : 'titlebar',
                    title  : 'Azure Mobile - Basic Data Example'
                },
                {
                    xtype  : 'toolbar',
                    docked : 'bottom',
                    items  : [
                        {
                            xtype       : 'textfield',
                            placeHolder : 'Enter new task',
                            flex        : 1
                        },
                        {
                            xtype  : 'button',
                            action : 'add',
                            text   : 'Add'
                        }
                    ]
                }
            ]
        }
    });

###Zajištění spolupráce všech součástí

**Název souboru**: app/controller/Main.js

Posledním krokem v naší aplikaci jsou reakce na stisknutí tlačítek (odstranit, uložit atd.), které poskytují logiku za všemi těmito požadavky. Sencha Touch využívá řadiče, které naslouchají těmto událostem a reagují odpovídajícím způsobem.

    Ext.define('Basic.controller.Main', {
        extend : 'Ext.app.Controller',

        config : {
            refs : {
                todoField : 'main toolbar textfield',
                main      : 'main'
            },

            control : {
                'button[action=add]'    : {
                    tap : 'onAddItem'
                },
                'button[action=reload]' : {
                    tap : 'onReload'
                },

                main : {
                    activate      : 'loadInitialData',
                    itemdoubletap : 'onItemEdit'
                },

                'basic-dataitem checkboxfield' : {
                    change : 'onItemCompleteTap'
                },

                'basic-dataitem button' : {
                    tap : 'onItemDeleteTap'
                }
            }
        },

        loadInitialData : function () {
            Ext.getStore('TodoItems').load();
        },

        onItemDeleteTap : function (button, e, eOpts) {
            var store    = Ext.getStore('TodoItems'),
                dataItem = button.up('dataitem'),
                rec      = dataItem.getRecord();

            rec.erase({
                success: function (rec, operation) {
                    store.remove(rec);
                },
                failure: function (rec, operation) {
                    Ext.Msg.alert(
                        'Error',
                        Ext.util.Format.format('There was an error deleting this task.<br/><br/>    Status Code: {0}<br/>Status Text: {1}',
                        operation.error.status,
                        operation.error.statusText)
                    );
                }
            });
        },

        onItemCompleteTap : function (checkbox, newVal, oldVal, eOpts) {
            var dataItem = checkbox.up('dataitem'),
                rec      = dataItem.getRecord(),
                recVal   = rec.get('complete');

            // this check is needed to prevent an issue where multiple creates get triggered from one create
            if (newVal !== recVal) {
                rec.set('complete', newVal);
                rec.save({
                    success: function (rec, operation) {
                        rec.commit();
                    },
                    failure: function (rec, operation) {
                        // since there was a failure doing the update on the server then silently reject the change
                        rec.reject(true);
                        Ext.Msg.alert(
                            'Error',
                            Ext.util.Format.format('There was an error updating this task.<br/><br/>Status Code: {0}<br/>Status Text: {1}',
                            operation.error.status,
                            operation.error.statusText)
                        );
                    }
                });
            }
        },

        onItemEdit : function (list, index, target, record, e, eOpts) {
            var rec = list.getSelection()[0];

            Ext.Msg.prompt('Edit', 'Rename task',
                function (buttonId, value) {
                    if (buttonId === 'ok') {
                        rec.set('text', value);
                        rec.save({
                            success: function (rec, operation) {
                                rec.commit();
                            },
                            failure: function (rec, operation) {
                                // since there was a failure doing the update on the server then reject the change
                                rec.reject();
                                Ext.Msg.alert(
                                    'Error',
                                    Ext.util.Format.format('There was an error updating this task.<br/><br/>Status Code: {0}<br/>Status Text: {1}',
                                    operation.error.status,
                                    operation.error.statusText)
                                );
                            }
                        });
                    }
                },
                null,
                false,
                record.get('text')
            );
        },

        onReload : function () {
            Ext.getStore('TodoItems').load();
        },

        onAddItem : function () {
            var me = this,
                rec,
                store = Ext.getStore('TodoItems'),
                field = me.getTodoField(),
                value = field.getValue();

            if (value === '') {
                Ext.Msg.alert('Error', 'Please enter Task name', Ext.emptyFn);
            }
            else {
                rec = Ext.create('Basic.model.TodoItem', {
                    complete : false,
                    text     : value
                });
                //store.insert(0, rec); //insert at the top
                //store.sync();
                rec.save({
                    success: function (rec, operation) {
                        store.insert(0, rec); //insert at the top
                        field.setValue('');
                    },
                    failure: function (rec, operation) {
                        Ext.Msg.alert(
                            'Error',
                            Ext.util.Format.format('There was an error creating this task.<br/><br/>Status Code: {0}<br/>Status Text: {1}',
                            operation.error.status,
                            operation.error.statusText)
                        );
                    }
                });
            }
        }
    });

###Spojení všech součástí dohromady

**Název souboru**: app.js

Posledním krokem je dokončení úprav souboru hlavní aplikace a zadání informací o modelech, úložištích, zobrazeních a řadičích, které jste definovali. Zdrojové soubory pro tyto prostředky se do aplikace načtou automaticky. Nakonec je volána metoda spuštění, která vytvoří a zobrazí primární zobrazení aplikace „Basic.main.View“.


    Ext.Loader.setConfig({
        enabled : true,
        paths   : {
            'Ext'       : 'touch/src',
            'Ext.azure' : 'packages/azure/src'
        }
    });

    Ext.application({
        name : 'Basic',

        requires : [
            'Ext.MessageBox',
            'Ext.azure.Azure'
        ],

        views : [
            'Main'
        ],

        controllers : [
            'Main'
        ],

        stores : [
            'TodoItems'
        ],

        azure : {
            appUrl : 'YOUR_APP_URL.azure-mobile.net',
            appKey : 'YOUR_APP_KEY'
        },

        icon : {
            '57'  : 'resources/icons/Icon.png',
            '72'  : 'resources/icons/Icon~ipad.png',
            '114' : 'resources/icons/Icon@2x.png',
            '144' : 'resources/icons/Icon~ipad@2x.png'
        },

        isIconPrecomposed : true,

        startupImage : {
            '320x460'   : 'resources/startup/320x460.jpg',
            '640x920'   : 'resources/startup/640x920.png',
            '768x1004'  : 'resources/startup/768x1004.png',
            '748x1024'  : 'resources/startup/748x1024.png',
            '1536x2008' : 'resources/startup/1536x2008.png',
            '1496x2048' : 'resources/startup/1496x2048.png'
        },

        launch : function () {
            // Destroy the #appLoadingIndicator element
            Ext.fly('appLoadingIndicator').destroy();

            // Initialize Azure
            Ext.Azure.init(this.config.azure);

            // Initialize the main view
            Ext.Viewport.add(Ext.create('Basic.view.Main'));
        },

        onUpdated : function () {
            Ext.Msg.confirm(
                "Application Update",
                "This application has just successfully been updated to the latest version. Reload now?",
                function (buttonId) {
                    if (buttonId === 'yes') {
                        window.location.reload();
                    }
                }
            );
        }
    });

###Hostování a spuštění aplikace Sencha Touch

Závěrečnou fází tohoto kurzu je hostování a spuštění nové aplikace v místním počítači.

  1. V terminálu přejděte do umístění rozbalené aplikace.

  2. Pomocí Sencha Cmd spusťte následující příkazy:

    * *obnovení aplikace sencha* : tato akce řekne Sencha Cmd, že má vyhledat všechny závislosti aplikace a stáhnout všechny potřebné balíčky (například [rozšíření Sencha Touch pro Azure](https://market.sencha.com/extensions/sencha-extensions-for-microsoft-azure)).

    * *spuštění webu sencha* : tato akce spustí místní webový server k otestování naší aplikace.

    ![spuštění webu sencha](./media/partner-sencha-mobile-services-get-started/sencha-web-start.png)

  3. Otevřete adresu URL uvedenou v terminálu ve webovém prohlížeči a spusťte aplikaci (např. http://localhost:1841).

  4. V aplikaci zadejte smysluplný text, například „Dokončit kurz“ a klikněte na tlačítko **Přidat**.

    ![nová položka todo](./media/partner-sencha-mobile-services-get-started/new-todo-item.png)

    Tím se odešle požadavek POST do nové mobilní služby hostované v Azure. Data z požadavku se vloží do tabulky TodoItem.

  5. Zpět v [portál Azure Classic] klikněte na kartu **Data** a pak klikněte na tabulku TodoItems.

    ![Tabulka položek todo](./media/partner-sencha-mobile-services-get-started/mobile-data-tab.png)

    Tato možnost vám umožňuje procházet data vložená aplikací do tabulky.

    ![procházet tabulku todo](./media/partner-sencha-mobile-services-get-started/mobile-data-browse.png)

##Další kroky
Teď, když jste dokončili průvodce Začínáme, se naučíte, jak provádět další důležité úkoly v Mobile Services pomocí Sencha.

[Stáhněte](https://github.com/arthurakay/sencha-touch-azure-example) úplnou vzorovou aplikaci s dalšími styly a funkcemi a zjistěte, co Sencha Touch dokáže!

Poté se ponořte do dalších informací o rozšíření Sencha Touch pro Azure:

  * [Prohlídka](http://docs.sencha.com/touch-azure/1.0.0/#!/guide/data_filters) vzorové aplikace
  * Získání nápovědy na [fórech Sencha](http://www.sencha.com/forum)
  * Procházet [dokumentaci Sencha](http://docs.sencha.com/)
  * Používání Sencha s Azure Mobile Services: [(Video)](http://channel9.msdn.com/Shows/Cloud+Cover/Episode-126-Using-Sencha-With-Windows-Azure-Mobile-Services)


##Další zdroje

  * [Stáhnout Sencha Touch](http://pages.sencha.com/touch-for-azure.html)
  * [Rozšíření Sencha Touch pro Azure](https://market.sencha.com/extensions/sencha-extensions-for-microsoft-azure)


##Souhrn

Zde uvedené příklady se nachází v rozšíření Sencha Touch pro balíček Azure a jsou umístěny ve vzorovém adresáři jako příklad Základní data. Existuje několik dalších dostupných příkladů, které vám mají ukázat další funkce toto rozšíření spolu s podrobnými poznámkami a vysvětlivkami.

Další informace o začátcích se Sencha Touch naleznete v úplné sadě [průvodců](http://docs.sencha.com/touch/#!/guide)


[AZURE.INCLUDE [app-service-disqus-feedback-slug](../../includes/app-service-disqus-feedback-slug.md)]

<!-- images -->
[0]: ./media/partner-sencha-mobile-services-get-started/finished-app.png

[portál Azure Classic]: https://manage.windowsazure.com/


<!--HONumber=Sep16_HO3-->


