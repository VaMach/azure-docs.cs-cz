---
title: "Vytvoření webové aplikace .NET Core využívající databázi SQL ve službě Azure App Service v Linuxu | Dokumentace Microsoftu"
description: "Naučte se zprovoznit aplikaci .NET Core ve službě Azure App Service v Linuxu s připojením k databázi SQL."
services: app-service\web
documentationcenter: dotnet
author: cephalin
manager: syntaxc4
editor: 
ms.assetid: 0b4d7d0e-e984-49a1-a57a-3c0caa955f0e
ms.service: app-service-web
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: tutorial
ms.date: 10/10/2017
ms.author: cephalin
ms.custom: mvc
ms.openlocfilehash: 804294e91375e0fb5b11190ae969710bbd4c15b1
ms.sourcegitcommit: 9d317dabf4a5cca13308c50a10349af0e72e1b7e
ms.translationtype: HT
ms.contentlocale: cs-CZ
ms.lasthandoff: 02/01/2018
---
# <a name="build-a-net-core-and-sql-database-web-app-in-azure-app-service-on-linux"></a>Vytvoření webové aplikace .NET Core využívající databázi SQL ve službě Azure App Service v Linuxu

> [!NOTE]
> Tento článek nasadí aplikaci do služby App Service v Linuxu. Nasazení do služby App Service ve _Windows_ je popsáno v tématu[Vytvoření webové aplikace .NET Core využívající SQL Database ve službě Azure App Service](../app-service-web-tutorial-dotnetcore-sqldb.md).
>

[App Service v Linuxu](app-service-linux-intro.md) je vysoce škálovatelná služba s automatickými opravami pro hostování webů s využitím operačního systému Linux. Tento kurz demonstruje postup při vytváření webové aplikace .NET Core a jejím připojení k databázi SQL. Po dokončení budete mít aplikaci .NET Core MVC spuštěnou ve službě App Service v Linuxu.

![aplikace spuštěná ve službě App Service v Linuxu](./media/tutorial-dotnetcore-sqldb-app/azure-app-in-browser.png)

Naučíte se:

> [!div class="checklist"]
> * Vytvořit databázi SQL v Azure
> * Připojit aplikaci .NET Core k databázi SQL
> * Nasadit aplikaci do Azure
> * Aktualizovat datový model a znovu nasadit aplikaci
> * Streamovat diagnostické protokoly z Azure
> * Spravovat aplikaci na webu Azure Portal

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="prerequisites"></a>Požadavky

Pro absolvování tohoto kurzu potřebujete:

1. [Nainstalovat Git](https://git-scm.com/).
1. [Nainstalovat sadu .NET Core SDK 1.1.2](https://github.com/dotnet/core/blob/master/release-notes/download-archives/1.1.2-download.md)

## <a name="create-local-net-core-app"></a>Vytvoření místní aplikace .NET Core

V tomto kroku nastavíte místní projekt .NET Core.

### <a name="clone-the-sample-application"></a>Klonování ukázkové aplikace

V okně terminálu přejděte pomocí příkazu `cd` do pracovního adresáře.

Spuštěním následujících příkazů naklonujte ukázkové úložiště a přejděte do jeho kořenové složky.

```bash
git clone https://github.com/azure-samples/dotnetcore-sqldb-tutorial
cd dotnetcore-sqldb-tutorial
```

Ukázkový projekt obsahuje základní aplikaci CRUD (vytváření-čtení-aktualizace-odstraňování) pomocí [Entity Framework Core](https://docs.microsoft.com/ef/core/).

### <a name="run-the-application"></a>Spuštění aplikace

Spuštěním následujících příkazů nainstalujte požadované balíčky, spusťte operace migrace databáze a spusťte aplikaci.

```bash
dotnet restore
dotnet ef database update
dotnet run
```

V prohlížeči přejděte na `http://localhost:5000`. Vyberte odkaz **Vytvořit nový** a vytvořte několik položek _úkolů_.

![úspěšné připojení k databázi SQL](./media/tutorial-dotnetcore-sqldb-app/local-app-in-browser.png)

.NET Core můžete kdykoli zastavit stisknutím `Ctrl+C` v terminálu.

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

## <a name="create-production-sql-database"></a>Vytvoření produkční databáze SQL

V tomto kroku vytvoříte databázi SQL v Azure. Po nasazení do Azure bude aplikace používat tuto cloudovou databázi.

Jako databáze SQL se v tomto kurzu používá [Azure SQL Database](/azure/sql-database/).

### <a name="create-a-resource-group"></a>Vytvoření skupiny prostředků

[!INCLUDE [Create resource group](../../../includes/app-service-web-create-resource-group-no-h.md)]

### <a name="create-a-sql-database-logical-server"></a>Vytvoření logického serveru databáze SQL

V Cloud Shellu vytvořte logický server databáze SQL příkazem[`az sql server create`](/cli/azure/sql/server?view=azure-cli-latest#az_sql_server_create).

Nahraďte zástupný symbol *\<server_name>* jedinečným názvem databáze SQL. Tento název se používá jako součást koncového bodu databáze SQL (`<server_name>.database.windows.net`), takže název musí být jedinečný v rámci všech logických serverů v Azure. Název smí obsahovat jen malá písmena, číslice a znak spojovníku (-) a musí mít délku 3 až 50 znaků. Dále nahraďte zástupné symboly *\<db_username>* a *\<db_password>* uživatelským jménem a heslem, které si zvolíte. 


```azurecli-interactive
az sql server create --name <server_name> --resource-group myResourceGroup --location "West Europe" --admin-user <db_username> --admin-password <db_password>
```

Po vytvoření logického serveru databáze SQL se v rozhraní příkazového řádku Azure zobrazí podobné informace jako v následujícím příkladu:

```json
{
  "administratorLogin": "sqladmin",
  "administratorLoginPassword": null,
  "fullyQualifiedDomainName": "<server_name>.database.windows.net",
  "id": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/myResourceGroup/providers/Microsoft.Sql/servers/<server_name>",
  "identity": null,
  "kind": "v12.0",
  "location": "westeurope",
  "name": "<server_name>",
  "resourceGroup": "myResourceGroup",
  "state": "Ready",
  "tags": null,
  "type": "Microsoft.Sql/servers",
  "version": "12.0"
}
```

### <a name="configure-a-server-firewall-rule"></a>Konfigurace pravidla brány firewall serveru

Vytvoření [pravidla brány firewall na úrovni serveru Azure SQL Database](../../sql-database/sql-database-firewall-configure.md) pomocí příkazu [`az sql server firewall create`](/cli/azure/sql/server/firewall-rule?view=azure-cli-latest#az_sql_server_firewall_rule_create). Pokud je jako počáteční i koncová adresa IP nastavená hodnota 0.0.0.0, je brána firewall otevřená jen pro ostatní prostředky Azure. 

```azurecli-interactive
az sql server firewall-rule create --resource-group myResourceGroup --server <server_name> --name AllowYourIp --start-ip-address 0.0.0.0 --end-ip-address 0.0.0.0
```

### <a name="create-a-database"></a>Vytvoření databáze

Vytvořte na serveru databázi s [úrovní výkonu S0](../../sql-database/sql-database-service-tiers.md) pomocí příkazu [`az sql db create`](/cli/azure/sql/db?view=azure-cli-latest#az_sql_db_create).

```azurecli-interactive
az sql db create --resource-group myResourceGroup --server <server_name> --name coreDB --service-objective S0
```

### <a name="create-connection-string"></a>Vytvoření připojovacího řetězce

V následujícím řetězci nahraďte zástupné symboly *\<server_name>*, *\<db_username>* a *\<db_password>* hodnotami, které jste použili dřív.

```
Server=tcp:<server_name>.database.windows.net,1433;Initial Catalog=coreDB;Persist Security Info=False;User ID=<db_username>;Password=<db_password>;MultipleActiveResultSets=False;Encrypt=True;TrustServerCertificate=False;Connection Timeout=30;
```

Toto je připojovací řetězec pro aplikaci .NET Core. Zkopírujte ho pro pozdější použití.

## <a name="deploy-app-to-azure"></a>Nasazení aplikace do Azure

V tomto kroku nasadíte aplikaci .NET Core připojenou k databázi SQL do služby App Service v Linuxu.

### <a name="configure-local-git-deployment"></a>Konfigurace nasazení místního gitu

[!INCLUDE [Configure a deployment user](../../../includes/configure-deployment-user-no-h.md)]

### <a name="create-an-app-service-plan"></a>Vytvoření plánu služby App Service

[!INCLUDE [Create app service plan](../../../includes/app-service-web-create-app-service-plan-linux-no-h.md)]

### <a name="create-a-web-app"></a>Vytvoření webové aplikace

[!INCLUDE [Create web app](../../../includes/app-service-web-create-web-app-dotnetcore-no-h.md)] 

### <a name="configure-an-environment-variable"></a>Konfigurace proměnné prostředí

Pokud chcete nastavit pro svou aplikaci Azure připojovací řetězce, použijte v Cloud Shellu příkaz [`az webapp config appsettings set`](/cli/azure/webapp/config/appsettings?view=azure-cli-latest#az_webapp_config_appsettings_set). V následujícím příkazu nahraďte zástupný symbol *\<app name>* a parametr *\<connection_string>* připojovacím řetězcem, který jste vytvořili dříve.

```azurecli-interactive
az webapp config connection-string set --resource-group myResourceGroup --name <app name> --settings MyDbConnection='<connection_string>' --connection-string-type SQLServer
```

Dále nastavte proměnnou `ASPNETCORE_ENVIRONMENT` aplikace na hodnotu _Production_. Toto nastavení vám umožňuje zjistit, jestli je aplikace spuštěná v Azure, protože v místním vývojovém prostředí používáte SQLite a v prostředí Azure používáte SQL Database.

V následujícím příkladu se konfiguruje proměnná `ASPNETCORE_ENVIRONMENT` aplikace ve webové aplikaci Azure. Nahraďte zástupný symbol *\<app_name>*.

```azurecli-interactive
az webapp config appsettings set --name <app_name> --resource-group myResourceGroup --settings ASPNETCORE_ENVIRONMENT="Production"
```

### <a name="connect-to-sql-database-in-production"></a>Připojení k databázi SQL v produkčním prostředí

V místním úložišti otevřete Startup.cs a vyhledejte následující kód:

```csharp
services.AddDbContext<MyDatabaseContext>(options =>
        options.UseSqlite("Data Source=localdatabase.db"));
```

Nahraďte ho následujícím kódem, který používá proměnné prostředí, které jste nakonfigurovali dřív.

```csharp
// Use SQL Database if in Azure, otherwise, use SQLite
if(Environment.GetEnvironmentVariable("ASPNETCORE_ENVIRONMENT") == "Production")
    services.AddDbContext<MyDatabaseContext>(options =>
            options.UseSqlServer(Configuration.GetConnectionString("MyDbConnection")));
else
    services.AddDbContext<MyDatabaseContext>(options =>
            options.UseSqlite("Data Source=MvcMovie.db"));

// Automatically perform database migration
services.BuildServiceProvider().GetService<MyDatabaseContext>().Database.Migrate();
```

Pokud tento kód zjistí, že je spuštěný v produkčním prostředí (což znamená prostředí Azure), použije nakonfigurovaný připojovací řetězec pro připojení k databázi SQL.

Volání `Database.Migrate()` vám pomůže při spuštění v Azure, protože automaticky vytvoří databáze, které vaše aplikace .NET Core potřebuje, podle příslušné konfigurace migrace. 

Uložte provedené změny a potom je potvrďte v úložišti Gitu. 

```bash
git commit -am "connect to SQLDB in Azure"
```

### <a name="push-to-azure-from-git"></a>Přenos z Gitu do Azure

[!INCLUDE [app-service-plan-no-h](../../../includes/app-service-web-git-push-to-azure-no-h.md)]

```bash
Counting objects: 98, done.
Delta compression using up to 8 threads.
Compressing objects: 100% (92/92), done.
Writing objects: 100% (98/98), 524.98 KiB | 5.58 MiB/s, done.
Total 98 (delta 8), reused 0 (delta 0)
remote: Updating branch 'master'.
remote: .
remote: Updating submodules.
remote: Preparing deployment for commit id '0c497633b8'.
remote: Generating deployment script.
remote: Project file path: ./DotNetCoreSqlDb.csproj
remote: Generated deployment script files
remote: Running deployment command...
remote: Handling ASP.NET Core Web Application deployment.
remote: .
remote: .
remote: .
remote: Finished successfully.
remote: Running post deployment command(s)...
remote: Deployment successful.
remote: App container will begin restart within 10 seconds.
To https://<app_name>.scm.azurewebsites.net/<app_name>.git
 * [new branch]      master -> master
```

### <a name="browse-to-the-azure-web-app"></a>Přechod do webové aplikace Azure

Ve webovém prohlížeči přejděte k nasazené webové aplikaci.

```bash
http://<app_name>.azurewebsites.net
```

Přidejte několik položek úkolů.

![aplikace spuštěná ve službě App Service v Linuxu](./media/tutorial-dotnetcore-sqldb-app/azure-app-in-browser.png)

**Blahopřejeme!** Používáte aplikaci .NET Core řízenou daty ve službě App Service v Linuxu.

## <a name="update-locally-and-redeploy"></a>Místní aktualizace a opětovné nasazení

V tomto kroku provedete změnu schématu databáze a publikujete ji v Azure.

### <a name="update-your-data-model"></a>Aktualizace datového modelu

Otevřete _Models\Todo.cs_ v editoru kódu. Do třídy `ToDo` přidejte následující vlastnost:

```csharp
public bool Done { get; set; }
```

### <a name="run-code-first-migrations-locally"></a>Místní spuštění migrace Code First

Spuštěním několika příkazů aktualizujte místní databázi.

```bash
dotnet ef migrations add AddProperty
```

Aktualizujte místní databázi:

```bash
dotnet ef database update
```

### <a name="use-the-new-property"></a>Použití nové vlastnosti

Proveďte v kódu několik změn, aby aplikace využívala vlastnost `Done`. Pro zjednodušení budete v tomto kurzu měnit jen zobrazení `Index` a `Create`, aby se vlastnost projevila v praxi.

Otevřete _Controllers\TodosController.cs_.

Najděte metodu `Create()` a přidejte vlastnosti `Done` do seznamu vlastností v atributu `Bind`. Po dokončení bude podpis vaší metody `Create()` vypadat podobně jako následující kód:

```csharp
public async Task<IActionResult> Create([Bind("ID,Description,CreatedDate,Done")] Todo todo)
```

Otevřete _Views\Todos\Create.cshtml_.

V kódu Razor byste měli vidět prvek `<div class="form-group">` pro `Description`a pak další prvek `<div class="form-group">` pro `CreatedDate`. Přímo za tyto dva prvky přidejte další prvek `<div class="form-group">` pro `Done`:

```csharp
<div class="form-group">
    <label asp-for="Done" class="col-md-2 control-label"></label>
    <div class="col-md-10">
        <input asp-for="Done" class="form-control" />
        <span asp-validation-for="Done" class="text-danger"></span>
    </div>
</div>
```

Otevřete _Views\Todos\Index.cshtml_.

Vyhledejte prázdný prvek `<th></th>`. Přímo nad tento prvek přidejte následující kód Razor:

```csharp
<th>
    @Html.DisplayNameFor(model => model.Done)
</th>
```

Najděte prvek `<td>`, který obsahuje pomocné rutiny značky `asp-action`. Přímo nad tento prvek přidejte následující kód Razor:

```csharp
<td>
    @Html.DisplayFor(modelItem => item.CreatedDate)
</td>
```

To je všechno, co potřebujete, aby se v zobrazení `Index` a `Create` projevily změny.

### <a name="test-your-changes-locally"></a>Místní test provedených změn

Spusťte aplikaci místně.

```bash
dotnet run
```

V prohlížeči přejděte na adresu `http://localhost:5000/`. Teď můžete přidat položku úkolu a zaškrtnout možnost **Hotovo**. Položka by se měla zobrazit na domovské stránce jako dokončená. Nezapomeňte, že v zobrazení `Edit` se pole `Done` nezobrazí, protože jste zobrazení `Edit` nezměnili.

### <a name="publish-changes-to-azure"></a>Publikování změn v Azure

```bash

git commit -am "added done field"
git push azure master
```

Po dokončení operace `git push` přejděte do webové aplikace Azure a vyzkoušejte nové funkce.

![Webová aplikace Azure po migraci Code First](./media/tutorial-dotnetcore-sqldb-app/this-one-is-done.png)

Všechny vaše existující položky úkolů jsou nadále zobrazené. Při opětovném publikování aplikace .NET Core nedojde ke ztrátě existujících dat v databázi SQL. Migrace Entity Framework Core také změní jen datové schéma, ale existující data ponechá beze změny.

## <a name="manage-your-azure-web-app"></a>Správa webové aplikace Azure

Přejděte na web [Azure Portal](https://portal.azure.com) k webové aplikaci, kterou jste vytvořili.

V levé nabídce klikněte na **App Services** a pak klikněte na název vaší webové aplikace Azure.

![Navigace portálem k webové aplikaci Azure](./media/tutorial-dotnetcore-sqldb-app/access-portal.png)

Na začátku portál zobrazí stránku **Přehled** vaší webové aplikace. Tato stránka poskytuje přehled, jak si vaše aplikace stojí. Tady můžete také provést základní úlohy správy, jako je procházení, zastavení, spuštění, restartování a odstranění. Karty na levé straně stránky obsahují různé stránky konfigurace, které můžete otevřít.

![Stránka služby App Service na webu Azure Portal](./media/tutorial-dotnetcore-sqldb-app/web-app-blade.png)

[!INCLUDE [cli-samples-clean-up](../../../includes/cli-samples-clean-up.md)]

<a name="next"></a>
## <a name="next-steps"></a>Další kroky

Naučili jste se:

> [!div class="checklist"]
> * Vytvořit databázi SQL v Azure
> * Připojit aplikaci .NET Core k databázi SQL
> * Nasadit aplikaci do Azure
> * Aktualizovat datový model a znovu nasadit aplikaci
> * Streamovat protokoly z Azure do terminálu
> * Spravovat aplikaci na webu Azure Portal

V dalším kurzu se dozvíte, jak namapovat vlastní název DNS na webovou aplikaci.

> [!div class="nextstepaction"]
> [Mapování existujícího vlastního názvu DNS na Azure Web Apps](../app-service-web-tutorial-custom-domain.md)