---
title: "Vytvoření a správě Azure databáze pro server databáze MySQL pomocí portálu Azure | Microsoft Docs"
description: "Tento článek popisuje, jak můžete rychle vytvořit novou databázi MySQL server Azure a spravovat server pomocí portálu Azure."
services: mysql
author: v-chenyh
ms.author: nolanwu
editor: jasonwhowell
manager: jhubbard
ms.service: mysql-database
ms.topic: article
ms.date: 09/15/2017
ms.openlocfilehash: b73fe2214a165d7c02c0a58551d8b84bee39f919
ms.sourcegitcommit: d87b039e13a5f8df1ee9d82a727e6bc04715c341
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 02/21/2018
---
# <a name="create-and-manage-azure-database-for-mysql-server-using-azure-portal"></a>Vytvoření a správě Azure databáze pro server databáze MySQL pomocí portálu Azure
Toto téma popisuje, jak můžete rychle vytvořit novou databázi MySQL serveru Azure. Zahrnuje také informace o tom, jak spravovat server pomocí portálu Azure. Správa serveru zahrnuje zobrazení Podrobnosti o serveru a databází, resetuje se heslo a odstranění serveru.

## <a name="log-in-to-the-azure-portal"></a>Přihlášení k portálu Azure Portal
Přihlaste se k portálu [Azure Portal](https://portal.azure.com).

## <a name="create-an-azure-database-for-mysql-server"></a>Vytvoření serveru Azure Database for MySQL
Postupujte podle těchto kroků k vytvoření Azure databáze MySQL serveru s názvem "mysqlserver4demo."

1. Klikněte **vytvořit prostředek** tlačítko umístěné v levém horním rohu portálu Azure.

2. Na nové stránce vyberte **databáze**a pak na stránce databáze, vyberte **Azure Database pro databázi MySQL**.

    > Azure databáze pro server databáze MySQL byla vytvořena s definovanou sadu [výpočetního prostředí a úložiště](./concepts-compute-unit-and-storage.md) prostředky. Vytvoření databáze v rámci skupiny prostředků Azure a v databázi aplikace Azure pro server databáze MySQL.

   ![create-new-server](./media/howto-create-manage-server-portal/create-new-server.png)

3. Vyplňte Azure databáze MySQL formuláře pomocí následující informace:

    | **Pole formuláře** | **Popis pole** |
    |----------------|-----------------------|
    | *Název serveru* | Azure mysql (název serveru je globálně jedinečný) |
    | *Předplatné* | MySQLaaS (vyberte z rozevírací nabídky) |
    | *Skupina prostředků* | MyResource (vytvořit novou skupinu prostředků nebo použijte existující) |
    | *Přihlašovací jméno správce serveru* | myadmin (název účtu správce instalace) |
    | *Heslo* | Instalační program heslo účtu správce |
    | *Potvrdit heslo* | potvrďte heslo účtu správce |
    | *Umístění* | Severní Evropa (vyberte mezi Severní Evropa a západní USA) |
    | *Verze* | 5.6 (zvolte Azure databáze MySQL server verze) |

4. Klikněte na tlačítko **cenová úroveň** k určení úrovně služby a výkonu pro nový server. Výpočetní jednotka lze nastavit v rozsahu 50 až 100 v základní vrstvě, 100 až 200 ve standardní vrstvě a úložiště lze přidat podle zahrnuté množství. Tato příručka postupy umožňuje zvolte 50 výpočetní jednotka a 50 GB. Klikněte na tlačítko **OK** uložte svůj výběr.

   ![create-server-pricing-tier](./media/howto-create-manage-server-portal/create-server-pricing-tier.png)

5. Klikněte na **Vytvořit**, aby se server zřídil. Zřizování trvá několik minut.

    > Vyberte **připnout na řídicí panel** možnost, povolíte snadné sledování vašich nasazení.
    > [!NOTE]
    > I když v základní vrstvě až 1 000 GB a 10 000 GB ve standardní vrstvě je podporována pro úložiště, pro verzi Public Preview maximální velikost úložiště je dočasně omezena na 1 000 GB.</Include>

## <a name="update-an-azure-database-for-mysql-server"></a>Aktualizovat databázi služby Azure pro server databáze MySQL
Po zřízení nového serveru má uživatel dvě možnosti pro úpravy existujícího serveru: resetovat heslo správce nebo změnou výpočetní jednotky škálování nahoru nebo dolů na server.

### <a name="change-the-administrator-user-password"></a>Změna hesla správce uživatele
1. Na serveru **přehled** okně klikněte na tlačítko **resetovat heslo** k naplnění okno zadání a potvrzení hesla.

2. Zadejte nové heslo a potvrzení hesla do okna, jak je znázorněno:

   ![Resetování hesla](./media/howto-create-manage-server-portal/reset-password.png)

3. Klikněte na tlačítko **OK** uložit nové heslo.

### <a name="scale-updown-by-changing-compute-units"></a>Změnou výpočetní jednotky škálování nahoru/dolů

1. V okně serveru v části **nastavení**, klikněte na tlačítko **cenová úroveň** otevřete okno cenová úroveň pro databázi Azure pro server databáze MySQL.

2. Postupujte podle kroku 4 v **vytvoření databáze Azure pro server databáze MySQL** změnit výpočetní jednotky ve stejné cenovou úroveň.

## <a name="delete-an-azure-database-for-mysql-server"></a>Odstraňte databázi Azure pro server databáze MySQL

1. Na serveru **přehled** okně klikněte **odstranit** příkazového tlačítka, otevřete okno potvrzení odstranění.

2. Zadejte správný název serveru do vstupního pole v okně pro dvojité potvrzení.

3. Klikněte **odstranit** tlačítko znovu pro potvrzení odstraňování akci a potom počkejte "úspěch odstranění" překryvné zobrazí na panelu oznámení.

## <a name="list-the-azure-database-for-mysql-databases"></a>Seznam databáze Azure pro databází MySQL
Na serveru **přehled** okno, posuňte se dolů, dokud neuvidíte databázi dlaždici v dolní části. V tabulce jsou uvedeny všechny databáze. Klikněte **odstranit** příkazového tlačítka, otevřete okno potvrzení odstranění.

   ![Zobrazit databáze](./media/howto-create-manage-server-portal/show-databases.png)

## <a name="show-details-of-an-azure-database-for-mysql-server"></a>Zobrazit podrobnosti databáze Azure pro server databáze MySQL
V okně serveru v části **nastavení**, klikněte na tlačítko **vlastnosti** otevřete **vlastnosti** okna a potom zobrazit všechny podrobné informace o serveru.

## <a name="next-steps"></a>Další postup

[Rychlý úvod: Vytvoření databáze Azure pro server databáze MySQL pomocí portálu Azure](./quickstart-create-mysql-server-database-using-azure-portal.md)