---
title: "Návrh první databáze Azure Database for MySQL – Azure Portal | Microsoft Docs"
description: "Tento kurz vysvětluje, jak vytvořit a spravovat databázi a server Azure Database for MySQL pomocí webu Azure Portal."
services: mysql
author: v-chenyh
ms.author: v-chenyh
manager: jhubbard
editor: jasonwhowell
ms.service: mysql-database
ms.topic: tutorial
ms.date: 11/03/2017
ms.custom: mvc
ms.openlocfilehash: 76cccf9e2ce0a1e59b43646c43ac165d46dade4a
ms.sourcegitcommit: 9d317dabf4a5cca13308c50a10349af0e72e1b7e
ms.translationtype: HT
ms.contentlocale: cs-CZ
ms.lasthandoff: 02/01/2018
---
# <a name="design-your-first-azure-database-for-mysql-database"></a>Návrh první databáze Azure Database for MySQL
Azure Database for MySQL je spravovaná služba, která umožňuje spouštět, spravovat a škálovat vysoce dostupné databáze MySQL v cloudu. Azure Portal umožňuje snadnou správu vašeho serveru a návrh databáze.

V tomto kurzu se naučíte, jak pomocí webu Azure Portal provést následující kroky:

> [!div class="checklist"]
> * Vytvoření Azure Database for MySQL
> * Konfigurace brány firewall serveru
> * Vytvoření databáze pomocí nástroje pro příkazový řádek mysql
> * Načtení ukázkových dat
> * Dotazování dat
> * Aktualizace dat
> * Obnovení dat

## <a name="sign-in-to-the-azure-portal"></a>Přihlášení k webu Azure Portal
Otevřete svůj oblíbený webový prohlížeč a přejděte na [webu Microsoft Azure Portal](https://portal.azure.com/). Zadejte přihlašovací údaje pro přihlášení k portálu. Výchozím zobrazením je váš řídicí panel služby.

## <a name="create-an-azure-database-for-mysql-server"></a>Vytvoření serveru Azure Database for MySQL
Server Azure Database for MySQL se vytvoří s definovanou sadou [výpočetních prostředků a prostředků úložiště](./concepts-compute-unit-and-storage.md). Server se vytvoří v rámci [skupiny prostředků Azure](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-overview).

1. Přejděte na **Databáze** > **Azure Database for MySQL**. Pokud v kategorii **Databáze** nemůžete najít Server MySQL, klikněte na **Zobrazit vše**, aby se zobrazily všechny dostupné databázové služby. Službu také můžete rychle vyhledat tak, že do vyhledávacího pole zadáte **Azure Database for MySQL**.
![2-1 Přechod na MySQL](./media/tutorial-design-database-using-portal/2_1-Navigate-to-MySQL.png)

2. Klikněte na dlaždici **Azure Database for MySQL** a potom na **Vytvořit**.

V tomto příkladu zadejte do formuláře Azure Database for MySQL následující informace:

| **Nastavení** | **Navrhovaná hodnota** | **Popis pole** |
|---|---|---|
| *Název serveru* | myserver4demo  | Název serveru musí být globálně jedinečný. |
| *Předplatné* | mysubscription | Vyberte vaše předplatné z rozevíracího seznamu. |
| *Skupina prostředků* | myresourcegroup | Vytvořte skupinu prostředků nebo použijte existující. |
| *Přihlašovací jméno správce serveru* | myadmin | Název účtu správce instalace. |
| *Heslo* |  | Nastavte silné heslo účtu správce. |
| *Potvrdit heslo* |  | Potvrďte heslo účtu správce. |
| *Umístění* |  | Vyberte dostupnou oblast. |
| *Verze* | 5.7 | Zvolte nejnovější verzi. |
| *Konfigurovat výkon* | Basic, 50 výpočetních jednotek, 50 GB  | Zvolte **Cenovou úroveň**, **Výpočetní jednotky**, **Úložiště (GB)** a potom klikněte na **OK**. |
| *Připnout na řídicí panel* | Zaškrtnout | Doporučuje se toto zaškrtávací políčko zaškrtnout, abyste později server snadno našli. |
Poté klikněte na možnost **Vytvořit**. Po jedné až dvou minutách bude server Azure Database for MySQL spuštěný v cloudu. Pokud chcete monitorovat proces nasazení, klikněte na tlačítko **Oznámení** na panelu nástrojů.

## <a name="configure-firewall"></a>Konfigurace brány firewall
Databáze Azure Database for MySQL jsou chráněné bránou firewall. Ve výchozím nastavení se všechna připojení k serveru a databází uvnitř serveru odmítají. Než se poprvé připojíte k Azure Database for MySQL, nakonfigurujte bránu firewall tak, že do ní přidáte IP adresu veřejné sítě klienta (nebo rozsah IP adres).

1. Klikněte na nově vytvořený server a potom na **Zabezpečení připojení**.
   ![3-1 Zabezpečení připojení](./media/tutorial-design-database-using-portal/3_1-Connection-security.png)
2. Můžete zde zvolit **Přidat moji IP adresu** nebo nakonfigurovat pravidla brány firewall. Po vytvoření pravidel nezapomeňte kliknout na **Uložit**.
Teď se můžete připojit k serveru pomocí nástroje příkazového řádku mysql nebo nástroje grafického rozhraní MySQL Workbench.

> [!TIP]
> Server Azure Database for MySQL komunikuje prostřednictvím portu 3306. Pokud se pokoušíte připojit z podnikové sítě, nemusí být odchozí provoz přes port 3306 bránou firewall vaší sítě povolený. V takovém případě se nemůžete k serveru Azure SQL připojit, dokud vaše IT oddělení port 3306 neotevře.

## <a name="get-connection-information"></a>Získání informací o připojení
Na webu Azure Portal získejte plně kvalifikovaný **Název serveru** a **Přihlašovací jméno správce serveru** pro váš server Azure Database for MySQL. Plně kvalifikovaný název serveru použijete k připojení k serveru pomocí nástroje příkazového řádku mysql. 

1. Na [webu Azure Portal](https://portal.azure.com/) klikněte v levé nabídce na **Všechny prostředky**, zadejte název a vyhledejte svůj server Azure Database for MySQL. Výběrem názvu serveru zobrazte podrobnosti.

2. Pod nadpisem Nastavení klikněte na **vlastnosti**. Poznamenejte si **NÁZEV SERVERU** a **PŘIHLAŠOVACÍ JMÉNO SPRÁVCE SERVERU**. Může kliknout na tlačítko kopírování vedle jednotlivých polí a zkopírovat údaje do schránky.
   ![4-2 Vlastnosti serveru](./media/tutorial-design-database-using-portal/4_2-server-properties.png)

V tomto příkladu je název serveru *myserver4demo.mysql.database.azure.com* a přihlašovací jméno správce serveru je *myadmin@myserver4demo*.

## <a name="connect-to-the-server-using-mysql"></a>Připojení k serveru pomocí mysql
Použijte [nástroj pro příkazový řádek mysql](https://dev.mysql.com/doc/refman/5.7/en/mysql.html) k navázání připojení k serveru Azure Database for MySQL. Nástroj pro příkazový řádek mysql můžete spustit v prohlížeči pomocí služby Azure Cloud Shell nebo na vlastním počítači pomocí lokálně nainstalovaných nástrojů mysql. Pokud chcete spustit Azure Cloud Shell, klikněte na tlačítko `Try It` na bloku kódu v tomto článku nebo přejděte na webu Azure Portal a klikněte na ikonu `>_` na panelu nástrojů vpravo nahoře. 

Zadejte příkaz pro připojení:
```azurecli-interactive
mysql -h myserver4demo.mysql.database.azure.com -u myadmin@myserver4demo -p
```

## <a name="create-a-blank-database"></a>Vytvoření prázdné databáze
Po připojení k serveru vytvořte prázdnou databázi, se kterou budete pracovat.
```sql
CREATE DATABASE mysampledb;
```

Na příkazovém řádku spusťte následující příkaz, který přepne připojení na tuto nově vytvořenou databázi:
```sql
USE mysampledb;
```

## <a name="create-tables-in-the-database"></a>Vytvoření tabulek v databázi
Teď víte, jak se připojit k databázi Azure Database for MySQL, a můžete začít provádět některé základní úlohy:

Nejdřív vytvoříte tabulku a načtete do ní data. Pojďme vytvořit tabulku, ve které jsou uložené informace o inventáři.
```sql
CREATE TABLE inventory (
    id serial PRIMARY KEY, 
    name VARCHAR(50), 
    quantity INTEGER
);
```

## <a name="load-data-into-the-tables"></a>Načtení dat do tabulek
Teď máte vytvořenou tabulku a můžete do ní vložit data. V otevřeném okně příkazového řádku spusťte následující dotaz, který vloží několik řádků dat.
```sql
INSERT INTO inventory (id, name, quantity) VALUES (1, 'banana', 150); 
INSERT INTO inventory (id, name, quantity) VALUES (2, 'orange', 154);
```

Tabulka, kterou jste vytvořili dříve, teď obsahuje dva řádky ukázkových dat.

## <a name="query-and-update-the-data-in-the-tables"></a>Dotazování na data a aktualizace dat v tabulkách
Spuštěním následujícího příkazu načtete informace z databázové tabulky.
```sql
SELECT * FROM inventory;
```

Data v tabulce můžete také aktualizovat.
```sql
UPDATE inventory SET quantity = 200 WHERE name = 'banana';
```

Při načtení dat se řádek příslušným způsobem aktualizuje.
```sql
SELECT * FROM inventory;
```

## <a name="restore-a-database-to-a-previous-point-in-time"></a>Obnovení databáze k dřívějšímu bodu v čase
Představte si, že omylem odstraníte důležitou databázovou tabulku a nemůžete data snadno obnovit. Azure Database for MySQL umožňuje obnovení serveru do určitého bodu v čase a vytvoření kopie databází na novém serveru. Tento nový server můžete použít k obnovení odstraněných dat. Následující kroky obnoví ukázkový server do bodu před přidáním tabulky.

1. Na webu Azure Portal vyhledejte svoji databázi Azure Database for MySQL. Na stránce **Přehled** klikněte na panelu nástrojů na **Obnovit**. Otevře se stránka Obnovit.

   ![10-1 Obnovení databáze](./media/tutorial-design-database-using-portal/10_1-restore-a-db.png)

2. Do formuláře **Obnovit** zadejte požadované údaje:
   
   ![10-2 Formulář Obnovit](./media/tutorial-design-database-using-portal/10_2-restore-form.png)
   
   - **Bod obnovení:** Vyberte v zadaném časovém rozmezí okamžik, který chcete obnovit. Nezapomeňte převést své místní časové pásmo na čas UTC.
   - **Obnovit na nový server:** Zadejte nový název serveru, na který chcete provést obnovení.
   - **Umístění:** Oblast se shoduje se zdrojovým serverem a nedá se změnit.
   - **Cenová úroveň:** Cenová úroveň se shoduje se zdrojovým serverem a nedá se změnit.
   
3. Kliknutím na **OK** provedete obnovení serveru [k určitému bodu v čase](./howto-restore-server-portal.md) před odstraněním tabulky. Při obnovení serveru se vytvoří nová kopie serveru k zadanému bodu v čase. 

## <a name="next-steps"></a>Další kroky
V tomto kurzu jste se naučili, jak na webu Azure Portal provádět následující kroky:

> [!div class="checklist"]
> * Vytvoření Azure Database for MySQL
> * Konfigurace brány firewall serveru
> * Vytvoření databáze pomocí nástroje pro příkazový řádek mysql
> * Načtení ukázkových dat
> * Dotazování dat
> * Aktualizace dat
> * Obnovení dat

> [!div class="nextstepaction"]
> [Postup připojení aplikací k Azure Database for MySQL](./howto-connection-string.md)
