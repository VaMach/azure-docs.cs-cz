---
title: "Správa clusterů HDInsight připojený k doméně - Azure | Microsoft Docs"
description: "Zjistěte, jak Správa clusterů HDInsight připojený k doméně"
services: hdinsight
documentationcenter: 
author: saurinsh
manager: jhubbard
editor: cgronlun
tags: 
ms.assetid: 6ebc4d2f-2f6a-4e1e-ab6d-af4db6b4c87c
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 10/25/2016
ms.author: saurinsh
ms.openlocfilehash: 0fc32960fc2f1ae69315dbfd6bfb8c34c4adc0fa
ms.sourcegitcommit: be0d1aaed5c0bbd9224e2011165c5515bfa8306c
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 12/01/2017
---
# <a name="manage-domain-joined-hdinsight-clusters"></a>Správa clusterů HDInsight připojený k doméně
Další uživatelé a role v doméně HDInsight a Správa clusterů HDInsight připojený k doméně.

## <a name="users-of-domain-joined-hdinsight-clusters"></a>Uživatelé clustery HDInsight připojený k doméně
Cluster služby HDInsight, který není připojený k doméně má dva uživatelské účty, které jsou vytvořeny při vytvoření clusteru:

* **Správce Ambari**: Tento účet je také označován jako *Hadoop uživatele* nebo *HTTP uživatele*. Tento účet lze použít pro přihlášení k Ambari na https://&lt;clustername >. azurehdinsight.net. Můžete ho také použít ke spouštění dotazů na zobrazení Ambari, spouštění úloh prostřednictvím externích nástrojů (tj. prostředí PowerShell, Templeton, Visual Studio) a ověření pomocí ovladače Hive ODBC a nástrojů BI (tj. aplikace Excel, PowerBI nebo Tableau).
* **Uživatel SSH**: Tento účet je možné pomocí protokolu SSH a spuštěním příkazu "sudo" příkazů. Má kořenový oprávnění pro virtuální počítače s Linuxem.

Cluster HDInsight připojený k doméně má tři nové uživatele kromě správce Ambari a uživatel SSH.

* **Správce škálu**: Tento účet je místní účet správce Apache škálu. Není uživatele domény služby active directory. Tento účet slouží k nastavení zásad a zajistit další uživatelé, správci nebo delegovaní správci (takže tito uživatelé mohou spravovat zásady). Ve výchozím nastavení je uživatelské jméno *správce* a heslo je stejné jako heslo správce Ambari. Na stránce nastavení v škálu lze aktualizovat heslo.
* **Uživatel domény s oprávněními správce clusteru**: Tento účet je určený jako správce clusteru Hadoop, včetně Ambari a škálu uživatele domény služby active directory. Při vytváření clusteru, je třeba zadat pověření uživatele. Tento uživatel má následující oprávnění:

  * Připojení počítače k doméně a umístěte je v rámci organizační jednotky, které zadáte během vytváření clusteru.
  * Vytvořte objekty služby v rámci organizační jednotky, které zadáte během vytváření clusteru.
  * Vytvořte reverzní záznamy DNS.

    Všimněte si, že tito ostatní uživatelé AD také mají tato oprávnění.

    Existují některé koncové body v rámci clusteru (například Templeton), které nejsou spravovány nástrojem škálu a proto nejsou zabezpečené. Tyto koncové body jsou uzamčené pro všechny uživatele kromě domény uživatele Správce clusteru.
* **Regulární**: při vytváření clusteru, můžete zadat více skupin služby active directory. Uživatelé v těchto skupinách se budou synchronizovat s škálu a Ambari. Tito uživatelé jsou uživatelé domény a budou mít přístup jenom spravovaná škálu koncových bodů (například Hiveserver2). Všechny zásady RBAC a auditování bude pro tyto uživatele.

## <a name="roles-of-domain-joined-hdinsight-clusters"></a>Role clusterů HDInsight připojený k doméně
HDInsight připojený k doméně mají následující role:

* Správce clusteru
* Operátor clusteru
* Správce služeb
* Operátor služby
* Uživatel clusteru

**Chcete-li zjistit, jaká oprávnění z těchto rolí**

1. Otevřete uživatelské rozhraní pro správu Ambari.  V tématu [otevřete uživatelské rozhraní pro správu Ambari](#open-the-ambari-management-ui).
2. V levé nabídce klikněte na tlačítko **role**.
3. Klepněte na modré otazník zobrazíte oprávnění:

    ![Připojené k doméně oprávnění role HDInsight](./media/apache-domain-joined-manage/hdinsight-domain-joined-roles-permissions.png)

## <a name="open-the-ambari-management-ui"></a>Otevřete uživatelské rozhraní pro správu Ambari
1. Přihlaste se k portálu [Azure Portal](https://portal.azure.com).
2. Otevřete v okně clusteru HDInsight. V tématu [seznamu a zobrazit clustery](../hdinsight-administer-use-management-portal.md#list-and-show-clusters).
3. Klikněte na tlačítko **řídicí panel** z hlavní nabídky pro otevření Ambari.
4. Přihlaste se k použití clusteru správce domény uživatelské jméno a heslo Ambari.
5. Klikněte **správce** rozevírací nabídce z horní pravým dolním a potom klikněte na **spravovat Ambari**.

    ![Připojené k doméně HDInsight spravovat Ambari](./media/apache-domain-joined-manage/hdinsight-domain-joined-manage-ambari.png)

    Uživatelské rozhraní bude vypadat takto:

    ![Připojené k doméně HDInsight Ambari uživatelské rozhraní pro správu](./media/apache-domain-joined-manage/hdinsight-domain-joined-ambari-management-ui.png)

## <a name="list-the-domain-users-synchronized-from-your-active-directory"></a>Seznam uživatelů domény synchronizované z Active Directory
1. Otevřete uživatelské rozhraní pro správu Ambari.  V tématu [otevřete uživatelské rozhraní pro správu Ambari](#open-the-ambari-management-ui).
2. V levé nabídce klikněte na tlačítko **uživatelé**. Zobrazí se všichni uživatelé, které jsou synchronizované z vaší služby Active Directory do clusteru HDInsight.

    ![Připojené k doméně seznamu uživatelé uživatelské rozhraní HDInsight Ambari správy](./media/apache-domain-joined-manage/hdinsight-domain-joined-ambari-management-ui-users.png)

## <a name="list-the-domain-groups-synchronized-from-your-active-directory"></a>Seznam skupiny v doméně synchronizované z Active Directory
1. Otevřete uživatelské rozhraní pro správu Ambari.  V tématu [otevřete uživatelské rozhraní pro správu Ambari](#open-the-ambari-management-ui).
2. V levé nabídce klikněte na tlačítko **skupiny**. Zobrazí se všechny skupiny, které jsou synchronizované z vaší služby Active Directory do clusteru HDInsight.

    ![Připojené k doméně seznam skupin pro HDInsight Ambari správu uživatelského rozhraní](./media/apache-domain-joined-manage/hdinsight-domain-joined-ambari-management-ui-groups.png)

## <a name="configure-hive-views-permissions"></a>Konfigurace zobrazení Hive oprávnění
1. Otevřete uživatelské rozhraní pro správu Ambari.  V tématu [otevřete uživatelské rozhraní pro správu Ambari](#open-the-ambari-management-ui).
2. V levé nabídce klikněte na tlačítko **zobrazení**.
3. Klikněte na tlačítko **HIVE** zobrazíte podrobnosti.

    ![Připojené k doméně správy HDInsight Ambari Hive zobrazení uživatelského rozhraní](./media/apache-domain-joined-manage/hdinsight-domain-joined-ambari-management-ui-hive-views.png)
4. Klikněte **zobrazení Hive** odkaz pro konfiguraci zobrazení Hive.
5. Přejděte dolů k položce **oprávnění** části.

    ![Konfigurace oprávnění připojený k doméně správy HDInsight Ambari Hive zobrazení uživatelského rozhraní](./media/apache-domain-joined-manage/hdinsight-domain-joined-ambari-management-ui-hive-views-permissions.png)
6. Klikněte na tlačítko **přidat uživatele** nebo **přidat skupinu**a pak zadejte uživatele nebo skupiny, které můžete použít zobrazení Hive.

## <a name="configure-users-for-the-roles"></a>Konfigurovat uživatele pro role
 Seznam rolí a oprávnění k nim najdete v sekci [clustery HDInsight role z doméně](#roles-of-domain---joined-hdinsight-clusters).

1. Otevřete uživatelské rozhraní pro správu Ambari.  V tématu [otevřete uživatelské rozhraní pro správu Ambari](#open-the-ambari-management-ui).
2. V levé nabídce klikněte na tlačítko **role**.
3. Klikněte na tlačítko **přidat uživatele** nebo **přidat skupinu** přiřadit uživatelů a skupin pro různé role.

## <a name="next-steps"></a>Další kroky
* Pokud chcete konfigurovat cluster HDInsight připojený k doméně, přečtěte si téma [Konfigurace clusterů HDInsight připojených k doméně](apache-domain-joined-configure.md).
* Pokud chcete konfigurovat zásady Hivu a spouštět dotazy Hivu, přečtěte si téma [Konfigurace zásad Hivu pro clustery HDInsight připojené k doméně](apache-domain-joined-run-hive.md).
* Spuštění dotazů Hive pomocí protokolu SSH v clusterech HDInsight připojený k doméně, najdete v části [použití SSH s HDInsight](../hdinsight-hadoop-linux-use-ssh-unix.md#domainjoined).
