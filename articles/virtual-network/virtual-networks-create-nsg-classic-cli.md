---
title: "Postup vytvoření skupin Nsg v klasickém režimu pomocí rozhraní příkazového řádku Azure | Microsoft Docs"
description: "Postup vytvoření a nasazení skupin Nsg v klasickém režimu pomocí rozhraní příkazového řádku Azure"
services: virtual-network
documentationcenter: na
author: jimdial
manager: carmonm
editor: tysonn
tags: azure-service-management
ms.assetid: 17d98950-5fbb-4653-bef6-d822ab37541e
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/02/2016
ms.author: jdial
ms.openlocfilehash: 115a1937a4c88ba2b986a40c84b1b759ed5e03b5
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="how-to-create-nsgs-classic-in-the-azure-cli"></a>Vytvoření skupiny zabezpečení sítě (klasické) v rozhraní příkazového řádku Azure
[!INCLUDE [virtual-networks-create-nsg-selectors-classic-include](../../includes/virtual-networks-create-nsg-selectors-classic-include.md)]

[!INCLUDE [virtual-networks-create-nsg-intro-include](../../includes/virtual-networks-create-nsg-intro-include.md)]

[!INCLUDE [azure-arm-classic-important-include](../../includes/azure-arm-classic-important-include.md)]

Tento článek se týká modelu nasazení Classic. Můžete také [vytvářet skupiny Nsg v modelu nasazení Resource Manager](virtual-networks-create-nsg-arm-cli.md).

[!INCLUDE [virtual-networks-create-nsg-scenario-include](../../includes/virtual-networks-create-nsg-scenario-include.md)]

Níže uvedené příkazy rozhraní příkazového řádku Azure ukázka očekávat jednoduché prostředí již vytvořeny podle výše uvedené scénáře. Pokud chcete ke spuštění příkazů, jak jsou zobrazeny v tomto dokumentu, nejprve vytvořit testovací prostředí podle [vytvoření virtuální sítě](virtual-networks-create-vnet-classic-cli.md).

## <a name="how-to-create-the-nsg-for-the-front-end-subnet"></a>Postup vytvoření skupina NSG pro podsítě front end
Chcete-li vytvořit skupinu NSG s názvem s názvem **NSG front-endu** závislosti na scénáři výše, použijte následující postup.

1. Pokud jste rozhraní příkazového řádku Azure nikdy nepoužívali, přejděte na téma [Instalace a konfigurace rozhraní příkazového řádku Azure](../cli-install-nodejs.md) a postupujte podle pokynů až do chvíle, kdy můžete vybrat svůj účet a předplatné Azure.
2. Spustit  **`azure config mode`**  příkaz Přepnout do klasického režimu, jak je uvedeno níže.
   
        azure config mode asm
   
    Očekávaný výstup:
   
        info:    New mode is asm
3. Spustit  **`azure network nsg create`**  příkazu vytvořte skupinu NSG.
   
        azure network nsg create -l uswest -n NSG-FrontEnd
   
    Očekávaný výstup:
   
        info:    Executing command network nsg create
        info:    Creating a network security group "NSG-FrontEnd"
        info:    Looking up the network security group "NSG-FrontEnd"
        data:    Name                            : NSG-FrontEnd
        data:    Location                        : West US
        data:    Security group rules:
        data:    Name                               Source IP           Source Port  Destination IP   Destination Port  Protocol  Type      Action  Prior
        ity  Default
        data:    ---------------------------------  ------------------  -----------  ---------------  ----------------  --------  --------  ------  -----
        ---  -------
        data:    ALLOW VNET OUTBOUND                VIRTUAL_NETWORK     *            VIRTUAL_NETWORK  *                 *         Outbound  Allow   65000
             true   
        data:    ALLOW VNET INBOUND                 VIRTUAL_NETWORK     *            VIRTUAL_NETWORK  *                 *         Inbound   Allow   65000
             true   
        data:    ALLOW AZURE LOAD BALANCER INBOUND  AZURE_LOADBALANCER  *            *                *                 *         Inbound   Allow   65001
             true   
        data:    ALLOW INTERNET OUTBOUND            *                   *            INTERNET         *                 *         Outbound  Allow   65001
             true   
        data:    DENY ALL OUTBOUND                  *                   *            *                *                 *         Outbound  Deny    65500
             true   
        data:    DENY ALL INBOUND                   *                   *            *                *                 *         Inbound   Deny    65500
             true   
        info:    network nsg create command OK
   
    Parametry:
   
   * **-l (nebo --location)**. Oblast Azure, kde bude vytvořena nová skupina NSG. Pro náš scénář *westus*.
   * **-n (nebo --name)**. Název nové skupiny NSG. Pro náš scénář *NSG front-endu*.
4. Spustit  **`azure network nsg rule create`**  příkaz k vytvoření pravidla, která umožňuje přístup k portu 3389 (RDP) z Internetu.
   
        azure network nsg rule create -a NSG-FrontEnd -n rdp-rule -c Allow -p Tcp -r Inbound -y 100 -f Internet -o * -e * -u 3389
   
    Očekávaný výstup:
   
        info:    Executing command network nsg rule create
        info:    Looking up the network security group "NSG-FrontEnd"
        info:    Creating a network security rule "rdp-rule"
        info:    Looking up the network security group "NSG-FrontEnd"
        data:    Name                            : rdp-rule
        data:    Source address prefix           : INTERNET
        data:    Source Port                     : *
        data:    Destination address prefix      : *
        data:    Destination Port                : 3389
        data:    Protocol                        : TCP
        data:    Type                            : Inbound
        data:    Action                          : Allow
        data:    Priority                        : 100
        info:    network nsg rule create command OK
   
    Parametry:
   
   * **-a (nebo--nsg-name)**. Název skupiny NSG, ve kterém se vytvoří pravidlo. Pro náš scénář *NSG front-endu*.
   * **-n (nebo --name)**. Název pro nové pravidlo. Pro náš scénář *rdp pravidlo*.
   * **-c (nebo--akce)**. Úroveň přístupu pro pravidlo (zakázat nebo povolit).
   * **-p (nebo--protocol)**. Protokol (Tcp, Udp nebo *) pro pravidlo.
   * **-r (nebo--typu)**. Směr připojení (příchozí nebo odchozí).
   * **-y (nebo--priority)**. Priorita pravidla.
   * **-f (nebo--předpona zdrojové adresy)**. Předpona zdrojové adresy v CIDR nebo pomocí výchozí značky.
   * **-e (nebo--rozsah zdrojových portů)**. Zdrojový port, nebo rozsah portů.
   * **-e (nebo--předpona cílové adresy)**. Předpona cílové adresy v CIDR nebo pomocí výchozí značky.
   * **-u (nebo--rozsah cílových portů)**. Cílový port, nebo rozsah portů.
5. Spustit  **`azure network nsg rule create`**  příkaz k vytvoření pravidla, která umožňuje přístup k portu 80 (HTTP) z Internetu.
   
        azure network nsg rule create -a NSG-FrontEnd -n web-rule -c Allow -p Tcp -r Inbound -y 200 -f Internet -o * -e * -u 80
   
    Očekávaný putput:
   
        info:    Executing command network nsg rule create
        info:    Looking up the network security group "NSG-FrontEnd"
        info:    Creating a network security rule "web-rule"
        info:    Looking up the network security group "NSG-FrontEnd"
        data:    Name                            : web-rule
        data:    Source address prefix           : INTERNET
        data:    Source Port                     : *
        data:    Destination address prefix      : *
        data:    Destination Port                : 80
        data:    Protocol                        : TCP
        data:    Type                            : Inbound
        data:    Action                          : Allow
        data:    Priority                        : 200
        info:    network nsg rule create command OK
6. Spustit  **`azure network nsg subnet add`**  příkaz propojení NSG na podsítě front end.
   
        azure network nsg subnet add -a NSG-FrontEnd --vnet-name TestVNet --subnet-name FrontEnd
   
    Očekávaný výstup:
   
        info:    Executing command network nsg subnet add
        info:    Looking up the network security group "NSG-FrontEnd"
        info:    Looking up the subnet "FrontEnd"
        info:    Looking up network configuration
        info:    Creating a network security group "NSG-FrontEnd"
        info:    network nsg subnet add command OK

## <a name="how-to-create-the-nsg-for-the-back-end-subnet"></a>Postup vytvoření skupina NSG pro podsíť back-end
Chcete-li vytvořit skupinu NSG s názvem s názvem *NSG back-end* závislosti na scénáři výše, použijte následující postup.

1. Spustit  **`azure network nsg create`**  příkazu vytvořte skupinu NSG.
   
        azure network nsg create -l uswest -n NSG-BackEnd
   
    Očekávaný výstup:
   
        info:    Executing command network nsg create
        info:    Creating a network security group "NSG-BackEnd"
        info:    Looking up the network security group "NSG-BackEnd"
        data:    Name                            : NSG-BackEnd
        data:    Location                        : West US
        data:    Security group rules:
        data:    Name                               Source IP           Source Port  Destination IP   Destination Port  Protocol  Type      Action  Prior
        ity  Default
        data:    ---------------------------------  ------------------  -----------  ---------------  ----------------  --------  --------  ------  -----
        ---  -------
        data:    ALLOW VNET OUTBOUND                VIRTUAL_NETWORK     *            VIRTUAL_NETWORK  *                 *         Outbound  Allow   65000
             true   
        data:    ALLOW VNET INBOUND                 VIRTUAL_NETWORK     *            VIRTUAL_NETWORK  *                 *         Inbound   Allow   65000
             true   
        data:    ALLOW AZURE LOAD BALANCER INBOUND  AZURE_LOADBALANCER  *            *                *                 *         Inbound   Allow   65001
             true   
        data:    ALLOW INTERNET OUTBOUND            *                   *            INTERNET         *                 *         Outbound  Allow   65001
             true   
        data:    DENY ALL OUTBOUND                  *                   *            *                *                 *         Outbound  Deny    65500
             true   
        data:    DENY ALL INBOUND                   *                   *            *                *                 *         Inbound   Deny    65500
             true   
        info:    network nsg create command OK
   
    Parametry:
   
   * **-l (nebo --location)**. Oblast Azure, kde bude vytvořena nová skupina NSG. Pro náš scénář *westus*.
   * **-n (nebo --name)**. Název nové skupiny NSG. Pro náš scénář *NSG front-endu*.
2. Spustit  **`azure network nsg rule create`**  příkaz k vytvoření pravidla, která umožňuje přístup k portu 1433 (SQL) z podsítě front end.
   
        azure network nsg rule create -a NSG-BackEnd -n sql-rule -c Allow -p Tcp -r Inbound -y 100 -f 192.168.1.0/24 -o * -e * -u 1433
   
    Očekávaný výstup:
   
        info:    Executing command network nsg rule create
        info:    Looking up the network security group "NSG-BackEnd"
        info:    Creating a network security rule "sql-rule"
        info:    Looking up the network security group "NSG-BackEnd"
        data:    Name                            : sql-rule
        data:    Source address prefix           : 192.168.1.0/24
        data:    Source Port                     : *
        data:    Destination address prefix      : *
        data:    Destination Port                : 1433
        data:    Protocol                        : TCP
        data:    Type                            : Inbound
        data:    Action                          : Allow
        data:    Priority                        : 100
        info:    network nsg rule create command OK
3. Spustit  **`azure network nsg rule create`**  příkaz pro vytvoření pravidla, které odepře přístup k Internetu.
   
        azure network nsg rule create -a NSG-BackEnd -n web-rule -c Deny -p Tcp -r Outbound -y 200 -f * -o * -e Internet -u 80
   
    Očekávaný putput:
   
        info:    Executing command network nsg rule create
        info:    Looking up the network security group "NSG-BackEnd"
        info:    Creating a network security rule "web-rule"
        info:    Looking up the network security group "NSG-BackEnd"
        data:    Name                            : web-rule
        data:    Source address prefix           : *
        data:    Source Port                     : *
        data:    Destination address prefix      : INTERNET
        data:    Destination Port                : 80
        data:    Protocol                        : TCP
        data:    Type                            : Outbound
        data:    Action                          : Deny
        data:    Priority                        : 200
        info:    network nsg rule create command OK
4. Spustit  **`azure network nsg subnet add`**  příkaz propojení NSG k podsíti back-end.
   
        azure network nsg subnet add -a NSG-BackEnd --vnet-name TestVNet --subnet-name BackEnd
   
    Očekávaný výstup:
   
        info:    Executing command network nsg subnet add
        info:    Looking up the network security group "NSG-BackEndX"
        info:    Looking up the subnet "BackEnd"
        info:    Looking up network configuration
        info:    Creating a network security group "NSG-BackEndX"
        info:    network nsg subnet add command OK

