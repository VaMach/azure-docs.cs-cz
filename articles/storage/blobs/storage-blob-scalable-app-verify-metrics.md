---
title: "Ověřte, propustnosti a latence metriky pro účet úložiště na portálu Azure | Microsoft Docs"
description: "Zjistěte, jak ověřit, propustnosti a latence metriky pro účet úložiště na portálu."
services: storage
documentationcenter: 
author: georgewallace
manager: jeconnoc
editor: 
ms.service: storage
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: csharp
ms.topic: tutorial
ms.date: 12/12/2017
ms.author: gwallace
ms.custom: mvc
ms.openlocfilehash: b3102bd4e40e10fe88c12295794da37e359c56f1
ms.sourcegitcommit: 922687d91838b77c038c68b415ab87d94729555e
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 12/13/2017
---
# <a name="verify-throughput-and-latency-metrics-for-a-storage-account"></a>Ověřte, propustnosti a latence metriky pro účet úložiště

V tomto kurzu je součástí čtyři a poslední část řady. V předchozích kurzech jste zjistili, jak nahrávání a stahování larges množství náhodná data k účtu úložiště Azure. Tento kurz ukazuje, jak metriky můžete zobrazit propustnosti a latence na portálu Azure.

V rámci čtyři řady, zjistíte, jak:

> [!div class="checklist"]
> * Konfigurace grafů na portálu Azure
> * Ověření metrik latence a propustnosti

[Úložiště Azure metriky](../common/storage-metrics-in-azure-monitor.md?toc=%2fazure%2fstorage%2fblobs%2ftoc.json) Azure monitorování se používá k poskytnutí jednotný pohled na výkon a dostupnost účtu úložiště.

## <a name="configure-metrics"></a>Konfigurace metriky

Přejděte na **metriky (preview)** pod **nastavení** ve vašem účtu úložiště.

Vyberte objekt Blob z **SUB služby** rozevíracího seznamu.

V části **METRIKA**, vyberte jednu z metriky najít v následující tabulce:

Následující metriky můžete získat představu o latence a propustnosti aplikace. Metriky, které jste nakonfigurovali v portálu jsou průměry 1 minutu. Pokud transakce dokončení uprostřed minutu minut dat je halfed pro průměr. V aplikaci byly vypršel časový limit operace nahrávání a stahování a zadaný výstup z skutečné množství času, kterou trvalo nahrávání a stahování souborů. Tyto informace můžete použít ve spojení s portálu metriky k plnému pochopení propustnost.

|Metrika|Definice|
|---|---|
|**Latence E2E úspěch**|Průměrná latence začátku do konce úspěšné požadavky na služby úložiště nebo zadaná operace rozhraní API. Tato hodnota zahrnuje požadovaná doba zpracování v rámci Azure Storage pro čtení požadavku, odeslání odpovědi a přijímat potvrzení odpovědi.|
|**Latence serveru úspěšné**|Průměrná doba používá ke zpracování v případě úspěšné žádosti úložiště Azure. Tato hodnota nezahrnuje latenci sítě, zadaný v SuccessE2ELatency. |
|**Transakce**|Počet požadavků na služby úložiště nebo zadaná operace rozhraní API. Toto číslo zahrnuje úspěšné i neúspěšné požadavky a požadavky, které vytváří chyby. V příkladu velikost bloku byl nastaven na hodnotu 100 MB. Každý blok 100 MB v takovém případě je považován za transakci.|
|**Příjem příchozích dat**|Množství vstupní data. Tato hodnota zahrnuje příjem příchozích dat z externí klienta do Azure Storage, jakož i příjem příchozích dat v rámci Azure. |
|**Odchozí**|Množství odchozí data. Tato hodnota zahrnuje odchozí z externí klienta do Azure Storage a také odchozí v rámci Azure. Toto číslo v důsledku toho nemusí odpovídat fakturovatelný odchozí. |

Vyberte **posledních 24 hodin (Automatická)** vedle **čas**. Zvolte **poslední hodinu** a **minutu** pro **členitost času**, pak klikněte na tlačítko **použít**.

![Metriky účtu úložiště](./media/storage-blob-scalable-app-verify-metrics/figure1.png)

Grafy může mít více než jeden metriku, které jsou jim přiřazeny, ale přiřazení více než jeden metrika zakáže možnost do skupiny tím, že dimenze.

## <a name="dimensions"></a>Dimenze

[Dimenze](../common/storage-metrics-in-azure-monitor.md?toc=%2fazure%2fstorage%2fblobs%2ftoc.json#metrics-dimensions) se používají k hlubší viděl do grafy a získat podrobnější informace. Jiné metriky mají různé dimenze. Jednu dimenzi, která je k dispozici je **název rozhraní API** dimenze. Tato dimenze dělí na graf do každé samostatné volání rozhraní API. První následující obrázek ukazuje graf aplikace příklad celkový počet transakcí pro účet úložiště. Druhý obrázek ukazuje stejné grafu, ale s rozhraním API název dimenze vybrané. Jak můžete vidět, je uvedena každou transakci poskytnutí další podrobnosti do kolik volání byly provedeny podle názvu rozhraní API.

![Metriky účet úložiště – transakce bez dimenze](./media/storage-blob-scalable-app-verify-metrics/transactionsnodimensions.png)

![Metriky účet úložiště – transakce](./media/storage-blob-scalable-app-verify-metrics/transactions.png)

## <a name="clean-up-resources"></a>Vyčištění prostředků

Pokud už je nepotřebujete, odstraňte skupinu prostředků, virtuální počítač a všechny související prostředky. To pokud chcete udělat, vyberte skupinu prostředků pro virtuální počítač a klikněte na příkaz odstranit.

## <a name="next-steps"></a>Další kroky

V části čtyři řady Seznámili jste se zobrazení metriky pro příklad řešení, jako například:

> [!div class="checklist"]
> * Konfigurace grafů na portálu Azure
> * Ověření metrik latence a propustnosti

Tento odkaz zobrazíte ukázky předdefinovaných úložiště.

> [!div class="nextstepaction"]
> [Ukázky skriptu úložiště Azure](storage-samples-blobs-cli.md)

[previous-tutorial]: storage-blob-scalable-app-download-files.md