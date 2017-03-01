---
title: "Vytvoření webové aplikace pomocí rozhraní Django v Azure"
description: "Tento kurz vás seznámí s postupem spuštění webové aplikace v jazyce Python ve službě Azure App Service Web Apps."
services: app-service\web
documentationcenter: python
tags: python
author: huguesv
manager: erikre
editor: 
ms.assetid: 9be1a05a-9460-49ae-94fb-9798f82c11cf
ms.service: app-service-web
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: python
ms.topic: hero-article
ms.date: 02/19/2016
ms.author: huvalo
translationtype: Human Translation
ms.sourcegitcommit: b1a633a86bd1b5997d5cbf66b16ec351f1043901
ms.openlocfilehash: c6679cf7a6b059ee7c28e1754c54afa6414d633a
ms.lasthandoff: 01/20/2017


---
# <a name="creating-web-apps-with-django-in-azure"></a>Vytvoření webové aplikace pomocí rozhraní Django v Azure
Tento kurz popisuje, jak začít a spustit jazyk Python ve službě [Azure App Service Web Apps](http://go.microsoft.com/fwlink/?LinkId=529714). Služba Web Apps poskytuje omezené bezplatné hostování a rychlé nasazení, a navíc můžete používat jazyk Python! Souběžně s růstem aplikace můžete přejít na placené hostování a můžete také integrovat se všemi ostatními službami Azure.

Vytvoříte aplikaci pomocí webového rozhraní Django (pro rozhraní [Flask](web-sites-python-create-deploy-flask-app.md) a [Bottle](web-sites-python-create-deploy-bottle-app.md) jsou k dispozici alternativní verze tohoto kurzu). Vytvoříte webovou aplikaci z Azure Marketplace, nastavíte nasazení Git a místně naklonujete úložiště. Poté místně spustíte aplikaci, provedete změny, potvrdíte je a nuceně vložíte do Azure. V tomto kurzu se dozvíte, jak to provést ze systému Windows nebo Mac/Linux.

[!INCLUDE [create-account-and-websites-note](../../includes/create-account-and-websites-note.md)]

> [!NOTE]
> Pokud chcete začít používat službu Azure App Service před registrací k účtu Azure, přejděte k možnosti [Vyzkoušet službu App Service](https://azure.microsoft.com/try/app-service/), kde můžete okamžitě vytvořit krátkodobou úvodní webovou aplikaci. Není vyžadována platební karta a nevzniká žádný závazek.
> 
> 

## <a name="prerequisites"></a>Požadavky
* Windows, Mac nebo Linux
* Python 2.7 nebo 3.4
* setuptools, pip, virtualenv (pouze Python 2.7)
* Git
* [Python Tools pro Visual Studio][Python Tools pro Visual Studio] (PTVS) – Poznámka: Tato položka je nepovinná.

**Poznámka**: Publikování TFS není u projektů v jazyce Python aktuálně podporováno.

### <a name="windows"></a>Windows
Nemáte-li ještě nainstalován jazyk Python 2.7 nebo 3.4 (32bitová verze), doporučujeme pomocí instalačního programu webové platformy nainstalovat [Azure SDK pro Python 2.7] nebo sadu [Azure SDK pro Python 3.4]. Tím se nainstaluje 32bitová verze jazyka Python, setuptools, pip, virtualenv atd. (32bitová verze jazyka Python je nainstalována v hostitelských počítačích Azure). Alternativně můžete získat jazyk Python z webu [python.org].

V případě Git doporučujeme [Git pro Windows] nebo [GitHub pro Windows]. Pokud používáte Visual Studio, můžete použít integrovanou podporu Git.

Doporučujeme také nainstalovat nástroje [Python Tools 2.2 pro Visual Studio]. Tato položka je volitelná, ale pokud máte sadu [Visual Studio], včetně bezplatné sady Visual Studio Community 2013 nebo Visual Studio Express 2013 pro Web, tato položka vám poskytne skvělé rozhraní IDE pro jazyk Python.

### <a name="maclinux"></a>Mac/Linux
Již byste měli mít nainstalován jazyk Python a Git, ale ujistěte se, zda máte Python 2.7 nebo 3.4.

## <a name="web-app-creation-on-portal"></a>Vytvoření webové aplikace v portálu
Prvním krokem při vytváření aplikace je vytvoření webové aplikace pomocí [Azure Portal](https://portal.azure.com).

1. Přihlaste se k portálu Azure a v levém dolním rohu klikněte na tlačítko **NOVÉ**.
2. Do vyhledávacího pole zadejte „python“.
3. Ve výsledcích hledání vyberte položku **Django** (publikováno PTVS) a klikněte na **Vytvořit**.
4. Nakonfigurujte novou aplikaci Django, například pro ni vytvořte nový plán služby App Service a novou skupinu prostředků. Poté klikněte na možnost **Vytvořit**.
5. Pro nově vytvořenou webovou aplikaci nakonfigurujte publikování Git podle pokynů uvedených v tématu [Místní nasazení GIT ve službě Azure App Service](app-service-deploy-local-git.md).

## <a name="application-overview"></a>Přehled aplikace
### <a name="git-repository-contents"></a>Obsah úložiště Git
Zde je uveden přehled souborů, které naleznete v počátečním úložišti Git, jež budeme v následující části klonovat.

    \app\__init__.py
    \app\forms.py
    \app\models.py
    \app\tests.py
    \app\views.py
    \app\static\content\
    \app\static\fonts\
    \app\static\scripts\
    \app\templates\about.html
    \app\templates\contact.html
    \app\templates\index.html
    \app\templates\layout.html
    \app\templates\login.html
    \app\templates\loginpartial.html
    \DjangoWebProject\__init__.py
    \DjangoWebProject\settings.py
    \DjangoWebProject\urls.py
    \DjangoWebProject\wsgi.py

Hlavní zdroje pro aplikaci. Skládá se ze 3 stran (index, about, contact) s rozložením předlohy. Statický obsah a skripty obsahují položky bootstrap, jquery, modernizr a respond.

    \manage.py

Serverová podpora místní správy a vývoje. Tuto položku použijte k místnímu spuštění aplikace, synchronizaci databáze atd.

    \db.sqlite3

Výchozí databáze. Obsahuje nezbytné tabulky pro spuštění aplikace, ale neobsahuje žádné uživatele (chcete-li vytvořit uživatele, synchronizujte databázi).

    \DjangoWebProject.pyproj
    \DjangoWebProject.sln

Soubory projektu pro použití s nástroji [Python Tools pro Visual Studio].

    \ptvs_virtualenv_proxy.py

Proxy server služby IIS pro virtuální prostředí a podpora vzdáleného ladění nástrojů PTVS.

    \requirements.txt

Externí balíčky vyžadované touto aplikací. Skript nasazení nainstaluje nástrojem pip balíčky uvedené v tomto souboru.

    \web.2.7.config
    \web.3.4.config

Konfigurační soubory služby IIS. Skript nasazení použije příslušný soubor web.x.y.config a zkopíruje jej jako soubor web.config.

### <a name="optional-files---customizing-deployment"></a>Volitelné soubory – přizpůsobení nasazení
[!INCLUDE [web-sites-python-django-customizing-deployment](../../includes/web-sites-python-django-customizing-deployment.md)]

### <a name="optional-files---python-runtime"></a>Volitelné soubory – modul Python runtime
[!INCLUDE [web-sites-python-customizing-runtime](../../includes/web-sites-python-customizing-runtime.md)]

### <a name="additional-files-on-server"></a>Další soubory na serveru
Na serveru existují některé soubory, které nejsou přidány do úložiště git. Tyto soubory jsou vytvořeny skriptem nasazení.

    \web.config

Konfigurační soubor služby IIS. Tento soubor je vytvořen ze souboru web.x.y.config při každém nasazení.

    \env\

Virtuální prostředí Python. Toto prostředí je vytvořeno během nasazení, pokud ve webové aplikaci ještě neexistuje kompatibilní virtuální prostředí. Balíčky uvedené v souboru requirements.txt jsou nainstalovány nástrojem pip, avšak nástroj pip instalaci přeskočí, pokud jsou dané balíčky již nainstalovány.

Následující 3 části popisují postup při vývoji webové aplikace ve 3 různých prostředích:

* Windows s nástroji Python Tools pro Visual Studio
* Windows s příkazovým řádkem
* Mac/Linux s příkazovým řádkem

## <a name="web-app-development---windows---python-tools-for-visual-studio"></a>Vývoj webových aplikací – Windows – nástroje Python Tools pro Visual Studio
### <a name="clone-the-repository"></a>Klonování úložiště
Nejprve naklonujte úložiště pomocí adresy URL poskytnuté na portálu Azure. Další informace naleznete v tématu [Místní nasazení přes Git do Azure App Service](app-service-deploy-local-git.md).

Otevřete soubor řešení (.sln), který je zahrnut v kořenovém adresáři úložiště.

![](./media/web-sites-python-create-deploy-django-app/ptvs-solution-django.png)

### <a name="create-virtual-environment"></a>Vytvoření virtuálního prostředí
Nyní vytvoříme virtuální prostředí pro místní vývoj. Klikněte pravým tlačítkem na položku **Prostředí Python** a vyberte možnost **Přidat virtuální prostředí...**.

* Ujistěte se, zda název prostředí je `env`.
* Vyberte základní překladač. Nezapomeňte použít stejnou verzi jazyka Python, jaká byla vybrána pro webovou aplikaci (v souboru runtime.txt nebo v okně **Nastavení aplikace** webové aplikace na portálu Azure).
* Ujistěte se, zda je zaškrtnutá možnost stažení a instalace balíčků.

![](./media/web-sites-python-create-deploy-django-app/ptvs-add-virtual-env-27.png)

Klikněte na možnost **Vytvořit**. Tím dojde k vytvoření virtuálního prostředí a instalaci závislostí uvedených v souboru requirements.txt.

### <a name="create-a-superuser"></a>Vytvoření superuživatele
V databázi, která je součástí aplikace, není definován žádný superuživatel. Chcete-li používat funkci přihlašování v aplikaci nebo rozhraní správce Django (pokud se jej rozhodnete povolit), bude nutné vytvořit superuživatele.

Spusťte tento příkaz z příkazového řádku ze složky projektu:

    env\scripts\python manage.py createsuperuser

Postupujte podle výzev a nastavte uživatelské jméno, heslo atd.

### <a name="run-using-development-server"></a>Spuštění pomocí vývojového serveru
Stisknutím klávesy F5 spusťte ladění, čímž se automaticky otevře webový prohlížeč s místně spuštěnou stránkou.

![](./media/web-sites-python-create-deploy-django-app/windows-browser-django.png)

Můžete nastavit zarážky ve zdrojích, používat okna kukátka atd. Další informace o jednotlivých funkcích naleznete v části [Dokumentace nástrojů Python Tools pro Visual Studio].

### <a name="make-changes"></a>Provedení změn
Nyní můžete experimentovat tím, že budete provádět změny zdrojů a/nebo šablon aplikace.

Jakmile změny otestujete, potvrďte je do úložiště Git:

![](./media/web-sites-python-create-deploy-django-app/ptvs-commit-django.png)

### <a name="install-more-packages"></a>Instalace dalších balíčků
Aplikace může mít kromě jazyka Python a rozhraní Django také další závislosti.

Další balíčky můžete nainstalovat pomocí nástroje pip. Chcete-li nainstalovat balíček, klikněte pravým tlačítkem na virtuální prostředí a vyberte možnost **Instalovat balíček Python**.

Chcete-li nainstalovat sadu Azure SDK pro Python, která umožňuje přístup k úložišti Azure, sběrnici Service Bus a dalším službám Azure, zadejte `azure`:

![](./media/web-sites-python-create-deploy-django-app/ptvs-install-package-dialog.png)

Klikněte pravým tlačítkem na virtuální prostředí a výběrem možnosti **Generovat soubor requirements.txt** aktualizujte soubor requirements.txt.

Poté potvrďte změny souboru requirements.txt do úložiště Git.

### <a name="deploy-to-azure"></a>Nasazení do Azure
Chcete-li aktivovat nasazení, klikněte na možnost **Synchronizovat** nebo **Vložit změny (push)**. Synchronizace provádí vložení změn (push) i přijetí změn (pull).

![](./media/web-sites-python-create-deploy-django-app/ptvs-git-push.png)

První nasazení bude určitou dobu trvat, neboť bude vytvářet virtuální prostředí, instalovat balíčky atd.

Sada Visual Studio nezobrazuje průběh nasazení. Chcete-li překontrolovat výstup, informace naleznete v tématu [Řešení potíží – nasazení](#troubleshooting-deployment).

Chcete-li zobrazit změny, přejděte na adresu URL Azure.

## <a name="web-app-development---windows---command-line"></a>Vývoj webových aplikací – Windows – příkazový řádek
### <a name="clone-the-repository"></a>Klonování úložiště
Nejprve naklonujte úložiště pomocí adresy URL poskytnuté na portálu Azure a přidejte úložiště Azure jako vzdálené. Další informace naleznete v tématu [Místní nasazení přes Git do Azure App Service](app-service-deploy-local-git.md).

    git clone <repo-url>
    cd <repo-folder>
    git remote add azure <repo-url>

### <a name="create-virtual-environment"></a>Vytvoření virtuálního prostředí
Vytvoříme nové virtuální prostředí pro účely vývoje (nepřidávejte jej do úložiště). Virtuální prostředí v jazyce Python nejsou přemístitelná, a proto si každý vývojář pracující na aplikaci vytvoří místně své vlastní virtuální prostředí.

Nezapomeňte použít stejnou verzi jazyka Python, jaká byla vybrána pro webovou aplikaci (v souboru runtime.txt nebo v okně Nastavení aplikace webové aplikace na portálu Azure).

Pro jazyk Python 2.7:

    c:\python27\python.exe -m virtualenv env

Pro jazyk Python 3.4:

    c:\python34\python.exe -m venv env

Nainstalujte veškeré případné externí balíčky požadované aplikací. Můžete použít soubor requirements.txt v kořenovém adresáři úložiště k instalaci balíčků ve virtuálním prostředí:

    env\scripts\pip install -r requirements.txt

### <a name="create-a-superuser"></a>Vytvoření superuživatele
V databázi, která je součástí aplikace, není definován žádný superuživatel. Chcete-li používat funkci přihlašování v aplikaci nebo rozhraní správce Django (pokud se jej rozhodnete povolit), bude nutné vytvořit superuživatele.

Spusťte tento příkaz z příkazového řádku ze složky projektu:

    env\scripts\python manage.py createsuperuser

Postupujte podle výzev a nastavte uživatelské jméno, heslo atd.

### <a name="run-using-development-server"></a>Spuštění pomocí vývojového serveru
Následujícím příkazem můžete aplikaci spustit v rámci vývojového serveru:

    env\scripts\python manage.py runserver

Konzola zobrazí adresa URL a port, jimž server naslouchá:

![](./media/web-sites-python-create-deploy-django-app/windows-run-local-django.png)

Poté tuto adresu URL otevřete ve webovém prohlížeči.

![](./media/web-sites-python-create-deploy-django-app/windows-browser-django.png)

### <a name="make-changes"></a>Provedení změn
Nyní můžete experimentovat tím, že budete provádět změny zdrojů a/nebo šablon aplikace.

Jakmile změny otestujete, potvrďte je do úložiště Git:

    git add <modified-file>
    git commit -m "<commit-comment>"

### <a name="install-more-packages"></a>Instalace dalších balíčků
Aplikace může mít kromě jazyka Python a rozhraní Django také další závislosti.

Další balíčky můžete nainstalovat pomocí nástroje pip. Chcete-li nainstalovat sadu Azure SDK pro Python, která umožňuje přístup k úložišti Azure, sběrnici Service Bus a dalším službám Azure, zadejte:

    env\scripts\pip install azure

Nezapomeňte aktualizovat soubor requirements.txt:

    env\scripts\pip freeze > requirements.txt

Potvrďte změny:

    git add requirements.txt
    git commit -m "Added azure package"

### <a name="deploy-to-azure"></a>Nasazení do Azure
Chcete-li aktivovat nasazení, nuceně vložte (push) změny do Azure:

    git push azure master

Zobrazí se výstup skriptu nasazení, včetně vytvoření virtuálního prostředí, instalace balíčků, vytvoření souboru web.config.

Chcete-li zobrazit změny, přejděte na adresu URL Azure.

## <a name="web-app-development---maclinux---command-line"></a>Vývoj webových aplikací – Mac/Linux – příkazový řádek
### <a name="clone-the-repository"></a>Klonování úložiště
Nejprve naklonujte úložiště pomocí adresy URL poskytnuté na portálu Azure a přidejte úložiště Azure jako vzdálené. Další informace naleznete v tématu [Místní nasazení přes Git do Azure App Service](app-service-deploy-local-git.md).

    git clone <repo-url>
    cd <repo-folder>
    git remote add azure <repo-url>

### <a name="create-virtual-environment"></a>Vytvoření virtuálního prostředí
Vytvoříme nové virtuální prostředí pro účely vývoje (nepřidávejte jej do úložiště). Virtuální prostředí v jazyce Python nejsou přemístitelná, a proto si každý vývojář pracující na aplikaci vytvoří místně své vlastní virtuální prostředí.

Nezapomeňte použít stejnou verzi jazyka Python, jaká byla vybrána pro webovou aplikaci (v souboru runtime.txt nebo v okně Nastavení aplikace webové aplikace na portálu Azure).

Pro jazyk Python 2.7:

    python -m virtualenv env

Pro jazyk Python 3.4:

    python -m venv env

nebo

    pyvenv env

Nainstalujte veškeré případné externí balíčky požadované aplikací. Můžete použít soubor requirements.txt v kořenovém adresáři úložiště k instalaci balíčků ve virtuálním prostředí:

    env/bin/pip install -r requirements.txt

### <a name="create-a-superuser"></a>Vytvoření superuživatele
V databázi, která je součástí aplikace, není definován žádný superuživatel. Chcete-li používat funkci přihlašování v aplikaci nebo rozhraní správce Django (pokud se jej rozhodnete povolit), bude nutné vytvořit superuživatele.

Spusťte tento příkaz z příkazového řádku ze složky projektu:

    env/bin/python manage.py createsuperuser

Postupujte podle výzev a nastavte uživatelské jméno, heslo atd.

### <a name="run-using-development-server"></a>Spuštění pomocí vývojového serveru
Následujícím příkazem můžete aplikaci spustit v rámci vývojového serveru:

    env/bin/python manage.py runserver

Konzola zobrazí adresa URL a port, jimž server naslouchá:

![](./media/web-sites-python-create-deploy-django-app/mac-run-local-django.png)

Poté tuto adresu URL otevřete ve webovém prohlížeči.

![](./media/web-sites-python-create-deploy-django-app/mac-browser-django.png)

### <a name="make-changes"></a>Provedení změn
Nyní můžete experimentovat tím, že budete provádět změny zdrojů a/nebo šablon aplikace.

Jakmile změny otestujete, potvrďte je do úložiště Git:

    git add <modified-file>
    git commit -m "<commit-comment>"

### <a name="install-more-packages"></a>Instalace dalších balíčků
Aplikace může mít kromě jazyka Python a rozhraní Django také další závislosti.

Další balíčky můžete nainstalovat pomocí nástroje pip. Chcete-li nainstalovat sadu Azure SDK pro Python, která umožňuje přístup k úložišti Azure, sběrnici Service Bus a dalším službám Azure, zadejte:

    env/bin/pip install azure

Nezapomeňte aktualizovat soubor requirements.txt:

    env/bin/pip freeze > requirements.txt

Potvrďte změny:

    git add requirements.txt
    git commit -m "Added azure package"

### <a name="deploy-to-azure"></a>Nasazení do Azure
Chcete-li aktivovat nasazení, nuceně vložte (push) změny do Azure:

    git push azure master

Zobrazí se výstup skriptu nasazení, včetně vytvoření virtuálního prostředí, instalace balíčků, vytvoření souboru web.config.

Chcete-li zobrazit změny, přejděte na adresu URL Azure.

## <a name="troubleshooting---package-installation"></a>Řešení potíží – instalace balíčku
[!INCLUDE [web-sites-python-troubleshooting-package-installation](../../includes/web-sites-python-troubleshooting-package-installation.md)]

## <a name="troubleshooting---virtual-environment"></a>Řešení potíží – virtuální prostředí
[!INCLUDE [web-sites-python-troubleshooting-virtual-environment](../../includes/web-sites-python-troubleshooting-virtual-environment.md)]

## <a name="troubleshooting---static-files"></a>Řešení potíží – statické soubory
Rozhraní Django obsahuje koncepci shromažďování statických souborů. Tato akce kopíruje veškeré statické soubory z původního umístění do jediné složky. V případě této aplikace jsou kopírovány do složky `/static`.

Tato akce se provádí proto, že statické soubory mohou pocházet z různých „aplikací“ Django. Statické soubory z rozhraní správce Django jsou například umístěny v podsložce knihovny Django ve virtuálním prostředí. Statické soubory definované touto aplikací jsou umístěny ve složce `/app/static`. Při používání vícero „aplikací“ Django se statické soubory nacházejí na více místech.

Při spuštění aplikace v režimu ladění obsluhuje aplikace statické soubory z původního umístění.

Při spuštění aplikace v režimu vydání aplikace statické soubory **neobsluhuje**. Za obsluhu souborů je odpovědný webový server. U této aplikace bude služba IIS obsluhovat statické soubory z adresáře `/static`.

Shromažďování statických souborů je prováděno automaticky v rámci skriptu nasazení, přičemž dříve shromážděné soubory jsou vymazány. To znamená, že shromažďování probíhá při každém nasazení, čímž nasazení mírně zpomaluje, ale současně zajišťuje, aby nebyly dostupné zastaralé soubory, a předchází tak potenciálním potížím se zabezpečením.

Chcete-li u aplikace rozhraní Django přeskočit shromažďování statických souborů:

    \.skipDjango

Potom bude nutné provést shromažďování ručně v místním počítači:

    env\scripts\python manage.py collectstatic

Potom odeberte složku `\static` z `.gitignore` a přidejte ji do úložiště Git.

## <a name="troubleshooting---settings"></a>Řešení potíží – nastavení
Různá nastavení aplikace lze změnit v souboru `DjangoWebProject/settings.py`.

Z důvodu usnadnění práce vývojářů je povolen režim ladění. Díky tomu je například možné při místním spuštění zobrazit obrázky a další statický obsah, aniž by bylo nutné shromažďovat statické soubory.

Chcete-li zakázat režim ladění:

    DEBUG = False

Když je zakázáno ladění, hodnotu položky `ALLOWED_HOSTS` je nutné aktualizovat tak, aby obsahovala název hostitele Azure. Příklad:

    ALLOWED_HOSTS = (
        'pythonapp.azurewebsites.net',
    )

Případně lze povolit všechny hostitele:

    ALLOWED_HOSTS = (
        '*',
    )

Je pravděpodobné, že v praxi budete chtít získávání názvu hostitele a přepínání mezi režimem ladění a vydání vyřešit komplexněji.

Můžete nastavit proměnné prostředí prostřednictvím stránky **KONFIGURACE** portálu Azure, a to v části **nastavení aplikace**.  To může být užitečné pro nastavení hodnot, které nechcete zobrazit ve zdrojích (připojovací řetězce, hesla atd.) nebo které chcete v Azure a v místním počítači nastavit odlišně. V souboru `settings.py` můžete dotazem zjišťovat proměnné prostředí pomocí `os.getenv`.

## <a name="using-a-database"></a>Používání databáze
Databáze, která je součástí aplikace, je databáze SQLite. Jedná se o praktickou a užitečnou výchozí databázi pro použití při vývoji, protože nevyžaduje téměř žádné nastavení. Databáze je uložena v souboru db.sqlite3 ve složce projektu.

Azure poskytuje databázové služby, které lze snadno použít z aplikace Django. Kurzy zaměřené na používání databáze [SQL Database] a [MySQL] z aplikace Django vysvětlují postup vytvoření databázové služby, změny nastavení databáze v souboru `DjangoWebProject/settings.py` a popisují knihovny, které je nutné nainstalovat.

Samozřejmě, pokud raději spravujete své vlastní databázové servery, můžete k tomu použít virtuální počítače se systémem Windows nebo Linux spuštěné v Azure.

## <a name="django-admin-interface"></a>Rozhraní správce Django
Jakmile začnete vytvářet modely, budete chtít databázi naplnit určitými daty. Jedním ze snadných způsobů, jak interaktivně přidávat a upravovat obsah, je použít rozhraní pro správu Django.

Kód rozhraní pro správu je uveden jako komentář ve zdrojích aplikace, ale je přehledně označen tak, abyste jej mohli snadno povolit (vyhledejte „admin“).

Po jeho povolení synchronizujte databázi, spusťte aplikaci a přejděte do `/admin`.

## <a name="next-steps"></a>Další kroky
Potřebujete-li další informace o rozhraní Django a nástrojích Python Tools pro Visual Studio, použijte tyto odkazy:

* [Dokumentace rozhraní Django]
* [Dokumentace nástrojů Python Tools pro Visual Studio]

Informace týkající se použití databáze SQL Database a MySQL naleznete v tématech:

* [Django a MySQL v Azure s nástroji Python Tools pro Visual Studio]
* [Django a SQL Database v Azure s nástroji Python Tools pro Visual Studio]

Další informace naleznete ve [Středisku pro vývojáře Python](/develop/python/).

## <a name="whats-changed"></a>Co se změnilo
* Průvodce změnou z webů na službu App Service naleznete v tématu: [Služba Azure App Service a její vliv na stávající služby Azure](http://go.microsoft.com/fwlink/?LinkId=529714)

<!--Link references-->
[Django a MySQL v Azure s nástroji Python Tools pro Visual Studio]: web-sites-python-ptvs-django-mysql.md
[Django a SQL Database v Azure s nástroji Python Tools pro Visual Studio]: web-sites-python-ptvs-django-sql.md
[SQL Database]: web-sites-python-ptvs-django-sql.md
[MySQL]: web-sites-python-ptvs-django-mysql.md

<!--External Link references-->
[Azure SDK pro Python 2.7]: http://go.microsoft.com/fwlink/?linkid=254281
[Azure SDK pro Python 3.4]: http://go.microsoft.com/fwlink/?linkid=516990
[python.org]: http://www.python.org/
[Git pro Windows]: http://msysgit.github.io/
[GitHub pro Windows]: https://windows.github.com/
[Python Tools pro Visual Studio]: http://aka.ms/ptvs
[Python Tools 2.2 pro Visual Studio]: http://go.microsoft.com/fwlink/?LinkID=624025
[Visual Studio]: http://www.visualstudio.com/
[Dokumentace nástrojů Python Tools pro Visual Studio]: http://aka.ms/ptvsdocs
[Dokumentace rozhraní Django]: https://www.djangoproject.com/

