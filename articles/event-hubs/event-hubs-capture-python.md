---
title: "Azure Event Hubs zaznamenat návod | Microsoft Docs"
description: "Ukázka pomocí sady Azure SDK pro Python, která demonstruje použití funkci zachycení centra událostí."
services: event-hubs
documentationcenter: 
author: djrosanova
manager: timlt
editor: 
ms.assetid: bdff820c-5b38-4054-a06a-d1de207f01f6
ms.service: event-hubs
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/05/2017
ms.author: sethm
ms.openlocfilehash: cdbb2baea2bc6c45908369ff821c264b66053d95
ms.sourcegitcommit: 6f33adc568931edf91bfa96abbccf3719aa32041
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 12/22/2017
---
# <a name="event-hubs-capture-walkthrough-python"></a>Názorný postup zachycení centra událostí: Python

Zaznamenat centra událostí je funkce služby Event Hubs, která umožňuje automaticky doručování streamování dat v Centru událostí k účtu úložiště objektů Blob v Azure podle svého výběru. Tato funkce umožňuje snadno provádět, dávkového zpracování na data v reálném čase streamování. Tento článek popisuje, jak používat zaznamenat centra událostí s Python. Další informace o zachycení centra událostí najdete v tématu [článek s přehledem](event-hubs-capture-overview.md).

V tomto příkladu [Azure Python SDK](https://azure.microsoft.com/develop/python/) k předvedení funkci zachycení. Sender.py program odešle simulovanou telemetrii prostředí do centra událostí ve formátu JSON. Centrum událostí je nakonfigurované použití funkce zachycení zápis tato data do úložiště objektů blob v dávkách. Aplikace capturereader.py pak přečte tyto objekty BLOB a vytvoří soubor připojení na zařízení a pak zapíše data do souborů CSV.

## <a name="what-will-be-accomplished"></a>Co všechno zvládneme

1. Vytvořte účet Azure Blob Storage a kontejner objektů blob v něm, pomocí portálu Azure.
2. Vytvořte na obor názvů centra událostí, pomocí portálu Azure.
3. Vytvoření centra událostí s funkci zachycení povoleno, pomocí portálu Azure.
4. Posílat data do centra událostí se skript v jazyce Python.
5. Čtení souborů z zachytávání a zpracovat je jiný skript v jazyce Python.

## <a name="prerequisites"></a>Požadavky

- Python 2.7.x
- Předplatné Azure
- Aktivní [Event Hubs obor názvů a události rozbočovače.](event-hubs-create.md)

[!INCLUDE [create-account-note](../../includes/create-account-note.md)]

## <a name="create-an-azure-storage-account"></a>Vytvoření účtu služby Azure Storage
1. Přihlaste se k webu [Azure Portal][Azure portal].
2. V levém navigačním podokně portálu klikněte na **nový**, pak klikněte na tlačítko **úložiště**a potom klikněte na **účet úložiště**.
3. Vyplňte pole v okně účtu úložiště a pak klikněte na tlačítko **vytvořit**.
   
   ![][1]
4. Jakmile ověříte **úspěšné nasazení** zprávy, klikněte na název nového účtu úložiště a v **Essentials** okně klikněte na tlačítko **objekty BLOB**. Když **služba objektů Blob** okno otevře, klikněte na tlačítko **+ kontejner** v horní části. Název kontejneru **zaznamenat**, pak zavřete **služba objektů Blob** okno.
5. Klikněte na tlačítko **přístupové klíče** v levém okně a zkopírujte název účtu úložiště a hodnotu **key1**. Uložte tyto hodnoty do programu Poznámkový blok nebo některé dočasné umístění.

## <a name="create-a-python-script-to-send-events-to-your-event-hub"></a>Vytvořit skript v jazyce Python odesílat události do vašeho centra událostí
1. Otevřete svém oblíbeném editoru Python, jako například [Visual Studio Code][Visual Studio Code].
2. Vytvořte skript volá **sender.py**. Tento skript odesílá 200 události do vašeho centra událostí. Jsou jednoduché prostředí odečty odeslaných ve formátu JSON.
3. Vložte následující kód do sender.py:
   
  ```python
  import uuid
  import datetime
  import random
  import json
  from azure.servicebus import ServiceBusService
   
  sbs = ServiceBusService(service_namespace='INSERT YOUR NAMESPACE NAME', shared_access_key_name='RootManageSharedAccessKey', shared_access_key_value='INSERT YOUR KEY')
  devices = []
  for x in range(0, 10):
      devices.append(str(uuid.uuid4()))
   
  for y in range(0,20):
      for dev in devices:
          reading = {'id': dev, 'timestamp': str(datetime.datetime.utcnow()), 'uv': random.random(), 'temperature': random.randint(70, 100), 'humidity': random.randint(70, 100)}
          s = json.dumps(reading)
          sbs.send_event('INSERT YOUR EVENT HUB NAME', s)
      print y
  ```
4. Aktualizujte předchozí kód, který použije váš obor názvů, hodnota klíče a název centra událostí, který jste získali při vytvoření oboru názvů služby Event Hubs.

## <a name="create-a-python-script-to-read-your-capture-files"></a>Vytvořit skript v jazyce Python číst vaše soubory zachytávání

1. Vyplňte okno a klikněte na tlačítko **vytvořit**.
2. Vytvořte skript volá **capturereader.py**. Tento skript načte zaznamenané soubory a vytvoří soubor na zařízení k zápisu dat pouze pro toto zařízení.
3. Vložte následující kód do capturereader.py:
   
  ```python
  import os
  import string
  import json
  import avro.schema
  from avro.datafile import DataFileReader, DataFileWriter
  from avro.io import DatumReader, DatumWriter
  from azure.storage.blob import BlockBlobService
   
  def processBlob(filename):
      reader = DataFileReader(open(filename, 'rb'), DatumReader())
      dict = {}
      for reading in reader:
          parsed_json = json.loads(reading["Body"])
          if not 'id' in parsed_json:
              return
          if not dict.has_key(parsed_json['id']):
              list = []
              dict[parsed_json['id']] = list
          else:
              list = dict[parsed_json['id']]
              list.append(parsed_json)
      reader.close()
      for device in dict.keys():
          deviceFile = open(device + '.csv', "a")
          for r in dict[device]:
              deviceFile.write(", ".join([str(r[x]) for x in r.keys()])+'\n')
   
  def startProcessing(accountName, key, container):
      print 'Processor started using path: ' + os.getcwd()
      block_blob_service = BlockBlobService(account_name=accountName, account_key=key)
      generator = block_blob_service.list_blobs(container)
      for blob in generator:
          #content_length == 508 is an empty file, so only process content_length > 508 i.e. skip  empty files
          if blob.properties.content_length > 508:
              print('Downloaded a non empty blob: ' + blob.name)
              cleanName = string.replace(blob.name, '/', '_')
              block_blob_service.get_blob_to_path(container, blob.name, cleanName)
              processBlob(cleanName)
              os.remove(cleanName)
          block_blob_service.delete_blob(container, blob.name)
  startProcessing('YOUR STORAGE ACCOUNT NAME', 'YOUR KEY', 'capture')
  ```
4. Nezapomeňte vložit na odpovídající hodnoty pro název účtu úložiště a klíč ve volání `startProcessing`.

## <a name="run-the-scripts"></a>Spuštěním skriptů
1. Otevřete příkazový řádek, který má Python v jeho cestu a poté spusťte tyto příkazy pro instalaci požadovaných balíčků Python:
   
  ```
  pip install azure-storage
  pip install azure-servicebus
  pip install avro
  ```
   
  Pokud máte starší verzi azure-storage nebo azure, budete možná muset použít **– upgrade** možnost
   
  Může se také muset spustit následující (není nutné u většiny systémů):
   
  ```
  pip install cryptography
  ```
2. Přejděte do adresáře kdekoli uloženy sender.py a capturereader.py a spusťte tento příkaz:
   
  ```
  start python sender.py
  ```
   
  Tento příkaz spustí nový proces Python ke spuštění odesílatele.
3. Nyní Počkejte několik minut pro zaznamenání ke spuštění. Potom zadejte do původní okno příkazového řádku následující příkaz:
   
   ```
   python capturereader.py
   ```

   Tento snímek procesoru používá místní adresář ke stažení všechny objekty BLOB z účtu úložiště nebo kontejneru. Zpracuje všechny, které nejsou prázdné a zapíše výsledky jako soubory .csv do místního adresáře.

## <a name="next-steps"></a>Další postup

Další informace o službě Event Hubs najdete na následujících odkazech:

* [Přehled služby Event Hubs zachycení][Overview of Event Hubs Capture]
* [Ukázkové aplikace, které používají službu Event Hubs](https://github.com/Azure/azure-event-hubs/tree/master/samples)
* [Přehled služby Event Hubs][Event Hubs overview]

[Azure portal]: https://portal.azure.com/
[Overview of Event Hubs Capture]: event-hubs-capture-overview.md
[1]: ./media/event-hubs-archive-python/event-hubs-python1.png
[About Azure storage accounts]:../storage/common/storage-create-storage-account.md
[Visual Studio Code]: https://code.visualstudio.com/
[Event Hubs overview]: event-hubs-what-is-event-hubs.md
