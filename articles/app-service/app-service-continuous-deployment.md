---
title: "Průběžné nasazování do Azure App Service | Microsoft Docs"
description: "Naučte se povolit průběžné nasazování do služby Azure App Service."
services: app-service
documentationcenter: 
author: dariagrigoriu
manager: erikre
editor: mollybos
ms.assetid: 6adb5c84-6cf3-424e-a336-c554f23b4000
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/28/2016
ms.author: dariagrigoriu
ms.openlocfilehash: ef5924607868bcb3dc35e279539b78d5a0e17c1a
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/11/2017
---
# <a name="continuous-deployment-to-azure-app-service"></a>Průběžné nasazování do služby Azure App Service
V tomto kurzu se dozvíte, jak nakonfigurovat pracovní postup průběžné nasazování pro vaše [Azure Web Apps](app-service-web-overview.md). Služby App Service integrace s BitBucket, Githubu, a [Visual Studio Team Services (VSTS)](https://www.visualstudio.com/team-services/) umožňuje průběžné nasazování workflow, kde Azure vyžaduje nejnovější aktualizace z projektu publikována do některého z těchto služeb. Průběžné nasazování je skvělou možností pro projekty, u kterých se integruje více příspěvků nebo u kterých integrace probíhá často.

Chcete zjistit, jak nakonfigurovat průběžné nasazování ručně z úložiště v cloudu nejsou uvedené pomocí portálu Azure (například [GitLab](https://gitlab.com/)), najdete v části [nastavení průběžné nasazování pomocí ruční kroky](https://github.com/projectkudu/kudu/wiki/Continuous-deployment#setting-up-continuous-deployment-using-manual-steps).

## <a name="overview"></a>Povolení průběžného nasazování
Pokud chcete povolit průběžné nasazování, postupujte takto:

1. Publikujte obsah aplikace do úložiště, které se bude používat pro průběžné nasazování.  
    Další informace o publikování projektu v těchto službách najdete v článcích [Vytvoření úložiště (GitHub)], [Vytvoření úložiště (BitBucket)] a [Začínáme se službou VSTS].
2. V okně s nabídkou aplikace na [Azure Portal] klikněte na tlačítko **NASAZENÍ APLIKACE > Možnosti nasazení**. Klikněte na **Zvolit zdroj** a pak vyberte zdroj nasazení.  
   
    ![](./media/app-service-continuous-deployment/cd_options.png)
   
   > [!NOTE]
   > Pokud chcete konfigurovat účet služby VSTS pro nasazení služby App Service, prostudujte si tento [kurz](https://github.com/projectkudu/kudu/wiki/Setting-up-a-VSTS-account-so-it-can-deploy-to-a-Web-App).
   > 
   > 
3. Dokončete pracovní postup ověřování.
4. V okně **Zdroj nasazení** zvolte projekt a větev, ze které se má nasazení provádět. Až to budete mít, klikněte na **OK**.
   
    ![](./media/app-service-continuous-deployment/github_option.png)
   
   > [!NOTE]
   > Při povolování průběžného nasazování s použitím GitHubu nebo BitBucketu se zobrazí veřejné i soukromé projekty.
   > 
   > 
   
    Služba App Service vytvoří přidružení s vybraným úložištěm, vyžádá si soubory z určené větve a bude udržovat klon úložiště pro vaši aplikaci služby App Service. Při konfiguraci průběžného nasazování služby VSTS z portálu Azure Portal integrace využívá [stroj pro nasazení Kudu](https://github.com/projectkudu/kudu/wiki) služby App Service, který s každým příkazem `git push` již automatizuje úlohy sestavení a nasazení. Průběžné nasazování ve službě VSTS nemusíte nastavovat samostatně. Po dokončení tohoto procesu se v okně aplikace **Možnosti nasazení** zobrazí aktivní nasazení, které indikuje, že nasazení proběhlo úspěšně.
5. Pokud chcete ověřit úspěšné nasazení aplikace, klikněte na možnost **Adresa URL** v horní části okna aplikace na portálu Azure Portal.
6. Pokud chcete ověřit, že se průběžné nasazování provádí ze zvoleného úložiště, vložte do úložiště nějakou změnu. Aplikace by se měla krátce po dokončení vkládání do úložiště aktualizovat a změny by se měly projevit. Vložení aktualizace můžete ověřit v okně **Možnosti nasazení** v aplikaci.

## <a name="VSsolution"></a>Průběžné nasazování řešení sady Visual Studio
Vložení řešení sady Visual Studio do služby Azure App Service může představovat prosté vložení jednoduchého souboru index.html. Proces nasazení služby App Service zvyšuje efektivitu všech podrobností, včetně obnovení závislostí NuGet sestavení binárních souborů aplikace. Stačí, abyste doporučené postupy správy a údržby zdrojového kódu dodržovali jen ve svém úložišti Git, a nasazení aplikace App Service se postará o všechno ostatní.

Kroky při vkládání řešení sady Visual Studio do služby App Service jsou stejné jako v [předchozí části](#overview), pokud řešení a úložiště nakonfigurujete takto:

* Pomocí možnosti správy zdrojového kódu sady Visual Studio vygenerujte soubor `.gitignore` podobně jako na následujícím obrázku nebo do kořenového adresáře úložiště ručně přidejte soubor `.gitignore` s obsahem podobným této [ukázce souboru .gitignore](https://github.com/github/gitignore/blob/master/VisualStudio.gitignore).
  
  ![](./media/app-service-continuous-deployment/VS_source_control.png)
* Přidejte adresářový strom celého řešení do úložiště tak, aby se soubor .sln nacházel v kořenovém adresáři úložiště.

Po nastavení úložiště podle uvedeného popisu a konfiguraci aplikace v Azure tak, aby probíhalo průběžné nasazování z jednoho z úložišť Git online, můžete vyvíjet aplikaci ASP.NET místně v sadě Visual Studio a průběžně kód nasazovat prostým vložením změn do příslušného úložiště Git online.

## <a name="disableCD"></a>Zakázání průběžného nasazování
Pokud chcete průběžné nasazování zakázat, postupujte takto:

1. V okně s nabídkou aplikace na [Azure Portal] klikněte na tlačítko **NASAZENÍ APLIKACE > Možnosti nasazení**. Klikněte na tlačítko **Odpojit** v okně **Možnosti nasazení**.
   
    ![](./media/app-service-continuous-deployment/cd_disconnect.png)
2. Po zvolení odpovědi **Ano** na potvrzovací zprávu se můžete vrátit do okna aplikace a kliknout na **NASAZENÍ APLIKACE > Možnosti nasazení**, pokud chcete nastavit publikování z jiného zdroje.

## <a name="additional-resources"></a>Další zdroje
* [Postup při zkoumání běžných problémů s průběžným nasazováním](https://github.com/projectkudu/kudu/wiki/Investigating-continuous-deployment)
* [Způsob používání prostředí PowerShell pro Azure]
* [Způsob používání nástrojů příkazového řádku Azure pro Mac a Linux]
* [Dokumentace pro Git]
* [Projekt Kudu](https://github.com/projectkudu/kudu/wiki)
* [Použití Azure automaticky generovat kanálu CI nebo CD pro nasazení aplikace ASP.NET 4](https://www.visualstudio.com/docs/build/get-started/aspnet-4-ci-cd-azure-automatic)

> [!NOTE]
> Pokud chcete začít používat Azure App Service před registrací účtu Azure, přejděte k [možnosti vyzkoušet si App Service](https://azure.microsoft.com/try/app-service/), kde si můžete hned vytvořit krátkodobou úvodní webovou aplikaci. Nevyžaduje se žádná platební karta a nevzniká žádný závazek.
> 
> 

[Azure Portal]: https://portal.azure.com
[VSTS Portal]: https://www.visualstudio.com/en-us/products/visual-studio-team-services-vs.aspx
[Installing Git]: http://git-scm.com/book/en/Getting-Started-Installing-Git
[Způsob používání prostředí PowerShell pro Azure]: /powershell/azureps-cmdlets-docs
[Způsob používání nástrojů příkazového řádku Azure pro Mac a Linux]:../cli-install-nodejs.md
[Dokumentace pro Git]: http://git-scm.com/documentation

[Vytvoření úložiště (GitHub)]: https://help.github.com/articles/create-a-repo
[Vytvoření úložiště (BitBucket)]: https://confluence.atlassian.com/display/BITBUCKET/Create+an+Account+and+a+Git+Repo
[Začínáme se službou VSTS]: https://www.visualstudio.com/docs/vsts-tfs-overview
