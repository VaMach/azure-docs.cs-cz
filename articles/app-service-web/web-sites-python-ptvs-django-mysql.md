---
title: "Django a MySQL v Azure s nástroji Python Tools 2.2 pro Visual Studio"
description: "Naučte se používat nástroje Python Tools pro Visual Studio k vytvoření webové aplikace Django, která ukládá data do instance databáze MySQL, a k nasazení této aplikace do webové aplikace služby Azure App Service Web Apps."
services: app-service\web
documentationcenter: python
author: huguesv
manager: wpickett
editor: 
ms.assetid: c60a50b5-8b5e-4818-a442-16362273dabb
ms.service: app-service-web
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: python
ms.topic: get-started-article
ms.date: 07/07/2016
ms.author: huvalo
translationtype: Human Translation
ms.sourcegitcommit: 2ea002938d69ad34aff421fa0eb753e449724a8f
ms.openlocfilehash: 759441c5d64ee59f13d50eb415fbaa884dd4821a


---
# <a name="django-and-mysql-on-azure-with-python-tools-22-for-visual-studio"></a>Django a MySQL v Azure s nástroji Python Tools 2.2 pro Visual Studio
[!INCLUDE [tabs](../../includes/app-service-web-get-started-nav-tabs.md)]

V tomto kurzu budete používat nástroje [Python Tools pro Visual Studio](https://www.visualstudio.com/vs/python) k vytvoření jednoduché hlasovací webové aplikace pomocí jedné z ukázkových šablon PTVS. Naučíte se používat službu MySQL hostovanou v Azure, konfigurovat webovou aplikaci k používání MySQL a publikovat webovou aplikaci do služby [Azure App Service Web Apps](http://go.microsoft.com/fwlink/?LinkId=529714).

> [!NOTE]
> Informace obsažené v tomto kurzu jsou také k dispozici v následujícím videu:
> 
> [PTVS 2.1: aplikace Django s MySQL][video]
> 
> 

Ve [Středisku pro vývojáře Python] naleznete další články týkající se vývoje služby Azure App Service Web Apps s nástroji PTVS pomocí webového rozhraní Bottle, Flask a Django, se službami Azure Table Storage, MySQL a SQL Database. Ačkoli se tento článek zaměřuje na službu App Service, postup při vývoji služeb [Azure Cloud Services] je obdobný.

## <a name="prerequisites"></a>Požadavky
* Visual Studio 2015
* [Python 2.7 (32bitová verze)] nebo [Python 3.4 (32bitová verze)]
* [Python Tools 2.2 pro Visual Studio]
* [Python Tools 2.2 pro Visual Studio – ukázky VSIX]
* [Azure SDK Tools for VS 2015]
* Django 1.9 nebo novější

[!INCLUDE [create-account-and-websites-note](../../includes/create-account-and-websites-note.md)]

<!-- This note should not render as part of the the previous include. -->

> [!NOTE]
> Pokud chcete začít používat službu Azure App Service před registrací k účtu Azure, přejděte k možnosti [Vyzkoušet službu App Service](http://go.microsoft.com/fwlink/?LinkId=523751), kde můžete okamžitě vytvořit krátkodobou úvodní webovou aplikaci. Není požadována platební karta a nevzniká žádný závazek.
> 
> 

## <a name="create-the-project"></a>Vytvoření projektu
V této části vytvoříte projekt sady Visual Studio pomocí vzorové šablony. Vytvoříte virtuální prostředí a nainstalujte požadované balíčky. Vytvoříte místní databázi pomocí SQLite. Poté aplikaci místně spustíte.

1. V sadě Visual Studio vyberte položku **Soubor**, **Nový projekt**.
2. Šablony projektů z [Python Tools 2.2 pro Visual Studio – ukázky VSIX] jsou dostupné v části **Python**, **Ukázky**. Vyberte položku **Hlasovací webový projekt Django** a kliknutím na tlačítko OK vytvořte projekt.
   
    ![Dialogové okno Nový projekt](./media/web-sites-python-ptvs-django-mysql/PollsDjangoNewProject.png)
3. Zobrazí se výzva k instalaci externích balíčků. Vyberte možnost **Instalovat do virtuálního prostředí**.
   
    ![Dialogové okno Externí balíčky](./media/web-sites-python-ptvs-django-mysql/PollsDjangoExternalPackages.png)
4. Jako základní překladač vyberte jazyk **Python 2.7** nebo **Python 3.4**.
   
    ![Dialogové okno Přidání virtuálního prostředí](./media/web-sites-python-ptvs-django-mysql/PollsCommonAddVirtualEnv.png)
5. V **Průzkumníku řešení** klikněte pravým tlačítkem na uzel projektu a vyberte položku **Python** a poté položku **Django migrace**.  Pak vyberte možnost **Vytvořit superživatele Django**.
6. Tím se otevřete Konzola pro správu Django a ve složce projektu se vytvoří databáze SQLite. Postupujte podle výzev a vytvořte uživatele.
7. Stisknutím klávesy `F5` se ujistěte, zda aplikace pracuje.
8. V horním navigačním panelu klikněte na možnost **Přihlásit**.
   
    ![Navigační panel Django](./media/web-sites-python-ptvs-django-mysql/PollsDjangoCommonBrowserLocalMenu.png)
9. Zadejte přihlašovací údaje uživatele, kterého jste vytvořili při synchronizaci databáze.
   
    ![Přihlašovací formulář](./media/web-sites-python-ptvs-django-mysql/PollsDjangoCommonBrowserLocalLogin.png)
10. Klikněte na možnost **Vytvořit ukázková hlasování**.
    
     ![Vytvoření ukázkových hlasování](./media/web-sites-python-ptvs-django-mysql/PollsDjangoCommonBrowserNoPolls.png)
11. Klikněte na hlasování a hlasujte.
    
     ![Hlasování v ukázkových hlasováních](./media/web-sites-python-ptvs-django-mysql/PollsDjangoSqliteBrowser.png)

## <a name="create-a-mysql-database"></a>Vytvoření databáze MySQL Database
Jako databázi vytvoříte databázi ClearDB hostovanou v MySQL v Azure.

Alternativně můžete vytvořit svůj vlastní virtuální počítač spuštěný v Azure a poté sami nainstalovat a spravovat MySQL.

Následujícím postupem můžete vytvořit databázi s bezplatným plánem.

1. Přihlaste se k [Azure Portal].
2. V horní části podokna navigace klikněte na položku **NOVÉ**, **Data + úložiště** a poté na položku **Databáze MySQL**.
3. Nakonfigurujte novou databázi MySQL tím, že vytvoříte novou skupinu prostředků a vyberte pro ni vhodné umístění.
4. Po vytvoření databáze MySQL klikněte v okně databáze na možnost **Vlastnosti**.
5. Tlačítkem kopírování vložte hodnotu **PŘIPOJOVACÍ ŘETĚZEC** do schránky.

## <a name="configure-the-project"></a>Konfigurace projektu
V této části nakonfigurujete webovou aplikaci k používání databáze MySQL, kterou jste právě vytvořili. Nainstalujete také další balíčky Python, které jsou nezbytné pro používání databází MySQL s rozhraním Django. Poté webovou aplikaci místně spustíte.

1. V sadě Visual Studio otevřete soubor **settings.py** ze složky *NázevProjektu*. Dočasně vložte připojovací řetězec do editoru. Připojovací řetězec má tento formát:
   
        Database=<NAME>;Data Source=<HOST>;User Id=<USER>;Password=<PASSWORD>
   
    Změňte výchozí parametr databáze **ENGINE** tak, aby používal MySQL, a nastavte hodnoty parametrů **NAME**, **USER**, **PASSWORD** a **HOST** z **CONNECTIONSTRING**.
   
        DATABASES = {
            'default': {
                'ENGINE': 'django.db.backends.mysql',
                'NAME': '<Database>',
                'USER': '<User Id>',
                'PASSWORD': '<Password>',
                'HOST': '<Data Source>',
                'PORT': '',
            }
        }
2. V Průzkumníku řešení v části **Prostředí Python** klikněte pravým tlačítkem na virtuální prostředí a vyberte položku **Instalovat balíček Python**.
3. Nainstalujte balíček `mysqlclient` pomocí funkce **pip**.
   
    ![Dialogové okno instalace balíčku](./media/web-sites-python-ptvs-django-mysql/PollsDjangoMySQLInstallPackage.png)
4. V **Průzkumníku řešení** klikněte pravým tlačítkem na uzel projektu a vyberte položku **Python** a poté položku **Django migrace**.  Pak vyberte možnost **Vytvořit superživatele Django**.
   
    Tím se vytvoří tabulky pro databázi MySQL, kterou jste vytvořili v předchozí části. Postupujte podle výzev a vytvořte uživatele, který se nemusí shodovat s uživatelem v databázi SQLite vytvořené v první části tohoto článku.
5. Spusťte aplikaci klávesou `F5`. Hlasování vytvořená pomocí položky **Vytvořit ukázková hlasování** a data odeslaná při hlasování budou serializována v databázi MySQL.

## <a name="publish-the-web-app-to-azure-app-service"></a>Publikování webové aplikace do služby Azure App Service
Sada Azure .NET SDK poskytuje snadný způsob, jak nasadit webovou aplikaci do služby Azure App Service.

1. V **Průzkumníku řešení** klikněte pravým tlačítkem na uzel projektu a vyberte možnost **Publikovat**.
   
    ![Dialogové okno Publikování webu](./media/web-sites-python-ptvs-django-mysql/PollsCommonPublishWebSiteDialog.png)
2. Klikněte na **Microsoft Azure App Service**.
3. Kliknutím na možnost **Nové** vytvořte novou webovou aplikaci.
4. Vyplňte následující pole a klikněte na možnost **Vytvořit**:
   
   * **Název webové aplikace**
   * **Plán služby App Service**
   * **Skupina prostředků**
   * **Oblast**
   * Položku **Databázový server** ponechte nastavenou na možnost **Bez databáze**
5. Přijměte veškerá ostatní výchozí nastavení a klikněte na možnost **Publikovat**.
6. Automaticky se otevře webový prohlížeč s publikovanou webovou aplikací. Měli byste vidět, že webová aplikace funguje očekávaným způsobem a využívá databázi **MySQL** hostovanou v Azure.
   
    ![Webový prohlížeč](./media/web-sites-python-ptvs-django-mysql/PollsDjangoAzureBrowser.png)
   
    Blahopřejeme! Úspěšně jste publikovali webovou aplikaci založenou na MySQL do Azure.

## <a name="next-steps"></a>Další kroky
Potřebujete-li další informace o nástrojích Python Tools pro Visual Studio, rozhraní Django a MySQL, použijte tyto odkazy.

* [Dokumentace nástrojů Python Tools pro Visual Studio]
  * [Webové projekty]
  * [Projekty cloudových služeb]
  * [Vzdálené ladění v Microsoft Azure]
* [Dokumentace rozhraní Django]
* [MySQL]

Další informace naleznete ve [Středisku pro vývojáře Python](/develop/python/).

<!--Link references-->

[Středisku pro vývojáře Python]: /develop/python/
[Azure Cloud Services]: ../cloud-services/cloud-services-python-ptvs.md

<!--External Link references-->

[Azure Portal]: https://portal.azure.com
[Python Tools pro Visual Studio]: https://www.visualstudio.com/vs/python/
[Python Tools 2.2 pro Visual Studio]: http://go.microsoft.com/fwlink/?LinkID=624025
[Python Tools 2.2 pro Visual Studio – ukázky VSIX]: http://go.microsoft.com/fwlink/?LinkID=624025
[Azure SDK Tools for VS 2015]: http://go.microsoft.com/fwlink/?LinkId=518003
[Python 2.7 (32bitová verze)]: http://go.microsoft.com/fwlink/?LinkId=517190
[Python 3.4 (32bitová verze)]: http://go.microsoft.com/fwlink/?LinkId=517191
[Dokumentace nástrojů Python Tools pro Visual Studio]: http://aka.ms/ptvsdocs
[Vzdálené ladění v Microsoft Azure]: http://go.microsoft.com/fwlink/?LinkId=624026
[Webové projekty]: http://go.microsoft.com/fwlink/?LinkId=624027
[Projekty cloudových služeb]: http://go.microsoft.com/fwlink/?LinkId=624028
[Dokumentace rozhraní Django]: https://www.djangoproject.com/
[MySQL]: http://www.mysql.com/
[video]: http://youtu.be/oKCApIrS0Lo



<!--HONumber=Nov16_HO2-->


