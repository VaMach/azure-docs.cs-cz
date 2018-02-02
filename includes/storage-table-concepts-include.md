## <a name="what-is-table-storage"></a>Co je služba Table Storage
Služba Azure Table Storage ukládá velké objemy strukturovaných dat. Služba je úložištěm dat typu NoSQL, které přijímá ověřená volání z cloudu Azure i z prostředí mimo něj. Tabulky Azure jsou ideální pro ukládání strukturovaných, nerelačních dat. Mezi běžná použití služby Table Storage patří:

* Ukládání terabajtů strukturovaných dat, která můžou obsluhovat škálované webové aplikace
* Ukládání datových sad, které nevyžadují komplexní spojení, cizí klíče nebo uložené postupy a které můžete denormalizovat kvůli rychlému přístupu
* Rychle dotazování na data pomocí clusterovaného indexu
* Přístup k datům pomocí protokolu OData a dotazů LINQ s knihovnami .NET datové služby WCF

Službu Table Storage můžete použít k ukládání a dotazování obrovských sad strukturovaných, nerelačních dat a vaše tabulky se budou s rostoucími požadavky škálovat.

## <a name="table-storage-concepts"></a>Koncepty služby Table Storage
Služba Table Storage obsahuje následující komponenty:

![Diagram komponent služby Table Storage][Table1]

* **Formát adresy URL:** účty Azure Table Storage, použijte tento formát:`http://<storage account>.table.core.windows.net/<table>`

  Účty Azure Cosmos DB tabulky API použijte tento formát:`http://<storage account>.table.cosmosdb.azure.com/<table>`  

  Tabulky Azure můžete adresovat přímo pomocí této adresy s protokolem OData. Další informace najdete na webu [OData.org][OData.org].
* **Účty:** veškerý přístup do služby Azure Storage se provádí prostřednictvím účtu úložiště. Podrobné informace o kapacitě účtu úložiště najdete v článku [Škálovatelnost a cíle výkonnosti úložiště Azure](../articles/storage/common/storage-scalability-targets.md). 

    Veškerý přístup k databázi Cosmos Azure se provádí prostřednictvím účtu tabulky rozhraní API. V tématu [vytvořit účet rozhraní API tabulky](../articles/cosmos-db/create-table-dotnet.md#create-a-database-account) podrobnosti vytvoření účtu tabulky rozhraní API.
* **Tabulka**: Tabulka je kolekcí entit. Tabulky nevynucují u entit schéma, což znamená, že jedna tabulka může obsahovat entity s různými sadami vlastností.  
* **Entita**: Entita je sada vlastností, která se podobá řádku databáze. Entita ve službě Azure Storage může mít velikost až 1MB. Entita v Azure Cosmos DB může mít velikost až 2MB.
* **Vlastnosti**: Vlastnost je pár název-hodnota. Každá entita může obsahovat až 252 vlastností pro ukládání dat. Každá entita má také tři systémové vlastnosti, které určují klíč oddílu, klíč řádku a časové razítko. Entity se stejným klíčem oddílu můžete dotazovat rychleji a můžete je vkládat nebo aktualizovat v atomických operacích. Klíč řádku entity je jedinečným identifikátorem v rámci oddílu.

Podrobnosti o pojmenovávání tabulek a vlastnostech najdete v článku [Principy datového modelu služby Table](/rest/api/storageservices/Understanding-the-Table-Service-Data-Model).

[Table1]: ./media/storage-table-concepts-include/table1.png
[OData.org]: http://www.odata.org/
