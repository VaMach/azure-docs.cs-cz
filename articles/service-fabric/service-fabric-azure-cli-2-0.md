---
title: "Začínáme s platformou Azure Service Fabric a Azure CLI 2.0"
description: "Zjistěte, jak používat modul příkazů Azure Service Fabric v Azure CLI verze 2.0. Zjistěte, jak se připojit ke clusteru a jak spravovat aplikace."
services: service-fabric
author: samedder
manager: timlt
ms.service: service-fabric
ms.topic: get-started-article
ms.date: 06/21/2017
ms.author: edwardsa
ms.translationtype: HT
ms.sourcegitcommit: 49bc337dac9d3372da188afc3fa7dff8e907c905
ms.openlocfilehash: ee3302b984ca2f5509755dc17b0a5fd06ace0afe
ms.contentlocale: cs-cz
ms.lasthandoff: 07/14/2017

---
# <a name="azure-service-fabric-and-azure-cli-20"></a>Azure Service Fabric a Azure CLI 2.0

Rozhraní příkazového řádku Azure (Azure CLI) verze 2.0 obsahuje příkazy, které vám pomůžou se správou clusterů Azure Service Fabric. Zjistěte, jak začít s Azure CLI a platformou Service Fabric.

## <a name="install-azure-cli-20"></a>Instalace Azure CLI 2.0

K interakci s clustery Service Fabric a jejich správě můžete použít příkazy Azure CLI 2.0. Pokud chcete získat nejnovější verzi Azure CLI, postupujte podle [procesu standardní instalace Azure CLI 2.0](https://docs.microsoft.com/en-us/cli/azure/install-azure-cli).

Další informace najdete v [přehledu Azure CLI 2.0](https://docs.microsoft.com/en-us/cli/azure/overview).

## <a name="azure-cli-syntax"></a>Syntaxe Azure CLI

V Azure CLI mají všechny příkazy Service Fabric předponu `az sf`. Obecné informace o příkazech, které můžete použít, získáte pomocí příkazu `az sf -h`. Nápovědu ke konkrétnímu příkazu získáte pomocí příkazu `az sf <command> -h`.

Příkazy Service Fabric v Azure CLI používají tento vzorec pojmenování:

```azurecli
az sf <object> <action>
```

Objekt `<object>` je cílem pro akci `<action>`.

## <a name="select-a-cluster"></a>Výběr clusteru

Před provedením jakékoli operace musíte vybrat cluster, ke kterému se připojíte. Příklad najdete v následujícím kódu. Kód se připojí k nezabezpečenému clusteru.

> [!WARNING]
> Nepoužívejte nezabezpečené clustery Service Fabric v produkčním prostředí.

```azurecli
az sf cluster select --endpoint http://testcluster.com:19080
```

Koncový bod clusteru musí mít předponu `http` nebo `https`. Musí zahrnovat port pro bránu HTTP. Tento port a adresa jsou stejné jako adresa URL nástroje Service Fabric Explorer.

Pro clustery zabezpečené pomocí certifikátu můžete použít buď nešifrované soubory .pem, nebo soubory .crt a .key. Například:

```azurecli
az sf cluster select --endpoint https://testsecurecluster.com:19080 --pem ./client.pem
```

Další informace najdete v tématu [Připojení k zabezpečenému clusteru Azure Service Fabric](service-fabric-connect-to-secure-cluster.md).

> [!NOTE]
> Příkaz `select` před navrácením nereaguje na žádné požadavky. Pokud chcete ověřit správné zadání clusteru, použijte příkaz jako `az sf cluster health`. Ověřte, že příkaz nevrací žádné chyby.

## <a name="basic-operations"></a>Základní operace

Informace o připojení ke clusteru se uchovávají napříč více relacemi Azure CLI. Po výběru clusteru Service Fabric na něm můžete spouštět jakékoli příkazy Service Fabric.

Pokud například chcete získat stav clusteru Service Fabric, použijte následující příkaz:

```azurecli
az sf cluster health
```

Výsledkem příkazu bude následující výstup (za předpokladu, že je v konfiguraci Azure CLI nastavený výstup ve formátu JSON):

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

## <a name="tips-and-troubleshooting"></a>Tipy a řešení potíží

Následující informace můžou být užitečné, pokud při používání příkazů Service Fabric v Azure CLI narazíte na problémy.

### <a name="convert-a-certificate-from-pfx-to-pem-format"></a>Převod certifikátu z formátu PFX na PEM

Azure CLI podporuje certifikáty na straně klienta v podobě souborů PEM (s příponou .pem). Pokud používáte soubory PFX ze systému Windows, musíte tyto certifikáty převést na formát PEM. K převodu souboru PFX na soubor PEM použijte následující příkaz:

```bash
openssl pkcs12 -in certificate.pfx -out mycert.pem -nodes
```

Další informace najdete v [dokumentace k OpenSSL](https://www.openssl.org/docs/).

### <a name="connection-issues"></a>Problémy s připojením

Některé operace můžou generovat následující zprávu:

`Failed to establish a new connection: [Errno 8] nodename nor servname provided, or not known`

Ověřte, že zadaný koncový bod clusteru je dostupný a naslouchá. Ověřte také, že je na daném hostiteli a portu dostupné uživatelské rozhraní Service Fabric Explorer. Pokud chcete aktualizovat koncový bod, použijte příkaz `az sf cluster select`.

### <a name="detailed-logs"></a>Podrobné protokoly

Podrobné protokoly jsou často užitečné při ladění nebo hlášení problému. Azure CLI nabízí globální příznak `--debug`, kterým se zvyšuje úroveň podrobností souborů protokolů.

### <a name="command-help-and-syntax"></a>Nápověda k příkazům a jejich syntaxe

Příkazy Service Fabric se řídí stejnými konvencemi jako Azure CLI. Pokud chcete získat nápovědu ke konkrétnímu příkazu nebo skupině příkazů, použijte příznak `-h`:

```azurecli
az sf application -h
```

Tady je další příklad:

```azurecli
az sf application create -h
```

