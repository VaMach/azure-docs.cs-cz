<properties
   pageTitle="Vyrovnání zatížení kontejnerů v clusteru Azure Container Service | Microsoft Azure"
   description="Článek popisuje, jak se vyrovnává zatížení více kontejnerů v clusteru Azure Container Service."
   services="container-service"
   documentationCenter=""
   authors="rgardler"
   manager="timlt"
   editor=""
   tags="acs, azure-container-service"
   keywords="Kontejnery, mikroslužby, DC/OS, Azure"/>

<tags
   ms.service="container-service"
   ms.devlang="na"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="07/11/2016"
   ms.author="rogardle"/>


# Vyrovnání zatížení kontejnerů v clusteru Azure Container Service

V tomto článku se podíváme, jak se pomocí nástroje Marathon-LB vytváří vnitřní nástroj pro vyrovnávání zatížení v prostředí Azure Container Service spravovaném systémem DC/OS. To vám umožní horizontálně škálovat aplikace. Taky vám to umožní využít veřejný i soukromý cluster v tom smyslu, že na veřejný umístíte nástroje pro vyrovnávání zatížení, zatímco na soukromý kontejnery aplikací.

## Požadavky

[Nasaďte instanci Azure Container Service](container-service-deployment.md) s typem orchestrátoru DC/OS a [ověřte, že se váš klient může připojit ke clusteru](container-service-connect.md). 

## Vyrovnávání zatížení

V sestavovaném clusteru Container Service jsou dvě vrstvy vyrovnávající zatížení: 

  1. Nástroj Azure Load Balancer poskytuje veřejné vstupní body (ty, ke kterým se dostanou koncoví uživatelé). To automaticky poskytuje služba Azure Container Service. Ve výchozím nastavení zpřístupňuje porty 80, 443 a 8080.
  2. Nástroj Marathon Load Balancer (marathon-lb) směruje příchozí požadavky na instance kontejneru, které tyto požadavky vyřizují. Marathon-lb se bude dynamicky adaptovat podle toho, jak budeme navyšovat kapacitu kontejnerů, které poskytují naši webovou službu. Tento nástroj pro vyrovnávání zatížení není zahrnutý ve výchozí instalaci služby Container Service, lze jej však velmi snadno doinstalovat.

## Marathon Load Balancer

Marathon Load Balancer se sám dynamicky rekonfiguruje na základě kontejnerů, které jste nasadili. Kromě toho je také odolný vůči ztrátě kontejneru nebo agenta. Když k takové ztrátě dojde, Apache Mesos jednoduše restartuje kontejner na jiném místě a marathon-lb se adaptuje.

K instalaci nástroje Marathon Load Balancer můžete použít buď webové uživatelské rozhraní DC/OS, nebo příkazový řádek DC/OS.

### Instalace nástroje Marathon-LB pomocí webového uživatelského rozhraní DC/OS

  1. Klikněte na „Universe“.
  2. Vyhledejte „Marathon-LB“.
  3. Klikněte na Instalovat.

![Instalace nástroje marathon-LB pomocí webového uživatelského rozhraní DC/OS](./media/dcos/marathon-lb-install.png)

### Instalace nástroje marathon-LB pomocí rozhraní příkazového řádku DC/OS

Po instalaci rozhraní příkazového řádku DC/OS a ověření, že se můžete připojit ke svému clusteru, spusťte ze svého klientského počítače následující příkaz:

```bash
dcos package install marathon-lb
```

Tento příkaz na veřejný cluster automaticky nainstaluje nástroj pro vyrovnávání zatížení.

## Nasazení webové aplikace s vyrovnáváním zatížení

Jakmile máme balíček marathon-lb, můžeme nasadit kontejner aplikace, u kterého chceme vyrovnávat zatížení. V tomto příkladu nasadíme pomocí následující konfigurace jednoduchý webový server:

```json
{
  "id": "web",
  "container": {
    "type": "DOCKER",
    "docker": {
      "image": "yeasy/simple-web",
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

  * Hodnotu parametru `HAProxy_0_VHOST` nastavte na plně kvalifikovaný název domény (FQDN) nástroje pro vyrovnávání zatížení pro vaše agenty. Ten má formát `<acsName>agents.<region>.cloudapp.azure.com`. Pokud například vytvoříte cluster Container Service s názvem `myacs` v oblasti `West US`, plně kvalifikovaný název domény bude `myacsagents.westus.cloudapp.azure.com`. Můžete ho zjistit také tak, že při procházení prostředků ve skupině prostředků, kterou jste vytvořili pro službu Container Service na webu [Azure Portal](https://portal.azure.com), najdete nástroj pro vyrovnávání zatížení na základě slova „agent“ v názvu.
  * Nastavte servicePort na port > = 10 000. Tím se identifikuje služba, která běží v tomto kontejneru. Nástroj marathon-lb použije toto nastavení k identifikaci služby, jejíž zatížení má vyrovnávat.
  * Nastavte parametr `HAPROXY_GROUP` ze skupiny popisků (labels) na „external“.
  * Nastavte `hostPort` na 0. To znamená, že nástroj Marathon libovolně přidělí některý z dostupných portů.
  * Nastavte `instances` na počet instancí, které chcete vytvořit. Tento počet instancí lze kdykoli později zvýšit nebo snížit.

Mějte na paměti, že Marathon se ve výchozím nastavení nasazuje na soukromý cluster. To znamená, že výše popsané nasazení bude přístupné jen prostřednictvím nástroje pro vyrovnávání zatížení, což právě většinou chceme.

### Nasazení pomocí webového uživatelského rozhraní DC/OS

  1. Navštivte stránku nástroje Marathon na adrese http://localhost/marathon (po nastavení [tunelu SSH ](container-service-connect.md) a kliknutí na `Create Appliction`
  2. V dialogovém okně `New Application` klikněte v pravém horním rohu na `JSON Mode`
  3. Vložte do editoru výše uvedený text JSON
  4. Klikněte na `Create Appliction`

### Nasazení pomocí rozhraní příkazového řádku DC/OS

Pokud chcete tuto aplikaci nasadit pomocí rozhraní příkazového řádku DC/OS, jednoduše zkopírujte výše uvedený text JSON do souboru s názvem `hello-web.json` a spusťte následující příkaz:

```bash
dcos marathon app add hello-web.json
```

## Azure Load Balancer

Služba Azure Load Balancer ve výchozím nastavení zpřístupňuje porty 80, 8080 a 443. Pokud používáte některý z těchto tří portů (jako my v příkladu výše), není třeba provádět žádnou další akci. Měli byste mít možnost volat plně kvalifikovaný název domény agenta nástroje pro vyrovnávání zatížení a při každé aktualizaci se bude volat jeden z vašich tří webových serverů, budou se vždy střídat dokola. Pokud ale používáte jiný port, bude potřeba přidat pravidlo kruhového dotazování a otestovat v nástroji pro vyrovnávání zatížení port, který jste použili. To lze udělat z [rozhraní příkazového řádku Azure](../xplat-cli-azure-resource-manager.md) pomocí příkazů `azure lb rule create` a `azure lb probe create`. Můžete to také provést pomocí webu Azure Portal.


## Další scénáře

Možná řešíte scénář, ve kterém pomocí různých domén zpřístupňujete různé služby. Příklad:

mojedomena1.com -> Azure LB:80 -> marathon-lb:10001 -> mujkontejner1:33292  
mojedomena2.com -> Azure LB:80 -> marathon-lb:10002 -> mujkontejner2:22321

Pokud chcete dosáhnout právě tohoto, přečtěte si téma [Virtuální hostitelé](https://mesosphere.com/blog/2015/12/04/dcos-marathon-lb/). Najdete v něm způsob, jak přiřadit domény ke konkrétním cestám nástroje marathon-lb.

Alternativě můžete také zpřístupnit jiné porty a přemapovat je na správné služby za nástrojem marathon-lb. Příklad:

Azure lb:80 -> marathon-lb:10001 -> mujkontejner:233423  
Azure lb:8080 -> marathon-lb:1002 -> mujkontejner2:33432


## Další kroky

Další informace o nástroji [marathon-lb](https://dcos.io/docs/1.7/usage/service-discovery/marathon-lb/) naleznete v dokumentaci DC/OS.



<!--HONumber=Sep16_HO3-->


