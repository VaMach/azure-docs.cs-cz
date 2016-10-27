<properties
   pageTitle="Interakce se sestavami pomocí rozhraní API pro JavaScript | Microsoft Azure"
   description="Power BI Embedded, interakce se sestavami pomocí rozhraní API pro JavaScript"
   services="power-bi-embedded"
   documentationCenter=""
   authors="guyinacube"
   manager="erikre"
   editor=""
   tags=""/>
<tags
   ms.service="power-bi-embedded"
   ms.devlang="NA"
   ms.topic="hero-article"
   ms.tgt_pltfrm="NA"
   ms.workload="powerbi"
   ms.date="10/04/2016"
   ms.author="asaxton"/>


# Interakce se sestavami Power BI pomocí rozhraní API pro JavaScript

Rozhraní API pro JavaScript Power BI umožňuje snadné vkládání sestav Power BI do vašich aplikací. Prostřednictvím tohoto rozhraní API mohou vaše aplikace programově interagovat s různými elementy sestav, jako jsou stránky a filtry. Díky této interaktivitě se sestavy Power BI stanou integrálnější součástí vaší aplikace.

Ke vložení sestavy Power BI do své aplikace použijete element iframe, který je hostovaný jako součást aplikace. Element iframe slouží jako hranice mezi vaší aplikací a sestavou, jak je vidět na následujícím obrázku. 

![Vložený element iframe Power BI bez rozhraní API pro Javascript](media\powerbi-embedded-interact-with-reports\powerbi-embedded-interact-report-1.png)

Element iframe sice proces vkládání dost usnadňuje, ale bez rozhraní API pro JavaScript API spolu sestava a aplikace nemohou interagovat. Tento nedostatek interakce potom může mít za následek pocit, že sestava vlastně není součástí příslušné aplikace. Sestava a aplikace skutečně potřebují vzájemně komunikovat, jak ukazuje následující obrázek.

![Vložený element iframe Power BI s rozhraním API pro Javascript](media\powerbi-embedded-interact-with-reports\powerbi-embedded-interact-report-2.png)

Rozhraní API pro JavaScript Power BI umožňuje psát kód, který bezpečně prochází hranicí elementu iframe. Díky tomu aplikace může programově provádět v sestavě různé akce a naslouchat událostem z akcí, které uživatelé v této sestavě provedou.

## K čemu je možné využít rozhraní API pro JavaScript Power BI?
Pomocí rozhraní API pro JavaScript API můžete spravovat sestavy, přejít na stránky v sestavě, filtrovat sestavu a zpracovávat události vkládání. Strukturu rozhraní API ukazuje následující diagram.

![Diagram rozhraní API pro JavaScript Power BI](media\powerbi-embedded-interact-with-reports\powerbi-embedded-interact-report-3.png)


### Správa sestav
Rozhraní API pro Javascript umožňuje spravovat chování na úrovni sestav a stránek:

- Zabezpečené vložení konkrétní sestavy Power BI do vaší aplikace – vyzkoušejte si [ukázkovou aplikaci s vložením](http://azure-samples.github.io/powerbi-angular-client/#/scenario1)
  - Nastavení přístupového tokenu
- Konfigurace sestavy
  - Povolení a zákaz podokna filtru a podokna navigace stránkami – vyzkoušejte si [ukázkovou aplikaci s aktualizací nastavení](http://azure-samples.github.io/powerbi-angular-client/#/scenario6)
  - Nastavení výchozích hodnot pro stránky a filtry – vyzkoušejte si [ukázku nastavení výchozích hodnot](http://azure-samples.github.io/powerbi-angular-client/#/scenario5)
- Nastavení a ukončení režimu zobrazení na celé obrazovce

[Další informace o vkládání sestav](https://github.com/Microsoft/PowerBI-JavaScript/wiki/Embedding-Basics)


### Přechod na stránky v sestavě
Rozhraní API pro JavaScript umožňuje prohlédnout všechny stránky sestavy a nastavit aktuální stránku. Vyzkoušejte si [ukázkovou aplikaci s navigací](http://azure-samples.github.io/powerbi-angular-client/#/scenario3).

[Další informace o navigaci stránkami](https://github.com/Microsoft/PowerBI-JavaScript/wiki/Page-Navigation)

### Filtrování sestavy
Rozhraní API pro JavaScript poskytuje základní a rozšířené možnosti filtrování pro vložené sestavy a stránky sestav. Vyzkoušejte si [ukázkovou aplikaci filtrování](http://azure-samples.github.io/powerbi-angular-client/#/scenario4) a prohlédněte si základní kód.  


#### Základní filtry
Základní filtr je postavený na úrovni hierarchie nebo sloupce a obsahuje seznam hodnot, které se mají zahrnout nebo vyloučit.

```
const basicFilter: pbi.models.IBasicFilter = {
  $schema: "http://powerbi.com/product/schema#basic",
  target: {
    table: "Store",
    column: "Count"
  },
  operator: "In",
  values: [1,2,3,4]
}
```


#### Rozšířené filtry
Rozšířené filtry využívají logické operátory AND a OR a přijímají jednu nebo dvě podmínky. Každá z těchto podmínek má vlastní operátor a hodnotu. Podporované podmínky:

- Žádný
- LessThan
- LessThanOrEqual
- GreaterThan
- GreaterThanOrEqual
- Contains
- DoesNotContain
- StartsWith
- DoesNotStartWith
- Is
- IsNot
- IsBlank
- IsNotBlank

```
const advancedFilter: pbi.models.IAdvancedFilter = {
  $schema: "http://powerbi.com/product/schema#advanced",
  target: {
    table: "Store",
    column: "Name"
  },
  logicalOperator: "Or",
  conditions: [
    {
      operator: "Contains",
      value: "Wash"
    },
    {
      operator: "Contains",
      value: "Park"
    }
  ]
}
```
[Další informace o filtrování](https://github.com/Microsoft/PowerBI-JavaScript/wiki/Filters)


### Zpracování událostí
Kromě odesílání informací do elementu iframe může aplikace také přijímat informace o následujících událostech, jejichž původcem je iframe:

- Embed
  - loaded
  - error
- Reports
  - pageChanged
  - dataSelected (už brzy)

[Další informace o zpracování událostí](https://github.com/Microsoft/PowerBI-JavaScript/wiki/Handling-Events)


## Další kroky
Další informace o rozhraní API pro JavaScript Power BI najdete pod následujícími odkazy:

- [Wikiweb rozhraní API pro JavaScript](https://github.com/Microsoft/PowerBI-JavaScript/wiki)
- [Referenční informace k objektovému modelu](https://microsoft.github.io/powerbi-models/modules/_models_.html)
- Ukázky
  - [Úhlová](http://azure-samples.github.io/powerbi-angular-client)
  - [Ember](https://github.com/Microsoft/powerbi-ember)
- [Živá ukázka](https://microsoft.github.io/PowerBI-JavaScript/demo/)



<!--HONumber=Oct16_HO3-->


