
<properties
   pageTitle="Vytvoření služby Application Gateway pomocí šablon Azure Resource Manageru | Microsoft Azure"
   description="Tato stránka obsahuje pokyny pro vytvoření služby Azure Application Gateway pomocí šablony Azure Resource Manageru."
   documentationCenter="na"
   services="application-gateway"
   authors="georgewallace"
   manager="jdial"
   editor="tysonn"/>
<tags
   ms.service="application-gateway"
   ms.devlang="na"
   ms.topic="hero-article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="08/09/2016"
   ms.author="gwallace"/>


# Vytvoření služby Application Gateway pomocí šablony Azure Resource Manageru

Služba Azure Application Gateway je nástroj pro vyrovnávání zatížení vrstvy 7. Poskytuje převzetí služeb při selhání, směrování výkonu požadavků HTTP mezi různými servery, ať už jsou místní nebo v cloudu. Služba Application Gateway poskytuje následující funkce doručování aplikací: vyrovnávání zatížení HTTP, spřažení relace na základě souborů cookie a přesměrování zpracování Secure Sockets Layer (SSL).

> [AZURE.SELECTOR]
- [portál Azure](application-gateway-create-gateway-portal.md)
- [Azure Resource Manager PowerShell](application-gateway-create-gateway-arm.md)
- [Azure Classic PowerShell](application-gateway-create-gateway.md)
- [Šablona Azure Resource Manageru](application-gateway-create-gateway-arm-template.md)

<BR>

Dozvíte se, jak stáhnout a upravit existující šablonu Azure Resource Manageru z webu GitHub a jak ji nasadit z webu GitHub, prostředí PowerShell a rozhraní příkazového řádku Azure.

Pokud šablonu Azure Resource Manageru jednoduše nasazujete přímo z GitHubu beze změn, přejděte k části o nasazení šablony z GitHubu.


## Scénář

V tomto scénáři provedete tyto kroky:

- Vytvoříte službu Application Gateway se dvěma instancemi.
- Vytvoříte virtuální síť s názvem VirtualNetwork1 s vyhrazeným blokem CIDR 10.0.0.0/16.
- Vytvoříte podsíť s názvem Appgatewaysubnet, která používá blok CIDR 10.0.0.0/28.
- Nastavíte dvě dříve nakonfigurované back-end IP adresy pro webové servery, které chcete použít k vyrovnávání zatížení datových přenosů. V tomto příkladu šablony se jedná o back-end IP adresy 10.0.1.10 a 10.0.1.11.

>[AZURE.NOTE] Tato nastavení jsou parametry této šablony. Pokud ji chcete přizpůsobit, můžete změnit pravidla, naslouchací proces a protokol SSL, který otevírá azuredeploy.json.



![Scénář](./media/application-gateway-create-gateway-arm-template/scenario-arm.png)



## Stažení a pochopení šablony Azure Resource Manageru

Z webu GitHub si můžete stáhnout existující šablonu Azure Resource Manageru, která umožňuje vytvořit virtuální síť a dvě podsítě, provést v ní jakékoli změny a opakovaně ji používat. Postupujte tímto způsobem:

1. Přejděte na [Vytvořit Application Gateway](https://github.com/Azure/azure-quickstart-templates/tree/master/101-application-gateway-create).
2. Klikněte na **azuredeploy.json** a potom na **RAW**.
3. Uložte soubor do místní složky v počítači.
4. Pokud už šablony Azure Resource Manageru znáte, pokračujte krokem 7.
5. Otevřete soubor, který jste uložili, a prohlédněte si jeho obsah v části **parameters** na řádku 5. Parametry šablony Azure Resource Manageru představují zástupce hodnot, které můžete doplnit během nasazování.

  	| Parametr | Popis |
  	|---|---|
  	| **location** | Oblast Azure, kde je služba Application Gateway vytvořena |
  	| **VirtualNetwork1** | Název nové virtuální sítě |
  	| **addressPrefix** | Adresní prostor virtuální sítě ve formátu CIDR |
  	| **ApplicationGatewaysubnet** | Název podsítě služby Application Gateway |
  	| **subnetPrefix** | Blok CIDR podsítě služby Application Gateway |
  	| **skuname** | Velikost instance SKU |
  	| **capacity** | Počet instancí |
  	| **backendaddress1** | IP adresa prvního webového serveru |
  	| **backendaddress2** | IP adresa druhého webového serveru |


>[AZURE.IMPORTANT] Šablony Azure Resource Manageru udržované na webu GitHub se můžou časem změnit. Před použitím proto šablonu vždycky nejdřív zkontrolujte.

6. Prohlédněte si obsah v části **resources** a všimněte si následujících parametrů:

    - **type**. Typ prostředku vytvořeného šablonou. V tomto případě je to typ **Microsoft.Network/applicationGateways**, který představuje službu Application Gateway.
    - **name**. Název prostředku. Všimněte si volání **[parameters('applicationGatewayName')]**, které znamená, že název doplníte jako vstup nebo je doplněn souborem parametru během nasazení.
    - **properties**. Seznam vlastností prostředku. Tato šablona používá při vytváření služby Application Gateway virtuální síť a veřejnou IP adresu.

7. Vraťte se na stránku [https://github.com/Azure/azure-quickstart-templates/blob/master/101-application-gateway-create/](https://github.com/Azure/azure-quickstart-templates/blob/master/101-application-gateway-create).
8. Klikněte na **azuredeploy-paremeters.json** a potom klikněte na **RAW**.
9. Uložte soubor do místní složky v počítači.
10. Otevřete soubor, který jste uložili, a upravte hodnoty parametrů. K nasazení služby Application Gateway popsané v tomto scénáři použijte následující hodnoty.

        {
          "$schema": "http://schema.management.azure.com/schemas/2015-01-01/deploymentParameters.json#",
        {
        "location" : {
        "value" : "West US"
        },
        "addressPrefix": {
        "value": "10.0.0.0/16"
        },
        "subnetPrefix": {
        "value": "10.0.0.0/24"
        },
        "skuName": {
        "value": "Standard_Small"
        },
        "capacity": {
        "value": 2
        },
        "backendIpAddress1": {
        "value": "10.0.1.10"
        },
        "backendIpAddress2": {
        "value": "10.0.1.11"
        }
        }

11. Uložte soubor. Šablonu JSON a šablonu parametrů můžete otestovat pomocí online ověřovacích nástrojů JSON, jako je třeba [JSlint.com](http://www.jslint.com/).

## Nasazení šablony Azure Resource Manageru pomocí prostředí PowerShell

Pokud jste prostředí Azure PowerShell nikdy nepoužívali, přejděte na článek [Instalace a konfigurace prostředí Azure PowerShell](../powershell-install-configure.md) a použijte pokyny pro přihlášení ke službě Azure a vyberte své předplatné.

### Krok 1

        Login-AzureRmAccount



### Krok 2

Zkontrolujte předplatná pro příslušný účet.

        get-AzureRmSubscription

Zobrazí se výzva k ověření pomocí přihlašovacích údajů.<BR>

### Krok 3

Zvolte předplatné Azure, které chcete použít. <BR>


        Select-AzureRmSubscription -Subscriptionid "GUID of subscription"


### Krok 4


Pokud je to potřeba, vytvořte pomocí rutiny **New-AzureResourceGroup** skupinu prostředků. V následujícím příkladu vytvoříte novou skupinu prostředků s názvem AppgatewayRG v umístění Východní USA.

     New-AzureRmResourceGroup -Name AppgatewayRG -Location "East US"
        VERBOSE: 5:38:49 PM - Created resource group 'AppgatewayRG' in location 'eastus'


        ResourceGroupName : AppgatewayRG
        Location          : eastus
        ProvisioningState : Succeeded
        Tags              :
        Permissions       :
                     Actions  NotActions
                     =======  ==========
                      *

        ResourceId        : /subscriptions/xxxxxxxxxxxxxxxxxxxxxxxxxxxxxx/resourceGroups/AppgatewayRG

Spuštěním rutiny **New-AzureRmResourceGroupDeployment** nasadíte novou virtuální síť pomocí šablony a souborů parametrů, které jste stáhli a upravili v krocích výše.

        New-AzureRmResourceGroupDeployment -Name TestAppgatewayDeployment -ResourceGroupName AppgatewayRG `
           -TemplateFile C:\ARM\azuredeploy.json -TemplateParameterFile C:\ARM\azuredeploy-parameters.json

Příkazový řádek vygeneruje následující výstup:

        DeploymentName    : testappgatewaydeployment
        ResourceGroupName : appgatewayRG
        ProvisioningState : Succeeded
        Timestamp         : 9/19/2015 1:49:41 AM
        Mode              : Incremental
        TemplateLink      :
        Parameters        :
                   Name             Type                       Value
                   ===============  =========================  ==========
                   location         String                     East US
                   addressPrefix    String                     10.0.0.0/16
                   subnetPrefix     String                     10.0.0.0/24
                   skuName          String                     Standard_Small
                   capacity         Int                        2
                   backendIpAddress1  String                     10.0.1.10
                   backendIpAddress2  String                     10.0.1.11

        Outputs           :


## Nasazení šablony Azure Resource Manageru pomocí rozhraní příkazového řádku Azure

Pokud chcete nasadit šablonu Azure Resource Manageru, kterou jste stáhli, pomocí rozhraní příkazového řádku Azure CLI, použijte následující postup:

### Krok 1

Pokud jste rozhraní příkazového řádku Azure nikdy nepoužívali, přejděte na téma [Instalace a konfigurace rozhraní příkazového řádku Azure](../xplat-cli-install.md) a postupujte podle pokynů až do chvíle, kdy můžete vybrat svůj účet a předplatné Azure.
### Krok 2

Spuštěním příkazu **azure config mode** přejděte do režimu Resource Manager, jak vidíte níže.

    azure config mode arm

Toto je očekávaný výstup výše uvedeného příkazu:

    info:   New mode is arm

### Krok 3

Pokud je to potřeba, vytvořte novou skupinu prostředků spuštěním příkazu **azure group create**, jak vidíte níže. Prohlédněte si výstup příkazu. Seznam uvedený za výstupem vysvětluje použité parametry. Další informace o skupinách prostředků najdete v článku [Přehled Azure Resource Manageru](../resource-group-overview.md).

    azure group create -n appgatewayRG -l eastus

**-n (nebo --name)**. Název nové skupiny prostředků. V našem scénáři je to název *appgatewayRG*.

**-l (nebo --location)**. Oblast Azure, ve které je vytvořena nová skupina prostředků. V našem scénáři je to *eastus*.

### Krok 4

Spuštěním rutiny **azure group deployment create** nasadíte novou virtuální síť pomocí šablony a souborů parametrů, které jste stáhli a upravili v předchozích krocích. Seznam uvedený za výstupem vysvětluje použité parametry.

    azure group deployment create -g appgatewayRG -n TestAppgatewayDeployment -f C:\ARM\azuredeploy.json -e C:\ARM\azuredeploy-parameters.json

Toto je očekávaný výstup výše uvedeného příkazu:

    azure group deployment create -g appgatewayRG -n TestAppgatewayDeployment -f C:\ARM\azuredeploy.json -e C:\ARM\azuredeploy-parameters.json
    info:    Executing command group deployment create
    + Initializing template configurations and parameters
    + Creating a deployment
    info:    Created template deployment "TestAppgatewayDeployment"
    + Waiting for deployment to complete
    data:    DeploymentName     : TestAppgatewayDeployment
    data:    ResourceGroupName  : appgatewayRG
    data:    ProvisioningState  : Succeeded
    data:    Timestamp          : 2015-09-21T20:50:27.5129912Z
    data:    Mode               : Incremental
    data:    Name               Type    Value
    data:    -----------------  ------  --------------
    data:    location           String  East US
    data:    addressPrefix      String  10.0.0.0/16
    data:    subnetPrefix       String  10.0.0.0/24
    data:    skuName            String  Standard_Small
    data:    capacity           Int     2
    data:    backendIpAddress1  String  10.0.1.10
    data:    backendIpAddress2  String  10.0.1.11
    info:    group deployment create command OK

**-g (nebo --resource-group)**. Název skupiny prostředků, ve které je vytvořena nová virtuální síť.

**-f (nebo --template-file)**. Cesta k souboru šablony Azure Resource Manageru.

**-e (nebo --parameters-file)**. Cesta k souboru parametrů Azure Resource Manageru.

## Nasazení šablony Azure Resource Manageru pomocí metody Click to Deploy

Metoda Click to Deploy je další způsob použití šablon Azure Resource Manageru. Je to snadný způsob, jak používat šablony na webu Azure Portal.


### Krok 1
Přejděte na stránku [Create an application gateway with public IP](https://azure.microsoft.com/documentation/templates/101-application-gateway-public-ip/) (Vytvořit službu Application Gateway s veřejnou IP adresou).


### Krok 2

Klikněte na **Nasadit do Azure**.

![Nasazení do Azure](./media/application-gateway-create-gateway-arm-template/deploytoazure.png)

### Krok 3

Zadejte na portálu parametry šablony nasazení a klikněte na **OK**.

![Parametry](./media/application-gateway-create-gateway-arm-template/ibiza1.png)

### Krok 4

Vyberte **Právní podmínky** a klikněte na **Koupit**.

### Krok 5

V okně Vlastní nasazení klikněte na **Vytvořit**.



## Další kroky

Pokud chcete konfigurovat přesměrování zpracování SSL, přejděte do části [Konfigurace aplikační brány pro přesměrování zpracování SSL](application-gateway-ssl.md).

Pokud chcete provést konfiguraci aplikační brány pro použití s interním nástrojem pro vyrovnávání zatížení, přečtěte si část [Vytvoření aplikační brány s interním nástrojem pro vyrovnávání zatížení (ILB)](application-gateway-ilb.md).

Pokud chcete další informace o obecných možnostech vyrovnávání zatížení, přečtěte si část:

- [Nástroj pro vyrovnávání zatížení Azure](https://azure.microsoft.com/documentation/services/load-balancer/)
- [Azure Traffic Manager](https://azure.microsoft.com/documentation/services/traffic-manager/)



<!--HONumber=Aug16_HO4-->


