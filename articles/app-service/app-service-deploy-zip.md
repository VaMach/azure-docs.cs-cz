---
title: "Nasazení aplikace do Azure App Service pomocí souboru ZIP | Microsoft Docs"
description: "Informace o nasazení aplikace do služby Azure App Service pomocí souboru ZIP."
services: app-service
documentationcenter: 
author: cephalin
manager: cfowler
editor: 
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/05/2017
ms.author: cephalin;sisirap
ms.openlocfilehash: a0e4df0ef0a1c873f1efcac1d8dbfe3cada18218
ms.sourcegitcommit: 0e4491b7fdd9ca4408d5f2d41be42a09164db775
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 12/14/2017
---
# <a name="deploy-your-app-to-azure-app-service-with-a-zip-file"></a>Nasazení aplikace do Azure App Service pomocí souboru ZIP

Tento článek ukazuje, jak použít soubor ZIP k nasazení vaší webové aplikace na [Azure App Service](app-service-web-overview.md). 

Toto nasazení souboru ZIP používá stejnou službu Kudu nasazení průběžné na základě integrace této zajišťuje. Kudu podporuje následující funkce pro nasazení soubor ZIP: 

- Odstraňování souborů zbyly z předchozí nasazení.
- Možnost Zapnout výchozí proces sestavení, která zahrnuje obnovení balíčků.
- [Vlastní nastavení nasazení](https://github.com/projectkudu/kudu/wiki/Configurable-settings#repository-and-deployment-related-settings), včetně spouštění skriptů nasazení.  
- Protokoly nasazení. 

Další informace najdete v tématu [Kudu dokumentaci](https://github.com/projectkudu/kudu/wiki/Deploying-from-a-zip-file).

## <a name="prerequisites"></a>Požadavky

Pokud chcete provést kroky v tomto článku:

* [Vytvořit aplikaci služby App Service](/azure/app-service/), nebo použijte aplikaci, kterou jste vytvořili pro jiné kurzu.

## <a name="create-a-project-zip-file"></a>Vytvořte soubor ZIP projektu

>[!NOTE]
> Pokud jste stáhli soubory v souboru ZIP, nejdřív extrahujte soubory. Například pokud jste stáhli soubor ZIP z Githubu, nemůžete nasadit tento soubor jako-je. GitHub přidá další vnořené adresáře, které nefungují službou App Service. 
>

V místní okno terminálu přejděte do kořenového adresáře projektu aplikace. 

Tento adresář musí obsahovat vstupní soubor do vaší webové aplikace, jako například _index.html_, _index.php_, a _app.js_. Může také obsahovat soubory balíčku správy jako _project.json_, _composer.json_, _package.json_, _bower.json_a _requirements.txt_.

Vytvořte archivu ZIP všechny položky ve vašem projektu. Následující příkaz použije výchozí nástroj v terminálu:

```
# Bash
zip -r <file-name>.zip .

# PowerShell
Compress-Archive -Path * -DestinationPath <file-name>.zip
``` 

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

## <a name="upload-zip-file-to-cloud-shell"></a>Nahrajte soubor ZIP do prostředí cloudu

Pokud chcete místo toho spusťte rozhraní příkazového řádku Azure z místního terminálu, tento krok přeskočit.

Postupujte podle kroků v tomto poli se nahrát váš soubor ZIP do prostředí cloudu. 

[!INCLUDE [app-service-web-upload-zip.md](../../includes/app-service-web-upload-zip-no-h.md)]

Další informace najdete v tématu [zachovat soubory v prostředí cloudu Azure](../cloud-shell/persisting-shell-storage.md).

## <a name="deploy-zip-file"></a>Nasazení souboru ZIP

Nahrávaný soubor ZIP nasazení do webové aplikace pomocí [az webapp nasazení zdroj konfigurace zip](/cli/azure/webapp/deployment/source?view=azure-cli-latest#az_webapp_deployment_source_config_zip) příkaz. Pokud se rozhodnete použít cloudové prostředí, zkontrolujte, zda vaše verze rozhraní příkazového řádku Azure je 2.0.21 nebo novější. Chcete zobrazit verzi, spustíte `az --version` příkaz v místní okno terminálu. 

Následující příklad nasadí soubor ZIP, který jste nahráli. Pokud používáte místní instalace rozhraní příkazového řádku Azure, zadejte cestu k souboru ZIP místní pro `--src`.   

```azurecli-interactive
az webapp deployment source config-zip --resource-group myResouceGroup --name <app_name> --src clouddrive/<filename>.zip
```

Tento příkaz nasadí soubory a adresáře ze souboru ZIP souborů do výchozí složky aplikace služby App Service (`\home\site\wwwroot`) a restartování aplikace. Pokud je nakonfigurovaná procesu žádné další vlastní sestavení, je také spustit. Další informace najdete v tématu [Kudu dokumentaci](https://github.com/projectkudu/kudu/wiki/Deploying-from-a-zip-file).

Chcete-li zobrazit seznam nasazení pro tuto aplikaci, musíte použít rozhraní API REST (viz další část). 

[!INCLUDE [app-service-deploy-zip-push-rest](../../includes/app-service-deploy-zip-push-rest.md)]  

## <a name="next-steps"></a>Další kroky

Pro pokročilejší scénáře nasazení, zkuste [nasazení do Azure s Gitem](app-service-deploy-local-git.md). Na základě Git nasazení do Azure umožňuje verzí, obnovení balíčků, MSBuild a další.

## <a name="more-resources"></a>Další zdroje informací

* [Kudu: Nasazení ze souboru zip](https://github.com/projectkudu/kudu/wiki/Deploying-from-a-zip-file)
* [Přihlašovací údaje pro nasazení služby Azure App Service](app-service-deploy-ftp.md)
