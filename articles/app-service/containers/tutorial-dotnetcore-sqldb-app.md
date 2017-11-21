---
title: "Vytvoření webové aplikace .NET Core a databáze SQL ve službě Azure App Service v systému Linux | Microsoft Docs"
description: "Další informace o získání aplikace na .NET Core práce ve službě Azure App Service pro systémy Linux, připojení k databázi SQL."
services: app-service\web
documentationcenter: nodejs
author: cephalin
manager: syntaxc4
editor: 
ms.assetid: 0b4d7d0e-e984-49a1-a57a-3c0caa955f0e
ms.service: app-service-web
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: nodejs
ms.topic: tutorial
ms.date: 10/10/2017
ms.author: cephalin
ms.custom: mvc
ms.openlocfilehash: ef68f64437935f08f76c29ecf15d574279cca7f1
ms.sourcegitcommit: f67f0bda9a7bb0b67e9706c0eb78c71ed745ed1d
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/20/2017
---
# <a name="build-a-net-core-and-sql-database-web-app-in-azure-app-service-on-linux"></a>Vytvoření webové aplikace .NET Core a databáze SQL ve službě Azure App Service v systému Linux

[Aplikační služby v systému Linux](app-service-linux-intro.md) nabízí vysoce škálovatelnou a automatických oprav webové hostitelské služby pomocí operační systém Linux. Tento kurz ukazuje, jak vytvořit webovou aplikaci .NET Core a připojte ho k databázi SQL. Když jste hotovi, budete mít .NET Core MVC aplikaci spuštěnou ve službě App Service v systému Linux.

![aplikaci spuštěnou ve službě App Service v systému Linux](./media/tutorial-dotnetcore-sqldb-app/azure-app-in-browser.png)

Co zjistíte, jak:

> [!div class="checklist"]
> * Vytvoření databáze SQL v Azure
> * Připojit .NET Core aplikace k databázi SQL
> * Nasazení aplikace do Azure
> * Aktualizovat datový model a aplikaci znovu nasaďte
> * Diagnostické protokoly datového proudu z Azure
> * Spravovat aplikaci na portálu Azure

## <a name="prerequisites"></a>Požadavky

Pro absolvování tohoto kurzu potřebujete:

1. [Nainstalovat Git](https://git-scm.com/).
1. [Instalace .NET Core SDK 1.1.2](https://github.com/dotnet/core/blob/master/release-notes/download-archives/1.1.2-download.md)

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="create-local-net-core-app"></a>Vytvořit místní aplikace .NET Core

V tomto kroku nastavíte místní projektu .NET Core.

### <a name="clone-the-sample-application"></a>Klonování ukázkové aplikace

V okně terminálu `cd` do pracovního adresáře.

Spusťte následující příkazy a klonovat úložiště v ukázkové a změňte svůj kořen.

```bash
git clone https://github.com/azure-samples/dotnetcore-sqldb-tutorial
cd dotnetcore-sqldb-tutorial
```

Ukázkový projekt obsahuje základní aplikace CRUD (vytvoření čtení aktualizace odstranění) pomocí [Entity Framework Core](https://docs.microsoft.com/ef/core/).

### <a name="run-the-application"></a>Spuštění aplikace

Spusťte následující příkazy pro instalaci požadovaných balíčků, spuštění migrace databáze a spusťte aplikaci.

```bash
dotnet restore
dotnet ef database update
dotnet run
```

V prohlížeči přejděte na `http://localhost:5000`. Vyberte **vytvořit nový** propojit a vytvořit pár _úkolů_ položky.

![úspěšně se připojuje k SQL Database](./media/tutorial-dotnetcore-sqldb-app/local-app-in-browser.png)

Chcete-li zastavit .NET Core kdykoli, stiskněte `Ctrl+C` v terminálu.

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

## <a name="create-production-sql-database"></a>Vytvoření provozní databáze SQL

V tomto kroku vytvoříte databázi SQL v Azure. Po nasazení aplikace do Azure se používá tato databáze cloudu.

Pro databázi SQL, tento kurz používá [Azure SQL Database](/azure/sql-database/).

### <a name="create-a-resource-group"></a>Vytvoření skupiny prostředků

[!INCLUDE [Create resource group](../../../includes/app-service-web-create-resource-group-no-h.md)]

### <a name="create-a-sql-database-logical-server"></a>Vytvoření logického serveru SQL Database

V prostředí cloudu, vytvořte logický server databáze SQL se [az sql server vytvořit](/cli/azure/sql/server#create) příkaz.

Nahraďte  *\<název_serveru >* zástupný symbol jedinečný název databáze SQL. Tento název se používá jako součást koncový bod SQL Database, `<server_name>.database.windows.net`, takže název musí být jedinečný v rámci všechny logické servery v Azure. Název musí obsahovat jenom malá písmena, číslice a pomlčky (-) a musí být v rozmezí 3 až 50 znaků. Také nahraďte  *\<db_username >* a  *\<db_password >* uživatelské jméno a heslo, podle vašeho výběru. 


```azurecli-interactive
az sql server create --name <server_name> --resource-group myResourceGroup --location "West Europe" --admin-user <db_username> --admin-password <db_password>
```

Při vytvoření logického serveru SQL Database rozhraní příkazového řádku Azure obsahuje informace o podobně jako v následujícím příkladu:

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

Vytvořte [pravidlo brány firewall na úrovni serveru služby Azure SQL Database](../../sql-database/sql-database-firewall-configure.md) pomocí příkazu [az sql server firewall create](/cli/azure/sql/server#create). Počáteční IP adresu a koncová IP adresa nastaven na hodnotu 0.0.0.0, je pouze otevřít bránu firewall pro ostatní prostředky služby Azure. 

```azurecli-interactive
az sql server firewall-rule create --resource-group myResourceGroup --server <server_name> --name AllowYourIp --start-ip-address 0.0.0.0 --end-ip-address 0.0.0.0
```

### <a name="create-a-database"></a>Vytvoření databáze

Vytvořte na serveru databázi s [úrovní výkonu S0](../../sql-database/sql-database-service-tiers.md) pomocí příkazu [az sql db create](/cli/azure/sql/db#create).

```azurecli-interactive
az sql db create --resource-group myResourceGroup --server <server_name> --name coreDB --service-objective S0
```

### <a name="create-connection-string"></a>Vytvoření připojovacího řetězce

Nahraďte následující řetězec s  *\<název_serveru >*,  *\<db_username >*, a  *\<db_password >* můžete použít dříve.

```
Server=tcp:<server_name>.database.windows.net,1433;Initial Catalog=coreDB;Persist Security Info=False;User ID=<db_username>;Password=<db_password>;MultipleActiveResultSets=False;Encrypt=True;TrustServerCertificate=False;Connection Timeout=30;
```

Toto je připojovací řetězec pro aplikace .NET Core. Zkopírujte jej později pro použití.

## <a name="deploy-app-to-azure"></a>Nasazení aplikace do Azure

V tomto kroku nasadíte aplikaci .NET Core databázi SQL připojit do služby App Service v systému Linux.

### <a name="configure-local-git-deployment"></a>Konfigurace nasazení místního gitu

[!INCLUDE [Configure a deployment user](../../../includes/configure-deployment-user-no-h.md)]

### <a name="create-an-app-service-plan"></a>Vytvoření plánu služby App Service

[!INCLUDE [Create app service plan](../../../includes/app-service-web-create-app-service-plan-linux-no-h.md)]

### <a name="create-a-web-app"></a>Vytvoření webové aplikace

[!INCLUDE [Create web app](../../../includes/app-service-web-create-web-app-dotnetcore-no-h.md)] 

### <a name="configure-an-environment-variable"></a>Nakonfigurujte proměnné prostředí

Pokud chcete nastavit připojovací řetězce pro vaše aplikace Azure, použijte [aktualizovat az webapp konfigurace appsettings](/cli/azure/webapp/config/appsettings#update) příkazu v prostředí cloudu. V následujícím příkazu nahraďte  *\<název aplikace >*, a taky  *\<connection_string >* parametr připojovacím řetězcem, který jste vytvořili dříve.

```azurecli-interactive
az webapp config connection-string set --resource-group myResourceGroup --name <app name> --settings MyDbConnection='<connection_string>' --connection-string-type SQLServer
```

Dále nastavte `ASPNETCORE_ENVIRONMENT` nastavení aplikace nastavte na _produkční_. Toto nastavení umožňuje vědět, zda jsou spuštěné v Azure, protože používáte SQLLite pro místní vývojové prostředí a databáze SQL pro prostředí Azure.

Následující příklad konfiguruje `ASPNETCORE_ENVIRONMENT` nastavení aplikace v Azure webové aplikace. Nahraďte  *\<app_name >* zástupný symbol.

```azurecli-interactive
az webapp config appsettings set --name <app_name> --resource-group myResourceGroup --settings ASPNETCORE_ENVIRONMENT="Production"
```

### <a name="connect-to-sql-database-in-production"></a>Připojení k databázi SQL v produkčním prostředí

V místní úložiště otevřete Startup.cs a vyhledejte následující kód:

```csharp
services.AddDbContext<MyDatabaseContext>(options =>
        options.UseSqlite("Data Source=localdatabase.db"));
```

Nahraďte ji následujícím kódem, který používá proměnné prostředí, které jste nakonfigurovali dřív.

```csharp
// Use SQL Database if in Azure, otherwise, use SQLLite
if(Environment.GetEnvironmentVariable("ASPNETCORE_ENVIRONMENT") == "Production")
    services.AddDbContext<DotNetCoreSqlDbContext>(options =>
            options.UseSqlServer(Configuration.GetConnectionString("MyDbConnection")));
else
    services.AddDbContext<DotNetCoreSqlDbContext>(options =>
            options.UseSqlite("Data Source=MvcMovie.db"));

// Automatically perform database migration
services.BuildServiceProvider().GetService<DotNetCoreSqlDbContext>().Database.Migrate();
```

Pokud tento kód zjistí, zda je spuštěna v produkčním prostředí, (což znamená, prostředí Azure) a poté používá připojovací řetězec je nakonfigurovaný pro připojení k databázi SQL.

`Database.Migrate()` Volání vám pomůže při spuštění v Azure, protože automaticky vytvoří databáze, vašim potřebám aplikace .NET Core, v závislosti na jeho konfiguraci migrace. 

Uložte provedené změny.

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

### <a name="browse-to-the-azure-web-app"></a>Přejděte do webové aplikace Azure

Přejděte do nasazené webové aplikace pomocí webového prohlížeče.

```bash
http://<app_name>.azurewebsites.net
```

Přidejte několik položek úkolů.

![aplikaci spuštěnou ve službě App Service v systému Linux](./media/tutorial-dotnetcore-sqldb-app/azure-app-in-browser.png)

**Blahopřejeme!** Používáte .NET Core aplikace na základě dat ve službě App Service v systému Linux.

## <a name="update-locally-and-redeploy"></a>Aktualizovat místně a znovu nasaďte

V tomto kroku změňte svého schématu databáze a publikujete ho v Azure.

### <a name="update-your-data-model"></a>Aktualizovat data modelu

Otevřete _Models\Todo.cs_ v editoru kódu. Přidejte následující vlastnosti, která má `ToDo` třídy:

```csharp
public bool Done { get; set; }
```

### <a name="run-code-first-migrations-locally"></a>Spustit migrace Code First místně

Spusťte několik příkazy provádění aktualizací místní databázi.

```bash
dotnet ef migrations add AddProperty
```

Aktualizace místní databázi:

```bash
dotnet ef database update
```

### <a name="use-the-new-property"></a>Použít nové vlastnosti

Některé změny v kódu k použití `Done` vlastnost. Pro zjednodušení v tomto kurzu se pouze chystáte změnit `Index` a `Create` zobrazení naleznete ve vlastnosti v akci.

Otevřete _Controllers\TodosController.cs_.

Najít `Create()` metoda a přidejte `Done` do seznamu vlastností v `Bind` atribut. Když jste hotovi, vaše `Create()` podpis metody vypadá jako následující kód:

```csharp
public async Task<IActionResult> Create([Bind("ID,Description,CreatedDate,Done")] Todo todo)
```

Otevřete _Views\Todos\Create.cshtml_.

V kódu Razor, měli byste vidět `<div class="form-group">` element pro `Description`a pak další `<div class="form-group">` element pro `CreatedDate`. Hned za tyto dva prvky, přidejte další `<div class="form-group">` element pro `Done`:

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

Vyhledejte prázdné `<th></th>` elementu. Nad tento element přidejte následující kód Razor:

```csharp
<th>
    @Html.DisplayNameFor(model => model.Done)
</th>
```

Najít `<td>` elementu, který obsahuje `asp-action` značky pomocné rutiny. Nad tento element přidejte následující kód Razor:

```csharp
<td>
    @Html.DisplayFor(modelItem => item.CreatedDate)
</td>
```

To je všechno potřebujete zobrazit změny v `Index` a `Create` zobrazení.

### <a name="test-your-changes-locally"></a>Otestujte provedené změny místně

Místní spuštění aplikace.

```bash
dotnet run
```

V prohlížeči přejděte na `http://localhost:5000/`. Teď můžete přidat položku seznamu úkolů a zkontrolujte **provádí**. Potom ho měli zobrazí v domovské stránce jako dokončené položka. Nezapomeňte, že `Edit` zobrazení nezobrazí `Done` pole, protože nebyla změní `Edit` zobrazení.

### <a name="publish-changes-to-azure"></a>Publikování změn do Azure

```bash

git commit -am "added done field"
git push azure master
```

Jednou `git push` je dokončení, přejděte do vaší webové aplikace Azure a vyzkoušet nové funkce.

![Webové aplikace Azure po migraci první kódu](./media/tutorial-dotnetcore-sqldb-app/this-one-is-done.png)

Všechny vaše existující položky seznamu úkolů se pořád zobrazí. Při opětovném aplikace .NET Core, není stávající data v databázi SQL ztraceny. Entity Framework Core migrace také, pouze změní schéma dat a svoje existující data zůstanou zachovány.

## <a name="manage-your-azure-web-app"></a>Správa Azure webové aplikace

Přejděte na [portál Azure](https://portal.azure.com) zobrazíte webové aplikace, které jste vytvořili.

V levé nabídce klikněte na **App Services** a pak klikněte na název vaší webové aplikace Azure.

![Navigace portálem k webové aplikaci Azure](./media/tutorial-dotnetcore-sqldb-app/access-portal.png)

Ve výchozím nastavení, zobrazí na portálu vaší webové aplikace **přehled** stránky. Tato stránka poskytuje přehled, jak si vaše aplikace stojí. Tady můžete také provést základní úlohy správy, jako je procházení, zastavení, spuštění, restartování a odstranění. Karty na levé straně stránky zobrazí stránek jinou konfiguraci, že můžete otevřít.

![Stránka služby App Service na webu Azure Portal](./media/tutorial-dotnetcore-sqldb-app/web-app-blade.png)

[!INCLUDE [cli-samples-clean-up](../../../includes/cli-samples-clean-up.md)]

<a name="next"></a>
## <a name="next-steps"></a>Další kroky

Co jste se naučili:

> [!div class="checklist"]
> * Vytvoření databáze SQL v Azure
> * Připojit .NET Core aplikace k databázi SQL
> * Nasazení aplikace do Azure
> * Aktualizovat datový model a aplikaci znovu nasaďte
> * Datový proud protokolů z Azure terminálu
> * Spravovat aplikaci na portálu Azure

Přechodu na dalším kurzu se dozvíte, jak namapovat vlastní název DNS do vaší webové aplikace.

> [!div class="nextstepaction"]
> [Mapování existujícího vlastního názvu DNS na Azure Web Apps](../app-service-web-tutorial-custom-domain.md)