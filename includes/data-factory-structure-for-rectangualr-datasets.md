## <a name="specifying-structure-definition-for-rectangular-datasets"></a>Zadání definice struktury obdélníková datové sady
V části struktura v datových sadách JSON je **volitelné** část obdélníková tabulky (s řádky a sloupce) a obsahuje kolekci sloupců pro tabulku. V části struktura budete používat pro buď typ informacemi pro převody typů nebo provádění mapování sloupce. Následující části popisují tyto funkce podrobně. 

Všechny sloupce obsahují následující vlastnosti:

| Vlastnost | Popis | Požaduje se |
| --- | --- | --- |
| jméno |Název sloupce. |Ano |
| type |Datový typ sloupce. Najdete v části převody typu níže pro další podrobnosti týkající se kdy by měl můžete určit informace o typu |Ne |
| Jazyková verze |.NET na základě jazykové verze má být použita, pokud je zadaný typ a typ formátu .NET Datetime nebo Datetimeoffset. Výchozí hodnota je "en-us". |Ne |
| Formát |Řetězec formátu, který se má použít, když je zadaný typ a .NET typ Datetime nebo Datetimeoffset. |Ne |

Následující příklad ukazuje oddílu struktura JSON pro tabulku, která má tři sloupce ID uživatele, název a lastlogindate.

```json
"structure": 
[
    { "name": "userid"},
    { "name": "name"},
    { "name": "lastlogindate"}
],
```

Použijte následující pokyny pro kdy "struktury" informace a co mají být zahrnuty **struktura** části.

* **Pro strukturovaná data zdroje** , data schématu a zadejte informace o úložišti spolu s daty, samotné (zdrojů jako tabulky Azure SQL Server, Oracle, atd.), musíte zadat v části "struktura" pouze v případě, že chcete provádět mapování sloupců z konkrétního zdroje na konkrétní sloupce v jímka a jejich názvy sloupců nejsou stejné (viz podrobnosti v následující části mapování sloupců). 
  
    Jak je uvedeno nahoře, je v části "struktura" volitelné informace o typu. Strukturované zdroje informací o typu je již k dispozici jako součást definice datové sady v úložišti dat, proto by nemělo zahrnete informací o typu zahrnete v části "struktura".
* **Pro schéma pro zdroje dat pro čtení (konkrétně objektů blob v Azure)** můžete k ukládání dat bez ukládání žádné schéma nebo typ informace s daty. Pro tyto typy zdrojů dat by měla obsahovat "struktura" 2 následujících případech:
  * Chcete mapování sloupců.
  * Pokud je datová sada zdroji v aktivitě kopírování, můžete zadat informace o typu "struktury" a objektu pro vytváření dat bude používat tento typ informace pro převod na nativní typy pro jímky. V tématu [přesun dat do a z Azure Blob](../articles/data-factory/v1/data-factory-azure-blob-connector.md) Další informace najdete v článku.

### <a name="supported-net-based-types"></a>Podporováno. Na základě NET typy
Objekt pro vytváření dat podporuje následující specifikací CLS kompatibilní .NET na základě typu hodnoty pro poskytnutí informací o typu "struktury" pro schéma na zdrojů dat čtení objektů blob v Azure.

* Int16
* Int32 
* Int64
* Jeden
* Double
* Decimal
* Byte]
* BOOL
* Řetězec 
* Identifikátor GUID
* Data a času
* Datový typ DateTimeOffset
* Časový interval 

Pro hodnotu Datetime a Datetimeoffset můžete volitelně specifikovat řetězec "culture" & "format" usnadňuje analýzy vaše vlastní řetězce data a času. Viz ukázka pro převod typů níže.

