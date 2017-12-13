---
title: "Nastavit data aplikací jako vysoce dostupný v Azure | Microsoft Docs"
description: "Geograficky redundantní úložiště s přístupem pro čtení pomocí vysoce zpřístupnit data aplikací"
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
ms.date: 11/15/2017
ms.author: gwallace
ms.custom: mvc
ms.openlocfilehash: 286013aaa5335689206514027bef80b250643be1
ms.sourcegitcommit: d247d29b70bdb3044bff6a78443f275c4a943b11
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 12/13/2017
---
# <a name="make-your-application-data-highly-available-with-azure-storage"></a>Zajistit vysokou dostupnost s Azure storage data aplikací

V tomto kurzu je součástí, jednu z řady. V tomto kurzu se dozvíte, jak chcete nastavit vysokou dostupnost v Azure data aplikací. Jakmile budete hotovi, máte základní konzolovou aplikaci .NET, která odešle a načte objekt blob [čtení geograficky redundantní](../common/storage-redundancy.md#read-access-geo-redundant-storage) účet úložiště (RA-GRS). RA-GRS funguje tak, že replikace transakce z primárního na sekundární oblast. Tento proces replikace zaručuje, že data v sekundární oblasti jsou nakonec byl konzistentní. Aplikace používá [jistič](/azure/architecture/patterns/circuit-breaker.md) vzor k určení kterému koncovému bodu se připojit k. Aplikace se při selhání se simuluje přepne do sekundárního koncového bodu.

V rámci jedna řada, zjistíte, jak:

> [!div class="checklist"]
> * vytvořit účet úložiště
> * Stažení ukázky
> * Nastavení připojovacího řetězce
> * Spuštěním konzolové aplikace

## <a name="prerequisites"></a>Požadavky

K provedení kroků v tomto kurzu je potřeba:

* Nainstalovat [Visual Studio 2017](https://www.visualstudio.com/downloads/) s následujícími sadami funkcí:
  - **Azure – vývoj**

  ![Vývoj pro Azure (v části Web a Cloud)](media/storage-create-geo-redundant-storage/workloads.png)

* Stáhněte a nainstalujte [Fiddler](https://www.telerik.com/download/fiddler)

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="log-in-to-the-azure-portal"></a>Přihlášení k portálu Azure Portal

Přihlaste se k portálu [Azure Portal](https://portal.azure.com/).

## <a name="create-a-storage-account"></a>vytvořit účet úložiště

Účet úložiště poskytuje jedinečný obor názvů pro ukládání a přístup k vaší datové objekty Azure storage.

Postupujte podle těchto kroků můžete vytvořit účet geograficky redundantní úložiště s přístupem pro čtení:

1. Vyberte **nový** nalezeno tlačítko v levém horním rohu portálu Azure.

2. Vyberte **úložiště** z **nový** a vyberte **účet úložiště – objekt blob, soubor, tabulka, fronta** pod **doporučený**.
3. Vyplňte formulář účet úložiště s následující informace, jak ukazuje následující obrázek a vyberte **vytvořit**:

   | Nastavení       | Navrhovaná hodnota | Popis |
   | ------------ | ------------------ | ------------------------------------------------- |
   | **Název** | můj_účet_úložiště | Jedinečnou hodnotu pro váš účet úložiště |
   | **Model nasazení** | Resource Manager  | Správce prostředků obsahuje nejnovější funkce.|
   | **Typ účtu** | Obecné účely | Podrobnosti o typech účtů najdete v tématu [typy účtů úložiště](../common/storage-introduction.md#types-of-storage-accounts) |
   | **Výkon** | Standard | Ukázkový scénář stačí Standard. |
   | **Replikace**| Geograficky redundantní úložiště s přístupem pro čtení (RA-GRS) | To je nezbytné pro ukázku pracovat. |
   |**Požadované zabezpečení přenosu** | Zakázáno| Zabezpečení přenosu se pro tento scénář nevyžaduje. |
   |**Předplatné** | Vaše předplatné |Podrobnosti o vašich předplatných najdete v tématu [Předplatná](https://account.windowsazure.com/Subscriptions). |
   |**Skupina prostředků** | myResourceGroup |Platné názvy skupin prostředků najdete v tématu [Pravidla a omezení pojmenování](https://docs.microsoft.com/azure/architecture/best-practices/naming-conventions). |
   |**Umístění** | Východ USA | Vyberte umístění. |

![Vytvořit účet úložiště](media/storage-create-geo-redundant-storage/figure1.png)

## <a name="download-the-sample"></a>Stažení ukázky

[Stáhněte si ukázkový projekt](https://github.com/Azure-Samples/storage-dotnet-circuit-breaker-pattern-ha-apps-using-ra-grs/archive/master.zip).

Extrahování (rozbalte) storage-dotnet-circuit-breaker-pattern-ha-apps-using-ra-grs.zip souboru.
Obsahuje ukázkový projekt konzolové aplikace.

## <a name="set-the-connection-string"></a>Nastavení připojovacího řetězce

V aplikaci je nutné zadat připojovací řetězec pro váš účet úložiště. Doporučujeme uložit tento připojovací řetězec v rámci proměnné prostředí v místním počítači spuštění aplikace. Proveďte jeden z příkladů níže v závislosti na operačním systému vytvořte proměnnou prostředí.

Na portálu Azure přejděte do účtu úložiště. Vyberte **přístupové klíče** pod **nastavení** ve vašem účtu úložiště. Kopírování **připojovací řetězec** z primární nebo sekundární klíč. Nahraďte \<yourconnectionstring\> s samotný připojovací řetězec spuštěním jednoho z následujících příkazů podle operačního systému. Tento příkaz uloží proměnné prostředí v místním počítači. V systému Windows, není dostupná, dokud nebude znovu načíst proměnnou prostředí **příkazového řádku** nebo prostředí používáte. Nahraďte  **\<storageConnectionString\>**  následující ukázka:

### <a name="linux"></a>Linux

```bash
export storageconnectionstring=<yourconnectionstring>
```

### <a name="windows"></a>Windows

```cmd
setx storageconnectionstring "<yourconnectionstring>"
```

![soubor konfigurace aplikace](media/storage-create-geo-redundant-storage/figure2.png)

## <a name="run-the-console-application"></a>Spuštěním konzolové aplikace

V sadě Visual Studio, stiskněte klávesu **F5** nebo vyberte **spustit** spustit ladění aplikace. Visual studio automaticky obnovení chybějících balíčků NuGet pokud nakonfigurovaný, přejděte k [instalace a přeinstalace balíčky se obnovení balíčku](https://docs.microsoft.com/nuget/consume-packages/package-restore#package-restore-overview) Další informace.

Spustí okno konzoly a začne aplikace spuštěna. Aplikace odešle **HelloWorld.png** bitovou kopii z řešení k účtu úložiště. Aplikace zkontroluje, aby bitovou kopii replikoval na sekundární koncový bod RA-GRS. Poté zahájí stahování obrázku až 999 x. Každý pro čtení je reprezentována **P** nebo **S**. Kde **P** představuje primární koncový bod a **S** reprezentuje sekundární koncový bod.

![Konzolové aplikace spuštěná](media/storage-create-geo-redundant-storage/figure3.png)

V ukázkovém kódu `RunCircuitBreakerAsync` úloh v `Program.cs` souboru se používá ke stahování bitovou kopii pomocí účtu úložiště [DownloadToFileAsync](/dotnet/api/microsoft.windowsazure.storage.blob.cloudblockblob.downloadtofileasync?view=azure-dotnet) metoda. Před stahování [informacím OperationContext](/dotnet/api/microsoft.windowsazure.storage.operationcontext?view=azure-dotnet) je definována. Kontext operace definuje obslužné rutiny událostí, které budou spuštěny po úspěšném dokončení stahování nebo pokud stahování selže a je opakování.

### <a name="retry-event-handler"></a>Opakujte obslužné rutiny události

`OperationContextRetrying` Obslužné rutiny události je volána, když stažení bitové kopie selže a je nastavený na rety. Pokud se dosáhne maximálního počtu opakování pokusů, které jsou definovány v aplikaci, [LocationMode](/dotnet/api/microsoft.windowsazure.storage.blob.blobrequestoptions.locationmode?view=azure-dotnet#Microsoft_WindowsAzure_Storage_Blob_BlobRequestOptions_LocationMode) požadavku se změní na `SecondaryOnly`. Toto nastavení vynutí se aplikace pokusí stáhnout bitovou kopii z sekundární koncový bod. Tato konfigurace snižuje čas potřebný k požadují bitovou kopii jako primární koncový bod není opakovat po neomezenou dobu.

```csharp
private static void OperationContextRetrying(object sender, RequestEventArgs e)
{
    retryCount++;
    Console.WriteLine("Retrying event because of failure reading the primary. RetryCount = " + retryCount);

    // Check if we have had more than n retries in which case switch to secondary.
    if (retryCount >= retryThreshold)
    {

        // Check to see if we can fail over to secondary.
        if (blobClient.DefaultRequestOptions.LocationMode != LocationMode.SecondaryOnly)
        {
            blobClient.DefaultRequestOptions.LocationMode = LocationMode.SecondaryOnly;
            retryCount = 0;
        }
        else
        {
            throw new ApplicationException("Both primary and secondary are unreachable. Check your application's network connection. ");
        }
    }
}
```

### <a name="request-completed-event-handler"></a>Dokončit žádost o obslužnou rutinu události

`OperationContextRequestCompleted` Obslužné rutiny události je volána po úspěšné stažení bitové kopie. Pokud aplikace používá sekundární koncový bod, aplikace budou nadále používat tento koncový bod až 20 x. Po dobu 20, aplikace sady [LocationMode](/dotnet/api/microsoft.windowsazure.storage.blob.blobrequestoptions.locationmode?view=azure-dotnet#Microsoft_WindowsAzure_Storage_Blob_BlobRequestOptions_LocationMode) zpět na `PrimaryThenSecondary` a opakuje primární koncový bod. Pokud je požadavek úspěšné, aplikace bude číst z primární koncový bod.

```csharp
private static void OperationContextRequestCompleted(object sender, RequestEventArgs e)
{
    if (blobClient.DefaultRequestOptions.LocationMode == LocationMode.SecondaryOnly)
    {
        // You're reading the secondary. Let it read the secondary [secondaryThreshold] times, 
        //    then switch back to the primary and see if it's available now.
        secondaryReadCount++;
        if (secondaryReadCount >= secondaryThreshold)
        {
            blobClient.DefaultRequestOptions.LocationMode = LocationMode.PrimaryThenSecondary;
            secondaryReadCount = 0;
        }
    }
}
```

## <a name="next-steps"></a>Další kroky

V rámci jedna řada jste se dozvěděli o tom, jak aplikaci vysokou dostupností s účty úložiště RA-GRS, jako například:

> [!div class="checklist"]
> * vytvořit účet úložiště
> * Stažení ukázky
> * Nastavení připojovacího řetězce
> * Spuštěním konzolové aplikace

Přechodu na druhé části série se dozvíte, jak vynutit aplikace pomocí sekundárního koncového bodu RA-GRS a nasimulujte selhání.

> [!div class="nextstepaction"]
> [Simulace selhání připojení ke koncovému bodu vašeho primárního úložiště](storage-simulate-failure-ragrs-account-app.md)
