---
title: "Příklad další data připojení ke zdroji možné přípravy dat Azure Machine Learning | Microsoft Docs"
description: "Tento dokument obsahuje sadu příklady připojení zdroje dat, které je možné při přípravě dat Azure Machine Learning"
services: machine-learning
author: euangMS
ms.author: euang
manager: lanceo
ms.reviewer: garyericson, jasonwhowell, mldocs
ms.service: machine-learning
ms.workload: data-services
ms.custom: 
ms.devlang: 
ms.topic: article
ms.date: 09/11/2017
ms.openlocfilehash: 3ffe0e385f9dd71d8341305f42ceb10e0ea49af4
ms.sourcegitcommit: 68aec76e471d677fd9a6333dc60ed098d1072cfc
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 12/18/2017
---
# <a name="sample-of-custom-source-connections-python"></a>Ukázka připojení vlastní zdroje (Python) 
Před čtením této příloha, přečtěte si [Python rozšiřitelnost přehled](data-prep-python-extensibility-overview.md).

## <a name="load-data-from-dataworld"></a>Načtení dat z data.world

### <a name="prerequisites"></a>Požadavky

#### <a name="register-yourself-at-dataworld"></a>Sami zaregistrovat na data.world
Budete potřebovat token rozhraní API z webu data.world.

#### <a name="install-dataworld-library"></a>Nainstaluje data.world knihovny

Otevřete rozhraní příkazového řádku Azure Machine Learning Workbench výběrem **soubor** > **otevřete rozhraní příkazového řádku**.

```console
pip install git+git://github.com/datadotworld/data.world-py.git
```

Potom spustíte `dw configure` na příkazovém řádku, které vás vyzve k zadání vašeho tokenu. Když zadáte token, .dw nebo adresář bude vytvořen v domovském adresáři a váš token je uložený existuje.

```
API token (obtained at: https://data.world/settings/advanced): <enter API token here>
```
Nyní byste měli mít importovat data.world knihovny.

#### <a name="load-data-into-data-preparation"></a>Načtení dat do data přípravy

Vytvořte nový založených na skriptech datový tok. Potom pomocí následujícího skriptu k načítání dat z data.world.

```python
#paths = df['Path'].tolist()

import datadotworld as dw

#Load the dataset.
lds = dw.load_dataset('data-society/the-simpsons-by-the-data')

#Load specific data frame from the dataset.
df = lds.dataframes['simpsons_episodes']

```

## <a name="load-azure-cosmos-db-data-into-data-preparation"></a>Načtení dat Azure Cosmos DB do Příprava dat

Vytvořit nový tok dat založených na skriptech a pak použít následující skript k načítání dat z Azure Cosmos DB. (Knihovny musí být napřed nainstalované. Další informace najdete v předchozí odkaz na dokument, který jsme propojit.)

```python
import pydocumentdb
import pydocumentdb.document_client as document_client

import pandas as pd

config = { 
    'ENDPOINT': '<Endpoint>',
    'MASTERKEY': '<Key>',
    'DOCUMENTDB_DATABASE': '<DBName>',
    'DOCUMENTDB_COLLECTION': '<collectionname>'
};

# Initialize the Python DocumentDB client.
client = document_client.DocumentClient(config['ENDPOINT'], {'masterKey': config['MASTERKEY']})

# Read databases and take first since id should not be duplicated.
db = next((data for data in client.ReadDatabases() if data['id'] == config['DOCUMENTDB_DATABASE']))

# Read collections and take first since id should not be duplicated.
coll = next((coll for coll in client.ReadCollections(db['_self']) if coll['id'] == config['DOCUMENTDB_COLLECTION']))

docs = client.ReadDocuments(coll['_self'])

df = pd.DataFrame(list(docs))
```
