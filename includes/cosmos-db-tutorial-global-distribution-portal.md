
Můžete informace o Azure Cosmos DB globální distribuce v této Azure pátek video s Scott Hanselman a Karthik Raman hlavní manažer inženýrství.

>[!VIDEO https://channel9.msdn.com/Shows/Azure-Friday/Planet-Scale-NoSQL-with-DocumentDB/player]  

Další informace o tom, jak globální replikace databáze v Azure Cosmos DB funguje, najdete v části [distribuci dat globálně pomocí Cosmos DB](../articles/cosmos-db/distribute-data-globally.md).

## <a id="addregion"></a>Přidejte globální databáze oblastí pomocí portálu Azure
Je k dispozici ve všech Azure Cosmos DB [oblastí Azure] [ azureregions] celém světě. Po výběru výchozí úroveň konzistence pro váš účet databáze, můžete přidružit jeden nebo více oblastí (v závislosti na vaši volbu výchozí konzistence úrovně a globální distribuci potřeby).

1. V [portál Azure](https://portal.azure.com/), v levém panelu klikněte na **Azure Cosmos DB**.
2. V **Azure Cosmos DB** okně, vyberte databázi účet změnit.
3. V okně účtu klikněte na **replikovat data globálně** z nabídky.
4. V **replikovat data globálně** okně vyberte oblasti, které chcete přidat nebo odebrat tak, že kliknete na oblasti v mapě a pak klikněte na **Uložit**. A náklady na přidání oblastí najdete [stránce s cenami](https://azure.microsoft.com/pricing/details/cosmos-db/) nebo [distribuci dat globálně pomocí Azure Cosmos DB](../articles/cosmos-db/distribute-data-globally.md) Další informace najdete v článku.
   
    ![Klikněte na oblasti v mapě přidat nebo odebrat je][1]
    
Jakmile přidáte druhý oblasti, **ruční převzetí služeb při selhání** je zapnuta možnost **replikovat data globálně** okno na portálu. Tuto možnost můžete testovat proces převzetí služeb při selhání nebo změňte oblasti primární zápisu. Jakmile přidáte třetí oblasti, **priorit převzetí služeb při selhání** možnost je povolená na stejné okno, ve kterém můžete změnit pořadí převzetí služeb při selhání pro čtení.  

### <a name="selecting-global-database-regions"></a>Výběr oblasti globální databáze
Existují dvě běžné scénáře pro konfiguraci dvou nebo více oblastí:

1. Doručování s nízkou latencí přístup k datům pro koncové uživatele bez ohledu na to, kde se nachází po celém světě
2. Přidání místní odolnost pro provozní kontinuitu a zotavení po havárii (BCDR)

Pro doručení s nízkou latencí pro koncové uživatele, doporučujeme nasadit obě aplikace a přidat Azure Cosmos DB v oblastech, které je odpovídají k umístění aplikace uživatele.

Pro BCDR, doporučuje se přidat na základě páru oblasti popsané v oblastech [obchodní kontinuitu a zotavení po havárii (BCDR): spárovat oblasti Azure] [ bcdr] článku.

<!--

## <a id="selectwriteregion"></a>Select the write region

While all regions associated with your Cosmos DB database account can serve reads (both, single item as well as multi-item paginated reads) and queries, only one region can actively receive the write (insert, upsert, replace, delete) requests. To set the active write region, do the following  


1. In the **Azure Cosmos DB** blade, select the database account to modify.
2. In the account blade, click **Replicate data globally** from the menu.
3. In the **Replicate data globally** blade, click **Manual Failover** from the top bar.
    ![Change the write region under Azure Cosmos DB Account > Replicate data globally > Manual Failover][2]
4. Select a read region to become the new write region, click the checkbox to confirm triggering a failover, and click OK
    ![Change the write region by selecting a new region in list under Azure Cosmos DB Account > Replicate data globally > Manual Failover][3]

--->

<!--Image references-->
[1]: ./media/cosmos-db-tutorial-global-distribution-portal/azure-cosmos-db-add-region.png
[2]: ./media/cosmos-db-tutorial-global-distribution-portal/azure-cosmos-db-manual-failover-1.png
[3]: ./media/cosmos-db-tutorial-global-distribution-portal/azure-cosmos-db-manual-failover-2.png

<!--Reference style links - using these makes the source content way more readable than using inline links-->
[bcdr]: https://azure.microsoft.com/documentation/articles/best-practices-availability-paired-regions/
[consistency]: ../articles/cosmos-db/consistency-levels.md
[azureregions]: https://azure.microsoft.com/regions/#services
[offers]: https://azure.microsoft.com/pricing/details/cosmos-db/
