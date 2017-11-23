---
title: "Azure SQL Data Warehouse – Automatizace výpočetních úrovní služby SQL Data Warehouse pomocí Azure Functions | Dokumentace Microsoftu"
description: "Použití funkcí Azure ke správě výpočetního výkonu datového skladu."
services: sql-data-warehouse
documentationcenter: NA
author: hirokib
manager: johnmac
editor: barbkess
ms.assetid: 52DFC191-E094-4B04-893F-B64D5828A901
ms.service: sql-data-warehouse
ms.devlang: NA
ms.topic: hero-article
ms.tgt_pltfrm: NA
ms.workload: data-services
ms.custom: quickstart
ms.date: 11/06/2017
ms.author: elbutter
ms.openlocfilehash: e2138f26a78338406b466bdd2aa6e756e602726d
ms.sourcegitcommit: c7215d71e1cdeab731dd923a9b6b6643cee6eb04
ms.translationtype: HT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/17/2017
---
# <a name="use-azure-functions-to-automate-sql-dw-compute-levels"></a>Automatizace výpočetních úrovní služby SQL Data Warehouse pomocí Azure Functions

Tento kurz vám ukáže, jak můžete pomocí Azure Functions spravovat výpočetní úrovně služby Azure SQL Data Warehouse. Tyto architektury se doporučuje používat se službou SQL Data Warehouse [optimalizovanou pro zajištění elasticity][Performance Tiers].

Abyste mohli používat aplikaci Azure Function App se službou SQL Data Warehouse, musíte ve stejném předplatném jako instanci datového skladu vytvořit [účet instančního objektu](https://docs.microsoft.com/en-us/azure/azure-resource-manager/resource-group-create-service-principal-portal) s přístupem Přispěvatel. 

## <a name="deploy-timer-based-scaler-with-an-azure-resource-manager-template"></a>Nasazení škálovače založeného na časovači pomocí šablony Azure Resource Manageru

K nasazení šablony budete potřebovat následující informace:

- Název skupiny prostředků, ve které je vaše instance SQL Data Warehouse
- Název logického serveru, na kterém je vaše instance SQL Data Warehouse
- Název vaší instance SQL Data Warehouse
- ID tenanta (ID adresáře) vaší služby Azure Active Directory
- ID předplatného 
- ID aplikace instančního objektu
- Tajný klíč instančního objektu

Až budete mít výše uvedené informace, nasaďte tuto šablonu:

<a href="https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FMicrosoft%2Fsql-data-warehouse-samples%2Fmaster%2Farm-templates%2FsqlDwTimerScaler%2Fazuredeploy.json" target="_blank">
<img src="https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/1-CONTRIBUTION-GUIDE/images/deploytoazure.png"/>
</a>

Po nasazení šablony byste měli mít tři nové prostředky: bezplatný plán služby Azure App Service, plán aplikace Function App založený na spotřebě a účet úložiště, který bude zpracovávat protokolování a frontu operací. Pokračujte a přečtěte si další části, ve kterých se dozvíte, jak upravit nasazené funkce, aby vyhovovaly vašim požadavkům.

### <a name="change-the-scale-up-or-scale-down-compute-level"></a>Změna výpočetní úrovně vertikálního navýšení nebo snížení kapacity

1. Přejděte do své služby Function App. Pokud jste nasadili šablonu s výchozími hodnotami, měla by tato služba mít název *DWOperations*. Po otevření vaší aplikace Function App byste si měli všimnout, že do vaší služby Function App je nasazených pět funkcí. 

   ![Funkce nasazené s šablonou](media/manage-compute-with-azure-functions/five-functions.png)

2. Vyberte *DWScaleDownTrigger* nebo *DWScaleUpTrigger* podle toho, jestli chcete změnit čas vertikálního navýšení nebo snížení kapacity. V rozevírací nabídce vyberte Integrace.

   ![Výběr možnosti Integrace pro funkci](media/manage-compute-with-azure-functions/select-integrate.png)

3. Momentálně by zobrazená hodnota měla být *%ScaleDownTime%* nebo *%ScaleUpTime%*. Tyto hodnoty označují, že je plán založený na hodnotách definovaných ve vašem [Nastavení aplikace][Application Settings]. Prozatím to můžete ignorovat a plán změnit na preferovaný čas podle dalších kroků.

4. V oblasti plánu přidejte libovolný čas v podobě výrazu CRON, který odráží, jak často chcete vertikálně navyšovat kapacitu služby SQL Data Warehouse. 

  ![Změna plánu funkce](media/manage-compute-with-azure-functions/change-schedule.png)

  Hodnota `schedule` je [výraz CRON](http://en.wikipedia.org/wiki/Cron#CRON_expression), který zahrnuje těchto šest polí: 
  ```json
  {second} {minute} {hour} {day} {month} {day-of-week}
  ```

  Například hodnota *0 30 9 * * 1-5* by odpovídala triggeru aktivovanému každý všední den v 9:30. Další informace najdete v [příkladech plánů][schedule examples] služby Azure Functions.


### <a name="change-the-scale-up-or-scale-down-time"></a>Změna času vertikálního navýšení nebo snížení kapacity

1. Přejděte do své služby Function App. Pokud jste nasadili šablonu s výchozími hodnotami, měla by tato služba mít název *DWOperations*. Po otevření vaší aplikace Function App byste si měli všimnout, že do vaší služby Function App je nasazených pět funkcí. 

2. Vyberte *DWScaleDownTrigger* nebo *DWScaleUpTrigger* podle toho, jestli chcete změnit hodnotu výpočetního výkonu vertikálního navýšení nebo snížení kapacity. Po výběru funkcí by se ve vašem podokně měl zobrazit soubor *index.js*.

   ![Změna výpočetní úrovně triggeru funkce](media/manage-compute-with-azure-functions/index-js.png)

3. Změňte hodnotu *ServiceLevelObjective* na požadovanou úroveň a klikněte na Uložit. To je výpočetní úroveň, na kterou se vaše instance datového skladu bude škálovat podle plánu definovaného v části Integrace.

### <a name="use-pause-or-resume-instead-of-scale"></a>Použití pozastavení nebo obnovení místo škálování 

Aktuálně jsou ve výchozím nastavení zapnuté funkce *DWScaleDownTrigger* a *DWScaleUpTrigger*. Pokud místo toho chcete použít funkce pozastavení a obnovení, můžete povolit *DWPauseTrigger* nebo *DWResumeTrigger*.

1. Přejděte do podokna Funkce.

   ![Podokno Funkce](media/manage-compute-with-azure-functions/functions-pane.png)



2. Klikněte na posuvný přepínač u odpovídajících triggerů, které chcete povolit.

3. Přejděte na karty *Integrace* příslušných triggerů a změňte jejich plán.

   [!NOTE]: The functional difference between the scaling triggers and the pause/resume triggers is the message that is sent to the queue. See [Add a new trigger function][Add a new trigger function] for more information.



### <a name="add-a-new-trigger-function"></a>Přidání nové funkce triggeru

Aktuálně jsou součástí šablony pouze dvě škálovací funkce. To znamená, že v průběhu dne můžete vertikálně snížit i zvýšit kapacitu pouze jednou. Pokud chcete mít podrobnější kontrolu, například vertikálně snižovat kapacitu několikrát denně nebo mít jiné chování škálování o víkendech, budete muset přidat další trigger.

1. Vytvořte novou prázdnou funkci. Vyberte tlačítko *+* vedle umístění Funkce a zobrazte tak podokno šablony funkce.

   ![Vytvoření nové funkce](media/manage-compute-with-azure-functions/create-new-function.png)

2. Jako Jazyk vyberte *Javascript* a pak vyberte *TimerTrigger*.

   ![Vytvoření nové funkce](media/manage-compute-with-azure-functions/timertrigger-js.png)

3. Zadejte název funkce a nastavte plán. Tento obrázek ukazuje, jak aktivovat funkci každou sobotu o půlnoci (v noci z pátku).

   ![Vertikální snížení kapacity v sobotu](media/manage-compute-with-azure-functions/scale-down-saturday.png)

4. Zkopírujte obsah souboru *index.js* z některé jiné funkce triggeru.

   ![Zkopírování obsahu souboru index.js](media/manage-compute-with-azure-functions/index-js.png)

5. Nastavte proměnnou operation na požadované chování následujícím způsobem:

   ```javascript
   // Resume the data warehouse instance
   var operation = {
       "operationType": "ResumeDw"
   }

   // Pause the data warehouse instance
   var operation = {
       "operationType": "PauseDw"
   }

   // Scale the data warehouse instance to DW600
   var operation = {
       "operationType": "ScaleDw",
       "ServiceLevelObjective": "DW600"
   }
   ```


### <a name="complex-scheduling"></a>Složité plánování

Tato část vám stručně ukáže, co je potřeba k dosažení složitějšího plánování možností pozastavení, obnovení a škálování.

#### <a name="example-1"></a>Příklad 1:

Každodenní vertikální navýšení kapacity v 8:00 na úroveň DW600 a vertikální snížení kapacity ve 20:00 na úroveň DW200.

| Funkce  | Plán     | Operace                                |
| :-------- | :----------- | :--------------------------------------- |
| Funkce1 | 0 0 8 * * *  | `var operation = {"operationType": "ScaleDw",  "ServiceLevelObjective": "DW600"}` |
| Funkce2 | 0 0 20 * * * | `var operation = {"operationType": "ScaleDw", "ServiceLevelObjective": "DW200"}` |

#### <a name="example-2"></a>Příklad 2: 

Každodenní vertikální navýšení kapacity v 8:00 na úroveň DW1000, jedno vertikální snížení kapacity na úroveň DW600 v 16:00 a vertikální snížení kapacity ve 22:00 na úroveň DW200.

| Funkce  | Plán     | Operace                                |
| :-------- | :----------- | :--------------------------------------- |
| Funkce1 | 0 0 8 * * *  | `var operation = {"operationType": "ScaleDw",  "ServiceLevelObjective": "DW1000"}` |
| Funkce2 | 0 0 16 * * * | `var operation = {"operationType": "ScaleDw", "ServiceLevelObjective": "DW600"}` |
| Funkce3 | 0 0 22 * * * | `var operation = {"operationType": "ScaleDw", "ServiceLevelObjective": "DW200"}` |

#### <a name="example-3"></a>Příklad 3: 

Vertikální navýšení kapacity v 8:00 na úroveň DW1000 a jedno vertikální snížení kapacity na úroveň DW600 v 16:00 ve všední dny. Pozastavení v pátek ve 23:00 a obnovení v pondělí v 7:00 ráno.

| Funkce  | Plán       | Operace                                |
| :-------- | :------------- | :--------------------------------------- |
| Funkce1 | 0 0 8 * * 1-5  | `var operation = {"operationType": "ScaleDw",    "ServiceLevelObjective": "DW1000"}` |
| Funkce2 | 0 0 16 * * 1-5 | `var operation = {"operationType": "ScaleDw", "ServiceLevelObjective": "DW600"}` |
| Funkce3 | 0 0 23 * * 5   | `var operation = {"operationType": "PauseDw"}` |
| Funkce4 | 0 0 7 * * 0    | `var operation = {"operationType": "ResumeDw"}` |



## <a name="next-steps"></a>Další kroky

Přečtěte si další informace o [triggeru časovače](../azure-functions/functions-create-scheduled-function.md) pro funkce Azure.

Podívejte se na [úložiště ukázek](https://github.com/Microsoft/sql-data-warehouse-samples) pro službu SQL Data Warehouse.



[schedule examples]: ../azure-functions/functions-bindings-timer.md#example

[Application Settings]: ../azure-functions/functions-how-to-use-azure-function-app-settings.md
[Add a new trigger function]: manage-compute-with-azure-functions.md#add-a-new-trigger-function

[Performance Tiers]: performance-tiers.md
