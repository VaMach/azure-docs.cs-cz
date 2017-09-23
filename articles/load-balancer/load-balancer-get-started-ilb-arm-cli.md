---
title: "Vytvoření interního nástroje pro vyrovnávání zatížení – rozhraní příkazového řádku Azure | Dokumentace Microsoftu"
description: "Zjistěte, jak vytvořit interní nástroj pro vyrovnávání zatížení pomocí rozhraní příkazového řádku Azure v Resource Manageru"
services: load-balancer
documentationcenter: na
author: kumudd
manager: timlt
tags: azure-resource-manager
ms.assetid: c7a24e92-b4da-43c0-90f2-841c1b7ce489
ms.service: load-balancer
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 01/23/2017
ms.author: kumud
ms.translationtype: Human Translation
ms.sourcegitcommit: 0d8472cb3b0d891d2b184621d62830d1ccd5e2e7
ms.openlocfilehash: 128b91c685b5f7e494a69ca5b04165a0ee7cbb78
ms.contentlocale: cs-cz
ms.lasthandoff: 03/21/2017

---

# <a name="create-an-internal-load-balancer-by-using-the-azure-cli"></a>Vytvoření interního nástroje pro vyrovnávání zatížení pomocí rozhraní příkazového řádku Azure

> [!div class="op_single_selector"]
> * [Azure Portal](../load-balancer/load-balancer-get-started-ilb-arm-portal.md)
> * [PowerShell](../load-balancer/load-balancer-get-started-ilb-arm-ps.md)
> * [Azure CLI](../load-balancer/load-balancer-get-started-ilb-arm-cli.md)
> * [Šablona](../load-balancer/load-balancer-get-started-ilb-arm-template.md)

[!INCLUDE [load-balancer-get-started-ilb-intro-include.md](../../includes/load-balancer-get-started-ilb-intro-include.md)]

> [!NOTE]
> Azure má dva různé modely nasazení pro vytváření prostředků a práci s nimi: [Resource Manager a klasický model](../azure-resource-manager/resource-manager-deployment-model.md).  Tento článek se věnuje modelu nasazení Resource Manager, který Microsoft doporučuje pro většinu nových nasazení namísto [klasického modelu nasazení](load-balancer-get-started-ilb-classic-cli.md).

[!INCLUDE [load-balancer-get-started-ilb-scenario-include.md](../../includes/load-balancer-get-started-ilb-scenario-include.md)]

## <a name="deploy-the-solution-by-using-the-azure-cli"></a>Nasazení řešení pomocí rozhraní příkazového řádku Azure

Následující postup ukazuje, jak vytvořit internetový nástroj pro vyrovnávání zatížení pomocí Azure Resource Manageru s rozhraním příkazového řádku. S Azure Resource Managerem se jednotlivé prostředky vytvoří a nakonfigurují zvlášť, následně se spojí dohromady a vytvoří prostředek.

Pokud chcete nasadit nástroj pro vyrovnávání zatížení, je nutné vytvořit a nakonfigurovat následující objekty:

* **Konfigurace front-endových IP adres**: obsahuje veřejné IP adresy pro příchozí síťový provoz.
* **Back-endový fond adres**: obsahuje síťová rozhraní, která umožňují virtuálním počítačům přijímat síťový provoz z nástroje pro vyrovnávání zatížení.
* **Pravidla vyrovnávání zatížení**: obsahuje pravidla, která mapují veřejný port v nástroji pro vyrovnávání zatížení na port v back-endovém fondu adres.
* **Pravidla příchozího překladu adres (NAT)**: obsahuje pravidla, která mapují veřejný port v nástroji pro vyrovnávání zatížení na port konkrétního virtuálního počítače v back-endovém fondu adres.
* **Testy**: obsahuje testy stavu sloužící ke kontrole dostupnosti instancí virtuálních počítačů v back-endovém fondu adres.

Další informace najdete v tématu [Podpora služby Load Balancer v Azure Resource Manageru](load-balancer-arm.md).

## <a name="set-up-cli-to-use-resource-manager"></a>Nastavení rozhraní příkazového řádku pro použití Resource Manageru

1. Pokud jste rozhraní příkazového řádku Azure nikdy nepoužívali, přejděte na téma [Instalace a konfigurace rozhraní příkazového řádku Azure](../cli-install-nodejs.md). Postupujte podle pokynů až do chvíle, kdy máte vybrat svůj účet a předplatné Azure.
2. Spuštěním příkazu **azure config mode** přejděte do režimu Resource Manager, jak vidíte níže:

    ```azurecli
    azure config mode arm
    ```

    Očekávaný výstup:

        info:    New mode is arm

## <a name="create-an-internal-load-balancer-step-by-step"></a>Vytvoření interního nástroje pro vyrovnávání zatížení krok za krokem

1. Přihlaste se k Azure.

    ```azurecli
    azure login
    ```

    Po zobrazení výzvy zadejte své přihlašovací údaje Azure.

2. Přepněte nástroje příkazového řádku do režimu Azure Resource Manager.

    ```azurecli
    azure config mode arm
    ```

## <a name="create-a-resource-group"></a>Vytvoření skupiny prostředků

Všechny prostředky v Azure Resource Manageru jsou přidruženy ke skupině prostředků. Pokud jste tak ještě neučinili, vytvořte skupinu prostředků.

```azurecli
azure group create <resource group name> <location>
```

## <a name="create-an-internal-load-balancer-set"></a>Vytvoření sady interního nástroje pro vyrovnávání zatížení

1. Vytvořte interní nástroj pro vyrovnávání zatížení.

    V následujícím scénáři se vytvoří skupina prostředků nrprg v oblasti Východní USA.

    ```azurecli
    azure network lb create --name nrprg --location eastus
    ```

   > [!NOTE]
   > Všechny prostředky interního nástroje pro vyrovnávání zatížení, jako jsou virtuální sítě a podsítě virtuálních sítí, musí být ve stejné skupině prostředků a ve stejné oblasti.

2. Vytvořte front-endovou IP adresu pro tento interní nástroj pro vyrovnávání zatížení.

    IP adresa, kterou použijete, se musí nacházet v rozsahu podsítě vaší virtuální sítě.

    ```azurecli
    azure network lb frontend-ip create --resource-group nrprg --lb-name ilbset --name feilb --private-ip-address 10.0.0.7 --subnet-name nrpvnetsubnet --subnet-vnet-name nrpvnet
    ```

3. Vytvořte back-endový fond adres.

    ```azurecli
    azure network lb address-pool create --resource-group nrprg --lb-name ilbset --name beilb
    ```

    Po definování front-endové IP adresy a back-endového fondu adres můžete vytvořit pravidla nástroje pro vyrovnávání zatížení, pravidla příchozího překladu adres (NAT) a vlastní testy stavu.

4. Vytvořte pravidlo nástroje pro vyrovnávání zatížení pro tento interní nástroj pro vyrovnávání zatížení.

    Po provedení předchozích kroků vytvoří tento příkaz pravidlo nástroje pro vyrovnávání zatížení pro naslouchání na portu 1433 ve front-endovém fondu a posílání síťového provozu s vyrovnáváním zatížení do back-endového fondu adres rovněž na portu 1433.

    ```azurecli
    azure network lb rule create --resource-group nrprg --lb-name ilbset --name ilbrule --protocol tcp --frontend-port 1433 --backend-port 1433 --frontend-ip-name feilb --backend-address-pool-name beilb
    ```

5. Vytvořte pravidla příchozího překladu adres (NAT).

    Pravidla příchozího překladu adres (NAT) slouží k vytvoření koncových bodů v nástroji pro vyrovnávání zatížení, které vedou ke konkrétním instancím virtuálních počítačů. Předchozí kroky vytvořily dvě pravidla překladu adres (NAT) pro vzdálenou plochu.

    ```azurecli
    azure network lb inbound-nat-rule create --resource-group nrprg --lb-name ilbset --name NATrule1 --protocol TCP --frontend-port 5432 --backend-port 3389

    azure network lb inbound-nat-rule create --resource-group nrprg --lb-name ilbset --name NATrule2 --protocol TCP --frontend-port 5433 --backend-port 3389
    ```

6. Vytvořte testy stavu pro tento nástroj pro vyrovnávání zatížení.

    Test stavu kontroluje všechny instance virtuálních počítačů a ověřuje, že mohou posílat síťový provoz. Instance virtuálního počítače, u níž se kontroly testu nezdaří, se odebere z nástroje pro vyrovnávání zatížení do té doby, než znovu přejde do režimu online a kontrola testu určí, že je v pořádku.

    ```azurecli
    azure network lb probe create --resource-group nrprg --lb-name ilbset --name ilbprobe --protocol tcp --interval 300 --count 4
    ```

    > [!NOTE]
    > Platforma Microsoft Azure používá pro řadu scénářů správy statickou, veřejně směrovatelnou IPv4 adresu. Jedná se o IP adresu 168.63.129.16. Tuto IP adresu by neměla blokovat žádná brána firewall, protože by to mohlo způsobit neočekávané chování.
    > Vzhledem k internímu vyrovnávání zatížení Azure slouží tato IP adresa monitorovacím testům z nástroje pro vyrovnávání zatížení k určování stavu virtuálních počítačů v sadě s vyrovnáváním zatížení. Pokud se k omezení provozu na virtuální počítače Azure v sadě s interním vyrovnáváním zatížení používá skupina zabezpečení sítě nebo pokud je použita na podsíti virtuální sítě, ujistěte se, že je přidáno pravidlo zabezpečení sítě umožňující provoz z adresy 168.63.129.16.

## <a name="create-nics"></a>Vytvoření síťových rozhraní

Je nutné vytvořit síťová rozhraní (nebo upravit stávající) a potom je přidružit k pravidlům překladu adres (NAT), pravidlům nástroje pro vyrovnávání zatížení a testům.

1. Vytvořte síťové rozhraní s názvem *lb-nic1-be* a přidružte jej k pravidlu překladu adres (NAT) *rdp1* a back-endovému fondu adres *beilb*.

    ```azurecli
    azure network nic create --resource-group nrprg --name lb-nic1-be --subnet-name nrpvnetsubnet --subnet-vnet-name nrpvnet --lb-address-pool-ids "/subscriptions/####################################/resourceGroups/nrprg/providers/Microsoft.Network/loadBalancers/nrplb/backendAddressPools/beilb" --lb-inbound-nat-rule-ids "/subscriptions/####################################/resourceGroups/nrprg/providers/Microsoft.Network/loadBalancers/nrplb/inboundNatRules/rdp1" --location eastus
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

2. Vytvořte síťové rozhraní s názvem *lb-nic2-be* a přidružte jej k pravidlu překladu adres (NAT) *rdp2* a back-endovému fondu adres *beilb*.

    ```azurecli
    azure network nic create --resource-group nrprg --name lb-nic2-be --subnet-name nrpvnetsubnet --subnet-vnet-name nrpvnet --lb-address-pool-ids "/subscriptions/####################################/resourceGroups/nrprg/providers/Microsoft.Network/loadBalancers/nrplb/backendAddressPools/beilb" --lb-inbound-nat-rule-ids "/subscriptions/####################################/resourceGroups/nrprg/providers/Microsoft.Network/loadBalancers/nrplb/inboundNatRules/rdp2" --location eastus
    ```

3. Vytvořte virtuální počítač s názvem *DB1* a přidružte jej k síťovému rozhraní s názvem *lb-nic1-be*. Účet úložiště s názvem *web1nrp* je vytvořen ještě před spuštěním následujícího příkazu:

    ```azurecli
    azure vm create --resource--resource-grouproup nrprg --name DB1 --location eastus --vnet-name nrpvnet --vnet-subnet-name nrpvnetsubnet --nic-name lb-nic1-be --availset-name nrp-avset --storage-account-name web1nrp --os-type Windows --image-urn MicrosoftWindowsServer:WindowsServer:2012-R2-Datacenter:4.0.20150825
    ```
    > [!IMPORTANT]
    > Virtuální počítače v nástroji pro vyrovnávání zatížení musí být ve stejné skupině dostupnosti. Skupinu dostupnosti vytvoříte pomocí příkazu `azure availset create`.

4. Vytvořte virtuální počítač s názvem *DB2* a přidružte jej k síťovému rozhraní s názvem *lb-nic2-be*. Účet úložiště s názvem *web1nrp* byl vytvořen ještě před spuštěním následujícího příkazu.

    ```azurecli
    azure vm create --resource--resource-grouproup nrprg --name DB2 --location eastus --vnet-name nrpvnet --vnet-subnet-name nrpvnetsubnet --nic-name lb-nic2-be --availset-name nrp-avset --storage-account-name web2nrp --os-type Windows --image-urn MicrosoftWindowsServer:WindowsServer:2012-R2-Datacenter:4.0.20150825
    ```

## <a name="delete-a-load-balancer"></a>Odstranění nástroje pro vyrovnávání zatížení

K odebrání nástroje pro vyrovnávání zatížení použijte následující příkaz:

```azurecli
azure network lb delete --resource-group nrprg --name ilbset
```

## <a name="next-steps"></a>Další kroky

[Konfigurace distribučního režimu nástroje pro vyrovnávání zatížení pomocí spřažení se zdrojovou IP adresou](load-balancer-distribution-mode.md)

[Konfigurace nastavení časového limitu nečinnosti protokolu TCP pro nástroj pro vyrovnávání zatížení](load-balancer-tcp-idle-timeout.md)


