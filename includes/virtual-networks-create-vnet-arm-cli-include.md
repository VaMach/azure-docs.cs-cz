## <a name="how-to-create-a-vnet-using-the-azure-cli"></a>Vytvoření sítě VNet pomocí rozhraní příkazového řádku Azure
Pomocí rozhraní příkazového řádku Azure můžete spravovat svoje prostředky Azure z příkazového řádku jakéhokoliv počítače se systémem Windows, Linux a OS X. Pokud chcete vytvořit síť VNet pomocí rozhraní příkazového řádku Azure, postupujte následujícím způsobem.

1. Pokud jste rozhraní příkazového řádku Azure nikdy nepoužívali, přejděte na téma [Instalace a konfigurace rozhraní příkazového řádku Azure](../articles/cli-install-nodejs.md) a postupujte podle pokynů až do chvíle, kdy můžete vybrat svůj účet a předplatné Azure.
2. Spuštěním příkazu **azure config mode** přejděte do režimu Resource Manager, jak vidíte níže.
   
        azure config mode arm
   
    Toto je očekávaný výstup výše uvedeného příkazu:
   
        info:    New mode is arm
3. V případě potřeby vytvořte novou skupinu prostředků spuštěním příkazu **azure group create**, jak vidíte níže. Prohlédněte si výstup příkazu. Seznam uvedený za výstupem vysvětluje použité parametry. Další informace o skupinách prostředků najdete v článku [Přehled Azure Resource Manageru](../articles/azure-resource-manager/resource-group-overview.md#resource-groups).
   
        azure group create -n TestRG -l centralus
   
    Toto je očekávaný výstup výše uvedeného příkazu:
   
        info:    Executing command group create
        + Getting resource group TestRG
        + Creating resource group TestRG
        info:    Created resource group TestRG
        data:    Id:                  /subscriptions/628dad04-b5d1-4f10-b3a4-dc61d88cf97c/resourceGroups/TestRG
        data:    Name:                TestRG
        data:    Location:            centralus
        data:    Provisioning State:  Succeeded
        data:    Tags: null
        data:
        info:    group create command OK
   
   * **-n (nebo --name)**. Název nové skupiny prostředků. V našem scénáři je to *TestRG*.
   * **-l (nebo --location)**. Oblast Azure, ve které se nová skupina prostředků vytvoří. V našem scénáři je to *centralus*.
4. Spuštěním příkazu **azure network vnet create** vytvořte síť VNet a podsíť, jak vidíte níže. 
   
        azure network vnet create -g TestRG -n TestVNet -a 192.168.0.0/16 -l centralus
   
    Toto je očekávaný výstup výše uvedeného příkazu:
   
        info:    Executing command network vnet create
        + Looking up virtual network "TestVNet"
        + Creating virtual network "TestVNet"
        + Loading virtual network state
        data:    Id                              : /subscriptions/628dad04-b5d1-4f10-b3a4-dc61d88cf97c/resourceGroups/TestRG/providers/Microsoft.Network/virtualNetworks/TestVNet2
        data:    Name                            : TestVNet
        data:    Type                            : Microsoft.Network/virtualNetworks
        data:    Location                        : centralus
        data:    ProvisioningState               : Succeeded
        data:    Address prefixes:
        data:      192.168.0.0/16
        info:    network vnet create command OK
   
   * **-g (nebo --resource-group)**. Název skupiny prostředků, ve které se vytvoří síť VNet. V našem scénáři je to *TestRG*.
   * **-n (nebo --name)**. Název sítě VNet, která se má vytvořit. V našem scénáři je to *TestVNet*.
   * **-a (nebo --address-prefixes)**. Seznam bloků CIDR používaných pro adresní prostor sítě VNet. V našem scénáři je to *192.168.0.0/16*
   * **-l (nebo --location)**. Oblast Azure, ve které bude síť VNet vytvořena. V našem scénáři je to *centralus*.
5. Spuštěním příkazu **azure network vnet subnet create** vytvořte podsíť, jak vidíte níže. Prohlédněte si výstup příkazu. Seznam uvedený za výstupem vysvětluje použité parametry.
   
        azure network vnet subnet create -g TestRG -e TestVNet -n FrontEnd -a 192.168.1.0/24
   
    Toto je očekávaný výstup výše uvedeného příkazu:
   
        info:    Executing command network vnet subnet create
        + Looking up the subnet "FrontEnd"
        + Creating subnet "FrontEnd"
        + Looking up the subnet "FrontEnd"
        data:    Id                              : /subscriptions/628dad04-b5d1-4f10-b3a4-dc61d88cf97c/resourceGroups/TestRG/providers/Microsoft.Network/virtualNetworks/TestVNet/subnets/FrontEnd
        data:    Type                            : Microsoft.Network/virtualNetworks/subnets
        data:    ProvisioningState               : Succeeded
        data:    Name                            : FrontEnd
        data:    Address prefix                  : 192.168.1.0/24
        data:
        info:    network vnet subnet create command OK
   
   * **-e (nebo --vnet-name**. Název sítě VNet, ve které se vytvoří podsíť. V našem scénáři je to *TestVNet*.
   * **-n (nebo --name)**. Název nové podsítě. V našem scénáři je to *FrontEnd*.
   * **-a (nebo --address-prefixes)**. Blok CIDR podsítě. V našem scénáři je to *192.168.1.0/24*.
6. V případě potřeby opakováním kroku 5 výše vytvořte další podsítě. V našem scénáři spuštěním následujícího příkazu vytvořte podsíť *BackEnd*.
   
        azure network vnet subnet create -g TestRG -e TestVNet -n BackEnd -a 192.168.2.0/24
7. Spuštěním příkazu **azure network vnet show** zobrazíte vlastnosti nové sítě VNet, jak vidíte níže.
   
        azure network vnet show -g TestRG -n TestVNet
   
    Toto je očekávaný výstup výše uvedeného příkazu:
   
        info:    Executing command network vnet show
        + Looking up virtual network "TestVNet"
        data:    Id                              : /subscriptions/628dad04-b5d1-4f10-b3a4-dc61d88cf97c/resourceGroups/TestRG/providers/Microsoft.Network/virtualNetworks/TestVNet
        data:    Name                            : TestVNet
        data:    Type                            : Microsoft.Network/virtualNetworks
        data:    Location                        : centralus
        data:    ProvisioningState               : Succeeded
        data:    Address prefixes:
        data:      192.168.0.0/16
        data:    Subnets:
        data:      Name                          : FrontEnd
        data:      Address prefix                : 192.168.1.0/24
        data:
        data:      Name                          : BackEnd
        data:      Address prefix                : 192.168.2.0/24
        data:
        info:    network vnet show command OK

