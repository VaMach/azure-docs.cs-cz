<properties
   pageTitle="Služba Marathon specifická pro aplikaci nebo pro uživatele | Microsoft Azure"
   description="Vytvoření služby Marathon specifické pro aplikaci nebo uživatele"
   services="container-service"
   documentationCenter=""
   authors="rgardler"
   manager="timlt"
   editor=""
   tags="acs, azure-container-service"
   keywords="Containers, Marathon, Micro-services, DC/OS, Azure"/>

<tags
   ms.service="container-service"
   ms.devlang="na"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="04/12/2016"
   ms.author="rogardle"/>

# Vytvoření služby Marathon specifické pro aplikaci nebo uživatele

Azure Container Service poskytuje sadu hlavních serverů, na kterých předem konfigurujeme Apache Mesos a Marathon. Ty je možné použít k orchestrování aplikací v clusteru, ale vhodnější je hlavní servery k tomuto účelu nepoužívat. Například při úpravách konfigurace Marathonu je nutné se k přihlásit k samotným hlavním serverům a provést změny přímo na nich – to může vést ke vzniku jedinečných hlavních serverů, které se jen málo liší od těch standardních a je třeba o ně nezávisle pečovat a spravovat je. Kromě toho konfigurace, kterou vyžaduje jeden tým, nemusí být optimální pro jiný tým. V tomto článku si vysvětlíme, jak přidat službu Marathon specifickou pro uživatele nebo aplikaci.

Protože tato služba bude patřit jedinému uživateli nebo týmu, je možné ji nakonfigurovat, jakkoli si budoucí uživatelé přejí. Kromě toho Azure Container Service zajistí, že služba bude stále spuštěná. Pokud by služba selhala, Azure Container Service vám ji restartuje. Ve většině případů si ani nevšimnete, že došlo k výpadku.

## Předpoklady

[Nasaďte instanci Azure Container Service](container-service-deployment.md) s typem orchestrátoru DC/OS, [ověřte, že se klient může připojit ke clusteru](container-service-connect.md), a [AZURE.INCLUDE [install the DC/OS CLI](../../includes/container-service-install-dcos-cli-include.md)].

## Vytvoření služby Marathon specifické pro aplikaci nebo uživatele

Začněte tím, že vytvoříte konfigurační soubor JSON, který definuje název vytvářené aplikační služby. Zde jako název rozhraní používáme `marathon-alice`. Uložte soubor s názvem, jako je `marathon-alice.json`:

```json
{"marathon": {"framework-name": "marathon-alice" }}
```

Pak pomocí rozhraní příkazového řádku DC/OS nainstalujte instanci Marathonu s možnostmi nastavenými v konfiguračním souboru:

```bash
dcos package install --options=marathon-alice.json marathon
```

Nyní byste svou službu `marathon-alice` měli vidět spuštěnou na kartě služeb v uživatelském rozhraní DC/OS. Pokud chcete k uživatelskému rozhraní přistupovat přímo, bude na adrese `http://<hostname>/service/marathon-alice/`.

## Nastavení rozhraní příkazového řádku DC/OS pro přístup ke službě

Volitelně je možné následujícím způsobem nakonfigurovat rozhraní příkazového řádku DC/OS pro přístup k této nové službě, a to tím, že nastavíte vlastnost `marathon.url`, aby ukazovala na instanci `marathon-alice`:

```bash
dcos config set marathon.url http://<hostname>/service/marathon-alice/
```

Pomocí příkazu `dcos config show` je možné ověřit, se kterou instancí Marathonu vaše rozhraní příkazového řádku pracuje. Příkazem `dcos config unset marathon.url` se pak můžete vrátit zpět k používání hlavní služby Marathon.


<!--HONumber=Jun16_HO2-->


