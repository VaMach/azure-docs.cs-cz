---
title: "Pomocí rozšíření CustomScript na virtuální počítač s Linuxem | Microsoft Docs"
description: "Další informace o použití rozšíření CustomScript k nasazení aplikací na virtuální počítače s Linuxem v Azure vytvořit pomocí modelu nasazení classic."
editor: tysonn
manager: timlt
documentationcenter: 
services: virtual-machines-linux
author: gbowerman
tags: azure-service-management
ms.assetid: e535241d-feca-4412-b07a-67c936ba88a0
ms.service: virtual-machines-linux
ms.workload: multiple
ms.tgt_pltfrm: linux
ms.devlang: na
ms.topic: article
ms.date: 06/01/2017
ms.author: guybo
ms.openlocfilehash: cb1fc9a44dc9e57d9cc9f1c546ad937d67e63c2f
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/11/2017
---
# <a name="deploy-a-lamp-app-using-the-azure-customscript-extension-for-linux"></a>Nasazení aplikace LAMP pomocí rozšíření Azure CustomScript pro Linux
> [!IMPORTANT] 
> Azure má dva různé modely nasazení pro vytváření a práci s prostředky: [Resource Manager a klasický](../../../resource-manager-deployment-model.md). Tento článek se zabývá pomocí modelu nasazení Classic. Microsoft doporučuje, aby byl ve většině nových nasazení použit model Resource Manager. Informace o nasazení svítilna zásobníku pomocí modelu Resource Manager najdete v tématu [zde](../tutorial-lamp-stack.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

Rozšíření CustomScript Microsoft Azure pro Linux poskytuje způsob, jak přizpůsobit virtuálních počítačů (VM) spuštěním libovolný kód napsaný v jakékoli skriptovací jazyk, který nepodporuje virtuální počítač (například Python a Bash). To poskytuje flexibilní způsob k automatizaci nasazení aplikace do více počítačů.

Můžete nasadit rozšíření CustomScript pomocí portálu Azure, prostředí Windows PowerShell nebo rozhraní příkazového řádku Azure (Azure CLI).

V tomto článku, který použijeme rozhraní příkazového řádku Azure k nasazení jednoduché aplikace svítilna do virtuálního počítače s Ubuntu vytvořit pomocí modelu nasazení classic.

## <a name="prerequisites"></a>Požadavky
V tomto příkladu nejdřív vytvořte dva virtuální počítače Azure s Ubuntu 14.04 nebo novějším. Virtuální počítače se označují jako *skriptu vm* a *svítilna vm*. Při vytváření virtuálních počítačů, použijte jedinečné názvy. Jeden slouží ke spuštění příkazů rozhraní příkazového řádku a jeden slouží k nasazení aplikace svítilna.

Musíte taky účtu Azure Storage a klíč pro přístup k ní (získáte to z portálu Azure).

Pokud potřebujete pomoc, vytváření virtuální počítače s Linuxem v Azure naleznete [vytvořit virtuálním počítači systémem Linux](createportal.md).

Instalace příkazů se předpokládá, Ubuntu, ale můžete přizpůsobit instalace pro všechny podporované distro Linux.

Virtuální počítač skriptu vm musí mít nainstalovaný, má funkční připojení k Azure rozhraní příkazového řádku Azure. Pomoc s to naleznete [instalace a konfigurace rozhraní příkazového řádku Azure](../../../cli-install-nodejs.md).

## <a name="upload-a-script"></a>Odeslat skript
Pomocí rozšíření CustomScript spuštění skriptu na virtuálním počítači vzdálené instalace svítilna zásobníku a vytvoření stránky PHP. Chcete-li skript přístup odkudkoli jsme budete ho nahrát jako Azure blob.

### <a name="script-overview"></a>Přehled skriptů
Příklad skriptu nainstaluje svítilna zásobníku do Ubuntu (včetně nastavení bezobslužné instalace MySQL), zapíše jednoduchého souboru PHP a spustí Apache.

    #!/bin/bash
    # set up a silent install of MySQL
    dbpass="mySQLPassw0rd"

    export DEBIAN_FRONTEND=noninteractive
    echo mysql-server-5.6 mysql-server/root_password password $dbpass | debconf-set-selections
    echo mysql-server-5.6 mysql-server/root_password_again password $dbpass | debconf-set-selections

    # install the LAMP stack
    apt-get -y install apache2 mysql-server php5 php5-mysql  

    # write some PHP
    echo \<center\>\<h1\>My Demo App\</h1\>\<br/\>\</center\> > /var/www/html/phpinfo.php
    echo \<\?php phpinfo\(\)\; \?\> >> /var/www/html/phpinfo.php

    # restart Apache
    apachectl restart

### <a name="upload-script"></a>Nahrát skriptu
Uložte skript jako textový soubor, například *install_lamp.sh*a odešlete ji do úložiště Azure. Můžete provést snadno pomocí rozhraní příkazového řádku Azure. Následující příklad odešle soubor do kontejneru úložiště s názvem "skripty". Pokud kontejner neexistuje musíte nejprve vytvořit.

    azure storage blob upload -a <yourStorageAccountName> -k <yourStorageKey> --container scripts ./install_lamp.sh

Také vytvořte soubor JSON, který popisuje, jak se stáhnout skript z úložiště Azure. Uložit jako *public_config.json* (nahrazení "mystorage" s název účtu úložiště):

    {"fileUris":["https://mystorage.blob.core.windows.net/scripts/install_lamp.sh"], "commandToExecute":"sh install_lamp.sh" }


## <a name="deploy-the-extension"></a>Nasazení rozšíření
Teď můžete použít příkaz Další k nasazení rozšíření CustomScript Linux na vzdálený počítač pomocí rozhraní příkazového řádku Azure.

    azure vm extension set -c "./public_config.json" lamp-vm CustomScript Microsoft.Azure.Extensions 2.0

Předchozí příkaz stáhne a běží *install_lamp.sh* skriptu ve virtuálním počítači názvem *svítilna vm*.

Vzhledem k tomu, že aplikace zahrnuje webový server, musíte otevřít port naslouchání v protokolu HTTP na vzdálený počítač následujícím příkazem.

    azure vm endpoint create -n Apache -o tcp lamp-vm 80 80

## <a name="monitoring-and-troubleshooting"></a>Monitorování a řešení potíží
Můžete se podívat na tom, jak dobře vlastní skript se spustí prohlédnutím souboru protokolu na vzdálený počítač. SSH k *svítilna vm* a tail soubor protokolu následujícím příkazem.

    cd /var/log/azure/customscript
    tail -f handler.log

Po spuštění rozšíření CustomScript můžete procházet na stránku PHP, který jste vytvořili pro informace. Stránka PHP, například v tomto článku je *http://lamp-vm.cloudapp.net/phpinfo.php*.

## <a name="additional-resources"></a>Další zdroje
Stejný základní postup můžete použít k nasazení aplikace složitější. V tomto příkladu se instalační skript uložit jako veřejného objektu blob ve službě Azure Storage. Bezpečnější možnost může být jako zabezpečené objektů blob s úložiště instalační skript [zabezpečený přístup k podpisu](https://msdn.microsoft.com/library/azure/ee395415.aspx) (SAS).

Další zdroje pro rozhraní příkazového řádku Azure, Linux a rozšíření CustomScript jsou uvedeny dále.

[Automatizovat úkoly vlastního nastavení virtuálního počítače s Linuxem pomocí rozšíření CustomScript](https://azure.microsoft.com/blog/2014/08/20/automate-linux-vm-customization-tasks-using-customscript-extension/)

[Rozšíření Azure Linux (Githubu)](https://github.com/Azure/azure-linux-extensions)