## Co je služba Blob Storage?

Azure Blob Storage je služba, která slouží k ukládání velkých objemů nestrukturovaných dat objektů, například textu nebo binárních dat, která jsou přístupná odkudkoli na světě prostřednictvím protokolů HTTP nebo HTTPS. Službu Blob Storage můžete používat ke zveřejňování dat pro celý svět, nebo k soukromému ukládání dat aplikací.

Mezi běžná použití služby Blob Storage patří:

- poskytování obrázků nebo dokumentů přímo do prohlížeče
- ukládání souborů pro distribuovaný přístup
- streamování videa a zvuku
- ukládání dat pro zálohování a obnovování, zotavení po havárii a pro archivaci
- ukládání dat, které bude analyzovat místní nebo v Azure hostovaná služba

## Koncepty služby Blob service

Služba Blob service obsahuje následující součásti:

![Blob1][Blob1]

- **Účet úložiště:** Veškerý přístup ke službě Azure Storage se provádí prostřednictvím účtu úložiště. Tento účet úložiště může být **účtem úložiště pro obecné účely** nebo **účtem služby Blob Storage**, který se specializuje na ukládání objektů blob. Další informace o účtech úložiště najdete v článku [Účty úložiště Azure](../articles/storage/storage-create-storage-account.md).

- **Kontejner:** Kontejner zajišťuje seskupení sady objektů blob. Všechny objekty blob musí být v kontejneru. Účet může obsahovat neomezený počet kontejnerů. Kontejner můžete pojmout neomezený počet objektů blob. Všimněte si, že název kontejneru musí být psaný malými písmeny.

- **Objekt blob:** Soubor libovolného typu a velikosti. Úložiště Azure Storage nabízí tři typy objektů blob – objekty blob bloků, doplňovací objekty blob a objekty blob stránek.

    *Objekty blob bloků* jsou ideální pro ukládání textových nebo binárních souborů, například dokumentů a mediálních souborů. *Doplňovací objekty blob* jsou podobné objektům blob bloku v tom, že je tvoří bloky, ale jsou optimalizované pro doplňovací operace, takže se hodí pro scénáře protokolování. Jeden objekt blob bloku nebo doplňovací objekt blob může obsahovat až 50 000 bloků, každý o velikosti až 4 MB, v celkové velikosti o něco větší než 195 GB (4 MB × 50 000).

    *Objekty blob stránek* můžou mít velikost až 1 TB a jsou efektivnější pro časté operace čtení a zápisu. Služba Azure Virtual Machines používá objekty blob stránek jako disky pro operační systém a ukládání dat.

    Podrobnosti o vytváření názvů kontejnerů a objektů blob najdete v článku [Názvy kontejnerů, objektů blob a metadat a odkazování na ně](https://msdn.microsoft.com/library/azure/dd135715.aspx).


[Blob1]: ./media/storage-blob-concepts-include/blob1.jpg



<!--HONumber=sep16_HO2-->


