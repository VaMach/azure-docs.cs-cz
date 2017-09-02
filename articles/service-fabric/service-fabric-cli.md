---
title: "Začínáme s Azure Service Fabric CLI (sfctl)"
description: "Naučte se používat Azure Service Fabric CLI. Zjistěte, jak se připojit ke clusteru a jak spravovat aplikace."
services: service-fabric
author: samedder
manager: timlt
ms.service: service-fabric
ms.topic: get-started-article
ms.date: 08/22/2017
ms.author: edwardsa
ms.translationtype: HT
ms.sourcegitcommit: 25e4506cc2331ee016b8b365c2e1677424cf4992
ms.openlocfilehash: 5ce9adf6c82e3a5521883c5de1e0689d5bf0d94e
ms.contentlocale: cs-cz
ms.lasthandoff: 08/24/2017

---
# <a name="azure-service-fabric-command-line"></a>Příkazový řádek Azure Service Fabric

Azure Service Fabric CLI (sfctl) je nástroj příkazového řádku pro práci s entitami Azure Service Fabric a jejich správu. Sfctl lze použít s clustery systému Windows, nebo Linux. Sfctl běží na libovolné platformě, kde je podporován python.

## <a name="prerequisites"></a>Požadavky

Před instalací se ujistěte, že ve vašem prostředí je nainstalovaný python a pip. Další informace najdete v [úvodní dokumentaci nástroje pip](https://pip.pypa.io/en/latest/quickstart/) a oficiální [instalační dokumentaci pro python](https://wiki.python.org/moin/BeginnersGuide/Download).

I když je podporovaná verze pythonu 2.7 i 3.6, doporučuje se používat python 3.6.

## <a name="install"></a>Instalace

Nástroj Azure Service Fabric CLI (sfctl) je zabalený jako balíček pythonu. Chcete-li nainstalovat nejnovější verzi, spusťte:

```bash
pip install sfctl
```

Po instalaci spusťte `sfctl -h`, abyste získali informace o dostupných příkazech.

## <a name="cli-syntax"></a>Syntaxe rozhraní příkazového řádku

Příkazy mají vždy předponu `sfctl`. Obecné informace o všech příkazech, které můžete použít, získáte zadáním `sfctl -h`. Nápovědu ke konkrétnímu příkazu získáte pomocí příkazu `sfctl <command> -h`.

Příkazy dodržují opakovatelnou strukturu, kdy cíl příkazu předchází operaci nebo akci:

```azurecli
sfctl <object> <action>
```

V tomto příkladu je `<object>` cílem pro `<action>`.

## <a name="select-a-cluster"></a>Výběr clusteru

Před provedením jakékoli operace musíte vybrat cluster, ke kterému se připojíte. Například spuštěním následujícího příkazu vyberete cluster s názvem `testcluster.com` a připojíte se k němu.

> [!WARNING]
> Nepoužívejte nezabezpečené clustery Service Fabric v produkčním prostředí.

```azurecli
sfctl cluster select --endpoint http://testcluster.com:19080
```

Koncový bod clusteru musí mít předponu `http` nebo `https`. Musí zahrnovat port pro bránu HTTP. Tento port a adresa jsou stejné jako adresa URL nástroje Service Fabric Explorer.

Pro clustery, které jsou zabezpečené pomocí certifikátu, můžete určit certifikát kódovaný PEM. Certifikát lze zadat jako jeden soubor, nebo jako dvojici certifikátu a klíče.

```azurecli
sfctl cluster select --endpoint https://testsecurecluster.com:19080 --pem ./client.pem
```

Další informace najdete v tématu [Připojení k zabezpečenému clusteru Azure Service Fabric](service-fabric-connect-to-secure-cluster.md).

## <a name="basic-operations"></a>Základní operace

Informace o připojení ke clusteru se uchovávají napříč více relacemi sfctl. Po výběru clusteru Service Fabric na něm můžete spouštět jakékoli příkazy Service Fabric.

Pokud například chcete získat stav clusteru Service Fabric, použijte následující příkaz:

```azurecli
sfctl cluster health
```

Příkaz vrátí následující výstup:

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

Zde jsou některé návrhy a tipy pro řešení běžných problémů.

### <a name="convert-a-certificate-from-pfx-to-pem-format"></a>Převod certifikátu z formátu PFX na PEM

Service Fabric CLI podporuje certifikáty na straně klienta v podobě souborů PEM (s příponou .pem). Pokud používáte soubory PFX ze systému Windows, musíte tyto certifikáty převést na formát PEM. K převodu souboru PFX na soubor PEM použijte následující příkaz:

```bash
openssl pkcs12 -in certificate.pfx -out mycert.pem -nodes
```

Další informace najdete v [dokumentace k OpenSSL](https://www.openssl.org/docs/).

### <a name="connection-issues"></a>Problémy s připojením

Některé operace můžou generovat následující zprávu:

`Failed to establish a new connection: [Errno 8] nodename nor servname provided, or not known`

Ověřte, že zadaný koncový bod clusteru je dostupný a naslouchá. Ověřte také, že je na daném hostiteli a portu dostupné uživatelské rozhraní Service Fabric Explorer. Pokud chcete aktualizovat koncový bod, použijte příkaz `sfctl cluster select`.

### <a name="detailed-logs"></a>Podrobné protokoly

Podrobné protokoly jsou často užitečné při ladění nebo hlášení problému. Existuje globální příznak `--debug`, kterým se zvyšuje úroveň podrobností souborů protokolů.

### <a name="command-help-and-syntax"></a>Nápověda k příkazům a jejich syntaxe

Pokud chcete získat nápovědu ke konkrétnímu příkazu nebo skupině příkazů, použijte příznak `-h`:

```azurecli
sfctl application -h
```

Další příklad:

```azurecli
sfctl application create -h
```

## <a name="next-steps"></a>Další kroky

* [Nasazení aplikace pomocí Azure Service Fabric CLI](service-fabric-application-lifecycle-sfctl.md)
* [Začínáme se Service Fabric v Linuxu](service-fabric-get-started-linux.md)

