---
title: "Konfigurace clusterů HDInsight připojený k doméně - Azure | Microsoft Docs"
description: "Zjistěte, jak připravit a nakonfigurovat clustery HDInsight připojený k doméně"
services: hdinsight
documentationcenter: 
author: saurinsh
manager: cgronlun
editor: cgronlun
tags: 
ms.assetid: 0cbb49cc-0de1-4a1a-b658-99897caf827c
ms.service: hdinsight
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 01/24/2018
ms.author: saurinsh
ms.openlocfilehash: 6284b246c071fb99a8b47845aca34b6262e5b856
ms.sourcegitcommit: 79683e67911c3ab14bcae668f7551e57f3095425
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/25/2018
---
# <a name="configure-domain-joined-hdinsight-sandbox-environment"></a>Konfigurace prostředí izolovaného prostoru HDInsight připojený k doméně

Zjistěte, jak nastavit cluster Azure HDInsight pomocí samostatné služby Active Directory a [Apache škálu](http://hortonworks.com/apache/ranger/) využívat výhod silné ověřování a bohaté přístupu podle rolí zásady řízení (RBAC). Další informace najdete v tématu [clustery HDInsight připojený k doméně zavádět](apache-domain-joined-introduction.md).

Bez clusteru HDInsight připojený k doméně, každý cluster může mít pouze účet uživatele Hadoop HTTP a uživatelský účet SSH.  Ověřování více uživatelů lze dosáhnout pomocí:

-   Samostatné služby Active Directory spuštěné v Azure IaaS.
-   Azure Active Directory.

Pomocí samostatné služby Active Directory spuštěné v Azure IaaS je popsaná v tomto článku. Je nejjednodušší architekturu, kterou můžete získat podporu pro více uživatelů v HDInsight podle zákazníka. Tento článek popisuje dva přístupy pro tuto konfiguraci:

- Možnost 1: Použijte jednu šablonu správu prostředků Azure k vytvoření samostatné služby active directory a clusteru HDInsight.
- Možnost 2: Celého procesu je rozdělen do následujících kroků:
    - Vytvoření služby Active Directory pomocí šablony.
    - Instalační program LDAPS.
    - Vytvoření AD uživatelů a skupin
    - Vytvoření clusteru HDInsight

> [!IMPORTANT]
> Oozie není povoleno v doméně HDInsight.

## <a name="prerequisite"></a>Požadavek
* Předplatné Azure

## <a name="option-1-one-step-approach"></a>Možnost 1: jednoduchý přístup
V této části otevřete šablonu správu prostředků Azure z portálu Azure. Šablona se používá k vytvoření samostatné služby Active Directory a HDInsight cluster. Můžete vytvořit aktuálně připojený k doméně clusteru Hadoop, Spark cluster a interaktivní dotazu cluster.

1. Kliknutím na následující obrázek otevřete šablonu na portálu Azure Portal. Tuto šablonu najdete v [šablonách Azure pro rychlý start](https://azure.microsoft.com/resources/templates/).
   
    Vytvoření clusteru Spark:

    <a href="https://portal.azure.com/#create/Microsoft.Template/uri/http%3A%2F%2Fhditutorialdata.blob.core.windows.net%2Fdomain-joined%2Fspark%2Ftemplate.json" target="_blank"><img src="../hbase/media/apache-hbase-tutorial-get-started-linux/deploy-to-azure.png" alt="Deploy to Azure"></a>

    K vytvoření clusteru interaktivní dotazu:

    <a href="https://portal.azure.com/#create/Microsoft.Template/uri/http%3A%2F%2Fhditutorialdata.blob.core.windows.net%2Fdomain-joined%2Finteractivequery%2Ftemplate.json" target="_blank"><img src="../hbase/media/apache-hbase-tutorial-get-started-linux/deploy-to-azure.png" alt="Deploy to Azure"></a>

    Vytvoření clusteru Hadoop:

    <a href="https://portal.azure.com/#create/Microsoft.Template/uri/http%3A%2F%2Fhditutorialdata.blob.core.windows.net%2Fdomain-joined%2Fhadoop%2Ftemplate.json" target="_blank"><img src="../hbase/media/apache-hbase-tutorial-get-started-linux/deploy-to-azure.png" alt="Deploy to Azure"></a>

2. Zadejte hodnoty, vyberte **souhlasím s podmínkami a ujednáními výše uvedených**, vyberte **připnout na řídicí panel**a potom klikněte na **nákupu**. Ukazatelem myši přihlašovací vysvětlení vedle pole zobrazíte popisy. Většina hodnot jsou vyplněna. Můžete buď výchozí hodnoty, nebo vlastní hodnoty.

    - **Skupina prostředků**: Zadejte název skupiny prostředků Azure.
    - **Umístění**: Vyberte umístění, které je blízko vás.
    - **Název nového účtu úložiště**: Zadejte název účtu úložiště Azure. Tento nový účet úložiště je primární řadič domény, BDC a HDInsight cluster používá jako výchozí účet úložiště.
    - **Uživatelské jméno správce**: Zadejte uživatelské jméno správce domény.
    - **Heslo správce**: Zadejte heslo správce domény.
    - **Název domény**: výchozí název je *contoso.com*.  Pokud změníte název domény, je nutné také aktualizovat **zabezpečení certifikátu LDAP** pole a **rozlišující název organizační jednotky** pole.
    - **Předpona DNS**: Zadejte předponu DNS pro veřejnou IP adresu použitou nástrojem pro vyrovnávání zatížení.
    - **Název clusteru**: Zadejte název clusteru HDInsight.
    - **Typ clusteru**: Tato hodnota se nezmění. Pokud chcete změnit typ clusteru, použijte konkrétní šablonu v posledním kroku.
    - **Zabezpečené heslo certifikátu Ldap**: použijte výchozí hodnotu, pokud nezměníte pole certifikátu zabezpečení protokolu LDAP.
    Některé hodnoty jsou pevně zakódovaná v šabloně, například počet instancí pracovního procesu uzlu je dva.  Chcete-li změnit hodnoty, pevně, klikněte na tlačítko **úpravy šablony**.

    ![Šablona upravit připojený k doméně clusteru HDInsight](./media/apache-domain-joined-configure/hdinsight-domain-joined-edit-template.png)

Po úspěšném dokončení šabloně nejsou 23 prostředky vytvořené ve skupině prostředků.

## <a name="option-2-multi-step-approach"></a>Možnost 2: vícekrokový přístup

V této části jsou čtyři kroky:

1. Vytvoření služby Active Directory pomocí šablony.
2. Instalační program LDAPS.
3. Vytvoření AD uživatelů a skupin
4. Vytvoření clusteru HDInsight

### <a name="create-an-active-directory"></a>Vytvoření služby Active Directory

Šablona Azure Resource Manageru usnadňuje vytváření prostředků Azure. V této části můžete použít [šablony Azure QuickStart](https://azure.microsoft.com/resources/templates/active-directory-new-domain-ha-2-dc/) pro vytvoření nové doménové struktury a domény s dva virtuální počítače. Dva virtuální počítače sloužit jako primární řadič domény a řadičem domény.

**Vytvoření domény s dva řadiče domény**

1. Kliknutím na následující obrázek otevřete šablonu na portálu Azure Portal.

    <a href="https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2Factive-directory-new-domain-ha-2-dc%2Fazuredeploy.json" target="_blank"><img src="./media/apache-domain-joined-configure/deploy-to-azure.png" alt="Deploy to Azure"></a>

    Šablona vypadá takto:

    ![Připojené k doméně HDInsight vytvářet virtuální počítače domény doménové struktury](./media/apache-domain-joined-configure/hdinsight-domain-joined-create-arm-template.png)

2. Zadejte následující hodnoty:

    - **Předplatné:** Vyberte předplatné Azure.
    - **Název skupiny prostředků**: Zadejte název skupiny prostředků.  Skupiny prostředků slouží ke správě vašich prostředků Azure, které se vztahují k projektu.
    - **Umístění**: Vyberte Azure umístění, které je blízko vás.
    - **Uživatelské jméno správce**: Toto je uživatelské jméno správce domény. Tento uživatel není uživatelský účet HTTP clusteru HDInsight. Toto je účet, který používáte v rámci tohoto kurzu.
    - **Heslo správce**: Zadejte heslo správce domény.
    - **Název domény**: název domény musí být dvoudílný název. Například: contoso.com, nebo contoso.local nebo hdinsight.test.
    - **Předpona DNS**: Zadejte předponu DNS
    - **Portu RDP primárního řadiče domény**: (použijte výchozí hodnotu v tomto kurzu)
    - **BDC portu RDP**: (použijte výchozí hodnotu v tomto kurzu)
    - **umístění artefakty**: (použijte výchozí hodnotu v tomto kurzu)
    - **token SAS umístění artefakty**: (nevyplňujte pro tento kurz.)

Pro vytvoření prostředků trvá asi 20 minut.

### <a name="setup-ldaps"></a>Instalační program LDAPS

Přístup protokolu LDAP (Lightweight Directory) se používá ke čtení z a zapisovat do služby AD.

**Pro připojení k primární řadič domény pomocí vzdálené plochy**

1. Přihlaste se k webu [Azure Portal](https://portal.azure.com).
2. Otevřete skupinu prostředků a pak otevřete virtuální počítač primárního řadiče domény. Výchozí název primárního řadiče domény je adPDC. 
3. Klikněte na tlačítko **Connect** pro připojení k primární řadič domény pomocí vzdálené plochy.

    ![Připojené k doméně HDInsight připojení vzdálené plochy primárního řadiče domény](./media/apache-domain-joined-configure/hdinsight-domain-joined-remote-desktop-pdc.png)


**Chcete-li přidat službu Active Directory Certificate services**

4. Otevřete **správce serveru** Pokud není otevřen.
5. Klikněte na tlačítko **spravovat**a potom klikněte na **přidat role a funkce**.

    ![Připojené k doméně HDInsight přidání rolí a funkcí](./media/apache-domain-joined-configure/hdinsight-domain-joined-add-roles.png)
5. Z "Než začnete", klikněte na tlačítko **Další**.
6. Vyberte **instalace na základě rolí nebo na základě funkcí**a potom klikněte na **Další**.
7. Vyberte primární řadič domény a pak klikněte na tlačítko **Další**.  Výchozí název primárního řadiče domény je adPDC.
8. Vyberte **služby Active Directory Certificate Services**.
9. Klikněte na tlačítko **přidat funkce** z tohoto dialogového okna v automaticky otevřeném okně.
10. Postupujte podle pokynů průvodce, použijte výchozí nastavení pro zbytek postupu.
11. Průvodce zavřete kliknutím na **Zavřít**.

**Konfigurovat certifikát služby AD**

1. Ve Správci serveru klikněte na ikonu žlutý oznámení a pak klikněte na tlačítko **konfigurace Active Directory Certificate services**.

    ![HDInsight doméně nakonfigurovat certifikát služby AD](./media/apache-domain-joined-configure/hdinsight-domain-joined-configure-ad-certificate.png)

2. Klikněte na tlačítko **služby rolí** na levé straně vyberte **certifikační autority**a potom klikněte na **Další**.
3. Postupujte podle pokynů průvodce, použijte výchozí nastavení pro zbytek procesu (klikněte na tlačítko **konfigurace** na poslední krok).
4. Průvodce zavřete kliknutím na **Zavřít**.

### <a name="optional-create-ad-users-and-groups"></a>(Volitelné) Vytvoření AD uživatelů a skupin

**Vytvoření uživatelů a skupin ve službě AD**
1. Připojit k primární řadič domény pomocí vzdálené plochy
1. Otevřete **uživatelé a počítače Active Directory**.
2. V levém podokně vyberte název vaší domény.
3. Klikněte **vytvořte nového uživatele v aktuálním kontejneru** ikonu na horní nabídce.

    ![Vytvoření uživatelů připojených k doméně HDInsight](./media/apache-domain-joined-configure/hdinsight-domain-joined-create-ad-user.png)
4. Postupujte podle pokynů vytvořte několik uživatelů. Například hiveuser1 a hiveuser2.
5. Klikněte **vytvořit novou skupinu v aktuálním kontejneru** ikonu na horní nabídce.
6. Postupujte podle pokynů vytvořte skupinu s názvem **HDInsightUsers**.  Tato skupina se používá při vytváření clusteru HDInsight později v tomto kurzu.

> [!IMPORTANT]
> Před vytvořením clusteru HDInsight připojený k doméně, je nutné restartovat virtuální počítač primárního řadiče domény.

### <a name="create-an-hdinsight-cluster-in-the-vnet"></a>Vytvoření clusteru HDInsight ve virtuální síti

V této části použijete k přidání clusteru služby HDInsight do virtuální sítě, které jste vytvořili dříve v tomto kurzu pomocí šablony Resource Manageru na portálu Azure. V tomto článku se vztahuje pouze na konkrétní informace o konfiguraci clusteru připojený k doméně.  Obecné informace najdete v tématu [vytvořit systémem Linux clusterů v HDInsight pomocí portálu Azure](../hdinsight-hadoop-create-linux-clusters-portal.md).  

**K vytvoření clusteru HDInsight se připojený k doméně**

1. Přihlaste se k portálu [Azure Portal](https://portal.azure.com).
2. Otevřete skupinu prostředků, které jste vytvořili dříve v tomto kurzu pomocí šablony Resource Manageru.
3. Přidejte do skupiny prostředků clusteru služby HDInsight.
4. Vyberte **vlastní** možnost:

    ![Možnost vytvořit vlastní připojený k doméně HDInsight](./media/apache-domain-joined-configure/hdinsight-domain-joined-portal-custom-configuration-option.png)

    Jsou šesti oddíly pomocí možnosti vlastní konfigurace: základy, úložiště, aplikace, velikost clusteru, rozšířená nastavení a souhrn.
5. V **Základy** části:

    - Typ clusteru: vyberte **balíček zabezpečení Enterprise**. Aktuálně balíček zabezpečení Enterprise jde Povolit jenom pro následující typy clusteru: Hadoop, interaktivní dotazu a Spark.

        ![Balíček zabezpečení připojené k podnikové doméně HDInsight](./media/apache-domain-joined-configure/hdinsight-creation-enterprise-security-package.png)
    - Uživatelské jméno přihlášení clusteru: Toto je uživatel Hadoop HTTP. Tento účet se liší od účtu správce domény.
    - Skupina prostředků: Vyberte skupinu prostředků, který jste vytvořili dříve pomocí šablony Resource Manageru.
    - Umístění: Umístění musí být stejný jako ten, který jste použili při vytváření virtuální sítě a řadiče domény pomocí šablony Resource Manageru.

6. V **upřesňující nastavení** části:

    - Domain settings:

        ![Upřesnit nastavení domény připojený k doméně HDInsight](./media/apache-domain-joined-configure/hdinsight-domain-joined-portal-advanced-domain-settings.png)
        
        - Název domény: Zadejte název domény, kterou jste použili v [vytvoření služby Active Directory](#create-an-active-directory).
        - Uživatelské jméno domény: Zadejte uživatelské jméno správce AD, kterou jste použili v [vytvoření služby Active Directory](#create-an-active-directory).
        - Organizační jednotka: Najdete na snímku obrazovky příklad.
        - Adresa URL LDAPS: Najdete v článku na snímku obrazovky příklad
        - Přístup skupiny uživatelů: Zadejte název skupiny uživatelů, kterou jste vytvořili v [vytvořit AD Uživatelé a skupiny](#optionally-createad-users-and-groups)
    - Virtuální sítě: vyberte virtuální síť, kterou jste vytvořili v [vytvoření služby Active Directory](#create-an-active-directory). Výchozí název použité v šabloně je **adVNET**.
    - Podsítě: Je výchozím názvem používaným v šabloně **adSubnet**.



Po dokončení tohoto kurzu můžete cluster odstranit. Pomocí HDInsight jsou vaše data uložena v Azure Storage, takže můžete clusteru bezpečně odstranit, pokud není používán. Za cluster služby HDInsight se účtují poplatky, i když se nepoužívá. Vzhledem k tomu, že poplatky za cluster představují několikanásobek poplatků za úložiště, dává ekonomický smysl odstraňovat clustery, které nejsou používány. Postup odstranění clusteru naleznete v tématu [Správa clusterů systému Hadoop v HDInsight pomocí portálu Azure](../hdinsight-administer-use-management-portal.md#delete-clusters).

## <a name="next-steps"></a>Další postup
* Pokud chcete konfigurovat zásady Hivu a spouštět dotazy Hivu, přečtěte si téma [Konfigurace zásad Hivu pro clustery HDInsight připojené k doméně](apache-domain-joined-run-hive.md).
* Pro připojení ke clusterům HDInsight připojený k doméně pomocí protokolu SSH, najdete v části [použití SSH se systémem Linux Hadoop v HDInsight ze systému Linux, Unix nebo OS X](../hdinsight-hadoop-linux-use-ssh-unix.md#domainjoined).

