---
title: "Pokyny pro příklad víceklientské aplikace SQL Database - Wingtip SaaS | Microsoft Docs"
description: "Poskytuje kroky a pokyny pro instalaci a spuštění aplikace víceklientské ukázka, která používá Azure SQL Database, například adresář Wingtip SaaS."
keywords: kurz k sql database
services: sql-database
author: MightyPen
manager: craigg
ms.service: sql-database
ms.custom: scale out apps
ms.workload: On Demand
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/12/2017
ms.author: genemi
ms.openlocfilehash: 4c90d70bb3b043ef81a224f0f69107eaa6eb0547
ms.sourcegitcommit: afc78e4fdef08e4ef75e3456fdfe3709d3c3680b
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/16/2017
---
# <a name="guidance-and-tips-for-azure-sql-database-multi-tenant-saas-app-example"></a>Pokyny a tipů, například aplikace SaaS víceklientské Azure SQL Database


## <a name="download-and-unblock-the-wingtip-tickets-saas-database-per-tenant-scripts"></a>Stáhněte si a odblokování databáze SaaS lístky Wingtip za klienta skripty

Spustitelný soubor obsah (skripty, knihovny DLL) mohou být blokovány Windows, když jsou soubory zip stažené z externího zdroje a rozbalené. Při extrahování skripty ze souboru zip, ***použijte následující postup, chcete-li odblokovat soubor .zip před extrahování***. Tím se zajistí, že je povoleno spustit skripty.

1. Přejděte do [databáze SaaS lístky Wingtip za úložiště GitHub klienta](https://github.com/Microsoft/WingtipTicketsSaaS-DbPerTenant).
2. Klikněte na tlačítko **klonovat nebo stáhnout**.
3. Klikněte na tlačítko **stáhnout ZIP** a soubor uložte.
4. Klikněte pravým tlačítkem myši **WingtipTicketsSaaS-DbPerTenant-master.zip** soubor a vyberte **vlastnosti**.
5. Na **Obecné** vyberte **Odblokovat**.
6. Klikněte na **OK**.
7. Extrahujte soubory.

Skripty, které jsou umístěné v *... \\Learning moduly* složky.


## <a name="working-with-the-wingtip-tickets-saas-database-per-tenant-powershell-scripts"></a>Práce s databáze SaaS lístky Wingtip za klienta PowerShell skripty

Chcete-li maximální využití ukázku ponořte do zadané skripty. Použití zarážek a krok prostřednictvím skriptů, prozkoumání podrobnosti o tom, jak jsou implementované různé vzorce SaaS. Snadno procházet postupně zadaný skriptech a modulech pro nejlepší pochopení, doporučujeme používat [prostředí PowerShell ISE](https://msdn.microsoft.com/powershell/scripting/core-powershell/ise/introducing-the-windows-powershell-ise).

### <a name="update-the-configuration-file-for-your-deployment"></a>Aktualizovat konfigurační soubor pro vaše nasazení

Upravit **UserConfig.psm1** soubor s hodnotou prostředků skupiny a uživatele, který nastavíte během nasazení:

1. Otevřete *prostředí PowerShell ISE* a zatížení... \\Učení moduly\\*UserConfig.psm1* 
2. Aktualizace *ResourceGroupName* a *název* s konkrétními hodnotami pro vaše nasazení (na řádky 10 a 11 pouze).
3. Uložte změny!

Tyto hodnoty nastavení tady jednoduše nebude nutné aktualizovat tyto hodnoty specifické pro nasazení v každé skriptu.

### <a name="execute-scripts-by-pressing-f5"></a>Provádění skriptů stisknutím klávesy F5

Používat několik skriptů *$PSScriptRoot* k procházení složek, a *$PSScriptRoot* je Vyhodnocená jenom když jsou spouštět skripty, stisknutím klávesy **F5**.  Zvýraznění a spuštění výběr (**F8**) může vést k chybám, takže stiskněte **F5** při spouštění skriptů.

### <a name="step-through-the-scripts-to-examine-the-implementation"></a>Procházení skriptů k vyzkoušení implementace

Nejlepší způsob, jak porozumět skripty je procházení je chcete zobrazit, co dělají. Podívejte se na zahrnutou **Demo -** skripty, které jsou k dispozici snadno postupujte podle pracovní postup vysoké úrovně. **Demo -** skripty zobrazit kroky potřebné k provedení každý úkol, takže nastavit zarážky a přejít k podrobnostem hlubší do jednotlivých volání zobrazíte podrobnosti implementace pro různé vzorce SaaS.

Tipy pro zkoumání a procházení skriptů prostředí PowerShell:

- Otevřete **Demo -** skripty v integrovaném Skriptovacím prostředí PowerShell.
- Spustit nebo pokračovat s **F5** (pomocí **F8** se nedoporučuje, protože *$PSScriptRoot* při spuštění výběr skriptu, nebude hodnocen).
- Pokud chcete umístit zarážky, klikněte na řádek nebo ho vyberte a stiskněte klávesu **F9**.
- Funkci nebo volání skriptu můžete vynechat stisknutím klávesy **F10**.
- Na funkci nebo volání skriptu můžete přejít stisknutím klávesy **F11**.
- Aktuální funkci nebo vlání skriptu můžete ukončit stisknutím kombinace **Shift + F11**.


## <a name="explore-database-schema-and-execute-sql-queries-using-ssms"></a>Prozkoumání databázového schématu a spouštění dotazů SQL pomocí SSMS

Použití [SQL Server Management Studio (SSMS)](https://docs.microsoft.com/sql/ssms/download-sql-server-management-studio-ssms) k připojení a procházet aplikačních serverů a databází.

Nasazení původně má dva servery SQL Database pro připojení k – *tenants1-dpt -&lt;uživatele&gt;*  serveru a *katalogu-dpt -&lt;uživatele&gt;* serveru. Aby se zajistilo úspěšné ukázku připojení, mají oba servery [pravidlo brány firewall](sql-database-firewall-configure.md) povolení všechny IP adresy prostřednictvím.


1. Otevřete *SSMS* a připojte se k *tenants1-dpt -&lt;uživatele&gt;. database.windows.net* serveru.
2. Klikněte na **Připojit** > **Databázový stroj...**:

   ![katalogový server](media/saas-dbpertenant-wingtip-app-guidance-tips/connect.png)

3. Přihlašovací údaje pro ukázku jsou: Přihlašovací jméno = *developer*, Heslo = *P@ssword1*

   ![připojení](media/saas-dbpertenant-wingtip-app-guidance-tips/tenants1-connect.png)

4. Opakujte kroky 2 až 3 a připojte se k *katalogu-dpt -&lt;uživatele&gt;. database.windows.net* serveru.


Po úspěšném připojení byste měli vidět oba servery. Seznam databází se může lišit v závislosti na klienty, kterou jste zřídili.

![průzkumník objektů](media/saas-dbpertenant-wingtip-app-guidance-tips/object-explorer.png)



## <a name="next-steps"></a>Další kroky

[Nasazení databáze SaaS lístky Wingtip na aplikaci klienta](saas-dbpertenant-get-started-deploy.md)

