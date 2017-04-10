---
title: "Azure Portal: Vytvoření databáze SQL | Dokumentace Microsoftu"
description: "Naučíte se vytvářet logický server služby SQL Database, pravidlo brány firewall na úrovni serveru a databáze na webu Azure Portal. Také se naučíte dotazovat databázi SQL Azure pomocí webu Azure Portal."
keywords: "kurz k sql database, vytvoření databáze sql"
services: sql-database
documentationcenter: 
author: CarlRabeler
manager: jhubbard
editor: 
ms.assetid: aeb8c4c3-6ae2-45f7-b2c3-fa13e3752eed
ms.service: sql-database
ms.custom: quick start create
ms.workload: data-management
ms.tgt_pltfrm: portal
ms.devlang: na
ms.topic: hero-article
ms.date: 04/03/2017
ms.author: carlrab
translationtype: Human Translation
ms.sourcegitcommit: 303cb9950f46916fbdd58762acd1608c925c1328
ms.openlocfilehash: c0c6cdf8aa48568b7a4468dd87b2896f94fc1bf6
ms.lasthandoff: 04/04/2017


---
# <a name="create-an-azure-sql-database-in-the-azure-portal"></a>Vytvoření databáze SQL Azure na webu Azure Portal

Tento úvodní kurz vás provede postupy vytvoření databáze SQL v Azure.  Azure SQL Database je nabídka „databáze jako služby“, která umožňuje spouštění a škálování vysoce dostupné databáze SQL Serveru v cloudu.  Tento rychlý start ukazuje, jak začít tím, že vytvoříte novou databázi SQL pomocí webu Azure Portal.

Pokud ještě nemáte předplatné Azure, vytvořte si [bezplatný účet](https://azure.microsoft.com/free/) před tím, než začnete.

## <a name="log-in-to-the-azure-portal"></a>Přihlášení k webu Azure Portal

Přihlaste se k [portálu Azure](https://portal.azure.com/).

## <a name="create-a-sql-database"></a>Vytvoření databáze SQL

Databáze SQL Azure se vytvoří s definovanou sadou [výpočetních prostředků a prostředků úložiště](sql-database-service-tiers.md). Databáze se vytvoří v rámci [skupiny prostředků Azure](../azure-resource-manager/resource-group-overview.md) a na [logickém serveru Azure SQL Database](sql-database-features.md). 

Postupujte podle následujících kroků a vytvořte databázi SQL obsahující ukázková data Adventure Works LT. 

1. Klikněte na tlačítko **Nový** v levém horním rohu webu Azure Portal.

2. Na stránce **Nový** vyberte **Databáze** a na stránce **Databáze** vyberte **SQL Database**.

    ![create database-1](./media/sql-database-get-started/create-database-1.png)

3. Vyplňte formulář databáze SQL pomocí následujících informací, jak je vidět na předchozím obrázku:     
   - Název databáze: **mySampleDatabase**
   - Skupina prostředků: **myResourceGroup**
   - Zdroj: **Ukázka (AdventureWorksLT)**

4. Klikněte na **Server** a vytvořte a nakonfigurujte nový server pro novou databázi. Vyplňte **formuláře nového serveru** zadáním globálně jedinečného názvu serveru, jména pro přihlašování správce serveru a hesla podle svého výběru. 

    ![create database-server](./media/sql-database-get-started/create-database-server.png)
5. Klikněte na **Vybrat**.

6. Klikněte na **Cenová úroveň** a určete úroveň služby a úroveň výkonu pro novou databázi. Pro tento rychlý start vyberte **20 DTU** a **250** GB úložiště.

    ![create database-s1](./media/sql-database-get-started/create-database-s1.png)

7. Klikněte na tlačítko **Použít**.  

8. Klikněte na **Vytvořit**, aby se databáze zřídila. Zřizování trvá několik minut. 

9. Na panelu nástrojů klikněte na **Oznámení** a sledujte proces nasazení.

    ![oznámení](./media/sql-database-get-started/notification.png)


## <a name="create-a-server-level-firewall-rule"></a>Vytvoření pravidla brány firewall na úrovni serveru

Služba SQL Database vytvoří bránu firewall na úrovni serveru, aby zabránila externím aplikacím a nástrojům v připojení k serveru nebo ke kterékoli databázi na serveru, pokud není vytvořené pravidlo brány firewall k otevření brány firewall pro konkrétní IP adresy. Postupujte podle těchto kroků a vytvořte [pravidlo brány firewall na úrovni serveru služby SQL Database](sql-database-firewall-configure.md) pro vaši IP adresu klienta a umožněte externí připojení přes bránu firewall služby SQL Database pouze pro vaši IP adresu. 

1. Po dokončení nasazení klikněte na **Databáze SQL** z nabídky na levé straně a klikněte na novou databázi **mySampleDatabase** na stránce **Databáze SQL**. Otevře se stránka s přehledem pro vaši databázi, na které se zobrazí plně kvalifikovaný název serveru (například **mynewserver20170327.database.windows.net**) a možnosti pro další konfiguraci.

      ![pravidlo brány firewall serveru](./media/sql-database-get-started/server-firewall-rule.png) 

2. Klikněte na **Nastavit bránu firewall serveru** na panelu nástrojů, jak je vidět na předchozím obrázku. Otevře se stránka **Nastavení brány firewall** pro server služby SQL Database. 

3. Na panelu nástrojů klikněte na **Přidat IP adresu klienta** a pak klikněte na **Uložit**. Vytvoří se pravidlo brány firewall na úrovni serveru pro vaši aktuální IP adresu.

      ![nastavení pravidla brány firewall serveru](./media/sql-database-get-started/server-firewall-rule-set.png) 

4. Kliknutím na **OK** a pak na **X** zavřete stránku **Nastavení brány firewall**.

Nyní se můžete z této IP adresy připojit k databázi a jejímu serveru pomocí aplikace SQL Server Management Studio nebo jiného nástroje podle vašeho výběru použitím účtu správce serveru vytvořeného dříve.

## <a name="query-the-sql-database"></a>Dotazování databáze SQL

Když jsme vytvořili naši databázi SQL, naplnili jsme ji ukázkovou databází **AdventureWorksLT** (to byla jedna z možností, kterou jsme vybrali dříve v uživatelském rozhraní vytvoření v tomto rychlém startu). Použijeme integrovaný nástroj pro dotazy na webu Azure Portal k dotazům na data. 

1. Na stránce služby SQL Database pro vaši databázi klikněte na panelu nástrojů na **Nástroje**. Otevře se stránka **Nástroje**.

     ![nabídka nástroje](./media/sql-database-get-started/tools-menu.png) 

2. Klikněte na **Editor dotazů (Preview)**, zaškrtněte políčko **Podmínky verze Preview** a pak klikněte na **OK**. Otevře se stránka Editor dotazů.

3. Klikněte na **Přihlásit**, na vyzvání vyberte **Ověřování SQL Serveru** a pak zadejte přihlašovací jméno a heslo správce serveru, které jste vytvořili dříve.

    ![přihlášení](./media/sql-database-get-started/login.png) 

4. Přihlaste se kliknutím na **OK**.

5. Když jste ověřeni, do podokna editoru dotazů zadejte následující dotaz.

   ```
   SELECT TOP 20 pc.Name as CategoryName, p.name as ProductName
   FROM SalesLT.ProductCategory pc
   JOIN SalesLT.Product p
   ON pc.productcategoryid = p.productcategoryid;
   ```

6. Klikněte na **Spustit** a pak zkontrolujte výsledky dotazu v podokně **Výsledky**.

    ![výsledky editoru dotazů](./media/sql-database-get-started/query-editor-results.png)

7. Klikněte na **X**, abyste zavřeli stránku **Editor dotazů**, a pak klikněte na **X** znovu, abyste zavřeli stránku **Nástroje**.

## <a name="clean-up-resources"></a>Vyčištění prostředků

Další rychlé starty v této kolekci jsou postavené na tomto rychlém startu. Pokud chcete pokračovat v práci s dalšími rychlými starty nebo kurzy, nevyčišťujte prostředky vytvořené v rámci tohoto rychlého startu. Pokud pokračovat nechcete, pomocí následujících kroků odstraňte všechny prostředky vytvořené tímto rychlým startem na webu Azure Portal.

1. Na webu Azure Portal v nabídce vlevo klikněte na **Skupiny prostředků** a pak na **myResourceGroup**. 
2. Na stránce skupiny prostředků klikněte na **Odstranit**, do textového pole zadejte **myResourceGroup** a pak klikněte na **Odstranit**.

## <a name="next-steps"></a>Další kroky

- Informace o připojení a dotazování pomocí aplikace SQL Server Management Studio najdete v tématu [Připojení a dotazování pomocí SSMS](sql-database-connect-query-ssms.md).
- Informace o připojení pomocí sady Visual Studio najdete v tématu [Připojení a dotazování pomocí sady Visual Studio](sql-database-connect-query.md).
- Technický přehled služby SQL Database najdete v tématu [O službě SQL Database](sql-database-technical-overview.md).

