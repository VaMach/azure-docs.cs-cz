---
title: "Začínáme vytvářet internetový nástroj pro vyrovnávání zatížení v modelu nasazení Classic pomocí rozhraní příkazového řádku Azure | Dokumentace Microsoftu"
description: "Zjistěte, jak vytvořit internetový nástroj pro vyrovnávání zatížení v modelu nasazení Classic pomocí rozhraní příkazového řádku Azure"
services: load-balancer
documentationcenter: na
author: sdwheeler
manager: carmonm
editor: 
tags: azure-service-management
ms.assetid: e433a824-4a8a-44d2-8765-a74f52d4e584
ms.service: load-balancer
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/09/2016
ms.author: sewhee
translationtype: Human Translation
ms.sourcegitcommit: 2ea002938d69ad34aff421fa0eb753e449724a8f
ms.openlocfilehash: e8a346c1b2d430eceb4aa1b8bc94fbbe89394556


---
# <a name="get-started-creating-an-internet-facing-load-balancer-classic-in-the-azure-cli"></a>Začínáme vytvářet internetový nástroj pro vyrovnávání zatížení (Classic) v rozhraní příkazového řádku Azure
[!INCLUDE [load-balancer-get-started-internet-classic-selectors-include.md](../../includes/load-balancer-get-started-internet-classic-selectors-include.md)]

[!INCLUDE [load-balancer-get-started-internet-intro-include.md](../../includes/load-balancer-get-started-internet-intro-include.md)]

[!INCLUDE [azure-arm-classic-important-include](../../includes/azure-arm-classic-important-include.md)]

Tento článek se týká modelu nasazení Classic. Případně [zjistěte, jak vytvořit internetový nástroj pro vyrovnávání zatížení pomocí Azure Resource Manageru](load-balancer-get-started-internet-arm-ps.md).

[!INCLUDE [load-balancer-get-started-internet-scenario-include.md](../../includes/load-balancer-get-started-internet-scenario-include.md)]

## <a name="step-by-step-creating-an-internet-facing-load-balancer-using-cli"></a>Vytvoření internetového nástroje pro vyrovnávání zatížení pomocí rozhraní příkazového řádku krok za krokem
Tento průvodce ukazuje, jak vytvořit internetový nástroj pro vyrovnávání zatížení založený na výše uvedeném scénáři.

1. Pokud jste rozhraní příkazového řádku Azure nikdy nepoužívali, přejděte na téma [Instalace a konfigurace rozhraní příkazového řádku Azure](../xplat-cli-install.md) a postupujte podle pokynů až do chvíle, kdy můžete vybrat svůj účet a předplatné Azure.
2. Spuštěním příkazu **azure config mode** přejděte do režimu Classic, jak vidíte níže.
   
        azure config mode asm
   
    Očekávaný výstup:
   
        info:    New mode is asm

## <a name="create-endpoint-and-load-balancer-set"></a>Vytvoření koncového bodu a sady nástroje pro vyrovnávání zatížení
Tento scénář předpokládá, že byly vytvořeny virtuální počítače web1 a web2.
Tento průvodce vytvoří sadu nástroje pro vyrovnávání zatížení používající port 80 jako veřejný port a port 80 jako místní port. Port testu je také nakonfigurován na port 80 a název sady nástroje pro vyrovnávání zatížení je lbset.

### <a name="step-1"></a>Krok 1
Vytvořte první koncový bod a sadu nástroje pro vyrovnávání zatížení pro virtuální počítač web1 pomocí příkazu `azure network vm endpoint create`.

    azure vm endpoint create web1 80 -k 80 -o tcp -t 80 -b lbset

Použité parametry:

**-k** – port místního virtuálního počítače<br>
**-o** – protokol<BR>
**-t** – port testu<BR>
**-b** – název nástroje pro vyrovnávání zatížení<BR>

## <a name="step-2"></a>Krok 2
Přidejte do sady nástroje pro vyrovnávání zatížení druhý virtuální počítač web2.

    azure vm endpoint create web2 80 -k 80 -o tcp -t 80 -b lbset

## <a name="step-3"></a>Krok 3
Ověřte konfiguraci nástroje pro vyrovnávání zatížení pomocí příkazu `azure vm show`.

    azure vm show web1

Výstup bude:

    data:    DNSName "contoso.cloudapp.net"
    data:    Location "East US"
    data:    VMName "web1"
    data:    IPAddress "10.0.0.5"
    data:    InstanceStatus "ReadyRole"
    data:    InstanceSize "Standard_D1"
    data:    Image "a699494373c04fc0bc8f2bb1389d6106__Windows-Server-2012-R2-2015
    6-en.us-127GB.vhd"
    data:    OSDisk hostCaching "ReadWrite"
    data:    OSDisk name "joaoma-1-web1-0-201509251804250879"
    data:    OSDisk mediaLink "https://XXXXXXXXXXXXXXX.blob.core.windows.
    /vhds/joaomatest-web1-2015-09-25.vhd"
    data:    OSDisk sourceImageName "a699494373c04fc0bc8f2bb1389d6106__Windows-Se
    r-2012-R2-20150916-en.us-127GB.vhd"
    data:    OSDisk operatingSystem "Windows"
    data:    OSDisk iOType "Standard"
    data:    ReservedIPName ""
    data:    VirtualIPAddresses 0 address "XXXXXXXXXXXXXXXX"
    data:    VirtualIPAddresses 0 name "XXXXXXXXXXXXXXXXXXXX"
    data:    VirtualIPAddresses 0 isDnsProgrammed true
    data:    Network Endpoints 0 loadBalancedEndpointSetName "lbset"
    data:    Network Endpoints 0 localPort 80
    data:    Network Endpoints 0 name "tcp-80-80"
    data:    Network Endpoints 0 port 80
    data:    Network Endpoints 0 loadBalancerProbe port 80
    data:    Network Endpoints 0 loadBalancerProbe protocol "tcp"
    data:    Network Endpoints 0 loadBalancerProbe intervalInSeconds 15
    data:    Network Endpoints 0 loadBalancerProbe timeoutInSeconds 31
    data:    Network Endpoints 0 protocol "tcp"
    data:    Network Endpoints 0 virtualIPAddress "XXXXXXXXXXXX"
    data:    Network Endpoints 0 enableDirectServerReturn false
    data:    Network Endpoints 1 localPort 5986
    data:    Network Endpoints 1 name "PowerShell"
    data:    Network Endpoints 1 port 5986
    data:    Network Endpoints 1 protocol "tcp"
    data:    Network Endpoints 1 virtualIPAddress "XXXXXXXXXXXX"
    data:    Network Endpoints 1 enableDirectServerReturn false
    data:    Network Endpoints 2 localPort 3389
    data:    Network Endpoints 2 name "Remote Desktop"
    data:    Network Endpoints 2 port 58081
    info:    vm show command OK

## <a name="create-a-remote-desktop-endpoint-for-a-virtual-machine"></a>Vytvoření koncového bodu vzdálené plochy pro virtuální počítač
Koncový bod vzdálené plochy k přesměrování síťového provozu z veřejného portu na místní port konkrétního virtuálního počítače můžete vytvořit pomocí příkazu `azure vm endpoint create`.

    azure vm endpoint create web1 54580 -k 3389


## <a name="remove-virtual-machine-from-load-balancer"></a>Odebrání virtuálního počítače z nástroje pro vyrovnávání zatížení
Je třeba odstranit koncový bod přidružený k sadě nástroje pro vyrovnávání zatížení z virtuálního počítače. Po odebrání koncového bodu již virtuální počítač nebude patřit do sady nástroje pro vyrovnávání zatížení.

 Když použijeme výše uvedený příklad, koncový bod vytvořený pro virtuální počítač web1 můžete odebrat z nástroje pro vyrovnávání zatížení lbset pomocí příkazu `azure vm endpoint delete`.

    azure vm endpoint delete web1 tcp-80-80


> [!NOTE]
> Další možnosti správy koncových bodů můžete prozkoumat pomocí příkazu `azure vm endpoint --help`.
> 
> 

## <a name="next-steps"></a>Další kroky
[Začínáme s konfigurací interního nástroje pro vyrovnávání zatížení](load-balancer-get-started-ilb-arm-ps.md)

[Konfigurace distribučního režimu nástroje pro vyrovnávání zatížení](load-balancer-distribution-mode.md)

[Konfigurace nastavení časového limitu nečinnosti protokolu TCP pro nástroj pro vyrovnávání zatížení](load-balancer-tcp-idle-timeout.md)




<!--HONumber=Nov16_HO2-->


