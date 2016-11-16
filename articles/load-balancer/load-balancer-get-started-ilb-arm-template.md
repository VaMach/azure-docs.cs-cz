---
title: "Vytvoření interního nástroje pro vyrovnávání zatížení pomocí šablony v Resource Manageru | Dokumentace Microsoftu"
description: "Zjistěte, jak vytvořit interní nástroj pro vyrovnávání zatížení pomocí šablony v Resource Manageru"
services: load-balancer
documentationcenter: na
author: sdwheeler
manager: carmonm
editor: 
tags: azure-resource-manager
ms.assetid: 64150862-6ced-42de-85dc-89d323257d7c
ms.service: load-balancer
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 10/24/2016
ms.author: sewhee
translationtype: Human Translation
ms.sourcegitcommit: 2ea002938d69ad34aff421fa0eb753e449724a8f
ms.openlocfilehash: 482c9cd46902d9e3f4e1e0f001182fdb43ce9367


---
# <a name="create-an-internal-load-balancer-using-a-template"></a>Vytvoření interního nástroje pro vyrovnávání zatížení pomocí šablony
[!INCLUDE [load-balancer-get-started-ilb-arm-selectors-include.md](../../includes/load-balancer-get-started-ilb-arm-selectors-include.md)]

<BR>
[!INCLUDE [load-balancer-get-started-ilb-intro-include.md](../../includes/load-balancer-get-started-ilb-intro-include.md)]

[!INCLUDE [azure-arm-classic-important-include](../../includes/learn-about-deployment-models-rm-include.md)]

[model nasazení Classic](load-balancer-get-started-ilb-classic-ps.md)

[!INCLUDE [load-balancer-get-started-ilb-scenario-include.md](../../includes/load-balancer-get-started-ilb-scenario-include.md)]

## <a name="deploy-the-template-by-using-click-to-deploy"></a>Nasazení šablony pomocí metody Click to Deploy
Ukázková šablona, která je k dispozici ve veřejném úložišti, používá soubor parametrů obsahující výchozí hodnoty, které se použijí k vygenerování výše popsaného scénáře. Pokud chcete nasadit tuto šablonu pomocí metody Click to Deploy, pokračujte na [tento odkaz](https://github.com/Azure/azure-quickstart-templates/tree/master/201-2-vms-internal-load-balancer), klikněte na **Nasadit do Azure**, v případě potřeby nahraďte výchozí hodnoty parametrů, a pokračujte podle pokynů na portálu.

## <a name="deploy-the-template-by-using-powershell"></a>Nasazení šablony pomocí prostředí PowerShell
Pokud chcete nasadit šablonu, kterou jste stáhli, pomocí prostředí PowerShell, použijte následující postup.

1. Pokud jste prostředí Azure PowerShell nikdy nepoužívali, přejděte na téma [Instalace a konfigurace prostředí Azure PowerShell](../powershell-install-configure.md) a proveďte všechny pokyny, abyste se mohli přihlásit k Azure a vybrat své předplatné.
2. Stáhněte si soubor parametrů na místní disk.
3. Upravte soubor a uložte ho.
4. Spusťte rutinu **New-AzureRmResourceGroupDeployment**, která pomocí šablony vytvoří skupinu prostředků.
   
        New-AzureRmResourceGroupDeployment -Name TestRG -Location westus `
            -TemplateFile 'https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/201-2-vms-internal-load-balancer/azuredeploy.json' `
            -TemplateParameterFile 'C:\temp\azuredeploy.parameters.json'

## <a name="deploy-the-template-by-using-the-azure-cli"></a>Nasazení šablony pomocí rozhraní příkazového řádku Azure
Pokud chcete nasadit šablonu pomocí rozhraní příkazového řádku Azure, použijte následující postup.

1. Pokud jste rozhraní příkazového řádku Azure nikdy nepoužívali, přejděte na téma [Instalace a konfigurace rozhraní příkazového řádku Azure](../xplat-cli-install.md) a postupujte podle pokynů až do chvíle, kdy můžete vybrat svůj účet a předplatné Azure.
2. Spuštěním příkazu **azure config mode** přejděte do režimu Resource Manager, jak vidíte níže.
   
        azure config mode arm
   
    Toto je očekávaný výstup výše uvedeného příkazu:
   
        info:    New mode is arm
3. Otevřete soubor parametrů, vyberte jeho obsah a ten uložte do souboru ve svém počítači. V tomto příkladu jsme uložili soubor parametrů do souboru *parameters.json*.
4. Spuštěním příkazu **azure group deployment create** nasaďte nový interní nástroj pro vyrovnávání zatížení pomocí šablony a souborů parametrů, které jste stáhli a upravili v předchozích krocích. Seznam uvedený za výstupem vysvětluje použité parametry.
   
        azure group create --name TestRG --location westus --template-uri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/201-2-vms-internal-load-balancer/azuredeploy.json --parameters-file parameters.json

## <a name="next-steps"></a>Další kroky
[Konfigurace distribučního režimu nástroje pro vyrovnávání zatížení pomocí spřažení se zdrojovou IP adresou](load-balancer-distribution-mode.md)

[Konfigurace nastavení časového limitu nečinnosti protokolu TCP pro nástroj pro vyrovnávání zatížení](load-balancer-tcp-idle-timeout.md)




<!--HONumber=Nov16_HO2-->


