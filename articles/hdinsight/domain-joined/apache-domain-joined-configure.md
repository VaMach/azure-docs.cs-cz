---
title: "Konfigurace clusterů HDInsight připojený k doméně - Azure | Microsoft Docs"
description: "Zjistěte, jak připravit a nakonfigurovat clustery HDInsight připojený k doméně"
services: hdinsight
documentationcenter: 
author: saurinsh
manager: jhubbard
editor: cgronlun
tags: 
ms.assetid: 0cbb49cc-0de1-4a1a-b658-99897caf827c
ms.service: hdinsight
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 12/15/2017
ms.author: saurinsh
ms.openlocfilehash: 0a9ed1cad8b8d4c566a0da16ac78d096efe187a5
ms.sourcegitcommit: b7adce69c06b6e70493d13bc02bd31e06f291a91
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 12/19/2017
---
# <a name="configure-domain-joined-hdinsight-sandbox-environment"></a>Konfigurace prostředí izolovaného prostoru HDInsight připojený k doméně

Zjistěte, jak nastavit cluster Azure HDInsight pomocí samostatné služby Active Directory a [Apache škálu](http://hortonworks.com/apache/ranger/) využívat výhod silné ověřování a bohaté přístupu podle rolí zásady řízení (RBAC). Další informace najdete v tématu [clustery HDInsight připojený k doméně zavádět](apache-domain-joined-introduction.md).

Bez clusteru HDInsight připojený k doméně, každý cluster může mít pouze účet uživatele Hadoop HTTP a uživatelský účet SSH.  Ověřování více uživatelů lze dosáhnout pomocí:

-   Samostatné služby Active Directory spuštěné v Azure IaaS
-   Azure Active Directory
-   Active Directory spuštěné v místním prostředí zákazníků.

Pomocí samostatné služby Active Directory spuštěné v Azure IaaS je popsaná v tomto článku. Je nejjednodušší architekturu, kterou můžete získat podporu pro více uživatelů v HDInsight podle zákazníka. 

> [!IMPORTANT]
> Oozie není povoleno v doméně HDInsight.

## <a name="prerequisite"></a>Požadavek
* Předplatné Azure

## <a name="create-an-active-directory"></a>Vytvoření služby Active Directory

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

## <a name="setup-ldaps"></a>Instalační program LDAPS

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

2. Klikněte na tlačítko ** vyberte služby rolí na levé straně, **certifikační autority**a potom klikněte na **Další**.
3. Postupujte podle pokynů průvodce, použijte výchozí nastavení pro zbytek procesu (klikněte na tlačítko **konfigurace** na poslední krok).
4. Průvodce zavřete kliknutím na **Zavřít**.

## <a name="optional-create-ad-users-and-groups"></a>(Volitelné) Vytvoření AD uživatelů a skupin

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

## <a name="create-an-hdinsight-cluster-in-the-vnet"></a>Vytvoření clusteru HDInsight ve virtuální síti

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

    - Nastavení domény:

        ![Upřesnit nastavení domény připojený k doméně HDInsight](./media/apache-domain-joined-configure/hdinsight-domain-joined-portal-advanced-domain-settings.png)
        
        - Název domény: Zadejte název domény, kterou jste použili v [vytvoření služby Active Directory](#create-an-active-directory).
        - Uživatelské jméno domény: Zadejte uživatelské jméno správce AD, kterou jste použili v [vytvoření služby Active Directory](#create-an-active-directory).
        - Organizační jednotka: Najdete na snímku obrazovky příklad.
        - Adresa URL LDAPS: Najdete v článku na snímku obrazovky příklad
        - Přístup skupiny uživatelů: Zadejte název skupiny uživatelů, kterou jste vytvořili v [vytvořit AD Uživatelé a skupiny](#optionally-createad-users-and-groups)
    - Virtuální sítě: vyberte virtuální síť, kterou jste vytvořili v [vytvoření služby Active Directory](#create-an-active-directory). Výchozí název použité v šabloně je **adVNET**.
    - Podsítě: Je výchozím názvem používaným v šabloně **adSubnet**.



Po dokončení tohoto kurzu můžete cluster odstranit. Pomocí HDInsight jsou vaše data uložena v Azure Storage, takže můžete clusteru bezpečně odstranit, pokud není používán. Za cluster služby HDInsight se účtují poplatky, i když se nepoužívá. Vzhledem k tomu, že poplatky za cluster představují několikanásobek poplatků za úložiště, dává ekonomický smysl odstraňovat clustery, které nejsou používány. Postup odstranění clusteru naleznete v tématu [Správa clusterů systému Hadoop v HDInsight pomocí portálu Azure](../hdinsight-administer-use-management-portal.md#delete-clusters).

## <a name="next-steps"></a>Další kroky
* Pokud chcete konfigurovat zásady Hivu a spouštět dotazy Hivu, přečtěte si téma [Konfigurace zásad Hivu pro clustery HDInsight připojené k doméně](apache-domain-joined-run-hive.md).
* Pro připojení ke clusterům HDInsight připojený k doméně pomocí protokolu SSH, najdete v části [použití SSH se systémem Linux Hadoop v HDInsight ze systému Linux, Unix nebo OS X](../hdinsight-hadoop-linux-use-ssh-unix.md#domainjoined).

