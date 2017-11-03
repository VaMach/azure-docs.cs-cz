---
title: "Vytváření clusterů systému Hadoop pomocí šablon - Azure HDInsight | Microsoft Docs"
description: "Naučte se vytvářet clustery pro HDInsight pomocí šablony Resource Manageru"
services: hdinsight
documentationcenter: 
tags: azure-portal
author: mumian
manager: jhubbard
editor: cgronlun
ms.assetid: 00a80dea-011f-44f0-92a4-25d09db9d996
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 06/30/2017
ms.author: jgao
ms.openlocfilehash: 82733e2a3025f932961122bad9d70c26896837b7
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/11/2017
---
# <a name="create-hadoop-clusters-in-hdinsight-by-using-resource-manager-templates"></a>Vytvoření clusterů systému Hadoop v HDInsight pomocí šablony Resource Manageru
[!INCLUDE [selector](../../includes/hdinsight-create-linux-cluster-selector.md)]

V tomto článku se dozvíte několik způsobů, jak vytvořit clustery se Azure HDInsight pomocí šablony Azure Resource Manager. Další informace najdete v tématu [nasazení aplikace pomocí šablony Azure Resource Manageru](../azure-resource-manager/resource-group-template-deploy.md). Další informace o dalších funkcí a nástrojů pro vytváření clusteru, klikněte na tlačítko volič karty v horní této stránce nebo v tématu [metody vytváření clusterů](hdinsight-hadoop-provision-linux-clusters.md#cluster-setup-methods).

## <a name="prerequisites"></a>Požadavky
[!INCLUDE [delete-cluster-warning](../../includes/hdinsight-delete-cluster-warning.md)]

Podle pokynů v tomto článku, budete potřebovat:

* [Předplatné](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).
* Prostředí Azure PowerShell nebo Azure CLI.

[!INCLUDE [use-latest-version](../../includes/hdinsight-use-latest-powershell-and-cli.md)]

### <a name="resource-manager-templates"></a>Šablony Resource Manageru
Šablonu Resource Manager umožňuje snadné vytváření následující pro vaši aplikaci v rámci jediné koordinované operace:
* Clustery prostředí HDInsight a jejich závislé prostředky (například výchozí účet úložiště)
* Další prostředky (například Azure SQL Database k použití Apache Sqoop)

V šabloně definujete prostředky, které jsou potřebné pro aplikaci. Můžete určit taky parametry nasazení pro vstupní hodnoty pro různá prostředí. Šablona se skládá z JSON a výrazy, které můžete použít k vytvoření hodnot pro vaše nasazení.

Můžete najít ukázky šablony HDInsight v [šablon Azure rychlý Start](https://azure.microsoft.com/resources/templates/?term=hdinsight). Použít napříč platformami [Visual Studio Code](https://code.visualstudio.com/#alt-downloads) s [Resource Manager rozšíření](https://marketplace.visualstudio.com/items?itemName=msazurermtools.azurerm-vscode-tools) nebo textovém editoru a uložit šablonu do souboru na pracovní stanici. Zjistíte, jak volat šablony pomocí různých metod.

Další informace o šablonách Resource Manager najdete v následujících článcích:

* [Vytváření šablon Azure Resource Manageru](../azure-resource-manager/resource-group-authoring-templates.md)
* [Nasazení aplikace pomocí šablony Azure Resource Manager](../azure-resource-manager/resource-group-template-deploy.md)

## <a name="generate-templates"></a>Generování šablon

Pomocí portálu Azure, můžete konfigurovat vlastnosti clusteru a potom uložte šablonu ještě před nasazením. Pak můžete znovu použít šablonu.

**Ke generování šablony pomocí portálu Azure**

1. Přihlaste se k webu [Azure Portal](https://portal.azure.com).
2. Klikněte na tlačítko **nový** v levé nabídce klikněte na tlačítko **Intelligence + analýzy**a potom klikněte na **HDInsight**.
3. Postupujte podle pokynů a zadejte vlastnosti. Můžete použít buď **rychle vytvořit** nebo **vlastní** možnost.
4. Na **Souhrn** , klikněte na **stáhnout šablonu a parametry**:

    ![Vytvoření stažení šablony správce prostředků clusteru HDInsight Hadoop](./media/hdinsight-hadoop-create-linux-clusters-arm-templates/hdinsight-create-cluster-resource-manager-template-download.png)

    Zobrazí seznam soubor šablony, soubor parametrů a ukázky kódu, které jsou používány k nasazení šablony:

    ![HDInsight Hadoop vytvoření clusteru možnosti stahování šablony Resource Manageru](./media/hdinsight-hadoop-create-linux-clusters-arm-templates/hdinsight-create-cluster-resource-manager-template-download-options.png)

    Tady můžete šablonu stáhnout, uložit do knihovny šablony nebo nasazení šablony.

    Chcete-li získat přístup k šabloně v knihovně, klikněte na tlačítko **další služby** z levé nabídce a pak klikněte na tlačítko **šablony** (v části **jiných** kategorie).

    > [!Note]
    > Soubor šablony a parametry se musí použít společně. Jinak můžete získat neočekávané výsledky. Například výchozí **clusterKind** hodnota vlastnosti je vždy **hadoop**, bez ohledu na tom, co jste zadejte před stažením šablony.



## <a name="deploy-with-powershell"></a>Nasazení s využitím PowerShellu

Tento postup vytvoří Hadoop cluster v HDInsight.

1. Uložení souboru JSON v [příloha](#appx-a-arm-template) do pracovní stanice. Ve skriptu prostředí PowerShell, je název souboru `C:\HDITutorials-ARM\hdinsight-arm-template.json`.
2. V případě potřeby nastavte parametry a proměnné.
3. Spustíte šablonu pomocí následujícího skriptu prostředí PowerShell:

        ####################################
        # Set these variables
        ####################################
        #region - used for creating Azure service names
        $nameToken = "<Enter an Alias>"
        $templateFile = "C:\HDITutorials-ARM\hdinsight-arm-template.json"
        #endregion

        ####################################
        # Service names and variables
        ####################################
        #region - service names
        $namePrefix = $nameToken.ToLower() + (Get-Date -Format "MMdd")

        $resourceGroupName = $namePrefix + "rg"
        $hdinsightClusterName = $namePrefix + "hdi"
        $defaultStorageAccountName = $namePrefix + "store"
        $defaultBlobContainerName = $hdinsightClusterName

        $location = "East US 2"

        $armDeploymentName = $namePrefix
        #endregion

        ####################################
        # Connect to Azure
        ####################################
        #region - Connect to Azure subscription
        Write-Host "`nConnecting to your Azure subscription ..." -ForegroundColor Green
        try{Get-AzureRmContext}
        catch{Login-AzureRmAccount}
        #endregion

        # Create a resource group
        New-AzureRmResourceGroup -Name $resourceGroupName -Location $Location

        # Create cluster and the dependent storage account
        $parameters = @{clusterName="$hdinsightClusterName"}

        New-AzureRmResourceGroupDeployment `
            -Name $armDeploymentName `
            -ResourceGroupName $resourceGroupName `
            -TemplateFile $templateFile `
            -TemplateParameterObject $parameters

        # List cluster
        Get-AzureRmHDInsightCluster -ResourceGroupName $resourceGroupName -ClusterName $hdinsightClusterName

    Skript prostředí PowerShell lze konfigurovat pouze název clusteru. Název účtu úložiště je pevně zakódovaná v šabloně. Zobrazí se výzva k zadání hesla uživatele clusteru. (Výchozí uživatelské jméno **správce**.) Také budete vyzváni k zadání hesla uživatele SSH. (Výchozí uživatelské jméno SSH **sshuser**.)  

Další informace najdete v tématu [nasadit v prostředí PowerShell](../azure-resource-manager/resource-group-template-deploy.md#deploy-local-template).

## <a name="deploy-with-cli"></a>Nasazení pomocí rozhraní příkazového řádku
Následující příklad používá rozhraní příkazového řádku Azure (CLI). Vytvoří cluster a jeho účet závislého úložiště a kontejneru voláním šablony Resource Manageru:

    azure login
    azure config mode arm
    azure group create -n hdi1229rg -l "East US"
    azure group deployment create --resource-group "hdi1229rg" --name "hdi1229" --template-file "C:\HDITutorials-ARM\hdinsight-arm-template.json"

Zobrazí se výzva k zadání:
* Název clusteru.
* Heslo uživatele clusteru. (Výchozí uživatelské jméno **správce**.)
* Heslo uživatele SSH. (Výchozí uživatelské jméno SSH **sshuser**.)

Následující kód obsahuje vložené parametry:

    azure group deployment create --resource-group "hdi1229rg" --name "hdi1229" --template-file "c:\Tutorials\HDInsightARM\create-linux-based-hadoop-cluster-in-hdinsight.json" --parameters '{\"clusterName\":{\"value\":\"hdi1229\"},\"clusterLoginPassword\":{\"value\":\"Pass@word1\"},\"sshPassword\":{\"value\":\"Pass@word1\"}}'

## <a name="deploy-with-the-rest-api"></a>Nasazení pomocí rozhraní REST API
V tématu [nasazení pomocí rozhraní REST API](../azure-resource-manager/resource-group-template-deploy-rest.md).

## <a name="deploy-with-visual-studio"></a>Nasazení s využitím sady Visual Studio
 Pomocí sady Visual Studio vytvořte projekt skupiny prostředků a nasaďte ho do Azure v uživatelském rozhraní. Vyberete typ zdroje, které chcete zahrnout do projektu. Tyto prostředky se automaticky přidají do šablony Resource Manageru. Projekt také obsahuje skript prostředí PowerShell k nasazení šablony.

Úvod do skupiny prostředků pomocí sady Visual Studio, najdete v části [vytvoření a nasazení skupin prostředků Azure pomocí sady Visual Studio](../azure-resource-manager/vs-azure-tools-resource-groups-deployment-projects-create-deploy.md).

## <a name="troubleshoot"></a>Řešení potíží

Pokud narazíte na problémy s vytvářením clusterů HDInsight, podívejte se na [požadavky na řízení přístupu](hdinsight-administer-use-portal-linux.md#create-clusters).

## <a name="next-steps"></a>Další kroky
V tomto článku jste se naučili několik způsobů, jak vytvořit cluster služby HDInsight. Další informace naleznete v následujících článcích:

* Příklad nasazení prostředků prostřednictvím klientské knihovny .NET, naleznete v části [nasadit prostředky pomocí knihovny .NET a šablonu](../virtual-machines/windows/csharp-template.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).
* Podrobný příklad nasazení aplikace naleznete v tématu [zřídit a nasadit mikroslužeb předvídatelné v Azure](../app-service/app-service-deploy-complex-application-predictably.md).
* Pokyny pro nasazení řešení do různých prostředí najdete v článku věnovaném [testovacím a vývojovým prostředím v Microsoft Azure](../solution-dev-test-environments.md).
* Další informace o části šablony Azure Resource Manageru najdete v tématu [vytváření šablon](../azure-resource-manager/resource-group-authoring-templates.md).
* Seznam funkcí v šablonu Azure Resource Manager můžete použít, najdete v části [funkce šablon](../azure-resource-manager/resource-group-template-functions.md).

## <a name="appendix-resource-manager-template-to-create-a-hadoop-cluster"></a>Dodatek: Šablony Resource Manageru k vytvoření clusteru Hadoop
Následující šablony Azure Resource Manager vytvoří cluster systémem Linux Hadoop se účet závislého úložiště Azure.

> [!NOTE]
> Tato ukázka obsahuje informace o konfiguraci pro metaúložiště Hive a metaúložiště Oozie. Odebrat oddíl nebo nakonfigurujte části před použitím šablony.
>
>

    {
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "clusterName": {
        "type": "string",
        "metadata": {
            "description": "The name of the HDInsight cluster to create."
        }
        },
        "clusterLoginUserName": {
        "type": "string",
        "defaultValue": "admin",
        "metadata": {
            "description": "These credentials can be used to submit jobs to the cluster and to log into cluster dashboards."
        }
        },
        "clusterLoginPassword": {
        "type": "securestring",
        "metadata": {
            "description": "The password must be at least 10 characters in length and must contain at least one digit, one non-alphanumeric character, and one upper or lower case letter."
        }
        },
        "sshUserName": {
        "type": "string",
        "defaultValue": "sshuser",
        "metadata": {
            "description": "These credentials can be used to remotely access the cluster."
        }
        },
        "sshPassword": {
        "type": "securestring",
        "metadata": {
            "description": "The password must be at least 10 characters in length and must contain at least one digit, one non-alphanumeric character, and one upper or lower case letter."
        }
        },
        "location": {
        "type": "string",
        "defaultValue": "East US",
        "allowedValues": [
            "East US",
            "East US 2",
            "North Central US",
            "South Central US",
            "West US",
            "North Europe",
            "West Europe",
            "East Asia",
            "Southeast Asia",
            "Japan East",
            "Japan West",
            "Australia East",
            "Australia Southeast"
        ],
        "metadata": {
            "description": "The location where all azure resources will be deployed."
        }
        },
        "clusterType": {
        "type": "string",
        "defaultValue": "hadoop",
        "allowedValues": [
            "hadoop",
            "hbase",
            "storm",
            "spark"
        ],
        "metadata": {
            "description": "The type of the HDInsight cluster to create."
        }
        },
        "clusterWorkerNodeCount": {
        "type": "int",
        "defaultValue": 2,
        "metadata": {
            "description": "The number of nodes in the HDInsight cluster."
        }
        }
    },
    "variables": {
        "defaultApiVersion": "2015-05-01-preview",
        "clusterApiVersion": "2015-03-01-preview",
        "clusterStorageAccountName": "[concat(parameters('clusterName'),'store')]"
    },
    "resources": [
        {
        "name": "[variables('clusterStorageAccountName')]",
        "type": "Microsoft.Storage/storageAccounts",
        "location": "[parameters('location')]",
        "apiVersion": "[variables('defaultApiVersion')]",
        "dependsOn": [ ],
        "tags": { },
        "properties": {
            "accountType": "Standard_LRS"
        }
        },
        {
        "name": "[parameters('clusterName')]",
        "type": "Microsoft.HDInsight/clusters",
        "location": "[parameters('location')]",
        "apiVersion": "[variables('clusterApiVersion')]",
        "dependsOn": [ "[concat('Microsoft.Storage/storageAccounts/',variables('clusterStorageAccountName'))]" ],
        "tags": {

        },
        "properties": {
            "clusterVersion": "3.4",
            "osType": "Linux",
            "tier": "standard",
            "clusterDefinition": {
            "kind": "[parameters('clusterType')]",
            "configurations": {
                "gateway": {
                "restAuthCredential.isEnabled": true,
                "restAuthCredential.username": "[parameters('clusterLoginUserName')]",
                "restAuthCredential.password": "[parameters('clusterLoginPassword')]"
                },
                "hive-site": {
                    "javax.jdo.option.ConnectionDriverName": "com.microsoft.sqlserver.jdbc.SQLServerDriver",
                    "javax.jdo.option.ConnectionURL": "jdbc:sqlserver://myadla0901dbserver.database.windows.net;database=myhive20160901;encrypt=true;trustServerCertificate=true;create=false;loginTimeout=300",
                    "javax.jdo.option.ConnectionUserName": "johndole",
                    "javax.jdo.option.ConnectionPassword": "myPassword$"
                },
                "hive-env": {
                    "hive_database": "Existing MSSQL Server database with SQL authentication",
                    "hive_database_name": "myhive20160901",
                    "hive_database_type": "mssql",
                    "hive_existing_mssql_server_database": "myhive20160901",
                    "hive_existing_mssql_server_host": "myadla0901dbserver.database.windows.net",
                    "hive_hostname": "myadla0901dbserver.database.windows.net"
                },
                "oozie-site": {
                    "oozie.service.JPAService.jdbc.driver": "com.microsoft.sqlserver.jdbc.SQLServerDriver",
                    "oozie.service.JPAService.jdbc.url": "jdbc:sqlserver://myadla0901dbserver.database.windows.net;database=myhive20160901;encrypt=true;trustServerCertificate=true;create=false;loginTimeout=300",
                    "oozie.service.JPAService.jdbc.username": "johndole",
                    "oozie.service.JPAService.jdbc.password": "myPassword$",
                    "oozie.db.schema.name": "oozie"
                },
                "oozie-env": {
                    "oozie_database": "Existing MSSQL Server database with SQL authentication",
                    "oozie_database_name": "myhive20160901",
                    "oozie_database_type": "mssql",
                    "oozie_existing_mssql_server_database": "myhive20160901",
                    "oozie_existing_mssql_server_host": "myadla0901dbserver.database.windows.net",
                    "oozie_hostname": "myadla0901dbserver.database.windows.net"
                }            
            }
            },
            "storageProfile": {
            "storageaccounts": [
                {
                "name": "[concat(variables('clusterStorageAccountName'),'.blob.core.windows.net')]",
                "isDefault": true,
                "container": "[parameters('clusterName')]",
                "key": "[listKeys(resourceId('Microsoft.Storage/storageAccounts', variables('clusterStorageAccountName')), variables('defaultApiVersion')).keys[0].value]"
                }
            ]
            },
            "computeProfile": {
            "roles": [
                {
                "name": "headnode",
                "targetInstanceCount": "2",
                "hardwareProfile": {
                    "vmSize": "Standard_D3"
                },
                "osProfile": {
                    "linuxOperatingSystemProfile": {
                    "username": "[parameters('sshUserName')]",
                    "password": "[parameters('sshPassword')]"
                    }
                }
                },
                {
                "name": "workernode",
                "targetInstanceCount": "[parameters('clusterWorkerNodeCount')]",
                "hardwareProfile": {
                    "vmSize": "Standard_D3"
                },
                "osProfile": {
                    "linuxOperatingSystemProfile": {
                    "username": "[parameters('sshUserName')]",
                    "password": "[parameters('sshPassword')]"
                    }
                }
                }
            ]
            }
        }
        }
    ],
    "outputs": {
        "cluster": {
        "type": "object",
        "value": "[reference(resourceId('Microsoft.HDInsight/clusters',parameters('clusterName')))]"
        }
    }
    }

## <a name="appendix-resource-manager-template-to-create-a-spark-cluster"></a>Dodatek: Šablony Resource Manageru k vytvoření clusteru Spark

Tato část obsahuje šablonu Resource Manager, který můžete použít k vytvoření clusteru HDInsight Spark. Tato šablona zahrnuje konfigurace pro `spark-defaults` a `spark-thrift-sparkconf` (pro clustery Spark 1.6) a `spark2-defaults` a `spark2-thrift-sparkconf` (pro clustery Spark 2). Kromě toho HDInsight vypočítá a nastaví konfigurace, jako `spark.executor.instances`, `spark.executor.memory`, a `spark.executor.cores` na základě velikosti clusteru. 

Pokud nastavíte všechny jeden parametr v části v rámci samotné šablony, HDInsight nepodporuje výpočtu a nastavit další parametry do stejné části. Například parametr `spark.executor.instances` probíhá `spark-defaults` konfigurace. Pokud nastavíte parametr jiné (například `spark.yarn.exector.memoryOverhead`) v `spark-defaults` konfigurace, HDInsight nepodporuje výpočtu a nastavit `spark.executor.instances` také parametr.

    {
    "$schema": "http://schema.management.azure.com/schemas/2014-04-01-preview/deploymentTemplate.json#",
    "contentVersion": "0.9.0.0",
    "parameters": {
        "clusterName": {
            "type": "string",
            "metadata": {
                "description": "The name of the HDInsight cluster to create."
            }
        },
        "clusterLoginUserName": {
            "type": "string",
            "defaultValue": "admin",
            "metadata": {
                "description": "These credentials can be used to submit jobs to the cluster and to log into cluster dashboards."
            }
        },
        "clusterLoginPassword": {
            "type": "securestring",
            "metadata": {
                "description": "The password must be at least 10 characters in length and must contain at least one digit, one non-alphanumeric character, and one upper or lower case letter."
            }
        },
        "location": {
            "type": "string",
            "defaultValue": "southcentralus",
            "metadata": {
                "description": "The location where all azure resources will be deployed."
            }
        },
        "clusterVersion": {
            "type": "string",
            "defaultValue": "3.5",
            "metadata": {
                "description": "HDInsight cluster version."
            }
        },
        "clusterWorkerNodeCount": {
            "type": "int",
            "defaultValue": 4,
            "metadata": {
                "description": "The number of nodes in the HDInsight cluster."
            }
        },
        "clusterKind": {
            "type": "string",
            "defaultValue": "SPARK",
            "metadata": {
                "description": "The type of the HDInsight cluster to create."
            }
        },
        "sshUserName": {
            "type": "string",
            "defaultValue": "sshuser",
            "metadata": {
                "description": "These credentials can be used to remotely access the cluster."
            }
        },
        "sshPassword": {
            "type": "securestring",
            "metadata": {
                "description": "The password must be at least 10 characters in length and must contain at least one digit, one non-alphanumeric character, and one upper or lower case letter."
            }
        }
    },
    "variables": {
        "defaultApiVersion": "2017-06-01",
        "clusterStorageAccountName": "[concat(parameters('clusterName'),'store')]"
    },
    "resources": [
        {
        "name": "[variables('clusterStorageAccountName')]",
        "type": "Microsoft.Storage/storageAccounts",
        "location": "[parameters('location')]",
        "apiVersion": "[variables('defaultApiVersion')]",
        "dependsOn": [ ],
        "tags": { },
        "properties": {
            "accountType": "Standard_LRS"
        }
        },
    {
            "apiVersion": "2015-03-01-preview",
            "name": "[parameters('clusterName')]",
            "type": "Microsoft.HDInsight/clusters",
            "location": "[parameters('location')]",
            "dependsOn": [],
            "properties": {
                "clusterVersion": "[parameters('clusterVersion')]",
                "osType": "Linux",
                "tier": "standard",
                "clusterDefinition": {
                    "kind": "[parameters('clusterKind')]",
                    "configurations": {
                        "gateway": {
                            "restAuthCredential.isEnabled": true,
                            "restAuthCredential.username": "[parameters('clusterLoginUserName')]",
                            "restAuthCredential.password": "[parameters('clusterLoginPassword')]"
                        },
                        "spark-defaults": {
                            "spark.executor.cores": "2"
                        },
                        "spark-thrift-sparkconf": {
                            "spark.yarn.executor.memoryOverhead": "896"
                        }
                    }
                },
                "storageProfile": {
                    "storageaccounts": [
                        {
                            "name": "[concat(variables('clusterStorageAccountName'),'.blob.core.windows.net')]",
                            "isDefault": true,
                            "container": "[parameters('clusterName')]",
                            "key": "[listKeys(resourceId('Microsoft.Storage/storageAccounts', variables('clusterStorageAccountName')), variables('defaultApiVersion')).keys[0].value]"
                        }
                    ]
                },
                "computeProfile": {
                    "roles": [
                        {
                            "name": "headnode",
                            "minInstanceCount": 1,
                            "targetInstanceCount": 2,
                            "hardwareProfile": {
                                "vmSize": "Standard_D12"
                            },
                            "osProfile": {
                                "linuxOperatingSystemProfile": {
                                    "username": "[parameters('sshUserName')]",
                                    "password": "[parameters('sshPassword')]"
                                }
                            },
                            "virtualNetworkProfile": null,
                            "scriptActions": []
                        },
                        {
                            "name": "workernode",
                            "minInstanceCount": 1,
                            "targetInstanceCount": 4,
                            "hardwareProfile": {
                                "vmSize": "Standard_D4"
                            },
                            "osProfile": {
                                "linuxOperatingSystemProfile": {
                                    "username": "[parameters('sshUserName')]",
                                    "password": "[parameters('sshPassword')]"
                                    }
                                },
                                "virtualNetworkProfile": null,
                                "scriptActions": []
                            }
                        ]
                    }
                }
            }
        ]
    }
