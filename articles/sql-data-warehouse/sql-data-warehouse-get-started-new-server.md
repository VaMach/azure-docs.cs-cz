<properties
   pageTitle="Vytvoření databáze SQL Data Warehouse na portálu Azure | Microsoft Azure"
   description="Naučte se vytvářet Azure SQL Data Warehouse na portálu Azure."
   services="sql-data-warehouse"
   documentationCenter="NA"
   authors="barbkess"
   manager="jhubbard"
   editor=""
   tags="azure-sql-data-warehouse"/>

<tags
   ms.service="sql-data-warehouse"
   ms.devlang="NA"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-services"
   ms.date="05/03/2016"
   ms.author="lodipalm;"/>

# Vytvoření nového logického serveru

V rámci služeb SQL Database a SQL Data Warehouse je každá databáze přiřazena k serveru a každý server je přiřazen ke geografickému umístění. Serveru se říká logický SQL server.

> [AZURE.NOTE] <a name="note"></a>Logický SQL server:
  >
  > + Zajišťuje konzistentní způsob konfigurace více databází v rámci stejného geografického umístění.
  > + Není to fyzický hardware jako v případě místního serveru. Je součástí softwaru služby. Říkáme mu proto *logický server*.
  > + Může být hostitelem více databází, aniž by to mělo vliv na výkon.
  > + V jeho názvu se používá malé písmeno *s*. SQL **s**erver je logický server Azure, zatímco SQL **S**erver je databázový produkt společnosti Microsoft určený pro místní prostředí.

1. Klikněte na **Server** > **Vytvořit nový server**. Za server nic neplatíte. Pokud už máte logický SQL server V12, který chcete používat, zvolte existující server a přejděte k dalšímu kroku.

    ![Vytvoření nového serveru](./media/sql-data-warehouse-get-started-provision/create-server.png)

2. Zadejte informace o **novém serveru**.

    - **Název serveru**: Zadejte název logického serveru. Tento název je pro každé geografické umístění jedinečný.
    - **Jméno správce serveru:** Zadejte uživatelské jméno pro účet správce serveru.
    - **Heslo:** Zadejte heslo správce serveru.
    - **Umístění:** Zvolte geografické umístění serveru. Aby se zkrátila doba přenosu dat, je nejlepší umístit si server geograficky v blízkosti dalších datových prostředků, ke kterým bude tato databáze potřebovat přístup.
    - **Vytvořit server V12:** Pro SQL Data Warehouse je jedinou možností možnost ANO.
    - **Povolit službám Azure přístup k serveru:** Pro SQL Data Warehouse je vždy zaškrtnuto.

    >[AZURE.NOTE] Nezapomeňte si někam uložit informace o názvu serveru, jménu správce serveru a heslu.  Budete je potřebovat k přihlášení k serveru.

3. Kliknutím na **OK** uložíte nastavení konfigurace logického SQL serveru a vrátíte se do okna SQL Data Warehouse.

    ![Nakonfigurování nového serveru](./media/sql-data-warehouse-get-started-provision/configure-server.png)



<!--HONumber=Jun16_HO2-->


