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
ms.openlocfilehash: faddb73522200f60f18294dc43e8d235943f8bbb
ms.sourcegitcommit: f46cbcff710f590aebe437c6dd459452ddf0af09
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 12/20/2017
---
# <a name="zip-push-deployment-for-azure-functions"></a>ZIP nabízené nasazení pro Azure Functions 
Tento článek popisuje postup nasazení soubory projektu funkce aplikace do Azure ze souboru ZIP (komprimované). Zjistíte, jak provést nasazení push, pomocí rozhraní příkazového řádku Azure i pomocí rozhraní REST API. 

Azure Functions nabízí celou řadu průběžné možnosti nasazení a integraci, které jsou k dispozici ve službě Azure App Service. Další informace najdete v tématu [průběžné nasazování pro Azure Functions](functions-continuous-deployment.md). 

Pro rychlejší iterace během vývoje je často snazší nasazování soubory projektu funkce aplikace přímo z komprimovaný soubor ZIP. Toto nasazení soubor .zip používá stejnou službu Kudu, že zajišťuje nepřetržité integrace nasazení na základě, včetně:

+ Odstranění souborů, které byly ponechány z dřívějších nasazení.
+ Vlastní nastavení nasazení, včetně spouštění skriptů nasazení.
+ Protokoly nasazení.
+ Synchronizuje se aktivuje funkce v [plánu spotřeby](functions-scale.md) aplikaci funkce.

Další informace najdete v tématu [odkaz nasazení nabízené .zip](https://github.com/projectkudu/kudu/wiki/Deploying-from-a-zip-file). 

## <a name="deployment-zip-file-requirements"></a>Požadavky na soubor .zip nasazení
Soubor .zip, který používáte pro nasazení nabízené musí obsahovat všechny soubory projektu ve vaší aplikaci funkce, včetně kódu funkce. 

>[!IMPORTANT]
> Pokud použijete nasazení nabízené .zip, se odstraní všechny soubory ze stávajícího nasazení, které se nenacházejí v souboru ZIP z vaší aplikace. funkce.  

### <a name="function-app-folder-structure"></a>Struktura složek aplikace – funkce

[!INCLUDE [functions-folder-structure](../../includes/functions-folder-structure.md)]

### <a name="download-your-function-app-project"></a>Stáhněte si svůj projekt aplikace – funkce

Při vývoji na místním počítači, je snadné vytvořit soubor .zip funkce složky projektu aplikace na vašem vývojovém počítači. 

Možná jste ale vytvořili funkcí pomocí editoru na portálu Azure. Stažení projektu funkce aplikace z portálu: 

1. Přihlaste se k [portál Azure](https://portal.azure.com)a poté přejděte k vaší aplikaci funkce.

2. Na **přehled** vyberte **stažení obsahu aplikace**. Vyberte možnosti stahování a pak vyberte **Stáhnout**.     

    ![Stáhněte si projekt aplikace – funkce](./media/deployment-zip-push/download-project.png)

Soubor stažený .zip je ve správném formátu znovu ji publikovat do funkce aplikace pomocí nabízených nasazení .zip.

Soubor ZIP můžete také stáhnout z úložiště Githubu. Uvědomte si, že když si stáhnete úložiště GitHub jako soubor ZIP, Githubu přidá úroveň další složky pro větev. To další složky úrovně znamená, že nemůžete nasadit přímo jako soubor .zip ji stáhnout z webu GitHub. Pokud používáte úložiště GitHub udržovat funkce aplikace, měli byste použít [průběžnou integraci](functions-continuous-deployment.md) k nasazení své aplikace.  

## <a name="cli"></a>Nasazení pomocí rozhraní příkazového řádku Azure

Rozhraní příkazového řádku Azure můžete použít k aktivaci nabízených nasazení. Soubor ZIP aplikace funkce nabízené nasadit pomocí [az functionapp nasazení zdroj konfigurace zip](/cli/azure/functionapp/deployment/source#az_functionapp_deployment_source_config_zip) příkaz. Chcete-li použít tento příkaz, musíte použít Azure CLI verze 2.0.21 nebo novější. Pokud chcete zjistit, jaká verze rozhraní příkazového řádku Azure používáte, použijte `az --version` příkaz.

V následujícím příkazu nahraďte `<zip_file_path>` zástupný symbol cestu k umístění ve vašem souboru ZIP. Také nahraďte `<app_name>` s jedinečným názvem aplikace funkce. 

```azurecli-interactive
az functionapp deployment source config-zip  -g myResourceGroup -n \
<app_name> --src <zip_file_path>
```
Tento příkaz nasadí soubory projektu ze souboru ZIP staženého do funkce aplikace v Azure. Následně restartuje aplikace. Chcete-li zobrazit seznam nasazení pro tuto aplikaci funkce, je nutné použít rozhraní REST API.

Pokud používáte rozhraní příkazového řádku Azure v místním počítači, `<zip_file_path>` je cesta k souboru ZIP ve vašem počítači. Rozhraní příkazového řádku Azure můžete také spouštět [prostředí cloudu Azure](../cloud-shell/overview.md). Pokud používáte cloudové prostředí, musíte nejprve nahrát soubor ZIP nasazení Azure Files účtu, který je spojen s své cloudové prostředí. V takovém případě `<zip_file_path>` je umístění úložiště, které používá váš účet cloudové prostředí. Další informace najdete v tématu [zachovat soubory v prostředí cloudu Azure](../cloud-shell/persisting-shell-storage.md).


[!INCLUDE [app-service-deploy-zip-push-rest](../../includes/app-service-deploy-zip-push-rest.md)]

[!INCLUDE [app-service-deploy-zip-push-custom](../../includes/app-service-deploy-zip-push-custom.md)]

## <a name="next-steps"></a>Další kroky

> [!div class="nextstepaction"]
> [Průběžné nasazování se službou Azure Functions](functions-continuous-deployment.md)

[.zip push deployment reference topic]: https://github.com/projectkudu/kudu/wiki/Deploying-from-a-zip-file
