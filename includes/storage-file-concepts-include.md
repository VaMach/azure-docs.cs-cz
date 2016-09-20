## Co je služba File Storage?

Služba File Storage nabízí sdílené úložiště pro aplikace, které používají standardní protokol SMB 2.1 nebo SMB 3.0. Virtuální počítače a cloudové služby Microsoft Azure můžou sdílet souborová data mezi komponentami aplikace přes sdílené složky a místní aplikace můžou k souborovým datům ve sdílené složce přistupovat přes rozhraní API služby File Storage.

Aplikace běžící v cloudových službách nebo virtuálních počítačích Azure můžou připojit sdílené úložiště služby File Storage pro přístup k datům souborů stejným způsobem, jako desktopová aplikace připojí typickou sdílenou složku SMB. Potom může sdílené úložiště File Storage připojit a používat libovolný počet virtuálních počítačů nebo rolí Azure.

Protože sdílené úložiště File Storage je standardní složkou sdílených souborů v Azure, která je sdílená pomocí protokolu SMB, aplikace běžící v Azure můžou k datům ve sdíleném úložišti přistupovat přes rozhraní API určená pro vstupně–výstupní operace se soubory. Vývojáři tedy můžou využít svoje dovednosti a znalosti kódu při migraci stávajících aplikací. Profesionálové v oblasti výpočetní techniky můžou pomocí rutin prostředí PowerShell vytvářet, připojovat a spravovat sdílené složky File Storage v rámci správy aplikací Azure. Tato příručka obsahuje příklady obou.

Mezi běžná použití služby File Storage patří:

- Migrace místních aplikací, které závisí na sdílených složkách, aby mohly spouštět virtuální počítače nebo cloudové služby Azure bez nákladných přepisů
- Ukládání nastavení sdílených aplikací, například do konfiguračních souborů
- Ukládání diagnostických dat, například protokolů, metrik a výpisů stavu systému ve sdíleném umístění 
- Ukládání nástrojů potřebných pro vývoj nebo správu virtuálních počítačů nebo cloudových služeb Azure

## Koncepty služby File Storage

Služba File Storage obsahuje následující součásti:

![files-concepts][files-concepts]

-   **Účet úložiště:** Veškerý přístup ke službě Azure Storage se provádí prostřednictvím účtu úložiště. Podrobné informace o kapacitě účtu úložiště najdete v článku [Škálovatelnost a cíle výkonnosti služby Azure Storage](../articles/storage/storage-scalability-targets.md).

-   **Sdílená složka:** Sdílená složka úložiště File Storage představuje sdílenou složku protokolu SMB v Azure. 
    Všechny adresáře a soubory musí být vytvořeny v nadřazené sdílené složce. Účet může obsahovat neomezený počet sdílených složek a sdílená složka můžete obsahovat neomezený počet souborů až do 5 TB celkové kapacity sdílené složky.

-   **Adresář:** Volitelná hierarchie adresářů. 

-   **Soubor:** Soubor ve sdílené složce. Soubor může mít velikost až 1 TB.

-   **Formát adresy URL:** Soubory jsou adresovatelné v následujícím formátu adresy URL:   
    https://`<storage
    account>`.file.core.windows.net/`<share>`/`<directory/directories>`/`<file>`  
    
    Následující příklad adresy URL můžete použít k adresování jednoho ze souborů ve výše uvedeném diagramu:  
    `http://samples.file.core.windows.net/logs/CustomLogs/Log1.txt`

Podrobnosti o pojmenovávání sdílených složek, adresářů a souborů najdete v článku [Pojmenování a odkazování na sdílené složky, soubory a metadata](http://msdn.microsoft.com/library/azure/dn167011.aspx).

[files-concepts]: ./media/storage-file-concepts-include/files-concepts.png


<!--HONumber=sep16_HO2-->


