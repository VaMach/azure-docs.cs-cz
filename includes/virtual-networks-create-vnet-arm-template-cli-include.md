## Nasazení šablony ARM pomocí rozhraní příkazového řádku Azure

Pokud chcete nasadit šablonu ARM, kterou jste stáhli, pomocí rozhraní příkazového řádku Azure, použijte následující postup.

1. Pokud jste rozhraní příkazového řádku Azure nikdy nepoužívali, přejděte na téma [Instalace a konfigurace rozhraní příkazového řádku Azure](../articles/xplat-cli-install.md) a postupujte podle pokynů až do chvíle, kdy můžete vybrat svůj účet a předplatné Azure.
2. Spuštěním příkazu **`azure config mode`** přepněte do režimu Resource Manager, jak vidíte níže.

        azure config mode arm

    Toto je očekávaný výstup výše uvedeného příkazu:

        info:    New mode is arm

3. V případě potřeby vytvořte novou skupinu prostředků spuštěním příkazu **`azure group create`**, jak vidíte níže. Všimněte si výstupu příkazu. Seznam uvedený za výstupem vysvětluje použité parametry. Další informace o skupinách prostředků najdete v článku [Přehled Azure Resource Manageru](../articles/resource-group-overview.md).

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

    - **-n (nebo --name)**. Název nové skupiny prostředků. V našem scénáři je to *TestRG*.
    - **-l (nebo --location)**. Oblast Azure, ve které se nová skupina prostředků vytvoří. V našem scénáři je to *centralus*.

4. Spuštěním rutiny **`azure group deployment create`** nasadíte novou síť VNet pomocí šablony a souborů parametrů, které jste stáhli a upravili v předchozích krocích. Seznam uvedený za výstupem vysvětluje použité parametry.

        azure group deployment create -g TestRG -n TestVNetDeployment -f C:\ARM\azuredeploy.json -e C:\ARM\azuredeploy-parameters.json

    Toto je očekávaný výstup výše uvedeného příkazu:

        info:    Executing command group deployment create
        + Initializing template configurations and parameters
        + Creating a deployment
        info:    Created template deployment "TestVNetDeployment"
        + Registering providers
        info:    Registering provider microsoft.network
        + Waiting for deployment to complete
        data:    DeploymentName     : TestVNetDeployment
        data:    ResourceGroupName  : TestRG
        data:    ProvisioningState  : Succeeded
        data:    Timestamp          : 2015-08-14T21:56:11.152759Z
        data:    Mode               : Incremental
        data:    Name           Type    Value
        data:    -------------  ------  --------------
        data:    location       String  Central US
        data:    vnetName       String  TestVNet
        data:    addressPrefix  String  192.168.0.0/16
        data:    subnet1Prefix  String  192.168.1.0/24
        data:    subnet1Name    String  FrontEnd
        data:    subnet2Prefix  String  192.168.2.0/24
        data:    subnet2Name    String  BackEnd
        info:    group deployment create command OK

    - **-g (nebo --resource-group)**. Název skupiny prostředků, ve které se vytvoří nová síť VNet.
    - **-f (nebo --template-file)**. Cesta k souboru šablony ARM.
    - **-e (nebo --parameters-file)**. Cesta k souboru parametrů ARM.

5. Spuštěním příkazu **`azure network vnet show`** zobrazíte vlastnosti nové sítě VNet, jak vidíte níže.

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


<!--HONumber=Sep16_HO3-->


