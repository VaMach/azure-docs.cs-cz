---
title: "ZIP nabízené nasazení pro Azure Functions | Microsoft Docs"
description: "Vlastnosti nasazení soubor .zip služby nasazení Kudu použijte k publikování Azure Functions."
services: functions
documentationcenter: na
author: ggailey777
manager: cfowler
editor: 
tags: 
ms.service: functions
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: multiple
ms.workload: na
ms.date: 12/06/2017
ms.author: glenga
ms.openlocfilehash: 5c8c608126f20aa5f5dc52bb8e24cfec14fd00f5
ms.sourcegitcommit: 4ac89872f4c86c612a71eb7ec30b755e7df89722
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 12/07/2017
---
# <a name="zip-push-deployment-for-azure-functions"></a>ZIP nabízené nasazení pro Azure Functions 
Toto téma popisuje postup nasazení soubory projektu funkce aplikace do Azure ze souboru ZIP (komprimované). Zjistíte, jak to provést nabízené nasazení pomocí rozhraní příkazového řádku Azure a rozhraní REST API. Azure Functions nabízí celou řadu průběžné nasazování a možností integrace poskytovaných službou Azure App Service, další informace najdete v tématu [průběžné nasazování pro Azure Functions](functions-continuous-deployment.md). Pro rychlejší iterace během vývoje, je však často snazší nasazování soubory projektu funkce aplikace přímo z komprimovaný soubor ZIP.

Toto nasazení soubor .zip používá stejnou Kudu službu této zajišťuje nepřetržité integrace nasazení na základě, která zahrnuje následující funkce:

+ Odstranění souborů pozůstatek předchozího nasazení.
+ Vlastní nastavení nasazení, včetně spouštění skriptů nasazení.
+ Protokoly nasazení.
+ Aktivace funkce v synchronizaci [plánu spotřeby](functions-scale.md) aplikaci funkce.

Další informace najdete v tématu [.zip nabízené nasazení referenční téma]. 

## <a name="requirements-of-the-deployment-zip-file"></a>Požadavky na nasazení souboru ZIP 
Soubor .zip, který používáte pro nasazení nabízené musí obsahuje všechny soubory projektu ve vaší aplikaci funkce, včetně kódu funkce. 

>[!IMPORTANT]
> Pokud použijete nasazení nabízené .zip, jsou odstraněny soubory ze stávajícího nasazení nebyl nalezen v souboru ZIP z vaší aplikace. funkce.  

### <a name="function-app-folder-structure"></a>Struktura složek aplikace – funkce

[!INCLUDE [functions-folder-structure](../../includes/functions-folder-structure.md)]

### <a name="downloading-your-function-app-project"></a>Stahování projektu aplikace – funkce

Při provádění místní vývoj, je snadné vytvořit soubor .zip funkce složky projektu aplikace na vašem vývojovém počítači. Však může také vytvoříte funkcí pomocí editoru v portálu. Stažení projektu funkce aplikace z portálu: 

1. Přihlaste se k [portál Azure](https://portal.azure.com) a přejděte do aplikaci funkce.

2. V **přehled** vyberte **stažení obsahu aplikace**, vyberte z možností stažení a klikněte na **Stáhnout**.     

    ![Stáhněte si projekt aplikace – funkce](./media/deployment-zip-push/download-project.png)

Soubor stažený .zip je ve správném formátu znovu ji publikovat do funkce aplikace pomocí nabízených nasazení .zip.

GitHub také umožňuje stáhnout soubor .zip z úložiště. Mějte na paměti, když si stáhnete úložiště GitHub jako soubor ZIP, Githubu přidá další složky úrovně pro větev, což znamená, že nemůžete nasadit soubor .zip stáhnout z Githubu, jako je. Používáte-li udržovat aplikace funkce úložiště GitHub, měli byste použít [průběžnou integraci](functions-continuous-deployment.md) k nasazení své aplikace.  

## <a name="cli"></a>Nasazení pomocí rozhraní příkazového řádku Azure

Rozhraní příkazového řádku Azure můžete použít k aktivaci nabízených nasazení. Soubor ZIP aplikace funkce nabízené nasadit pomocí [az functionapp nasazení zdroj konfigurace zip](/cli/azure/functionapp/deployment/source#az_functionapp_deployment_source_config_zip) příkaz. Je nutné použít Azure CLI verze 2.0.21 nebo novější. Použití `az --version` příkazu zobrazte jaká verze, kterou používáte.

V následujícím příkazu nahraďte `<zip_file_path>` zástupný symbol cestu k umístění ve vašem souboru ZIP. Také nahraďte `<app_name>` s jedinečným názvem aplikace funkce. 

```azurecli-interactive
az functionapp deployment source config-zip  -g myResourceGroup -n \
<app_name> --src <zip_file_path>
```
Tento příkaz nasadí soubory projektu ze souboru ZIP staženého do funkce aplikace v Azure a restartování aplikace. Chcete-li zobrazit seznam nasazení pro tuto aplikaci funkce, je nutné použít rozhraní REST API.

Pokud používáte Azure CLI v místním počítači, `<zip_file_path>` je cesta k souboru ZIP ve vašem počítači. Rozhraní příkazového řádku Azure můžete také spouštět [prostředí cloudu Azure](../cloud-shell/overview.md). Pokud používáte cloudové prostředí, musíte nejprve nahrát soubor ZIP nasazení do Azure File účet přidružený ke své cloudové prostředí. V takovém případě `<zip_file_path>` je umístění úložiště používané účtu cloudové prostředí. Další informace najdete v tématu [zachovat soubory v prostředí cloudu Azure](../cloud-shell/persisting-shell-storage.md).


[!INCLUDE [app-service-deploy-zip-push-rest](../../includes/app-service-deploy-zip-push-rest.md)]

[!INCLUDE [app-service-deploy-zip-push-custom](../../includes/app-service-deploy-zip-push-custom.md)]

## <a name="next-steps"></a>Další kroky

> [!div class="nextstepaction"]
> [Průběžné nasazování se službou Azure Functions](functions-continuous-deployment.md)

[.zip nabízené nasazení referenční téma]: https://github.com/projectkudu/kudu/wiki/Deploying-from-a-zip-file
