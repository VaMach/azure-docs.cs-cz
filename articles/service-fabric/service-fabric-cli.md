---
title: "Začínáme s Azure Service Fabric CLI"
description: "Naučte se používat Azure Service Fabric CLI. Zjistěte, jak se připojit ke clusteru a jak spravovat aplikace."
services: service-fabric
author: samedder
manager: timlt
ms.service: service-fabric
ms.topic: get-started-article
ms.date: 08/22/2017
ms.author: edwardsa
ms.translationtype: HT
ms.sourcegitcommit: fda37c1cb0b66a8adb989473f627405ede36ab76
ms.openlocfilehash: 851f04c8b5eee762ec43060f02c8b83f00c1782e
ms.contentlocale: cs-cz
ms.lasthandoff: 09/14/2017

---
# <a name="azure-service-fabric-cli"></a>Azure Service Fabric CLI

Rozhraní příkazového řádku (CLI) Azure Service Fabric je nástroj příkazového řádku pro práci s entitami Service Fabric a jejich správu. Service Fabric CLI lze použít s clustery s Windows nebo Linuxem. Service Fabric CLI funguje na libovolné platformě, kde je podporován Python.

## <a name="prerequisites"></a>Požadavky

Před instalací se ujistěte, že je ve vašem prostředí nainstalovaný Python a pip. Další informace najdete v [úvodní dokumentaci nástroje pip](https://pip.pypa.io/en/latest/quickstart/) a oficiální [dokumentaci k instalaci Pythonu](https://wiki.python.org/moin/BeginnersGuide/Download).

I když je podporovaný Python 2.7 i 3.6, doporučujeme používat Python 3.6. Následující část popisuje, jak nainstalovat všechny požadované součásti a rozhraní příkazového řádku.

## <a name="install-pip-python-and-the-service-fabric-cli"></a>Instalace nástroje pip, Pythonu a Service Fabric CLI

 Na své platformě můžete pip a Python nainstalovat mnoha způsoby. Tady je několik kroků, pomocí kterých můžete rychle nastavit Python 3.6 a pip na hlavních operačních systémech.

### <a name="windows"></a>Windows

Ve Windows 10, Windows Server 2016 a Windows Server 2012 R2 použijte standardní oficiální pokyny k instalaci. Instalační program Pythonu ve výchozím nastavení nainstaluje také pip.

1. Přejděte na oficiální [stránku pro stažení Pythonu](https://www.python.org/downloads/) a stáhněte nejnovější vydanou verzi Pythonu 3.6.

2. Spusťte instalační program.

3. V dolní části příkazového řádku vyberte **Add Python 3.6 to PATH** (Přidat Python 3.6 do CESTA).

4. Vyberte **Install Now** (Nainstalovat) a dokončete instalaci.

Teď můžete otevřít nové příkazové okno a získat verze Pythonu i nástroje pip.

```bat
python --version
pip --version
```

Potom spusťte následující příkaz a nainstalujte Service Fabric CLI:

```
pip install sfctl
sfctl -h
```

### <a name="ubuntu"></a>Ubuntu

V systému Ubuntu 16.04 Desktop můžete Python 3.6 nainstalovat pomocí osobního archivu balíčků (PPA) třetí strany.

Z terminálu spusťte následující příkazy:

```bash
sudo add-apt-repository ppa:jonathonf/python-3.6
sudo apt-get update
sudo apt-get install python3.6
sudo apt-get install python3-pip
```

Potom spusťte následující příkaz, pokud chcete nainstalovat Service Fabric CLI pouze pro vaši instalaci Pythonu 3.6:

```bash
python3.6 -m pip install sfctl
sfctl -h
```

Tyto kroky nemají vliv na systémovou instalaci Pythonu 3.5 a 2.7. Nepokoušejte se upravovat tyto instalace, pokud dobře neznáte Ubuntu.

### <a name="macos"></a>MacOS

V systému MacOS doporučujeme použít [správce balíčků HomeBrew](https://brew.sh). Pokud HomeBrew ještě není nainstalovaný, nainstalujte ho spuštěním následujícího příkazu:

```bash
/usr/bin/ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"
```

Potom z terminálu nainstalujte Python 3.6, pip a Service Fabric CLI spuštěním následujících příkazů:

```bash
brew install python3
pip3 install sfctl
sfctl -h
```

Tyto kroky neupravují systémovou instalaci Pythonu 2.7.

## <a name="cli-syntax"></a>Syntaxe rozhraní příkazového řádku

Příkazy mají vždy předponu `sfctl`. Obecné informace o všech příkazech, které můžete použít, získáte pomocí příkazu `sfctl -h`. Nápovědu ke konkrétnímu příkazu získáte pomocí příkazu `sfctl <command> -h`.

Příkazy dodržují opakovatelnou strukturu, kdy cíl příkazu předchází operaci nebo akci.

```azurecli
sfctl <object> <action>
```

V tomto příkladu je `<object>` cílem pro `<action>`.

## <a name="select-a-cluster"></a>Výběr clusteru

Před provedením jakékoli operace musíte vybrat cluster, ke kterému se připojíte. Pokud například chcete vybrat cluster `testcluster.com` a připojit se k němu, spusťte následující příkaz:

> [!WARNING]
> Nepoužívejte nezabezpečené clustery Service Fabric v produkčním prostředí.

```azurecli
sfctl cluster select --endpoint http://testcluster.com:19080
```

Koncový bod clusteru musí mít předponu `http` nebo `https`. Musí zahrnovat port pro bránu HTTP. Tento port a adresa jsou stejné jako adresa URL nástroje Service Fabric Explorer.

Pro clustery, které jsou zabezpečené pomocí certifikátu, můžete určit certifikát kódovaný PEM. Certifikát lze zadat jako jeden soubor nebo jako pár certifikátu a klíče.

```azurecli
sfctl cluster select --endpoint https://testsecurecluster.com:19080 --pem ./client.pem
```

Další informace najdete v tématu [Připojení k zabezpečenému clusteru Azure Service Fabric](service-fabric-connect-to-secure-cluster.md).

## <a name="basic-operations"></a>Základní operace

Informace o připojení ke clusteru se uchovávají napříč více relacemi Service Fabric CLI. Po výběru clusteru Service Fabric na něm můžete spouštět jakékoli příkazy Service Fabric.

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

Tady jsou některé návrhy a tipy pro řešení běžných problémů.

### <a name="convert-a-certificate-from-pfx-to-pem-format"></a>Převod certifikátu z formátu PFX na PEM

Service Fabric CLI podporuje certifikáty na straně klienta v podobě souborů PEM (s příponou .pem). Pokud používáte soubory PFX ze systému Windows, musíte tyto certifikáty převést na formát PEM. K převodu souboru PFX na soubor PEM použijte následující příkaz:

```bash
openssl pkcs12 -in certificate.pfx -out mycert.pem -nodes
```

Další informace najdete v [dokumentace k OpenSSL](https://www.openssl.org/docs/).

### <a name="connection-problems"></a>Problémy s připojením

Některé operace můžou generovat následující zprávu:

`Failed to establish a new connection: [Errno 8] nodename nor servname provided, or not known`

Ověřte, že zadaný koncový bod clusteru je dostupný a naslouchá. Ověřte také, že je na daném hostiteli a portu dostupné uživatelské rozhraní Service Fabric Explorer. Pokud chcete aktualizovat koncový bod, použijte příkaz `sfctl cluster select`.

### <a name="detailed-logs"></a>Podrobné protokoly

Podrobné protokoly jsou často užitečné při ladění nebo hlášení problému. Globální příznak `--debug` zvyšuje úroveň podrobností souborů protokolů.

### <a name="command-help-and-syntax"></a>Nápověda k příkazům a jejich syntaxe

Pokud chcete získat nápovědu ke konkrétnímu příkazu nebo skupině příkazů, použijte příznak `-h`.

```azurecli
sfctl application -h
```

Tady je další příklad:

```azurecli
sfctl application create -h
```

## <a name="next-steps"></a>Další kroky

* [Nasazení aplikace pomocí Azure Service Fabric CLI](service-fabric-application-lifecycle-sfctl.md)
* [Začínáme se Service Fabric v Linuxu](service-fabric-get-started-linux.md)

