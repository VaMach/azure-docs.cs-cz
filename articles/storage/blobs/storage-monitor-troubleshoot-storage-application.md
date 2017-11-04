---
title: "Sledování a řešení potíží k aplikaci cloudové úložiště v Azure | Microsoft Docs"
description: "Pomocí diagnostických nástrojů, metriky a výstrahy sledovat cloudových aplikací a řešení potíží."
services: storage
documentationcenter: 
author: georgewallace
manager: timlt
editor: 
ms.service: storage
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: csharp
ms.topic: tutorial
ms.date: 09/19/2017
ms.author: gwallace
ms.custom: mvc
ms.openlocfilehash: db88c331f79d83e0124519f8b6dbb34514b456dd
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/11/2017
---
# <a name="monitor-and-troubleshoot-a-cloud-storage-application"></a>Monitorování a řešení potíží s aplikací na cloudové úložiště

V tomto kurzu je součástí čtyři a poslední část řady. Zjistíte, jak ke sledování a řešení potíží s aplikací na cloudové úložiště.

V rámci čtyři řady, zjistíte, jak:

> [!div class="checklist"]
> * Zapnout protokolování a metriky
> * Povolit výstrahy pro chyb autorizace
> * Spusťte test provoz s nesprávnou tokeny SAS
> * Stáhněte si a analyzovat protokoly

[Azure storage analytics](../common/storage-analytics.md) poskytuje data protokolování a metriky pro účet úložiště. Tato data poskytují přehled o stavu vašeho účtu úložiště. Předtím, než může být viditelnost do účtu úložiště, budete muset nastavit shromažďování dat. Tento proces zahrnuje zapnutí protokolování, konfigurace metriky a povolení výstrahy.

Protokolování a metriky z účty úložiště jsou povolené z **diagnostiky** na portálu Azure. Existují dva typy metrik. **Agregační** shromažďovat metriky procenta vstupní/výstupní, dostupnosti, latence a úspěch. Tyto metriky se shromažďují pro objekt blob, fronty, tabulky a souborové služby. **Na rozhraní API** shromažďuje stejnou sadu metriky pro každé operace úložiště v rozhraní API služby Azure Storage. Protokolování úložiště umožňuje podrobnosti záznamu pro úspěšné i neúspěšné požadavky ve vašem účtu úložiště. Tyto protokoly umožňují najdete v části Podrobnosti o čtení, zápisu a odstraňování operace u vaší Azure tabulky, fronty a objekty BLOB. Umožňují také zobrazíte důvody pro neúspěšné požadavky například časové limity, omezení a chyb autorizace.

## <a name="log-in-to-the-azure-portal"></a>Přihlášení k portálu Azure Portal

Přihlaste se k portálu [Azure Portal](https://portal.azure.com).

## <a name="turn-on-logging-and-metrics"></a>Zapnout protokolování a metriky

V nabídce vlevo vyberte **skupiny prostředků**, vyberte **myResourceGroup**a pak vyberte v seznamu prostředků účtu úložiště.

V části **diagnostiky** nastavit **stav** k **na**. Ujistěte se, **Blob agregovaná metrika**, **Blob metriku API**, a **Blob protokoly** jsou povoleny.

Po dokončení klikněte na tlačítko **uložit**

![Podokno diagnostiky](media/storage-monitor-troubleshoot-storage-application/figure1.png)

## <a name="enable-alerts"></a>Povolit výstrahy

Výstrahy umožňují e-mailem správci nebo aktivovat webhook, jehož podle metriky před nedodržením prahovou hodnotu. V tomto příkladu je povolit upozornění `SASClientOtherError` metriku.

### <a name="navigate-to-the-storage-account-in-the-azure-portal"></a>Přejděte na účet úložiště na portálu Azure

V nabídce vlevo vyberte **skupiny prostředků**, vyberte **myResourceGroup**a pak vyberte v seznamu prostředků účtu úložiště.

V části **monitorování** vyberte **výstrah pravidla**.

Vyberte **+ přidat upozornění**v části **přidání pravidla výstrahy**, zadejte požadované informace. Zvolte `SASClientOtherError` z **metrika** rozevíracího seznamu.

![Podokno diagnostiky](media/storage-monitor-troubleshoot-storage-application/figure2.png)

## <a name="simulate-an-error"></a>Simulovat chybu

K simulaci platný výstrahy, můžete se pokusit požadavku neexistující objekt blob z účtu úložiště. Chcete-li to provést, nahraďte `<incorrect-blob-name>` s hodnotou, která neexistuje. Spusťte následující ukázka kódu několikrát k simulaci neúspěšné požadavky objektů blob.

```azurecli-interactive
sasToken=$(az storage blob generate-sas \
    --account-name <storage-account-name> \
    --account-key <storage-account-key> \
    --container-name <container> \
    --name <incorrect-blob-name> \
    --permissions r \
    --expiry `date --date="next day" +%Y-%m-%d` \
    --output tsv)

curl https://<storage-account-name>.blob.core.windows.net/<container>/<incorrect-blob-name>?$sasToken
```

Na následujícím obrázku je příklad upozornění, že je založena na simulované selhání spustil se v předchozím příkladu.

 ![Příklad upozornění](media/storage-monitor-troubleshoot-storage-application/alert.png)

## <a name="download-and-view-logs"></a>Stažení a zobrazit protokoly

Protokoly úložiště ukládání dat do sady objektů BLOB v kontejneru objektů blob s názvem **$logs** ve vašem účtu úložiště. Tento kontejner nezobrazuje Pokud seznamu všechny kontejnery objektů blob ve vašem účtu, ale pokud přímý přístup můžete zobrazit jeho obsah.

V tomto scénáři použijete [Microsoft Message Analyzer](http://technet.microsoft.com/library/jj649776.aspx) pro interakci s vaším účtem úložiště Azure.

### <a name="download-microsoft-message-analyzer"></a>Stáhněte si Microsoft Message Analyzer

Stáhněte si [Microsoft Message Analyzer](https://www.microsoft.com/download/details.aspx?id=44226) a instalace aplikace.

Spuštění aplikace a zvolte **soubor** > **otevřete** > **z jiných zdrojů souboru**.

V **souboru selektor** dialogovém okně, vyberte **+ přidat připojení Azure**. Zadejte ve vaší **název účtu úložiště** a **klíč účtu** a klikněte na tlačítko **OK**.

![Microsoft Message Analyzer - přidat dialogové okno připojení úložiště Azure](media/storage-monitor-troubleshoot-storage-application/figure3.png)

Jakmile se připojíte, rozbalte kontejnery ve stromu úložiště zobrazení umožní zobrazit objekty BLOB protokolu. Vyberte nejnovější protokolu a klikněte na **OK**.

![Microsoft Message Analyzer - přidat dialogové okno připojení úložiště Azure](media/storage-monitor-troubleshoot-storage-application/figure4.png)

Na **novou relaci** dialogové okno, klikněte na tlačítko **spustit** Chcete-li zobrazit protokol.

Jakmile se otevře v protokolu, můžete zobrazit události úložiště. Jak je vidět na následujícím obrázku, došlo `SASClientOtherError` spuštěno na účet úložiště. Další informace o protokolování úložiště, najdete v článku [Storage Analytics](../common/storage-analytics.md).

![Microsoft Message Analyzer - zobrazování událostí](media/storage-monitor-troubleshoot-storage-application/figure5.png)

[Storage Explorer](https://azure.microsoft.com/features/storage-explorer/) je jiný nástroj, který slouží k interakci s účty úložiště, včetně **$logs** kontejneru a protokoly, které jsou v ní obsažené.

## <a name="next-steps"></a>Další kroky

V rámci čtyři a poslední část řady jste zjistili, jak ke sledování a řešení potíží s váš účet úložiště, jako například:

> [!div class="checklist"]
> * Zapnout protokolování a metriky
> * Povolit výstrahy pro chyb autorizace
> * Spusťte test provoz s nesprávnou tokeny SAS
> * Stáhněte si a analyzovat protokoly

Tento odkaz zobrazíte ukázky předdefinovaných úložiště.

> [!div class="nextstepaction"]
> [Ukázky skriptu úložiště Azure](storage-samples-blobs-cli.md)