---
title: "Azure Data Lake Analytics kvótami | Microsoft Docs"
description: "Zjistěte, jak upravit a zvýšit maximální kvóty v účtech Azure Data Lake Analytics (ADLA)."
services: data-lake-analytics
keywords: Azure Data Lake Analytics
documentationcenter: 
author: omidm1
editor: omidm1
ms.assetid: 49416f38-fcc7-476f-a55e-d67f3f9c1d34
ms.service: data-lake-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 06/18/2017
ms.author: omidm
ms.openlocfilehash: 957f306ea0e80b5830ad64e5ef06c6d122d9eccc
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/11/2017
---
# <a name="azure-data-lake-analytics-quota-limits"></a>Maximální kvóty Azure Data Lake Analytics

Zjistěte, jak upravit a zvýšit maximální kvóty v účtech Azure Data Lake Analytics (ADLA). Znalost těchto mezních hodnot vám mohou pomoci porozumět chování vaší úlohy U-SQL. Kvótami všechny jsou logicky, takže může zvýšit maximální limit oslovit nám.

## <a name="azure-subscriptions-limits"></a>Omezení předplatných Azure

**Maximální počet ADLA účty podle předplatného:** 5

 Toto je maximální počet ADLA účty, které můžete vytvořit na jedno předplatné. Pokud se pokusíte vytvořit účet šesté ADLA, bude dojde k chybě "Bylo dosaženo maximální počet účtů Data Lake Analytics povoleno (5) v oblasti pod názvem odběru". V takovém případě buď odstranit všechny nepoužívané ADLA účty, nebo se obraťte na nás pomocí [otevřením lístku podpory](#increase-maximum-quota-limits).

## <a name="adla-account-limits"></a>Limity účtu ADLA

**Maximální počet jednotek Analytics (Austrálie) na účet:** 250

Toto je maximální počet Austrálie, které můžou běžet současně ve vašem účtu. Pokud váš celkový počet spuštění Austrálie pro všechny úlohy překračuje tento limit, novější úlohy se zařadí do fronty automaticky. Například:

* Pokud máte pouze jednu úlohu s 250 Austrálie při odesílání druhý úloha se bude čekat ve frontě úloh, dokud první úloha dokončena.
* Pokud již máte pět spuštěné úlohy a každý používá 50 Austrálie při odesílání šesté úlohu, která potřebuje 20 Austrálie čeká ve frontě úloh, dokud jsou 20 Austrálie k dispozici.

**Maximální počet souběžných úloh U-SQL na účet:** 20

Toto je maximální počet úloh, které můžou běžet současně ve vašem účtu. Vyšší než tato hodnota novější úlohy se zařadí do fronty automaticky.

## <a name="adjust-adla-quota-limits-per-account"></a>Upravit ADLA kvótami každý účet

1. Přihlaste se k portálu [Azure Portal](https://portal.azure.com).
2. Zvolte existující účet ADLA.
3. Klikněte na **Vlastnosti**.
4. Upravit **paralelismus** a **souběžných úloh** podle svých potřeb.

    ![Okno portálu Azure Data Lake Analytics](./media/data-lake-analytics-quota-limits/data-lake-analytics-quota-properties.png)

## <a name="increase-maximum-quota-limits"></a>Zvýšit maximální kvóty

1. Otevřete žádost o podporu na portálu Azure.

    ![Okno portálu Azure Data Lake Analytics](./media/data-lake-analytics-quota-limits/data-lake-analytics-quota-help-support.png)

    ![Okno portálu Azure Data Lake Analytics](./media/data-lake-analytics-quota-limits/data-lake-analytics-quota-support-request.png)
2. Vyberte typ problému **kvóty**.
3. Vyberte vaše **předplatné** (ujistěte se, není "zkušební" předplatné).
4. Vyberte typ kvóty **Data Lake Analytics**.

    ![Okno portálu Azure Data Lake Analytics](./media/data-lake-analytics-quota-limits/data-lake-analytics-quota-support-request-basics.png)

5. V okně problém popisují požadovanou zvyšte limit s **podrobnosti** z Proč potřebujete tuto kapacitu navíc.

    ![Okno portálu Azure Data Lake Analytics](./media/data-lake-analytics-quota-limits/data-lake-analytics-quota-support-request-details.png)

6. Ověřte kontaktní informace a vytvořte žádost o podporu.

Společnost Microsoft nezkontroluje vaši žádost a pokusí se co nejdříve podle vašich obchodních potřeb.

## <a name="next-steps"></a>Další kroky

* [Přehled služby Microsoft Azure Data Lake Analytics](data-lake-analytics-overview.md)
* [Správa Azure Data Lake Analytics pomocí Azure PowerShellu](data-lake-analytics-manage-use-powershell.md)
* [Monitorování úloh Azure Data Lake Analytics a odstraňování potíží pomocí webu Azure Portal](data-lake-analytics-monitor-and-troubleshoot-jobs-tutorial.md)
