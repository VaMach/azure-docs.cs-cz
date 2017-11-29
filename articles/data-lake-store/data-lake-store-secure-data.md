---
title: "Zabezpečení dat uložených v Azure Data Lake Store | Microsoft Docs"
description: "Zjistěte, jak k zabezpečení dat v Azure Data Lake Store pomocí skupin a seznamy řízení přístupu"
services: data-lake-store
documentationcenter: 
author: nitinme
manager: jhubbard
editor: cgronlun
ms.assetid: ca35e65f-3986-4f1b-bf93-9af6066bb716
ms.service: data-lake-store
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 11/28/2017
ms.author: nitinme
ms.openlocfilehash: e246abaf8d1d7939765e1c878f6dfaf6375294f6
ms.sourcegitcommit: 651a6fa44431814a42407ef0df49ca0159db5b02
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/28/2017
---
# <a name="securing-data-stored-in-azure-data-lake-store"></a>Zabezpečení dat uložených v Azure Data Lake Store
Zabezpečení dat v Azure Data Lake Store je přístup třech krocích.

1. Začněte vytvořením skupiny zabezpečení v Azure Active Directory (AAD). Tyto skupiny zabezpečení slouží k implementaci řízení přístupu na základě role (RBAC) na portálu Azure. Další informace najdete v tématu [řízení přístupu na základě rolí ve službě Microsoft Azure](../active-directory/role-based-access-control-configure.md).
2. Skupiny zabezpečení AAD přiřadíte k účtu Azure Data Lake Store. To řídí přístup k účtu Data Lake Store z portál a management operace z portálu nebo rozhraní API.
3. Přiřazení skupiny zabezpečení AAD jako přístup seznamy řízení (ACL) v systému souborů Data Lake Store.
4. Kromě toho můžete také nastavit rozsah IP adres pro klienty, kteří měli přístup k datům v Data Lake Store.

Tento článek obsahuje pokyny o tom, jak pomocí portálu Azure k provádění úloh výše. Podrobnější informace o tom, jak Data Lake Store implementuje zabezpečení na úrovni účtu a data, najdete v části [zabezpečení v Azure Data Lake Store](data-lake-store-security-overview.md). Nabídnout detailní informace o tom, jak jsou implementované seznamy ACL v Azure Data Lake Store najdete v tématu [přehled o řízení přístupu v Data Lake Store](data-lake-store-access-control.md).

## <a name="prerequisites"></a>Požadavky
Je nutné, abyste před zahájením tohoto kurzu měli tyto položky:

* **Předplatné Azure**. Viz [Získání bezplatné zkušební verze Azure](https://azure.microsoft.com/pricing/free-trial/).
* **Účet Azure Data Lake Store**. Pokyny o tom, jak vytvořit najdete v tématu [Začínáme s Azure Data Lake Store](data-lake-store-get-started-portal.md)

## <a name="create-security-groups-in-azure-active-directory"></a>Vytvoření skupin zabezpečení v Azure Active Directory
Pokyny k vytváření skupin zabezpečení AAD a jak přidat uživatele do skupiny, najdete v části [Správa skupin zabezpečení v Azure Active Directory](../active-directory/active-directory-groups-create-azure-portal.md).

> [!NOTE] 
> Uživatelé a další skupiny můžete přidat do skupiny ve službě Azure AD pomocí portálu Azure. Ale chcete-li přidat hlavní název služby do skupiny, použijte [modulu Azure AD PowerShell](../active-directory/active-directory-accessmanagement-groups-settings-v2-cmdlets.md).
> 
> ```powershell
> # Get the desired group and service principal and identify the correct object IDs
> Get-AzureADGroup -SearchString "<group name>"
> Get-AzureADServicePrincipal -SearchString "<SPI name>"
> 
> # Add the service principal to the group
> Add-AzureADGroupMember -ObjectId <Group object ID> -RefObjectId <SPI object ID>
> ```
 
## <a name="assign-users-or-security-groups-to-azure-data-lake-store-accounts"></a>Přiřadit uživatele nebo skupiny zabezpečení účtů Azure Data Lake Store
Když přiřadíte uživatele nebo skupiny zabezpečení účtů Azure Data Lake Store, můžete řídit přístup ke operace správy v účtu pomocí portálu Azure a rozhraní API Správce Azure Resource Manager. 

1. Otevřete účet Azure Data Lake Store. V levém podokně klikněte na tlačítko **Procházet**, klikněte na tlačítko **Data Lake Store**a potom v okně Data Lake Store klikněte na název účtu, ke kterému chcete přiřadit uživatele nebo novou skupinu zabezpečení.

2. V okně Nastavení účtu Data Lake Store, klikněte na tlačítko **řízení přístupu (IAM)**. V okně výchozí seznamy **správci předplatného** skupiny jako vlastníka.
   
    ![Přiřazení skupiny zabezpečení k účtu Azure Data Lake Store](./media/data-lake-store-secure-data/adl.select.user.icon.png "skupinu zabezpečení přiřadit k účtu Azure Data Lake Store")

    Existují dva způsoby, jak přidat skupinu a přiřaďte příslušných rolí.
   
    * Přidat skupinu uživatelů nebo k účtu a pak mu přiřaďte roli, nebo
    * Přidejte roli a potom přiřadit role uživatele nebo skupiny.
     
    V této části se podíváme na první postup přidání skupiny a pak přiřazení rolí. Můžete provést podobným způsobem nejprve vyberte roli a potom přiřadit skupiny do této role.
4. V **uživatelé** okně klikněte na tlačítko **přidat** otevřete **přidat přístup** okno. V **přidat přístup** okně klikněte na tlačítko **vyberte roli**a potom vyberte roli pro uživatele nebo skupinu.
   
    ![Přidání role pro uživatele](./media/data-lake-store-secure-data/adl.add.user.1.png "přidání role pro uživatele")
   
    **Vlastníka** a **Přispěvatel** role umožňují přístup k různým funkcím pro správu v účtu data lake. Pro uživatele, kteří budou pracovat s daty v úložišti data lake, můžete přidat je do ** čtečky ** role. Rozsah těchto rolí je omezený na operace správy vztahující se k účtu Azure Data Lake Store.
   
    Pro data oprávnění systému operations jednotlivých souborů definovat, co můžete dělat uživatele. Proto uživatele s role Čtenář můžete zobrazit jenom nastavení pro správu, které jsou přidružené k účtu, ale můžete potenciálně čtení a zápisu dat podle přiřazeného oprávnění systému souborů. Oprávnění systému souborů data Lake Store jsou popsány v [skupinu zabezpečení přiřadit jako seznamy ACL pro systém souborů Azure Data Lake Store](#filepermissions).
5. V **přidat přístup** okně klikněte na tlačítko **přidat uživatele** otevřete **přidat uživatele** okno. V tomto okně vyhledejte skupinu zabezpečení, kterou jste vytvořili dříve v Azure Active Directory. Pokud máte spoustu skupin vyhledávat od, použijte k filtrování název skupiny v horní části textového pole. Klikněte na **Vybrat**.
   
    ![Přidat skupinu zabezpečení](./media/data-lake-store-secure-data/adl.add.user.2.png "přidat skupinu zabezpečení")
   
    Pokud chcete přidat skupinu nebo uživatele, který není uveden, můžete je pozvat pomocí **pozvat** ikonu a zadání e-mailovou adresu pro uživatele nebo skupinu.
6. Klikněte na **OK**. Měli byste vidět skupiny zabezpečení přidat, jak je uvedeno níže.
   
    ![Skupiny zabezpečení přidat](./media/data-lake-store-secure-data/adl.add.user.3.png "přidat skupinu zabezpečení")

7. Skupině zabezpečení nebo uživatelů nyní má přístup k účtu Azure Data Lake Store. Pokud chcete k poskytování přístupu konkrétním uživatelům, můžete je přidat do skupiny zabezpečení. Podobně pokud chcete odvolat přístup pro uživatele, můžete je odebrat ze skupiny zabezpečení. Víc skupin zabezpečení můžete také přiřadit k účtu. 

## <a name="filepermissions"></a>Přiřadit uživatele nebo skupiny zabezpečení jako seznamy řízení přístupu k systému souborů Azure Data Lake Store
Přiřazením zabezpečení uživatelů nebo skupin na systém souborů Azure Data Lake nastavit řízení přístupu na data uložená v Azure Data Lake Store.

1. V okně účtu Data Lake Store klikněte na možnost **Průzkumník dat**.
   
    ![Vytváření adresářů v účtu Data Lake Store](./media/data-lake-store-secure-data/adl.start.data.explorer.png "vytváření adresářů v účtu Data Lake")
2. V **Průzkumníku dat** okně klikněte na soubor nebo složku, pro který chcete konfigurovat seznam ACL a potom klikněte na **přístup**. Do souboru přiřadit seznamu ACL, musíte kliknout na **přístup** z **náhled souboru** okno.
   
    ![Nastavit seznamy ACL v systému souborů Data Lake](./media/data-lake-store-secure-data/adl.acl.1.png "nastavit seznamy ACL v Data Lake systému souborů")
3. **Přístup** okno obsahuje standardní přístup a vlastní přístup již přiřazen do kořenového adresáře. Klikněte **přidat** ikonu, čímž přidáte vlastní úroveň seznamy ACL.
   
    ![Standardní a vlastní přístup](./media/data-lake-store-secure-data/adl.acl.2.png "standardní a vlastní přístup")
   
   * **Standardní přístup** je stylu systému UNIX přístup, kde zadáte pro čtení, zápisu, provést (rwx) třídy tři odlišné uživatelů: vlastník, skupiny a dalších.
   * **Vlastní přístup** odpovídá POSIX seznamy ACL, která umožňuje nastavit oprávnění pro konkrétní jmenovaní uživatelé nebo skupiny a jenom vlastníka souboru nebo skupiny. 
     
     Další informace najdete v tématu [seznamy ACL HDFS](https://hadoop.apache.org/docs/current/hadoop-project-dist/hadoop-hdfs/HdfsPermissionsGuide.html#ACLs_Access_Control_Lists). Další informace o tom, jak jsou implementované seznamy ACL v Data Lake Store najdete v tématu [řízení přístupu v Data Lake Store](data-lake-store-access-control.md).
4. Klikněte na tlačítko **přidat** ikonu Otevřít **přidat vlastní přístup** okno. V tomto okně klikněte na tlačítko **vybrat uživatele nebo skupinu**a potom v **vybrat uživatele nebo skupinu** okno, podívejte se na skupinu zabezpečení, kterou jste vytvořili dříve v Azure Active Directory. Pokud máte spoustu skupin vyhledávat od, použijte k filtrování název skupiny v horní části textového pole. Klikněte na skupinu, kterou chcete přidat a pak klikněte na **vyberte**.
   
    ![Přidat skupinu](./media/data-lake-store-secure-data/adl.acl.3.png "přidat skupinu")
5. Klikněte na tlačítko **vyberte oprávnění**, vybrat oprávnění a zda chcete přiřadit oprávnění jako výchozí seznam ACL, přístup k seznamu ACL, nebo obojí. Klikněte na **OK**.
   
    ![Přiřazení oprávnění k seskupení](./media/data-lake-store-secure-data/adl.acl.4.png "přiřadit oprávnění k seskupení")
   
    Další informace o oprávněních v Data Lake Store a seznamy ACL výchozí nebo přístupu najdete v tématu [řízení přístupu v Data Lake Store](data-lake-store-access-control.md).
6. V **přidat vlastní přístup** okně klikněte na tlačítko **OK**. Nově přidané skupiny s příslušnými oprávněními, se teď uvedený v **přístup** okno.
   
    ![Přiřazení oprávnění k seskupení](./media/data-lake-store-secure-data/adl.acl.5.png "přiřadit oprávnění k seskupení")
   
   > [!IMPORTANT]
   > V aktuální verzi, můžete mít pouze 9 položky v rámci **vlastní přístup**. Pokud chcete přidat více než 9 uživatele, měli byste vytvořit skupiny zabezpečení přidat uživatele do skupiny zabezpečení, přidejte poskytovat přístup do těchto skupin zabezpečení pro účet Data Lake Store.
   > 
   > 
7. V případě potřeby můžete také upravit přístupová oprávnění, po přidání skupiny. Zaškrtněte políčko pro každý typ oprávnění (pro čtení, zápisu, spouštění) založené na tom, zda chcete odebrat nebo přiřadit tato oprávnění ke skupině zabezpečení nebo zrušte. Klikněte na tlačítko **Uložit** a uložte změny, nebo **zahodit** vrátit zpět změny.

## <a name="set-ip-address-range-for-data-access"></a>Nastavit rozsah IP adres pro přístup k datům
Azure Data Lake Store umožňuje další uzamčení přístup k úložišti dat na úrovni sítě. Můžete povolit bránu firewall, zadejte IP adresu nebo zadejte rozsah IP adres pro klienty důvěryhodné. Po povolení pouze klienti, kteří mají IP adresy v definovaném rozsahu může připojit k úložišti.

![Nastavení brány firewall a IP přístup](./media/data-lake-store-secure-data/firewall-ip-access.png "nastavení a IP adresu brány Firewall")

## <a name="remove-security-groups-for-an-azure-data-lake-store-account"></a>Odebrání skupiny zabezpečení k účtu Azure Data Lake Store
Když odeberete skupin zabezpečení z účtů Azure Data Lake Store, jsou pouze změna přístup na operace správy v účtu, pomocí webu Azure Portal a rozhraní API Správce Azure Resource Manager.

1. V okně účtu Data Lake Store, klikněte na tlačítko **nastavení**. Z **nastavení** okně klikněte na tlačítko **uživatelé**.
   
    ![Přiřazení skupiny zabezpečení k účtu Azure Data Lake](./media/data-lake-store-secure-data/adl.select.user.icon.png "skupinu zabezpečení přiřadit k účtu Azure Data Lake")
2. V **uživatelé** okně klikněte na skupinu zabezpečení, kterou chcete odebrat.
   
    ![Odebrat skupinu zabezpečení](./media/data-lake-store-secure-data/adl.add.user.3.png "odebrat skupinu zabezpečení")
3. V okně skupiny zabezpečení, klikněte na tlačítko **odebrat**.
   
    ![Odebrat skupinu zabezpečení](./media/data-lake-store-secure-data/adl.remove.group.png "odebrat skupinu zabezpečení")

## <a name="remove-security-group-acls-from-azure-data-lake-store-file-system"></a>Odebrat skupinu zabezpečení seznamy ACL systému souborů Azure Data Lake Store
Když odeberete skupiny zabezpečení seznamy ACL systému souborů Azure Data Lake Store, změníte přístup k datům v Data Lake Store.

1. V okně účtu Data Lake Store klikněte na možnost **Průzkumník dat**.
   
    ![Vytváření adresářů v účtu Data Lake](./media/data-lake-store-secure-data/adl.start.data.explorer.png "vytváření adresářů v účtu Data Lake")
2. V **Průzkumníku dat** okně klikněte na soubor nebo složku, pro který chcete odebrat seznamu ACL a potom v okně účtu, klikněte **přístup** ikonu. Odebrat seznamu ACL pro soubor, musíte kliknout na **přístup** z **náhled souboru** okno.
   
    ![Nastavit seznamy ACL v systému souborů Data Lake](./media/data-lake-store-secure-data/adl.acl.1.png "nastavit seznamy ACL v Data Lake systému souborů")
3. V **přístup** okně z **vlastní přístup** části, klikněte na skupinu zabezpečení, kterou chcete odebrat. V **vlastní přístup** okně klikněte na tlačítko **odebrat** a pak klikněte na **OK**.
   
    ![Přiřazení oprávnění k seskupení](./media/data-lake-store-secure-data/adl.remove.acl.png "přiřadit oprávnění k seskupení")

## <a name="see-also"></a>Viz také
* [Přehled Azure Data Lake Store](data-lake-store-overview.md)
* [Kopírování dat z úložiště objektů BLOB Azure do Data Lake Store](data-lake-store-copy-data-azure-storage-blob.md)
* [Použití Azure Data Lake Analytics se službou Data Lake Store](../data-lake-analytics/data-lake-analytics-get-started-portal.md)
* [Použití Azure HDInsight se službou Data Lake Store](data-lake-store-hdinsight-hadoop-use-portal.md)
* [Začínáme s Data Lake Store pomocí prostředí PowerShell](data-lake-store-get-started-powershell.md)
* [Začínáme s Data Lake Store pomocí sady .NET SDK](data-lake-store-get-started-net-sdk.md)
* [Zobrazení protokolů diagnostiky pro Data Lake Store](data-lake-store-diagnostic-logs.md)

