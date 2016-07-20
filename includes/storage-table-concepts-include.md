## Co je služba Table?

Služba Azure Table Storage ukládá velké objemy strukturovaných dat. Služba je úložištěm dat typu NoSQL, které přijímá ověřená volání z cloudu Azure i z prostředí mimo něj. Tabulky Azure jsou ideální pro ukládání strukturovaných, nerelačních dat. Mezi běžná použití služby Table patří:

-   Ukládání terabajtů strukturovaných dat, která můžou obsluhovat škálované webové aplikace
-   Ukládání datových sad, které nevyžadují komplexní spojení, cizí klíče nebo uložené postupy a které můžete denormalizovat kvůli rychlému přístupu
-   Rychle dotazování na data pomocí clusterovaného indexu
-   Přístup k datům pomocí protokolu OData a dotazů LINQ s knihovnami .NET datové služby WCF

Službu Table můžete použít k ukládání a dotazování obrovských sad strukturovaných, nerelačních dat a vaše tabulky se budou s rostoucími požadavky škálovat.

## Koncepty služby Table

Služba Table obsahuje následující součásti:

![Table1][Table1]

-   **Formát adresy URL:** Kód adresuje tabulky na účtu pomocí následujícího formátu adresy:   
    http://`<storage account>`.table.core.windows.net/`<table>`  
      
    Tabulky Azure můžete adresovat přímo pomocí této adresy s protokolem OData. Další informace najdete na webu [OData.org][].

-   **Účet úložiště:** Veškerý přístup ke službě Azure Storage se provádí prostřednictvím účtu úložiště. Podrobné informace o kapacitě účtu úložiště najdete v článku [Škálovatelnost a cíle výkonnosti služby Azure Storage](storage-scalability-targets.md).

-   **Tabulka**: Tabulka je kolekcí entit. Tabulky nevynucují u entit schéma, což znamená, že jedna tabulka může obsahovat entity s různými sadami vlastností. Počet tabulek, které může účet úložiště obsahovat, je omezený pouze limitem kapacity účtu úložiště.

-   **Entita**: Entita je sada vlastností, která se podobá řádku databáze. Entita může mít velikost až 1 MB.

-   **Vlastnosti**: Vlastnost je pár název-hodnota. Každá entita může obsahovat až 252 vlastností pro ukládání dat. Každá entita má také tři systémové vlastnosti, které určují klíč oddílu, klíč řádku a časové razítko. Entity se stejným klíčem oddílu můžete dotazovat rychleji a můžete je vkládat nebo aktualizovat v atomických operacích. Klíč řádku entity je jedinečným identifikátorem v rámci oddílu.

Podrobnosti o pojmenovávání tabulek a vlastnostech najdete v článku [Principy datového modelu služby Table](https://msdn.microsoft.com/library/azure/dd179338.aspx).
  
  [Table1]: ./media/storage-table-concepts-include/table1.png
  [OData.org]: http://www.odata.org/



<!--HONumber=Jun16_HO2-->


