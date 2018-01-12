---
title: "Migrace místních dat do služby Azure Storage pomocí AzCopy | Microsoft Docs"
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
ms.openlocfilehash: 3dbfb935ac0b134e127a5dccb7bc76716c688e8a
ms.sourcegitcommit: c4cc4d76932b059f8c2657081577412e8f405478
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/11/2018
---
#  <a name="migrate-on-premises-data-to-cloud-storage-by-using-azcopy"></a>Migrace místních dat do cloudového úložiště pomocí nástroje AzCopy

AzCopy je nástroj příkazového řádku pro kopírování dat do nebo z úložiště objektů Azure Blob, soubory Azure a Azure Table storage, pomocí jednoduchých příkazů. Příkazy jsou navrženy pro optimální výkon. Může kopírovat data mezi systém souborů a účet úložiště nebo mezi účty úložiště.  

Dvě verze AzCopy můžete stáhnout:

* [AzCopy v systému Linux](storage-use-azcopy.md) sestavena pomocí rozhraní .NET Framework Core. Jeho cílem platformy Linux prostřednictvím nabídky stylu POSIX možnosti příkazového řádku. 
* [AzCopy v systému Windows](../storage-use-azcopy.md) sestavena pomocí rozhraní .NET Framework. Nabízí možnosti příkazového řádku Windows stylu. 
 
V tomto kurzu se naučíte:

> [!div class="checklist"]
> * Vytvoření účtu úložiště. 
> * Pomocí nástroje AzCopy nahrát všechna vaše data.
> * Upravte data pro účely testování.
> * Vytvoření naplánované úlohy nebo procesu cron úlohy k identifikaci nové soubory nahrát.

## <a name="prerequisites"></a>Požadavky

K dokončení tohoto kurzu, stáhněte si nejnovější verzi AzCopy na [Linux](https://docs.microsoft.com/azure/storage/common/storage-use-azcopy-linux#download-and-install-azcopy) nebo [Windows](http://aka.ms/downloadazcopy). 

[!INCLUDE [storage-quickstart-tutorial-create-account-portal](../../../includes/storage-quickstart-tutorial-create-account-portal.md)]

>[!NOTE]
>Pokud chcete být schopni stáhnout objekty BLOB ze sekundární oblasti do místního úložiště a naopak a nastavte **replikace** k **čtení access geograficky redundantní úložiště**. Výběrem této možnosti vytvoříte [geograficky redundantní úložiště](https://docs.microsoft.com/azure/storage/common/storage-redundancy#geo-redundant-storage) účtu. 
>
>

## <a name="create-a-container"></a>Vytvoření kontejneru

Objekty blob se vždy nahrávají do kontejneru. Kontejnery můžete použít k uspořádání skupin objektů BLOB jako uspořádání souborů ve vašem počítači ve složkách. 

Postupujte podle těchto kroků můžete vytvořit kontejner:

1. Vyberte **účty úložiště** tlačítko na hlavní stránce a vyberte účet úložiště, který jste vytvořili.
2. Vyberte **objekty BLOB** pod **služby**a potom vyberte **kontejneru**. 

   ![Vytvoření kontejneru](media/storage-azcopy-migrate-on-premises-data/CreateContainer.png)
 
Názvy kontejnerů musí začínat písmenem nebo číslicí. Se může obsahovat pouze písmena, číslice a znak pomlčka (-). Další informace o pojmenování kontejnerů a objektů blob najdete v tématu [Názvy kontejnerů, objektů blob a metadat a odkazování na ně](/rest/api/storageservices/naming-and-referencing-containers--blobs--and-metadata).

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

Nahraďte `<key>` a `key` nahraďte svým klíčem účtu. Na portálu Azure můžete načíst klíč účtu výběrem **přístupové klíče** pod **nastavení** ve vašem účtu úložiště. Vyberte klíč a vložte jej do příkazu AzCopy. Pokud zadaný cílový kontejner neexistuje, AzCopy ji vytvoří a odešle soubor do ní. Aktualizujte cestu ke zdrojovému adresáři dat a nahraďte **stránku Můj účet** v cílová adresa URL s názvem svého účtu úložiště.

Chcete-li nahrát obsah zadaného adresáře k rekurzivnímu úložiště objektů Blob, zadejte `--recursive` (Linux) nebo `/S` možnost (Windows). Když spustíte nástroj AzCopy s jedním z těchto možností, všechny podsložky a jejich soubory jsou také odeslány.

## <a name="upload-modified-files-to-blob-storage"></a>Nahrání upravených souborů do úložiště objektů Blob
Můžete použít AzCopy k [nahrání souborů](https://docs.microsoft.com/azure/storage/common/storage-use-azcopy#other-azcopy-features) podle jejich čas poslední změny. Zkuste to, upravovat nebo vytvářet nové soubory zdrojový adresář pro účely testování. Odesílat pouze aktualizované nebo nové soubory, přidejte `--exclude-older` (Linux) nebo `/XO` (Windows) parametr k příkazu AzCopy.

Pokud chcete kopírovat zdroje prostředky, které neexistují v cílovém, zadat oba `--exclude-older` a `--exclude-newer` (Linux) nebo `/XO` a `/XN` parametry (Windows) v příkazu AzCopy. AzCopy ukládání pouze aktualizovaná data, podle časového razítka.
 
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
Můžete vytvořit naplánovanou úlohu nebo cron úlohu, která běží AzCopy příkazu skriptu. Skript identifikuje a odesílá je nová místní data do cloudového úložiště v určitém časovém intervalu. 

Příkaz AzCopy zkopírujte do textového editoru. Aktualizujte hodnoty parametru příkazu AzCopy na odpovídající hodnoty. Uložte soubor jako `script.sh` (Linux) nebo `script.bat` (Windows) pro AzCopy. 

# <a name="linuxtablinux"></a>[Linux](#tab/linux)
    azcopy --source /mnt/myfiles --destination https://myaccount.blob.core.windows.net/mycontainer --dest-key <key> --recursive --exclude-older --exclude-newer --verbose >> Path/to/logfolder/`date +\%Y\%m\%d\%H\%M\%S`-cron.log

# <a name="windowstabwindows"></a>[Windows](#tab/windows)
    cd C:\Program Files (x86)\Microsoft SDKs\Azure\AzCopy
    AzCopy /Source: C:\myfolder  /Dest:https://myaccount.blob.core.windows.net/mycontainer /DestKey: key /V /XO /XN >C:\Path\to\logfolder\azcopy%date:~-4,4%%date:~-7,2%%date:~-10,2%%time:~-11,2%%time:~-8,2%%time:~-5,2%.log
---

AzCopy běží s podrobné `--verbose` (Linux) nebo `/V` možnost (Windows). Výstup se přesměruje do souboru protokolu. 

V tomto kurzu [Schtasks](https://msdn.microsoft.com/library/windows/desktop/bb736357(v=vs.85).aspx) se používá k vytvoření naplánované úlohy v systému Windows. [Crontab](http://crontab.org/) příkaz se používá k vytvoření úlohy cron v systému Linux. 
 **Schtasks** umožňuje správci vytvořit, odstranit, dotaz, změnit, spuštění a ukončení naplánované úlohy v místním nebo vzdáleném počítači. **Cron** umožňuje uživatelům Linux a Unix ke spuštění příkazů nebo skriptů v zadané datum a čas pomocí [cron výrazy](https://en.wikipedia.org/wiki/Cron#CRON_expression).


# <a name="linuxtablinux"></a>[Linux](#tab/linux)
Chcete-li vytvořit úlohu cron v systému Linux, zadejte následující příkaz na terminál: 

```bash
crontab -e 
*/5 * * * * sh /path/to/script.sh 
```

Určení výraz cron `*/5 * * * * ` v příkazu znamená, že skript prostředí `script.sh` měly být spuštěny každých pět minut. Můžete naplánovat skript, který chcete spustit v určitém čase denně, měsíčně nebo ročně. Další informace o nastavení datum a čas pro spuštění úloh najdete v tématu [cron výrazy](https://en.wikipedia.org/wiki/Cron#CRON_expression). 

# <a name="windowstabwindows"></a>[Windows](#tab/windows)
Pokud chcete vytvořit naplánovanou úlohu v systému Windows, zadejte následující příkaz na příkazovém řádku nebo v prostředí PowerShell:

```cmd 
schtasks /CREATE /SC minute /MO 5 /TN "AzCopy Script" /TR C:\Users\username\Documents\script.bat
```

Příkaz používá:
- `/SC` Parametru určete plán minutu.
- `/MO` Parametr k určení intervalu pět minut.
- `/TN` Parametru určete název úlohy.
- `/TR` Parametru určete cestu k `script.bat` souboru. 

Další informace o vytvoření naplánované úlohy v systému Windows, najdete v části [Schtasks](https://technet.microsoft.com/library/cc772785(v=ws.10).aspx#BKMK_minutes).

---
 
Chcete-li ověřit, že úloha naplánované úlohy nebo procesu cron běží správně, vytvořte nové soubory ve vašem `myfolder` adresáře. Počkejte, než pět minut potvrďte, že byly nové soubory nahrány do účtu úložiště. Přejděte do adresáře protokolu zobrazit protokoly pro výstup naplánované úlohy nebo procesu cron úlohy. 

Další informace o způsoby, jak přesunout místní data do služby Azure Storage a naopak najdete v tématu [přesun dat do a ze služby Azure Storage](https://docs.microsoft.com/azure/storage/common/storage-moving-data?toc=%2fazure%2fstorage%2ffiles%2ftoc.json).  

## <a name="next-steps"></a>Další postup
Další informace o Azure Storage a AzCopy najdete v následujících zdrojích informací:

* [Úvod do Azure Storage](../storage-introduction.md)
* [Přenos dat pomocí nástroje AzCopy v systému Windows](storage-use-azcopy.md) 
* [Přenos dat pomocí nástroje AzCopy v systému Linux](storage-use-azcopy-linux.md) 
* [vytvořit účet úložiště](../storage-create-storage-account.md)
* [Spravovat objekty BLOB pomocí Průzkumníka úložiště](https://docs.microsoft.com/azure/vs-azure-tools-storage-explorer-blobs)  






 
 

