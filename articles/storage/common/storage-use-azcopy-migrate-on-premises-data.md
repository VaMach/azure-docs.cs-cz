---
title: "Migrovat místní data do služby Azure Storage s AzCopy | Microsoft Docs"
description: "K migraci dat nebo zkopírování dat z objektu blob, table a obsah souboru nebo pomocí nástroje AzCopy. Snadno migrujte data z místního úložiště do úložiště Azure."
services: storage
author: ruthogunnnaike
manager: jeconnoc
ms.service: storage
ms.tgt_pltfrm: na
ms.devlang: azcopy
ms.topic: tutorial
ms.date: 12/14/2017
ms.author: v-ruogun
ms.openlocfilehash: f2c0b80248ef706394b3f84df4c3da26fb79026a
ms.sourcegitcommit: 85012dbead7879f1f6c2965daa61302eb78bd366
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/02/2018
---
#  <a name="migrate-on-premises-data-to-cloud-storage-with-azcopy"></a>Migrace místních dat do cloudového úložiště pomocí nástroje AzCopy

AzCopy je nástroj příkazového řádku pro kopírování dat z úložiště Microsoft Azure Blob, soubor a tabulky, jednoduché příkazy určené pro optimální výkon. Může kopírovat data mezi systém souborů a účet úložiště nebo mezi účty úložiště.  

Existují dvě verze nástroje AzCopy, které si můžete stáhnout:

* [AzCopy v systému Linux](storage-use-azcopy.md) sestavena pomocí rozhraní .NET Framework Core, které cílí platformy Linux nabídky stylu POSIX možnosti příkazového řádku. 
* [AzCopy v systému Windows](../storage-use-azcopy.md) je obsažena v rozhraní .NET Framework a nabízí možnosti příkazového řádku Windows stylu. 
 
V tomto kurzu se naučíte:

> [!div class="checklist"]
> * vytvořit účet úložiště 
> * Pomocí nástroje AzCopy nahrát všechna vaše data
> * Upravit data pro účely testování
> * Vytvoření naplánované úlohy nebo procesu cron úlohy k identifikaci nových souborů pro nahrání

## <a name="prerequisites"></a>Požadavky

Pro absolvování tohoto kurzu potřebujete: 

* Stáhněte si nejnovější verzi AzCopy na [Linux](https://docs.microsoft.com/azure/storage/common/storage-use-azcopy-linux#download-and-install-azcopy) nebo [Windows](http://aka.ms/downloadazcopy). 

## <a name="log-in-to-the-azure-portalhttpsportalazurecom"></a>Přihlaste se k portálu [Azure Portal](https://portal.azure.com/).

[!INCLUDE [storage-quickstart-tutorial-create-account-portal](../../../includes/storage-quickstart-tutorial-create-account-portal.md)]

>[!NOTE]
>Pokud chcete být schopni stáhnout objekty BLOB ze sekundární oblasti do místního úložiště a naopak a nastavte **replikace** k **čtení access geograficky redundantní úložiště**. Výběrem této možnosti vytvoříte [geograficky redundantní úložiště](https://docs.microsoft.com/azure/storage/common/storage-redundancy#geo-redundant-storage) účtu. 
>
>

## <a name="create-a-container"></a>Vytvoření kontejneru

Objekty blob se vždy nahrávají do kontejneru. Kontejnery můžete uspořádat skupiny objektů BLOB jako uspořádání souborů ve vašem počítači ve složkách. 

Postupujte podle těchto kroků můžete vytvořit kontejner:

1. Vyberte **účty úložiště** tlačítko na hlavní stránce a vyberte účet úložiště, který jste vytvořili.
2. Vyberte **objekty BLOB** pod **služby**, pak vyberte **kontejneru**. 

![Vytvoření kontejneru](media/storage-azcopy-migrate-on-premises-data/CreateContainer.png)
 
Názvy kontejnerů musí začínat písmenem nebo číslicí a smí obsahovat jenom písmena, číslice a pomlčky (-). Další informace o pojmenování kontejnerů a objektů blob najdete v tématu [Názvy kontejnerů, objektů blob a metadat a odkazování na ně](/rest/api/storageservices/naming-and-referencing-containers--blobs--and-metadata).

## <a name="upload-all-files-in-a-folder-to-blob-storage"></a>Odeslat všechny soubory ve složce do úložiště objektů Blob

AzCopy můžete použít k nahrání všechny soubory ve složce do úložiště objektů Blob na [Windows](https://docs.microsoft.com/azure/storage/common/storage-use-azcopy#upload-blobs-to-blob-storage) nebo [Linux](https://docs.microsoft.com/azure/storage/common/storage-use-azcopy-linux#blob-download). Nahrát všechny objekty BLOB ve složce, zadejte následující příkaz AzCopy:

# <a name="linuxtablinux"></a>[Linux](#tab/linux)
    azcopy \
        --source /mnt/myfolder \
        --destination https://myaccount.blob.core.windows.net/mycontainer \
        --dest-key <key> \
        --recursive

# <a name="windowstabwindows"></a>[Windows](#tab/windows)
    AzCopy /Source:C:\myfolder /Dest:https://myaccount.blob.core.windows.net/mycontainer /DestKey: key /S
---

Nahraďte  **\<klíč\>**  nahraďte svým klíčem účtu. Na portálu Azure můžete načíst klíč účtu výběrem **přístupové klíče** v části nastavení ve vašem účtu úložiště. Vyberte klíč a vložte jej do příkazu AzCopy. Pokud zadaný cílový kontejner neexistuje, AzCopy ji vytvoří a odešle soubor do ní. Aktualizujte cestu ke zdrojovému adresáři dat a nahraďte **stránku Můj účet** v cílová adresa URL pro váš název účtu úložiště.

Zadejte `--recursive` a `/S` možnosti, na Linuxu a Windows, nahrát obsah zadaného adresáře k rekurzivnímu úložiště objektů Blob. Když spustíte nástroj AzCopy s jedním z těchto možností, všechny podsložky a jejich soubory jsou také odeslány.

## <a name="upload-modified-files-to-blob-storage"></a>Odeslat změněné soubory do úložiště objektů Blob
Můžete použít AzCopy k [nahrání souborů](https://docs.microsoft.com/azure/storage/common/storage-use-azcopy#other-azcopy-features) podle jejich čas poslední změny. Zkuste to, upravovat nebo vytvářet nové soubory zdrojový adresář pro účely testování. Odesílat pouze aktualizované nebo nové soubory, přidejte `--exclude-older`nebo `/XO` parametru AzCopy Linux a Windows příkaz v uvedeném pořadí.

Pokud chcete kopírovat zdroje prostředky, které neexistují v cílovém, zadat oba `--exclude-older` a `--exclude-newer` nebo `/XO` a `/XN` parametry v AzCopy Linux a Windows příkaz v uvedeném pořadí. AzCopy ukládání pouze aktualizovaná data, které jsou založené na jejich časové razítko.
 
# <a name="linuxtablinux"></a>[Linux](#tab/linux)
    azcopy \
    --source /mnt/myfolder \
    --destination https://myaccount.blob.core.windows.net/mycontainer \
    --dest-key <key> \
    --recursive \
    --exclude-older

# <a name="windowstabwindows"></a>[Windows](#tab/windows)
    AzCopy /Source:C:\myfolder /Dest:https://myaccount.blob.core.windows.net/mycontainer /DestKey: key /S /XO
---

## <a name="create-a-scheduled-task-or-cron-job"></a>Vytvořit úlohu naplánované úlohy nebo procesu cron 
Můžete vytvořit naplánovaných úloh/cron úlohu, která spouští skript AzCopy příkaz, který identifikuje a odesílá je nová místní data do cloudového úložiště v určitém časovém intervalu. 

Příkaz AzCopy zkopírujte do textového editoru. Aktualizujte hodnoty parametru příkazu AzCopy na odpovídající hodnoty. Uložte soubor jako `script.sh` nebo `script.bat` pro AzCopy na Linuxu a Windows v uvedeném pořadí. 

# <a name="linuxtablinux"></a>[Linux](#tab/linux)
    azcopy --source /mnt/myfiles --destination https://myaccount.blob.core.windows.net/mycontainer --dest-key <key> --recursive --exclude-older --exclude-newer --verbose >> Path/to/logfolder/`date +\%Y\%m\%d\%H\%M\%S`-cron.log

# <a name="windowstabwindows"></a>[Windows](#tab/windows)
    cd C:\Program Files (x86)\Microsoft SDKs\Azure\AzCopy
    AzCopy /Source: C:\myfolder  /Dest:https://myaccount.blob.core.windows.net/mycontainer /DestKey: key /V /XO /XN >C:\Path\to\logfolder\azcopy%date:~-4,4%%date:~-7,2%%date:~-10,2%%time:~-11,2%%time:~-8,2%%time:~-5,2%.log
---

AzCopy je spustit s podrobné `--verbose` (Linux) a `/V` možnost (Windows) a výstup se přesměruje do souboru protokolu. 

V tomto kurzu [Schtasks](https://msdn.microsoft.com/library/windows/desktop/bb736357(v=vs.85).aspx) se používá k vytvoření naplánované úlohy v systému Windows, a [Crontab](http://crontab.org/) příkaz se používá k vytvoření úlohy cron v systému Linux. 
 **Schtasks** umožňuje správci vytvořit, odstranit, dotaz, změnit, spuštění a ukončení naplánované úlohy v místním nebo vzdáleném počítači. **Cron** umožňuje uživatelům Linux a Unix ke spuštění příkazů nebo skriptů v zadané datum a čas pomocí [výraz cron](https://en.wikipedia.org/wiki/Cron#CRON_expression)


# <a name="linuxtablinux"></a>[Linux](#tab/linux)
**Chcete-li vytvořit úlohu cron v systému Linux**, zadejte následující příkaz na terminál. 

```bash
crontab -e 
*/5 * * * * sh /path/to/script.sh 
```

Určení výraz cron `*/5 * * * * ` v příkazu označuje skriptu prostředí `script.sh` by měly být provedeny každých pět minut. Tento skript můžete naplánovat na spuštění v určitém čase denně, měsíčně nebo ročně. V tématu [cron výrazy](https://en.wikipedia.org/wiki/Cron#CRON_expression) Další informace o nastavení datum a čas pro spuštění úloh. 

# <a name="windowstabwindows"></a>[Windows](#tab/windows)
**Chcete-li vytvořit naplánovanou úlohu v systému Windows**, zadejte následující příkaz na příkazový řádek nebo prostředí PowerShell.

```cmd 
schtasks /CREATE /SC minute /MO 5 /TN "AzCopy Script" /TR C:\Users\username\Documents\script.bat
```

Příkaz používá `/SC` parametr k určení minut plánu a `/MO` parametr k určení intervalu pět minut. `/TN` Parametr slouží k zadání názvu úlohy, a `/TR` parametru určete cestu k `script.bat` souboru. Navštivte [schtasks](https://technet.microsoft.com/library/cc772785(v=ws.10).aspx#BKMK_minutes) Další informace o vytvoření naplánované úlohy v systému Windows.

---
 
K ověření naplánované úlohy/cron provede úlohu správně, vytvořte nové soubory ve vašem adresáři `myfolder`. Počkejte pět minut a ověřte, zda že byly nové soubory nahrány do účtu úložiště. Přejděte do adresáře protokolu zobrazit protokoly pro výstup úlohy naplánované úlohy nebo procesu cron. 

Navštivte [přesunout na místní datové](https://docs.microsoft.com/azure/storage/common/storage-moving-data?toc=%2fazure%2fstorage%2ffiles%2ftoc.json) Další informace o způsoby, jak přesunout místní data do úložiště Azure a naopak.  

## <a name="next-steps"></a>Další postup
Další informace o Azure Storage a AzCopy najdete v následujících zdrojích informací:

### <a name="azure-storage-documentation"></a>Dokumentace k Azure Storage:
* [Úvod do Azure Storage](../storage-introduction.md)
* [Přenos dat pomocí nástroje AzCopy v systému Windows](storage-use-azcopy.md) 
* [Přenos dat pomocí nástroje AzCopy v systému Linux](storage-use-azcopy-linux.md) 
* [vytvořit účet úložiště](../storage-create-storage-account.md)
* [Spravovat objekty BLOB pomocí Průzkumníka úložiště](https://docs.microsoft.com/azure/vs-azure-tools-storage-explorer-blobs)  






 
 

