---
title: "Řešení potíží s instancí Azure kontejnerů"
description: "Zjistěte, jak vyřešit problémy s instancí kontejnerů Azure"
services: container-instances
author: seanmck
manager: timlt
ms.service: container-instances
ms.topic: article
ms.date: 01/02/2018
ms.author: seanmck
ms.custom: mvc
ms.openlocfilehash: 561729e5e495500222ccec5b4b536a3152cb25e3
ms.sourcegitcommit: 782d5955e1bec50a17d9366a8e2bf583559dca9e
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 03/02/2018
---
# <a name="troubleshoot-deployment-issues-with-azure-container-instances"></a>Řešení potíží s nasazením s instancemi Azure kontejneru

Tento článek ukazuje, jak vyřešit problémy při nasazení kontejnerů do Azure kontejner instancí. Také popisuje některé běžné problémy, které můžete narazit na.

## <a name="get-diagnostic-events"></a>Získání diagnostických událostí

Chcete-li zobrazit protokoly z kódu aplikace v rámci kontejneru, můžete použít [az kontejneru protokoly] [ az-container-logs] příkaz. Ale pokud vaše kontejneru není úspěšně nasazena, je potřeba zkontrolovat diagnostické informace poskytované poskytovatelem prostředků Azure kontejner instancí. Pokud chcete zobrazit události pro vaše kontejneru, spusťte [az kontejneru zobrazit] [ az-container-show] příkaz:

```azurecli-interactive
az container show --resource-group myResourceGroup --name mycontainer
```

Výstup obsahuje základní vlastnosti kontejneru, společně s událostí nasazení (viz zde zkrácený):

```JSON
{
  "containers": [
    {
      "command": null,
      "environmentVariables": [],
      "image": "microsoft/aci-helloworld",
      ...
        "events": [
          {
            "count": 1,
            "firstTimestamp": "2017-12-21T22:50:49+00:00",
            "lastTimestamp": "2017-12-21T22:50:49+00:00",
            "message": "pulling image \"microsoft/aci-helloworld\"",
            "name": "Pulling",
            "type": "Normal"
          },
          {
            "count": 1,
            "firstTimestamp": "2017-12-21T22:50:59+00:00",
            "lastTimestamp": "2017-12-21T22:50:59+00:00",
            "message": "Successfully pulled image \"microsoft/aci-helloworld\"",
            "name": "Pulled",
            "type": "Normal"
          },
          {
            "count": 1,
            "firstTimestamp": "2017-12-21T22:50:59+00:00",
            "lastTimestamp": "2017-12-21T22:50:59+00:00",
            "message": "Created container with id 2677c7fd54478e5adf6f07e48fb71357d9d18bccebd4a91486113da7b863f91f",
            "name": "Created",
            "type": "Normal"
          },
          {
            "count": 1,
            "firstTimestamp": "2017-12-21T22:50:59+00:00",
            "lastTimestamp": "2017-12-21T22:50:59+00:00",
            "message": "Started container with id 2677c7fd54478e5adf6f07e48fb71357d9d18bccebd4a91486113da7b863f91f",
            "name": "Started",
            "type": "Normal"
          }
        ],
        "previousState": null,
        "restartCount": 0
      },
      "name": "mycontainer",
      "ports": [
        {
          "port": 80,
          "protocol": null
        }
      ],
      ...
    }
  ],
  ...
}
```

## <a name="common-deployment-issues"></a>Běžné problémy při nasazení

Tento účet pro většinu chyb v nasazení existuje několik běžných problémů.

## <a name="image-version-not-supported"></a>Nepodporovaná verze bitové kopie

Pokud bitovou kopii není zadaný, že kontejner instancí Azure nepodporuje, bude vrácena chyba formuláře `ImageVersionNotSupported`. Hodnota chyby zobrazí `The version of image '{0}' is not supported.`. Tato chyba se uplatní bitové kopie systému Windows. 1709 zmírnit použijte bitovou kopii systému Windows LTS. Podpora pro bitové kopie systému Windows. 1709 probíhá.

## <a name="unable-to-pull-image"></a>Nelze pro vyžádání obsahu image

Pokud instance kontejner Azure nemůže původně vyžádání bitové kopie, se pokusí po nějakou dobu před selháním nakonec. Pokud nelze načíst obrázek, událostmi, jako je následující jsou uvedeny ve výstupu [az kontejneru zobrazit][az-container-show]:

```bash
"events": [
  {
    "count": 3,
    "firstTimestamp": "2017-12-21T22:56:19+00:00",
    "lastTimestamp": "2017-12-21T22:57:00+00:00",
    "message": "pulling image \"microsoft/aci-hellowrld\"",
    "name": "Pulling",
    "type": "Normal"
  },
  {
    "count": 3,
    "firstTimestamp": "2017-12-21T22:56:19+00:00",
    "lastTimestamp": "2017-12-21T22:57:00+00:00",
    "message": "Failed to pull image \"microsoft/aci-hellowrld\": rpc error: code 2 desc Error: image t/aci-hellowrld:latest not found",
    "name": "Failed",
    "type": "Warning"
  },
  {
    "count": 3,
    "firstTimestamp": "2017-12-21T22:56:20+00:00",
    "lastTimestamp": "2017-12-21T22:57:16+00:00",
    "message": "Back-off pulling image \"microsoft/aci-hellowrld\"",
    "name": "BackOff",
    "type": "Normal"
  }
],
```

Vyřešit, odstraňte kontejneru a opakujte vaše nasazení, platící zvýšené pozornosti, že jste správně zadali název bitové kopie.

## <a name="container-continually-exits-and-restarts"></a>Ukončení a restartování průběžně kontejneru

Pokud vaše kontejneru dokončí a automaticky restartuje, je nutné nastavit [restartujte zásad](container-instances-restart-policy.md) z **OnFailure –** nebo **nikdy**. Pokud zadáte **OnFailure** a stále najdete potom restartování, může být problém s aplikací nebo skript spustit ve vašem kontejneru.

Zahrnuje rozhraní API instancí kontejneru `restartCount` vlastnost. Chcete-li zkontrolovat počet restartování pro kontejner, můžete použít [az kontejneru zobrazit] [ az-container-show] v Azure CLI 2.0. V následující příklad výstupu (který byl zkrácen jako stručný výtah), se zobrazí `restartCount` vlastnost na konci výstupu.

```json
...
 "events": [
   {
     "count": 1,
     "firstTimestamp": "2017-11-13T21:20:06+00:00",
     "lastTimestamp": "2017-11-13T21:20:06+00:00",
     "message": "Pulling: pulling image \"myregistry.azurecr.io/aci-tutorial-app:v1\"",
     "type": "Normal"
   },
   {
     "count": 1,
     "firstTimestamp": "2017-11-13T21:20:14+00:00",
     "lastTimestamp": "2017-11-13T21:20:14+00:00",
     "message": "Pulled: Successfully pulled image \"myregistry.azurecr.io/aci-tutorial-app:v1\"",
     "type": "Normal"
   },
   {
     "count": 1,
     "firstTimestamp": "2017-11-13T21:20:14+00:00",
     "lastTimestamp": "2017-11-13T21:20:14+00:00",
     "message": "Created: Created container with id bf25a6ac73a925687cafcec792c9e3723b0776f683d8d1402b20cc9fb5f66a10",
     "type": "Normal"
   },
   {
     "count": 1,
     "firstTimestamp": "2017-11-13T21:20:14+00:00",
     "lastTimestamp": "2017-11-13T21:20:14+00:00",
     "message": "Started: Started container with id bf25a6ac73a925687cafcec792c9e3723b0776f683d8d1402b20cc9fb5f66a10",
     "type": "Normal"
   }
 ],
 "previousState": null,
 "restartCount": 0
...
}
```

> [!NOTE]
> Většina kontejneru bitových kopií pro Linuxových distribucích prostředí, jako je například bash, nastavit jako výchozí příkaz. Vzhledem k tomu, že prostředí svoje vlastní není služba dlouho běžící, tyto kontejnery okamžitě ukončit a spadají do smyčku restartování při konfiguraci s výchozím **vždy** začít znovu.

## <a name="container-takes-a-long-time-to-start"></a>Kontejner trvá dlouhou dobu spuštění

Pokud vaše kontejneru trvá dlouhou dobu spuštění, ale nakonec úspěšná, začít hledáním na velikost bitové kopie kontejneru. Protože Azure kontejner instancí vrátí kontejner image na vyžádání, čas spuštění, které zaznamenáte přímo souvisí s jeho velikost.

Velikost vaší image kontejneru pomocí rozhraní příkazového řádku Dockeru můžete zobrazit:

```bash
docker images
```

Výstup:

```bash
REPOSITORY                             TAG                 IMAGE ID            CREATED             SIZE
microsoft/aci-helloworld               latest              7f78509b568e        13 days ago         68.1MB
```

Klíč k udržování velikosti obrázků malé zajišťuje, že finální image neobsahuje nic, který není nutný za běhu. Jeden ze způsobů, jak provést toto je s [více fáze sestavení][docker-multi-stage-builds]. Více fáze sestavení zkontrolujte usnadňují zajistěte, aby finální image obsahuje pouze artefakty potřebné pro vaši aplikaci a ne všechny nadbytečné obsahu, kterou nebyla nutná v čase vytvoření buildu.

Jiný způsob, jak snížit dopad vyžádání obsahu bitové kopie na vaše kontejneru spuštění je hostitelem kontejneru image pomocí klíče registru kontejner Azure ve stejné oblasti, kde chcete používat Azure kontejner instancí. To zkracuje síťové cestě, která bitovou kopii kontejneru je potřeba cestují, výrazně zkrátit dobu stahování.

## <a name="resource-not-available-error"></a>Prostředek není k dispozici – chyba

Z důvodu různých místních prostředků zatížení v Azure, může dojít k následující chybě, při pokusu o nasazení instance kontejneru:

`The requested resource with 'x' CPU and 'y.z' GB memory is not available in the location 'example region' at this moment. Please retry with a different resource request or in another location.`

Tato chyba znamená, že kvůli případě velkého zatížení v oblasti, ve které se pokoušíte nasadit prostředky zadané pro váš kontejner nelze přidělit v daném čase. Pomocí jednoho nebo více z následujících kroků pro zmírnění dopadů váš problém vyřešit.

* Zkontrolujte vaše nastavení nasazení kontejneru spadal do parametrech definovaných v [kvóty a dostupnost v oblastech Azure kontejner instancí](container-instances-quotas.md#region-availability)
* Zadejte nižší nastavení procesoru a paměti pro kontejner
* Nasazení v jiné oblasti Azure
* Nasazení později

<!-- LINKS - External -->
[docker-multi-stage-builds]: https://docs.docker.com/engine/userguide/eng-image/multistage-build/

<!-- LINKS - Internal -->
[az-container-logs]: /cli/azure/container#az_container_logs
[az-container-show]: /cli/azure/container#az_container_show