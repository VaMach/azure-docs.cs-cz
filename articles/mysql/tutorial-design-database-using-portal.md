---
title: "Navrhnout první databáze Azure pro databázi MySQL. portálu Azure | Microsoft Docs"
description: "Tento kurz vysvětluje, jak vytvořit a spravovat Azure databáze MySQL server a databáze pomocí portálu Azure."
services: mysql
author: v-chenyh
ms.author: v-chenyh
manager: jhubbard
editor: jasonwhowell
ms.service: mysql
ms.topic: tutorial
ms.date: 06/06/2017
ms.custom: mvc
ms.openlocfilehash: 92ae809646cb956b0b7cb71cf2a21610bb2d610f
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/11/2017
---
# <a name="design-your-first-azure-database-for-mysql-database"></a>Navrhnout první databáze Azure pro databázi MySQL
Azure Database for MySQL je spravovaná služba, která umožňuje spouštět, spravovat a škálovat vysoce dostupné databáze MySQL v cloudu. Pomocí portálu Azure, můžete snadno spravovat váš server a návrhu databáze.

V tomto kurzu pomocí portálu Azure další postup:

> [!div class="checklist"]
> * Vytvoření Azure databáze pro databázi MySQL
> * Konfigurace brány firewall serveru
> * Vytvořit databázi pomocí nástroje příkazového řádku mysql
> * Načíst ukázková data
> * Dotazování dat
> * Aktualizace dat
> * Obnovení dat

## <a name="sign-in-to-the-azure-portal"></a>Přihlášení k webu Azure Portal
Otevřete Oblíbené webový prohlížeč a přejděte [portálu Microsoft Azure](https://portal.azure.com/). Zadejte přihlašovací údaje pro přihlášení k portálu. Výchozím zobrazením je váš řídicí panel služby.

## <a name="create-an-azure-database-for-mysql-server"></a>Vytvoření serveru Azure Database for MySQL
Server Azure Database for MySQL se vytvoří s definovanou sadou [výpočetních prostředků a prostředků úložiště](./concepts-compute-unit-and-storage.md). Server se vytvoří v rámci [skupiny prostředků Azure](https://docs.microsoft.com/en-us/azure/azure-resource-manager/resource-group-overview).

1. Přejděte na **databáze** > **Azure databáze pro databázi MySQL**. Pokud nemůžete najít MySQL serveru v části **databáze** kategorii, klikněte na tlačítko **zobrazit všechny** zobrazíte všechny služby k dispozici databáze. Můžete také zadat **Azure Database pro databázi MySQL** do vyhledávacího pole rychle najít službu.
![2-1, přejděte na MySQL](./media/tutorial-design-database-using-portal/2_1-Navigate-to-MySQL.png)

2. Klikněte na tlačítko **Azure Database pro databázi MySQL** dlaždici a potom klikněte na **vytvořit**.

V našem příkladu vyplňte Azure databáze MySQL formuláře s následujícími informacemi:

| **Nastavení** | **Navrhovaná hodnota** | **Popis pole** |
|---|---|---|
| *Název serveru* | myserver4demo  | Název serveru musí být globálně jedinečný. |
| *Předplatné* | mysubscription | Vyberte vaše předplatné z rozevíracího seznamu. |
| *Skupina prostředků* | myresourcegroup | Vytvořte skupinu prostředků nebo použijte existující. |
| *Přihlašovací jméno správce serveru* | myadmin | Instalační program název účtu správce. |
| *Heslo* |  | Nastavte silné heslo účtu správce. |
| *Potvrdit heslo* |  | Potvrďte heslo účtu správce. |
| *Umístění* |  | Vyberte dostupnou oblast. |
| *Verze* | 5.7 | Zvolte nejnovější verzi. |
| *Konfigurovat výkon* | Základní, 50 výpočetní jednotky, 50 GB  | Zvolte **Cenovou úroveň**, **Výpočetní jednotky**, **Úložiště (GB)** a potom klikněte na **OK**. |
| *Připnout na řídicí panel* | Zaškrtnout | Doporučuje se toto zaškrtávací políčko zaškrtnout, abyste později server snadno našli. |
Poté klikněte na možnost **Vytvořit**. Po jedné až dvou minutách bude server Azure Database for MySQL spuštěný v cloudu. Můžete kliknout na **oznámení** tlačítka na panelu nástrojů ke sledování procesu nasazení.

## <a name="configure-firewall"></a>Konfigurace brány firewall
Azure databáze pro databázi MySQL jsou chráněné bránou firewall. Ve výchozím nastavení všechna připojení k serveru a databází uvnitř serveru odmítají. Před připojením ke službě Azure Database pro databázi MySQL poprvé, nakonfigurujte bránu firewall, přidejte klientský počítač veřejné síti IP adresu (nebo rozsah IP adres).

1. Klikněte na nově vytvořený serveru a pak klikněte na tlačítko **zabezpečení připojení**.
   ![Zabezpečení připojení 3-1](./media/tutorial-design-database-using-portal/3_1-Connection-security.png)
2. Můžete **přidat Moje IP**, nebo můžete nakonfigurovat pravidla brány firewall v tomto poli. Po vytvoření pravidel nezapomeňte kliknout na **Uložit**.
Nyní můžete připojit k serveru pomocí nástroje příkazového řádku mysql nebo MySQL Workbench grafickým uživatelským rozhraním.

> [!TIP]
> Azure databáze MySQL server pro komunikuje přes port 3306. Pokud se pokoušíte připojit z podnikové sítě, nemusí být odchozí provoz přes port 3306 bránou firewall vaší sítě povolený. Pokud ano, můžete nemůže připojit k Azure MySQL serveru Pokud vaše IT oddělení otevře port 3306.

## <a name="get-connection-information"></a>Získání informací o připojení
Získat plně kvalifikovaný **název serveru** a **přihlašovací jméno správce pro Server** pro vaši databázi Azure pro server databáze MySQL na portálu Azure. Použijete název plně kvalifikovaný serveru k připojení k serveru pomocí nástroje příkazového řádku mysql. 

1. V [portál Azure](https://portal.azure.com/), klikněte na tlačítko **všechny prostředky** z nabídky na levé straně, zadejte název a vyhledávání pro vaši databázi Azure pro server databáze MySQL. Vyberte název serveru zobrazíte podrobnosti.

2. V části nastavení klikněte na **vlastnosti**. Poznamenejte si **název serveru** a **PŘIHLAŠOVACÍ jméno pro SERVER správce**. Může klikněte na tlačítko Kopírovat vedle každého pole zkopírovat do schránky.
   ![Vlastnosti serveru 4-2](./media/tutorial-design-database-using-portal/4_2-server-properties.png)

V tomto příkladu je název serveru *myserver4demo.mysql.database.azure.com* a přihlašovací jméno správce serveru je *myadmin@myserver4demo*.

## <a name="connect-to-the-server-using-mysql"></a>Připojit k serveru pomocí mysql
Použijte [nástroj pro příkazový řádek mysql](https://dev.mysql.com/doc/refman/5.7/en/mysql.html) k navázání připojení k serveru Azure Database for MySQL. Nástroj příkazového řádku mysql můžete spustit z prostředí cloudové služby Azure v prohlížeči nebo vlastní počítače pomocí nástroje mysql nainstalované místně. Chcete-li spustit prostředí cloudu Azure, klikněte na tlačítko `Try It` tlačítko blok kódu v tomto článku, nebo navštívit portál Azure a klikněte na tlačítko `>_` ikonu v pravém horním panelu nástrojů. 

Zadejte příkaz pro připojení:
```azurecli-interactive
mysql -h myserver4demo.mysql.database.azure.com -u myadmin@myserver4demo -p
```

## <a name="create-a-blank-database"></a>Vytvoření prázdné databáze
Po připojení k serveru vytvořte prázdnou databázi pro práci s.
```sql
CREATE DATABASE mysampledb;
```

Do příkazového řádku spusťte následující příkaz k přepínači připojení k této nově vytvořené databáze:
```sql
USE mysampledb;
```

## <a name="create-tables-in-the-database"></a>Vytváření tabulek v databázi
Teď, když víte, jak se připojit k databázi Azure pro databázi MySQL, jsme projít jak provést některé základní úlohy.

Jsme nejprve vytvořit tabulku a načíst určitými daty. Umožňuje vytvořit tabulku, která ukládá informace o inventáři.
```sql
CREATE TABLE inventory (
    id serial PRIMARY KEY, 
    name VARCHAR(50), 
    quantity INTEGER
);
```

## <a name="load-data-into-the-tables"></a>Načtení dat do tabulky
Teď, když máme tabulku, jsme do něj vložte některá data. V okně Otevřít příkazového řádku spusťte následující dotaz vložit některé řádky dat.
```sql
INSERT INTO inventory (id, name, quantity) VALUES (1, 'banana', 150); 
INSERT INTO inventory (id, name, quantity) VALUES (2, 'orange', 154);
```

Nyní máte dva řádky ukázkových dat do tabulky, které jste vytvořili dříve.

## <a name="query-and-update-the-data-in-the-tables"></a>Dotaz a aktualizovat data v tabulkách
Spustíte následující dotaz pro načtení informací z tabulky databáze.
```sql
SELECT * FROM inventory;
```

Můžete také aktualizovat data v tabulkách.
```sql
UPDATE inventory SET quantity = 200 WHERE name = 'banana';
```

Načtení dat získá příslušným způsobem aktualizuje řádek.
```sql
SELECT * FROM inventory;
```

## <a name="restore-a-database-to-a-previous-point-in-time"></a>Obnovení databáze k dřívějšímu bodu v čase
Představte si, že omylem odstraněn k tabulce důležité databáze a nelze snadno obnovit data. Databáze pro databázi MySQL Azure umožňuje obnovit server k určitému bodu v čase, vytváření kopie databáze do nového serveru. Tento nový server můžete obnovit odstraněná data. Ukázka serveru bod před přidáním tabulky obnovit následující kroky.

1. Na portálu Azure vyhledejte Azure Database pro databázi MySQL. Na **přehled** klikněte na tlačítko **obnovení** na panelu nástrojů. Otevře se stránka obnovení.

   ![10-1 obnovení databáze](./media/tutorial-design-database-using-portal/10_1-restore-a-db.png)

2. Vyplňte **obnovení** formuláře se požadované informace.
   
   ![Formulář obnovení 10-2](./media/tutorial-design-database-using-portal/10_2-restore-form.png)
   
   - **Bod obnovení**: Vyberte bodu v čase, kterou chcete obnovit, v časovém rámci uvedené. Zajistěte, aby převést vaše místní časové pásmo UTC.
   - **Obnovit na nový server**: Zadejte nový název serveru, kterou chcete obnovit.
   - **Umístění**: oblast je stejný jako zdrojový server a nelze ji změnit.
   - **Cenová úroveň**: cenová úroveň je stejný jako zdrojový server a nedá se změnit.
   
3. Klikněte na tlačítko **OK** a obnovte server do [obnovit k určitému bodu v čase](./howto-restore-server-portal.md) předtím, než tabulka byla odstraněna. Obnovení serveru vytvoří novou kopii serveru od bodu v čase, který zadáte. 

## <a name="next-steps"></a>Další kroky
V tomto kurzu použijete Azure portálu na zjištěné postup:

> [!div class="checklist"]
> * Vytvoření Azure databáze pro databázi MySQL
> * Konfigurace brány firewall serveru
> * Vytvořit databázi pomocí nástroje příkazového řádku mysql
> * Načíst ukázková data
> * Dotazování dat
> * Aktualizace dat
> * Obnovení dat

> [!div class="nextstepaction"]
> [Postup připojení aplikace k databázi Azure pro databázi MySQL](./howto-connection-string.md)
