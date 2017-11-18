---
title: "Rychlý úvod: Cassandra rozhraní API pomocí Python - Azure Cosmos DB | Microsoft Docs"
description: "Tento rychlý start ukazuje, jak používat Azure Cosmos DB Apache Cassandra rozhraní API pro vytvoření profilu aplikace s Pythonem"
services: cosmos-db
documentationcenter: 
author: mimig1
manager: jhubbard
editor: 
ms.assetid: 4ebc883e-c512-4e34-bd10-19f048661159
ms.service: cosmos-db
ms.custom: quick start connect, mvc
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: python
ms.topic: quickstart
ms.date: 11/15/2017
ms.author: govindk
ms.openlocfilehash: 4a2347fe9578b35c95d240c5c4dd2bf062077ece
ms.sourcegitcommit: a036a565bca3e47187eefcaf3cc54e3b5af5b369
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/17/2017
---
# <a name="quickstart-build-a-cassandra-app-with-python-and-azure-cosmos-db"></a>Rychlý úvod: Sestavení aplikace Cassandra s Python a Azure Cosmos DB

Tento rychlý start ukazuje způsob použití Python a Azure Cosmos DB [Cassandra API](cassandra-introduction.md) k vytvoření profilu aplikace klonováním příklad z Githubu. Tento rychlý start také vás provede procesem vytvoření účtu Azure Cosmos DB pomocí portálu Azure založených na webu.

Azure Cosmos DB je globálně distribuované databáze více modelu služby společnosti Microsoft. Můžete rychle vytvořit a dotazovat dokumentu, tabulka, klíč hodnota a graf databází, které těžit z globální distribuci a možnosti vodorovné škálování základem Azure Cosmos DB.   

## <a name="prerequisites"></a>Požadavky

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]Alternativně můžete [zkuste Azure Cosmos DB zdarma](https://azure.microsoft.com/try/cosmosdb/) bez předplatného Azure, bez poplatků a závazky.

Přístup k rozhraní API služby Azure Cosmos DB Cassandra programu preview. Pokud jste ještě nenainstalovali pro přístup k ještě [nyní](cassandra-introduction.md#sign-up-now).

Navíc platí:
* [Python](https://www.python.org/downloads/) v2.7.14 verze
* [Git](http://git-scm.com/)
* [Ovladač Python pro Apache Cassandra](https://github.com/datastax/python-driver)

## <a name="create-a-database-account"></a>Vytvoření účtu databáze

Než bude možné vytvořit databázi dokumentů, musíte vytvořit účet Cassandra s Azure Cosmos DB.

[!INCLUDE [cosmos-db-create-dbaccount-cassandra](../../includes/cosmos-db-create-dbaccount-cassandra.md)]

## <a name="clone-the-sample-application"></a>Klonování ukázkové aplikace

Nyní Pojďme klonování Cassandra API aplikace z githubu, nastavení připojovacího řetězce a potom ho spusťte. Uvidíte, jak snadno se pracuje s daty prostřednictvím kódu programu. 

1. Otevřete okno terminálu git, jako je například git bash a použít `cd` příkaz Přejít do složky pro instalaci ukázkové aplikace. 

    ```bash
    cd "C:\git-samples"
    ```

2. Ukázkové úložiště naklonujete spuštěním následujícího příkazu. Tento příkaz vytvoří kopii ukázková aplikace ve vašem počítači. 

    ```bash
    git clone https://github.com/Azure-Samples/azure-cosmos-db-cassandra-python-getting-started.git
    ```

## <a name="review-the-code"></a>Kontrola kódu

Tento krok je volitelný. Pokud vás zajímá učení vytváření databázových prostředků v kódu, můžete zkontrolovat následující fragmenty kódu. Fragmenty kódu jsou převzaty z `pyquickstart.py` souboru. Jinak, můžete přeskočit na [aktualizovat připojovací řetězec](#update-your-connection-string). 

* Uživatelské jméno a heslo se nastavuje pomocí stránce připojovací řetězec na portálu Azure. Nahraďte path\to\cert s cestou k vaší X509 certifikátu.

   ```python
    ssl_opts = {
            'ca_certs': 'path\to\cert',
            'ssl_version': ssl.PROTOCOL_TLSv1_2
            }
    auth_provider = PlainTextAuthProvider( username=cfg.config['username'], password=cfg.config['password'])
    cluster = Cluster([cfg.config['contactPoint']], port = cfg.config['port'], auth_provider=auth_provider, ssl_options=ssl_opts)
    session = cluster.connect()
   
   ```

* `cluster` Inicializován s contactPoint informace. ContactPoint se načtou z portálu Azure.

    ```python
   cluster = Cluster([cfg.config['contactPoint']], port = cfg.config['port'], auth_provider=auth_provider)
    ```

* `cluster` Připojí k rozhraní API služby Azure DB Cosmos Cassandra.

    ```python
    session = cluster.connect()
    ```

* Vytvoří se nový keyspace.

    ```python
   session.execute('CREATE KEYSPACE IF NOT EXISTS uprofile WITH replication = {\'class\': \'NetworkTopologyStrategy\', \'datacenter1\' : \'1\' }')
    ```

* Je-li vytvořit novou tabulku.

   ```
   session.execute('CREATE TABLE IF NOT EXISTS uprofile.user (user_id int PRIMARY KEY, user_name text, user_bcity text)');
   ```

* Klíč/hodnota entity jsou vloženy.

    ```Python
    insert_data = session.prepare("INSERT INTO  uprofile.user  (user_id, user_name , user_bcity) VALUES (?,?,?)")
    batch = BatchStatement()
    batch.add(insert_data, (1, 'LyubovK', 'Dubai'))
    batch.add(insert_data, (2, 'JiriK', 'Toronto'))
    batch.add(insert_data, (3, 'IvanH', 'Mumbai'))
    batch.add(insert_data, (4, 'YuliaT', 'Seattle'))
    ....
    session.execute(batch)
    ```

* Dotaz pro získání získat všechny klíčové hodnoty.

    ```Python
    rows = session.execute('SELECT * FROM uprofile.user')
    ```  
    
* Dotaz pro získání klíč hodnota.

    ```Python
    
    rows = session.execute('SELECT * FROM uprofile.user where user_id=1')
    ```  

## <a name="update-your-connection-string"></a>Aktualizace připojovacího řetězce

Teď se vraťte zpátky na portál Azure Portal, kde najdete informace o připojovacím řetězci, a zkopírujte je do aplikace. To umožňuje aplikaci ke komunikaci s vaší hostované databází.

1. V [portál Azure](http://portal.azure.com/), klikněte na tlačítko **připojovací řetězec**. 

    Použití ![Tlačítko Kopírovat](./media/create-cassandra-python/copy.png) tlačítko na pravé straně obrazovky zkopírujte nejvyšší hodnotu, obraťte se na bodu.

    ![Zobrazení a zkopírování přístupový uživatelské jméno, heslo a obraťte se na bod v okně Azure řetězec portálu, připojení](./media/create-cassandra-python/keys.png)

2. Otevřete soubor `config.py`. 

3. Vložit hodnotu bodu obraťte se na portálu přes `<FILLME>` na řádku 10.

    Řádek 10 by teď měl vypadat podobně jako 

    `'contactPoint': 'cosmos-db-quickstarts.documents.azure.com:10350'`

4. Zkopírujte hodnotu uživatelského jména z portálu a vložte ji přes `<FILLME>` na řádku 6.

    Řádek 6 by teď měl vypadat podobně jako 

    `'username': 'cosmos-db-quickstart',`
    
5. Zkopírujte hodnotu HESLA z portálu a vložte ho přes `<FILLME>` na řádku 8.

    Řádek 8 by teď měl vypadat podobně jako

    `'password' = '2Ggkr662ifxz2Mg==`';`

6. Uložte soubor config.py.
    
## <a name="use-the-x509-certificate"></a>Použít X509 certifikát

1. Pokud potřebujete přidat Baltimore CyberTrust Root, má 02:00:00:b9 sériové číslo a SHA1 otisk prstu d4🇩🇪20:d0:5e:66:fc:53:fe:1a:50:88:2 c: 78:db:28:52:ca:e4:74. Lze ji stáhnout z https://cacert.omniroot.com/bc2025.crt, uložit do místního souboru s příponou CER

2. Otevřete pyquickstart.py a změňte 'path\to\cert' tak, aby odkazoval na nový certifikát.

3. Uložte pyquickstart.py.

## <a name="run-the-app"></a>Spuštění aplikace

1. Použijte příkaz cd v terminálu git změnit do azure-cosmos-db-cassandra-python-getting-started složky. 

2. Spusťte následující příkazy pro instalaci požadované moduly:

    ```python
    python -m pip install cassandra-driver
    python -m pip install prettytable
    python -m pip install requests
    python -m pip install pyopenssl
    ```

2. Spusťte následující příkaz, který spouští aplikaci uzlu:

    ```
    python pyquickstart.py
    ```

3. Zkontrolujte výsledky podle očekávání z příkazového řádku.

    Stisknutím kombinace kláves CTRL + C zastavení exection programu a zavřete okno konzoly. 

    ![Zobrazit a ověřit výstup](./media/create-cassandra-python/output.png)
    
    Nyní lze otevřít Průzkumníku dat na portálu Azure najdete v části dotazu, úpravu a pracovat s Tato nová data. 

    ![Zobrazení dat v Průzkumníku dat](./media/create-cassandra-python/data-explorer.png)

## <a name="review-slas-in-the-azure-portal"></a>Ověření smluv SLA na webu Azure Portal

[!INCLUDE [cosmosdb-tutorial-review-slas](../../includes/cosmos-db-tutorial-review-slas.md)]

## <a name="clean-up-resources"></a>Vyčištění prostředků

[!INCLUDE [cosmosdb-delete-resource-group](../../includes/cosmos-db-delete-resource-group.md)]

## <a name="next-steps"></a>Další kroky

V tomto rychlém startu jsme se seznámili s postupem vytvoření účtu databáze Azure Cosmos, vytvoření kolekce pomocí Průzkumníka dat a spuštění aplikace. Teď můžete do účtu databáze Cosmos importovat další data. 

> [!div class="nextstepaction"]
> [Importovat Cassandra data do Azure Cosmos DB](cassandra-import-data.md)

