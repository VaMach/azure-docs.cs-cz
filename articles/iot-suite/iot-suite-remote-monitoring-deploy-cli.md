---
title: "Nasazení řešení – Azure vzdáleného sledování Java | Microsoft Docs"
description: "V tomto kurzu se dozvíte, jak zřídit předkonfigurovaného řešení vzdáleného monitorování pomocí rozhraní příkazového řádku."
services: 
suite: iot-suite
author: dominicbetts
manager: timlt
ms.author: dobett
ms.service: iot-suite
ms.date: 01/29/2018
ms.topic: article
ms.devlang: NA
ms.tgt_pltfrm: NA
ms.workload: NA
ms.openlocfilehash: 94c3db3286623264e9df7873962d10dd5cc662d4
ms.sourcegitcommit: 9d317dabf4a5cca13308c50a10349af0e72e1b7e
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 02/01/2018
---
# <a name="deploy-the-remote-monitoring-preconfigured-solution-using-the-cli"></a>Nasazení předkonfigurovaného řešení vzdáleného monitorování pomocí rozhraní příkazového řádku

V tomto kurzu se dozvíte, jak zřídit předkonfigurované řešení vzdáleného monitorování. Nasazení řešení pomocí rozhraní příkazového řádku. Můžete taky nasadit řešení pomocí rozhraní založené na webu na azureiotsuite.com, další informace o této možnosti najdete [nasadit předkonfigurované řešení vzdáleného monitorování](iot-suite-remote-monitoring-deploy.md).

## <a name="prerequisites"></a>Požadavky

Chcete-li nasadit předkonfigurované řešení vzdáleného monitorování, potřebujete aktivní předplatné Azure.

Pokud nemáte účet, můžete si během několika minut vytvořit bezplatný účet zkušební. Podrobnosti najdete v článku [Bezplatná zkušební verze Azure](http://azure.microsoft.com/pricing/free-trial/).

Chcete-li spustit rozhraní příkazového řádku, musíte [Node.js](https://nodejs.org/) nainstalována na místním počítači.

## <a name="install-the-cli"></a>Instalace rozhraní příkazového řádku

Pokud chcete nainstalovat rozhraní příkazového řádku, spusťte následující příkaz v prostředí příkazového řádku:

```cmd/sh
npm install iot-solutions -g
```

## <a name="sign-in-to-the-cli"></a>Přihlaste se k rozhraní příkazového řádku

Než bude možné nasadit předkonfigurované řešení, musíte se přihlásit k vašemu předplatnému Azure pomocí rozhraní příkazového řádku takto:

```cmd/sh
pcs login
```

Postupujte podle na obrazovce pokyny k dokončení procesu přihlášení.

## <a name="deployment-options"></a>Možnosti nasazení

Když nasadíte předkonfigurované řešení, máte několik možností, které konfigurují procesu nasazení:

| Možnost | Hodnoty | Popis |
| ------ | ------ | ----------- |
| Skladová jednotka (SKU)    | `basic`, `standard`, `local` | A _základní_ nasazení je určený pro testovací a ukázky, nasazování všechny mikroslužeb do jediného virtuálního počítače. A _standardní_ nasazení je určeno v produkčním prostředí se nasadí mikroslužeb do více virtuálních počítačů. A _místní_ nasazení nakonfiguruje kontejner Docker ke spuštění mikroslužeb na místním počítači a používá služby Azure, jako je například úložiště a Cosmos databáze, v cloudu. |
| Modul runtime | `dotnet`, `java` | Vybere jazyk provádění mikroslužeb. |

Další informace o tom, jak použít místní nasazení najdete v tématu [místního spuštění řešení vzdáleného monitorování](https://github.com/Azure/azure-iot-pcs-remote-monitoring-dotnet/wiki/Running-the-Remote-Monitoring-Solution-Locally#deploy-azure-services-and-set-environment-variables).

## <a name="deploy-the-preconfigured-solution"></a>Nasadit předkonfigurované řešení

### <a name="example-deploy-net-version"></a>Příklad: nasazení verze rozhraní .NET

Následující příklad ukazuje, jak nasadit základní, rozhraní .NET verze předkonfigurovaného řešení vzdáleného monitorování:

```cmd/sh
pcs -t remotemonitoring -s basic -r dotnet
```

### <a name="example-deploy-java-version"></a>Příklad: nasazení verzi Javy

Následující příklad ukazuje, jak nasadit standard Java verze předkonfigurovaného řešení vzdáleného monitorování:

```cmd/sh
pcs -t remotemonitoring -s standard -r java
```

### <a name="pcs-command-options"></a>Parametry příkazu počítače

Při spuštění `pcs` příkazu Nasaďte řešení, budete vyzváni k:

- Název pro vaše řešení. Tento název musí být jedinečný.
- Předplatné Azure, které se má použít.
- Umístění.
- Přihlašovací údaje pro virtuální počítače, které hostují mikroslužeb. Tyto přihlašovací údaje můžete použít k přístupu k virtuálním počítačům pro řešení potíží.

Když `pcs` příkaz skončí, zobrazí adresu URL nového nasazení předkonfigurované řešení. `pcs` Příkaz také vytvoří soubor `{deployment-name}-output.json` s další informace, jako je název služby IoT Hub, který byl pro vás zřízený.

Další informace o parametrech příkazového řádku spusťte příkaz:

```cmd/sh
pcs -h
```

Další informace o rozhraní příkazového řádku najdete v tématu [použití rozhraní příkazového řádku](https://github.com/Azure/pcs-cli/blob/master/README.md).

## <a name="next-steps"></a>Další postup

V tomto kurzu jste se naučili:

> [!div class="checklist"]
> * Konfigurace předkonfigurované řešení
> * Nasadit předkonfigurované řešení
> * Přihlaste se k předkonfigurované řešení

Teď, když jste nasadili řešení vzdáleného monitorování, dalším krokem je [prozkoumejte možnosti řídicí panel řešení](./iot-suite-remote-monitoring-deploy.md).

<!-- Next tutorials in the sequence -->