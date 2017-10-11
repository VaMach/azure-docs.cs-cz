# <a name="back-up-azure-unmanaged-vm-disks-with-incremental-snapshots"></a>Zálohování Azure nespravované disky virtuálních počítačů s přírůstkové snímky
## <a name="overview"></a>Přehled
Úložiště Azure poskytuje schopnost pořizovat snímky objektů BLOB. Snímky zaznamenání stavu objektů blob v tomto bodě v čase. V tomto článku jsme popisují scénáře, ve kterém můžete spravovat zálohy disků virtuálních počítačů pomocí snímků. Můžete použít tuto metodu, když jste zvolit nepoužívání Azure zálohování a obnovení služby a chcete vytvořit vlastní strategii zálohování pro disky virtuálního počítače.

Disky virtuálních počítačů Azure jsou uloženy jako objekty BLOB stránky ve službě Azure Storage. Vzhledem k tomu, že jsme se s popisem strategie zálohování pro disky virtuálního počítače v tomto článku, označujeme snímky v souvislosti s objekty BLOB stránky. Další informace o snímků, najdete v tématu [vytvoření snímku objektu Blob](https://docs.microsoft.com/rest/api/storageservices/Creating-a-Snapshot-of-a-Blob).

## <a name="what-is-a-snapshot"></a>Co je snímek?
Objekt blob snímek je jen pro čtení verze objektu blob zachycenou v bodě v čase. Po vytvoření snímku, ho můžete číst, kopírovat, nebo odstranit, ale nedojde ke změně. Snímky poskytují způsob, jak zálohovat objekt blob, jak se objevuje v časovém okamžiku. Dokud REST verze 2015-04-05 bylo možnost Kopírovat úplné snímky. S ostatními verze 2015-07-08 a vyšší, můžete také můžete zkopírovat přírůstkové snímky.

## <a name="full-snapshot-copy"></a>Kopírování úplné snímku
Snímky je možné zkopírovat do jiného účtu úložiště jako objekt blob zachovat zálohy základní objekt blob. Můžete také zkopírovat snímek přes jeho základní objekt blob, které je třeba obnovení objektu blob na starší verzi. Po zkopírování snímek z jednoho účtu úložiště do druhého zabírá na stejném místě jako objekt blob základní stránky. Proto kopírování celou snímky z jednoho účtu úložiště do druhého je pomalé a spotřebuje, kolik místa v cílový účet úložiště.

> [!NOTE]
> Pokud základní objekt blob zkopírujete na jiný cíl, nejsou spolu se ji zkopírovat snímky objektu blob. Podobně přepíšete základní objekt blob se kopie, neovlivní snímky přidružené základní objekt blob a zůstanou beze změn v části název základního objektu blob.
> 
> 

### <a name="back-up-disks-using-snapshots"></a>Zálohovat disky pomocí snímků
Jako strategie zálohování pro disky virtuálního počítače, můžete využít pravidelné snímky objektu blob disk nebo stránce a kopírování je do jiného úložiště účtu pomocí nástroje jako [kopírovat objekt Blob](https://docs.microsoft.com/rest/api/storageservices/Copy-Blob) operaci nebo [AzCopy](../articles/storage/common/storage-use-azcopy.md). Snímek můžete zkopírovat na cílový objekt blob stránky s jiným názvem. Výsledný objekt blob stránky cílové je objekt blob stránky lze zapisovat a není snímek. Později v tomto článku jsme popisují kroky pro zálohování disky virtuálního počítače pomocí snímků.

### <a name="restore-disks-using-snapshots"></a>Obnovení disků pomocí snímků
Pokud nastane čas k obnovení vašeho disku stabilní verzi, která byla dříve zaznamenána v jednom z snímků zálohy, můžete zkopírovat snímek přes základní stránky objektu blob. Po snímku je propagována do základní stránky objektu blob, zůstanou snímku, ale dojde k přepsání zdrojem kopie, kterou může být číst a zapisovat. Později v tomto článku jsme popisují postup obnovit předchozí verze disku z jeho snímků.

### <a name="implementing-full-snapshot-copy"></a>Implementace kopie úplné snímku
Pomocí těchto kroků můžete implementovat kopii úplné snímku

* Nejprve vytvořte snímek pomocí základní objekt blob [snímku Blob](https://docs.microsoft.com/rest/api/storageservices/Snapshot-Blob) operaci.
* Zkopírujte snímek pomocí účet cílového úložiště [kopírovat objekt Blob](https://docs.microsoft.com/rest/api/storageservices/Copy-Blob).
* Opakujte tento postup k udržování záložní kopie vaše základní objekt blob.

## <a name="incremental-snapshot-copy"></a>Kopírování přírůstkový snímek
Nová funkce v [GetPageRanges](https://docs.microsoft.com/rest/api/storageservices/Get-Page-Ranges) rozhraní API poskytuje mnohem lepší způsob, jak zálohovat snímky objekty BLOB stránky nebo disků. Rozhraní API vrátí seznam změn mezi základní objekt blob a snímky, což snižuje množství prostoru úložiště použít na účet záloh. Rozhraní API podporuje objekty BLOB stránky na Storage úrovně Premium, jakož i úložiště Standard Storage. Toto rozhraní API používáte, můžete vytvořit rychlejší a efektivnější řešení zálohování pro virtuální počítače Azure. Toto rozhraní API bude k dispozici REST verze 2015-07-08 a vyšší.

Přírůstkové kopie snímků umožňuje kopírovat z jednoho účtu úložiště do druhého rozdíl mezi,

* Základní objekt blob a jeho snímek nebo
* Všechny dva snímky základní objekt blob

Za předpokladu, že jsou splněny následující podmínky,

* Objekt blob byl vytvořen ledna-1-2016 nebo novější.
* Objekt blob nebyla přepsána [PutPage](https://docs.microsoft.com/rest/api/storageservices/Put-Page) nebo [kopírovat objekt Blob](https://docs.microsoft.com/rest/api/storageservices/Copy-Blob) mezi dvěma snímky.

**Poznámka:**: Tato funkce je dostupná pro Premium a Standard objekty BLOB stránky Azure.

Až budete mít vlastní strategii zálohování pomocí snímků, kopírování snímky z jednoho účtu úložiště do druhého může být pomalé a spotřebovat velké úložiště. Namísto kopírování celý snímek na účet úložiště záloh, můžete napsat rozdíl mezi po sobě jdoucích snímků na objekt blob stránky zálohování. Tímto způsobem je podstatně snížit čas pro kopírování a místa pro uložení zálohy.

### <a name="implementing-incremental-snapshot-copy"></a>Implementace přírůstkový snímek kopie
Pomocí těchto kroků můžete implementovat přírůstkový snímek kopie

* Pořízení snímku základní objekt blob pomocí [snímku Blob](https://docs.microsoft.com/rest/api/storageservices/Snapshot-Blob).
* Kopírování snímku do cílového úložiště pro zálohy účtu pomocí [kopírovat objekt Blob](https://docs.microsoft.com/rest/api/storageservices/Copy-Blob). Toto je objektů blob zálohy stránky. Pořízení snímku objektů blob zálohy stránky a uloží jej v zálohování účtu.
* Vytvořte nový snímek objektu blob základní pomocí snímku objektů Blob.
* Získat rozdíl mezi prvním a druhém snímkům základní objekt blob pomocí [GetPageRanges](https://docs.microsoft.com/rest/api/storageservices/Get-Page-Ranges). Použít nový parametr **prevsnapshot**, zadejte hodnotu data a času chcete získat rozdíl oproti snímku. Pokud tento parametr, REST odpověď obsahuje pouze stránky, které byly změněny mezi cíl snímku a předchozího snímku, včetně zrušte stránky.
* Použití [PutPage](https://docs.microsoft.com/rest/api/storageservices/Put-Page) použití těchto změn na objekt blob stránky zálohování.
* Nakonec pořízení snímku objektů blob zálohy stránky a uloží jej v účtu úložiště záloh.

V další části jsme se podrobněji popisují jak můžete spravovat zálohy disků pomocí přírůstkové kopie snímků

## <a name="scenario"></a>Scénář
V této části popisují jsme scénář, který zahrnuje vlastní strategii zálohování pro disky virtuálního počítače pomocí snímků.

Zvažte DS-series virtuální počítač Azure s připojený disk P30 úložiště premium. P30 disk označuje *mypremiumdisk* je uložený v účtu úložiště premium názvem *mypremiumaccount*. Účet standardního úložiště s názvem *mybackupstdaccount* se použije k uložení zálohy *mypremiumdisk*. Rádi bychom se zachovat snímek *mypremiumdisk* každých 12 hodin.

Další informace o vytváření účtu úložiště a disky, najdete v tématu [účty Azure storage](../articles/storage/storage-create-storage-account.md).

Další informace o zálohování virtuálních počítačů Azure, najdete v tématu [zálohování virtuálního počítače Azure plánování](../articles/backup/backup-azure-vms-introduction.md).

## <a name="steps-to-maintain-backups-of-a-disk-using-incremental-snapshots"></a>Postup Udržovat zálohy disku pomocí přírůstkové snímků
Následující kroky popisují postup pořízení snímků *mypremiumdisk* a udržovat záloh v *mybackupstdaccount*. Zálohování je objekt blob standardní stránky názvem *mybackupstdpageblob*. Objekt blob zálohování stránky vždy odráží stejného stavu jako poslední snímek *mypremiumdisk*.

1. Vytvořte objekt blob zálohování stránky pro disk úložiště premium, tak, že pořízení snímku *mypremiumdisk* názvem *mypremiumdisk_ss1*.
2. Zkopírujte tento snímek do mybackupstdaccount jako objekt blob stránky názvem *mybackupstdpageblob*.
3. Pořízení snímku *mybackupstdpageblob* názvem *mybackupstdpageblob_ss1*pomocí [snímku Blob](https://docs.microsoft.com/rest/api/storageservices/Snapshot-Blob) a uloží jej v *mybackupstdaccount*.
4. V daném intervalu zálohování vytvořit snímek jiného *mypremiumdisk*, například *mypremiumdisk_ss2*a ukládá je v *mypremiumaccount*.
5. Získat přírůstkové změny mezi dvěma snímky *mypremiumdisk_ss2* a *mypremiumdisk_ss1*pomocí [GetPageRanges](https://docs.microsoft.com/rest/api/storageservices/Get-Page-Ranges) na *mypremiumdisk_ ss2* s **prevsnapshot** parametr nastaven na časové razítko *mypremiumdisk_ss1*. Zapište tyto přírůstkové změny do objektů blob zálohy stránky *mybackupstdpageblob* v *mybackupstdaccount*. Pokud jsou v přírůstkové změny odstraněné rozsahy, je potřeba smazat z objektů blob zálohy stránky. Použití [PutPage](https://docs.microsoft.com/rest/api/storageservices/Put-Page) k zápisu do objektů blob zálohy stránky přírůstkové změny.
6. Pořízení snímku objektů blob zálohy stránky *mybackupstdpageblob*, volané *mybackupstdpageblob_ss2*. Odstranit předchozí snímek *mypremiumdisk_ss1* z prémiový účet úložiště.
7. Opakujte kroky 4 až 6 každých intervalu zálohování. Tímto způsobem můžete udržovat zálohy *mypremiumdisk* v standardní účet úložiště.

![Zálohování pomocí přírůstkové snímků disku](../articles/virtual-machines/windows/media/incremental-snapshots/storage-incremental-snapshots-1.png)

## <a name="steps-to-restore-a-disk-from-snapshots"></a>Postup obnovení disku ze snímků
Následující kroky popisují, jak obnovit premium disk *mypremiumdisk* na dřívějším snímkem z účtu úložiště pro zálohy *mybackupstdaccount*.

1. Identifikujte bod v čase, kterou chcete obnovit premium disk. Řekněme, že je snímek *mybackupstdpageblob_ss2*, který je uložený v účtu úložiště pro zálohy *mybackupstdaccount*.
2. V mybackupstdaccount, zvýšení úrovně snímku *mybackupstdpageblob_ss2* jako nový objekt blob zálohování základní stránky *mybackupstdpageblobrestored*.
3. Pořízení snímku tento objekt blob stránky obnovené zálohy, nazývá *mybackupstdpageblobrestored_ss1*.
4. Kopírovat objekt blob obnovené stránky *mybackupstdpageblobrestored* z *mybackupstdaccount* k *mypremiumaccount* jako nový disk premium *mypremiumdiskrestored*.
5. Pořízení snímku *mypremiumdiskrestored*, volané *mypremiumdiskrestored_ss1* pro provedení budoucí přírůstkové zálohování.
6. Bodu řady DS virtuální počítač obnovený disk *mypremiumdiskrestored* a odpojit starý *mypremiumdisk* z virtuálního počítače.
7. Zahájit proces zálohování popsané v předchozí části pro obnovené disk *mypremiumdiskrestored*pomocí *mybackupstdpageblobrestored* jako objekt blob zálohování stránky.

![Obnovení disku ze snímků](../articles/virtual-machines/windows/media/incremental-snapshots/storage-incremental-snapshots-2.png)

## <a name="next-steps"></a>Další kroky
Další informace o vytváření snímků objektu blob a plánování infrastruktury zálohování virtuálních počítačů pomocí následujících odkazů.

* [Vytvoření snímku objektu Blob](https://docs.microsoft.com/rest/api/storageservices/Creating-a-Snapshot-of-a-Blob)
* [Plánování vaší infrastruktury zálohování virtuálních počítačů](../articles/backup/backup-azure-vms-introduction.md)

