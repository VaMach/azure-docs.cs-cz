---
title: Jak pracovat se serverem back-end Node.js SDK pro Mobile Apps | Microsoft Docs
description: "Naučte se pracovat se serverem back-end Node.js SDK pro Azure App Service Mobile Apps."
services: app-service\mobile
documentationcenter: 
author: elamalani
manager: elamalani
editor: 
ms.assetid: e7d97d3b-356e-4fb3-ba88-38ecbda5ea50
ms.service: app-service-mobile
ms.workload: mobile
ms.tgt_pltfrm: mobile-multiple
ms.devlang: node
ms.topic: article
ms.date: 10/01/2016
ms.author: crdun
ms.openlocfilehash: 336da28bea7de313bced97e447fc6b7b1fb1390d
ms.sourcegitcommit: df4ddc55b42b593f165d56531f591fdb1e689686
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/04/2018
---
# <a name="how-to-use-the-azure-mobile-apps-nodejs-sdk"></a>Jak používat Azure Mobile Apps Node.js SDK
[!INCLUDE [app-service-mobile-selector-server-sdk](../../includes/app-service-mobile-selector-server-sdk.md)]

Tento článek obsahuje podrobné informace a příklady znázorňující, jak pracovat s back-end Node.js v Azure App Service Mobile Apps.

## <a name="Introduction"></a>Úvod
Azure App Service Mobile Apps poskytuje možnost Přidat mobilní optimalizovaná data přístup webového rozhraní API k webové aplikaci.  Azure App Service Mobile Apps SDK se poskytuje pro webové aplikace ASP.NET a Node.js.  Sada SDK poskytuje následující operace:

* Operace s tabulkou (pro čtení, příkaz Insert, Update, Delete) pro přístup k datům
* Operace vlastní rozhraní API

Obě operace zajištění ověřování napříč všech zprostředkovatelů identity povolené ve službě Azure App Service, včetně poskytovatelů sociálních identit jako je Facebook, Twitter, Google a Microsoft a také Azure Active Directory pro podnikové identity.

Můžete najít ukázky pro každý případ použití v [ukázky adresáře na Githubu].

## <a name="supported-platforms"></a>Podporované platformy
Azure Mobile Apps uzlu SDK podporuje aktuální verzi LTS uzlu a novější.  Od verze zápis, je na nejnovější verzi LTS v4.5.0 uzlu.  Jiné verze uzlu může fungovat, ale nejsou podporovány.

Azure Mobile Apps uzlu SDK podporuje dvě databáze ovladače – ovladač uzlu mssql podporuje SQL Azure a místní instance systému SQL Server.  Ovladač sqlite3 podporuje pouze do jedné instance databáze SQLite.

### <a name="howto-cmdline-basicapp"></a>Postupy: vytvoření základní Node.js back-end pomocí příkazového řádku
Všechny back-end Azure App Service Mobile aplikace Node.js se spustí jako ExpressJS aplikace.  ExpressJS je nejoblíbenější webové služby rozhraní k dispozici pro Node.js.  Můžete vytvořit základní [Express] aplikace následujícím způsobem:

1. V příkazu nebo v okně prostředí PowerShell vytvořte adresář pro váš projekt.

        mkdir basicapp
2. Spusťte npm init inicializovat struktura balíčku.

        cd basicapp
        npm init

    Příkaz init npm požádá sadu otázky k chybě při inicializaci projektu.  Najdete příklad výstupu:

    ![Init – výstup npm][0]
3. Z úložiště npm nainstalujte knihovny express a aplikace azure mobile.

        npm install --save express azure-mobile-apps
4. Vytvoření souboru app.js k implementaci základní mobilní serveru.

        var express = require('express'),
            azureMobileApps = require('azure-mobile-apps');

        var app = express(),
            mobile = azureMobileApps();

        // Define a TodoItem table
        mobile.tables.add('TodoItem');

        // Add the mobile API so it is accessible as a Web API
        app.use(mobile);

        // Start listening on HTTP
        app.listen(process.env.PORT || 3000);

Tato aplikace vytvoří WebAPI se mobile optimalizované s jeden koncový bod (`/tables/TodoItem`), který poskytuje přístup bez ověřování pro základní úložiště dat SQL pomocí dynamické schématu.  Je vhodné pro následující rychlé spuštění knihovny klienta:

* [Rychlý start Android klienta]
* [Rychlý start Apache Cordova klienta]
* [iOS klienta rychlý start]
* [Rychlé spuštění klienta Windows Store]
* [Rychlý start Xamarin.iOS klienta]
* [Rychlé spuštění klienta Xamarin.Android]
* [Rychlý start Xamarin.Forms klienta]

Můžete najít kód pro tuto základní aplikaci v [basicapp ukázce na Githubu].

### <a name="howto-vs2015-basicapp"></a>Postupy: vytvoření back-end uzlu pomocí sady Visual Studio 2015
Visual Studio 2015 vyžaduje rozšíření při vývoji aplikací Node.js v prostředí IDE.  Chcete-li začít, nainstalujte [Node.js Tools 1.1 pro sadu Visual Studio].  Po instalaci nástrojů pro Node.js pro sadu Visual Studio vytvořte aplikaci 4.x Express:

1. Otevřete **nový projekt** dialogové okno (z **soubor** > **nový** > **projekt...** ).
2. Rozbalte položku **šablony** > **JavaScript** > **Node.js**.
3. Vyberte **aplikace základní Azure Node.js Express 4**.
4. Zadejte název projektu.  Klikněte na *OK*.

    ![Nový projekt sady Visual Studio 2015][1]
5. Klikněte pravým tlačítkem myši **npm** uzel a vyberte možnost **nainstalovat nové balíčky npm...** .
6. Musíte aktualizovat katalog npm na vytvoření vaší první aplikace Node.js.  Klikněte na tlačítko **aktualizovat** v případě potřeby.
7. Zadejte *aplikace azure mobile* do vyhledávacího pole.  Klikněte **azure mobile apps 2.0.0** balíček a potom klikněte na **instalovat balíček**.

    ![Instalace nové balíčky npm][2]
8. Klikněte na **Zavřít**.
9. Otevřete *app.js* souboru přidání podpory pro Azure Mobile Apps SDK.  Na řádku 6 at dolní knihovny vyžadují příkazy, přidejte následující kód:

        var bodyParser = require('body-parser');
        var azureMobileApps = require('azure-mobile-apps');

    Na přibližně řádku 27 po další app.use příkazy přidejte následující kód:

        app.use('/users', users);

        // Azure Mobile Apps Initialization
        var mobile = azureMobileApps();
        mobile.tables.add('TodoItem');
        app.use(mobile);

    Uložte soubor.
10. Spusťte aplikaci místně (rozhraní API je zpracování na http://localhost: 3000) nebo publikování v Azure.

### <a name="create-node-backend-portal"></a>Postupy: vytvoření back-end Node.js, pomocí portálu Azure
Můžete vytvořit právo back-end mobilní aplikace v [portál Azure]. Můžete buď postupujte podle následujících kroků nebo vytvořit pomocí následujících klientských a serverových společně [vytvoření mobilní aplikace](app-service-mobile-ios-get-started.md) kurzu. Tento kurz obsahuje zjednodušenou verzi tyto pokyny a je nejvhodnější pro ověření projekty na konceptu.

[!INCLUDE [app-service-mobile-dotnet-backend-create-new-service-classic](../../includes/app-service-mobile-dotnet-backend-create-new-service-classic.md)]

Zpět v *Začínáme* okno, v části **vytvořit tabulku rozhraní API**, zvolte **Node.js** jako vaše **back-end jazyk**.
Zaškrtněte políčko pro "**beru na vědomí, že tato akce přepíše všechny lokality obsahu.**", pak klikněte na tlačítko **vytvořit tabulku TodoItem**.

### <a name="download-quickstart"></a>Postupy: stažení projektu pro rychlý start kódu Node.js back-end pomocí Git
Při vytváření back-end mobilní aplikace Node.js pomocí portálu **úvodní** okně projekt Node.js se vytvoří a nasadí na váš web. Můžete přidat tabulky a rozhraní API a upravit soubory kódu pro back-end Node.js na portálu. Různé nástroje pro nasazení můžete také stáhnout projektu back-end, můžete přidat nebo upravit tabulek a rozhraní API a pak znovu publikovat projektu. Další informace najdete v tématu [Příručka pro nasazení Azure App Service]. Následující postup používá úložiště Git ke stažení rychlé spuštění kódu projektu.

1. Pokud jste tak již neučinili, nainstalujte Git. Kroky potřebné k instalaci Git lišit podle operačních systémů. V tématu [instalace Git](http://git-scm.com/book/en/Getting-Started-Installing-Git) distribuce specifické pro operační systém a instalaci.
2. Postupujte podle kroků v [povolit úložišti aplikace služby App Service](../app-service/app-service-deploy-local-git.md#Step3) povolit úložiště Git pro váš web back-end, proto poznámku o nasazení uživatelské jméno a heslo.
3. V okně pro váš back-end mobilní aplikace, poznamenejte si **adresy URL pro klon Git** nastavení.
4. Spuštění `git clone` příkaz pomocí Gitu klonovat adresu URL, zadat heslo, pokud jsou povinné, jako v následujícím příkladu:

        $ git clone https://username@todolist.scm.azurewebsites.net:443/todolist.git
5. Přejděte do místního adresáře, který v předchozím příkladu je /todolist a Všimněte si, že byly staženy soubory projektu. Vyhledejte `todoitem.json` v soubor `/tables` adresáře.  Tento soubor definuje oprávnění v tabulce.  Také umožňuje vyhledat `todoitem.js` souboru ve stejném adresáři, který definuje této operace CRUD skriptů pro tabulku.
6. Po provedení změn do souborů projektu, spusťte následující příkaz pro přidání, potvrzení a pak odeslat změny na web:

        $ git commit -m "updated the table script"
        $ git push origin master

    Když přidáte nové soubory do projektu, musíte nejdřív provést `git add .` příkaz.

Pokaždé, když novou sadu potvrzení vložena do lokality, je znovu publikovat web.

### <a name="howto-publish-to-azure"></a>Postupy: publikování back-end Node.js v Azure
Microsoft Azure poskytuje mnoho mechanismy pro publikování váš back-end Azure App Service Mobile aplikace Node.js ve službě Azure.  Jedná se o použití nástroje pro nasazení, které jsou integrované do sady Visual Studio, nástroje příkazového řádku a průběžné nasazování pro různé zdrojového kódu.  Další informace v tomto tématu najdete v tématu [Příručka pro nasazení Azure App Service].

Aplikační služba Azure má konkrétní Rady, jak aplikaci Node.js, která si měli projít před nasazením:

* Postup [zadejte verzi uzlu]
* Postup [použijte moduly uzlu]

### <a name="howto-enable-homepage"></a>Postupy: povolení domovskou stránku pro aplikaci
Mnoho aplikací jsou kombinací webové a mobilní aplikace a rozhraní ExpressJS umožňuje zkombinovat dva omezující vlastnosti.  V některých případech ale můžete chtít pouze mobilní rozhraní implementovat.  Je vhodné zajistit, že cílová stránka zajistit app service je spuštěná.  Můžete zadat vlastní domovské stránky, nebo povolit dočasné domovskou stránku.  Pokud chcete povolit dočasné domovskou stránku, použijte následující postupy k vytváření instancí Azure Mobile Apps:

    var mobile = azureMobileApps({ homePage: true });

Pokud chcete pouze tuto možnost k dispozici při vývoji místně, můžete přidat toto nastavení vaší `azureMobile.js` souboru.

## <a name="TableOperations"></a>Operace s tabulkou
Server SDK pro Node.js aplikace azure mobile poskytuje mechanismus ke zveřejnění tabulek dat uložených ve službě Azure SQL Database jako WebAPI.  Jsou k dispozici pět operace.

| Operace | Popis |
| --- | --- |
| GET /tables/*tablename* |Získat všechny záznamy v tabulce |
| GET /tables/*tablename*/:id |Získejte konkrétní záznam v tabulce |
| POST /tables/*tablename* |Vytvořit záznam v tabulce |
| Oprava /tables/*tablename*/:id |Aktualizace záznamu v tabulce |
| ODSTRANĚNÍ /tables/*tablename*/:id |Odstranit záznam v tabulce |

Podporuje tato WebAPI [OData] a rozšiřuje schéma tabulky pro podporu [synchronizaci dat offline].

### <a name="howto-dynamicschema"></a>Postupy: definování tabulky pomocí dynamické schématu
Před použitím tabulky, musí být definovaný.  Tabulky lze definovat pomocí statické schématu (kde vývojář definuje sloupců ve schématu) nebo dynamicky (kde sady SDK řídí schéma založené na příchozí požadavky). Kromě toho můžete řídit vývojář konkrétních aspektů WebAPI přidáním kódu jazyka Javascript do definice.

Jako osvědčený postup musí definovat každá tabulka v souboru jazyka Javascript v adresáři tabulky a potom použít metodu tables.import() importovat tabulky.  Rozšíření aplikace basic, souboru app.js se upraví:

    var express = require('express'),
        azureMobileApps = require('azure-mobile-apps');

    var app = express(),
        mobile = azureMobileApps();

    // Define the database schema that is exposed
    mobile.tables.import('./tables');

    // Provide initialization of any tables that are statically defined
    mobile.tables.initialize().then(function () {
        // Add the mobile API so it is accessible as a Web API
        app.use(mobile);

        // Start listening on HTTP
        app.listen(process.env.PORT || 3000);
    });

Definice tabulky. / tables/TodoItem.js:

    var azureMobileApps = require('azure-mobile-apps');

    var table = azureMobileApps.table();

    // Additional configuration for the table goes here

    module.exports = table;

Tabulky ve výchozím nastavení používá dynamické schématu.  Chcete-li vypnout dynamické schématu globálně, nastavte nastavení aplikace **MS_DynamicSchema** má hodnotu false v rámci portálu Azure.

Kompletní příklad v můžete najít [todo ukázce na Githubu].

### <a name="howto-staticschema"></a>Postupy: definování tabulky pomocí statické schématu
Můžete definovat explicitně sloupce, které chcete zpřístupnit prostřednictvím WebAPI.  Aplikace azure mobile Node.js SDK automaticky přidá žádné další sloupce požadované pro synchronizaci dat offline do seznamu, který zadáte.  Například klientské aplikace rychlý start vyžadovat tabulku s dva sloupce: text (řetězec) a dokončete (boolean).  
Tabulka je možné definovat v tabulce JavaScript souboru definice (umístěný v adresáři tabulky) následujícím způsobem:

    var azureMobileApps = require('azure-mobile-apps');

    var table = azureMobileApps.table();

    // Define the columns within the table
    table.columns = {
        "text": "string",
        "complete": "boolean"
    };

    // Turn off dynamic schema
    table.dynamicSchema = false;

    module.exports = table;

Pokud definujete tabulky staticky, musíte také zavolat metodu tables.initialize() vytvořit schéma databáze při spuštění.  Vrátí metodu tables.initialize() [Promise] tak, aby webové služby neobsluhuje žádosti před databáze během inicializace.

### <a name="howto-sqlexpress-setup"></a>Postupy: použití SQL Express jako úložiště dat vývoj v místním počítači
Azure Mobile Apps AzureMobile aplikace uzlu SDK poskytuje tři možnosti obsluhující data z pole: Sada SDK poskytuje tři možnosti obsluhující data z pole:

* Použití **paměti** ovladače k poskytování úložiště dočasnou příklad
* Použití **mssql** ovladače k poskytování úložiště dat SQL Express pro vývoj
* Použití **mssql** ovladače k poskytování úložiště dat služby Azure SQL Database pro produkční prostředí

Azure Mobile Apps Node.js SDK používá [mssql Node.js balíček] vytvořit a používat připojení k SQL Express a SQL Database.  Tento balíček vyžaduje, abyste povolili připojení TCP ve vaší instanci SQL Express.

> [!TIP]
> Ovladač paměti neposkytuje kompletní sadu zařízení pro testování.  Pokud chcete otestovat váš back-end místně, doporučujeme použít úložiště dat SQL Express a ovladač mssql.
>
>

1. Stáhněte a nainstalujte [Microsoft SQL Server 2014 Express].  Zkontrolujte, zda že nainstalovat systém SQL Server 2014 Express s edicí nástroje.  Pokud požadujete explicitně podpora 64bitových technologií, 32bitová verze spotřebovává méně paměti při spuštění.
2. Spusťte Správce konfigurace SQL serveru 2014.

   1. Rozbalte **konfigurace sítě serveru SQL Server** uzel ve stromu levé nabídce.
   2. Klikněte na tlačítko **protokoly pro funkci SQLEXPRESS**.
   3. Klikněte pravým tlačítkem na **TCP/IP** a vyberte **povolit**.  Klikněte na tlačítko **OK** v dialogovém okně automaticky otevírané okno.
   4. Klikněte pravým tlačítkem na **TCP/IP** a vyberte **vlastnosti**.
   5. Klikněte **IP adresy** kartě.
   6. Najít **IPAll** uzlu.  V **TCP Port** zadejte **1433**.

          ![Configure SQL Express for TCP/IP][3]
   7. Klikněte na **OK**.  Klikněte na tlačítko **OK** v dialogovém okně automaticky otevírané okno.
   8. Klikněte na tlačítko **služby SQL Server** v nabídce levé stromu.
   9. Klikněte pravým tlačítkem na **SQL Server (SQLEXPRESS)** a vyberte **restartovat**
   10. Zavřete Správci konfigurace systému SQL Server 2014.
3. Spusťte SQL Server 2014 Management Studio a připojte se k místní instanci SQL Express

   1. Klikněte pravým tlačítkem na vaší instance v Průzkumníku objektů a vyberte **vlastnosti**
   2. Vyberte **zabezpečení** stránky.
   3. Ujistěte se, **režimu SQL Server a ověřování systému Windows** je vybrána
   4. Klikněte na tlačítko **OK**.

          ![Configure SQL Express Authentication][4]
   5. Rozbalte položku **zabezpečení** > **přihlášení** v Průzkumníku objektů
   6. Klikněte pravým tlačítkem na **přihlášení** a vyberte **nové přihlašovací údaje...**
   7. Zadejte přihlašovací jméno.  Vyberte **Ověřování SQL Serveru**.  Zadejte heslo a potom zadejte stejné heslo v **potvrzení hesla**.  Heslo musí splňovat požadavky na složitost systému Windows.
   8. Klikněte na tlačítko **OK**.

          ![Add a new user to SQL Express][5]
   9. Klikněte pravým tlačítkem na nové přihlášení a vyberte **vlastnosti**
   10. Vyberte **role serveru** stránky
   11. Zaškrtněte políčko vedle položky **dbcreator** role serveru
   12. Klikněte na tlačítko **OK**.
   13. Zavřete SQL Management Studio Server 2015

Zkontrolujte, zda že záznam uživatelské jméno a heslo, které jste vybrali.  Musíte přiřadit další role serveru nebo oprávnění v závislosti na požadavcích vaší konkrétní databáze.

Čtení aplikace Node.js **SQLCONNSTR_MS_TableConnectionString** proměnnou prostředí pro připojovací řetězec pro tuto databázi.  Tuto proměnnou lze nastavit v rámci vašeho prostředí.  Můžete například použít PowerShell k nastavení této proměnné prostředí:

    $env:SQLCONNSTR_MS_TableConnectionString = "Server=127.0.0.1; Database=mytestdatabase; User Id=azuremobile; Password=T3stPa55word;"

Přístup k databázi přes připojení TCP/IP a zadejte uživatelské jméno a heslo pro připojení.

### <a name="howto-config-localdev"></a>Postupy: nakonfigurujete svůj projekt pro místní vývoj
Azure Mobile Apps přečte soubor JavaScript s názvem *azureMobile.js* z místního systému souborů.  Nepoužívejte pro konfiguraci Azure Mobile Apps SDK v produkčním prostředí – použijte nastavení aplikace v rámci tohoto souboru [portál Azure] místo.  *AzureMobile.js* soubor by měl exportovat objekt konfigurace.  Nejběžnější nastavení jsou:

* Nastavení databáze
* Nastavení protokolování diagnostiky
* Alternativní nastavení CORS

Příklad *azureMobile.js* odpovídá souboru implementace předchozí nastavení databáze:

    module.exports = {
        cors: {
            origins: [ 'localhost' ]
        },
        data: {
            provider: 'mssql',
            server: '127.0.0.1',
            database: 'mytestdatabase',
            user: 'azuremobile',
            password: 'T3stPa55word'
        },
        logging: {
            level: 'verbose'
        }
    };

Doporučujeme, abyste přidali *azureMobile.js* k vaší *.gitignore* souboru (nebo jiné zdrojového kódu ignorovat souboru) zabránit hesla ukládají v cloudu.  Vždy konfigurujte nastavení produkční v nastavení aplikace v rámci [portál Azure].

### <a name="howto-appsettings"></a>Postupy: Konfigurace nastavení aplikace pro mobilní aplikace
Většina nastavení v *azureMobile.js* soubor mít ekvivalentní nastavení aplikace [portál Azure].  Ke konfiguraci vaší aplikace v nastavení aplikace použijte následující seznam:

| Nastavení aplikace | *azureMobile.js* nastavení | Popis | Platné hodnoty |
|:--- |:--- |:--- |:--- |
| **MS_MobileAppName** |jméno |Název aplikace |řetězec |
| **MS_MobileLoggingLevel** |Logging.level |Úroveň minimální protokolu zpráv do protokolu |Chyba, upozornění, informace o podrobné nastavení, ladění, i |
| **MS_DebugMode** |ladit |Povolit nebo zakázat režim ladění |Hodnota TRUE, false |
| **MS_TableSchema** |data.Schema |Výchozí název schématu pro tabulky SQL |String (výchozí: dbo) |
| **MS_DynamicSchema** |data.dynamicSchema |Povolit nebo zakázat režim ladění |Hodnota TRUE, false |
| **MS_DisableVersionHeader** |verze (je nastavený na nedefinované) |Zakáže hlavičky X-záhlaví ZUMO-Server-Version |Hodnota TRUE, false |
| **MS_SkipVersionCheck** |skipversioncheck |Zakáže Kontrola verze klientského rozhraní API |Hodnota TRUE, false |

Nastavení aplikace nastavit:

1. Přihlaste se k portálu [portál Azure].
2. Vyberte **všechny prostředky** nebo **App Services** pak klikněte na název vaší mobilní aplikace.
3. Otevře se okno nastavení ve výchozím nastavení. Pokud není, klikněte na tlačítko **nastavení**.
4. Klikněte na tlačítko **nastavení aplikace** v hlavní nabídce.
5. Přejděte do části Nastavení aplikace.
6. Pokud vaše aplikace, nastavení už existuje, klikněte na hodnotu nastavení aplikace a příslušnou hodnotu upravte.
7. Pokud vaše aplikace nastavení neexistuje, zadejte do pole klíč a hodnotu v poli hodnota nastavení aplikace.
8. Po dokončení, klikněte na tlačítko **Uložit**.

Změna většinu nastavení aplikace vyžaduje restartování služby.

### <a name="howto-use-sqlazure"></a>Postupy: použití SQL Database jako produkční úložiště dat.
<!--- ALTERNATE INCLUDE - we can't use ../includes/app-service-mobile-dotnet-backend-create-new-service.md - slightly different semantics -->

Používání Azure SQL Database jako úložiště dat je stejný jako mezi všechny typy aplikací Azure App Service. Pokud jste to ještě neudělali, postupujte podle těchto kroků můžete vytvořit back-end mobilní aplikace.

1. Přihlaste se k portálu [portál Azure].
2. V horní části levé části okna, klikněte na tlačítko **+ nový** tlačítko > **Web + mobilní** > **mobilní aplikace**, pak zadejte název pro váš back-end mobilní aplikace.
3. V **skupiny prostředků** zadejte stejný název jako vaše aplikace.
4. Plán aplikační služby výchozí je vybrána.  Pokud chcete změnit plán služby App Service, můžete provést kliknutím plán služby App Service > **+ vytvořit nový**.  Zadejte název nového plánu služby App Service a vyberte požadované místo.  Klikněte na tlačítko cenová úroveň a vyberte příslušné cenovou úroveň pro službu. Vyberte **zobrazit všechny** do zobrazení více ceny možnosti, jako například **volné** a **sdílené**.  Až vyberete cenovou úroveň, klikněte na **vyberte** tlačítko.  Zpět v **plán služby App Service** okně klikněte na tlačítko **OK**.
5. Klikněte na možnost **Vytvořit**. Zřizování back-end mobilní aplikace může trvat několik minut.  Po zřízení back-end mobilní aplikace, portál ho otevře **nastavení** okno pro back-end mobilní aplikace.

Po vytvoření back-end mobilní aplikace, můžete se připojit existující databázi SQL k váš back-end mobilní aplikace nebo vytvořit novou databázi SQL.  V této části vytvoříme databázi SQL.

> [!NOTE]
> Pokud už máte databázi ve stejném umístění jako back-end mobilní aplikace, můžete místo toho zvolíte **použít existující databázi** a pak vyberte tuto databázi. Používání databáze v jiném umístění se nedoporučuje kvůli vyšší latence.
>
>

1. V nové mobilní aplikace back-end, klikněte na **nastavení** > **mobilní aplikace** > **Data** > **+ přidat**.
2. V **přidat datové připojení** okně klikněte na tlačítko **SQL Database – nakonfigurujte požadovaná nastavení** > **vytvořit novou databázi**.  Zadejte název nové databáze v **název** pole.
3. Klikněte na tlačítko **Server**.  V **nový server** okno, zadejte název jedinečné serveru v **název serveru** pole a zadejte vhodný **přihlašovací jméno správce serveru** a **heslo**.  Ujistěte se, **povolit službám azure přístup k serveru** je zaškrtnuté.  Klikněte na **OK**.

    ![Vytvoření databáze Azure SQL][6]
4. Na **novou databázi** okně klikněte na tlačítko **OK**.
5. Zpět na **přidat datové připojení** vyberte **připojovací řetězec**, zadejte přihlašovací jméno a heslo, které jste zadali při vytváření databáze.  Pokud použijete existující databázi, zadejte přihlašovací údaje pro tuto databázi.  Po zadání, klikněte na tlačítko **OK**.
6. Zpět na **přidat datové připojení** znovu, klikněte na **OK** k vytvoření databáze.

<!--- END OF ALTERNATE INCLUDE -->

Vytváření databáze může trvat několik minut.  Použití **oznámení** oblasti můžete sledovat průběh nasazení.  Není průběhu až do databáze byla úspěšně nasazena.  Jakmile úspěšně nasazena, je pro instanci databáze SQL na váš mobilní back-end nastavení aplikace vytvořit připojovací řetězec.  Zobrazí se toto nastavení aplikace **nastavení** > **nastavení aplikace** > **připojovací řetězce**.

### <a name="howto-tables-auth"></a>Postupy: ověřování vyžadovat pro přístup k tabulkám
Pokud chcete použít ověřování aplikace služby s koncovým bodem tabulky, je nutné nakonfigurovat aplikaci služby ověřování v [portál Azure] první.  Další podrobnosti o konfiguraci ověřování ve službě Azure App Service projděte si v Průvodci konfigurace pro zprostředkovatele identity, kterou chcete použít:

* [Postup konfigurace ověřování Azure Active Directory]
* [Postup konfigurace ověřování Facebook]
* [Postup konfigurace ověřování Google]
* [Jak nakonfigurovat Microsoft Authentication]
* [Jak nakonfigurovat ověřování služby Twitter]

Každá tabulka měla vlastností, které můžete použít k řízení přístupu k tabulce.  Následující příklad ukazuje staticky definovaných tabulku s je vyžadováno ověření.

    var azureMobileApps = require('azure-mobile-apps');

    var table = azureMobileApps.table();

    // Define the columns within the table
    table.columns = {
        "text": "string",
        "complete": "boolean"
    };

    // Turn off dynamic schema
    table.dynamicSchema = false;

    // Require authentication to access the table
    table.access = 'authenticated';

    module.exports = table;

Vlastnost přístupu může trvat jednu ze tří hodnot

* *Anonymní* označuje, že klientská aplikace může číst data bez ověřování
* *ověření* indikuje, že klientská aplikace, musí poslat platný ověřovací token s požadavkem
* *zakázané* označuje, že tato tabulka je aktuálně zakázán

Pokud vlastnost přístupu není definován, je povolený přístup bez ověřování.

### <a name="howto-tables-getidentity"></a>Postupy: použití ověřování deklarací identity s tabulkami
Můžete nastavit různé deklarace identity, které jsou požadovány při nastavení ověřování.  Tyto deklarace nejsou obvykle dostupné prostřednictvím `context.user` objektu.  Však mohou být načteny pomocí `context.user.getIdentity()` metoda.  `getIdentity()` Metoda vrátí Promise, který se přeloží na objekt.  Objekt se ukládá do klíčů metodou ověřování (facebook, google, twitter, microsoftaccount nebo aad).

Například pokud jste nastavili Account Microsoft ověřování a žádosti, které deklarace identity e-mailové adresy, můžete přidat e-mailovou adresu na záznam s řadičem následující tabulku:

    var azureMobileApps = require('azure-mobile-apps');

    // Create a new table definition
    var table = azureMobileApps.table();

    table.columns = {
        "emailAddress": "string",
        "text": "string",
        "complete": "boolean"
    };
    table.dynamicSchema = false;
    table.access = 'authenticated';

    /**
    * Limit the context query to those records with the authenticated user email address
    * @param {Context} context the operation context
    * @returns {Promise} context execution Promise
    */
    function queryContextForEmail(context) {
        return context.user.getIdentity().then((data) => {
            context.query.where({ emailAddress: data.microsoftaccount.claims.emailaddress });
            return context.execute();
        });
    }

    /**
    * Adds the email address from the claims to the context item - used for
    * insert operations
    * @param {Context} context the operation context
    * @returns {Promise} context execution Promise
    */
    function addEmailToContext(context) {
        return context.user.getIdentity().then((data) => {
            context.item.emailAddress = data.microsoftaccount.claims.emailaddress;
            return context.execute();
        });
    }

    // Configure specific code when the client does a request
    // READ - only return records belonging to the authenticated user
    table.read(queryContextForEmail);

    // CREATE - add or overwrite the userId based on the authenticated user
    table.insert(addEmailToContext);

    // UPDATE - only allow updating of record belong to the authenticated user
    table.update(queryContextForEmail);

    // DELETE - only allow deletion of records belong to the authenticated uer
    table.delete(queryContextForEmail);

    module.exports = table;

Pokud chcete zobrazit, jaké deklarace identity jsou k dispozici, zobrazíte pomocí webového prohlížeče `/.auth/me` koncový bod vašeho webu.

### <a name="howto-tables-disabled"></a>Postupy: zakázat přístup k určité tabulky operací
Kromě zobrazování v tabulce, vlastnost přístupu slouží k řízení jednotlivých operací.  Existují čtyři operace:

* *Přečtěte si* RESTful získat operace pro tabulku
* *Vložit* je operace RESTful POST v tabulce
* *Aktualizovat* je operace RESTful opravy v tabulce
* *Odstranit* RESTful odstranit operace v tabulce

Můžete například zadat jen pro čtení neověřené tabulku:

    var azureMobileApps = require('azure-mobile-apps');

    var table = azureMobileApps.table();

    // Read-Only table - only allow READ operations
    table.read.access = 'anonymous';
    table.insert.access = 'disabled';
    table.update.access = 'disabled';
    table.delete.access = 'disabled';

    module.exports = table;

### <a name="howto-tables-query"></a>Postupy: Upravit dotaz, který se používá s operace s tabulkou
Běžné požadavky pro operace s tabulkou je poskytnout omezeným zobrazením data.  Například může zadat tabulku, která je označené ID ověřeného uživatele tak, že můžete pouze pro čtení nebo aktualizovat vlastní záznamy.  Následující definice tabulky obsahuje tato funkce:

    var azureMobileApps = require('azure-mobile-apps');

    var table = azureMobileApps.table();

    // Define a static schema for the table
    table.columns = {
        "userId": "string",
        "text": "string",
        "complete": "boolean"
    };
    table.dynamicSchema = false;

    // Require authentication for this table
    table.access = 'authenticated';

    // Ensure that only records for the authenticated user are retrieved
    table.read(function (context) {
        context.query.where({ userId: context.user.id });
        return context.execute();
    });

    // When adding records, add or overwrite the userId with the authenticated user
    table.insert(function (context) {
        context.item.userId = context.user.id;
        return context.execute();
    });

    module.exports = table;

Operace, které obvykle spuštění dotazu mají vlastnost dotazu, můžete upravit místo, kde klauzule. Vlastnost dotaz je [QueryJS] objekt, který se používá k převodu dotaz OData na jinou hodnotu, která může zpracovat data back-end.  Jednoduché rovnosti případech (např. předchozí jeden) lze mapu. Můžete také přidat konkrétní klauzule SQL:

    context.query.where('myfield eq ?', 'value');

### <a name="howto-tables-softdelete"></a>Postupy: Konfigurace obnovitelného odstranění na tabulce
Obnovitelného odstranění neodstraní ve skutečnosti záznamy.  Místo toho se označí je jako v databázi odstranit nastavením odstraněný sloupec na hodnotu true.  Azure Mobile Apps SDK automaticky odebere obnovitelné odstranění záznamů z výsledků, pokud Mobile Client SDK používá IncludeDeleted().  Pokud chcete nakonfigurovat tabulku pro obnovitelného odstranění, nastavte `softDelete` vlastnost v souboru definice tabulky:

    var azureMobileApps = require('azure-mobile-apps');

    var table = azureMobileApps.table();

    // Define the columns within the table
    table.columns = {
        "text": "string",
        "complete": "boolean"
    };

    // Turn off dynamic schema
    table.dynamicSchema = false;

    // Turn on Soft Delete
    table.softDelete = true;

    // Require authentication to access the table
    table.access = 'authenticated';

    module.exports = table;

Byste měli zavést mechanismus pro vymazání záznamů – buď z klientské aplikace, prostřednictvím webové úlohy, funkce Azure nebo prostřednictvím vlastní rozhraní API.

### <a name="howto-tables-seeding"></a>Postupy: počáteční hodnoty databázi daty
Při vytváření nové aplikace, budete možná chtít počáteční hodnoty tabulku s daty.  To lze provést v rámci soubor JavaScript definice tabulky následujícím způsobem:

    var azureMobileApps = require('azure-mobile-apps');

    var table = azureMobileApps.table();

    // Define the columns within the table
    table.columns = {
        "text": "string",
        "complete": "boolean"
    };
    table.seed = [
        { text: 'Example 1', complete: false },
        { text: 'Example 2', complete: true }
    ];

    // Turn off dynamic schema
    table.dynamicSchema = false;

    // Require authentication to access the table
    table.access = 'authenticated';

    module.exports = table;

Synchronizace replik indexů data je možné pouze při vytváření tabulky pomocí Azure Mobile Apps SDK.  Pokud v tabulce již existuje v databázi, je do tabulky vložit žádná data.  Pokud je zapnutá dynamická schéma, schéma odvodit z dosazená data.

Doporučujeme, aby explicitně volání `tables.initialize()` metodu pro vytvoření tabulky při spuštění služby.

### <a name="Swagger"></a>Postupy: Povolení podpory Swagger
Azure App Service Mobile Apps se dodává s integrovanou [Swagger] podporovat.  Povolení podpory Swagger, nejprve nainstalujte rozhraní swagger jako závislost:

    npm install --save swagger-ui

Po instalaci můžete povolit podporu Swagger v konstruktoru sady Azure Mobile Apps:

    var mobile = azureMobileApps({ swagger: true });

Budete pravděpodobně pouze chcete povolit podporu Swagger v edicích vývoj.  Můžete to provést s využitím `NODE_ENV` nastavení aplikace:

    var mobile = azureMobileApps({ swagger: process.env.NODE_ENV !== 'production' });

Koncový bod swagger se nachází v http://*yoursite*.azurewebsites.net/swagger.  Dostanete uživatelské rozhraní Swagger pomocí `/swagger/ui` koncový bod.  Pokud budete chtít vyžadovat ověřování napříč celou aplikaci, vytvoří Swagger k chybě.  Nejlepších výsledků dosáhnete, zvolte tak, aby měl neověřené požadavky pomocí ověřování Azure App Service / nastavení autorizace, pak řídit, ověřování pomocí `table.access` vlastnost.

Můžete také přidat možnost Swagger vaše `azureMobile.js` souboru, pouze pokud chcete podporu Swagger při vývoji místně.

## <a name="a-namepushpush-notifications"></a><a name="push">Nabízená oznámení
Mobilní aplikace se integruje s Azure Notification Hubs umožnit odesílání cílové nabízených oznámení na miliony zařízení pro všechny hlavní platformy. Pomocí centra oznámení můžete odesílat nabízená oznámení iOS, Android a Windows zařízení. Další informace o všech, že lze provádět pomocí Notification Hubs najdete v tématu [přehledu této služby](../notification-hubs/notification-hubs-push-notification-overview.md).

### </a><a name="send-push"></a>Postupy: odesílání nabízených oznámení
Následující kód ukazuje, jak používat nabízená objekt k odesílání vysílání nabízených oznámení do registrovaná zařízení iOS:

    // Create an APNS payload.
    var payload = '{"aps": {"alert": "This is an APNS payload."}}';

    // Only do the push if configured
    if (context.push) {
        // Send a push notification using APNS.
        context.push.apns.send(null, payload, function (error) {
            if (error) {
                // Do something or log the error.
            }
        });
    }

Vytvořením šablony nabízené registrace z klienta, můžete místo toho odeslat zprávu nabízené šablony do zařízení na všech podporovaných platformách. Následující kód ukazuje, jak odeslat oznámení šablony:

    // Define the template payload.
    var payload = '{"messageParam": "This is a template payload."}';

    // Only do the push if configured
    if (context.push) {
        // Send a template notification.
        context.push.send(null, payload, function (error) {
            if (error) {
                // Do something or log the error.
            }
        });
    }


### <a name="push-user"></a>Postupy: odesílání nabízených oznámení pro ověřené uživatele pomocí značek
Pokud ověřený uživatel zaregistruje pro nabízená oznámení, se automaticky přidá značku ID uživatele pro registraci. Pomocí této značky možné posílat nabízená oznámení pro všechna zařízení zaregistrovat podle konkrétního uživatele. Následující kód získá identifikátor SID uživatele, které zadal žádost a odešle šablony nabízených oznámení do každé registrace zařízení pro tohoto uživatele:

    // Only do the push if configured
    if (context.push) {
        // Send a notification to the current user.
        context.push.send(context.user.id, payload, function (error) {
            if (error) {
                // Do something or log the error.
            }
        });
    }

Při registraci pro nabízená oznámení z ověřený klient, ujistěte se, že před pokusem o registraci dokončením ověřování.

## <a name="CustomAPI"></a>Vlastní rozhraní API
### <a name="howto-customapi-basic"></a>Postupy: definování vlastní rozhraní API
Kromě přístupu k datům rozhraní API prostřednictvím koncového bodu /tables Azure Mobile Apps může poskytnout vlastní pokrytí rozhraní API.  Vlastní rozhraní API jsou definovány podobným způsobem jako definice tabulek a stejná přístup zařízení, včetně ověřování.

Pokud chcete použít ověřování aplikace služby s rozhraním API vlastní, musíte nakonfigurovat aplikaci služby ověřování v [portál Azure] první.  Další podrobnosti o konfiguraci ověřování ve službě Azure App Service projděte si v Průvodci konfigurace pro zprostředkovatele identity, kterou chcete použít:

* [Postup konfigurace ověřování Azure Active Directory]
* [Postup konfigurace ověřování Facebook]
* [Postup konfigurace ověřování Google]
* [Jak nakonfigurovat Microsoft Authentication]
* [Jak nakonfigurovat ověřování služby Twitter]

Vlastní rozhraní API jsou definovány stejným způsobem jako rozhraní API tabulky.

1. Vytvoření **rozhraní api** adresáře
2. Vytvořte soubor JavaScript definice rozhraní API v **rozhraní api** adresáře.
3. Použít metodu import pro import **rozhraní api** adresáře.

Zde je definice rozhraní api prototypu založeny na basic aplikace ukázku, kterou jsme použili předtím.

    var express = require('express'),
        azureMobileApps = require('azure-mobile-apps');

    var app = express(),
        mobile = azureMobileApps();

    // Import the Custom API
    mobile.api.import('./api');

    // Add the mobile API so it is accessible as a Web API
    app.use(mobile);

    // Start listening on HTTP
    app.listen(process.env.PORT || 3000);

Podívejme příklad rozhraní API, která vrací data na serveru pomocí *Date.now()* metoda.  Tady je soubor api/date.js:

    var api = {
        get: function (req, res, next) {
            var date = { currentTime: Date.now() };
            res.status(200).type('application/json').send(date);
        });
    };

    module.exports = api;

Každý parametr je jedním z standardní RESTful příkazy - GET, POST, PATCH nebo odstranit.  Metoda je standard [ExpressJS Middleware] funkce, která odesílá požadované výstup.

### <a name="howto-customapi-auth"></a>Postupy: ověřování vyžadovat pro přístup k vlastní rozhraní API
Azure Mobile Apps SDK implementuje ověřování stejným způsobem pro koncový bod tabulky a vlastní rozhraní API.  Chcete-li přidat ověřování do rozhraní API vyvinuté v předchozí části, přidejte **přístup** vlastnost:

    var api = {
        get: function (req, res, next) {
            var date = { currentTime: Date.now() };
            res.status(200).type('application/json').send(date);
        });
    };
    // All methods must be authenticated.
    api.access = 'authenticated';

    module.exports = api;

Můžete také zadat ověřování na konkrétní operace:

    var api = {
        get: function (req, res, next) {
            var date = { currentTime: Date.now() };
            res.status(200).type('application/json').send(date);
        }
    };
    // The GET methods must be authenticated.
    api.get.access = 'authenticated';

    module.exports = api;

Pro vlastní rozhraní API, které vyžadují ověřování musí použít stejný token, který se používá pro koncový bod tabulky.

### <a name="howto-customapi-auth"></a>Postupy: zpracování nahrávání velkých souborů
Azure Mobile Apps SDK používá [textu analyzátor middleware](https://github.com/expressjs/body-parser) přijmout a dekódování obsah textu v vaše žádost.  Můžete předkonfigurovat textu analyzátor, který má přijmout nahrávání větší souborů:

    var express = require('express'),
        bodyParser = require('body-parser'),
        azureMobileApps = require('azure-mobile-apps');

    var app = express(),
        mobile = azureMobileApps();

    // Set up large body content handling
    app.use(bodyParser.json({ limit: '50mb' }));
    app.use(bodyParser.urlencoded({ limit: '50mb', extended: true }));

    // Import the Custom API
    mobile.api.import('./api');

    // Add the mobile API so it is accessible as a Web API
    app.use(mobile);

    // Start listening on HTTP
    app.listen(process.env.PORT || 3000);

Soubor je před samotným přenosem kódování base-64.  Tím se zvyšuje velikost skutečné nahrávání (a proto velikost musíte vzít v úvahu pro).

### <a name="howto-customapi-sql"></a>Postup: provedení vlastní SQL příkazy
Azure Mobile Apps SDK umožňuje přístup k celé kontextu prostřednictvím objektu žádosti umožňuje snadno provést parametrizované příkazy SQL k poskytovateli definované datové:

    var api = {
        get: function (request, response, next) {
            // Check for parameters - if not there, pass on to a later API call
            if (typeof request.params.completed === 'undefined')
                return next();

            // Define the query - anything that can be handled by the mssql
            // driver is allowed.
            var query = {
                sql: 'UPDATE TodoItem SET complete=@completed',
                parameters: [{
                    completed: request.params.completed
                }]
            };

            // Execute the query.  The context for Azure Mobile Apps is available through
            // request.azureMobile - the data object contains the configured data provider.
            request.azureMobile.data.execute(query)
            .then(function (results) {
                response.json(results);
            });
        }
    };

    api.get.access = 'authenticated';
    module.exports = api;

## <a name="Debugging"></a>Ladění, snadno tabulek a snadno rozhraní API
### <a name="howto-diagnostic-logs"></a>Postupy: ladění, Diagnostika a řešení potíží s Azure Mobile apps
Azure App Service poskytuje několik ladění a řešení potíží s techniky pro aplikace Node.js.
Naleznete v následujících článcích začít při řešení potíží váš back-end Node.js Mobile:

* [Monitorování služby Azure App Service]
* [Povolit protokolování diagnostiky v Azure App Service]
* [Řešení potíží s Azure App Service v sadě Visual Studio]

Node.js aplikací mít přístup k širokou škálu protokolů diagnostiky nástroje.  Interně používá Azure Mobile Apps Node.js SDK [Winstona] pro protokolování diagnostiky.  Protokolování je automaticky povolen povolením režimu ladění, nebo nastavením **MS_DebugMode** nastavení aplikace nastavte na hodnotu true v [portál Azure]. Vygenerovaný protokolů se objeví v diagnostických protokolů na [portál Azure].

### <a name="in-portal-editing"></a><a name="work-easy-tables"></a>Postupy: práce s tabulkami snadno na portálu Azure
Snadno tabulek v portálu umožňuje vytvářet a pracovat s tabulkami přímo na portálu. Datové sady můžete uložit do snadno tabulky ve formátu CSV. Všimněte si, že nemůžete použít názvy vlastností (v datovou sadu sdíleného svazku clusteru), které je v konfliktu s názvy vlastností systému back-end Azure Mobile Apps. Názvy vlastností systému jsou následující:
* CreatedAt
* updatedAt
* Odstranit
* verze

I můžete upravit pomocí editoru služby aplikace operace s tabulkou. Když kliknete na tlačítko **snadno tabulky** v nastavení vašeho back-end serveru, můžete přidávat, upravovat nebo odstranění tabulky. Můžete také zobrazit data v tabulce.

![Práce s tabulkami snadno](./media/app-service-mobile-node-backend-how-to-use-server-sdk/mobile-apps-easy-tables.png)

Na panelu příkazů pro tabulku k dispozici jsou následující příkazy:

* **Změna oprávnění** – změnit oprávnění pro čtení, vložit, aktualizovat a odstranit operací v tabulce.
  Možnosti jsou k povolení anonymního přístupu, vyžadují ověřování, nebo zakázat veškerý přístup k operaci.
* **Upravte skript** -souboru skriptu pro tabulku je otevřen v editoru služby aplikace.
* **Správa schématu** – přidání nebo odstranění sloupce nebo změna tabulky indexu.
* **Odstranit tabulku** -zkrátí existující tabulky se odstraňuje všechny řádky dat, ale ponechat schématu beze změny.
* **Odstranit řádky** -odstranit jednotlivé řádky dat.
* **Protokoly streamování zobrazení** -vás spojí se služba streamování protokolu pro váš web.

### <a name="work-easy-apis"></a>Postupy: práce s snadno rozhraní API na portálu Azure
Snadno rozhraní API na portálu umožňuje vytvářet a pracovat s vlastní rozhraní API přímo na portálu. Můžete upravit skripty rozhraní API pomocí editoru služby aplikace.

Když kliknete na tlačítko **rozhraní API pro snadný** v nastavení vašeho back-end serveru, můžete přidávat, upravovat nebo odstranit vlastní koncový bod rozhraní API.

![Práci s rozhraními API snadno](./media/app-service-mobile-node-backend-how-to-use-server-sdk/mobile-apps-easy-apis.png)

Na portálu můžete změnit přístupová oprávnění pro danou akci HTTP, upravte soubor skriptu rozhraní API v editoru služby aplikace nebo zobrazení protokolů streamování.

### <a name="online-editor"></a>Postupy: úpravy kódu v editoru služby aplikace
Portál Azure umožňuje upravit soubory skriptu back-end Node.js v editoru služby aplikace bez nutnosti stáhnout projektu do místního počítače. Postup úpravy souborů skriptů v editoru online:

1. V okně back-end mobilní aplikace, klikněte na tlačítko **všechna nastavení** > buď **snadno tabulky** nebo **rozhraní API pro snadný**, klikněte na tabulku nebo rozhraní API a pak klikněte na tlačítko **upravte skript**. Soubor skriptu je otevřen v editoru služby aplikace.

    ![App Service Editor](./media/app-service-mobile-node-backend-how-to-use-server-sdk/mobile-apps-visual-studio-editor.png)
2. Provedené změny do souboru kódu v editoru online. Změny se při psaní automaticky uloží.

<!-- Images -->
[0]: ./media/app-service-mobile-node-backend-how-to-use-server-sdk/npm-init.png
[1]: ./media/app-service-mobile-node-backend-how-to-use-server-sdk/vs2015-new-project.png
[2]: ./media/app-service-mobile-node-backend-how-to-use-server-sdk/vs2015-install-npm.png
[3]: ./media/app-service-mobile-node-backend-how-to-use-server-sdk/sqlexpress-config.png
[4]: ./media/app-service-mobile-node-backend-how-to-use-server-sdk/sqlexpress-authconfig.png
[5]: ./media/app-service-mobile-node-backend-how-to-use-server-sdk/sqlexpress-newuser-1.png
[6]: ./media/app-service-mobile-node-backend-how-to-use-server-sdk/dotnet-backend-create-db.png

<!-- URLs -->
[Rychlý start Android klienta]: app-service-mobile-android-get-started.md
[Rychlý start Apache Cordova klienta]: app-service-mobile-cordova-get-started.md
[iOS klienta rychlý start]: app-service-mobile-ios-get-started.md
[Rychlý start Xamarin.iOS klienta]: app-service-mobile-xamarin-ios-get-started.md
[Rychlé spuštění klienta Xamarin.Android]: app-service-mobile-xamarin-android-get-started.md
[Rychlý start Xamarin.Forms klienta]: app-service-mobile-xamarin-forms-get-started.md
[Rychlé spuštění klienta Windows Store]: app-service-mobile-windows-store-dotnet-get-started.md
[synchronizaci dat offline]: app-service-mobile-offline-data-sync.md
[Postup konfigurace ověřování Azure Active Directory]: ../app-service/app-service-mobile-how-to-configure-active-directory-authentication.md
[Postup konfigurace ověřování Facebook]: ../app-service/app-service-mobile-how-to-configure-facebook-authentication.md
[Postup konfigurace ověřování Google]: ../app-service/app-service-mobile-how-to-configure-google-authentication.md
[Jak nakonfigurovat Microsoft Authentication]: ../app-service/app-service-mobile-how-to-configure-microsoft-authentication.md
[Jak nakonfigurovat ověřování služby Twitter]: ../app-service/app-service-mobile-how-to-configure-twitter-authentication.md
[Příručka pro nasazení Azure App Service]: ../app-service/app-service-deploy-local-git.md
[Monitorování služby Azure App Service]: ../app-service/web-sites-monitor.md
[Povolit protokolování diagnostiky v Azure App Service]: ../app-service/web-sites-enable-diagnostic-log.md
[Řešení potíží s Azure App Service v sadě Visual Studio]: ../app-service/web-sites-dotnet-troubleshoot-visual-studio.md
[zadejte verzi uzlu]: ../nodejs-specify-node-version-azure-apps.md
[použijte moduly uzlu]: ../nodejs-use-node-modules-azure-apps.md
[Create a new Azure App Service]: ../app-service/
[azure-mobile-apps]: https://www.npmjs.com/package/azure-mobile-apps
[Express]: http://expressjs.com/
[Swagger]: http://swagger.io/

[portál Azure]: https://portal.azure.com/
[OData]: http://www.odata.org
[Promise]: https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Promise
[basicapp ukázce na Githubu]: https://github.com/azure/azure-mobile-apps-node/tree/master/samples/basic-app
[todo ukázce na Githubu]: https://github.com/azure/azure-mobile-apps-node/tree/master/samples/todo
[ukázky adresáře na Githubu]: https://github.com/azure/azure-mobile-apps-node/tree/master/samples
[static-schema sample on GitHub]: https://github.com/azure/azure-mobile-apps-node/tree/master/samples/static-schema
[QueryJS]: https://github.com/Azure/queryjs
[Node.js Tools 1.1 pro sadu Visual Studio]: https://github.com/Microsoft/nodejstools/releases/tag/v1.1-RC.2.1
[mssql Node.js balíček]: https://www.npmjs.com/package/mssql
[Microsoft SQL Server 2014 Express]: http://www.microsoft.com/en-us/server-cloud/Products/sql-server-editions/sql-server-express.aspx
[ExpressJS Middleware]: http://expressjs.com/guide/using-middleware.html
[Winstona]: https://github.com/winstonjs/winston
