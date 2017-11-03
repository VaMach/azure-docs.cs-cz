---
title: "Azure rychlý start - objekty přenosu do nebo z Azure Blob storage pomocí Python | Microsoft Docs"
description: "Naučte se rychle převést objekty do a z Azure Blob storage pomocí Python"
services: storage
documentationcenter: storage
author: ruthogunnnaike
manager: cwatson
editor: tysonn
ms.assetid: 
ms.custom: mvc
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: quickstart
ms.date: 10/12/2017
ms.author: v-ruogun
ms.openlocfilehash: 44ec416a814ff6a5fef79ef21e2f54ce4ce4da17
ms.sourcegitcommit: 9ae92168678610f97ed466206063ec658261b195
ms.translationtype: HT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/17/2017
---
#  <a name="transfer-objects-tofrom-azure-blob-storage-using-python"></a>Objekty přenosu do nebo z Azure Blob storage pomocí Python
V tento rychlý start a zjistěte, jak používat Python k odesílání, stahování a seznam objektů BLOB bloku v kontejneru v úložiště objektů Blob v Azure. 

## <a name="prerequisites"></a>Požadavky

K provedení kroků v tomto kurzu Rychlý start je potřeba: 
* Nainstalujte [Python](https://www.python.org/downloads/)
* Stáhněte a nainstalujte [sada SDK úložiště Azure pro jazyk Python](storage-python-how-to-use-blob-storage.md#download-and-install-azure-storage-sdk-for-python). 

Pokud ještě nemáte předplatné Azure, vytvořte si [bezplatný účet](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) před tím, než začnete.

## <a name="create-a-storage-account-using-the-azure-portal"></a>Vytvořit účet úložiště pomocí portálu Azure

Nejdřív vytvořte nový účet úložiště pro obecné účely pro tento rychlý start. 

1. Přejděte na [portál Azure](https://portal.azure.com) a přihlaste se pomocí účtu Azure. 
2. V nabídce centra vyberte **nový** > **úložiště** > **účet úložiště – objekt blob, soubor, tabulka, fronta**. 
3. Zadejte název účtu úložiště. Název musí být v rozmezí 3 až 24 znaků a může obsahovat jenom číslice a malá písmena. Také musí být jedinečný.
4. Nastavit `Deployment model` k **správce prostředků**.
5. Nastavit `Account kind` k **obecné účely**.
6. Nastavit `Performance` k **standardní**. 
7. Nastavit `Replication` k **místně redundantní úložiště (LRS)**.
8. Nastavit `Storage service encryption` k **zakázané**.
9. Nastavit `Secure transfer required` k **zakázané**.
10. Vyberte své předplatné. 
11. Pro `resource group`, vytvořte novou a zadat jedinečný název. 
12. Vyberte `Location` pro váš účet úložiště.
13. Zkontrolujte **připnout na řídicí panel** a klikněte na tlačítko **vytvořit** k vytvoření účtu úložiště. 

Po vytvoření účtu úložiště je připnutá na řídicí panel. Klikněte na ho otevřete. V části **nastavení**, klikněte na tlačítko **přístupové klíče**. Vyberte klíč a název účtu úložiště zkopírujte do schránky a pak ho vložte do poznámkového bloku pro pozdější použití.

## <a name="download-the-sample-application"></a>Stažení ukázkové aplikace
[Ukázkové aplikace](https://github.com/Azure-Samples/storage-blobs-python-quickstart.git) používán tento rychlý start je základní aplikace Python.  

Použití [git](https://git-scm.com/) stáhnout kopii aplikace na svoje vývojové prostředí. 

```bash
git clone https://github.com/Azure-Samples/storage-blobs-python-quickstart.git 
```

Tento příkaz provede klonování úložiště do složky místní git. Chcete-li spustit Python program, vyhledejte složku úložiště objektů BLOB python quickstart a example.py souboru.  

## <a name="configure-your-storage-connection-string"></a>Konfigurace připojovacího řetězce úložiště
V aplikaci, je nutné zadat klíč účtu úložiště název a účet k vytvoření `BlockBlobService` objektu. Otevřete `example.py` soubor v Průzkumníku řešení ve vaší IDE. Nahraďte **accountname** a **accountkey** hodnoty s názvem účtu a klíč. 

```python 
block_blob_service = BlockBlobService(account_name='accountname', account_key='accountkey') 
```

## <a name="run-the-sample"></a>Spustit ukázku
Tato ukázka vytvoří testovací soubor ve složce "Dokumenty". Ukázka programu nahrávání souboru testu do úložiště objektů Blob, obsahuje seznam objektů BLOB v kontejneru a stáhne soubor pod novým názvem. 

Spusťte ukázku. Tento výstup je příklad výstupu vráceného při spuštění aplikace:
  
```
Temp file = C:\Users\azureuser\Documents\QuickStart_9f4ed0f9-22d3-43e1-98d0-8b2c05c01078.txt

Uploading to Blob storage as blobQuickStart_9f4ed0f9-22d3-43e1-98d0-8b2c05c01078.txt

List blobs in the container
         Blob name: QuickStart_9f4ed0f9-22d3-43e1-98d0-8b2c05c01078.txt

Downloading blob to C:\Users\azureuser\Documents\QuickStart_9f4ed0f9-22d3-43e1-98d0-8b2c05c01078_DOWNLOADED.txt
```
Pokud můžete stisknutím libovolné klávesy, chcete-li pokračovat, odstraní ukázka programu kontejner úložiště a soubory. Než budete pokračovat, zkontrolujte složku "Dokumenty" pro dva soubory. Můžete je otevřít a zjistit, že jsou identické.

Můžete také použít nástroj, jako [Azure Storage Explorer](http://storageexplorer.com) k prohlížení souborů v Blob storage. Azure Storage Explorer je bezplatný nástroj napříč platformami, který umožňuje zobrazit informace o účtu úložiště. 

Jakmile ověříte soubory, stisknutí libovolné klávesy ukončíte ukázku a odstranit testovací soubory. Teď, když víte, jaké ukázku, otevřete soubor example.py podívejte se na kód. 

## <a name="get-references-to-the-storage-objects"></a>Získat odkazy na objekty úložiště
První věc udělat, je vytvořit odkazy na objekty používané pro přístup k a spravovat úložiště objektů Blob. Tyto objekty sestavení na sobě navzájem, a každou používá následuje v seznamu.

* Vytvoření instance **BlockBlobService** objekt, který odkazuje na službu objektů Blob v účtu úložiště. 

* Vytvoření instance **CloudBlobContainer** objekt, který reprezentuje kontejneru se připojujete. Kontejnery se používají k uspořádání objektů BLOB, jako jsou použít složek ve vašem počítači k uspořádání souborů.

Jakmile máte kontejneru objektů Blob v cloudu, můžete vytvořit instanci **CloudBlockBlob** objekt, který odkazuje na konkrétní objekt blob, ve kterém zajímá a provádět operace, jako je například odesílání, stahování a kopírování.

V této části vytváření instancí objektů, vytvořte nový kontejner a potom nastavit oprávnění na kontejner, aby se veřejné objekty BLOB. Kontejner se nazývá **quickstartblobs**. 


```python 
# Create the BlockBlockService that is used to call the Blob service for the storage account
block_blob_service = BlockBlobService(account_name='accountname', account_key='accountkey') 
 
# Create a container called 'quickstartblobs'.
container_name ='quickstartblobs'
block_blob_service.create_container(container_name) 

# Set the permission so the blobs are public.
block_blob_service.set_container_acl(container_name, public_access=PublicAccess.Container)
```
## <a name="upload-blobs-to-the-container"></a>Odešlete do kontejneru objektů BLOB

Úložiště objektů blob podporuje objekty blob bloku, doplňovací objekty blob a objekty blob stránky. Objekty BLOB bloku jsou nejčastěji používané a který se bude používat v tento rychlý start.  

Pokud chcete nahrát soubor do objektu blob, získáte úplnou cestu k souboru spojením název adresáře a název souboru na místní disk. Potom můžete nahrát soubor do zadané cesty pomocí **vytvořit\_objektů blob\_z\_cesta** metoda. 

Ukázkový kód vytvoří místní soubor má být použit pro nahrávání a stahování, uložení souboru k odeslání jako **soubor\_cesta\_k\_soubor** a název objektu blob jako **místní\_soubor\_název**. Následující příklad odešle soubor do kontejneru s názvem **quickstartblobs**.

```python
# Create a file in Documents to test the upload and download.
local_path=os.path.expanduser("~\Documents")
local_file_name ="QuickStart_" + str(uuid.uuid4()) + ".txt"
full_path_to_file =os.path.join(local_path, local_file_name)

# Write text to the file.
file = open(full_path_to_file,  'w')
file.write("Hello, World!")
file.close()

print("Temp file = " + full_path_to_file)
print("\nUploading to Blob storage as blob" + local_file_name)

# Upload the created file, use local_file_name for the blob name
block_blob_service.create_blob_from_path(container_name, local_file_name, full_path_to_file)
```

Existuje několik metod nahrávání, které můžete použít pomocí úložiště objektů Blob. Například pokud máte datový proud paměti, můžete použít **vytvořit\_objektů blob\_z\_datového proudu** metoda místo **vytvořit\_blob\_z\_cestu**. 

Objekty BLOB bloku může být tak velké, jaká 4.7 TB a může být jakýkoli z tabulky aplikace Excel video velkých souborů. Objekty BLOB stránky se primárně používají pro soubory VHD použité pro zálohování virtuálních počítačů IaaS. Append – objekty BLOB jsou použitá pro protokolování, například když chcete zapsat do souboru a potom mít přidání další informace. Většina objekty uložené v úložišti objektů Blob jsou objekty BLOB bloku.

## <a name="list-the-blobs-in-a-container"></a>Zobrazí seznam objektů blob v kontejneru

Získat seznam souborů v kontejneru pomocí **list_blobs** metoda. Tato metoda vrátí generátor. Následující kód načte seznam objektů BLOB a potom je, prochází zobrazující názvy objektů BLOB nalezené v kontejneru.  

```python
# List the blobs in the container
print("\nList blobs in the container")
    generator = block_blob_service.list_blobs(container_name)
    for blob in generator:
        print("\t Blob name: " + blob.name)
```

## <a name="download-the-blobs"></a>Stáhnout objekty BLOB

Stáhnout objekty BLOB na váš místní disk pomocí **získat\_objektů blob\_k\_cesta** metoda. Následující kód stáhne objekt blob nahráli v předchozí části. "_DOWNLOADED" se přidá jako příponu názvu objektu blob, abyste viděli oba soubory na místním disku. 

```python
# Download the blob(s).
# Add '_DOWNLOADED' as prefix to '.txt' so you can see both files in Documents.
full_path_to_file2 = os.path.join(local_path, string.replace(local_file_name ,'.txt', '_DOWNLOADED.txt'))
print("\nDownloading blob to " + full_path_to_file2)
block_blob_service.get_blob_to_path(container_name, local_file_name, full_path_to_file2)
```

## <a name="clean-up-resources"></a>Vyčištění prostředků
Pokud již nepotřebujete objekty BLOB nahrát tento rychlý start, můžete odstranit celou kontejneru pomocí **odstranit\_kontejneru**. Pokud již nejsou potřebné soubory vytvořené, můžete použít **odstranit\_blob** metoda odstranění souborů.

```python
# Clean up resources. This includes the container and the temp files
block_blob_service.delete_container(container_name)
os.remove(full_path_to_file)
os.remove(full_path_to_file2)
```

## <a name="next-steps"></a>Další kroky
 
V tento rychlý start zjistili, jak pro přenos souborů mezi místní disk a Azure blob storage pomocí Python. Další informace o práci s úložišti objektů blob, pokračujte do úložiště objektů Blob postupy.

> [!div class="nextstepaction"]
> [Postupy operace úložiště objektů BLOB](./storage-python-how-to-use-blob-storage.md)
 

Další informace o Storage Explorer a objekty BLOB najdete v tématu [prostředků úložiště Azure Blob spravovat pomocí Storage Exploreru](../../vs-azure-tools-storage-explorer-blobs.md?toc=%2fazure%2fstorage%2fblobs%2ftoc.json).
