---
title: "Vytvoření webové aplikace Ruby a MySQL v Azure App Service v systému Linux | Microsoft Docs"
description: "Další informace o získání Ruby aplikace v Azure, funguje připojení k databázi MySQL v Azure."
services: app-service\web
documentationcenter: nodejs
author: cephalin
manager: cfowler
ms.service: app-service-web
ms.workload: web
ms.devlang: nodejs
ms.topic: tutorial
ms.date: 12/21/2017
ms.author: cephalin
ms.custom: mvc
ms.openlocfilehash: 03b2b4e8f8827a08e1414512d848bd5bed48d674
ms.sourcegitcommit: df4ddc55b42b593f165d56531f591fdb1e689686
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/04/2018
---
# <a name="build-a-ruby-and-mysql-web-app-in-azure-app-service-on-linux"></a>Vytvoření webové aplikace Ruby a MySQL v Azure App Service v systému Linux

[App Service v Linuxu](app-service-linux-intro.md) je vysoce škálovatelná služba s automatickými opravami pro hostování webů s využitím operačního systému Linux. Tento kurz ukazuje, jak vytvořit Ruby webovou aplikaci a připojte ho k databázi MySQL. Jakmile budete hotovi, budete mít [Ruby, na které](http://rubyonrails.org/) aplikace běžící na App Service v systému Linux.

![Ruby, na které aplikaci spuštěnou ve službě Azure App Service](./media/tutorial-ruby-mysql-app/complete-checkbox-published.png)

V tomto kurzu se naučíte:

> [!div class="checklist"]
> * Vytvoření databáze MySQL v Azure
> * Připojení k MySQL Ruby, na které aplikace
> * Nasazení aplikace do Azure
> * Aktualizovat datový model a aplikaci znovu nasaďte
> * Diagnostické protokoly datového proudu z Azure
> * Spravovat aplikaci na portálu Azure

## <a name="prerequisites"></a>Požadavky

Pro absolvování tohoto kurzu potřebujete:

* [Nainstalovat Git](https://git-scm.com/).
* [Nainstalujte Ruby 2.3](https://www.ruby-lang.org/documentation/installation/)
* [Nainstalujte Ruby, na které 5.1](http://guides.rubyonrails.org/v5.1/getting_started.html)
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

Ukončení připojení k serveru zadáním `quit`.

```sql
quit
```

<a name="step2"></a>

## <a name="create-a-ruby-on-rails-app-locally"></a>Vytvoření Ruby ve které aplikaci místně
V tomto kroku získat Ruby, na které ukázkovou aplikaci, konfigurovat jeho připojení k databázi a spustit místně. 

### <a name="clone-the-sample"></a>Clone – ukázka

V okně terminálu `cd` do pracovního adresáře.

Ukázkové úložiště naklonujete spuštěním následujícího příkazu.

```bash
git clone https://github.com/Azure-Samples/rubyrails-tasks.git
```

`cd`do vašeho klonovaný adresáře. Nainstalujte požadované balíčky.

```bash
cd rubyrails-tasks
bundle install --path vendor/bundle
```

### <a name="configure-mysql-connection"></a>Konfigurace připojení databáze MySQL

V úložišti, otevřete _config/database.yml_ a zadat místní MySQL kořenové uživatelské jméno a heslo (řádku 13). Pokud jste nainstalovali MySQL pomocí nástroje, například [Homebrew](https://brew.sh/), pak přihlašovací údaje v souboru jsou již nastaven na výchozí hodnoty (což je `root` a prázdné heslo).

```txt
default: &default
  adapter: mysql2
  encoding: utf8
  pool: <%= ENV.fetch("RAILS_MAX_THREADS") { 5 } %>
  username: root
  password:
  socket: /tmp/mysql.sock
```

### <a name="run-the-sample-locally"></a>Spustit ukázku místně

Spustit [migrace které](http://guides.rubyonrails.org/active_record_migrations.html#running-migrations) k vytvoření tabulek aplikace potřebuje. Pokud chcete zjistit, které tabulky jsou vytvořené v byla migrace, vyhledejte v _db nebo migrovat_ adresáře v úložišti Git.

```bash
rake db:create
rake db:migrate
```

Spusťte aplikaci.

```bash
rails server
```

V prohlížeči přejděte na `http://localhost:3000`. Na stránce přidáte několik úloh.

![Ruby, na které úspěšně připojí k MySQL](./media/tutorial-ruby-mysql-app/mysql-connect-success.png)

Chcete-li zastavit, na které server, zadejte `Ctrl + C` v terminálu.

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

## <a name="create-mysql-in-azure"></a>Vytvoření databáze MySQL v Azure

V tomto kroku vytvoříte databázi MySQL v [Azure Database pro databázi MySQL (Preview)](/azure/mysql). Později nakonfigurujte její na které aplikace pro připojení k této databázi.

### <a name="create-a-resource-group"></a>Vytvoření skupiny prostředků

[!INCLUDE [Create resource group](../../../includes/app-service-web-create-resource-group-no-h.md)] 

### <a name="create-a-mysql-server"></a>Vytvoření databáze MySQL serveru

Vytvoření serveru ve službě Azure Database pro databázi MySQL (Preview) pomocí [az mysql server vytvořit](/cli/azure/mysql/server?view=azure-cli-latest#az_mysql_server_create) příkaz.

V následujícím příkazu nahraďte název serveru MySQL, kde uvidíte  _&lt;mysql_server_name >_ zástupný symbol (platnými znaky jsou `a-z`, `0-9`, a `-`). Tento název je součástí názvu hostitele serveru MySQL (`<mysql_server_name>.mysql.database.azure.com`), musí být globálně jedinečný.

```azurecli-interactive
az mysql server create --name <mysql_server_name> --resource-group myResourceGroup --location "North Europe" --admin-user adminuser --admin-password My5up3r$tr0ngPa$w0rd!
```

Při vytvoření serveru MySQL rozhraní příkazového řádku Azure obsahuje informace o podobně jako v následujícím příkladu:

```json
{
  "administratorLogin": "adminuser",
  "administratorLoginPassword": null,
  "fullyQualifiedDomainName": "<mysql_server_name>.database.mysql.database.azure.com",
  "id": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/myResourceGroup/providers/Microsoft.DBforMySQL/servers/<mysql_server_name>",
  "location": "northeurope",
  "name": "<mysql_server_name>",
  "resourceGroup": "myResourceGroup",
  ...
}
```

### <a name="configure-server-firewall"></a>Konfigurace brány firewall serveru

Vytvořte pravidlo brány firewall pro váš server MySQL a povolíte připojení klienta pomocí [az mysql pravidla brány firewall-vytvořit](/cli/azure/mysql/server/firewall-rule?view=azure-cli-latest#az_mysql_server_firewall_rule_create) příkaz.

```azurecli-interactive
az mysql server firewall-rule create --name allIPs --server <mysql_server_name> --resource-group myResourceGroup --start-ip-address 0.0.0.0 --end-ip-address 255.255.255.255
```

> [!NOTE]
> Azure databáze pro databázi MySQL (Preview) nepodporuje aktuálně omezení počtu připojení pouze ke službám Azure. Jak budou dynamicky přiřazovat IP adresy v Azure, je lepší povolit všechny IP adresy. Služba není ve verzi preview. Plánujeme lepší metody pro zabezpečení databáze.
>

### <a name="connect-to-production-mysql-server-locally"></a>Připojení k serveru pro produkční MySQL místně

V okně terminálu připojte k serveru databáze MySQL v Azure. Použít hodnotu zadanou dříve pro  _&lt;mysql_server_name >_.

```bash
mysql -u adminuser@<mysql_server_name> -h <mysql_server_name>.mysql.database.azure.com -P 3306 -p
```

Pokud budete vyzváni k zadání hesla, použijte _My5up3r$ tr0ngPa$ w0rd!_, který jste zadali při vytváření databázového serveru.

### <a name="create-a-production-database"></a>Vytvoření provozní databáze

Na `mysql` výzvu, vytvořit databázi.

```sql
CREATE DATABASE sampledb;
```

### <a name="create-a-user-with-permissions"></a>Vytvořit uživatele s oprávněními

Vytvořte uživatele databáze názvem _railsappuser_ a pojmenujte ho všechna oprávnění `sampledb` databáze.

```sql
CREATE USER 'railsappuser' IDENTIFIED BY 'MySQLAzure2017'; 
GRANT ALL PRIVILEGES ON sampledb.* TO 'railsappuser';
```

Ukončení připojení k serveru zadáním `quit`.

```sql
quit
```

## <a name="connect-app-to-azure-mysql"></a>Připojení aplikace k Azure MySQL

V tomto kroku připojíte Ruby, na které aplikace pro databázi MySQL, kterou jste vytvořili v Azure Database pro databázi MySQL (Preview).

<a name="devconfig"></a>

### <a name="configure-the-database-connection"></a>Konfigurace připojení k databázi

V úložišti, otevřete _config/database.yml_. V dolní části souboru nahraďte následujícím kódem produkční proměnné. Pro  _&lt;mysql_server_name >_ zástupného symbolu, použijte název serveru databáze MySQL, který jste vytvořili.

```txt
production:
  <<: *default
  host: <%= ENV['DB_HOST'] %>
  port: 3306
  database: <%= ENV['DB_DATABASE'] %>
  username: <%= ENV['DB_USERNAME'] %>
  password: <%= ENV['DB_PASSWORD'] %>
  sslca: ssl/BaltimoreCyberTrustRoot.crt.pem
```

Uložte změny.

> [!NOTE]
> `sslca` Přidána a ukazuje na stávající _.pem_ souboru v úložišti ukázka. Ve výchozím nastavení vynucuje Azure Database pro databázi MySQL připojení SSL od klientů. To `.pem` certifikát je, jak provedete připojení SSL k databázi Azure pro databázi MySQL. Další informace najdete v tématu [Konfigurace připojení SSL v aplikaci pro zabezpečené připojení k Azure Database for MySQL](../../mysql/howto-configure-ssl.md).
>

### <a name="test-the-application-locally"></a>Testování aplikace místně

V místní terminálu nastavte následující proměnné prostředí:

```bash
export DB_HOST=<mysql_server_name>.mysql.database.azure.com
export DB_DATABASE=sampledb 
export DB_USERNAME=railsappuser@<mysql_server_name>
export DB_PASSWORD=MySQLAzure2017
```

Spusťte migrace databáze které s provozním hodnoty, kterou jste právě nakonfigurovali k vytvoření tabulky v databázi MySQL v Azure Database pro databázi MySQL (Preview). 

```bash
rake db:migrate RAILS_ENV=production
```

Při spuštění v produkčním prostředí, které aplikace musí předkompilovaných prostředky. Generovat požadované prostředky pomocí následujícího příkazu:

```bash
rake assets:precompile
```

Tajné klíče v provozním prostředí které také používá ke správě zabezpečení. Generovat tajný klíč.

```bash
rails secret
```

Uložte tajný klíč do odpovídajících proměnné používané které produkčního prostředí. Pro usnadnění práce používáte stejný klíč pro obě proměnné.

```bash
export RAILS_MASTER_KEY=<output_of_rails_secret>
export SECRET_KEY_BASE=<output_of_rails_secret>
```

Povolte které produkčního prostředí k obsluze souborů JavaScript a CSS.

```bash
export RAILS_SERVE_STATIC_FILES=true
```

Spuštění ukázkové aplikace v provozním prostředí.

```bash
rails server -e production
```

Přejděte na `http://localhost:3000`. Pokud stránka načte bez chyb, Ruby, na které aplikace se připojuje k databázi MySQL v Azure.

Na stránce přidáte několik úloh.

![Ruby, na které se připojí úspěšně do Azure Database pro databázi MySQL (Preview)](./media/tutorial-ruby-mysql-app/azure-mysql-connect-success.png)

Chcete-li zastavit, na které server, zadejte `Ctrl + C` v terminálu.

### <a name="commit-your-changes"></a>Potvrdit změny

Spusťte následující příkazy Git potvrzení změny:

```bash
git add .
git commit -m "database.yml updates"
```

Je připravená k nasazení aplikace.

## <a name="deploy-to-azure"></a>Nasazení do Azure

V tomto kroku nasadíte aplikaci které MySQL připojení do služby Azure App Service.

### <a name="configure-a-deployment-user"></a>Konfigurace uživatele nasazení

[!INCLUDE [Configure deployment user](../../../includes/configure-deployment-user-no-h.md)]

### <a name="create-an-app-service-plan"></a>Vytvoření plánu služby App Service

[!INCLUDE [Create app service plan no h](../../../includes/app-service-web-create-app-service-plan-linux-no-h.md)]

### <a name="create-a-web-app"></a>Vytvoření webové aplikace

Ve službě Cloud Shell pomocí příkazu [az webapp create](/cli/azure/webapp?view=azure-cli-latest#az_webapp_create) vytvořte webovou aplikaci v plánu služby App Service `myAppServicePlan`. 

V následujícím příkladu nahraďte `<app_name>` globálně jedinečným názvem aplikace (platné znaky jsou `a-z`, `0-9` a `-`). Modul runtime je nastaven na `RUBY|2.3`, která nasadí [výchozí Ruby image](https://hub.docker.com/r/appsvc/ruby/). Pokud chcete zobrazit všechny podporované moduly runtime, spusťte příkaz [az webapp list-runtimes](/cli/azure/webapp?view=azure-cli-latest#az_webapp_list_runtimes). 

```azurecli-interactive
az webapp create --resource-group myResourceGroup --plan myAppServicePlan --name <app_name> --runtime "RUBY|2.3" --deployment-local-git
```

Po vytvoření webové aplikace Azure CLI zobrazí výstup podobný následujícímu příkladu:

```json
Local git is configured with url of 'https://<username>@<app_name>.scm.azurewebsites.net/<app_name>.git'
{
  "availabilityState": "Normal",
  "clientAffinityEnabled": true,
  "clientCertEnabled": false,
  "cloningInfo": null,
  "containerSize": 0,
  "dailyMemoryTimeQuota": 0,
  "defaultHostName": "<app_name>.azurewebsites.net",
  "deploymentLocalGitUrl": "https://<username>@<app_name>.scm.azurewebsites.net/<app_name>.git",
  "enabled": true,
  < JSON data removed for brevity. >
}
```

Vytvořili jste novou prázdnou webovou aplikaci s povoleným nasazením Gitu.

> [!NOTE]
> Adresa URL vzdáleného úložiště Git se zobrazuje ve vlastnosti `deploymentLocalGitUrl` ve formátu `https://<username>@<app_name>.scm.azurewebsites.net/<app_name>.git`. Tuto adresu URL si uložte, protože ji budete potřebovat později.
>

### <a name="configure-database-settings"></a>Konfiguruje nastavení databáze.

Ve službě App Service, můžete nastavit proměnné prostředí jako _nastavení aplikace_ pomocí [az webapp konfigurace appsettings sadu](/cli/azure/webapp/config/appsettings?view=azure-cli-latest#az_webapp_config_appsettings_set) příkazu v prostředí cloudu.

Následující příkaz prostředí cloudu nakonfiguruje nastavení aplikace `DB_HOST`, `DB_DATABASE`, `DB_USERNAME`, a `DB_PASSWORD`. Nahraďte zástupné symboly  _&lt;appname >_ a  _&lt;mysql_server_name >_.

```azurecli-interactive
az webapp config appsettings set --name <app_name> --resource-group myResourceGroup --settings DB_HOST="<mysql_server_name>.mysql.database.azure.com" DB_DATABASE="sampledb" DB_USERNAME="railsappuser@<mysql_server_name>" DB_PASSWORD="MySQLAzure2017"
```

### <a name="configure-rails-environment-variables"></a>Nakonfigurovat proměnné prostředí, které

V místní terminálu vygenerujte nový tajný klíč pro produkční prostředí které v Azure.

```bash
rails secret
```

Nakonfigurujte proměnné požadované pro které produkční prostředí.

V následujícím příkazu cloudové prostředí, nahraďte dva  _&lt;output_of_rails_secret >_ zástupné symboly nový tajný klíč vygenerované v místní terminálu.

```azurecli-interactive
az webapp config appsettings set --name <app_name> --resource-group myResourceGroup --settings RAILS_MASTER_KEY="<output_of_rails_secret>" SECRET_KEY_BASE="<output_of_rails_secret>" RAILS_SERVE_STATIC_FILES="true" ASSETS_PRECOMPILE="true"
```

`ASSETS_PRECOMPILE="true"`Určuje výchozí kontejner Ruby předkompilovat prostředky v každé nasazení Git.

### <a name="push-to-azure-from-git"></a>Přenos z Gitu do Azure

V místní terminálu přidejte do místního úložiště Git Azure vzdálené.

```bash
git remote add azure <paste_copied_url_here>
```

Doručte do Azure vzdálené nasazení Ruby, na které aplikace. Zobrazí se výzva k zadání hesla, který jste zadali dříve v rámci vytváření nasazení uživatele.

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

### <a name="browse-to-the-azure-web-app"></a>Přejděte do webové aplikace Azure

Přejděte do `http://<app_name>.azurewebsites.net` a přidejte do seznamu několik úloh.

![Ruby, na které aplikaci spuštěnou ve službě Azure App Service](./media/tutorial-ruby-mysql-app/ruby-mysql-in-azure.png)

Blahopřejeme, ve které aplikaci ve službě Azure App Service používáte Ruby se řízené daty.

## <a name="update-model-locally-and-redeploy"></a>Aktualizace modelu místně a znovu nasaďte

V tomto kroku provedete jednoduché změnu `task` datový model a webové aplikace a pak publikujte aktualizace do Azure.

Pro tento scénář úlohy upravit aplikaci tak, že můžete označit úlohu jako dokončenou.

### <a name="add-a-column"></a>Přidat sloupec

V terminálu přejděte do kořenového úložiště Git.

Generovat nové migrace, který přidává boolean sloupec s názvem `Done` k `Tasks` tabulky:

```bash
rails generate migration add_Done_to_Tasks Done:boolean
```

Tento příkaz generuje nový soubor migrace v _db nebo migrovat_ adresáře.


V terminálu spuštění migrace databáze které udělat změnu v místní databázi.

```bash
rake db:migrate
```

### <a name="update-application-logic"></a>Aktualizace aplikace logiky

Otevřete *app/controllers/tasks_controller.rb* souboru. Na konci souboru vyhledejte následující řádek:

```rb
params.require(:task).permit(:Description)
```

Upravit tento řádek, aby zahrnovalo nové `Done` parametr.

```rb
params.require(:task).permit(:Description, :Done)
```

### <a name="update-the-views"></a>Aktualizace zobrazení

Otevřete *app/views/tasks/_form.html.erb* souboru, který je upravit formuláře.

Vyhledejte řádek `<%=f.error_span(:Description) %>` a vložte následující kód pod ním:

```erb
<%= f.label :Done, :class => 'control-label col-lg-2' %>
<div class="col-lg-10">
  <%= f.check_box :Done, :class => 'form-control' %>
</div>
```

Otevřete *app/views/tasks/show.html.erb* souboru, který je stránka zobrazení jedním záznamem. 

Vyhledejte řádek `<dd><%= @task.Description %></dd>` a vložte následující kód pod ním:

```erb
  <dt><strong><%= model_class.human_attribute_name(:Done) %>:</strong></dt>
  <dd><%= check_box "task", "Done", {:checked => @task.Done, :disabled => true}%></dd>
```

Otevřete *app/views/tasks/index.html.erb* souboru, který je indexovou stránku pro všechny záznamy.

Vyhledejte řádek `<th><%= model_class.human_attribute_name(:Description) %></th>` a vložte následující kód pod ním:

```erb
<th><%= model_class.human_attribute_name(:Done) %></th>
```

Ve stejném souboru, vyhledejte řádek `<td><%= task.Description %></td>` a vložte následující kód pod ním:

```erb
<td><%= check_box "task", "Done", {:checked => task.Done, :disabled => true} %></td>
```

### <a name="test-the-changes-locally"></a>Testování, místně

V místní terminálu, které serverem.

```bash
rails server
```

Chcete-li zobrazit stav úlohy změnit, přejděte na `http://localhost:3000` a přidat nebo upravit položky.

![Přidání zaškrtnutím políčka úloh](./media/tutorial-ruby-mysql-app/complete-checkbox.png)

Chcete-li zastavit, na které server, zadejte `Ctrl + C` v terminálu.

### <a name="publish-changes-to-azure"></a>Publikování změn do Azure

V terminálu spusťte které migrace databáze pro produkční prostředí k provedení změn v databázi Azure.

```bash
rake db:migrate RAILS_ENV=production
```

Potvrďte všechny změny v úložišti Git a potom odešlete změny kódu do Azure.

```bash
git add .
git commit -m "added complete checkbox"
git push azure master
```

Jednou `git push` je dokončení, přejděte na webové aplikace Azure a testování nových funkcí.

![Model a databáze změny, které jsou publikovány do služby Azure](media/tutorial-ruby-mysql-app/complete-checkbox-published.png)

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

## <a name="next-steps"></a>Další postup

V tomto kurzu jste se naučili:

> [!div class="checklist"]
> * Vytvoření databáze MySQL v Azure
> * Připojení k MySQL Ruby, na které aplikace
> * Nasazení aplikace do Azure
> * Aktualizovat datový model a aplikaci znovu nasaďte
> * Diagnostické protokoly datového proudu z Azure
> * Spravovat aplikaci na portálu Azure

Přechodu na dalším kurzu se dozvíte, jak namapovat vlastní název DNS pro webovou aplikaci.

> [!div class="nextstepaction"]
> [Mapování existujícího vlastního názvu DNS na Azure Web Apps](../app-service-web-tutorial-custom-domain.md)
