---
title: "Správa oprávnění uživatele na úrovni souborů a složek - Azure HDInsight | Microsoft Docs"
description: "Jak spravovat souborům a složkám pro clusterů HDInsight připojený k doméně."
services: hdinsight
documentationcenter: 
tags: azure-portal
author: maxluk
manager: jhubbard
editor: cgronlun
ms.assetid: 
ms.service: hdinsight
ms.custom: hdinsightactive
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/26/2017
ms.author: maxluk
ms.openlocfilehash: 9ca91721e691eca239478c4ac8b85e2652babdfd
ms.sourcegitcommit: f8437edf5de144b40aed00af5c52a20e35d10ba1
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/03/2017
---
# <a name="manage-user-permissions-at-the-file-and-folder-levels"></a>Správa oprávnění uživatele na úrovni souborů a složek

[Clustery HDInsight připojený k doméně](./domain-joined/apache-domain-joined-introduction.md) použití silné ověřování s uživateli Azure Active Directory (Azure AD) a také *řízení přístupu na základě role* (RBAC) zásady pro různé služby, jako je například YARN a Hive. Pokud výchozí úložiště dat pro váš cluster je úložiště Azure, nebo služby Windows Azure úložiště objektů BLOB (WASB), může také vynucovat souboru a oprávnění na úrovni složky. Apache škálu můžete použít k řízení přístupu k souborům clusteru pro vaše synchronizované Azure AD Uživatelé a skupiny.
<!-- [synchronized Azure AD users and groups](hdinsight-sync-aad-users-to-cluster.md). -->

Apache škálu instance pro clustery služby HDInsight připojený k doméně je vybavená předem nakonfigurovaným službou škálu WASB. Služba škálu WASB je modul správy zásad, který je podobný škálu-HDFS, ale s jinou vynucení zásad přístupu pro škálu. Ve službě škálu WASB Pokud příchozí požadavek na prostředek nemá odpovídající zásadu škálu výchozí odpověď je zakázat. Službu škálu nepředává kontrole WASB oprávnění.

## <a name="permission-and-policy-model"></a>Model oprávnění a zásad

Požadavky na přístup prostředků vyhodnocují se pomocí následující postup:

![Apache škálu zásad vyhodnocení toku](./media/hdinsight-add-acls-at-file-folder-levels/ranger-policy-evaluation-flow.png)

ODEPŘÍT pravidla se vyhodnocují jako první, a potom povolit pravidla. Na konci odpovídající Pokud se splní žádné zásady, odepření je vrácena.

### <a name="user-variable"></a>Uživatelské proměnné

Můžete použít `{USER}` proměnná při přiřazování zásady pro uživatele pro přístup `/{username}` podadresáři, například:

```
resource: path=/app-logs/{USER}, user: {USER}, recursive=true, permissions: all, delegateAdmin=true
```

Výše uvedené zásady udělí uživatelům přístup k vlastní podsložky pod `/app-logs/` adresáře. Zde je, jak tyto zásady vypadá v uživatelském rozhraní škálu:

![Příklad použití uživatelské proměnné](./media/hdinsight-add-acls-at-file-folder-levels/user-variable.png)

### <a name="policy-model-examples"></a>Příklady zásad modelu

Následující tabulka uvádí několik příkladů, jak funguje modelu zásad:

| Škálu zásad | Existující systém souborů | Požadavek uživatele | výsledek |
| -- | -- | -- | -- |
| procedury/Data Regression/finance /, bob zápisu | procedury/Data Regression | Bob, vytvořte soubor /data/finance/mydatafile.txt | Povolit – zprostředkující složku, kterou 'financemi' je vytvořit, protože nadřazený kontrola |
| procedury/Data Regression/finance /, bob zápisu | procedury/Data Regression | Alice, vytvořte soubor /data/finance/mydatafile.txt | ODEPŘÍT - žádné odpovídající zásady |
| / data/finanční *, bob zápisu | procedury/Data Regression | Bob, vytvořte soubor /data/finance/mydatafile.txt | Povolit – v tomto případě volitelné rekurzivní zásady (`*`) je k dispozici, zjistit [zástupné znaky](#wildcards) |
| /data/Finance/mydatafile.txt bob zápisu | procedury/Data Regression | Bob, vytvořte soubor /data/finance/mydatafile.txt | ODEPŘÍT - předchůdce kontroly na ' nebo dat se nezdaří, protože nejsou žádné zásady |
| /data/Finance/mydatafile.txt bob zápisu | / data/finance | Bob, vytvořte soubor /data/finance/mydatafile.txt | ODEPŘÍT – žádné zásady pro předchůdce kontrolu na "/ data/finanční. |

Jsou zapotřebí oprávnění na úrovni složky nebo na úrovni souborů, v závislosti na typu operací. Například "pro čtení nebo otevřené" volání vyžaduje přístup pro čtení na úrovni souborů při volání "vytvořit" vyžaduje oprávnění na úrovni nadřazené složky.

### <a name="wildcards-"></a>Zástupné znaky (*)

Když zástupný znak (`*`) se nachází v cestě pro zásadu zástupného se vztahují na této cestě a jeho celý podstrom. Tato rekurze je stejná jako `recurse-flag`. V škálu-WASB zástupného označuje rekurze a odpovídající část jména.

## <a name="manage-file-and-folder-level-permissions-with-apache-ranger"></a>Správa souborů a oprávnění na úrovni složky s Apache škálu

Pokud jste tak již neučinili, postupujte podle [tyto pokyny](./domain-joined/apache-domain-joined-configure.md) ke zřízení nového clusteru, který je připojený k doméně.

Otevřete škálu WASB procházením `https://<YOUR CLUSTER NAME>.azurehdinsight.net/ranger/`. Zadejte uživatelské jméno správce clusteru a heslo, které jste definovali při vytváření clusteru.

Po přihlášení, najdete v části řídicího panelu škálu:

![Řídicí panel škálu](./media/hdinsight-add-acls-at-file-folder-levels/ranger-dashboard.png)

Chcete-li zobrazit aktuální souborům a složkám pro váš cluster přidruženému účtu úložiště Azure, klikněte na tlačítko  ***CLUSTERNAME*_wasb** odkaz v poli WASB ovládacího prvku.

![Řídicí panel škálu](./media/hdinsight-add-acls-at-file-folder-levels/wasb-dashboard-link.png)

Zobrazí se aktuální seznam zásad. Několik typické zásad jsou zahrnuty jako výchozí bod - zkontrolujte podrobnosti o každé zásady zobrazíte příklad použití.

Pro každou zásadu uvidíte, jestli je zásada povolená, jestli je nakonfigurovaná protokolování auditu a všechny přiřazené skupin a uživatelů. Existují dvě akce tlačítka pro každou zásadu: upravovat a odstraňovat.

![Seznam zásad](./media/hdinsight-add-acls-at-file-folder-levels/policy-list.png)

### <a name="adding-a-new-policy"></a>Přidání nové zásady

1. V horní pravé části stránky zásady WASB vyberte **přidat nové zásady**.

    ![Přidat nové zásady](./media/hdinsight-add-acls-at-file-folder-levels/add-new.png)

2. Zadejte popisný **název zásady**. Zadejte Azure **účet úložiště** pro váš cluster (*ACCOUNT_NAME*. blob.core.windows.net) a **kontejneru účtu úložiště** zadat, když jste vytvořili vaší cluster. Zadejte **relativní cesta** (relativní vůči cluster) pro přístup ke složce nebo souboru.

    ![Formulář nové zásady](./media/hdinsight-add-acls-at-file-folder-levels/new-policy.png)

3. Pod formulářem, zadejte vaše **povolit podmínky** pro tento nový prostředek. Vyberte příslušné skupiny a uživatelé a nastavte jejich oprávnění. V následujícím příkladu, bychom se všichni uživatelé v povolovat `sales` skupiny tak, aby měl přístup pro čtení a zápis.

    ![Povolit prodeje](./media/hdinsight-add-acls-at-file-folder-levels/allow-sales.png)

4. Vyberte **Uložit**.

### <a name="example-policy-conditions"></a>Příklad podmínky zásad

Apache škálu [toku vyhodnocení zásad](#permission-and-policy-model) umožňuje zadat libovolnou kombinaci povolit a zakázat podmínky podle svých potřeb. Tady je pár příkladů:

1. Povolit všem uživatelům prodeje, ale žádné učně:

    ![Povolit prodej, odepřít učně](./media/hdinsight-add-acls-at-file-folder-levels/allow-sales-deny-interns.png)

2. Povolit všem uživatelům prodeje a Odepřít všechny učně, s výjimkou učně s názvem "hiveuser3", který by měl mít přístup pro čtení:

    ![Povolit prodej, odepřít učně s výjimkou hiveuser3](./media/hdinsight-add-acls-at-file-folder-levels/allow-sales-deny-interns-except-hiveuser3.png)

## <a name="next-steps"></a>Další kroky

* [Nakonfigurovat zásady Hive v HDInsight připojený k doméně](./domain-joined/apache-domain-joined-run-hive.md)
* [Správa clusterů HDInsight připojený k doméně](./domain-joined/apache-domain-joined-manage.md)
* [Správa Ambari – autorizace uživatelům Ambari](hdinsight-authorize-users-to-ambari.md)

<!-- * [Synchronize Azure AD users and groups](hdinsight-sync-aad-users-to-cluster.md) -->

