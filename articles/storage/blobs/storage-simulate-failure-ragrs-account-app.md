---
title: "Simulace selhání při přístupu ke redundantní úložiště s přístupem pro čtení v Azure | Microsoft Docs"
description: "Simulovat k chybě při přístupu ke geograficky redundantní úložiště s přístupem pro čtení"
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
ms.date: 12/05/2017
ms.author: gwallace
ms.custom: mvc
ms.openlocfilehash: f709e216f9308c4405776b25ca44b0aaddd3d3f8
ms.sourcegitcommit: d247d29b70bdb3044bff6a78443f275c4a943b11
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 12/13/2017
---
# <a name="simulate-a-failure-in-accessing-read-access-redundant-storage"></a>Simulace selhání při přístupu ke redundantní úložiště s přístupem pro čtení

V tomto kurzu je součástí série, dva. V tomto kurzu můžete vložit selhání odpověď s Fiddler pro požadavky na [čtení geograficky redundantní](../common/storage-redundancy.md#read-access-geo-redundant-storage) účet úložiště (RA-GRS) a nasimulujte selhání aplikace čtení ze sekundárního koncového bodu.

![Scénář aplikace](media/storage-simulate-failure-ragrs-account-app/scenario.png)

V rámci dvě řady, zjistíte, jak:

> [!div class="checklist"]
> * Spuštění a pozastavení aplikace
> * Simulace selhání
> * Simulovat primární koncový bod obnovení

## <a name="prerequisites"></a>Požadavky

Pro absolvování tohoto kurzu potřebujete:

* Stáhněte a nainstalujte [Fiddler](https://www.telerik.com/download/fiddler)

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

K dokončení tohoto kurzu, je nutné provést předchozí kurzu úložiště: [zajistit vysokou dostupnost s Azure storage data aplikací][previous-tutorial].

## <a name="launch-fiddler"></a>Spusťte aplikaci fiddler

Otevřete aplikaci Fiddler, vyberte **pravidla** a **přizpůsobit pravidla**.

![Přizpůsobení Fiddler pravidla](media/storage-simulate-failure-ragrs-account-app/figure1.png)

Spustí aplikaci Fiddler ScriptEditor zobrazující **SampleRules.js** souboru. Tento soubor se používá k přizpůsobení aplikaci Fiddler. Vložte následující ukázka kódu v `OnBeforeResponse` funkce. Nový kód je označeno jako komentář k zajištění logiku, který vytváří není provádění okamžitě. Po dokončení vyberte **soubor** a **Uložit** uložte provedené změny.

```javascript
    /*
        // Simulate data center failure
        // After it is successfully downloading the blob, pause the code in the sample,
        // uncomment these lines of script, and save the script.
        // It will intercept the (probably successful) responses and send back a 503 error. 
        // When you're ready to stop sending back errors, comment these lines of script out again 
        //     and save the changes.

        if ((oSession.hostname == "contosoragrs.blob.core.windows.net") 
            && (oSession.PathAndQuery.Contains("HelloWorld"))) {
            oSession.responseCode = 503;  
        }
    */
```

![Vložení vlastní pravidlo](media/storage-simulate-failure-ragrs-account-app/figure2.png)

## <a name="start-and-pause-the-application"></a>Spuštění a pozastavení aplikace

V sadě Visual Studio, stiskněte klávesu **F5** nebo vyberte **spustit** spustit ladění aplikace. Jakmile aplikace zahájí čtení z primární koncový bod, stiskněte klávesu **libovolné klávesy** v okně konzoly pozastavit aplikace.

## <a name="simulate-failure"></a>Simulace selhání

S aplikací pozastavena, můžete nyní zrušte komentář u vlastní pravidlo jsme vám uložili v aplikaci Fiddler předchozí krok. Tento kód ukázkové vypadá pro požadavky na účet úložiště RA-GRS a pokud cesta obsahuje název bitové kopie, `HelloWorld`, vrátí kód odpovědi `503 - Service Unavailable`.

Přejděte na aplikaci Fiddler a vyberte **pravidla** -> **přizpůsobit pravidla...** .  Zrušte komentář u následující řádky, nahraďte `STORAGEACCOUNTNAME` s názvem účtu úložiště. Vyberte **soubor** -> **Uložit** uložte provedené změny.

```javascript
         if ((oSession.hostname == "STORAGEACCOUNTNAME.blob.core.windows.net")
         && (oSession.PathAndQuery.Contains("HelloWorld"))) {
         oSession.responseCode = 503;
         }
```

Chcete-li obnovit aplikace, stiskněte **libovolné klávesy** .

Jakmile se aplikace spustí znovu spustit, požadavky na primární koncový bod začne být neúspěšné. Aplikace se pokusí znovu připojit k primární koncový bod 5krát. Po prahové hodnoty chyb pět pokusů požadavků bitovou kopii z sekundární koncový bod jen pro čtení. Po aplikace úspěšně načte bitovou kopii 20krát ze sekundárního koncového bodu, aplikace se pokusí připojit k primární koncový bod. Pokud primární koncový bod je stále nedostupný, obnoví aplikace čtení ze sekundárního koncového bodu. Tento vzor je [jistič](/azure/architecture/patterns/circuit-breaker.md) vzor popsané v předchozí kurzu.

![Vložení vlastní pravidlo](media/storage-simulate-failure-ragrs-account-app/figure3.png)

## <a name="simulate-primary-endpoint-restoration"></a>Simulovat primární koncový bod obnovení

S Fiddler vlastní pravidlo nastavené v předchozím kroku požadavky na primární koncový bod selžou. Chcete-li simulovat primární koncový bod znovu funguje, odeberete logiku vložení `503` chyby.

Pozastavit aplikace, stiskněte klávesu **libovolné klávesy**.

### <a name="remove-the-custom-rule"></a>Odebrat vlastní pravidlo

Přejděte na aplikaci Fiddler a vyberte **pravidla** a **přizpůsobit pravidla...** .  Komentář nebo odebrat vlastní logika `OnBeforeResponse` funkce, ponechat výchozí funkce. Vyberte **soubor** a **Uložit** a uložte změny.

![Odebrat vlastní pravidlo](media/storage-simulate-failure-ragrs-account-app/figure5.png)

Po dokončení stiskněte **libovolné klávesy** obnovit aplikace. Aplikace pokračuje čtení z primární koncový bod, dokud volání 999 čtení.

![Obnovit aplikace](media/storage-simulate-failure-ragrs-account-app/figure4.png)

## <a name="next-steps"></a>Další kroky

V rámci dvě řady jste se dozvěděli o simulaci selhání při testování geograficky redundantní úložiště přístup pro čtení, například jak:

> [!div class="checklist"]
> * Spuštění a pozastavení aplikace
> * Simulace selhání
> * Simulovat primární koncový bod obnovení

Tento odkaz zobrazíte ukázky předdefinovaných úložiště.

> [!div class="nextstepaction"]
> [Ukázky skriptu úložiště Azure](storage-samples-blobs-cli.md)

[previous-tutorial]: storage-create-geo-redundant-storage.md