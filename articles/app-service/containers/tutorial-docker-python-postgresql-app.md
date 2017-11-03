---
title: "Vytvoření webové aplikace Docker Python a PostgreSQL v Azure | Microsoft Docs"
description: "Další informace o získání a Docker Python aplikaci v Azure, funguje s připojením k databázi PostgreSQL."
services: app-service\web
documentationcenter: python
author: berndverst
manager: erikre
editor: 
ms.assetid: 2bada123-ef18-44e5-be71-e16323b20466
ms.service: app-service-web
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: python
ms.topic: tutorial
ms.date: 05/03/2017
ms.author: beverst
ms.custom: mvc
ms.openlocfilehash: fa3aa3a73338970fde2d0b0230e7b2e6ca687dc9
ms.sourcegitcommit: b979d446ccbe0224109f71b3948d6235eb04a967
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/25/2017
---
# <a name="build-a-docker-python-and-postgresql-web-app-in-azure"></a>Vytvoření webové aplikace Docker Python a PostgreSQL v Azure

Webové aplikace pro kontejnery nabízí vysoce škálovatelnou a automatických oprav webové hostitelské služby. Tento kurz ukazuje, jak vytvořit základní webovou aplikaci Docker Python v Azure. Tuto aplikaci budete připojit k databázi PostgreSQL. Když jste hotovi, budete mít k aplikaci Python Flask systémem v rámci kontejner Docker [služby App Service v systému Linux](app-service-linux-intro.md).

![Docker Python Flask aplikace v App Service v systému Linux](./media/tutorial-docker-python-postgresql-app/docker-flask-in-azure.png)

Postupujte podle těchto kroků v systému macOS. Pokyny pro Linux a Windows jsou stejné ve většině případů, ale rozdíly nejsou popsané v tomto kurzu.
 
## <a name="prerequisites"></a>Požadavky

Pro absolvování tohoto kurzu potřebujete:

1. [Nainstalovat Git](https://git-scm.com/).
1. [Nainstalovat Python](https://www.python.org/downloads/).
1. [Nainstalujte a spusťte PostgreSQL](https://www.postgresql.org/download/)
1. [Nainstalujte Docker Community Edition](https://www.docker.com/community-edition)

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

Pokud se rozhodnete nainstalovat a používat rozhraní příkazového řádku (CLI) místně, musíte mít spuštěnou verzi Azure CLI 2.0 nebo novější. Verzi zjistíte spuštěním příkazu `az --version`. Pokud potřebujete instalaci nebo upgrade, přečtěte si téma [Instalace Azure CLI 2.0]( /cli/azure/install-azure-cli). 

## <a name="test-local-postgresql-installation-and-create-a-database"></a>Testování místní instalaci PostgreSQL a vytvořit databázi

Otevřete okno terminálu a spusťte `psql postgres` pro připojení k místní server PostgreSQL.

```bash
psql postgres
```

Pokud připojení úspěšné, je databáze PostgreSQL spuštěna. Pokud ne, ujistěte se, zda je spuštěná místní databázi PostgresQL podle kroků v [stáhne - PostgreSQL základní distribuční](https://www.postgresql.org/download/).

Vytvoření databáze názvem *eventregistration* a nastavení uživatele samostatné databáze s názvem *manager* heslem *supersecretpass*.

```bash
CREATE DATABASE eventregistration;
CREATE USER manager WITH PASSWORD 'supersecretpass';
GRANT ALL PRIVILEGES ON DATABASE eventregistration TO manager;
```
Typ *\q* ukončíte klienta PostgreSQL. 

<a name="step2"></a>

## <a name="create-local-python-flask-application"></a>Vytvořit místní aplikace Python Flask

V tomto kroku nastavíte místní projekt Python Flask.

### <a name="clone-the-sample-application"></a>Klonování ukázkové aplikace

Otevřete okno terminálu a `CD` do pracovního adresáře.

Spusťte následující příkazy a klonovat úložiště v ukázkové přejít na *0,1 initialapp* verzi.

```bash
git clone https://github.com/Azure-Samples/docker-flask-postgres.git
cd docker-flask-postgres
git checkout tags/0.1-initialapp
```

Tato ukázka úložiště obsahuje [Flask](http://flask.pocoo.org/) aplikace. 

### <a name="run-the-application"></a>Spuštění aplikace

> [!NOTE] 
> Později tento proces zjednodušit podle budovy kontejner Docker pro použití s produkční databázi.

Nainstalujte požadované balíčky a spusťte aplikaci.

```bash
pip install virtualenv
virtualenv venv
source venv/bin/activate
pip install -r requirements.txt
cd app
FLASK_APP=app.py DBHOST="localhost" DBUSER="manager" DBNAME="eventregistration" DBPASS="supersecretpass" flask db upgrade
FLASK_APP=app.py DBHOST="localhost" DBUSER="manager" DBNAME="eventregistration" DBPASS="supersecretpass" flask run
```

Po úplným načtením aplikace se zobrazí podobná následující zpráva:

```bash
INFO  [alembic.runtime.migration] Context impl PostgresqlImpl.
INFO  [alembic.runtime.migration] Will assume transactional DDL.
INFO  [alembic.runtime.migration] Running upgrade  -> 791cd7d80402, empty message
 * Serving Flask app "app"
 * Running on http://127.0.0.1:5000/ (Press CTRL+C to quit)
```

V prohlížeči přejděte na `http://127.0.0.1:5000`. Klikněte na tlačítko **zaregistrovat!** a vytvoření zkušebního uživatele.

![Aplikace Python Flask spuštěn místně](./media/tutorial-docker-python-postgresql-app/local-app.png)

Ukázkovou aplikaci Flask ukládá data uživatele v databázi. Pokud jste úspěšné při registraci uživatele, je vaše aplikace zápisu dat do místní databázi PostgreSQL.

Kdykoli zastavit Flask server kdykoli, zadejte Ctrl + C v terminálu. 

## <a name="create-a-production-postgresql-database"></a>Vytvořit databázi PostgreSQL výroby

V tomto kroku vytvoříte databázi PostgreSQL v Azure. Při nasazení aplikace do Azure, použije tuto databázi cloudu.

### <a name="log-in-to-azure"></a>Přihlaste se k Azure.

Nyní se chystáte použít 2.0 rozhraní příkazového řádku Azure k vytvoření prostředky potřebné k hostování vaší aplikace Python ve webové aplikaci pro kontejnery.  Přihlaste se k předplatnému Azure pomocí příkazu [az login](/cli/azure/#login) a postupujte podle pokynů na obrazovce.

```azurecli
az login
```

### <a name="create-a-resource-group"></a>Vytvoření skupiny prostředků

Vytvořte pomocí příkazu [az group create](/cli/azure/group#create) [skupinu prostředků](../../azure-resource-manager/resource-group-overview.md).

[!INCLUDE [Resource group intro](../../../includes/resource-group.md)]

Následující příklad vytvoří skupinu prostředků v oblasti západní USA:

```azurecli-interactive
az group create --name myResourceGroup --location "West US"
```

Použití [az služby App Service seznamu umístění](/cli/azure/appservice#list-locations) příkaz rozhraní příkazového řádku Azure k seznamu dostupných umístění.

### <a name="create-an-azure-database-for-postgresql-server"></a>Vytvoření serveru Azure Database for PostgreSQL

Vytvoření serveru PostgreSQL s [az postgres server vytvořit](/cli/azure/documentdb#create) příkaz.

V následujícím příkazu nahraďte název jedinečný serveru  *\<postgresql_name >* zástupný symbol a uživatel název  *\<admin_username >* zástupný symbol. Název serveru slouží jako součást váš koncový bod PostgreSQL (`https://<postgresql_name>.postgres.database.azure.com`), takže název musí být jedinečný v rámci všech serverech v Azure. Uživatelské jméno je pro uživatelský účet správce počáteční databáze. Zobrazí se výzva k vyberte heslo pro tohoto uživatele.

```azurecli-interactive
az postgres server create --resource-group myResourceGroup --name <postgresql_name> --admin-user <admin_username>
```

Při vytvoření databáze Azure PostgreSQL server pro rozhraní příkazového řádku Azure obsahuje informace o podobně jako v následujícím příkladu:

```json
{
  "administratorLogin": "<my_admin_username>",
  "fullyQualifiedDomainName": "<postgresql_name>.postgres.database.azure.com",
  "id": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/myResourceGroup/providers/Microsoft.DBforPostgreSQL/servers/<postgresql_name>",
  "location": "westus",
  "name": "<postgresql_name>",
  "resourceGroup": "myResourceGroup",
  "sku": {
    "capacity": 100,
    "family": null,
    "name": "PGSQLS3M100",
    "size": null,
    "tier": "Basic"
  },
  "sslEnforcement": null,
  "storageMb": 2048,
  "tags": null,
  "type": "Microsoft.DBforPostgreSQL/servers",
  "userVisibleState": "Ready",
  "version": null
}
```

### <a name="create-a-firewall-rule-for-the-azure-database-for-postgresql-server"></a>Vytvoření pravidla firewallu pro databázi Azure pro PostgreSQL serveru

Spusťte následující příkaz rozhraní příkazového řádku Azure pro povolení přístupu k databázi z všechny IP adresy.

```azurecli-interactive
az postgres server firewall-rule create --resource-group myResourceGroup --server-name <postgresql_name> --start-ip-address=0.0.0.0 --end-ip-address=255.255.255.255 --name AllowAllIPs
```

Rozhraní příkazového řádku Azure potvrdí vytvoření pravidla brány firewall se výstup podobný v následujícím příkladu:

```json
{
  "endIpAddress": "255.255.255.255",
  "id": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/myResourceGroup/providers/Microsoft.DBforPostgreSQL/servers/<postgresql_name>/firewallRules/AllowAllIPs",
  "name": "AllowAllIPs",
  "resourceGroup": "myResourceGroup",
  "startIpAddress": "0.0.0.0",
  "type": "Microsoft.DBforPostgreSQL/servers/firewallRules"
}
```

## <a name="connect-your-python-flask-application-to-the-database"></a>Připojení k databázi aplikace Python Flask

V tomto kroku připojíte vaši ukázkovou aplikaci Python Flask pro Azure databázi PostgreSQL serveru, který jste vytvořili.

### <a name="create-an-empty-database-and-set-up-a-new-database-application-user"></a>Vytvořit prázdnou databázi a nastavení nového uživatele databáze aplikace

Vytvořte uživatele databáze s přístupem k jedné databáze. Tyto přihlašovací údaje použijete k neudělujte aplikace úplný přístup k serveru.

Připojení k databázi (se zobrazí výzva k zadání hesla správce).

```bash
psql -h <postgresql_name>.postgres.database.azure.com -U <my_admin_username>@<postgresql_name> postgres
```

Vytvoření databáze a uživatele z příkazového řádku PostgreSQL.

```bash
CREATE DATABASE eventregistration;
CREATE USER manager WITH PASSWORD 'supersecretpass';
GRANT ALL PRIVILEGES ON DATABASE eventregistration TO manager;
```

Typ *\q* ukončíte klienta PostgreSQL.

### <a name="test-the-application-locally-against-the-azure-postgresql-database"></a>Testování aplikace místně v databázi Azure PostgreSQL

Po návratu teď do *aplikace* složky klonovaný úložiště Github, můžete spustit aplikace Python Flask aktualizací proměnné prostředí databáze.

```bash
FLASK_APP=app.py DBHOST="<postgresql_name>.postgres.database.azure.com" DBUSER="manager@<postgresql_name>" DBNAME="eventregistration" DBPASS="supersecretpass" flask db upgrade
FLASK_APP=app.py DBHOST="<postgresql_name>.postgres.database.azure.com" DBUSER="manager@<postgresql_name>" DBNAME="eventregistration" DBPASS="supersecretpass" flask run
```

Po úplným načtením aplikace se zobrazí podobná následující zpráva:

```bash
INFO  [alembic.runtime.migration] Context impl PostgresqlImpl.
INFO  [alembic.runtime.migration] Will assume transactional DDL.
INFO  [alembic.runtime.migration] Running upgrade  -> 791cd7d80402, empty message
 * Serving Flask app "app"
 * Running on http://127.0.0.1:5000/ (Press CTRL+C to quit)
```

Přejděte do http://127.0.0.1:5000 v prohlížeči. Klikněte na tlačítko **zaregistrovat!** a vytvořit testovací registrace. Jsou nyní zápis dat do databáze v Azure.

![Aplikace Python Flask spuštěn místně](./media/tutorial-docker-python-postgresql-app/local-app.png)

### <a name="running-the-application-from-a-docker-container"></a>Spuštění aplikace z kontejner Docker

Sestavení Docker kontejneru bitové kopie.

```bash
cd ..
docker build -t flask-postgresql-sample .
```

Docker zobrazí potvrzení, že úspěšně vytvořil kontejneru.

```bash
Successfully built 7548f983a36b
```

Přidání proměnné prostředí databáze do souboru proměnné prostředí *db.env*. Aplikace se připojí k produkční databázi PostgreSQL v Azure.

```text
DBHOST="<postgresql_name>.postgres.database.azure.com"
DBUSER="manager@<postgresql_name>"
DBNAME="eventregistration"
DBPASS="supersecretpass"
```

Spusťte aplikaci v rámci kontejner Docker. Následující příkaz určuje soubor proměnných prostředí a Flask výchozí port 5000 se mapuje na místní port 5000.

```bash
docker run -it --env-file db.env -p 5000:5000 flask-postgresql-sample
```

Výstup se podobá jste viděli dříve. Ale migrace počáteční databáze už je nutné provést a proto bude přeskočena.

```bash
INFO  [alembic.runtime.migration] Context impl PostgresqlImpl.
INFO  [alembic.runtime.migration] Will assume transactional DDL.
 * Serving Flask app "app"
 * Running on http://0.0.0.0:5000/ (Press CTRL+C to quit)
```

Databáze již obsahuje registrace, kterou jste vytvořili dříve.

![Docker na základě kontejneru aplikace Python Flask spuštěn místně](./media/tutorial-docker-python-postgresql-app/local-docker.png)

## <a name="upload-the-docker-container-to-a-container-registry"></a>Odešlete do kontejneru registru kontejner Docker

V tomto kroku nahrajte kontejner Docker do registru kontejneru. Budete používat Azure kontejneru registru, ale můžete také použít další oblíbených těch, jako je například Docker Hub.

### <a name="create-an-azure-container-registry"></a>Vytvoření služby Azure Container Registry

V následujícím příkazu k vytvoření kontejneru registru nahradit  *\<registry_name >* s názvem registru jedinečný kontejner Azure podle svého výběru.

```azurecli-interactive
az acr create --name <registry_name> --resource-group myResourceGroup --location "West US" --sku Basic
```

Výstup

```json
{
  "adminUserEnabled": false,
  "creationDate": "2017-05-04T08:50:55.635688+00:00",
  "id": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/myResourceGroup/providers/Microsoft.ContainerRegistry/registries/<registry_name>",
  "location": "westus",
  "loginServer": "<registry_name>.azurecr.io",
  "name": "<registry_name>",
  "provisioningState": "Succeeded",
  "sku": {
    "name": "Basic",
    "tier": "Basic"
  },
  "storageAccount": {
    "name": "<registry_name>01234"
  },
  "tags": {},
  "type": "Microsoft.ContainerRegistry/registries"
}
```

### <a name="retrieve-the-registry-credentials-for-pushing-and-pulling-docker-images"></a>Načtení registru přihlašovací údaje pro vkládání a stahování imagí Dockeru

Zobrazíte registru přihlašovací údaje, nejprve povolte režim správce.

```azurecli-interactive
az acr update --name <registry_name> --admin-enabled true
az acr credential show -n <registry_name>
```

Zobrazí dvě hesla. Poznamenejte si uživatelské jméno a heslo první.

```json
{
  "passwords": [
    {
      "name": "password",
      "value": "<registry_password>"
    },
    {
      "name": "password2",
      "value": "<registry_password2>"
    }
  ],
  "username": "<registry_name>"
}
```

### <a name="upload-your-docker-container-to-azure-container-registry"></a>Nahrát váš kontejner Docker do registru kontejner Azure

```bash
docker login <registry_name>.azurecr.io -u <registry_name> -p "<registry_password>"
docker tag flask-postgresql-sample <registry_name>.azurecr.io/flask-postgresql-sample
docker push <registry_name>.azurecr.io/flask-postgresql-sample
```

## <a name="deploy-the-docker-python-flask-application-to-azure"></a>Nasaďte aplikaci Docker Python Flask do Azure

V tomto kroku nasadíte Docker na základě kontejneru aplikace Python Flask do služby Azure App Service.

### <a name="create-an-app-service-plan"></a>Vytvoření plánu služby App Service

Pomocí příkazu [az appservice plan create](/cli/azure/appservice/plan#create) vytvořte plán služby App Service.

[!INCLUDE [app-service-plan](../../../includes/app-service-plan-linux.md)]

Následující příklad vytvoří plán aplikační služby se systémem Linux s názvem *myAppServicePlan* pomocí S1 cenová úroveň:

```azurecli-interactive
az appservice plan create --name myAppServicePlan --resource-group myResourceGroup --sku S1 --is-linux
```

Když je vytvořen plán služby App Service, rozhraní příkazového řádku Azure uvádí informace podobně jako v následujícím příkladu:

```json
{
  "adminSiteName": null,
  "appServicePlanName": "myAppServicePlan",
  "geoRegion": "West US",
  "hostingEnvironmentProfile": null,
  "id": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/myResourceGroup/providers/Microsoft.Web/serverfarms/myAppServicePlan", 
  "kind": "linux",
  "location": "West US",
  "maximumNumberOfWorkers": 10,
  "name": "myAppServicePlan",
  "numberOfSites": 0,
  "perSiteScaling": false,
  "provisioningState": "Succeeded",
  "reserved": true,
  "resourceGroup": "myResourceGroup",
  "sku": {
    "capabilities": null,
    "capacity": 1,
    "family": "S",
    "locations": null,
    "name": "S1",
    "size": "S1",
    "skuCapacity": null,
    "tier": "Standard"
  },
  "status": "Ready",
  "subscription": "00000000-0000-0000-0000-000000000000",
  "tags": null,
  "targetWorkerCount": 0,
  "targetWorkerSizeId": 0,
  "type": "Microsoft.Web/serverfarms",
  "workerTierName": null
}
```

### <a name="create-a-web-app"></a>Vytvoření webové aplikace

Vytvoření webové aplikace v *myAppServicePlan* plán služby App Service pomocí [az webapp vytvořit](/cli/azure/webapp#create) příkaz.

Webová aplikace získáte hostování místa k nasazení kódu a poskytuje adresu URL zobrazení nasazené aplikace. Použijte k vytvoření webové aplikace.

V následujícím příkazu nahraďte  *\<app_name >* zástupný symbol s jedinečným názvem aplikace. Tento název je součástí výchozí adresa URL pro webovou aplikaci, tak název musí být jedinečný v rámci všech aplikací v Azure App Service.

```azurecli
az webapp create --name <app_name> --resource-group myResourceGroup --plan myAppServicePlan
```

Po vytvoření webové aplikace se v rozhraní příkazového řádku Azure CLI zobrazí podobné informace jako v následujícím příkladu:

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

### <a name="configure-the-database-environment-variables"></a>Konfigurace databáze proměnné prostředí

V tomto kurzu definované proměnné prostředí pro připojení k vaší databázi PostgreSQL.

Ve službě App Service, můžete nastavit proměnné prostředí jako _nastavení aplikace_ pomocí [az webapp konfigurace appsettings sadu](/cli/azure/webapp/config#set) příkaz.

Následující příklad určuje podrobnosti připojení databáze jako nastavení aplikace. Používá také *PORT* proměnnou mapu PORT 5000 z vaší kontejner Docker pro příjem provozu HTTP na portu 80.

```azurecli-interactive
az webapp config appsettings set --name <app_name> --resource-group myResourceGroup --settings DBHOST="<postgresql_name>.postgres.database.azure.com" DBUSER="manager@<postgresql_name>" DBPASS="supersecretpass" DBNAME="eventregistration" PORT=5000
```

### <a name="configure-docker-container-deployment"></a>Konfigurace nasazení kontejner Docker

Služby App Service může automaticky stáhnout a spusťte kontejner Docker.

```azurecli
az webapp config container set --resource-group myResourceGroup --name <app_name> --docker-registry-server-user "<registry_name>" --docker-registry-server-password "<registry_password>" --docker-custom-image-name "<registry_name>.azurecr.io/flask-postgresql-sample" --docker-registry-server-url "https://<registry_name>.azurecr.io"
```

Kdykoli aktualizovat kontejner Docker nebo změnit nastavení, restartujte aplikaci. Restartování zajišťuje, že všechna nastavení použita a kontejneru nejnovější pocházejí z registru.

```azurecli-interactive
az webapp restart --resource-group myResourceGroup --name <app_name>
```

### <a name="browse-to-the-azure-web-app"></a>Přejděte do webové aplikace Azure 

Přejděte do nasazené webové aplikace pomocí webového prohlížeče. 

```bash 
http://<app_name>.azurewebsites.net 
```
> [!NOTE]
> Webové aplikace trvá déle, načíst, protože má kontejneru se stáhnou a spustí po změně konfigurace kontejneru.

Zobrazí dříve zaregistrovaný hosté, které byly uloženy do Azure produkční databázi v předchozím kroku.

![Docker na základě kontejneru aplikace Python Flask spuštěn místně](./media/tutorial-docker-python-postgresql-app/docker-app-deployed.png)

**Blahopřejeme!** Používáte Docker aplikace Python Flask na základě kontejneru ve službě Azure App Service.

## <a name="update-data-model-and-redeploy"></a>Aktualizace datový model a znovu ho zaveďte

V tomto kroku přidáte počtu účastníků k registraci jednotlivých událostí aktualizací modelu hosta.

Podívejte se *0,2 Migrace* verzi pomocí následujícího příkazu git:

```bash
git checkout tags/0.2-migration
```

Tato verze již provedli nezbytné změny zobrazení, řadiče a modelu. Zahrnuje také migrace databáze generované prostřednictvím *destilační přístroj* (`flask db migrate`). Zobrazí všechny změny provedené pomocí následujícího příkazu git:

```bash
git diff 0.1-initialapp 0.2-migration
```

### <a name="test-your-changes-locally"></a>Otestujte provedené změny místně

Spusťte následující příkazy k testování změny místně spuštěním flask server.

Mac / Linux:
```bash
source venv/bin/activate
cd app
FLASK_APP=app.py DBHOST="localhost" DBUSER="manager" DBNAME="eventregistration" DBPASS="supersecretpass" flask db upgrade
FLASK_APP=app.py DBHOST="localhost" DBUSER="manager" DBNAME="eventregistration" DBPASS="supersecretpass" flask run
```

Přejděte do http://127.0.0.1:5000 v prohlížeči a prohlédněte si změny. Vytvořte testovací registrace.

![Docker na základě kontejneru aplikace Python Flask spuštěn místně](./media/tutorial-docker-python-postgresql-app/local-app-v2.png)

### <a name="publish-changes-to-azure"></a>Publikování změn do Azure

Vytvořit novou bitovou kopii docker, push registru kontejneru a restartujte aplikaci.

```bash
docker build -t flask-postgresql-sample .
docker tag flask-postgresql-sample <registry_name>.azurecr.io/flask-postgresql-sample
docker push <registry_name>.azurecr.io/flask-postgresql-sample
az appservice web restart --resource-group myResourceGroup --name <app_name>
```

Přejděte do vaší webové aplikace Azure a znovu vyzkoušet nové funkce. Vytvořte další registrace události.

```bash 
http://<app_name>.azurewebsites.net 
```

![Docker Python Flask aplikace v Azure App Service](./media/tutorial-docker-python-postgresql-app/docker-flask-in-azure.png)

## <a name="manage-your-azure-web-app"></a>Správa Azure webové aplikace

Přejděte na [portál Azure](https://portal.azure.com) zobrazíte webové aplikace, které jste vytvořili.

V levé nabídce klikněte na **App Services** a pak klikněte na název vaší webové aplikace Azure.

![Navigace portálem k webové aplikaci Azure](./media/tutorial-docker-python-postgresql-app/app-resource.png)

Ve výchozím nastavení, zobrazí na portálu vaší webové aplikace **přehled** stránky. Tato stránka poskytuje přehled, jak si vaše aplikace stojí. Tady můžete také provést základní úlohy správy, jako je procházení, zastavení, spuštění, restartování a odstranění. Karty na levé straně stránky zobrazí stránek jinou konfiguraci, že můžete otevřít.

![Stránka služby App Service na webu Azure Portal](./media/tutorial-docker-python-postgresql-app/app-mgmt.png)

## <a name="next-steps"></a>Další kroky

Přechodu na dalším kurzu se dozvíte, jak namapovat vlastní název DNS do vaší webové aplikace.

> [!div class="nextstepaction"]
> [Mapování existujícího vlastního názvu DNS na Azure Web Apps](../app-service-web-tutorial-custom-domain.md)
