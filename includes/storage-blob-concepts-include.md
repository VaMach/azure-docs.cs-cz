## <a name="what-is-blob-storage"></a>Co je služba Blob storage?
Azure Blob Storage je služba, která slouží k ukládání velkých objemů nestrukturovaných dat objektů, například textu nebo binárních dat, která jsou přístupná odkudkoli na světě prostřednictvím protokolů HTTP nebo HTTPS. Službu Blob Storage můžete používat ke zveřejňování dat pro celý svět, nebo k soukromému ukládání dat aplikací.

Mezi běžná použití služby Blob Storage patří:

* Poskytování obrázků nebo dokumentů přímo do prohlížeče.
* Ukládání souborů pro distribuovaný přístup.
* Streamování videa a zvuku.
* Ukládání dat pro zálohování a obnovování, zotavení po havárii a archivaci.
* Ukládání dat pro analýzu pomocí místní nebo Azure hostovaná služba.

## <a name="blob-service-concepts"></a>Koncepty služby Blob service
Služba Blob service obsahuje následující součásti:

![Diagram architektury služby objektů Blob](./media/storage-blob-concepts-include/blob1.png)

* **Účet úložiště:** veškerý přístup do služby Azure Storage se provádí prostřednictvím účtu úložiště. Tento účet úložiště může být **účet úložiště pro obecné účely** nebo **účtem služby Blob storage**, který se specializuje na ukládání objektů nebo objekty BLOB. Další informace najdete v tématu [Účty Azure Storage](../articles/storage/common/storage-create-storage-account.md).
* **Kontejner:** Kontejner zajišťuje seskupení sady objektů blob. Všechny objekty blob musí být v kontejneru. Účet může obsahovat neomezený počet kontejnerů. Kontejner můžete pojmout neomezený počet objektů blob. Všimněte si, že název kontejneru musí být psaný malými písmeny.
* **Objekt blob:** Soubor libovolného typu a velikosti. Azure Storage nabízí tři typy objektů blob: objekty BLOB bloků, doplňovací objekty BLOB a objekty BLOB stránky.
  
    *Objekty blob bloků* jsou ideální pro ukládání textových nebo binárních souborů, například dokumentů a multimediálních souborů. Jeden objekt blob bloku může obsahovat až 50 000 bloků, každý o velikosti až 100 MB, v celkové velikosti o něco větší než 4,75 TB (100 MB × 50 000). 

    *Doplňovací objekty blob* jsou podobné objektům blob bloku v tom, že je tvoří bloky, ale jsou optimalizované pro doplňovací operace, takže se hodí pro scénáře protokolování. Jeden doplňovací objekt blob může obsahovat až 50 000 bloků, každý o velikosti až 4 MB, v celkové velikosti o něco větší než 195 GB (4 MB × 50 000).
  
    *Objekty blob stránek* můžou mít velikost až 1 TB a jsou efektivnější pro časté operace čtení a zápisu. Azure Virtual Machines používá objekty BLOB stránek jako operačního systému a datové disky.
  
    Podrobnosti o pojmenovávání kontejnerů a objektů BLOB najdete v tématu [pojmenování a odkazování na kontejnerů, objektů BLOB a metadat](/rest/api/storageservices/Naming-and-Referencing-Containers--Blobs--and-Metadata).

