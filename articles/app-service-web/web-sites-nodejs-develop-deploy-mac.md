---
title: "Vytvoření webové aplikace Node.js ve službě Azure App Service | Dokumentace Microsoftu"
description: "Naučte se nasadit aplikaci Node.js do webové aplikace ve službě Azure App Service."
services: app-service\web
documentationcenter: nodejs
author: rmcmurray
manager: erikre
editor: 
ms.assetid: 85af23df-54af-4430-8d77-a1f97e2f5b10
ms.service: app-service-web
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: nodejs
ms.topic: hero-article
ms.date: 12/22/2016
ms.author: robmcm
translationtype: Human Translation
ms.sourcegitcommit: dcda8b30adde930ab373a087d6955b900365c4cc
ms.openlocfilehash: 2537b0c4dcc71d02d25959a0b21d96bce7deaac6


---
# <a name="create-a-nodejs-web-app-in-azure-app-service"></a>Vytvoření webové aplikace Node.js ve službě Azure App Service
> [!div class="op_single_selector"]
> * [.Net](web-sites-dotnet-get-started.md)
> * [Node.js](web-sites-nodejs-develop-deploy-mac.md)
> * [Java](web-sites-java-get-started.md)
> * [PHP - Git](web-sites-php-mysql-deploy-use-git.md)
> * [PHP - FTP](web-sites-php-mysql-deploy-use-ftp.md)
> * [Python](web-sites-python-ptvs-django-mysql.md)
> 
> 

V tomto kurzu se dozvíte, jak vytvořit jednoduchou aplikaci [Node.js](http://nodejs.org) a jak ji pomocí [Git](http://git-scm.com) nasadit do [webové aplikace](app-service-web-overview.md) ve službě [Azure App Service](../app-service/app-service-value-prop-what-is.md). Pokyny v tomto kurzu platí pro všechny operační systémy, které podporují Node.js.

Naučíte se:

* Postup vytvoření webové aplikace ve službě Azure App Service pomocí portálu Azure.
* Postup nasazení aplikace Node.js do webové aplikace nuceným doručením (push) do úložiště Git webové aplikace.

Dokončená aplikace napíše do prohlížeče krátký řetězec „zdravím svět“.

![Prohlížeč se zobrazenou zprávou „Zdravím svět“.][helloworld-completed]

Kurzy a ukázkový kód se složitějšími aplikacemi Node.js a další témata týkající se použití Node.js v Azure naleznete ve [Středisku pro vývojáře Node.js](/develop/nodejs/).

> [!NOTE]
> K absolvování tohoto kurzu potřebujete účet Microsoft Azure. Pokud nemáte účet, můžete si [aktivovat výhody předplatitele sady Visual Studio](/en-us/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A261C142F) nebo se [zaregistrovat k bezplatné zkušební verzi](/en-us/pricing/free-trial/?WT.mc_id=A261C142F).
> 
> Chcete-li začít se službou Azure App Service dříve, než se zaregistrujete k účtu Azure, přejděte k možnosti [Vyzkoušet službu App Service](http://go.microsoft.com/fwlink/?LinkId=523751). Zde můžete ihned vytvořit krátkodobou úvodní webovou aplikaci ve službě App Service – aniž by byla požadována platební karta a bez jakýchkoli závazků.
> 
> 

## <a name="create-a-web-app-and-enable-git-publishing"></a>Vytvoření webové aplikace a povolení publikování Git
Chcete-li vytvořit webovou aplikaci ve službě Azure App Service a povolit publikování Git, postupujte podle těchto kroků. 

[Git](http://git-scm.com/) je distribuovaný systém správy verzí, který lze použít k nasazení webu Azure. Kód napsaný pro webovou aplikaci uložíte do místního úložiště Git a kód budete nasazovat do Azure nuceným doručením (push) do vzdáleného úložiště. Tato metoda nasazení je jednou z funkcí webových aplikací služby App Service.  

1. Přihlaste se k [portálu Azure](https://portal.azure.com).
2. Klikněte na ikonu **+ NOVÉ** v levé horní části portálu Azure.
3. Klikněte na možnost **Web + mobilní zařízení** a **Webová aplikace**.
   
    ![][portal-quick-create]
4. Zadejte název webové aplikace do pole **Webová aplikace**.
   
    Tento název musí být v doméně azurewebsites.net jedinečný, protože webová aplikace bude mít adresu URL {název}.azurewebsites.net. Není-li zadaný název jedinečný, v textovém poli se zobrazí červený vykřičník.
5. Vyberte možnost **Předplatné**.
6. Vyberte **skupinu prostředků** nebo vytvořte novou.
   
    Další informace o skupinách prostředků najdete v tématu [Přehled Azure Resource Manageru](../azure-resource-manager/resource-group-overview.md).
7. Vyberte **umístění/plán služby App Service** nebo vytvořte nové.
   
    Podrobnější informace o plánech služby App Service naleznete v tématu [Přehled plánů služby Azure App Service](../app-service/azure-web-sites-web-hosting-plans-in-depth-overview.md).
8. Klikněte na možnost **Vytvořit**.
   
    ![][portal-quick-create2]
   
    Zanedlouho, obvykle během méně než minuty, Azure dokončí vytváření nové webové aplikace.
9. Klikněte na možnost **Webové aplikace > {vaše nová webová aplikace}**.
   
    ![](./media/web-sites-nodejs-develop-deploy-mac/gotowebapp.png)
10. V okně **Webová aplikace** klikněte na část **Nasazení**.
    
    ![][deployment-part]
11. V okně **Průběžné nasazování** klikněte na možnost **Vybrat zdroj**.
12. Klikněte na možnost **Místní úložiště Git** a na tlačítko **OK**.
    
    ![][setup-git-publishing]
13. Pokud jste ještě nenastavili přihlašovací údaje pro nasazení, nastavte je nyní.
    
    a. V okně webové aplikace klikněte na možnost **Nastavení > Přihlašovací údaje pro nasazení**.
    
    ![][deployment-credentials]
    
    b. Vytvořte uživatelské jméno a heslo. 
    
    ![](./media/web-sites-nodejs-develop-deploy-mac/setdeploycreds.png)
14. V okně webové aplikace klikněte na možnost **Nastavení** a poté **Vlastnosti**.
    
    Chcete-li publikovat, je nutné provést nucené doručení (push) do vzdáleného úložiště Git. Adresa URL úložiště je uvedena v položce **URL GIT**. Tuto adresu URL použijete později v tomto kurzu.
    
    ![][git-url]

## <a name="build-and-test-your-application-locally"></a>Místní sestavení a testování aplikace
V této části vytvoříte soubor **server.js** obsahující mírně upravenou verzi příkladu „Zdravím svět“ z tématu [nodejs.org]. Kód přidá process.env.PORT jako port pro naslouchání při spuštění ve webové aplikaci Azure.

1. Vytvořte adresář s názvem *zdravímsvět*.
2. Pomocí textového editoru vytvořte v adresáři *zdravímsvět* nový soubor s názvem **server.js**.
3. Do souboru **server.js** zkopírujte následující kód a soubor uložte:
   
        var http = require('http')
        var port = process.env.PORT || 1337;
        http.createServer(function(req, res) {
          res.writeHead(200, { 'Content-Type': 'text/plain' });
          res.end('Hello World\n');
        }).listen(port);
4. Otevřete příkazový řádek a následujícím příkazem místně spusťte webovou aplikaci.
   
        node server.js
5. Otevřete webový prohlížeč a přejděte na adresu http://localhost:1337. 
   
    Otevře se webová stránka zobrazující pozdrav „Zdravím svět“, jak je znázorněno na následujícím snímku obrazovky.
   
    ![Prohlížeč se zobrazenou zprávou „Zdravím svět“.][helloworld-localhost]

## <a name="publish-your-application"></a>Publikování aplikace
1. Pokud jste tak již neučinili, nainstalujte Git.
   
    Pokyny k instalaci pro svou platformu naleznete na [stránce stahování Git](http://git-scm.com/download).
2. V příkazovém řádku přejděte do adresáře **zdravímsvět** a zadáním následujícího příkazu inicializujte místní úložiště Git.
   
        git init
3. Pomocí následujících příkazů přidejte soubory do úložiště:
   
        git add .
        git commit -m "initial commit"
4. Následujícím příkazem přidejte vzdálené řízení Git pro nucené doručení (push) aktualizací do webové aplikace, kterou jste vytvořili dříve:
   
        git remote add azure [URL for remote repository]
5. Nuceně vložte (push) změny do Azure následujícím příkazem:
   
        git push azure master
   
    Zobrazí se výzva k zadání hesla, které jste vytvořili dříve. Výstup se podobá následujícímu příkladu.
   
        Counting objects: 3, done.
        Delta compression using up to 8 threads.
        Compressing objects: 100% (2/2), done.
        Writing objects: 100% (3/3), 374 bytes, done.
        Total 3 (delta 0), reused 0 (delta 0)
        remote: New deployment received.
        remote: Updating branch 'master'.
        remote: Preparing deployment for commit id '5ebbe250c9'.
        remote: Preparing files for deployment.
        remote: Deploying Web.config to enable Node.js activation.
        remote: Deployment successful.
        To https://user@testsite.scm.azurewebsites.net/testsite.git
         * [new branch]      master -> master
6. Chcete-li zobrazit aplikaci, klikněte na tlačítko **Procházet** v části **Webová aplikace** portálu Azure.
   
    ![Tlačítko Procházet](./media/web-sites-nodejs-develop-deploy-mac/browsebutton.png)
   
    ![Zdravím svět v Azure](./media/web-sites-nodejs-develop-deploy-mac/helloworldazure.png)

## <a name="publish-changes-to-your-application"></a>Publikování změn do aplikace
1. Otevřete soubor **server.js** v textovém editoru a změňte řetězec „Zdravím svět\n“ na „Zdravím Azure\n“. 
2. Uložte soubor.
3. V příkazovém řádku přejděte do adresáře **zdravímsvět** a spusťte následující příkazy.
   
        git add .
        git commit -m "changing to hello azure"
        git push azure master
   
    Znovu se zobrazí výzva k zadání hesla.
4. Aktualizujte okno prohlížeče, v němž jste přešli na adresu URL webové aplikace.
   
    ![Webová stránka se zobrazenou zprávou „Zdravím Azure“][helloworld-completed]

## <a name="roll-back-a-deployment"></a>Vrácení nasazení zpět
V okně **Webová aplikace** můžete kliknout na možnost **Nastavení > Průběžné nasazování** a zobrazit historii nasazení v okně **Nasazení**. Potřebujete-li se vrátit zpět k předchozímu nasazení, můžete jej vybrat a poté v okně **Podrobnosti o nasazení** kliknout na tlačítko **Znovu nasadit**.

## <a name="next-steps"></a>Další kroky
Nasadili jste aplikaci Node.js do webové aplikace ve službě Azure App Service. Podrobnější informace o způsobu, jímž webové aplikace služby App Service spouštějí aplikace Node.js, naleznete v tématech [Azure App Service Web Apps: Node.js](http://blogs.msdn.com/b/silverlining/archive/2012/06/14/windows-azure-websites-node-js.aspx) a [Určení verze Node.js v aplikaci Azure](../nodejs-specify-node-version-azure-apps.md).

Node.js poskytuje bohatý ekosystém modulů, které mohou vaše aplikace využívat. Podrobnější informace o způsobu, jímž služba Web Apps pracuje s moduly, naleznete v tématu [Používání modulů Node.js s aplikacemi Azure](../nodejs-use-node-modules-azure-apps.md).

Narazíte-li na problémy s aplikací poté, co byla nasazena do Azure, vyhledejte informace týkající se diagnostiky problému v tématu [Postup ladění aplikace Node.js ve službě Azure App Service](web-sites-nodejs-debug.md) .

Tento článek používá portál Azure k vytvoření webové aplikace. Tytéž operace lze rovněž provést pomocí [rozhraní příkazového řádku Azure](../xplat-cli-install.md) nebo prostředí [Azure PowerShell](/powershell/azureps-cmdlets-docs).

Podrobnější informace týkající se postupu při vývoji aplikací Node.js v Azure naleznete ve [Středisku pro vývojáře Node.js](/develop/nodejs/).

[helloworld-completed]: ./media/web-sites-nodejs-develop-deploy-mac/helloazure.png
[helloworld-localhost]: ./media/web-sites-nodejs-develop-deploy-mac/helloworldlocal.png
[portal-quick-create]: ./media/web-sites-nodejs-develop-deploy-mac/create-quick-website.png
[portal-quick-create2]: ./media/web-sites-nodejs-develop-deploy-mac/create-quick-website2.png
[setup-git-publishing]: ./media/web-sites-nodejs-develop-deploy-mac/setup_git_publishing.png
[go-to-dashboard]: ./media/web-sites-nodejs-develop-deploy-mac/go_to_dashboard.png
[deployment-part]: ./media/web-sites-nodejs-develop-deploy-mac/deployment-part.png
[deployment-credentials]: ./media/web-sites-nodejs-develop-deploy-mac/deployment-credentials.png
[git-url]: ./media/web-sites-nodejs-develop-deploy-mac/git-url.png



<!--HONumber=Jan17_HO1-->


