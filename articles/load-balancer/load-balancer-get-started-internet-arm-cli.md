---
title: "Vytvoření internetového nástroje pro vyrovnávání zatížení – rozhraní příkazového řádku Azure | Dokumentace Microsoftu"
description: "Zjistěte, jak vytvořit internetový nástroj pro vyrovnávání zatížení v Resource Manageru pomocí rozhraní příkazového řádku Azure"
services: load-balancer
documentationcenter: na
author: kumudd
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: a8bcdd88-f94c-4537-8143-c710eaa86818
ms.service: load-balancer
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 01/23/2017
ms.author: kumud
translationtype: Human Translation
ms.sourcegitcommit: fd5960a4488f2ecd93ba117a7d775e78272cbffd
ms.openlocfilehash: 002fa917e0cc17c34e0e617a28f2e21ba9b7e35f

---
# <a name="creating-an-internet-load-balancer-using-the-azure-cli"></a>Vytvoření internetového nástroje pro vyrovnávání zatížení pomocí Azure CLI

> [!div class="op_single_selector"]
> * [Azure Portal](../load-balancer/load-balancer-get-started-internet-portal.md)
> * [PowerShell](../load-balancer/load-balancer-get-started-internet-arm-ps.md)
> * [Azure CLI](../load-balancer/load-balancer-get-started-internet-arm-cli.md)
> * [Šablona](../load-balancer/load-balancer-get-started-internet-arm-template.md)

[!INCLUDE [load-balancer-get-started-internet-intro-include.md](../../includes/load-balancer-get-started-internet-intro-include.md)]

[!INCLUDE [azure-arm-classic-important-include](../../includes/azure-arm-classic-important-include.md)]

Tento článek se týká modelu nasazení Resource Manager. Případně [zjistěte, jak vytvořit internetový nástroj pro vyrovnávání zatížení pomocí modelu nasazení Classic](load-balancer-get-started-internet-classic-portal.md).

[!INCLUDE [load-balancer-get-started-internet-scenario-include.md](../../includes/load-balancer-get-started-internet-scenario-include.md)]

## <a name="deploying-the-solution-using-the-azure-cli"></a>Nasazení řešení pomocí rozhraní příkazového řádku Azure

Následující postup ukazuje, jak vytvořit internetový nástroj pro vyrovnávání zatížení pomocí Azure Resource Manageru s rozhraním příkazového řádku. S Azure Resource Managerem se jednotlivé prostředky vytvoří a nakonfigurují zvlášť, následně se spojí dohromady a vytvoří prostředek.

Pokud chcete nasadit nástroj pro vyrovnávání zatížení, je nutné vytvořit a nakonfigurovat následující objekty:

* Konfigurace front-endových IP adres – obsahuje veřejné IP adresy pro příchozí síťový provoz.
* Back-endový fond adres – obsahuje síťová rozhraní, pomocí kterých virtuální počítače přijímají síťový provoz z nástroje pro vyrovnávání zatížení.
* Pravidla vyrovnávání zatížení – obsahuje pravidla mapující veřejný port v nástroji pro vyrovnávání zatížení na port v back-endovém fondu adres.
* Pravidla příchozího překladu adres (NAT) – obsahuje pravidla mapující veřejný port v nástroji pro vyrovnávání zatížení na port konkrétního virtuálního počítače v back-endovém fondu adres.
* Testy – obsahuje testy stavu sloužící ke kontrole dostupnosti instancí virtuálních počítačů v back-endovém fondu adres.

Další informace najdete v tématu [Podpora služby Load Balancer v Azure Resource Manageru](load-balancer-arm.md).

## <a name="set-up-cli-to-use-resource-manager"></a>Nastavení rozhraní příkazového řádku pro použití Resource Manageru

1. Pokud jste rozhraní příkazového řádku Azure nikdy nepoužívali, přejděte na téma [Instalace a konfigurace rozhraní příkazového řádku Azure](../xplat-cli-install.md) a postupujte podle pokynů až do chvíle, kdy můžete vybrat svůj účet a předplatné Azure.
2. Spuštěním příkazu **azure config mode** přejděte do režimu Resource Manager, jak vidíte níže.

    ```azurecli
        azure config mode arm
    ```

    Očekávaný výstup:

        info:    New mode is arm

## <a name="create-a-virtual-network-and-a-public-ip-address-for-the-front-end-ip-pool"></a>Vytvoření virtuální sítě a veřejné IP adresy pro front-endový fond IP adres

1. Vytvořte virtuální síť s názvem *NRPVnet* v umístění Východní USA pomocí skupiny prostředků s názvem *NRPRG*.

    ```azurecli
        azure network vnet create NRPRG NRPVnet eastUS -a 10.0.0.0/16
    ```

    Ve virtuální síti *NRPVnet* vytvořte podsíť s názvem *NRPVnetSubnet* s blokem CIDR 10.0.0.0/24.

    ```azurecli
        azure network vnet subnet create NRPRG NRPVnet NRPVnetSubnet -a 10.0.0.0/24
    ```

2. Vytvořte veřejnou IP adresu s názvem *NRPPublicIP*, kterou bude používat front-endový fond IP adres s názvem DNS *loadbalancernrp.eastus.cloudapp.azure.com*. Následující příkaz používá statický typ přidělování a časový limit nečinnosti 4 minuty.

    ```azurecli
        azure network public-ip create -g NRPRG -n NRPPublicIP -l eastus -d loadbalancernrp -a static -i 4
    ```

   > [!IMPORTANT]
   > Nástroj pro vyrovnávání zatížení použije název domény veřejné IP adresy jako svůj plně kvalifikovaný název domény. Tím se liší od modelu nasazení Classic, který používá cloudovou službu jako plně kvalifikovaný název domény nástroje pro vyrovnávání zatížení.
   > V tomto příkladu je plně kvalifikovaný název domény *loadbalancernrp.eastus.cloudapp.azure.com*.

## <a name="create-a-load-balancer"></a>Vytvoření nástroje pro vyrovnávání zatížení

Následující příkaz vytvoří nástroj pro vyrovnávání zatížení s názvem *NRPlb* ve skupině prostředků *NRPRG* v umístění Azure *Východní USA*.

    ```azurecli
    azure network lb create NRPRG NRPlb eastus
    ```

## <a name="create-a-front-end-ip-pool-and-a-backend-address-pool"></a>Vytvoření front-endového fondu IP adres a back-endového fondu adres
Tento příklad ukazuje, jak vytvořit front-endový fond IP adres, který přijímá příchozí síťový provoz v nástroji pro vyrovnávání zatížení, a back-endový fond IP adres, do kterého front-endový fond posílá síťový provoz s vyrovnáváním zatížení.

1. Vytvořte front-endový fond IP adres přidružující veřejnou IP adresu vytvořenou v předchozím kroku k nástroji pro vyrovnávání zatížení.

    ```azurecli
        azure network lb frontend-ip create nrpRG NRPlb NRPfrontendpool -i nrppublicip
    ```

2. Nastavte back-endový fond adres, který slouží k přijímání příchozího provozu z front-endového fondu IP adres.

    ```azurecli
        azure network lb address-pool create NRPRG NRPlb NRPbackendpool
    ```

## <a name="create-lb-rules-nat-rules-and-probe"></a>Vytvoření pravidel nástroje pro vyrovnávání zatížení, pravidel překladu adres (NAT) a testu

Tento příklad vytvoří následující položky.

* Pravidlo překladu adres (NAT), které veškerý příchozí provoz na portu 21 překládá na port 22<sup>1</sup>.
* Pravidlo překladu adres (NAT), které veškerý příchozí provoz na portu 23 překládá na port 22.
* Pravidlo nástroje pro vyrovnávání zatížení, které vyrovnává zatížení veškerého příchozího provozu na portu 80 na port 80 na adresách v back-endovém fondu.
* Pravidlo testu, které kontroluje stav na stránce *HealthProbe.aspx*.

<sup>1</sup> Pravidla překladu adres (NAT) se přidružují ke konkrétní instanci virtuálního počítače za nástrojem pro vyrovnávání zatížení. Síťový provoz přicházející na portu 21 se posílá na portu 22 na konkrétní virtuální počítač přidružený tímto pravidlem překladu adres (NAT). Pro pravidlo překladu adres (NAT) je nutné zadat protokol (UDP nebo TCP). Jednomu portu nelze přiřadit oba protokoly.

1. Vytvořte pravidla překladu adres (NAT).

    ```azurecli
        azure network lb inbound-nat-rule create --resource-group nrprg --lb-name nrplb --name ssh1 --protocol tcp --frontend-port 21 --backend-port 22
        azure network lb inbound-nat-rule create --resource-group nrprg --lb-name nrplb --name ssh2 --protocol tcp --frontend-port 23 --backend-port 22
    ```

2. Vytvořte pravidlo nástroje pro vyrovnávání zatížení.

    ```azurecli
        azure network lb rule create --resource-group nrprg nrplb --lb-name lbrule --protocol tcp --frontend-port 80 --backend-port 80 --frontend-ip-name NRPfrontendpool --backend-address-pool-name NRPbackendpool
    ```

3. Vytvořte test stavu.

    ```azurecli
        azure network lb probe create --resource-group nrprg --lb-name nrplb --name healthprobe --protocol "http" --port 80 --path healthprobe.aspx --interval 15 --count 4
    ```

4. Zkontrolujte nastavení.

    ```azurecli
        azure network lb show nrprg nrplb
    ```

    Očekávaný výstup:

        info:    Executing command network lb show
        + Looking up the load balancer "nrplb"
        + Looking up the public ip "NRPPublicIP"
        data:    Id                              : /subscriptions/####################################/resourceGroups/nrprg/providers/Microsoft.Network/loadBalancers/nrplb
        data:    Name                            : nrplb
        data:    Type                            : Microsoft.Network/loadBalancers
        data:    Location                        : eastus
        data:    Provisioning State              : Succeeded
        data:    Frontend IP configurations:
        data:      Name                          : NRPfrontendpool
        data:      Provisioning state            : Succeeded
        data:      Public IP address id          : /subscriptions/####################################/resourceGroups/NRPRG/providers/Microsoft.Network/publicIPAddresses/NRPPublicIP
        data:      Public IP allocation method   : Static
        data:      Public IP address             : 40.114.13.145
        data:
        data:    Backend address pools:
        data:      Name                          : NRPbackendpool
        data:      Provisioning state            : Succeeded
        data:
        data:    Load balancing rules:
        data:      Name                          : HTTP
        data:      Provisioning state            : Succeeded
        data:      Protocol                      : Tcp
        data:      Frontend port                 : 80
        data:      Backend port                  : 80
        data:      Enable floating IP            : false
        data:      Idle timeout in minutes       : 4
        data:      Frontend IP configuration     : /subscriptions/####################################/resourceGroups/nrprg/providers/Microsoft.Network/loadBalancers/nrplb/frontendIPConfigurations/NRPfrontendpool
        data:      Backend address pool          : /subscriptions/####################################/resourceGroups/nrprg/providers/Microsoft.Network/loadBalancers/nrplb/backendAddressPools/NRPbackendpool
        data:
        data:    Inbound NAT rules:
        data:      Name                          : ssh1
        data:      Provisioning state            : Succeeded
        data:      Protocol                      : Tcp
        data:      Frontend port                 : 21
        data:      Backend port                  : 22
        data:      Enable floating IP            : false
        data:      Idle timeout in minutes       : 4
        data:      Frontend IP configuration     : /subscriptions/####################################/resourceGroups/nrprg/providers/Microsoft.Network/loadBalancers/nrplb/frontendIPConfigurations/NRPfrontendpool
        data:
        data:      Name                          : ssh2
        data:      Provisioning state            : Succeeded
        data:      Protocol                      : Tcp
        data:      Frontend port                 : 23
        data:      Backend port                  : 22
        data:      Enable floating IP            : false
        data:      Idle timeout in minutes       : 4
        data:      Frontend IP configuration     : /subscriptions/####################################/resourceGroups/nrprg/providers/Microsoft.Network/loadBalancers/nrplb/frontendIPConfigurations/NRPfrontendpool
        data:
        data:    Probes:
        data:      Name                          : healthprobe
        data:      Provisioning state            : Succeeded
        data:      Protocol                      : Http
        data:      Port                          : 80
        data:      Interval in seconds           : 15
        data:      Number of probes              : 4
        data:
        info:    network lb show command OK

## <a name="create-nics"></a>Vytvoření síťových rozhraní

Je nutné vytvořit síťová rozhraní (nebo upravit stávající) a potom je přidružit k pravidlům překladu adres (NAT), pravidlům nástroje pro vyrovnávání zatížení a testům.

1. Vytvořte síťové rozhraní s názvem *lb-nic1-be* a přidružte jej k pravidlu překladu adres (NAT) *rdp1* a back-endovému fondu adres *NRPbackendpool*.

    ```azurecli
        azure network nic create --resource-group nrprg --name lb-nic1-be --subnet-name nrpvnetsubnet --subnet-vnet-name nrpvnet --lb-address-pool-ids "/subscriptions/####################################/resourceGroups/nrprg/providers/Microsoft.Network/loadBalancers/nrplb/backendAddressPools/NRPbackendpool" --lb-inbound-nat-rule-ids "/subscriptions/####################################/resourceGroups/nrprg/providers/Microsoft.Network/loadBalancers/nrplb/inboundNatRules/rdp1" eastus
    ```

    Očekávaný výstup:

        info:    Executing command network nic create
        + Looking up the network interface "lb-nic1-be"
        + Looking up the subnet "nrpvnetsubnet"
        + Creating network interface "lb-nic1-be"
        + Looking up the network interface "lb-nic1-be"
        data:    Id                              : /subscriptions/####################################/resourceGroups/nrprg/providers/Microsoft.Network/networkInterfaces/lb-nic1-be
        data:    Name                            : lb-nic1-be
        data:    Type                            : Microsoft.Network/networkInterfaces
        data:    Location                        : eastus
        data:    Provisioning state              : Succeeded
        data:    Enable IP forwarding            : false
        data:    IP configurations:
        data:      Name                          : NIC-config
        data:      Provisioning state            : Succeeded
        data:      Private IP address            : 10.0.0.4
        data:      Private IP Allocation Method  : Dynamic
        data:      Subnet                        : /subscriptions/####################################/resourceGroups/NRPRG/providers/Microsoft.Network/virtualNetworks/NRPVnet/subnets/NRPVnetSubnet
        data:      Load balancer backend address pools
        data:        Id                          : /subscriptions/####################################/resourceGroups/nrprg/providers/Microsoft.Network/loadBalancers/nrplb/backendAddressPools/NRPbackendpool
        data:      Load balancer inbound NAT rules:
        data:        Id                          : /subscriptions/####################################/resourceGroups/nrprg/providers/Microsoft.Network/loadBalancers/nrplb/inboundNatRules/rdp1
        data:
        info:    network nic create command OK

2. Vytvořte síťové rozhraní s názvem *lb-nic2-be* a přidružte jej k pravidlu překladu adres (NAT) *rdp2* a back-endovému fondu adres *NRPbackendpool*.

    ```azurecli
        azure network nic create --resource-group nrprg --name lb-nic2-be --subnet-name nrpvnetsubnet --subnet-vnet-name nrpvnet --lb-address-pool-ids "/subscriptions/####################################/resourceGroups/nrprg/providers/Microsoft.Network/loadBalancers/nrplb/backendAddressPools/NRPbackendpool" --lb-inbound-nat-rule-ids "/subscriptions/####################################/resourceGroups/nrprg/providers/Microsoft.Network/loadBalancers/nrplb/inboundNatRules/rdp2" eastus
    ```

3. Vytvořte virtuální počítač s názvem *web1* a přidružte jej k síťovému rozhraní s názvem *lb-nic1-be*. Účet úložiště s názvem *web1nrp* byl vytvořen ještě před spuštěním následujícího příkazu.

    ```azurecli
        azure vm create --resource-group nrprg --name web1 --location eastus --vnet-name nrpvnet --vnet-subnet-name nrpvnetsubnet --nic-name lb-nic1-be --availset-name nrp-avset --storage-account-name web1nrp --os-type Windows --image-urn MicrosoftWindowsServer:WindowsServer:2012-R2-Datacenter:4.0.20150825
    ```

    > [!IMPORTANT]
    > Virtuální počítače v nástroji pro vyrovnávání zatížení musí být ve stejné skupině dostupnosti. Skupinu dostupnosti vytvoříte pomocí příkazu `azure availset create`.

    Výstup by měl vypadat přibližně takto:

        info:    Executing command vm create
        + Looking up the VM "web1"
        Enter username: azureuser
        Enter password for azureuser: *********
        Confirm password: *********
        info:    Using the VM Size "Standard_A1"
        info:    The [OS, Data] Disk or image configuration requires storage account
        + Looking up the storage account web1nrp
        + Looking up the availability set "nrp-avset"
        info:    Found an Availability set "nrp-avset"
        + Looking up the NIC "lb-nic1-be"
        info:    Found an existing NIC "lb-nic1-be"
        info:    Found an IP configuration with virtual network subnet id "/subscriptions/####################################/resourceGroups/NRPRG/providers/Microsoft.Network/virtualNetworks/NRPVnet/subnets/NRPVnetSubnet" in the NIC "lb-nic1-be"
        info:    This is a NIC without publicIP configured
        + Creating VM "web1"
        info:    vm create command OK

    > [!NOTE]
    > Očekává se zobrazení informační zprávy **Jedná se o síťové rozhraní bez nakonfigurované veřejné IP adresy**, protože síťové rozhraní vytvořené pro nástroj pro vyrovnávání zatížení se k internetu připojuje pomocí veřejné IP adresy nástroje pro vyrovnávání zatížení.

    Vzhledem k tomu, že síťové rozhraní *lb-nic1-be* je přidružené k pravidlu překladu adres (NAT) *rdp1*, můžete se připojit k virtuálnímu počítači *web1* pomocí protokolu RDP přes port 3441 v nástroji pro vyrovnávání zatížení.

4. Vytvořte virtuální počítač s názvem *web2* a přidružte jej k síťovému rozhraní s názvem *lb-nic2-be*. Účet úložiště s názvem *web1nrp* byl vytvořen ještě před spuštěním následujícího příkazu.

    ```azurecli
        azure vm create --resource-group nrprg --name web2 --location eastus --vnet-name nrpvnet --vnet-subnet-name nrpvnetsubnet --nic-name lb-nic2-be --availset-name nrp-avset --storage-account-name web2nrp --os-type Windows --image-urn MicrosoftWindowsServer:WindowsServer:2012-R2-Datacenter:4.0.20150825
    ```

## <a name="update-an-existing-load-balancer"></a>Aktualizace stávajícího nástroje pro vyrovnávání zatížení
Můžete přidat pravidla odkazující na stávající nástroj pro vyrovnávání zatížení. V následujícím příkladu se přidá nové pravidlo nástroje pro vyrovnávání zatížení do stávajícího nástroje pro vyrovnávání zatížení **NRPlb**.

```azurecli
azure network lb rule create --resource-group nrprg --lb-name nrplb --name lbrule2 --protocol tcp --frontend-port 8080 --backend-port 8051 --frontend-ip-name frontendnrppool --backend-address-pool-name NRPbackendpool
```

## <a name="delete-a-load-balancer"></a>Odstranění nástroje pro vyrovnávání zatížení
K odebrání nástroje pro vyrovnávání zatížení použijte následující příkaz:

```azurecli
azure network lb delete --resource-group nrprg --name nrplb
```

## <a name="next-steps"></a>Další kroky
[Začínáme s konfigurací interního nástroje pro vyrovnávání zatížení](load-balancer-get-started-ilb-arm-cli.md)

[Konfigurace distribučního režimu nástroje pro vyrovnávání zatížení](load-balancer-distribution-mode.md)

[Konfigurace nastavení časového limitu nečinnosti protokolu TCP pro nástroj pro vyrovnávání zatížení](load-balancer-tcp-idle-timeout.md)



<!--HONumber=Jan17_HO4-->


