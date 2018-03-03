---
title: "Pomocí databáze poskytované RP adaptér SQL v Azure zásobníku | Microsoft Docs"
description: "Postup vytvoření a správě databází SQL, které jsou zřízené prostřednictvím poskytovatele prostředků adaptér SQL"
services: azure-stack
documentationCenter: 
author: mattbriggs
manager: femila
editor: 
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/28/2018
ms.author: mabrigg
ms.openlocfilehash: 39f6cc30191f07a7c891446a9132222a6d264dc4
ms.sourcegitcommit: 782d5955e1bec50a17d9366a8e2bf583559dca9e
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 03/02/2018
---
# <a name="create-sql-databases"></a>Vytvoření databáze SQL

*Platí pro: Azure zásobníku integrované systémy a Azure zásobníku Development Kit*

Databáze samoobslužné služby je zajišťována prostřednictvím portálu činnost koncového uživatele. Uživatel potřebuje odběr, který má nabídku, který obsahuje službu databáze.

1. Přihlaste se k [zásobník Azure](azure-stack-poc.md) portálu user portal (Správci služby můžete také použít portál pro správu).

2. Klikněte na tlačítko **+ nový** &gt; **Data + úložiště "** &gt; **databáze serveru SQL (preview)** &gt; **přidat**.

3. Vyplňte formulář s podrobnostmi o databázi, včetně **název databáze**, **maximální velikost**a podle potřeby změnit ostatní parametry. Jste vyzváni k vyberte SKU pro vaši databázi. Při přidávání hostitelskými servery, je přiřazený SKU. Databáze se vytvoří v tomto fondu hostování servery, které tvoří verze SKU.

  ![Nová databáze](./media/azure-stack-sql-rp-deploy/newsqldb.png)

  >[!NOTE]
  > Velikost databáze musí být minimálně 64 MB. Může být zvýšena pomocí nastavení.

4. Zadejte nastavení přihlášení: **přihlášení databázi**, a **heslo**. Tato nastavení jsou vytvořené pro přístup k této databázi pouze pověření ověřování SQL. Přihlašovací uživatelské jméno musí být globálně jedinečný. Vytvořit nové nastavení přihlášení nebo vyberte nějaký existující. Můžete znovu použít nastavení přihlášení pro jiné databáze pomocí stejné SKU.

    ![Vytvořte nové přihlašovací údaje databáze](./media/azure-stack-sql-rp-deploy/create-new-login.png)


5. Odeslání formuláře a počkejte na dokončení nasazení.

    V okně výsledné Všimněte si pole "Připojovací řetězec". V zásobníku vaší Azure můžete použít tento řetězec v jakékoli aplikaci, která vyžaduje přístup k serveru SQL (například webové aplikace).

    ![Načtení připojovacího řetězce](./media/azure-stack-sql-rp-deploy/sql-db-settings.png)

## <a name="delete-sql-databases"></a>Odstranění databáze SQL
Z portálu

>[!NOTE]
>
>Při odstranění databáze SQL AlwaysOn z RP se úspěšně odstranil z primární a skupiny dostupnosti AlwaysOn, ale podle návrhu SQL AG umístí databáze ve stavu v každé repliky obnovení a není odpojení databáze, pokud aktivuje. Pokud není vyřazen databáze sekundární repliky přejde do Not synchronizing stavu. Znovu přidávání nové databáze pro skupinu AG se stejným prostřednictvím RP stále funguje. V tématu ![odebrání sekundární databáze](https://docs.microsoft.com/sql/database-engine/availability-groups/windows/remove-a-secondary-database-from-an-availability-group-sql-server)

## <a name="manage-database-credentials"></a>Spravovat přihlašovací údaje databáze
Můžete aktualizovat přihlašovací údaje databáze (nastavení přihlášení).

## <a name="verify-sql-alwayson-databases"></a>Ověřte databází SQL AlwaysOn
AlwaysOn databáze by měl zobrazit, protože synchronizován a k dispozici na všech instancích a ve skupině dostupnosti. Po převzetí služeb při selhání by měl bezproblémově připojit databázi. Chcete-li ověřit, že databáze je synchronizace můžete použít SQL Server Management Studio:

![Ověřte AlwaysOn](./media/azure-stack-sql-rp-deploy/verifyalwayson.png)
