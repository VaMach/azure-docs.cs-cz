---
title: "Řešení potíží s instancí Azure kontejnerů"
description: "Zjistěte, jak vyřešit problémy s instancí kontejnerů Azure"
services: container-instances
documentationcenter: 
author: seanmck
manager: timlt
editor: 
tags: 
keywords: 
ms.assetid: 
ms.service: container-instances
ms.devlang: azurecli
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/31/2017
ms.author: seanmck
ms.custom: mvc
ms.openlocfilehash: ff6da0ce95d0405714602c3872da34a2bff344d3
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/11/2017
---
# <a name="troubleshoot-deployment-issues-with-azure-container-instances"></a>Řešení potíží s nasazením s instancemi Azure kontejneru

Tento článek ukazuje, jak vyřešit problémy při nasazení kontejnerů do Azure kontejner instancí. Také popisuje některé běžné problémy, které může spustit do.

## <a name="getting-diagnostic-events"></a>Získání diagnostických událostí

Chcete-li zobrazit protokoly z kódu aplikace v rámci kontejneru, můžete použít [az kontejneru protokoly](/cli/azure/container#logs) příkaz. Ale pokud vaše kontejneru není úspěšně nasazena, je potřeba zkontrolovat diagnostické informace poskytované poskytovatelem prostředků Azure kontejner instancí. Pokud chcete zobrazit události pro vaše kontejneru, spusťte následující příkaz:

```azurecli-interactive
az container show -n mycontainername -g myresourcegroup
```

Výstup obsahuje základní vlastnosti kontejneru, společně s událostí nasazení:

```bash
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
        "firstTimestamp": "2017-08-03T22:12:52+00:00",
        "lastTimestamp": "2017-08-03T22:12:52+00:00",
        "message": "Pulling: pulling image \"microsoft/aci-helloworld\"",
        "type": "Normal"
      },
      {
        "count": 1,
        "firstTimestamp": "2017-08-03T22:12:55+00:00",
        "lastTimestamp": "2017-08-03T22:12:55+00:00",
        "message": "Pulled: Successfully pulled image \"microsoft/aci-helloworld\"",
        "type": "Normal"
      },
      {
        "count": 1,
        "firstTimestamp": "2017-08-03T22:12:55+00:00",
        "lastTimestamp": "2017-08-03T22:12:55+00:00",
        "message": "Created: Created container with id 61602059d6c31529c27609ef4ec0c858b0a96150177fa045cf944d7cf8fbab69",
        "type": "Normal"
      },
      {
        "count": 1,
        "firstTimestamp": "2017-08-03T22:12:55+00:00",
        "lastTimestamp": "2017-08-03T22:12:55+00:00",
        "message": "Started: Started container with id 61602059d6c31529c27609ef4ec0c858b0a96150177fa045cf944d7cf8fbab69",
        "type": "Normal"
      }
    ],
    "name": "helloworld",
      "ports": [
        {
          "port": 80
        }
      ],
    ...
  ]
}
```

## <a name="common-deployment-issues"></a>Běžné problémy při nasazení

Tento účet pro většinu chyb v nasazení existuje několik běžných problémů.

### <a name="unable-to-pull-image"></a>Nelze pro vyžádání obsahu image

Pokud instance kontejner Azure nemůže původně vyžádání bitové kopie, se pokusí po nějakou dobu před selháním nakonec. Pokud nelze načíst obrázek, jsou uvedeny událostmi, jako je následující:

```bash
"events": [
  {
    "count": 1,
    "firstTimestamp": "2017-08-03T22:19:31+00:00",
    "lastTimestamp": "2017-08-03T22:19:31+00:00",
    "message": "Pulling: pulling image \"microsoft/aci-hellowrld\"",
    "type": "Normal"
  },
  {
    "count": 1,
    "firstTimestamp": "2017-08-03T22:19:32+00:00",
    "lastTimestamp": "2017-08-03T22:19:32+00:00",
    "message": "Failed: Failed to pull image \"microsoft/aci-hellowrld\": rpc error: code 2 desc Error: image microsoft/aci-hellowrld:latest not found",
    "type": "Warning"
  },
  {
    "count": 1,
    "firstTimestamp": "2017-08-03T22:19:33+00:00",
    "lastTimestamp": "2017-08-03T22:19:33+00:00",
    "message": "BackOff: Back-off pulling image \"microsoft/aci-hellowrld\"",
    "type": "Normal"
  }
]
```

Vyřešit, odstraňte kontejneru a opakujte vaše nasazení, platící zvýšené pozornosti, že jste správně zadali název bitové kopie.

### <a name="container-continually-exits-and-restarts"></a>Ukončení a restartování průběžně kontejneru

Kontejner instancí Azure v současné době podporuje pouze dlouhodobé služby. Pokud vaše kontejneru se používá k dokončení a ukončí, automaticky restartuje a znovu spustí. V takovém případě se zobrazí událostmi, jako je následující. Všimněte si, že kontejner úspěšně spustí a potom rychle restartuje. Zahrnuje rozhraní API instancí kontejneru `retryCount` restartování vlastnost, která ukazuje, jak často konkrétním kontejneru.

```bash
"events": [
  {
    "count": 5,
    "firstTimestamp": "2017-08-03T22:21:55+00:00",
    "lastTimestamp": "2017-08-03T22:23:22+00:00",
    "message": "Pulling: pulling image \"alpine\"",
    "type": "Normal"
  },
  {
    "count": 5,
    "firstTimestamp": "2017-08-03T22:21:57+00:00",
    "lastTimestamp": "2017-08-03T22:23:23+00:00",
    "message": "Pulled: Successfully pulled image \"alpine\"",
    "type": "Normal"
  },
  {
    "count": 1,
    "firstTimestamp": "2017-08-03T22:21:57+00:00",
    "lastTimestamp": "2017-08-03T22:21:57+00:00",
    "message": "Created: Created container with id ad2bf9bc51761c5f935260b4bab53b164d52d9cbc045b16afcb26fb4d14d0a70",
    "type": "Normal"
  },
  {
    "count": 1,
    "firstTimestamp": "2017-08-03T22:21:57+00:00",
    "lastTimestamp": "2017-08-03T22:21:57+00:00",
    "message": "Started: Started container with id ad2bf9bc51761c5f935260b4bab53b164d52d9cbc045b16afcb26fb4d14d0a70",
    "type": "Normal"
  },
  {
    "count": 1,
    "firstTimestamp": "2017-08-03T22:21:58+00:00",
    "lastTimestamp": "2017-08-03T22:21:58+00:00",
    "message": "Created: Created container with id 7687b9bd15dc01731fa66fc45f6f0241495600602dd03841e559453245e7f70b",
    "type": "Normal"
  },
  {
    "count": 1,
    "firstTimestamp": "2017-08-03T22:21:58+00:00",
    "lastTimestamp": "2017-08-03T22:21:58+00:00",
    "message": "Started: Started container with id 7687b9bd15dc01731fa66fc45f6f0241495600602dd03841e559453245e7f70b",
    "type": "Normal"
  },
  {
    "count": 13,
    "firstTimestamp": "2017-08-03T22:21:59+00:00",
    "lastTimestamp": "2017-08-03T22:24:36+00:00",
    "message": "BackOff: Back-off restarting failed container",
    "type": "Warning"
  },
  {
    "count": 1,
    "firstTimestamp": "2017-08-03T22:22:13+00:00",
    "lastTimestamp": "2017-08-03T22:22:13+00:00",
    "message": "Created: Created container with id 72e347e891290e238135e4a6b3078748ca25a1275dbbff30d8d214f026d89220",
    "type": "Normal"
  },
  ...
```

> [!NOTE]
> Většina kontejneru bitových kopií pro Linuxových distribucích prostředí, jako je například bash, nastavit jako výchozí příkaz. Vzhledem k tomu, že prostředí svoje vlastní není služba dlouho běžící, tyto kontejnery okamžitě ukončit a spadají do smyčku restartování.

### <a name="container-takes-a-long-time-to-start"></a>Kontejner trvá dlouhou dobu spuštění

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

Klíč k udržování velikosti obrázků malé zajišťuje, že finální image neobsahuje nic, který není nutný za běhu. Jeden ze způsobů, jak provést toto je s [více fáze sestavení](https://docs.docker.com/engine/userguide/eng-image/multistage-build/). Více fáze sestavení zkontrolujte usnadňují zajistěte, aby finální image obsahuje pouze artefakty potřebné pro vaši aplikaci a ne všechny nadbytečné obsahu, kterou nebyla nutná v čase vytvoření buildu.

Jiný způsob, jak snížit dopad vyžádání obsahu bitové kopie na vaše kontejneru spuštění je hostitelem kontejneru image pomocí klíče registru kontejner Azure ve stejné oblasti, kde chcete používat Azure kontejner instancí. To zkracuje síťové cestě, která bitovou kopii kontejneru je potřeba cestují, výrazně zkrátit dobu stahování.

### <a name="resource-not-available-error"></a>Prostředek není k dispozici – chyba

Z důvodu různých místních prostředků zatížení v Azure, může dojít k následující chybě, při pokusu o nasazení instance kontejneru:

`The requested resource with 'x' CPU and 'y.z' GB memory is not available in the location 'example region' at this moment. Please retry with a different resource request or in another location.`

Tato chyba znamená, že kvůli případě velkého zatížení v oblasti, ve které se pokoušíte nasadit prostředky zadané pro váš kontejner nelze přidělit v daném čase. Použít jeden nebo více následující kroky zmírňující rizika pro váš problém vyřešit.

* Zkontrolujte vaše nastavení nasazení kontejneru spadal do parametrech definovaných v [dostupnost v oblastech Azure kontejner instancí](container-instances-region-availability.md)
* Zadejte nižší nastavení procesoru a paměti pro kontejner
* Nasazení v jiné oblasti Azure
* Nasazení později
