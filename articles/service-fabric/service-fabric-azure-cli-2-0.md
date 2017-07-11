---
title: "Začínáme s platformou Service Fabric a Azure CLI 2.0"
description: "Jak používat modul příkazů Service Fabric v Azure CLI verze 2.0, včetně připojení ke clusteru a správy aplikací"
services: service-fabric
author: samedder
manager: timlt
ms.service: service-fabric
ms.topic: get-started-article
ms.date: 06/21/2017
ms.author: edwardsa
ms.translationtype: Human Translation
ms.sourcegitcommit: 6efa2cca46c2d8e4c00150ff964f8af02397ef99
ms.openlocfilehash: c5cc6e54acf27456185eeb48858c4d981aa46b4b
ms.contentlocale: cs-cz
ms.lasthandoff: 07/01/2017

---
<a id="service-fabric-and-azure-cli-20" class="xliff"></a>

# Service Fabric a Azure CLI 2.0

Nové Azure CLI 2.0 teď zahrnuje příkazy pro správu clusterů Service Fabric. Tato dokumentace obsahuje kroky pro začátek s Azure CLI.

<a id="install-azure-cli-20" class="xliff"></a>

## Instalace Azure CLI 2.0

Azure CLI teď zahrnuje příkazy pro interakci s clustery Service Fabric a jejich správu. Pokud chcete získat nejnovější Azure CLI, můžete postupovat podle [standardního procesu instalace](https://docs.microsoft.com/en-us/cli/azure/install-azure-cli).

Další informace najdete v [dokumentaci k Azure CLI 2.0](https://docs.microsoft.com/en-us/cli/azure/overview).

<a id="cli-syntax" class="xliff"></a>

## Syntaxe rozhraní příkazového řádku

Všechny příkazy Azure Service Fabric mají v Azure CLI předponu `az sf`. Další informace o dostupných příkazech a obecné informace získáte spuštěním příkazu `az sf -h`. Nebo můžete získat podrobnou nápovědu k jednotlivým příkazům spuštěním příkazu `az sf <command> -h`.

Příkazy Azure Service Fabric v rozhraní příkazového řádku používají formát pojmenování:

```azurecli
az sf <object> <action>
```

V tomto případě je objekt `<object>` cílem pro akci `<action>`.

<a id="selecting-a-cluster" class="xliff"></a>

## Výběr clusteru

Před provedením jakékoli operace musíte vybrat cluster, ke kterému se připojíte. Podívejte se například na následující fragment kódu pro připojení k nezabezpečenému clusteru.

> [!WARNING]
> Nepoužívejte nezabezpečené clustery Service Fabric pro produkční prostředí.

```azurecli
az sf cluster select --endpoint http://testcluster.com:19080
```

Koncový bod clusteru musí mít předponu `http` nebo `https` a musí zahrnovat port pro bránu HTTP. Tento port a adresa jsou stejné jako adresa URL nástroje Service Fabric Explorer.

Pro clustery zabezpečené pomocí certifikátu jsou podporované buď nezabezpečené soubory `pem`, nebo soubory `crt` a `key`.

```azurecli
az sf cluster select --endpoint https://testsecurecluster.com:19080 --pem ./client.pem
```

Další informace najdete v [podrobném dokumentu týkajícím se připojení k zabezpečeným clusterům](service-fabric-connect-to-secure-cluster.md).

> [!NOTE]
> Příkaz select neprovede žádné požadavky před vrácením. Pokud chcete ověřit správné zadání clusteru, spusťte příkaz jako `az sf cluster health` a zkontrolujte, že příkaz nevrací žádné chyby.

<a id="performing-basic-operations" class="xliff"></a>

## Provádění základních operací

Informace o připojení ke clusteru se uchovávají napříč různými relacemi Azure CLI. Jakmile je vybraný cluster Service Fabric, můžete spouštět jakékoli příkazy Service Fabric.

Pokud například chcete získat stav clusteru Service Fabric, spusťte následující příkaz

```azurecli
az sf cluster health
```

Za předpokladu, že je v konfiguraci Azure CLI nastavený výstup ve formátu JSON, bude výsledkem příkazu následující výstup:

```json
{
  "aggregatedHealthState": "Ok",
  "applicationHealthStates": [
    {
      "aggregatedHealthState": "Ok",
      "name": "fabric:/System"
    }
  ],
  "healthEvents": [],
  "nodeHealthStates": [
    {
      "aggregatedHealthState": "Ok",
      "id": {
        "id": "66aa824a642124089ee474b398d06a57"
      },
      "name": "_Test_0"
    }
  ],
  "unhealthyEvaluations": []
}
```

<a id="tips-and-faq" class="xliff"></a>

## Tipy a nejčastější dotazy

Následující informace můžou být užitečné v případě, že narazíte na problémy s příkazy Service Fabric v Azure CLI.

<a id="converting-a-certificate-from-pfx-to-pem" class="xliff"></a>

### Převod certifikátu z formátu PFX na PEM

Azure CLI podporuje certifikáty na straně klienta v podobě souborů PEM (s příponou `.pem`). Pokud používáte soubory PFX ve Windows, je nutné převést tyto certifikáty do formátu PEM. K převodu souboru PFX na soubor PEM použijte následující příkaz:

```bash
openssl pkcs12 -in certificate.pfx -out mycert.pem -nodes
```

Podrobnosti najdete v [dokumentaci k OpenSSL](https://www.openssl.org/docs/man1.0.1/apps/pkcs12.html).

<a id="connection-issues" class="xliff"></a>

### Problémy s připojením

Při provádění operací můžete narazit na následující chybu:

> `Failed to establish a new connection: [Errno 8] nodename nor servname provided, or not known`

V takovém případě znovu zkontrolujte, že zadaný koncový bod clusteru je dostupný a naslouchá. Také ověřte, že je na daném hostiteli a portu dostupné uživatelské rozhraní Service Fabric Explorer. Pomocí příkazu `az sf cluster select` aktualizujte koncový bod.

<a id="getting-detailed-logs" class="xliff"></a>

### Získání podrobných protokolů

Při ladění nebo hlášení chyb je užitečné zahrnout podrobné protokoly. Azure CLI zahrnuje globální příznak `--debug`, kterým se zvyšuje úroveň podrobností protokolů.

<a id="command-help-and-syntax" class="xliff"></a>

### Nápověda k příkazům a jejich syntaxe

Příkazy Service Fabric se řídí stejnou konvencí jako Azure CLI. Zadáním příznaku `-h` získáte nápovědu ke konkrétnímu příkazu nebo skupině příkazů. Například:

```azurecli
az sf application -h
```

nebo

```azurecli
az sf application create -h
```

