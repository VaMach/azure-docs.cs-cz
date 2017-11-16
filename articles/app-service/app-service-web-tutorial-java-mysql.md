---
title: "Vytvořit webovou aplikaci Java a MySQL v Azure"
description: "Zjistěte, jak získat aplikaci Java, která se připojuje ke službě databáze Azure MySQL práce v Azure App Service."
services: app-service\web
documentationcenter: Java
author: bbenz
manager: jeffsand
editor: jasonwhowell
ms.assetid: 
ms.service: app-service-web
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: java
ms.topic: tutorial
ms.date: 05/22/2017
ms.author: bbenz
ms.custom: mvc
ms.openlocfilehash: 58e0533db1bb907b0c9a09cdeb7aabbf120d506f
ms.sourcegitcommit: 3ee36b8a4115fce8b79dd912486adb7610866a7c
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/15/2017
---
# <a name="build-a-java-and-mysql-web-app-in-azure"></a>Vytvořit webovou aplikaci Java a MySQL v Azure

Tento kurz ukazuje, jak vytvořit webovou aplikaci Java v Azure a připojte ho k databázi MySQL. Jakmile budete hotovi, budete mít [pružiny spouštěcí](https://projects.spring.io/spring-boot/) ukládání dat v aplikaci [Azure Database pro databázi MySQL](https://docs.microsoft.com/azure/mysql/overview) systémem [Azure App Service Web Apps](app-service-web-overview.md).

![Java aplikace spuštěné v Azure App Service](./media/app-service-web-tutorial-java-mysql/appservice-web-app.png)

V tomto kurzu se naučíte:

> [!div class="checklist"]
> * Vytvoření databáze MySQL v Azure
> * Ukázkovou aplikaci připojit k databázi
> * Nasazení aplikace do Azure
> * Aktualizace a opětovné nasazení aplikace
> * Diagnostické protokoly datového proudu z Azure
> * Sledování aplikace na portálu Azure


## <a name="prerequisites"></a>Požadavky

1. [Stáhněte a nainstalujte Git](https://git-scm.com/)
1. [Stáhněte a nainstalujte sadu JDK Java 7 nebo novější](http://www.oracle.com/technetwork/java/javase/downloads/index.html)
1. [Stáhnout, nainstalovat a spustit MySQL](https://dev.mysql.com/doc/refman/5.7/en/installing.html) 

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

## <a name="prepare-local-mysql"></a>Příprava místního MySQL 

V tomto kroku vytvoříte databázi v místním serveru MySQL pro použití při testování aplikace místně na vašem počítači.

### <a name="connect-to-mysql-server"></a>Připojení k serveru databáze MySQL

Okno terminálu připojte k místní server MySQL. Chcete-li spustit všechny příkazy v tomto kurzu můžete toto okno terminálu.

```bash
mysql -u root -p
```

Pokud se zobrazí výzva k zadání hesla, zadejte heslo pro `root` účtu. Pokud si nepamatujete heslo kořenového účtu, najdete v části [MySQL: jak resetovat hesla kořenového](https://dev.mysql.com/doc/refman/5.7/en/resetting-permissions.html).

Pokud váš příkaz úspěšně proběhne, serveru databáze MySQL již spuštěna. Pokud ne, ujistěte se, zda je místní server MySQL spuštěná pomocí následujících [kroky po instalaci MySQL](https://dev.mysql.com/doc/refman/5.7/en/postinstallation.html).

### <a name="create-a-database"></a>Vytvoření databáze 

V `mysql` výzvu, vytvořte databázi a tabulku položkami seznamu úkolů.

```sql
CREATE DATABASE tododb;
```

Ukončení připojení k serveru zadáním `quit`.

```sql
quit
```

## <a name="create-and-run-the-sample-app"></a>Vytvoření a spuštění ukázkové aplikace 

V tomto kroku klonovat ukázkové pružiny spouštěcí aplikace, bude sloužit místní databázi MySQL a spustit ve vašem počítači. 

### <a name="clone-the-sample"></a>Clone – ukázka

V okně terminálu přejděte do pracovního adresáře a klonovat úložiště v ukázkové. 

```bash
git clone https://github.com/azure-samples/mysql-spring-boot-todo
```

### <a name="configure-the-app-to-use-the-mysql-database"></a>Nakonfiguruje aplikaci, kterou chcete použít databázi MySQL

Aktualizace `spring.datasource.password` a hodnota v *spring-boot-mysql-todo/src/main/resources/application.properties* pomocí stejného hesla kořenové použitý k otevření MySQL řádku:

```
spring.datasource.password=mysqlpass
```

### <a name="build-and-run-the-sample"></a>Sestavit a spustit ukázku

Sestavit a spustit ukázku pomocí obálku Maven zahrnuté v úložišti:

```bash
cd spring-boot-mysql-todo
mvnw package spring-boot:run
```

Otevřete prohlížeč, abyste `http://localhost:8080` zobrazíte v ukázce v akci. Při přidávání úkolů do seznamu, použijte následující příkazy SQL v řádku MySQL k zobrazení dat uložených v MySQL.

```SQL
use testdb;
select * from todo_item;
```

Zastavte aplikaci zasažení `Ctrl` + `C` v terminálu. 

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

## <a name="create-an-azure-mysql-database"></a>Vytvoření databáze MySQL na Azure

V tomto kroku vytvoříte [Azure Database pro databázi MySQL](../mysql/quickstart-create-mysql-server-database-using-azure-cli.md) pomocí [rozhraní příkazového řádku Azure](https://docs.microsoft.com/cli/azure/install-azure-cli). Nakonfigurujete ukázkovou aplikaci pro tuto databázi použít později v tomto kurzu.

### <a name="create-a-resource-group"></a>Vytvoření skupiny prostředků

Vytvoření [skupiny prostředků](../azure-resource-manager/resource-group-overview.md) s [vytvořit skupinu az](/cli/azure/group#create) příkaz. Skupinu prostředků Azure je logický kontejner, kde jsou související prostředky jako webové aplikace, databáze a účtů úložiště nasadit a spravovat. 

Následující příklad vytvoří skupinu prostředků v oblasti Severní Evropa:

```azurecli-interactive
az group create --name myResourceGroup --location "North Europe"
```    

Zobrazíte možné hodnoty, které můžete použít pro `--location`, použijte [míst seznamu služby App Service az](/cli/azure/appservice#list-locations) příkaz.

### <a name="create-a-mysql-server"></a>Vytvoření databáze MySQL serveru

V prostředí cloudu, vytvoření serveru ve službě Azure Database pro databázi MySQL (Preview) pomocí [az mysql server vytvořit](/cli/azure/mysql/server#create) příkaz.    
Nahraďte vlastní jedinečný název serveru databáze MySQL, kde uvidíte `<mysql_server_name>` zástupný symbol. Tento název je součástí název hostitele serveru MySQL, `<mysql_server_name>.mysql.database.azure.com`, takže ho musí být globálně jedinečný. Také nahraďte `<admin_user>` a `<admin_password>` vlastními hodnotami.

```azurecli-interactive
az mysql server create --name <mysql_server_name> --resource-group myResourceGroup --location "North Europe" --admin-user <admin_user> --admin-password <admin_password>
```

Při vytvoření serveru MySQL rozhraní příkazového řádku Azure obsahuje informace o podobně jako v následujícím příkladu:

```json
{
  "administratorLogin": "admin_user",
  "administratorLoginPassword": null,
  "fullyQualifiedDomainName": "mysql_server_name.mysql.database.azure.com",
  "id": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/myResourceGroup/providers/Microsoft.DBforMySQL/servers/mysql_server_name",
  "location": "northeurope",
  "name": "mysql_server_name",
  "resourceGroup": "mysqlJavaResourceGroup",
  ...
  < Output has been truncated for readability >
}
```

### <a name="configure-server-firewall"></a>Konfigurace brány firewall serveru

V prostředí cloudu, vytvořte pravidlo brány firewall pro váš server MySQL a povolíte připojení klienta pomocí [az mysql pravidla brány firewall-vytvořit](/cli/azure/mysql/server/firewall-rule#create) příkaz. 

```azurecli-interactive
az mysql server firewall-rule create --name allIPs --server <mysql_server_name> --resource-group myResourceGroup --start-ip-address 0.0.0.0 --end-ip-address 255.255.255.255
```

> [!NOTE]
> Azure databáze pro databázi MySQL (Preview) automaticky aktuálně neumožňuje připojení ze služby Azure. Jak budou dynamicky přiřazovat IP adresy v Azure, je lepší, pokud chcete povolit všechny IP adresy pro nyní. Služba se stále jeho verze preview, lepší metody pro zabezpečení databáze bude povolena.

## <a name="configure-the-azure-mysql-database"></a>Konfiguraci databáze MySQL na Azure

V místní okno terminálu připojení k serveru databáze MySQL v Azure. Použít hodnotu zadanou dříve pro `<admin_user>` a `<mysql_server_name>`.

```bash
mysql -u <admin_user>@<mysql_server_name> -h <mysql_server_name>.mysql.database.azure.com -P 3306 -p
```

### <a name="create-a-database"></a>Vytvoření databáze 

V `mysql` výzvu, vytvořte databázi a tabulku položkami seznamu úkolů.

```sql
CREATE DATABASE tododb;
```

### <a name="create-a-user-with-permissions"></a>Vytvořit uživatele s oprávněními

Vytvoření uživatele databáze a pojmenujte ho všechna oprávnění `tododb` databáze. Nahraďte zástupné symboly `<Javaapp_user>` a `<Javaapp_password>` s vlastními jedinečným názvem aplikace.

```sql
CREATE USER '<Javaapp_user>' IDENTIFIED BY '<Javaapp_password>'; 
GRANT ALL PRIVILEGES ON tododb.* TO '<Javaapp_user>';
```

Ukončení připojení k serveru zadáním `quit`.

```sql
quit
```

## <a name="deploy-the-sample-to-azure-app-service"></a>Ukázka nasazení do Azure App Service

Vytvořit plán aplikační služby Azure s **volné** cenová úroveň pomocí [vytvořit plán aplikační služby az](/cli/azure/appservice/plan#create) rozhraní příkazového řádku příkaz. Plán aplikační služby definuje fyzické prostředky, které jsou použity k hostování vaší aplikace. Všechny aplikace, které jsou přiřazené plán služby App Service sdílení těchto prostředků, což umožňuje uložit nákladů při hostování více aplikací. 

```azurecli-interactive
az appservice plan create --name myAppServicePlan --resource-group myResourceGroup --sku FREE
```

Až bude plán připravena, rozhraní příkazového řádku Azure ukazuje podobné výstupu v následujícím příkladu:

```json
{ 
  "adminSiteName": null,
  "appServicePlanName": "myAppServicePlan",
  "geoRegion": "North Europe",
  "hostingEnvironmentProfile": null,
  "id": "/subscriptions/0000-0000/resourceGroups/myResourceGroup/providers/Microsoft.Web/serverfarms/myAppServicePlan",
  "kind": "app",
  "location": "North Europe",
  "maximumNumberOfWorkers": 1,
  "name": "myAppServicePlan",
  ...
  < Output has been truncated for readability >
} 
``` 

### <a name="create-an-azure-web-app"></a>Vytvoření webové aplikace Azure

 V prostředí cloudu, pomocí [az webapp vytvořit](/cli/azure/appservice/web#create) rozhraní příkazového řádku příkaz k vytvoření definice webové aplikace v `myAppServicePlan` plán služby App Service. Definice webové aplikace adresa URL pro přístup k vaší aplikace pomocí poskytuje a konfiguruje celou řadu možností pro nasazení kódu do Azure. 

```azurecli-interactive
az webapp create --name <app_name> --resource-group myResourceGroup --plan myAppServicePlan
```

Nahraďte `<app_name>` zástupný symbol vlastní jedinečným názvem aplikace. Tento jedinečný název je součástí výchozí název domény pro webovou aplikaci, tak název musí být jedinečný v rámci všech aplikací v Azure. Můžete namapovat zadání názvu vlastní domény do webové aplikace ještě před zveřejněním pro vaše uživatele.

Při definici webové aplikace je připraven, rozhraní příkazového řádku Azure uvádí informace podobně jako v následujícím příkladu: 

```json 
{
  "availabilityState": "Normal",
  "clientAffinityEnabled": true,
  "clientCertEnabled": false,
  "cloningInfo": null,
  "containerSize": 0,
  "dailyMemoryTimeQuota": 0,
  "defaultHostName": "<app_name>.azurewebsites.net",
  "enabled": true,
   ...
  < Output has been truncated for readability >
}
```

### <a name="configure-java"></a>Konfigurace Java 

V prostředí cloudu, nastavit konfiguraci Java runtime, která vaše aplikace, musí se [aktualizace konfigurace webové služby App Service az](/cli/azure/appservice/web/config#update) příkaz.

Následující příkaz nakonfiguruje webové aplikace ke spuštění na poslední JDK 8 Java a [Apache Tomcat](http://tomcat.apache.org/) 8.0.

```azurecli-interactive
az webapp config set --name <app_name> --resource-group myResourceGroup --java-version 1.8 --java-container Tomcat --java-container-version 8.0
```

### <a name="configure-the-app-to-use-the-azure-sql-database"></a>Konfigurace aplikace, které chcete použít databázi Azure SQL

Než spustíte ukázkovou aplikaci, nastavte nastavení aplikace na webovou aplikaci k používání databáze MySQL na Azure, kterou jste vytvořili v Azure. Tyto vlastnosti jsou umístěny do webové aplikace jako proměnné prostředí a přepsat s hodnotami nastavenými v application.properties uvnitř zabalené webové aplikace. 

V prostředí cloudu, nastavte nastavení aplikace pomocí [az webapp konfigurace appsettings](https://docs.microsoft.com/cli/azure/appservice/web/config/appsettings) v rozhraní příkazového řádku:

```azurecli-interactive
az webapp config appsettings set --settings SPRING_DATASOURCE_URL="jdbc:mysql://<mysql_server_name>.mysql.database.azure.com:3306/tododb?verifyServerCertificate=true&useSSL=true&requireSSL=false" --resource-group myResourceGroup --name <app_name>
```

```azurecli-interactive
az webapp config appsettings set --settings SPRING_DATASOURCE_USERNAME=Javaapp_user@mysql_server_name --resource-group myResourceGroup --name <app_name>
```

```azurecli-interactive
az webapp config appsettings set --settings SPRING_DATASOURCE_URL=Javaapp_password --resource-group myResourceGroup --name <app_name>
```

### <a name="get-ftp-deployment-credentials"></a>Získat přihlašovací údaje pro nasazení serveru FTP 
Můžete nasadit aplikace do služby Azure App Service různými způsoby, včetně FTP, místní Git, GitHub, Visual Studio Team Services a BitBucket. V tomto příkladu FTP k nasazení. Soubor WAR dříve vytvořené na místním počítači do služby Azure App Service.

Chcete-li zjistit, jaké přihlašovací údaje předávat podél příkaz ftp do webové aplikace, použijte [az služby App Service web nasazení seznamu publikování profily](https://docs.microsoft.com/cli/azure/appservice/web/deployment#az_appservice_web_deployment_list_publishing_profiles) příkazu v prostředí cloudu: 

```azurecli-interactive
az webapp deployment list-publishing-profiles --name <app_name> --resource-group myResourceGroup --query "[?publishMethod=='FTP'].{URL:publishUrl, Username:userName,Password:userPWD}" --output json
```

```JSON
[
  {
    "Password": "aBcDeFgHiJkLmNoPqRsTuVwXyZ",
    "URL": "ftp://waws-prod-blu-069.ftp.azurewebsites.windows.net/site/wwwroot",
    "Username": "app_name\\$app_name"
  }
]
```

### <a name="upload-the-app-using-ftp"></a>Nahrání aplikace pomocí protokolu FTP

Vaše oblíbené nástroje FTP použít k nasazení. Soubor WAR */site/wwwroot/webapps* složky na adresu serveru, který je převzat ze `URL` pole v předchozí příkaz. Odeberte existující adresář aplikace výchozí (uživatel ROOT) a nahraďte existující ROOT.war s. Soubor WAR součástí výše v tomto kurzu.

```bash
ftp waws-prod-blu-069.ftp.azurewebsites.windows.net
Connected to waws-prod-blu-069.drip.azurewebsites.windows.net.
220 Microsoft FTP Service
Name (waws-prod-blu-069.ftp.azurewebsites.windows.net:raisa): app_name\$app_name
331 Password required
Password:
cd /site/wwwroot/webapps
mdelete -i ROOT/*
rmdir ROOT/
put target/TodoDemo-0.0.1-SNAPSHOT.war ROOT.war
```

### <a name="test-the-web-app"></a>Test webové aplikace

Přejděte do `http://<app_name>.azurewebsites.net/` a přidejte do seznamu několik úloh. 

![Java aplikace spuštěné v Azure App Service](./media/app-service-web-tutorial-java-mysql/appservice-web-app.png)

**Blahopřejeme!** Používáte datové aplikace v jazyce Java v Azure App Service.

## <a name="update-the-app-and-redeploy"></a>Aktualizace a opětovné nasazení aplikace

Aktualizujte aplikaci pro zahrnutí sloupec v seznamu úkolů určitý den, položka byla vytvořena. Spouštěcí pružiny zpracovává aktualizace schématu databáze pro vás jako změn datových modelů beze změny stávajících záznamů databáze.

1. V lokálním systému, otevře *src/main/java/com/example/fabrikam/TodoItem.java* a přidejte následující importy pro třídu:   

    ```java
    import java.text.SimpleDateFormat;
    import java.util.Calendar;
    ```

2. Přidat `String` vlastnost `timeCreated` k *src/main/java/com/example/fabrikam/TodoItem.java*, inicializace s časovým razítkem na vytvoření objektu. Přidání mechanismy získání nebo nastavení pro nové `timeCreated` vlastnost během úprav tohoto souboru.

    ```java
    private String name;
    private boolean complete;
    private String timeCreated;
    ...

    public TodoItem(String category, String name) {
       this.category = category;
       this.name = name;
       this.complete = false;
       this.timeCreated = new SimpleDateFormat("MMMM dd, YYYY").format(Calendar.getInstance().getTime());
    }
    ...
    public void setTimeCreated(String timeCreated) {
       this.timeCreated = timeCreated;
    }

    public String getTimeCreated() {
        return timeCreated;
    }
    ```

3. Aktualizace *src/main/java/com/example/fabrikam/TodoDemoController.java* řádek v `updateTodo` metodu a nastavit časové razítko:

    ```java
    item.setComplete(requestItem.isComplete());
    item.setId(requestItem.getId());
    item.setTimeCreated(requestItem.getTimeCreated());
    repository.save(item);
    ```

4. Přidáte podporu pro nové pole v šabloně Thymeleaf. Aktualizace *src/main/resources/templates/index.html* s novou hlavičkou tabulky pro časové razítko a nové pole, které chcete zobrazit hodnotu časového razítka v každém řádku dat tabulky.

    ```html
    <th>Name</th>
    <th>Category</th>
    <th>Time Created</th>
    <th>Complete</th>
    ...
    <td th:text="${item.category}">item_category</td><input type="hidden" th:field="*{todoList[__${i.index}__].category}"/>
    <td th:text="${item.timeCreated}">item_time_created</td><input type="hidden" th:field="*{todoList[__${i.index}__].timeCreated}"/>
    <td><input type="checkbox" th:checked="${item.complete} == true" th:field="*{todoList[__${i.index}__].complete}"/></td>
    ```

5. Znovu sestavte aplikaci:

    ```bash
    mvnw clean package 
    ```

6. FTP aktualizaci. WAR jako před, odebrání stávající *lokality/wwwroot/webapps/ROOT* adresáře a *ROOT.war*, pak odesílání aktualizovaný. Soubor WAR jako ROOT.war. 

Při aktualizaci aplikace, **čas vytvoření** sloupec je nyní viditelné. Když přidáte novou úlohu, aplikace bude naplnit časové razítko. Stávající úlohy zůstat beze změny a pracovat s aplikací, i když základní datový model byl změněn. 

![Aplikace v jazyce Java aktualizovat pomocí nového sloupce](./media/app-service-web-tutorial-java-mysql/appservice-updates-java.png)
      
## <a name="stream-diagnostic-logs"></a>Diagnostické protokoly datového proudu 

Při spuštění aplikace v jazyce Java v Azure App Service, můžete získat protokoly konzoly přesměruje přímo do terminálu. Tímto způsobem můžete získat stejné diagnostické zprávy pomoci při ladění chyb aplikace.

Spusťte protokolu streamování pomocí [az webapp protokolu poškozené databáze za](/cli/azure/webapp/log?view=azure-cli-latest#az_webapp_log_tail) příkazu v prostředí cloudu.

```azurecli-interactive 
az webapp log tail --name <app_name> --resource-group myResourceGroup 
``` 

## <a name="manage-your-azure-web-app"></a>Správa Azure webové aplikace

Přejděte na portálu Azure najdete v části webové aplikace, kterou jste vytvořili.

Chcete-li to provést, přihlaste se na adrese [https://portal.azure.com](https://portal.azure.com).

V levé nabídce klikněte na **App Service** a pak klikněte na název vaší webové aplikace Azure.

![Navigace portálem k webové aplikaci Azure](./media/app-service-web-tutorial-java-mysql/access-portal.png)

Ve výchozím nastavení bude okno vaší webové aplikace obsahovat stránku **Přehled**. Tato stránka poskytuje přehled, jak si vaše aplikace stojí. Zde můžete také provést úlohy správy, jako zastavení, spuštění, restartování a delete. Karty na levé straně okna obsahují další stránky konfigurace, které můžete otevřít.

![Okno App Service na webu Azure Portal](./media/app-service-web-tutorial-java-mysql/web-app-blade.png)

Tyto karty v okně zobrazují mnoho skvělých funkcí, které můžete do své webové aplikace přidat. Následující seznam obsahuje jen několik možností:
* Mapování vlastního názvu DNS
* Vazba vlastního certifikátu SSL
* Konfigurace průběžného nasazování
* Vertikální i horizontální navýšení kapacity
* Přidání ověřování uživatelů

## <a name="clean-up-resources"></a>Vyčištění prostředků

Pokud nepotřebujete tyto prostředky pro jiné kurzu (viz [další kroky](#next)), můžete je odstranit spuštěním následujícího příkazu v prostředí cloudu: 
  
```azurecli-interactive
az group delete --name myResourceGroup 
``` 

<a name="next"></a>

## <a name="next-steps"></a>Další kroky

> [!div class="checklist"]
> * Vytvoření databáze MySQL v Azure
> * Připojení k MySQL ukázkovou aplikaci Java
> * Nasazení aplikace do Azure
> * Aktualizace a opětovné nasazení aplikace
> * Diagnostické protokoly datového proudu z Azure
> * Spravovat aplikaci na portálu Azure

Přechodu na dalším kurzu se dozvíte, jak namapovat vlastní název DNS do aplikace.

> [!div class="nextstepaction"] 
> [Mapování existujícího vlastního názvu DNS na Azure Web Apps](app-service-web-tutorial-custom-domain.md)
