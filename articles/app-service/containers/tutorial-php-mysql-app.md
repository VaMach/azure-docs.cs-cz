---
title: "Vytvoření webové aplikace PHP a databáze MySQL v Azure | Microsoft Docs"
description: "Další informace o získání aplikace PHP v Azure, funguje připojení k databázi MySQL v Azure."
services: app-service\web
documentationcenter: nodejs
author: cephalin
manager: erikre
editor: 
ms.assetid: 14feb4f3-5095-496e-9a40-690e1414bd73
ms.service: app-service-web
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: nodejs
ms.topic: tutorial
ms.date: 07/21/2017
ms.author: cephalin
ms.custom: mvc
ms.openlocfilehash: 11e8708987f4e085fc8bf1db10144283a9a17d2e
ms.sourcegitcommit: 3e3a5e01a5629e017de2289a6abebbb798cec736
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/27/2017
---
# <a name="build-a-php-and-mysql-web-app-in-azure"></a>Vytvoření webové aplikace PHP a databáze MySQL v Azure

[Aplikační služby v systému Linux](app-service-linux-intro.md) nabízí vysoce škálovatelnou a automatických oprav webové hostitelské služby pomocí operační systém Linux. Tento kurz ukazuje, jak vytvořit webovou aplikaci PHP a připojte ho k databázi MySQL. Jakmile budete hotovi, budete mít [Laravel](https://laravel.com/) aplikace běžící na App Service v systému Linux.

![Aplikace PHP, které jsou spuštěné v Azure App Service](./media/tutorial-php-mysql-app/complete-checkbox-published.png)

V tomto kurzu se naučíte:

> [!div class="checklist"]
> * Vytvoření databáze MySQL v Azure
> * Připojení aplikace PHP k MySQL
> * Nasazení aplikace do Azure
> * Aktualizovat datový model a aplikaci znovu nasaďte
> * Diagnostické protokoly datového proudu z Azure
> * Spravovat aplikaci na portálu Azure

## <a name="prerequisites"></a>Požadavky

Pro absolvování tohoto kurzu potřebujete:

* [Nainstalovat Git](https://git-scm.com/).
* [Instalace PHP 5.6.4 nebo novější](http://php.net/downloads.php)
* [Nainstalujte autora](https://getcomposer.org/doc/00-intro.md)
* Povolit následující rozšíření PHP Laravel potřebám: OpenSSL, PDO MySQL, Mbstring, Tokenizátor, XML
* [Instalace a spuštění MySQL](https://dev.mysql.com/doc/refman/5.7/en/installing.html) 

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="prepare-local-mysql"></a>Příprava místního MySQL

V tomto kroku vytvoříte databázi v místní server MySQL pro použití v tomto kurzu.

### <a name="connect-to-local-mysql-server"></a>Připojení k místnímu serveru MySQL

Okno terminálu připojte k místní server MySQL. Chcete-li spustit všechny příkazy v tomto kurzu můžete toto okno terminálu.

```bash
mysql -u root -p
```

Pokud se zobrazí výzva k zadání hesla, zadejte heslo pro `root` účtu. Pokud si nepamatujete heslo kořenového účtu, najdete v části [MySQL: jak resetovat hesla kořenového](https://dev.mysql.com/doc/refman/5.7/en/resetting-permissions.html).

Pokud váš příkaz úspěšně proběhne, MySQL serveru běží. Pokud ne, ujistěte se, zda je místní server MySQL spuštěná pomocí následujících [kroky po instalaci MySQL](https://dev.mysql.com/doc/refman/5.7/en/postinstallation.html).

### <a name="create-a-database-locally"></a>Vytvoření databáze místně

Na `mysql` výzvu, vytvořit databázi.

```sql 
CREATE DATABASE sampledb;
```

Ukončení připojení k serveru zadáním `quit`.

```sql
quit
```

<a name="step2"></a>

## <a name="create-a-php-app-locally"></a>Vytvoření aplikace PHP místně
V tomto kroku získat ukázkovou aplikaci Laravel, konfigurovat jeho připojení k databázi a spustit místně. 

### <a name="clone-the-sample"></a>Clone – ukázka

V okně terminálu `cd` do pracovního adresáře.

Ukázkové úložiště naklonujete spuštěním následujícího příkazu.

```bash
git clone https://github.com/Azure-Samples/laravel-tasks
```

`cd`do vašeho klonovaný adresáře.
Nainstalujte požadované balíčky.

```bash
cd laravel-tasks
composer install
```

### <a name="configure-mysql-connection"></a>Konfigurace připojení databáze MySQL

V kořenovém adresáři úložiště, vytvořte soubor s názvem *.env*. Zkopírujte následující proměnné do *.env* souboru. Nahraďte  _&lt;root_password >_ zástupný symbol MySQL kořenového hesla.

```
APP_ENV=local
APP_DEBUG=true
APP_KEY=SomeRandomString

DB_CONNECTION=mysql
DB_HOST=127.0.0.1
DB_DATABASE=sampledb
DB_USERNAME=root
DB_PASSWORD=<root_password>
```

Informace o tom, jak Laravel používá _.env_ souborů najdete v tématu [konfigurace prostředí Laravel](https://laravel.com/docs/5.4/configuration#environment-configuration).

### <a name="run-the-sample-locally"></a>Spustit ukázku místně

Spustit [migrace databáze Laravel](https://laravel.com/docs/5.4/migrations) k vytvoření tabulek aplikace potřebuje. Pokud chcete zjistit, které tabulky jsou vytvořené v byla migrace, vyhledejte v _databáze nebo migrace_ adresáře v úložišti Git.

```bash
php artisan migrate
```

Vygenerujte nový klíč Laravel aplikace.

```bash
php artisan key:generate
```

Spusťte aplikaci.

```bash
php artisan serve
```

V prohlížeči přejděte na `http://localhost:8000`. Na stránce přidáte několik úloh.

![PHP úspěšně připojí k MySQL](./media/tutorial-php-mysql-app/mysql-connect-success.png)

Chcete-li zastavit PHP, zadejte `Ctrl + C` v terminálu.

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

## <a name="create-mysql-in-azure"></a>Vytvoření databáze MySQL v Azure

V tomto kroku vytvoříte databázi MySQL v [Azure Database pro databázi MySQL (Preview)](/azure/mysql). Později nakonfigurujete aplikace PHP pro připojení k této databázi.

### <a name="create-a-resource-group"></a>Vytvoření skupiny prostředků

[!INCLUDE [Create resource group](../../../includes/app-service-web-create-resource-group-no-h.md)] 

### <a name="create-a-mysql-server"></a>Vytvoření databáze MySQL serveru

Vytvoření serveru ve službě Azure Database pro databázi MySQL (Preview) pomocí [az mysql server vytvořit](/cli/azure/mysql/server#create) příkaz.

V následujícím příkazu nahraďte název serveru MySQL, kde uvidíte  _&lt;mysql_server_name >_ zástupný symbol (platnými znaky jsou `a-z`, `0-9`, a `-`). Tento název je součástí názvu hostitele serveru MySQL (`<mysql_server_name>.database.windows.net`), musí být globálně jedinečný.

```azurecli-interactive
az mysql server create --name <mysql_server_name> --resource-group myResourceGroup --location "North Europe" --admin-user adminuser --admin-password MySQLAzure2017 --ssl-enforcement Disabled
```

Při vytvoření serveru MySQL rozhraní příkazového řádku Azure obsahuje informace o podobně jako v následujícím příkladu:

```json
{
  "administratorLogin": "adminuser",
  "administratorLoginPassword": null,
  "fullyQualifiedDomainName": "<mysql_server_name>.database.windows.net",
  "id": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/myResourceGroup/providers/Microsoft.DBforMySQL/servers/<mysql_server_name>",
  "location": "northeurope",
  "name": "<mysql_server_name>",
  "resourceGroup": "myResourceGroup",
  ...
}
```

### <a name="configure-server-firewall"></a>Konfigurace brány firewall serveru

Vytvořte pravidlo brány firewall pro váš server MySQL a povolíte připojení klienta pomocí [az mysql pravidla brány firewall-vytvořit](/cli/azure/mysql/server/firewall-rule#create) příkaz.

```azurecli-interactive
az mysql server firewall-rule create --name allIPs --server <mysql_server_name> --resource-group myResourceGroup --start-ip-address 0.0.0.0 --end-ip-address 255.255.255.255
```

> [!NOTE]
> Azure databáze pro databázi MySQL (Preview) nepodporuje aktuálně omezení počtu připojení pouze ke službám Azure. Jak budou dynamicky přiřazovat IP adresy v Azure, je lepší povolit všechny IP adresy. Služba není ve verzi preview. Plánujeme lepší metody pro zabezpečení databáze.
>

### <a name="connect-to-production-mysql-server-locally"></a>Připojení k serveru pro produkční MySQL místně

V okně terminálu připojte k serveru databáze MySQL v Azure. Použít hodnotu zadanou dříve pro  _&lt;mysql_server_name >_.

```bash
mysql -u adminuser@<mysql_server_name> -h <mysql_server_name>.database.windows.net -P 3306 -p
```

Pokud budete vyzváni k zadání hesla, použijte _$tr0ngPa$ w0rd!_, který jste zadali při vytvoření databáze.

### <a name="create-a-production-database"></a>Vytvoření provozní databáze

Na `mysql` výzvu, vytvořit databázi.

```sql
CREATE DATABASE sampledb;
```

### <a name="create-a-user-with-permissions"></a>Vytvořit uživatele s oprávněními

Vytvořte uživatele databáze názvem _phpappuser_ a pojmenujte ho všechna oprávnění `sampledb` databáze.

```sql
CREATE USER 'phpappuser' IDENTIFIED BY 'MySQLAzure2017'; 
GRANT ALL PRIVILEGES ON sampledb.* TO 'phpappuser';
```

Ukončení připojení k serveru zadáním `quit`.

```sql
quit
```

## <a name="connect-app-to-azure-mysql"></a>Připojení aplikace k Azure MySQL

V tomto kroku připojíte aplikace PHP pro databázi MySQL, kterou jste vytvořili v Azure Database pro databázi MySQL (Preview).

<a name="devconfig"></a>

### <a name="configure-the-database-connection"></a>Konfigurace připojení k databázi

V kořenovém adresáři úložiště vytvořit _. env.production_ soubor a zkopírujte do něj následující proměnné. Nahraďte zástupný symbol  _&lt;mysql_server_name >_.

```
APP_ENV=production
APP_DEBUG=true
APP_KEY=SomeRandomString

DB_CONNECTION=mysql
DB_HOST=<mysql_server_name>.database.windows.net
DB_DATABASE=sampledb
DB_USERNAME=phpappuser@<mysql_server_name>
DB_PASSWORD=MySQLAzure2017
```
<!-- MYSQL_SSL=true -->

Uložte změny.

> [!TIP]
> Zabezpečit informace o připojení databáze MySQL, tento soubor je již vyloučen z úložiště Git (viz _.gitignore_ v kořenovém adresáři úložiště). Později zjistíte, jak nakonfigurovat proměnné prostředí ve službě App Service připojit k vaší databázi ve službě Azure Database pro databázi MySQL (Preview). Proměnné prostředí, nemusíte *.env* souboru ve službě App Service.
>

<!-- ### Configure SSL certificate

By default, Azure Database for MySQL enforces SSL connections from clients. To connect to your MySQL database in Azure, you must use a _.pem_ SSL certificate.

Open _config/database.php_ and add the _sslmode_ and _options_ parameters to `connections.mysql`, as shown in the following code.

```php
'mysql' => [
    ...
    'sslmode' => env('DB_SSLMODE', 'prefer'),
    'options' => (env('MYSQL_SSL')) ? [
        PDO::MYSQL_ATTR_SSL_KEY    => '/ssl/certificate.pem', 
    ] : []
],
```

To learn how to generate this _certificate.pem_, see [Configure SSL connectivity in your application to securely connect to Azure Database for MySQL](../../mysql/howto-configure-ssl.md).

> [!TIP]
> The path _/ssl/certificate.pem_ points to an existing _certificate.pem_ file in the Git repository. This file is provided for convenience in this tutorial. For best practice, you should not commit your _.pem_ certificates into source control. 
> -->

### <a name="test-the-application-locally"></a>Testování aplikace místně

Spustit migrace databáze Laravel s _. env.production_ jako soubor prostředí k vytvoření tabulky v databázi MySQL v Azure Database pro databázi MySQL (Preview). Nezapomeňte, že _. env.production_ obsahuje informace o připojení k vaší databázi MySQL v Azure.

```bash
php artisan migrate --env=production --force
```

_. env.production_ ještě nemá platnou aplikaci klíč. Vygenerujte nový token pro něj v terminálu.

```bash
php artisan key:generate --env=production --force
```

Spuštění ukázkové aplikace s _. env.production_ jako soubor prostředí.

```bash
php artisan serve --env=production
```

Přejděte na `http://localhost:8000`. Pokud stránka načte bez chyb, aplikace PHP se připojuje k databázi MySQL v Azure.

Na stránce přidáte několik úloh.

![PHP připojí úspěšně do Azure Database pro databázi MySQL (Preview)](./media/tutorial-php-mysql-app/mysql-connect-success.png)

Chcete-li zastavit PHP, zadejte `Ctrl + C` v terminálu.

### <a name="commit-your-changes"></a>Potvrdit změny

Spusťte následující příkazy Git potvrzení změny:

```bash
git add .
git commit -m "database.php updates"
```

Je připravená k nasazení aplikace.

## <a name="deploy-to-azure"></a>Nasazení do Azure

V tomto kroku nasadíte aplikaci PHP MySQL připojení do služby Azure App Service.

### <a name="configure-a-deployment-user"></a>Konfigurace uživatele nasazení

[!INCLUDE [Configure deployment user](../../../includes/configure-deployment-user-no-h.md)]

### <a name="create-an-app-service-plan"></a>Vytvoření plánu služby App Service

[!INCLUDE [Create app service plan no h](../../../includes/app-service-web-create-app-service-plan-linux-no-h.md)]

### <a name="create-a-web-app"></a>Vytvoření webové aplikace

[!INCLUDE [Create web app](../../../includes/app-service-web-create-web-app-php-no-h.md)] 

### <a name="configure-database-settings"></a>Konfigurace nastavení databáze

Ve službě App Service, můžete nastavit proměnné prostředí jako _nastavení aplikace_ pomocí [az webapp konfigurace appsettings sadu](/cli/azure/webapp/config/appsettings#set) příkaz.

Následující příkaz nakonfiguruje nastavení aplikace `DB_HOST`, `DB_DATABASE`, `DB_USERNAME`, a `DB_PASSWORD`. Nahraďte zástupné symboly  _&lt;appname >_ a  _&lt;mysql_server_name >_.

```azurecli-interactive
az webapp config appsettings set --name <app_name> --resource-group myResourceGroup --settings DB_HOST="<mysql_server_name>.database.windows.net" DB_DATABASE="sampledb" DB_USERNAME="phpappuser@<mysql_server_name>" DB_PASSWORD="MySQLAzure2017"
```
 <!-- MYSQL_SSL="true" -->

Můžete použít PHP [GETENV –](http://www.php.net/manual/function.getenv.php) metoda pro přístup k nastavení. Laravel kód používá [env](https://laravel.com/docs/5.4/helpers#method-env) obálku přes PHP `getenv`. Například MySQL konfigurace v _config/database.php_ vypadá jako následující kód:

```php
'mysql' => [
    'driver'    => 'mysql',
    'host'      => env('DB_HOST', 'localhost'),
    'database'  => env('DB_DATABASE', 'forge'),
    'username'  => env('DB_USERNAME', 'forge'),
    'password'  => env('DB_PASSWORD', ''),
    ...
],
```

### <a name="configure-laravel-environment-variables"></a>Konfigurace Laravel proměnné prostředí

Laravel musí klíčem aplikace ve službě App Service. Můžete ho nakonfigurovat pomocí nastavení aplikace.

Použití `php artisan` vygenerovat nový klíč aplikace bez uložení do _.env_.

```bash
php artisan key:generate --show
```

Nastavit klíč aplikace ve službě App Service webové aplikace pomocí [az webapp konfigurace appsettings sadu](/cli/azure/webapp/config/appsettings#set) příkaz. Nahraďte zástupné symboly  _&lt;appname >_ a  _&lt;outputofphpartisankey: generování >_.

```azurecli-interactive
az webapp config appsettings set --name <app_name> --resource-group myResourceGroup --settings APP_KEY="<output_of_php_artisan_key:generate>" APP_DEBUG="true"
```

`APP_DEBUG="true"`informuje Laravel vrátí informace o ladění v případě chyby zaznamená nasazené webové aplikace. Když spustíte produkční aplikace, nastavte ji na `false`, což je bezpečnější.

### <a name="set-the-virtual-application-path"></a>Nastavte cestu virtuální aplikace.

Nastavte cestu virtuální aplikace pro webovou aplikaci. Tento krok je nezbytný, protože [životního cyklu aplikace Laravel](https://laravel.com/docs/5.4/lifecycle) začíná v _veřejné_ adresář místo kořenový adresář aplikace. Ostatní platformy PHP, jejichž životního cyklu spuštění v kořenovém adresáři můžete pracovat bez ruční konfigurace cesta virtuální aplikace.

Nastavit cestu virtuální aplikace pomocí [aktualizace prostředků az](/cli/azure/resource#update) příkaz. Nahraďte  _&lt;appname >_ zástupný symbol.

```azurecli-interactive
az resource update --name web --resource-group myResourceGroup --namespace Microsoft.Web --resource-type config --parent sites/<app_name> --set properties.virtualApplications[0].physicalPath="site\wwwroot\public" --api-version 2015-06-01
```

Ve výchozím nastavení, Azure App Service body kořenovou cestu virtuální aplikace (_/_) do kořenového adresáře souborů nasazené aplikace (_sites\wwwroot_).

### <a name="push-to-azure-from-git"></a>Přenos z Gitu do Azure

Přidejte vzdálené úložiště Azure do místního úložiště Gitu.

```bash
git remote add azure <paste_copied_url_here>
```

Doručte do Azure vzdálené nasazení aplikace PHP. Zobrazí se výzva k zadání hesla, který jste zadali dříve v rámci vytváření nasazení uživatele.

```bash
git push azure master
```

Během nasazení Azure App Service komunikuje s Gitem jejím průběhu.

```bash
Counting objects: 3, done.
Delta compression using up to 8 threads.
Compressing objects: 100% (3/3), done.
Writing objects: 100% (3/3), 291 bytes | 0 bytes/s, done.
Total 3 (delta 2), reused 0 (delta 0)
remote: Updating branch 'master'.
remote: Updating submodules.
remote: Preparing deployment for commit id 'a5e076db9c'.
remote: Running custom deployment command...
remote: Running deployment command...
...
< Output has been truncated for readability >
```

> [!NOTE]
> Můžete si všimnout, že proces nasazení nainstaluje [autora](https://getcomposer.org/) balíčky na konci. Služby App Service tyto automatizaci během nasazení výchozí nespustí, takže toto úložiště ukázkové má tři další soubory v jeho kořenový adresář povolit:
>
> - `.deployment`– Tento soubor informuje služby App Service ke spuštění `bash deploy.sh` jako vlastní nasazení skriptu.
> - `deploy.sh`-Vlastní skript nasazení. Při kontrole souboru je se zobrazí, že běží `php composer.phar install` po `npm install`.
> - `composer.phar`-Autora Správce balíčků.
>
> Tento postup můžete použít k přidání jakéhokoli kroku k nasazení na základě Git do služby App Service. Další informace najdete v tématu [vlastní skript nasazení](https://github.com/projectkudu/kudu/wiki/Custom-Deployment-Script).
>

### <a name="browse-to-the-azure-web-app"></a>Přejděte do webové aplikace Azure

Přejděte do `http://<app_name>.azurewebsites.net` a přidejte do seznamu několik úloh.

![Aplikace PHP, které jsou spuštěné v Azure App Service](./media/tutorial-php-mysql-app/php-mysql-in-azure.png)

Blahopřejeme, používáte PHP aplikace na základě dat ve službě Azure App Service.

## <a name="update-model-locally-and-redeploy"></a>Aktualizace modelu místně a znovu nasaďte

V tomto kroku provedete jednoduché změnu `task` datový model a webové aplikace a pak publikujte aktualizace do Azure.

Pro tento scénář úlohy upravit aplikaci tak, že můžete označit úlohu jako dokončenou.

### <a name="add-a-column"></a>Přidá sloupec

V terminálu přejděte do kořenového úložiště Git.

Generovat nové migrace databáze pro `tasks` tabulky:

```bash
php artisan make:migration add_complete_column --table=tasks
```

Tento příkaz zobrazí název souboru migrace, aby se vygenerovala. Tento soubor v _databáze nebo migrace_ a otevřete ji.

Nahraďte `up` metoda následujícím kódem:

```php
public function up()
{
    Schema::table('tasks', function (Blueprint $table) {
        $table->boolean('complete')->default(False);
    });
}
```

Předchozí kód přidá boolean sloupec `tasks` tabulka s názvem `complete`.

Nahraďte `down` metodu akce vrácení zpět s následujícím kódem:

```php
public function down()
{
    Schema::table('tasks', function (Blueprint $table) {
        $table->dropColumn('complete');
    });
}
```

V terminálu spuštění migrace databáze Laravel udělat změnu v místní databázi.

```bash
php artisan migrate
```

Na základě [Laravel názvů](https://laravel.com/docs/5.4/eloquent#defining-models), modelu `Task` (najdete v části _app/Task.php_) se mapuje `tasks` tabulky ve výchozím nastavení.

### <a name="update-application-logic"></a>Aktualizace aplikace logiky

Otevřete *routes/web.php* souboru. Aplikace definuje jeho trasy a obchodní logiku sem.

Na konci souboru přidejte trasu následujícím kódem:

```php
/**
 * Toggle Task completeness
 */
Route::post('/task/{id}', function ($id) {
    error_log('INFO: post /task/'.$id);
    $task = Task::findOrFail($id);

    $task->complete = !$task->complete;
    $task->save();

    return redirect('/');
});
```

Předchozí kód je jednoduchý aktualizace do datového modelu přepnutím hodnotu `complete`.

### <a name="update-the-view"></a>Aktualizace zobrazení

Otevřete *resources/views/tasks.blade.php* souboru. Najít `<tr>` počáteční značce a nahraďte ho:

```html
<tr class="{{ $task->complete ? 'success' : 'active' }}" >
```

Předchozí kód změní barvu řádek v závislosti na tom, zda je úloha dokončena.

V dalším řádku máte následující kód:

```html
<td class="table-text"><div>{{ $task->name }}</div></td>
```

Celý řádek nahraďte následujícím kódem:

```html
<td>
    <form action="{{ url('task/'.$task->id) }}" method="POST">
        {{ csrf_field() }}

        <button type="submit" class="btn btn-xs">
            <i class="fa {{$task->complete ? 'fa-check-square-o' : 'fa-square-o'}}"></i>
        </button>
        {{ $task->name }}
    </form>
</td>
```

Předchozí kód přidá tlačítko pro odeslání, který odkazuje na trasy, která jste definovali dříve.

### <a name="test-the-changes-locally"></a>Testování, místně

V kořenovém adresáři úložiště Git spusťte vývojový server.

```bash
php artisan serve
```

Chcete-li zobrazit stav úlohy změnit, přejděte na `http://localhost:8000` a zaškrtněte políčko.

![Přidání zaškrtnutím políčka úloh](./media/tutorial-php-mysql-app/complete-checkbox.png)

Chcete-li zastavit PHP, zadejte `Ctrl + C` v terminálu.

### <a name="publish-changes-to-azure"></a>Publikování změn do Azure

V terminálu spusťte migrace databáze Laravel s provozním připojovací řetězec k provedení změn v databázi Azure.

```bash
php artisan migrate --env=production --force
```

Potvrďte všechny změny v úložišti Git a potom odešlete změny kódu do Azure.

```bash
git add .
git commit -m "added complete checkbox"
git push azure master
```

Jednou `git push` je dokončení, přejděte na webové aplikace Azure a testování nových funkcí.

![Model a databáze změny, které jsou publikovány do služby Azure](media/tutorial-php-mysql-app/complete-checkbox-published.png)

Pokud jste přidali všechny úlohy, se uchovávají v databázi. Aktualizace schématu data ponechat stávající data beze změn.

## <a name="manage-the-azure-web-app"></a>Správa webové aplikace Azure

Pokud chcete spravovat webovou aplikaci, kterou jste vytvořili, přejděte na web [Azure Portal](https://portal.azure.com).

V levé nabídce klikněte na **App Services** a pak klikněte na název vaší webové aplikace Azure.

![Navigace portálem k webové aplikaci Azure](./media/tutorial-php-mysql-app/access-portal.png)

Zobrazí se stránka s přehledem vaší webové aplikace. Zde můžete provádět základní správu úkoly, jako je zastavení, spuštění, restartování, procházet a delete.

V levé nabídce poskytuje stránky pro konfiguraci vaší aplikace.

![Stránka služby App Service na webu Azure Portal](./media/tutorial-php-mysql-app/web-app-blade.png)

[!INCLUDE [cli-samples-clean-up](../../../includes/cli-samples-clean-up.md)]

<a name="next"></a>

## <a name="next-steps"></a>Další kroky

V tomto kurzu jste se naučili:

> [!div class="checklist"]
> * Vytvoření databáze MySQL v Azure
> * Připojení aplikace PHP k MySQL
> * Nasazení aplikace do Azure
> * Aktualizovat datový model a aplikaci znovu nasaďte
> * Diagnostické protokoly datového proudu z Azure
> * Spravovat aplikaci na portálu Azure

Přechodu na dalším kurzu se dozvíte, jak namapovat vlastní název DNS pro webovou aplikaci.

> [!div class="nextstepaction"]
> [Mapování existujícího vlastního názvu DNS na Azure Web Apps](../app-service-web-tutorial-custom-domain.md)
