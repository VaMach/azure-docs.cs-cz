## <a name="repeatability-during-copy"></a>Opakovatelnosti během kopírování
Při kopírování dat do Azure SQL nebo systému SQL Server z jiných dat ukládá musí jeden opakovatelnosti mějte na paměti, aby se zabránilo neúmyslnému výstupy. 

Při kopírování dat do databáze serveru SQL/SQL Azure, aktivitě kopírování se ve výchozím nastavení připojení v datové sadě do tabulky jímky ve výchozím nastavení. Například při kopírování dat ze zdroje souboru CSV (data hodnot oddělených čárkami) obsahující dva záznamy k databázi Azure SQL nebo SQL Server, to je, jak vypadá v tabulce:

```
ID    Product        Quantity    ModifiedDate
...    ...            ...            ...
6    Flat Washer    3            2015-05-01 00:00:00
7     Down Tube    2            2015-05-01 00:00:00
```

Předpokládejme, že byly nalezeny chyby ve zdrojovém souboru a aktualizované množství dolů trubice od 2 do 4 ve zdrojovém souboru. Pokud znovu spustíte datový řez pro toto období, zjistíte dva nové záznamy připojenou k databázi Azure SQL nebo SQL Server. Níže předpokládá žádné sloupce v tabulce nejsou omezení primárního klíče.

```
ID    Product        Quantity    ModifiedDate
...    ...            ...            ...
6    Flat Washer    3            2015-05-01 00:00:00
7     Down Tube    2            2015-05-01 00:00:00
6    Flat Washer    3            2015-05-01 00:00:00
7     Down Tube    4            2015-05-01 00:00:00
```

Abyste tomu předešli, budete muset zadat UPSERT sémantiku s využitím jeden z nižší než 2 mechanismy, které jsou uvedeny níže.

> [!NOTE]
> Řez opětovně lze spustit automaticky v Azure Data Factory podle určené zásady opakování.
> 
> 

### <a name="mechanism-1"></a>Mechanismus 1
Můžete využít **sqlWriterCleanupScript** vlastnosti tak, aby nejprve provést čištění akce při spuštění řez. 

```json
"sink":  
{ 
  "type": "SqlSink", 
  "sqlWriterCleanupScript": "$$Text.Format('DELETE FROM table WHERE ModifiedDate >= \\'{0:yyyy-MM-dd HH:mm}\\' AND ModifiedDate < \\'{1:yyyy-MM-dd HH:mm}\\'', WindowStart, WindowEnd)"
}
```

Skript pro vyčištění by byl proveden první během kopírování pro danou řez, který by odstranit data z tabulek SQL odpovídající této řez. Aktivity budou následně vložení dat do tabulky SQL. 

Pokud je řez je nyní spusťte znovu a potom jste zjistí, že množství je aktualizovat, protože požadovaný.

```
ID    Product        Quantity    ModifiedDate
...    ...            ...            ...
6    Flat Washer    3            2015-05-01 00:00:00
7     Down Tube    4            2015-05-01 00:00:00
```

Předpokládejme, že odebrání záznamu ploché podložku z původní sdílený svazek clusteru. Opětovné spuštění řezu vznikna následující výsledek: 

```
ID    Product        Quantity    ModifiedDate
...    ...            ...            ...
7     Down Tube    4            2015-05-01 00:00:00
```
Nic nového museli provést. Aktivita kopírování spustili čištění skript, který chcete odstranit data odpovídající této řez. Pak jej číst vstupní ze souboru csv (což je obsaženy pouze 1 záznam) a vložit do tabulky. 

### <a name="mechanism-2"></a>Mechanismus 2
> [!IMPORTANT]
> sliceIdentifierColumnName není v současné době podporovaný pro Azure SQL Data Warehouse. 

Jiný mechanismus k dosažení opakovatelnosti spočívá v použití vyhrazených sloupec (**sliceIdentifierColumnName**) v cílové tabulce. V tomto sloupci se použije službou Azure Data Factory pro Ujistěte se, že zdrojový a cílový zůstane synchronizovaná. Tento přístup funguje, když je flexibilitu při změně nebo definování schématu cílové tabulky SQL. 

V tomto sloupci se použije službou Azure Data Factory pro účely opakovatelnost a v procesu Azure Data Factory nebude žádné změny schématu tabulky. Způsob použití tohoto přístupu:

1. Definujte sloupce binárního typu (32) v cílové tabulce SQL. Měla by existovat žádné omezení na tomto sloupci. V tomto příkladu budeme název v tomto sloupci jako 'ColumnForADFuseOnly'.
2. Ho použijte k aktivitě kopírování následujícím způsobem:
   
    ```json
    "sink":  
    { 
   
        "type": "SqlSink", 
        "sliceIdentifierColumnName": "ColumnForADFuseOnly"
    }
    ```

Azure Data Factory bude naplnit tento sloupec podle jeho potřeba zajistit, že zdrojový a cílový zůstane synchronizovaná. Hodnoty v tomto sloupci nepoužívejte mimo tento kontext uživatele. 

Podobně jako mechanismus 1, aktivity kopírování automaticky použije první vyčistit data pro danou řez z cílového tabulky SQL a poté spusťte aktivitě kopírování normálně k vložení dat ze zdroje do cíle pro tento řez. 

