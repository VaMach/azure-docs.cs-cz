---
title: "Jak používat úložiště objektů Blob (úložiště objektů) z Ruby | Microsoft Docs"
description: "Ukládejte nestrukturovaná data v cloudu pomocí Azure Blob Storage (úložiště objektů)."
services: storage
documentationcenter: ruby
author: tamram
manager: timlt
editor: tysonn
ms.assetid: e2fe4c45-27b0-4d15-b3fb-e7eb574db717
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: ruby
ms.topic: article
ms.date: 12/08/2016
ms.author: tamram
ms.openlocfilehash: 2c1534dcbb0e26ecdff7c057efb5094c60b5c5b7
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/11/2017
---
# <a name="how-to-use-blob-storage-from-ruby"></a>Používání úložiště Blob z Ruby
[!INCLUDE [storage-selector-blob-include](../../../includes/storage-selector-blob-include.md)]

[!INCLUDE [storage-try-azure-tools-queues](../../../includes/storage-try-azure-tools-blobs.md)]

## <a name="overview"></a>Přehled
Úložiště objektů blob v Azure je služba, která ukládá nestrukturovaná data v cloudu jako objekty nebo objekty blob. Do Blob storage se dá ukládat jakýkoli druh textu nebo binárních dat, jako je dokument, soubor médií nebo instalátor aplikace. Blob storage se také nazývá úložiště objektů.

Tento průvodce vám ukáže, jak provádět běžné scénáře s využitím úložiště objektů Blob. Ukázky jsou zapsány pomocí rozhraní API Ruby. Pokryté scénáře zahrnují **odesílání, výpis, stahování,** a **odstraňování** objekty BLOB.

[!INCLUDE [storage-blob-concepts-include](../../../includes/storage-blob-concepts-include.md)]

[!INCLUDE [storage-create-account-include](../../../includes/storage-create-account-include.md)]

## <a name="create-a-ruby-application"></a>Vytvoření Ruby aplikace
Vytvořte aplikaci pro poznámky Ruby. Pokyny najdete v tématu [Ruby, na které webové aplikace ve virtuálním počítači Azure](../../virtual-machines/linux/classic/virtual-machines-linux-classic-ruby-rails-web-app.md)

## <a name="configure-your-application-to-access-storage"></a>Konfigurace aplikace pro přístup k úložišti
Pokud chcete používat Azure Storage, musíte stáhnout a použít Ruby azure balíčku, který obsahuje sadu knihoven pohodlí, které komunikují s služby REST úložiště.

### <a name="use-rubygems-to-obtain-the-package"></a>Použití RubyGems získat balíček
1. Pomocí rozhraní příkazového řádku, jako například **prostředí PowerShell** (Windows), **Terminálové** (Mac), nebo **Bash** (Unix).
2. "Gem instalace azure" zadejte v příkazovém okně instalace gem a závislostí.

### <a name="import-the-package"></a>Import balíčku
Na začátek souboru Ruby, kde máte v úmyslu používat úložiště pomocí svém oblíbeném textovém editoru, přidejte následující:

```ruby
require "azure"
```

## <a name="set-up-an-azure-storage-connection"></a>Nastavit připojení k Azure Storage
Modul azure, bude číst proměnné prostředí **AZURE\_úložiště\_účet** a **AZURE\_úložiště\_ACCESS_KEY** informace požadované pro připojení k účtu úložiště Azure. Pokud nejsou nastavené těchto proměnných prostředí, je nutné zadat informace o účtu před použitím **Azure::Blob::BlobService** následujícím kódem:

```ruby
Azure.config.storage_account_name = "<your azure storage account>"
Azure.config.storage_access_key = "<your azure storage access key>"
```

K získání těchto hodnot z klasický nebo účet správce prostředků úložiště na portálu Azure:

1. Přihlaste se k portálu [Azure Portal](https://portal.azure.com).
2. Přejděte na účet úložiště, který chcete použít.
3. V okně nastavení na pravé straně klikněte na tlačítko **přístupové klíče**.
4. V okně klíče přístup, který se zobrazí uvidíte přístupový klíč 1 a 2 přístupový klíč. Můžete použít kteroukoli z nich.
5. Kliknutím na ikonu kopírování do schránky zkopírujte klíč.

## <a name="create-a-container"></a>Vytvoření kontejneru
[!INCLUDE [storage-container-naming-rules-include](../../../includes/storage-container-naming-rules-include.md)]

**Azure::Blob::BlobService** objekt vám umožňuje spolupracovat s kontejnery a objekty BLOB. Chcete-li vytvořit kontejner, použijte **vytvořit\_container()** metoda.

Následující příklad kódu vytvoří kontejner nebo vytiskne chybu, pokud existuje.

```ruby
azure_blob_service = Azure::Blob::BlobService.new
begin
    container = azure_blob_service.create_container("test-container")
rescue
    puts $!
end
```

Pokud chcete, aby soubory v kontejneru veřejného, můžete nastavit oprávnění kontejneru.

Můžete upravit pouze <strong>vytvořit\_container()</strong> volání předat **: veřejné\_přístup\_úroveň** možnost:

```ruby
container = azure_blob_service.create_container("test-container",
    :public_access_level => "<public access level>")
```

Platné hodnoty pro **: veřejné\_přístup\_úroveň** možnosti jsou:

* **objekt BLOB:** Určuje veřejný přístup pro čtení pro objekty BLOB. Data objektů BLOB v tomto kontejneru může číst přes anonymní žádost, ale kontejneru data nejsou k dispozici. Klienty nelze vytvořit výčet objektů BLOB v kontejneru přes anonymní žádost.
* **kontejner:** Určuje úplné veřejný přístup pro čtení pro data kontejnerů a objektů blob. Klienti, můžete vytvořit výčet objektů BLOB v kontejneru přes anonymní žádost, ale nemůže vytvořit výčet kontejnery v rámci účtu úložiště.

Alternativně můžete upravit úroveň veřejný přístup kontejner pomocí **nastavit\_kontejneru\_acl()** metoda k určení úrovně veřejný přístup.

Následující příklad kódu se změní na úroveň veřejný přístup **kontejneru**:

```ruby
azure_blob_service.set_container_acl('test-container', "container")
```

## <a name="upload-a-blob-into-a-container"></a>Nahrání objektu blob do kontejneru
Chcete-li nahrát obsah do objektu blob, použijte **vytvořit\_bloku\_blob()** metodu pro vytvoření objektu blob, použijte jako obsah objektu blob souboru nebo řetězec.

Následující kód nahrávání souboru **test.png** jako nový blob s názvem "– Objekt blob image" v kontejneru.

```ruby
content = File.open("test.png", "rb") { |file| file.read }
blob = azure_blob_service.create_block_blob(container.name,
    "image-blob", content)
puts blob.name
```

## <a name="list-the-blobs-in-a-container"></a>Zobrazí seznam objektů blob v kontejneru
K zobrazení seznamu kontejnery, použijte **list_containers()** metoda.
K zobrazení seznamu objektů BLOB v kontejneru, použijte **seznamu\_blobs()** metoda.

To výstupy adresy URL všech objektů BLOB v všechny kontejnery pro účet.

```ruby
containers = azure_blob_service.list_containers()
containers.each do |container|
    blobs = azure_blob_service.list_blobs(container.name)
    blobs.each do |blob|
    puts blob.name
    end
end
```

## <a name="download-blobs"></a>Stáhnout objekty blob
Chcete-li stáhnout objekty BLOB, použijte **získat\_blob()** metoda můžete načíst obsah.

Následující příklad kódu ukazuje, jak pomocí **získat\_blob()** ke stažení obsahu "Objekt blob image" a zapisovat do místního souboru.

```ruby
blob, content = azure_blob_service.get_blob(container.name,"image-blob")
File.open("download.png","wb") {|f| f.write(content)}
```

## <a name="delete-a-blob"></a>Odstranit objekt Blob
Nakonec pokud chcete odstranit objekt blob, použijte **odstranit\_blob()** metoda. Následující příklad kódu ukazuje, jak odstranit objekt blob.

```ruby
azure_blob_service.delete_blob(container.name, "image-blob")
```

## <a name="next-steps"></a>Další kroky
Další informace o složitějších úlohách úložiště najdete na následujících odkazech:

* [Blog týmu Azure Storage](http://blogs.msdn.com/b/windowsazurestorage/)
* [Azure SDK pro Ruby](https://github.com/WindowsAzure/azure-sdk-for-ruby) úložišti na Githubu
* [Přenos dat pomocí nástroje příkazového řádku AzCopy](../common/storage-use-azcopy.md?toc=%2fazure%2fstorage%2fblobs%2ftoc.json)

