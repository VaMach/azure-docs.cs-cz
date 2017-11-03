## <a name="repeatability-during-copy"></a>Opakovatelnosti během kopírování
Při kopírování dat z a do relační úložiště, budete muset opakovatelnosti mějte na paměti, aby se zabránilo neúmyslnému výstupy. 

Řez může být znovu automaticky v Azure Data Factory podle určené zásady opakování. Doporučujeme nastavit zásady opakovaných pokusů pro ochranu proti přechodných chyb. Proto opakovatelnosti je důležitým aspektem abyste dbali během přesunu dat. 

**Jako zdroj:**

> [!NOTE]
> Následující ukázky jsou pro Azure SQL, ale platí pro všechny datové úložiště, které podporuje obdélníková datové sady. Možná budete muset upravit **typ** zdroje a **dotazu** vlastnosti (například: dotazu místo sqlReaderQuery) pro data uložit.   
> 
> 

Při čtení z relační úložiště, by obvykle chcete číst pouze data odpovídající této řez. Způsob, jak tomu by pomocí proměnné WindowStart a WindowEnd, které jsou k dispozici v Azure Data Factory. Přečtěte si informace o proměnných a funkce v Azure Data Factory zde v [plánování a provádění](../articles/data-factory/v1/data-factory-scheduling-and-execution.md) článku. Příklad: 

```json
"source": {
"type": "SqlSource",
"sqlReaderQuery": "$$Text.Format('select * from MyTable where timestampcolumn >= \\'{0:yyyy-MM-dd HH:mm\\' AND timestampcolumn < \\'{1:yyyy-MM-dd HH:mm\\'', WindowStart, WindowEnd)"
},
```
Tento dotaz načte data z "MyTable" v trvání rozsah řezu. Spusťte tento řez by také vždy ujistěte se, toto chování. 

V ostatních případech možná budete chtít přečíst celou tabulku (Předpokládejme pro jednou přesunout jenom) a může sqlReaderQuery definujte takto:

```json
"source": 
{            
    "type": "SqlSource",
    "sqlReaderQuery": "select * from MyTable"
},
```
