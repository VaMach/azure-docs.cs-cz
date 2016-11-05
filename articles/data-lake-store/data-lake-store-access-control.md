---
title: Přehled řízení přístupu v Data Lake Store | Microsoft Docs
description: Zde se dozvíte, jak funguje řízení přístupu v Azure Data Lake Store
services: data-lake-store
documentationcenter: ''
author: nitinme
manager: jhubbard
editor: cgronlun

ms.service: data-lake-store
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 09/06/2016
ms.author: nitinme

---
# Řízení přístupu v Azure Data Lake Store
Data Lake Store implementuje model řízení přístupu, který je odvozen z HDFS a následně z modelu řízení přístupu POSIX. Tento článek shrnuje základy modelu řízení přístupu pro Data Lake Store. Další informace o modelu řízení přístupu HDFS najdete v příručce [HDFS Permissions Guide](https://hadoop.apache.org/docs/current/hadoop-project-dist/hadoop-hdfs/HdfsPermissionsGuide.html) (Průvodce oprávněními v HDFS).

## Seznamy řízení přístupu k souborům a složkám
Existují dva druhy seznamy řízení přístupu (ACL) – **přístupové seznamy ACL** a **výchozí seznamy ACL**.

* **Přístupové seznamy ACL** – Řídí přístup k objektu. Přístupové seznamy ACL jsou definovány pro soubory i složky.
* **Výchozí seznamy ACL** – „Šablona“ seznamů ACL přidružených ke složce, které určují přístupové seznamy ACL pro všechny podřízené položky vytvořené v rámci příslušné složky. Výchozí seznamy ACL nejsou definovány pro soubory.

![Seznamy ACL služby Data Lake Store](./media/data-lake-store-access-control/data-lake-store-acls-1.png)

Přístupové seznamy ACL i výchozí seznamy ACL mají stejnou strukturu.

![Seznamy ACL služby Data Lake Store](./media/data-lake-store-access-control/data-lake-store-acls-2.png)

> [!NOTE]
> Změna výchozího seznamu ACL u nadřazené položky nemá vliv na přístupový seznam ACL ani na výchozí seznam ACL u podřízených položek, které již existují.
> 
> 

## Uživatelé a identity
Každý soubor a složka má samostatná oprávnění pro tyto identity:

* Vlastnící uživatel
* Vlastnící skupina
* Pojmenovaní uživatelé
* Pojmenované skupiny
* Všichni ostatní uživatelé

Identity uživatelů a skupin jsou identity Azure Active Directory (AAD), takže pokud není uvedeno jinak, „uživatel“ v kontextu Data Lake Store může znamenat buď uživatele služby AAD, nebo skupinu zabezpečení služby AAD.

## Oprávnění
Pro objekt systému souborů jsou definována oprávnění **Číst**, **Zapisovat** a **Provést** a pro soubory a složky je lze používat podle následující tabulky.

|  | File | Složka |
| --- | --- | --- |
| **Číst (R)** |Může číst obsah souboru |Pro vypsání obsahu složky jsou vyžadována oprávnění **Číst** a **Provést**. |
| **Zapisovat (W)** |Může zapisovat do souboru nebo k němu připojovat data |Pro vytváření podřízených položek ve složce jsou vyžadována oprávnění **Zapisovat a Provést**. |
| **Provést (X)** |V kontextu Data Lake Store nemá žádný význam |Je vyžadováno pro přecházení mezi podřízenými položkami složky. |

### Zkrácené verze oprávnění
Zápis **RWX** se používá k označení **Číst + Zapisovat + Provést**. Používá se i zhuštěná číselná verze, která využívá nahrazení **Číst = 4**, **Zapisovat = 2** a **Provést = 1**, přičemž oprávnění je vyjádřeno součtem. Dále je uvedeno několik příkladů.

| Číselný tvar | Krátký tvar | Význam |
| --- | --- | --- |
| 7 |RWX |Číst + Zapisovat + Provést |
| 5 |R-X |Číst + Provést |
| 4 |R-- |Čtení |
| 0 |--- |Žádná oprávnění |

### Oprávnění se nedědí
V modelu stylu POSIX používaném službou Data Lake Store jsou oprávnění pro položku uložena přímo s příslušnou položkou. Jinými slovy: oprávnění pro položku nelze zdědit z nadřazených položek.

## Běžné scénáře týkající se oprávnění
Zde jsou uvedeny některé obvyklé scénáře, které vám pomohou pochopit, která oprávnění jsou nutná k provádění určitých operací s účtem Data Lake Store.

### Oprávnění potřebná ke čtení souboru
![Seznamy ACL služby Data Lake Store](./media/data-lake-store-access-control/data-lake-store-acls-3.png)

* Ke čtení souboru volající potřebuje oprávnění **Číst**.
* Pro všechny složky ve struktuře složek, které soubor obsahují, volající potřebuje oprávnění **Provést**.

### Oprávnění potřebná k připojení dat k souboru
![Seznamy ACL služby Data Lake Store](./media/data-lake-store-access-control/data-lake-store-acls-4.png)

* Pro soubor, ke kterému se mají připojit dat, volající potřebuje oprávnění **Zapisovat**.
* Pro všechny složky, které soubor obsahují, volající potřebuje oprávnění **Provést**.

### Oprávnění potřebná k odstranění souboru
![Seznamy ACL služby Data Lake Store](./media/data-lake-store-access-control/data-lake-store-acls-5.png)

* Pro nadřazenou složku volající potřebuje oprávnění **Zapisovat + Provést**.
* Pro všechny ostatní složky v cestě k souboru volající potřebuje oprávnění **Provést**.

> [!NOTE]
> Oprávnění k zápisu pro soubor nejsou vyžadována při odstraňování souboru, pokud jsou splněny obě uvedené podmínky.
> 
> 

### Oprávnění k výpisu obsahu složky
![Seznamy ACL služby Data Lake Store](./media/data-lake-store-access-control/data-lake-store-acls-6.png)

* K výpisu obsahu složky volající potřebuje oprávnění **Číst + Provést**.
* Pro všechny složky předchůdce volající potřebuje oprávnění **Provést**.

## Oprávnění k zobrazení v rámci Azure Portal
V okně **Průzkumník dat** účtu Data Lake Store klikněte na **Přístup**. Zobrazí se seznamy ACL pro soubor nebo složku. Po kliknutí na Přístup na následujícím snímku obrazovky se zobrazí seznamy ACL pro složku **catalog** v rámci účtu **mydatastore**.

![Seznamy ACL služby Data Lake Store](./media/data-lake-store-access-control/data-lake-store-show-acls-1.png)

Poté v okně **Přístup** kliknutím na **Jednoduché zobrazení** přejděte k jednoduššímu zobrazení.

![Seznamy ACL služby Data Lake Store](./media/data-lake-store-access-control/data-lake-store-show-acls-simple-view.png)

Kliknutím na **Podrobné zobrazení** přejdete k podrobnějšímu zobrazení.

![Seznamy ACL služby Data Lake Store](./media/data-lake-store-access-control/data-lake-store-show-acls-advance-view.png)

## Superuživatel
Superuživatel má ze všech uživatelů služby Data Lake Store nejrozsáhlejší oprávnění. Superuživatel:

* Má oprávnění RWX ke **všem** souborům a složkám.
* Může měnit oprávnění pro kterýkoli soubor nebo složku.
* Může měnit vlastnícího uživatele nebo vlastnící skupinu pro kterýkoli soubor nebo složku.

V Azure má účet Data Lake Store několik rolí Azure:

* Vlastníci
* Přispěvatelé
* Čtenáři
* Atd.

Všichni členové role **Vlastníci** pro účet Data Lake Store jsou automaticky superuživateli pro příslušný účet. Další informace o řízení přístupu na základě role najdete v tématu [Řízení přístupu na základě role](../active-directory/role-based-access-control-configure.md).

## Vlastnící uživatel
Uživatel, který položku vytvořil, je automaticky jejím vlastníkem. Vlastnící uživatel může:

* Měnit oprávnění souboru, jehož vlastníkem je.
* Měnit vlastnící skupinu pro vlastněný soubor, pokud je vlastnící uživatel členem cílové skupiny.

> [!NOTE]
> Vlastnící uživatel **nemůže** měnit vlastnícího uživatele u jiného vlastněného souboru. Vlastnícího uživatele souboru nebo složky, mohou měnit pouze superuživatelé.
> 
> 

## Vlastnící skupina
V seznamech ACL POSIX je ke každému uživateli přiřazena „primární skupina“. Uživatel „alice“ může například patřit do skupiny „finance“. Alice může patřit do více skupin, ale jedna skupina je vždy označena jako její primární skupina. Když Alice vytvoří soubor v rámci specifikace POSIX, bude jako vlastnící skupina tohoto souboru nastavena její primární skupina, což je v tomto případě skupina „finance“.

Když je vytvořena nová položka systému souborů, Data Lake Store přiřadí hodnotu vlastnící skupině. 

* **Případ 1** –Kořenová složka „/“. Tato složka se vytvoří při vytvoření účtu Data Lake Store. V takovém případě je vlastnící skupina nastavena podle uživatele, který účet vytvořil.
* **Případ 2** (všechny ostatní případy) – Při vytvoření nové položky se vlastnící skupina zkopíruje z nadřazené složky.

Vlastnící skupinu smí změnit:

* Všichni uživatelé
* Vlastnící uživatel, pokud je také členem cílové skupiny.

## Algoritmus kontroly přístupu
Následující obrázek ilustruje algoritmus kontroly přístupu pro účty Data Lake Store.

![Algoritmus seznamů ACL služby Data Lake Store](./media/data-lake-store-access-control/data-lake-store-acls-algorithm.png)

## Maska a „efektivní oprávnění“
**Maska** je hodnota RWX, která se používá k omezení přístupu pro **pojmenované uživatele**, **vlastnící skupinu** a **pojmenované skupiny** při použití algoritmu kontroly přístupu. Zde jsou uvedeny klíčové koncepce pro masku. 

* Maska vytvoří „efektivní oprávnění“; to znamená, že upraví oprávnění v době kontroly přístupu.
* Masku může přímo upravit vlastník souboru a kterýkoli superuživatel.
* Maska může odebrat oprávnění k vytvoření efektivního oprávnění. Maska **nemůže** přidávat oprávnění k efektivnímu oprávnění. 

Podívejme se na několik příkladů. V následujícím příkladu je maska nastavena na hodnotu **RWX**, což znamená, že maska neodebere žádná oprávnění. Všimněte si, že efektivní oprávnění pro pojmenovaného uživatele, vlastnící skupinu a pojmenovanou skupinu se během kontroly přístupu nezmění.

![Seznamy ACL služby Data Lake Store](./media/data-lake-store-access-control/data-lake-store-acls-mask-1.png)

V následujícím příkladu je nastavena maska **R-X**. To znamená, že v době kontroly přístupu **deaktivuje oprávnění Zapisovat** pro **pojmenovaného uživatele**, **vlastnící skupinu** a **pojmenovanou skupinu**.

![Seznamy ACL služby Data Lake Store](./media/data-lake-store-access-control/data-lake-store-acls-mask-2.png)

Pro informaci je zde uvedeno, kde se maska pro soubor či složku objevuje v rámci Azure Portal.

![Seznamy ACL služby Data Lake Store](./media/data-lake-store-access-control/data-lake-store-show-acls-mask-view.png)

> [!NOTE]
> Pro nový účet Data Lake Store je použita výchozí maska přístupového seznamu ACL a výchozího seznamu ACL kořenové složky („/“) s hodnotou RWX.
> 
> 

## Oprávnění pro nové soubory a složky
Při vytvoření nového souboru nebo složky v rámci existující složky se podle výchozího seznamu ACL pro nadřazenou složku určí:

* Výchozí seznam ACL a přístupový seznam ACL podřízené složky
* Přístupový seznam ACL podřízeného souboru (pro soubory není definován výchozí seznam ACL)

### Přístupový seznam ACL podřízeného souboru nebo složky
Když je vytvořen podřízený soubor nebo složka, je výchozí seznam ACL nadřazené položky zkopírován jako přístupový seznam ACL podřízeného souboru nebo složky. Pokud má navíc **jiný** uživatel nastavena oprávnění RWX ve výchozím seznamu ACL nadřazené položky, je zcela odebrán z přístupového seznamu ACL podřízené položky.

![Seznamy ACL služby Data Lake Store](./media/data-lake-store-access-control/data-lake-store-acls-child-items-1.png)

Ve většině scénářů pokrývají informace uvedené výše vše, co potřebujete vědět o určování přístupového seznamu ACL podřízené položky. Pokud jste však seznámeni se systémy POSIX a chcete podrobněji porozumět postupu dosažení této transformace, najdete informace v části [Role funkce umask při vytváření přístupového seznamu ACL pro nové soubory a složky](#umasks-role-in-creating-the-access-acl-for-new-files-and-folders) dále v tomto článku.

### Výchozí seznam ACL podřízené složky
Když je v rámci nadřazené složky vytvořena podřízená složka, výchozí seznam ACL nadřazené složky se beze změny zkopíruje jako výchozí seznam ACL podřízené složky.

![Seznamy ACL služby Data Lake Store](./media/data-lake-store-access-control/data-lake-store-acls-child-items-2.png)

## Pokročilá témata pro pochopení seznamů ACL v Data Lake Store
Níže je uvedeno několik pokročilých témat, která vám pomohou pochopit, jak se určují seznamy ACL pro soubory nebo složky Data Lake Store.

### Role funkce umask při vytváření přístupového seznamu ACL pro nové soubory a složky
Ve systému odpovídajícím standardu POSIX se používá obecná koncepce 9bitové hodnoty umask pro nadřazenou složku. Tato hodnota se používá při převodu oprávnění **vlastnícího uživatele**, **vlastnící skupiny** a **jiné** na přístupový seznam ACL nového podřízeného souboru nebo složky. Bity hodnoty umask identifikují bity, které mají být deaktivovány v přístupovém seznamu ACL podřízené položky. Tento postup se proto používá k selektivnímu zabránění šíření oprávnění pro vlastnícího uživatele, vlastnící skupinu a jiné.

V systému HDFS je umask zpravidla možnost konfigurace v rámci celé lokality a řídí ji správci. Data Lake Store používá funkci **umask v rámci účtu**, kterou nelze změnit. V následující tabulce je uvedena funkce umask služby Data Lake Store.

| Uživatelská skupina | Nastavení | Vliv na přístupový seznam ACL nové podřízené položky |
| --- | --- | --- |
| Vlastnící uživatel |--- |Žádný vliv |
| Vlastnící skupina |--- |Žádný vliv |
| Ostatní |RWX |Odebrání oprávnění Číst + Zapisovat + Provést |

Následující obrázek znázorňuje praktické použití funkce umask. Výsledkem je odebrání oprávnění **Číst + Zapisovat + Provést** pro **jiné** uživatele. Jelikož funkce umask neurčila bity pro **vlastnícího uživatele** a **vlastnící skupinu**, nejsou tato oprávnění transformována.

![Seznamy ACL služby Data Lake Store](./media/data-lake-store-access-control/data-lake-store-acls-umask.png) 

### Bit sticky
Bit sticky představuje pokročilejší funkci systému souborů POSIX. V kontextu Data Lake Store bit sticky pravděpodobně nebude potřebný.

V následující tabulce je popsáno fungování bitu sticky v Data Lake Store.

| Uživatelská skupina | File | Složka |
| --- | --- | --- |
| Bit sticky **VYPNUTÝ** |Žádný vliv |Žádný vliv |
| Bit sticky **ZAPNUTÝ** |Žádný vliv |Brání všem s výjimkou **superuživatelů** a **vlastnícího uživatele** podřízené položky v odstranění nebo přejmenování této podřízené položky. |

Bit sticky se v rámci Azure Portal nezobrazuje.

## Běžné otázky týkající se seznamů ACL v Data Lake Store
Zde je uvedeno několik otázek, které se často vyskytují v souvislosti se seznamy ACL v Data Lake Store.

### Je třeba povolit podporu pro seznamy ACL?
Ne. Řízení přístupu prostřednictvím seznamů ACL je pro účet Data Lake Store vždy aktivní.

### Jaká oprávnění jsou vyžadována pro rekurzivní odstranění složky a jejího obsahu?
* Pro nadřazenou složku musí být nastaveno oprávnění **Zapisovat + Provést**.
* Pro složku, která se má odstranit, a všechny složky, které obsahuje, se vyžaduje oprávnění **Číst + Zapisovat + Provést**.
  >[AZURE.NOTE] Odstraňování souborů ve složkách nevyžaduje oprávnění Zapisovat pro tyto soubory. Kořenovou složku „/“ navíc nelze **nijak** odstranit.

### Kdo je nastaven jako vlastník souboru nebo složky?
Vlastníkem souboru nebo složky se stane příslušný tvůrce.

### Kdo je nastaven jako vlastnící skupina souboru nebo složky při vytvoření?
Nastavení se zkopíruje z vlastnící skupiny nadřazené složky, v rámci které se nový soubor nebo složka vytvoří.

### Jsem vlastnícím uživatelem souboru, ale nemám potřebné oprávnění RWX. Co mám udělat?
Vlastnící uživatel může snadno změnit oprávnění k souboru a sám si udělit veškerá potřebná oprávnění RWX.

### Podporuje služba Data Lake Store dědění seznamů ACL?
Ne.

### Jaký je rozdíl mezi vlastnostmi maska a umask?
| Vlastnost maska | Vlastnost umask |
| --- | --- |
| Vlastnost **maska** je k dispozici u všech souborů a složek. |**Umask** je vlastnost účtu Data Lake Store. V rámci Data Lake Store tedy existuje pouze jedna vlastnost umask. |
| Vlastnost maska pro soubor nebo složku může změnit vlastnící uživatel nebo vlastnící skupina souboru či superuživatel. |Vlastnost umask nemůže změnit žádný uživatel, dokonce ani superuživatel. Tato hodnota je neměnná, konstantní. |
| Vlastnost maska se používá při provádění algoritmu kontroly přístupu za běhu a slouží k určení, zda má uživatel oprávnění k provedení operace pro soubor či složku. Rolí masky je vytvoření „efektivních oprávnění“ v době kontroly přístupu. |Vlastnost umask se během kontroly přístupu vůbec nepoužívá. Vlastnost umask se používá k určení přístupového seznamu ACL nových podřízených položek složky. |
| Maska je 3bitová hodnota RWX, která se vztahuje na pojmenovaného uživatele, pojmenovanou skupinu a vlastnícího uživatele v době kontroly přístupu. |Hodnota umask je 9bitová a vztahuje se na vlastnícího uživatele, vlastnící skupiny a jiných pro novou podřízenou položku. |

### Kde najdu další informace o modelu řízení přístupu POSIX?
* [http://www.vanemery.com/Linux/ACL/POSIX_ACL_on_Linux.html](http://www.vanemery.com/Linux/ACL/POSIX_ACL_on_Linux.html)
* [HDFS Permission Guide (Průvodce oprávněními v HDFS)](http://hadoop.apache.org/docs/current/hadoop-project-dist/hadoop-hdfs/HdfsPermissionsGuide.html) 
* [Nejčastější dotazy týkající se specifikace POSIX](http://www.opengroup.org/austin/papers/posix_faq.html)
* [POSIX 1003.1 2008](http://standards.ieee.org/findstds/standard/1003.1-2008.html)
* [POSIX 1003.1e 1997](http://users.suse.com/~agruen/acl/posix/Posix_1003.1e-990310.pdf)
* [POSIX ACL on Linux (Seznamy ACL v rámci specifikace POSIX v systému Linux)](http://users.suse.com/~agruen/acl/linux-acls/online/)
* [ACL using Access Control Lists on Linux (Řízení přístupu pomocí seznamů řízení přístupu v systému Linux)](http://bencane.com/2012/05/27/acl-using-access-control-lists-on-linux/)

## Viz také
* [Přehled Azure Data Lake Store](data-lake-store-overview.md)
* [Začínáme s Azure Data Lake Analytics](../data-lake-analytics/data-lake-analytics-get-started-portal.md)

<!--HONumber=Sep16_HO3-->


