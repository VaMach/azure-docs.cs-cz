<properties
    pageTitle="Vytvoření nového elastického fondu pomocí portálu Azure | Microsoft Azure"
    description="Jak přidat škálovatelný fond elastické databáze do konfigurace vaší databáze SQL pro snazší administraci a sdílení prostředků mezi mnoha databázemi."
    keywords="scalable database,database configuration"
    services="sql-database"
    documentationCenter=""
    authors="ninarn"
    manager="jhubbard"
    editor=""/>

<tags
    ms.service="sql-database"
    ms.devlang="NA"
    ms.date="05/06/2016"
    ms.author="ninarn"
    ms.workload="data-management"
    ms.topic="get-started-article"
    ms.tgt_pltfrm="NA"/>


# Vytvoření nového fondu elastické databáze pomocí portálu Azure

> [AZURE.SELECTOR]
- [Portál Azure](sql-database-elastic-pool-create-portal.md)
- [PowerShell](sql-database-elastic-pool-create-powershell.md)
- [C#](sql-database-elastic-pool-create-csharp.md)

Tento článek popisuje, jak vytvořit [fond elastické databáze](sql-database-elastic-pool.md) pomocí [portálu Azure](https://portal.azure.com/). Fond můžete vytvořit dvěma způsoby. Pokud znáte cílovou konfiguraci fondu, můžete ho vytvořit od začátku sami, nebo můžete začít s fondem, který vám doporučí služba. SQL Database má vestavěné inteligentní algoritmy, které vám na základě historické telemetrie využívání vašich databází doporučí konfiguraci fondu, pokud bude cenově výhodnější než aktuální stav.

Můžete přidat více fondů na jeden server, ale nemůžete přidat databáze z různých serverů do stejného fondu. K vytvoření fondu potřebujete nejméně jednu databázi na serveru verze 12. Pokud žádnou nemáte, přečtěte si téma [Vytvoření první databáze Azure SQL](sql-database-get-started.md). Můžete vytvořit fond obsahující jen jednu databázi, ale fondy jsou cenově efektivní, pouze pokud obsahují více databází. Viz článek [Cenové a výkonové požadavky fondu elastické databáze](sql-database-elastic-pool-guidance.md).

> [AZURE.NOTE] Fondy jsou aktuálně dostupné jen pro servery SQL Database verze 12. Pokud máte databáze na serveru verze 11, můžete je [pomocí skriptu prostředí PowerShell identifikovat jako kandidáty pro fond](sql-database-elastic-pool-database-assessment-powershell.md) na serveru verze 12 a potom [použít PowerShell k upgradu na verzi 12 a vytvořit fond](sql-database-upgrade-server-powershell.md) v jednom kroku.

## Krok 1: Vytvořte nový fond

Vytvořte fond elastické databáze přidáním nového fondu k serveru. Můžete přidat více fondů na jeden server, ale nemůžete přidat databáze z různých serverů do stejného fondu.

1. V [portálu Azure](http://portal.azure.com/) klikněte na **Servery SQL** a potom na server obsahující databáze, které chcete přidat do fondu.
2. Klikněte na **Nový fond**.

    ![Přidejte fond na server](./media/sql-database-elastic-pool-create-portal/new-pool.png)

    **- nebo -**

    Může se zobrazit zpráva, že se pro server (pouze verze 12) doporučují fondy elastických databází. Kliknutím na zprávu zobrazíte doporučení k vytvoření fondů založená na historické telemetrii využití databází. Pak kliknutím na úroveň zobrazte další podrobnosti a možnosti přizpůsobení fondu. Podrobnější informace o doporučeních najdete v části [Vysvětlení doporučení k fondům](#understand-pool-recommendations) dále v tomto tématu.

    ![doporučený fond](./media/sql-database-elastic-pool-create-portal/recommended-pool.png)

    Objeví se okno **Fond elastické databáze**, ve kterém můžete fond vytvořit. Pokud jste v předchozím kroku klikli na **Nový fond**, portál zvolí pro nastavení **Cenová úroveň** možnost **Standardní fond**, vytvoří jedinečný **Název** fondu a použije výchozí konfiguraci. Pokud jste zvolili doporučený fond, jsou již nastaveny doporučené hodnoty cenové úrovně a konfigurace fondu, ale můžete je stále změnit.

    ![Konfigurace elastického fondu](./media/sql-database-elastic-pool-create-portal/configure-elastic-pool.png)

3. Zadejte název elastického fondu nebo ponechte výchozí hodnotu.

## Krok 2: Zvolte cenovou úroveň

Cenová úroveň fondu určuje funkce, které jsou pro elastické databáze ve fondu dostupné, a maximální počet jednotek eDTU (eDTU MAX) a úložiště (GB) pro každou databázi. Podrobnosti viz Úrovně služeb

Chcete-li změnit cenovou úroveň fondu, klikněte na možnost **Cenová úroveň**, vyberte požadovanou úroveň a klikněte na tlačítko **Vybrat**.

> [AZURE.IMPORTANT] Až vyberete cenovou úroveň a potvrdíte svoji volbu kliknutím na **OK** v posledním kroku, nebude už možné cenovou úroveň fondu změnit. Cenovou úroveň existujícího elastického fondu je možné změnit pouze tak, že vytvoříte nový elastický fond s požadovanou cenovou úrovní a provedete migraci elastických databází do tohoto nového fondu.

![Výběr cenové úrovně](./media/sql-database-elastic-pool-create-portal/pricing-tier.png)

## Krok 3: Konfigurace fondu

Po nastavení cenové úrovně klikněte na tlačítko Konfigurovat fond a následně přidejte databáze a nastavte pro fond hodnoty eDTU a úložiště (GB), stejně jako minimální a maximální hodnoty eDTU pro elastické databáze ve fondu.

1. Klikněte na **Konfigurovat fond**
2. Vyberte databáze, které chcete přidat do fondu. Tento krok je při vytváření fondu volitelný. Databáze můžete přidat i po vytvoření fondu.
    Klikněte na možnost **Přidat databázi**, pak na databáze, které chcete přidat a potom na tlačítko **Vybrat**.

    ![Přidání databází](./media/sql-database-elastic-pool-create-portal/add-databases.png)

    Pokud databáze, s kterými pracujete, mají dostatek historických telemetrických dat, graf **Odhadované eDTU a využití GB** a pruhový graf **Skutečné využití eDTU** se aktualizují, aby vám pomohly s rozhodováním o hodnotách konfigurace. Služba vám také může zobrazovat doporučení s cílem nastavit optimální velikost fondu. Viz část [Dynamická doporučení](#dynamic-recommendations).

3. Pomocí ovládacích prvků na stránce **Konfigurace fondu** můžete zkontrolovat nastavení a konfigurovat fond. V tématu [Omezení elastických fondů](sql-database-elastic-pool.md#edtu-and-storage-limits-for-elastic-pools-and-elastic-databases) najdete podrobnosti o omezeních pro jednotlivé úrovně služby a téma [Cenové a výkonové požadavky fondů elastické databáze](sql-database-elastic-pool-guidance.md) obsahuje podrobné pokyny k optimalizaci velikosti fondu. Další podrobnosti o nastavení fondu najdete v tématu [Vlastnosti fondu elastické databáze](sql-database-elastic-pool.md#elastic-database-pool-properties).

    ![Konfigurace elastického fondu](./media/sql-database-elastic-pool-create-portal/configure-performance.png)

4. Po úpravě nastavení v okně **Konfigurace fondu** klikněte na tlačítko **Vybrat**.
5. Kliknutím na tlačítko **OK** vytvořte fond.


## Vysvětlení doporučení k fondům

Služba SQL Database vyhodnocuje historii využití a doporučí použití jednoho nebo několika fondů, jakmile to začne být cenově výhodnější než použití izolované databáze. Každé doporučení je konfigurováno pro jedinečnou podmnožinu databází serveru, která je pro fond nejvhodnější.

![doporučený fond](./media/sql-database-elastic-pool-create-portal/recommended-pool.png)  

Doporučení fondu zahrnuje:

- cenovou úroveň pro fond (Basic, Standard nebo Premium),
- vhodnou hodnotu **POOL eDTU** (označovanou také jako Max eDTU pro fond),
- hodnoty **eDTU MAX** a **eDTU MIN** pro každou databázi,
- seznam doporučených databází pro fond.

Při vytváření doporučení bere služba v úvahu telemetrická data za posledních 30 dní. Aby databáze mohla být zařazena mezi doporučené pro fond elastické databáze, musí existovat alespoň 7 dní. Databáze, které již ve fondu elastické databáze jsou, se nepovažují za kandidáty pro doporučení fondu elastické databáze.

Služba vyhodnocuje potřebné prostředky a cenovou výhodnost přesunu jednotlivých databází v každé úrovni služby do fondu ve stejné úrovni. Například pro všechny databáze na serveru, které jsou v úrovni Standard, se zvažuje výhodnost jejich přesunu do elastického fondu také s úrovní Standard. To znamená, že služba nikdy nenavrhne přesun databáze mezi úrovněmi, například přesun databáze s úrovní Standard do fondu s úrovní Premium.

### Dynamická doporučení

Po přidání databází do fondu se dynamicky vygeneruje doporučení na základě historie využití databází, které jste vybrali. Tato doporučení se zobrazí v grafu eDTU a využití GB a také v oblasti doporučení v horní části okna **Konfigurace fondu**. Tato doporučení jsou určena k tomu, aby vám pomohla vytvořit optimální fond pro vaše konkrétní databáze.

![dynamická doporučení](./media/sql-database-elastic-pool-create-portal/dynamic-recommendation.png)

## Další zdroje

- [Správa elastického fondu SQL Database pomocí portálu](sql-database-elastic-pool-manage-portal.md)
- [Správa elastického fondu SQL Database pomocí prostředí PowerShell](sql-database-elastic-pool-manage-powershell.md)
- [Správa elastického fondu SQL Database pomocí jazyka C#](sql-database-elastic-pool-manage-csharp.md)
- [Horizontální navýšení kapacity s Azure SQL Database](sql-database-elastic-scale-introduction.md) 




<!--HONumber=Jun16_HO2-->


