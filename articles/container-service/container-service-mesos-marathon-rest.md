<properties
   pageTitle="Správa kontejnerů v Azure Container Service přes rozhraní REST API | Microsoft Azure"
   description="Do clusteru Azure Container Service Mesos můžete nasadit kontejnery pomocí rozhraní Marathon REST API."
   services="container-service"
   documentationCenter=""
   authors="neilpeterson"
   manager="timlt"
   editor=""
   tags="acs, azure-container-service"
   keywords="Docker, Containers, Micro-services, Mesos, Azure"/>

<tags
   ms.service="container-service"
   ms.devlang="na"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="02/16/2016"
   ms.author="nepeters"/>

# Správa kontejnerů přes rozhraní REST API

DC/OS poskytuje prostředí pro nasazování a škálování clusterových úloh a zároveň poskytuje abstrakci používaného hardwaru. Nad DC/OS je rozhraní, které spravuje plánování a provádění výpočetních úloh.

I když jsou pro mnoho populárních úloh k dispozici rozhraní, tento dokument popisuje, jak je možné nasazení kontejnerů vytvořit a škálovat pomocí Marathonu. Než si projdete tyto příklady, budete potřebovat cluster DC/OS nakonfigurovaný v Azure Container Service. Kromě toho je nutné mít možnost se k tomuto clusteru připojit vzdáleně. Další informace k těmto záležitostem najdete v těchto článcích:

- [Nasazení clusteru Azure Container Service](container-service-deployment.md)
- [Připojení ke clusteru Azure Container Service](container-service-connect.md)

Až se připojíte ke clusteru Azure Container Service, budete mít na DC/OS a související rozhraní REST API přístup přes adresu http://localhost:local-port. Příklady v tomto dokumentu předpokládají, že máte k dispozici tunel na portu 80. Například koncový bod Marathonu je k dispozici na adrese `http://localhost/marathon/v2/`. Další informace o různých rozhraních API najdete v dokumentaci Mesosphere pro rozhraní [Marathon API](https://mesosphere.github.io/marathon/docs/rest-api.html) a [Chronos API](https://mesos.github.io/chronos/docs/api.html) a v dokumentaci Apache pro rozhraní [Mesos Scheduler API](http://mesos.apache.org/documentation/latest/scheduler-http-api/).

## Získání informací z DC/OS a Marathonu

Než do clusteru DC/OS nasadíte kontejnery, zjistěte si určité informace o clusteru DC/OS, například názvy a aktuální stav agentů DC/OS. To provedete tak, že zašlete dotaz na koncový bod `master/slaves` rozhraní REST API DC/OS. Pokud všechno proběhne správně, uvidíte seznam agentů DC/OS a u každého z nich několik vlastností.

```bash
curl http://localhost/mesos/master/slaves
```

Nyní pomocí koncového bodu Marathon `/apps` zkontrolujte aktuální nasazení aplikací v clusteru DC/OS. Pokud je to nový cluster, uvidíte prázdné pole aplikací.

```
curl localhost/marathon/v2/apps

{"apps":[]}
```

## Nasazení kontejneru formátovaného Dockerem

Kontejnery formátované Dockerem nasadíte přes Marathon pomocí souboru JSON, který popisuje zamýšlené nasazení. Následující ukázka nasadí kontejner Nginx a sváže port 80 agenta DC/OS s portem 80 kontejneru. Kromě toho si všimněte, že vlastnost acceptedResourceRoles je nastavena na slave_public. Tím se kontejner nasadí do agenta v sadě škálování veřejně přístupných agentů.

```json
{
  "id": "nginx",
  "cpus": 0.1,
  "mem": 16.0,
  "instances": 1,
    "acceptedResourceRoles": [
    "slave_public"
  ],
  "container": {
    "type": "DOCKER",
    "docker": {
      "image": "nginx",
      "network": "BRIDGE",
      "portMappings": [
        { "containerPort": 80, "hostPort": 80, "servicePort": 9000, "protocol": "tcp" }
      ]
    }
  }
}
```

Abyste mohli nasadit kontejner formátovaný Dockerem, vytvořte svůj vlastní soubor JSON nebo použijte ukázku, která je k dispozici v [ukázce Azure Container Service](https://raw.githubusercontent.com/rgardler/AzureDevTestDeploy/master/marathon/marathon.json). Uložte jej na dostupné místo. Pak následujícím příkazem nasaďte kontejner. Zadejte název souboru JSON.

```
# deploy container

curl -X POST http://localhost/marathon/v2/apps -d @marathon.json -H "Content-type: application/json"
```

Výstup se bude podobat tomuto:

```json
{"version":"2015-11-20T18:59:00.494Z","deploymentId":"b12f8a73-f56a-4eb1-9375-4ac026d6cdec"}
```

Nyní když Marathonu odešlete dotaz na aplikace, zobrazí se tato nová aplikace ve výstupu.

```
curl localhost/marathon/v2/apps
```

## Škálování kontejnerů

Rozhraní Marathon API je možné použít i k nasazením aplikací se škálováním pro horizontální navýšení nebo snížení kapacity. V předchozím příkladu jste nasadili jednu instanci aplikace. Nyní škálování aplikace navyšme na tři instance. To provedete tak, že pomocí následujícího textu JSON vytvoříte soubor JSON a uložíte ho na dostupném místě.

```json
{ "instances": 3 }
```

Spusťte následující příkaz, kterým se zahájí škálování aplikace na více systémů.

>[AZURE.NOTE] Identifikátor URI bude http://localhost/marathon/v2/apps/ a pak ID aplikace, která se bude škálovat. Pokud používáte ukázku Nginx, která je zde k dispozici, identifikátor URI by byl http://localhost/marathon/v2/apps/nginx.

```json
# scale container

curl http://localhost/marathon/v2/apps/nginx -H "Content-type: application/json" -X PUT -d @scale.json
```

Nakonec pošlete na koncový bod Marathon dotaz na aplikace. Nyní uvidíte, že tam jsou tři nové kontejnery Nginx.

```
curl localhost/marathon/v2/apps
```

## Toto praktické cvičení s použitím PowerShellu: Interakce rozhraní REST API Marathonu v PowerShellu

V systému Windows můžete tyto stejné akce provést pomocí příkazů PowerShellu.

Informace o clusteru DC/OS, jako jsou názvy agentů a jejich stav, získáte pomocí následujícího příkazu.

```powershell
Invoke-WebRequest -Uri http://localhost/mesos/master/slaves
```

Kontejnery formátované Dockerem nasadíte přes Marathon pomocí souboru JSON, který popisuje zamýšlené nasazení. Následující ukázka nasadí kontejner Nginx a sváže port 80 agenta DC/OS s portem 80 kontejneru.

```json
{
  "id": "nginx",
  "cpus": 0.1,
  "mem": 16.0,
  "instances": 1,
  "container": {
    "type": "DOCKER",
    "docker": {
      "image": "nginx",
      "network": "BRIDGE",
      "portMappings": [
        { "containerPort": 80, "hostPort": 80, "servicePort": 9000, "protocol": "tcp" }
      ]
    }
  }
}
```

Vytvořte svůj vlastní soubor JSON nebo použijte ukázku, která je k dispozici v [ukázce Azure Container Service](https://raw.githubusercontent.com/rgardler/AzureDevTestDeploy/master/marathon/marathon.json). Uložte jej na dostupné místo. Pak následujícím příkazem nasaďte kontejner. Zadejte název souboru JSON.

```powershell
# deploy container

Invoke-WebRequest -Method Post -Uri http://localhost/marathon/v2/apps -ContentType application/json -InFile 'c:\marathon.json'
```

Rozhraní Marathon API je možné použít i k nasazením aplikací se škálováním pro horizontální navýšení nebo snížení kapacity. V předchozím příkladu jste nasadili jednu instanci aplikace. Nyní škálování aplikace navyšme na tři instance. To provedete tak, že pomocí následujícího textu JSON vytvoříte soubor JSON a uložíte ho na dostupném místě.

```json
{ "instances": 3 }
```

Spusťte následující příkaz, kterým se zahájí škálování aplikace na více systémů.

> [AZURE.NOTE] Identifikátor URI bude http://localhost/marathon/v2/apps/ a pak ID aplikace, která se bude škálovat. Pokud používáte ukázku Nginx, která je zde k dispozici, identifikátor URI by byl http://localhost/marathon/v2/apps/nginx.

```powershell
# scale container

Invoke-WebRequest -Method Put -Uri http://localhost/marathon/v2/apps/nginx -ContentType application/json -InFile 'c:\scale.json'
```

## Další kroky

[Další informace o koncových bodech Mesos HTTP]( http://mesos.apache.org/documentation/latest/endpoints/)
[Další informace o rozhraní REST API Marathonu]( https://mesosphere.github.io/marathon/docs/rest-api.html)






<!--HONumber=Jun16_HO2-->


