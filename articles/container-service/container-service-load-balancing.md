<properties
   pageTitle="Vyrovnání zatížení v clusteru Azure Container Service | Microsoft Azure"
   description="V clusteru Azure Container Service můžete vyrovnávat zatížení."
   services="container-service"
   documentationCenter=""
   authors="rgardler"
   manager="timlt"
   editor=""
   tags="acs, azure-container-service"
   keywords="Containers, Micro-services, DC/OS, Azure"/>

<tags
   ms.service="container-service"
   ms.devlang="na"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="04/18/2016"
   ms.author="rogardle"/>

# Vyrovnání zatížení v clusteru Azure Container Service

V tomto článku nastavíme webový front-end, který je možné škálovat na více systémů, aby mohl poskytovat služby za hranicemi Azure LB.


## Předpoklady

[Nasaďte instanci Azure Container Service](container-service-deployment.md) s typem orchestrátoru DC/OS, [ověřte, že se klient může připojit ke clusteru](container-service-connect.md), a [AZURE.INCLUDE [install the DC/OS CLI](../../includes/container-service-install-dcos-cli-include.md)].


## Vyrovnávání zatížení

V clusteru Container Service existují dvě vrstvy vyrovnávání zatížení: Azure LB pro veřejné vstupní body (ty, které využívají koncoví uživatelé), a základní marathon-lb, který směruje příchozí požadavky na požadavky obsluhy instancí kontejnerů. Marathon-lb se bude dynamicky adaptovat podle aktuální škály kontejnerů, které poskytují danou službu.

## Marathon LB 

Řešení Marathon LB dynamicky změní svou konfiguraci podle kontejnerů, které jste nasadili. Kromě toho je také odolný vůči ztrátě kontejneru nebo agenta. Pokud ke ztrátě dojde, Mesos jednoduše restartuje kontejner na jiném místě a změní konfiguraci Marathon LB. 

Pokud chcete nainstalovat Marathon LB, spusťte ze svého klientského počítače následující příkaz:

```bash
dcos package install marathon-lb 
``` 

Nyní když už máme balíček marathon-lb, můžeme nasadit jednoduchý webový server. K tomu slouží následující konfigurace:


```json
{ 
  "id": "web", 
  "container": { 
    "type": "DOCKER", 
    "docker": { 
      "image": "tutum/hello-world", 
      "network": "BRIDGE", 
      "portMappings": [ 
        { "hostPort": 0, "containerPort": 80, "servicePort": 10000 } 
      ], 
      "forcePullImage":true 
    } 
  }, 
  "instances": 3, 
  "cpus": 0.1, 
  "mem": 65, 
  "healthChecks": [{ 
      "protocol": "HTTP", 
      "path": "/", 
      "portIndex": 0, 
      "timeoutSeconds": 10, 
      "gracePeriodSeconds": 10, 
      "intervalSeconds": 2, 
      "maxConsecutiveFailures": 10 
  }], 
  "labels":{ 
    "HAPROXY_GROUP":"external",
    "HAPROXY_0_VHOST":"YOUR FQDN",
    "HAPROXY_0_MODE":"http" 
  } 
}

```

Klíčovými částmi tohoto procesu je: 
  * Nastavení hodnoty HAProxy_0_VHOST na plně kvalifikovaný název domény nástroje pro vyrovnávání zatížení pro vaše agenty. To má formát `<acsName>agents.<region>.cloudapp.azure.com`. Pokud například vytvoříme cluster Container Service s názvem `myacs` v oblasti `West US`, plně kvalifikovaný název domény by byl `myacsagents.westus.cloudapp.azure.com`. Toto je možné zjistit také tak, že si při procházení prostředků ve skupině prostředků, kterou jste vytvořili pro kontejnerovou službu na [Portálu Azure](https://portal.azure.com), vyhledáte nástroj pro vyrovnávání zatížení na základě slova „agent“ v názvu.
  * Nastavte servicePort na port > = 10 000. Tím se identifikuje služba, která běží v tomto kontejneru. Marathon-lb toto nastavení použije, aby identifikoval služby, které by měl využít při vyrovnávání.
  * Nastavte popisek HAPROXY_GROUP na „external“.
  * Nastavte HostPort na 0. Tato možnost znamená, že marathon libovolně vyhradí některý z dostupných portů.

Zkopírujte tento text JSON do souboru s názvem `hello-web.json` a použijte jej k nasazení kontejneru: 

```bash
dcos marathon app add hello-web.json 
``` 

## Azure LB 

Ve výchozím nastavení Azure LB zpřístupňuje porty 80, 8080 a 443. Pokud používáte některý z těchto tří portů (jako my v příkladu výše), nepotřebujete provádět žádnou další akci: měli byste mít možnost zadat plně kvalifikovaný název domény agenta LB a při každé aktualizaci pak použít jeden ze tří webových serverů principem kruhového dotazování. Pokud ale používáte jiný port, bude potřeba přidat pravidlo kruhového dotazování a najít na Azure LB port, který jste použili. To je možné provést z [rozhraní příkazového řádku Azure XPLAT](../xplat-cli-azure-resource-manager.md) pomocí příkazů `azure lb rule create` a `azure lb probe create`.


## Další scénáře

Možná řešíte scénář, ve kterém pomocí různých domén zpřístupňujete různé služby. Například: 

mojedomena1.com -> Azure LB:80 -> marathon-lb:10001 -> mujkontejner1:33292  
mojedomena2.com -> Azure LB:80 -> marathon-lb:10002 -> mujkontejner2:22321 

Pokud chcete dosáhnout takovéto funkčnosti, přečtěte si téma [Virtuální hostitelé](https://mesosphere.com/blog/2015/12/04/dcos-marathon-lb/). Najdete v něm způsob, jak přiřadit domény ke konkrétním cestám marathon-lb.

Alternativě můžete zveřejnit různé porty a přemapovat je na správné služby za marathon lb. Například:

Azure lb:80 -> marathon-lb:10001 -> mujkontejner:233423  
Azure lb:8080 -> marathon-lb:1002 -> mujkontejner2:33432 
 

## Další kroky

Další informace o Marathon LB najdete v [tomto blogovém příspěvku](https://mesosphere.com/blog/2015/12/04/dcos-marathon-lb/).



<!--HONumber=Jun16_HO2-->


