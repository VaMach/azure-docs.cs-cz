## <a name="overview"></a>Přehled
Úložiště Azure File je služba, která nabízí sdílené složky v cloudu přes standardní [protokol SMB (Server Message Block)](https://msdn.microsoft.com/library/windows/desktop/aa365233.aspx). Podporují se SMB 2.1 i SMB 3.0. S Azure File Storage můžete rychle a bez nákladných přepisů migrovat starší aplikace, které spoléhají na sdílené složky, do Azure. Aplikace běžící v cloudových službách nebo virtuálních počítačích Azure nebo spuštěné z lokálních klientů můžou připojit sdílenou složku v cloudu stejným způsobem, jako desktopová aplikace připojí typickou sdílenou složku SMB. Potom může sdílenou složku File Storage připojit a používat libovolný počet aplikací.

Protože je sdílená složka File Storage standardní sdílnou složkou SMB, aplikace běžící v Azure můžou k datům ve sdílené složce přistupovat přes API pro vstup/výstup souborového systému. Vývojáři tedy můžou využít svoje dovednosti a znalosti kódu při migraci stávajících aplikací. Profesionálové v oblasti výpočetní techniky můžou pomocí rutin prostředí PowerShell vytvářet, připojovat a spravovat sdílené složky File Storage v rámci správy aplikací Azure.

Sdílené složky Azure můžete vytvářet pomocí [webu Azure Portal](https://portal.azure.com), rutin PowerShell pro Azure Storage, knihovny klienta Azure Storage nebo REST API pro Azure Storage. Protože jsou tyto sdílené složky sdílenými složkami SMB, můžete k nim také přistupovat přes standardní a známá rozhraní API pro souborové systémy.

<!--HONumber=Oct16_HO3-->


