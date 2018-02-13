---
title: "Shromážděte protokoly aktivity Azure do analýzy protokolů napříč předplatná | Microsoft Docs"
description: "Pomocí služby Event Hubs a aplikacích logiky shromažďovat data z protokol činnosti Azure a jeho odeslání pracovní prostor analýzy protokolů Azure v různých klienta."
services: log-analytics, logic-apps, event-hubs
documentationcenter: 
author: richrundmsft
manager: carmonm
editor: 
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 01/08/2018
ms.author: richrund; bwren
ms.openlocfilehash: ded0b4cdcbac747d52435023a24b5719f3c58758
ms.sourcegitcommit: be9a42d7b321304d9a33786ed8e2b9b972a5977e
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/19/2018
---
# <a name="collect-azure-activity-logs-into-log-analytics-across-subscriptions"></a>Shromážděte protokoly aktivity Azure do analýzy protokolů napříč odběrů

Tento článek obsahuje kroky prostřednictvím metody shromažďování protokolů Azure aktivity do pracovního prostoru analýzy protokolů pro Logic Apps pomocí konektoru kolektor dat analýzy protokolů Azure. Používejte postup v tomto článku, když potřebujete odeslání protokolů do pracovního prostoru v různých Azure Active Directory. Například pokud jste poskytovatel spravované služby, můžete chtít shromažďovat protokoly aktivity z předplatného zákazníka a uložit je do pracovního prostoru analýzy protokolů v svého vlastního předplatného.

Pokud pracovní prostor analýzy protokolů stejného předplatného Azure, nebo v jiném předplatném, avšak v Azure Active Directory, postupujte podle kroků v [řešení Azure aktivity protokolu](../log-analytics/log-analytics-activity.md) shromažďovat protokoly Azure aktivity.

## <a name="overview"></a>Přehled

Strategie použitá v tomto scénáři je tak, aby měl odesílat události do protokolu činnosti Azure [centra událostí](../event-hubs/event-hubs-what-is-event-hubs.md) kde [aplikace logiky](../logic-apps/logic-apps-overview.md) je odešle do pracovního prostoru analýzy protokolů. 

![Obrázek toku dat z aktivity protokolu do protokolu analýzy](media/log-analytics-activity-logs-subscriptions/data-flow-overview.png)

Výhody tohoto přístupu:
- Chcete-li s nízkou latencí vzhledem k tomu, že je protokol činnosti Azure streamování do centra událostí.  Aplikace logiky se pak spustí a odešle data k analýze protokolů. 
- Je požadován minimální kód a neexistuje žádná serverová infrastruktura nasazení.

Tento článek vás provede jednotlivými kroky postup:
1. Vytvoření centra událostí. 
2. Exportujte protokoly aktivity do centra událostí pomocí profilu exportu protokol činnosti Azure.
3. Vytvoření aplikace logiky ke čtení z centra událostí a odesílat události k analýze protokolů.

## <a name="requirements"></a>Požadavky
Toto jsou požadavky na prostředky Azure používá v tomto scénáři.

- Obor názvů centra událostí nemá být ve stejném předplatném jako předplatné emitování protokoly. Uživatel, který konfiguruje nastavení, musí mít příslušná přístupová oprávnění pro oba odběry. Pokud máte více předplatných ve stejné službě Azure Active directory, můžete odeslat protokoly aktivity pro všechna předplatná k rozbočovači jedna událost.
- Aplikace logiky může být v jiném předplatném. z centra událostí a nemusí být v Azure Active Directory. Aplikace logiky přečte z centra událostí pomocí centra událostí sdílený přístupový klíč.
- Pracovní prostor analýzy protokolů může být v jiné předplatné a Azure Active Directory z aplikace logiky, ale pro jednoduchost doporučujeme, aby byly ve stejném předplatném. Aplikace logiky odešle k analýze protokolů pomocí ID pracovního prostoru analýzy protokolů a klíče.



## <a name="step-1---create-an-event-hub"></a>Krok 1 – vytvoření centra událostí

<!-- Follow the steps in [how to create an Event Hubs namespace and Event Hub](../event-hubs/event-hubs-create.md) to create your event hub. -->

1. Na portálu Azure vyberte **nový**> **Internet věcí** > **Event Hubs**.

   ![Marketplace nového centra událostí](media/log-analytics-activity-logs-subscriptions/marketplace-new-event-hub.png)

3. V části **vytvoření oboru názvů**, buď zadejte nový obor názvů nebo vyberte nějaký existující. Systém okamžitě kontroluje, jestli je název dostupný.

   ![dialog centra událostí vytvořit bitovou kopii](media/log-analytics-activity-logs-subscriptions/create-event-hub1.png)

4. Vyberte cenovou úroveň (Basic nebo Standard), předplatné, skupinu prostředků a umístění pro nový prostředek.  Kliknutím na **Vytvořit** vytvoříte obor názvů. Možná budete muset počkat několik minut, než systém plně zřízení prostředků.
6. Klikněte na obor názvů, který jste právě vytvořili, ze seznamu.
7. Vyberte **zásady sdíleného přístupu**a potom klikněte na **RootManageSharedAccessKey**.

   ![Obrázek zásady přístupu sdílené centra událostí](media/log-analytics-activity-logs-subscriptions/create-event-hub7.png)
   
8. Kliknutím na tlačítko kopírovat zkopírujte připojovací řetězec **RootManageSharedAccessKey** do schránky. 

   ![Obrázek event hub sdílený přístupový klíč](media/log-analytics-activity-logs-subscriptions/create-event-hub8.png)

9. Do dočasného umístění, jako je například Poznámkový blok zkopírujte název centra událostí a buď primární nebo sekundární centra událostí připojovací řetězec. Aplikace logiky vyžaduje, aby tyto hodnoty.  Pro připojovací řetězec centra událostí můžete použít **RootManageSharedAccessKey** připojovací řetězec nebo vytvořit samostatné.  Připojovací řetězec, který používáte musí začínat znakem `Endpoint=sb://` a pro zásadu, která má být **spravovat** zásada přístupu.


## <a name="step-2---export-activity-logs-to-event-hub"></a>Krok 2 – protokoly aktivity Export do centra událostí

Pokud chcete povolit vysílání datového proudu protokolu aktivit, vyberte Namespace centra událostí a zásady sdíleného přístupu pro tento obor názvů. Centra událostí je v daném oboru názvů vytvořena, když dojde k první nové aktivity protokolu události. 

Můžete použít na obor názvů události rozbočovače, který není ve stejném předplatném jako předplatné emitování protokoly, ale odběry musí být v Azure Active Directory. Uživatel, který konfiguruje nastavení, musí mít odpovídající RBAC pro přístup k oba odběry. 

1. Na portálu Azure vyberte **monitorování** > **protokol aktivit**.
3. Klikněte **exportovat** tlačítka v horní části stránky.

   ![Obrázek azure monitorování v navigačním panelu](media/log-analytics-activity-logs-subscriptions/activity-log-blade.png)

4. Vyberte **předplatné** exportovat z, a pak klikněte na **Vybrat vše** v **oblasti** rozevíracího seznamu vyberte události pro prostředky ve všech oblastech. Klikněte **exportovat do centra událostí** zaškrtávací políčko.
7. Klikněte na tlačítko **obor názvů sběrnice**a pak vyberte **předplatné** s centrem událostí **názvový prostor události rozbočovače**a **zásady název centra událostí**.

    ![Obrázek export na stránku centra událostí](media/log-analytics-activity-logs-subscriptions/export-activity-log2.png)

11. Klikněte na tlačítko **OK** a potom **Uložit** uložit tato nastavení. Nastavení se použije okamžitě do vašeho předplatného.

<!-- Follow the steps in [stream the Azure Activity Log to Event Hubs](../monitoring-and-diagnostics/monitoring-stream-activity-logs-event-hubs.md) to configure a log profile that writes activity logs to an event hub. -->

## <a name="step-3---create-logic-app"></a>Krok 3 – vytvoření aplikace logiky

Jakmile jsou protokoly aktivity zápisu do centra událostí, vytvoříte aplikaci logiky shromažďovat protokoly z centra událostí a jejich zápis k analýze protokolů.

Aplikace logiky zahrnuje následující položky:
- [Centra událostí konektor](https://docs.microsoft.com/connectors/eventhubs/) aktivační událost číst z centra událostí.
- A [analyzovat JSON akce](../logic-apps/logic-apps-content-type.md) k extrakci události JSON.
- A [tvoří akce](../logic-apps/logic-apps-workflow-actions-triggers.md#compose-action) a převeďte JSON na objekt.
- A [analýzy protokolů odesílat data konektor](https://docs.microsoft.com/connectors/azureloganalyticsdatacollector/) odeslat data k analýze protokolů.

   ![Obrázek přidání aktivační událost rozbočovače do aplikace logiky](media/log-analytics-activity-logs-subscriptions/log-analytics-logic-apps-activity-log-overview.png)

### <a name="logic-app-requirements"></a>Požadavky na aplikaci logiky
Před vytvořením aplikace logiky, ujistěte se, že máte následující informace z předchozích kroků:
- Název centra událostí
- Události rozbočovače připojovací řetězec (buď primární nebo sekundární) pro obor názvů centra událostí.
- ID pracovního prostoru analýzy protokolů
- Sdílený klíč analýzy protokolů

Chcete-li získat název a připojovací řetězec centra událostí, postupujte podle kroků v [oprávnění k oboru názvů zkontrolujte Event Hubs a najít připojovací řetězec](../connectors/connectors-create-api-azure-event-hubs.md#check-event-hubs-namespace-permissions-and-find-the-connection-string).


### <a name="create-a-new-blank-logic-app"></a>Vytvoření nové prázdné aplikace logiky

1. Na portálu Azure vyberte **vytvořit prostředek** > **Enterprise integrace** > **aplikace logiky**.

    ![Nové aplikace logiky Marketplace.](media/log-analytics-activity-logs-subscriptions/marketplace-new-logic-app.png)

2. Zadejte nastavení v následující tabulce.

    ![Vytvoření aplikace logiky](media/log-analytics-activity-logs-subscriptions/create-logic-app.png)

   |Nastavení | Popis  |
   |:---|:---|
   | Název           | Jedinečný název pro aplikaci logiky. |
   | Předplatné   | Vyberte předplatné Azure, která bude obsahovat aplikaci logiky. |
   | Skupina prostředků | Vyberte existující skupinu prostředků Azure nebo vytvořte novou aplikaci logiky. |
   | Umístění       | Vyberte oblast datového centra pro nasazení aplikace logiky. |
   | Log Analytics  | Vyberte, pokud se chcete přihlásit analýzy protokolů stav každé spuštění aplikace logiky.  |

    
3. Vyberte **Vytvořit**. Když **nasazení bylo úspěšné** zobrazí oznámení, klikněte na **přejděte k prostředku** otevřete aplikaci logiky.

4. V části **Šablony** zvolte **Prázdná aplikace logiky**. 

Návrhář aplikace logiky nyní zobrazuje dostupné konektory a jejich aktivační události, které používáte ke spuštění pracovního postupu aplikace logiky.

<!-- Learn [how to create a logic app](../logic-apps/quickstart-create-first-logic-app-workflow.md). -->

### <a name="add-event-hub-trigger"></a>Přidat aktivační událost centra událostí

1. Do vyhledávacího pole pro návrhář aplikace logiky, zadejte *služby event hubs* filtru. Vyberte aktivační událost **Event Hubs – když jsou události dostupné v Centru událostí**.

   ![Obrázek přidání aktivační událost rozbočovače do aplikace logiky](media/log-analytics-activity-logs-subscriptions/logic-apps-event-hub-add-trigger.png)

2. Pokud se zobrazí výzva k zadání pověření, připojte k oboru názvů služby Event Hubs. Zadejte název připojení a pak připojovací řetězec, který jste zkopírovali.  Vyberte **Vytvořit**.

   ![Obrázek připojení rozbočovače událostí přidání do aplikace logiky](media/log-analytics-activity-logs-subscriptions/logic-apps-event-hub-add-connection.png)

3. Po vytvoření připojení, upravte nastavení pro aktivační událost. Začněte výběrem **statistiky operační protokoly** z **název centra událostí** rozevíracího seznamu.

   ![Když jsou události dostupné dialogové okno](media/log-analytics-activity-logs-subscriptions/logic-apps-event-hub-read-events.png)

5. Rozbalte položku **zobrazit rozšířené možnosti** a změňte **typ obsahu** k *application/json*

   ![Dialogové okno Konfigurace centra událostí](media/log-analytics-activity-logs-subscriptions/logic-apps-event-hub-configuration.png)

### <a name="add-parse-json-action"></a>Přidat akci analyzovat JSON

Výstup z centra událostí obsahuje datovou část JSON s pole záznamů. [Analyzovat JSON](../logic-apps/logic-apps-content-type.md) akce slouží k extrakci právě pole záznamy pro odesílání k analýze protokolů.

1. Klikněte na tlačítko **nový krok** > **přidat akci**
2. Do vyhledávacího pole zadejte *analyzovat json* filtru. Vyberte akci, která **Data operací - analyzovat JSON**.

   ![Přidání analýzy json akce do aplikace logiky](media/log-analytics-activity-logs-subscriptions/logic-apps-add-parse-json-action.png)

3. Kliknutím na tlačítko ve **obsahu** pole a pak vyberte *textu*.

4. Zkopírujte a vložte následující schéma do **schématu** pole.  Toto schéma odpovídá výstup z akce centra událostí.  

   ![Analyzovat json dialogové okno](media/log-analytics-activity-logs-subscriptions/logic-apps-parse-json-configuration.png)

``` json-schema
{
    "properties": {
        "body": {
            "properties": {
                "ContentData": {
                    "type": "string"
                },
                "Properties": {
                    "properties": {
                        "ProfileName": {
                            "type": "string"
                        },
                        "x-opt-enqueued-time": {
                            "type": "string"
                        },
                        "x-opt-offset": {
                            "type": "string"
                        },
                        "x-opt-sequence-number": {
                            "type": "number"
                        }
                    },
                    "type": "object"
                },
                "SystemProperties": {
                    "properties": {
                        "EnqueuedTimeUtc": {
                            "type": "string"
                        },
                        "Offset": {
                            "type": "string"
                        },
                        "PartitionKey": {},
                        "SequenceNumber": {
                            "type": "number"
                        }
                    },
                    "type": "object"
                }
            },
            "type": "object"
        },
        "headers": {
            "properties": {
                "Cache-Control": {
                    "type": "string"
                },
                "Content-Length": {
                    "type": "string"
                },
                "Content-Type": {
                    "type": "string"
                },
                "Date": {
                    "type": "string"
                },
                "Expires": {
                    "type": "string"
                },
                "Location": {
                    "type": "string"
                },
                "Pragma": {
                    "type": "string"
                },
                "Retry-After": {
                    "type": "string"
                },
                "Timing-Allow-Origin": {
                    "type": "string"
                },
                "Transfer-Encoding": {
                    "type": "string"
                },
                "Vary": {
                    "type": "string"
                },
                "X-AspNet-Version": {
                    "type": "string"
                },
                "X-Powered-By": {
                    "type": "string"
                },
                "x-ms-request-id": {
                    "type": "string"
                }
            },
            "type": "object"
        }
    },
    "type": "object"
}
```

>[!TIP]
> Datová část ukázky můžete získat kliknutím **spustit** a podíváte se **nezpracovaná výstup** z centra událostí.  Pak můžete použít tento výstup s **datová část ukázky použít ke generování schématu** v **analyzovat JSON** aktivity ke generování schématu.

### <a name="add-compose-action"></a>Přidat nové akce
[Vytvářené](../logic-apps/logic-apps-workflow-actions-triggers.md#compose-action) akce trvá výstup JSON a vytvoří objekt, který můžete použít akce analýzy protokolů.

1. Klikněte na tlačítko **nový krok** > **přidat akci**
2. Typ *tvoří* pro filtr a potom vyberte akci **tvoří Data operací -**.

    ![Přidat nové akce](media/log-analytics-activity-logs-subscriptions/logic-apps-add-compose-action.png)

3. Klikněte **vstupy** pole a vyberte **textu** v části **analyzovat JSON** aktivity.


### <a name="add-log-analytics-send-data-action"></a>Přidat akci Odeslat Data Log Analytics
[Kolektor dat analýzy protokolů Azure](https://docs.microsoft.com/connectors/azureloganalyticsdatacollector/) akce trvá objekt z akce vytvářené a odešle ho k analýze protokolů.

1. Klikněte na tlačítko **nový krok** > **přidat akci**
2. Typ *protokolu analýzy* pro filtr a potom vyberte akci **kolekcí dat Azure Log Analytics - odeslat Data**.

   ![Přidání analýzy protokolů odeslat akce dat v aplikacích logiky](media/log-analytics-activity-logs-subscriptions/logic-apps-send-data-to-log-analytics-connector.png)

3. Zadejte název připojení a vložte **ID pracovního prostoru** a **klíč pracovního prostoru** pro pracovní prostor analýzy protokolů.  Klikněte na možnost **Vytvořit**.

   ![Přidává se připojení analýzy protokolů v aplikacích logiky](media/log-analytics-activity-logs-subscriptions/logic-apps-log-analytics-add-connection.png)

4. Po vytvoření připojení, upravte nastavení v následující tabulce. 

    ![Konfigurace akce odeslání dat](media/log-analytics-activity-logs-subscriptions/logic-apps-send-data-to-log-analytics-configuration.png)

   |Nastavení        | Hodnota           | Popis  |
   |---------------|---------------------------|--------------|
   |Text JSON žádosti  | **Výstup** z **vytvářené** akce | Načte záznamy z textu vytvářené akce. |
   | Název vlastního protokolu | AzureActivity | Název vlastního protokolu tabulky v analýzy protokolů pro uložení importovaných dat vytvořit. |
   | čas generované pole | time | Nevybírejte pole JSON pro **čas** – stačí zadat čas aplikace word. Pokud vyberete pole JSON návrháře umístí **odeslat Data** akce do *pro každou* smyčky, který neodpovídá tomu, co chcete. |




10. Klikněte na tlačítko **Uložit** se uložit změny, které jste udělali svou aplikaci logiky.

## <a name="step-4---test-and-troubleshoot-the-logic-app"></a>Krok 4 - Test a řešení potíží s aplikaci logiky
Pomocí pracovního postupu dokončení můžete zjistit v Návrháři k ověření, že funguje bez chyby.

V návrháři aplikace logiky, klikněte na tlačítko **spustit** k testování aplikace logiky. Každý krok v aplikaci logiky zobrazuje ikona stavu, s bílým zaškrtnutí zelený kroužek udávající úspěch.

   ![Testování aplikace logiky](media/log-analytics-activity-logs-subscriptions/test-logic-app.png)

Pokud chcete zobrazit podrobné informace na každý krok, klikněte na název kroku rozbalte ho. Klikněte na **zobrazit nezpracované vstupy** a **zobrazit nezpracované výstupy** zobrazíte další informace na základě dat přijatých a odeslaných při každém kroku.

## <a name="step-5---view-azure-activity-log-in-log-analytics"></a>Krok 5 – zobrazení Aktivita Azure přihlášení analýzy protokolů
Posledním krokem je kontrola pracovní prostor analýzy protokolů a ujistěte se, že data jsou shromažďována dle očekávání.

1. Na portálu Azure vyberte **analýzy protokolů**.
2. Vyberte pracovní prostor a potom **hledání protokolů** dlaždici.
3. V panelu vyhledávání dotazu zadejte `AzureActivity_CL` a klikněte na tlačítko Hledat. Pokud nebyl název vlastního protokolu *AzureActivity*, zadejte název, který jste zvolili a připojte `_CL`.

>[!NOTE]
> První čas odeslání nové vlastní protokol k analýze protokolů to může trvat až jednu hodinu, pro vlastní protokol na možné vyhledávat.

>[!NOTE]
> Protokoly aktivity se zapisují do vlastní tabulky a nezobrazovat v [protokol aktivit řešení](./log-analytics-activity.md).


![Testování aplikace logiky](media/log-analytics-activity-logs-subscriptions/log-analytics-results.png)

## <a name="next-steps"></a>Další postup

V tomto článku jste vytvořili aplikaci logiky číst protokoly aktivity Azure z centra událostí a jejich odeslání k analýze protokolů pro analýzu. Další informace o vizualizace dat v analýzy protokolů, včetně vytváření řídicích panelů, přečtěte si kurz pro vizualizovat data.

> [!div class="nextstepaction"]
> [Vizualizace kurzu protokolu vyhledávání dat](./log-analytics-tutorial-dashboards.md)
