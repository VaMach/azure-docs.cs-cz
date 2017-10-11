---
title: "Webová aplikace s Express (Node.js) | Microsoft Docs"
description: "Kurz, který je založený na kurz cloudové služby a ukazuje, jak použít modul Express."
services: cloud-services
documentationcenter: nodejs
author: TomArcher
manager: routlaw
editor: 
ms.assetid: 24f8e7ef-e90d-4554-9b1e-a9b31d5824e5
ms.service: cloud-services
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: nodejs
ms.topic: article
ms.date: 08/17/2017
ms.author: tarcher
ms.openlocfilehash: 54b715695e24786ec4e8dfcabefc648d76179c8b
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/18/2017
---
# <a name="build-a-nodejs-web-application-using-express-on-an-azure-cloud-service"></a>Vytvoření webové aplikace Node.js pomocí expresního na cloudové služby Azure
Node.js zahrnuje minimální sadu funkcí, které v modulu runtime jádra.
3. stran moduly vývojáři často používají k poskytování dalších funkcí při vývoji aplikace Node.js. V tomto kurzu vytvoříte novou aplikaci pomocí [Express] [ Express] modul, který poskytuje MVC technologie pro vytváření webových aplikací Node.js.

Zde je snímek obrazovky dokončené aplikace:

![Webový prohlížeč zobrazující Vítá do Express v Azure](./media/cloud-services-nodejs-develop-deploy-express-app/node36.png)

## <a name="create-a-cloud-service-project"></a>Vytvoření projektu cloudové služby
[!INCLUDE [install-dev-tools](../../includes/install-dev-tools.md)]

Proveďte následující kroky k vytvoření nového projektu cloudové služby s názvem 'expressapp':

1. Z **nabídce Start** nebo **obrazovce Start**, vyhledejte **prostředí Windows PowerShell**. Nakonec klikněte pravým tlačítkem na **prostředí Windows PowerShell** a vyberte **spustit jako správce**.
   
    ![Azure PowerShell – ikona](./media/cloud-services-nodejs-develop-deploy-express-app/azure-powershell-start.png)
2. Přejděte do adresáře **c:\\uzlu** adresář a potom zadejte následující příkazy a vytvořte nové řešení s názvem **expressapp** a webové role s názvem **WebRole1**:
   
        PS C:\node> New-AzureServiceProject expressapp
        PS C:\Node\expressapp> Add-AzureNodeWebRole
        PS C:\Node\expressapp> Set-AzureServiceProjectRole WebRole1 Node 0.10.21
   
    > [!NOTE]
    > Ve výchozím nastavení **Add-AzureNodeWebRole** používá starší verze Node.js. **Set-AzureServiceProjectRole** výše dává pokyn, Azure a použít v0.10.21 uzlu.  Všimněte si, že parametry jsou malá a velká písmena.  Můžete ověřit kontrolou byla vybrána správná verze Node.js **moduly** vlastnost **WebRole1\package.json**.
    > 
    > 

## <a name="install-express"></a>Expresní instalace
1. Generátor Express nainstalujte pomocí následujícího příkazu:
   
        PS C:\node\expressapp> npm install express-generator -g
   
    Výstup příkazu npm by měla vypadat podobně jako výsledek níže. 
   
    ![Prostředí Windows PowerShell zobrazení výstupu npm express příkaz instalovat.](./media/cloud-services-nodejs-develop-deploy-express-app/express-g.png)
2. Přejděte do adresáře **WebRole1** adresáře a použijte příkaz express generovat nové aplikace:
   
        PS C:\node\expressapp\WebRole1> express
   
    Zobrazí se výzva k přepsání starší aplikace. Zadejte **y** nebo **Ano** pokračujte. Express způsobí vygenerování souboru app.js a strukturu složek pro vytváření aplikace.
   
    ![Výstup příkazu express](./media/cloud-services-nodejs-develop-deploy-express-app/node23.png)
3. Pokud chcete nainstalovat další závislosti, které jsou definované v souboru package.json, zadejte následující příkaz:
   
       PS C:\node\expressapp\WebRole1> npm install
   
   ![Příkaz instalovat výstup npm](./media/cloud-services-nodejs-develop-deploy-express-app/node26.png)
4. Použijte následující příkaz pro kopírování **bin/www** do souboru **server.js**. To je proto cloudové služby můžete najít vstupní bod pro tuto aplikaci.
   
       PS C:\node\expressapp\WebRole1> copy bin/www server.js
   
   Po dokončení tohoto příkazu, byste měli mít **server.js** soubor v adresáři WebRole1.
5. Změnit **server.js** k odebrání jednoho z '.' znaky z následujícího řádku.
   
       var app = require('../app');
   
   Po provedení této úpravy, řádku se zobrazí následující.
   
       var app = require('./app');
   
   Tato změna je vyžadována, protože jsme přesunout soubor (dříve **bin/www**,) do stejného adresáře jako soubor aplikace se vyžaduje. Po provedení této změny, uložte **server.js** souboru.
6. Spusťte aplikaci v emulátoru Azure použijte následující příkaz:
   
       PS C:\node\expressapp\WebRole1> Start-AzureEmulator -launch
   
    ![Webovou stránku obsahující Vítá vás express.](./media/cloud-services-nodejs-develop-deploy-express-app/node28.png)

## <a name="modifying-the-view"></a>Úprava zobrazení
Nyní upravte zobrazení tak, aby se zpráva "Vítá do Express v Azure".

1. Zadejte následující příkaz pro otevření souboru index.jade:
   
       PS C:\node\expressapp\WebRole1> notepad views/index.jade
   
   ![Obsah souboru index.jade.](./media/cloud-services-nodejs-develop-deploy-express-app/getting-started-19.png)
   
   Jade je výchozí modul zobrazení používané aplikace Express. Další informace o Jade zobrazovací modul, najdete v části [http://jade-lang.com][http://jade-lang.com].
2. Upravit poslední řádek textu připojením **v Azure**.
   
   ![Načte poslední řádek souboru index.jade: p Vítejte \#{title} v Azure](./media/cloud-services-nodejs-develop-deploy-express-app/node31.png)
3. Uložte tento soubor a ukončete Poznámkový blok.
4. Aktualizujte webový prohlížeč a zobrazí se provedené změny.
   
   ![Okno prohlížeče, tato stránka obsahuje Vítá vás Express v Azure](./media/cloud-services-nodejs-develop-deploy-express-app/node32.png)

Po otestování aplikace, použijte **Stop-AzureEmulator** pro zastavení emulátor.

## <a name="publishing-the-application-to-azure"></a>Publikování aplikace do Azure
V okně prostředí Azure PowerShell, použijte **Publish-AzureServiceProject** rutiny k nasazení aplikace do cloudové služby

    PS C:\node\expressapp\WebRole1> Publish-AzureServiceProject -ServiceName myexpressapp -Location "East US" -Launch

Po dokončení operace nasazení, prohlížeč a zobrazí webové stránky.

![Webový prohlížeč zobrazující stránku Express. Adresa URL značí, že je nyní hostované v Azure.](./media/cloud-services-nodejs-develop-deploy-express-app/node36.png)

## <a name="next-steps"></a>Další kroky
Další informace najdete ve [Středisku pro vývojáře Node.js](/develop/nodejs/).

[Node.js Web Application]: http://www.windowsazure.com/develop/nodejs/tutorials/getting-started/
[Express]: http://expressjs.com/
[http://jade-lang.com]: http://jade-lang.com


