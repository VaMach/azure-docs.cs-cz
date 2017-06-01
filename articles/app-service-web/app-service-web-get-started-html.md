---
title: "Vytvoření webové aplikace ve statickém HTML v Azure během pěti minut | Dokumentace Microsoftu"
description: "Nasazením ukázkové aplikace zjistíte, jak snadné je spustit webové aplikace ve službě App Service."
services: app-service\web
documentationcenter: 
author: rick-anderson
manager: wpickett
editor: 
ms.assetid: 60495cc5-6963-4bf0-8174-52786d226c26
ms.service: app-service-web
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: hero-article
ms.date: 05/08/2017
ms.author: riande
ms.custom: mvc
ms.translationtype: Human Translation
ms.sourcegitcommit: 71fea4a41b2e3a60f2f610609a14372e678b7ec4
ms.openlocfilehash: 895906e1ab4bc50093ed3b18f043c3dd515ca054
ms.contentlocale: cs-cz
ms.lasthandoff: 05/10/2017

---
# <a name="create-a-static-html-web-app-in-azure-in-five-minutes"></a>Vytvoření webové aplikace ve statickém HTML v Azure během pěti minut

Tento rychlý start vás provede postupem nasazení základní lokality HTML + CSS do Azure. Aplikaci spustíte s použitím [plánu služby Azure App Service](https://docs.microsoft.com/azure/app-service/azure-web-sites-web-hosting-plans-in-depth-overview) a vytvoříte v něm novou webovou aplikaci pomocí rozhraní [Azure CLI](https://docs.microsoft.com/en-us/cli/azure/get-started-with-azure-cli). Aplikaci nasadíte do Azure pomocí systému Git. Pokud máte splněné všechny požadavky, zabere vám tento kurz zhruba pět minut.

![hello-world-in-browser](media/app-service-web-get-started-html/hello-world-in-browser-az.png)

## <a name="prerequisites"></a>Požadavky

Před vytvořením této ukázky si stáhněte a nainstalujte následující komponenty:

- [Git](https://git-scm.com/)
- [Azure CLI 2.0](https://docs.microsoft.com/cli/azure/install-azure-cli)

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

## <a name="download-the-sample"></a>Stažení ukázky

V okně terminálu naklonujte úložiště ukázkové aplikace do místního počítače:

```bash
git clone https://github.com/Azure-Samples/html-docs-hello-world.git
```

## <a name="view-the-html"></a>Zobrazení kódu HTML

Přejděte do adresáře, který obsahuje ukázkový kód HTML. Otevřete v prohlížeči soubor *index.html*.

![hello-world-in-browser](media/app-service-web-get-started-html/hello-world-in-browser.png)

[!INCLUDE [login-to-azure](../../includes/login-to-azure.md)] 
[!INCLUDE [configure-deployment-user](../../includes/configure-deployment-user.md)] 

[!INCLUDE [app-service-web-quickstart1](../../includes/app-service-web-quickstart1.md)] 

Vytvořte [webovou aplikaci](app-service-web-overview.md) v plánu služby App Service `quickStartPlan`. Tato webová aplikace poskytuje prostor pro hostování vašeho kódu a adresu URL, na které si můžete nasazenou aplikaci zobrazit.

[!INCLUDE [app-service-web-quickstart2](../../includes/app-service-web-quickstart2.md)] 

Stránka běží jako webová aplikace Azure App Service:

![hello-world-in-browser](media/app-service-web-get-started-html/hello-world-in-browser-az.png)

## <a name="update-and-redeploy-the-app"></a>Aktualizace a opětovné nasazení aplikace

Otevřete soubor *index.html*. Změňte kód. Můžete třeba změnit `Hello world!` na `Hello Azure!`.

Potvrďte změny v Gitu a potom odešlete změny kódu do Azure.

```bash
git commit -am "updated HTML"
git push azure master
```

Po dokončení nasazení aktualizujte prohlížeč, aby se změny projevily.

[!INCLUDE [manage-azure-web-app](../../includes/manage-azure-web-app.md)]


[!INCLUDE [cli-samples-clean-up](../../includes/cli-samples-clean-up.md)]

## <a name="next-steps"></a>Další kroky

- Prozkoumejte [ukázkové skripty rozhraní CLI Web Apps](app-service-cli-samples.md).
- Zjistěte, jak provést [mapování vlastního názvu domény](app-service-web-tutorial-custom-domain.md), třeba contoso.com, do [aplikace služby App Service](app-service-web-tutorial-custom-domain.md).
