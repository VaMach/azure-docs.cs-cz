---
title: Kurz k NoSQL C++ pro DocumentDB | Microsoft Docs
description: "Kurz k NoSQL C++, v rámci kterého se vytváří databáze a konzolová aplikace v jazyce C++ pomocí DocumentDB se schválenou sadou SDK pro jazyk C++. DocumentDB je databázová služba NoSQL na globální úrovni."
services: documentdb
documentationcenter: cpp
author: asthana86
manager: jhubbard
editor: 
ms.assetid: b8756b60-8d41-4231-ba4f-6cfcfe3b4bab
ms.service: documentdb
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: cpp
ms.topic: hero-article
ms.date: 12/25/2016
ms.author: aasthan
translationtype: Human Translation
ms.sourcegitcommit: 72b2d9142479f9ba0380c5bd2dd82734e370dee7
ms.openlocfilehash: 78c3da6fd83a6fca0351a90846d10acd82924be3
ms.lasthandoff: 04/18/2017


---
# <a name="nosql-c-tutorial-documentdb-c-console-application"></a>Kurz k NoSQL C++: Vytvoření konzolové aplikace DocumentDB v jazyce C++
> [!div class="op_single_selector"]
> * [.NET](documentdb-get-started.md)
> * [.NET Core](documentdb-dotnetcore-get-started.md)
> * [Node.js pro MongoDB](documentdb-mongodb-samples.md)
> * [Node.js](documentdb-nodejs-get-started.md)
> * [Java](documentdb-java-get-started.md)
> * [C++](documentdb-cpp-get-started.md)
>  
> 
 

Vítejte v kurzu k NoSQL pro Azure DocumentDB se schválenou sadou SDK pro jazyk C++. Až projdete tímto kurzem, budete mít konzolovou aplikaci, která vytváří prostředky DocumentDB, včetně databáze v jazyce C++, a dotazuje se na ně.

Budeme se zabývat těmito tématy:

* Vytvoření a připojení k účtu DocumentDB
* Nastavení aplikace
* Vytvoření databáze DocumentDB v jazyce C++
* Vytvoření kolekce
* Vytvoření dokumentů JSON
* Dotazování na kolekci
* Nahrazení dokumentu
* Odstranění dokumentu
* Odstranění databáze DocumentDB v jazyce C++

Nemáte čas? Nevadí! Úplné řešení je k dispozici na [GitHubu](https://github.com/stalker314314/DocumentDBCpp). Rychlé pokyny najdete v části [Získání úplného řešení](#GetSolution).

Až tento kurz k C++ dokončíte, sdělte nám prosím svůj názor pomocí hlasovacích tlačítek v dolní části této stránky. 

Pokud chcete, abychom vás kontaktovali přímo, můžete nám nechat e-mailovou adresu v komentářích nebo [se na nás obraťte zde](https://www.research.net/r/8BKRJ3Z). 

Můžeme začít!

## <a name="prerequisites-for-the-c-tutorial"></a>Předpoklady pro kurz k C++
Ujistěte se prosím, že máte následující:

* Aktivní účet Azure. Pokud žádný nemáte, můžete si zaregistrovat [bezplatnou zkušební verzi Azure](https://azure.microsoft.com/pricing/free-trial/).
* [Visual Studio](https://www.visualstudio.com/downloads/) s nainstalovanými komponentami jazyka C++

## <a name="step-1-create-a-documentdb-account"></a>Krok 1: Vytvoření účtu DocumentDB
Vytvořme účet DocumentDB. Pokud už máte účet, který chcete použít, můžete přeskočit na [Nastavení aplikace C++](#SetupNode).

[!INCLUDE [documentdb-create-dbaccount](../../includes/documentdb-create-dbaccount.md)]

## <a id="SetupC++"></a>Krok 2: Nastavení aplikace C++
1. Otevřete Visual Studio a v nabídce **Soubor** klikněte na **Nový** a potom na **Projekt**. 
2. V okně **Nový projekt** v podokně **Nainstalováno** rozbalte nabídku **Visual C++**, klikněte na **Win32** a potom klikněte na **Konzolová aplikace Win32**. Jako název dokumentu zadejte hellodocumentdb a klikněte na **OK**. 
   
    ![Snímek obrazovky s průvodcem novým projektem](media/documentdb-cpp-get-started/hellodocumentdb.png)
3. Jakmile se spustí průvodce aplikací Win32, klikněte na **Dokončit**.
4. Po vytvoření projektu otevřete správce balíčku NuGet kliknutím pravým tlačítkem myši na projekt **hellodocumentdb** v **průzkumníku řešení** a potom na **Spravovat balíčky NuGet**. 
   
    ![Snímek obrazovky s možností Spravovat balíčky NuGet v nabídce projektu](media/documentdb-cpp-get-started/nuget.png)
5. Na kartě **NuGet: hellodocumentdb** klikněte na **Procházet** a vyhledejte *documentdbcpp*. Ve výsledcích vyberte položku DocumentDbCPP, jak je znázorněno na následujícím snímku obrazovky. Tento balíček nainstaluje odkazy na sadu C++ REST SDK, která závislostí pro DocumentDbCPP.  
   
    ![Snímek obrazovky se zvýrazněným balíčkem DocumentDbCpp](media/documentdb-cpp-get-started/documentdbcpp.png)
   
    Jakmile se balíčky přidají do vašeho projektu, můžeme začít psát kód.   

## <a id="Config"></a>Krok 3: Zkopírování podrobností o připojení z webu Azure Portal pro databázi DocumentDB
Otevřete web [Azure Portal](https://portal.azure.com) a přejděte do databázového účtu NoSQL (DocumentDB), který jste vytvořili. Pro další krok budete z webu Azure Portal potřebovat identifikátor URI a primární klíč, pomocí kterých navážete připojení z fragmentu kódu v jazyce C++. 

![Identifikátor URI a klíče pro DocumentDB na webu Azure Portal](media/documentdb-cpp-get-started/nosql-tutorial-keys.png)

## <a id="Connect"></a>Krok 4: Připojení k účtu DocumentDB
1. Do zdrojového kódu zadejte níže uvedené hlavičky a obory názvů za `#include "stdafx.h"`.
   
        #include <cpprest/json.h>
        #include <documentdbcpp\DocumentClient.h>
        #include <documentdbcpp\exceptions.h>
        #include <documentdbcpp\TriggerOperation.h>
        #include <documentdbcpp\TriggerType.h>
        using namespace documentdb;
        using namespace std;
        using namespace web::json;
2. Do hlavní funkce přidejte níže uvedený kód a změňte konfiguraci účtu a primární klíč tak, aby se shodovaly s nastavením databáze DocumentDB popsaným v kroku 3. 
   
        DocumentDBConfiguration conf (L"<account_configuration_uri>", L"<primary_key>");
        DocumentClient client (conf);
   
    Nyní, když máte kód pro inicializaci klienta documentdb, se budeme věnovat práci s prostředky DocumentDB.

## <a id="CreateDBColl"></a>Krok 5: Vytvoření databáze a kolekce v jazyce C++
Pokud s DocumentDB začínáte, přečtěte si ještě před provedením tohoto kroku, jak databáze, kolekce a dokumenty vzájemně komunikují. [Databáze](documentdb-resources.md#databases) je logický kontejner úložiště dokumentů rozděleného mezi kolekcemi. [Kolekce](documentdb-resources.md#collections) je kontejner dokumentů JSON a přidružené logiky javascriptové aplikace. Další informace o konceptech a hierarchickém modelu prostředků DocumentDB najdete v tématu [Koncepty a hierarchický model prostředků DocumentDB](documentdb-resources.md).

Databázi a odpovídající kolekci vytvoříte vložením níže uvedeného kódu na konec hlavní funkce. Prostřednictvím konfigurace klienta deklarované v předchozím kroku vznikne databáze „FamilyRegistry“ a kolekce „FamilyCollection“.

    try {
      shared_ptr<Database> db = client.CreateDatabase(L"FamilyRegistry");
      shared_ptr<Collection> coll = db->CreateCollection(L"FamilyCollection");
    } catch (DocumentDBRuntimeException ex) {
      wcout << ex.message();
    }


## <a id="CreateDoc"></a>Krok 6: Vytvoření dokumentu
[Dokumenty](documentdb-resources.md#documents) představují uživatelem definovaný (libovolný) obsah JSON. Nyní můžete vložit dokument do DocumentDB. Dokument můžete vytvořit zkopírováním níže uvedeného kódu na konec hlavní funkce. 

    try {
      value document_family;
      document_family[L"id"] = value::string(L"AndersenFamily");
      document_family[L"FirstName"] = value::string(L"Thomas");
      document_family[L"LastName"] = value::string(L"Andersen");
      shared_ptr<Document> doc = coll->CreateDocumentAsync(document_family).get();

      document_family[L"id"] = value::string(L"WakefieldFamily");
      document_family[L"FirstName"] = value::string(L"Lucy");
      document_family[L"LastName"] = value::string(L"Wakefield");
      doc = coll->CreateDocumentAsync(document_family).get();
    } catch (ResourceAlreadyExistsException ex) {
      wcout << ex.message();
    }

Souhrnně řečeno, tento kód vytvoří databázi, kolekci a dokumenty, na které se můžete dotazovat v průzkumníku dokumentů na webu Azure Portal. 

![Kurz k C++ – diagram ilustrující hierarchický vztah mezi účtem, databází, kolekcí a dokumenty](media/documentdb-cpp-get-started/documentdbdocs.png)

## <a id="QueryDB"></a>Krok 7: Dotazování prostředků DocumentDB
DocumentDB podporuje [bohaté dotazy](documentdb-sql-query.md) na dokumenty JSON uložené v každé z kolekcí. Následující ukázkový kód ukazuje dotaz vytvořený pomocí syntaxe DocumentDB SQL, kterou můžete spouštět oproti dokumentům vytvořeným v předchozím kroku.

Tato funkce využívá jako argumenty společně s klientem dokumentu i unikátní identifikátor nebo ID prostředku databáze a kolekce. Vložte tento kód před hlavní funkci.

    void executesimplequery(const DocumentClient &client,
                            const wstring dbresourceid,
                            const wstring collresourceid) {
      try {
        client.GetDatabase(dbresourceid).get();
        shared_ptr<Database> db = client.GetDatabase(dbresourceid);
        shared_ptr<Collection> coll = db->GetCollection(collresourceid);
        wstring coll_name = coll->id();
        shared_ptr<DocumentIterator> iter =
            coll->QueryDocumentsAsync(wstring(L"SELECT * FROM " + coll_name)).get();
        wcout << "\n\nQuerying collection:";
        while (iter->HasMore()) {
          shared_ptr<Document> doc = iter->Next();
          wstring doc_name = doc->id();
          wcout << "\n\t" << doc_name << "\n";
          wcout << "\t"
                << "[{\"FirstName\":"
                << doc->payload().at(U("FirstName")).as_string()
                << ",\"LastName\":" << doc->payload().at(U("LastName")).as_string()
                << "}]";
        }
      } catch (DocumentDBRuntimeException ex) {
        wcout << ex.message();
      }
    }

## <a id="Replace"></a>Krok 8: Nahrazení dokumentu
DocumentDB podporuje nahrazování dokumentů JSON, jak můžete vidět na níže uvedeném kódu. Tento kód vložte za funkci executesimplequery.

    void replacedocument(const DocumentClient &client, const wstring dbresourceid,
                         const wstring collresourceid,
                         const wstring docresourceid) {
      try {
        client.GetDatabase(dbresourceid).get();
        shared_ptr<Database> db = client.GetDatabase(dbresourceid);
        shared_ptr<Collection> coll = db->GetCollection(collresourceid);
        value newdoc;
        newdoc[L"id"] = value::string(L"WakefieldFamily");
        newdoc[L"FirstName"] = value::string(L"Lucy");
        newdoc[L"LastName"] = value::string(L"Smith Wakefield");
        coll->ReplaceDocument(docresourceid, newdoc);
      } catch (DocumentDBRuntimeException ex) {
        throw;
      }
    }

## <a id="Delete"></a>Krok 9: Odstranění dokumentu
DocumentDB podporuje odstraňování dokumentů JSON. Provedete to tak, že zkopírujete a vložíte níže uvedený kód za funkci replacedocument. 

    void deletedocument(const DocumentClient &client, const wstring dbresourceid,
                        const wstring collresourceid, const wstring docresourceid) {
      try {
        client.GetDatabase(dbresourceid).get();
        shared_ptr<Database> db = client.GetDatabase(dbresourceid);
        shared_ptr<Collection> coll = db->GetCollection(collresourceid);
        coll->DeleteDocumentAsync(docresourceid).get();
      } catch (DocumentDBRuntimeException ex) {
        wcout << ex.message();
      }
    }

## <a id="DeleteDB"></a>Krok 10: Odstranění databáze
Odstraněním vytvořené databáze dojde k odebrání databáze a všech jejích podřízených prostředků (kolekcí, dokumentů atd.).

Zkopírujte a za funkci deletedocument vložte následující fragment kódu (vyčištění funkce), který odebere databázi a všechny podřízené prostředky.

    void deletedb(const DocumentClient &client, const wstring dbresourceid) {
      try {
        client.DeleteDatabase(dbresourceid);
      } catch (DocumentDBRuntimeException ex) {
        wcout << ex.message();
      }
    }

## <a id="Run"></a>Krok 11: Spuštění celé konzolové aplikace jazyka C#
Vložili jsme kód, pomocí kterého můžete vytvářet, upravovat a odstraňovat různé prostředky DocumentDB nebo se na ně dotazovat.  Pojďme ho uvést do provozu přidáním volání těchto funkcí z naší hlavní funkce v projektu hellodocumentdb.cpp a některých diagnostických zpráv.

Provedete to tak, že hlavní funkci vaší aplikace nahradíte níže uvedeným kódem. account_configuration_uri a primary_key, které jste do kódu zkopírovali v kroku 3, se přepíší, takže tento řádek uložte nebo hodnoty znovu zkopírujte z webu. 

    int main() {
        try {
            // Start by defining your account's configuration
            DocumentDBConfiguration conf (L"<account_configuration_uri>", L"<primary_key>");
            // Create your client
            DocumentClient client(conf);
            // Create a new database
            try {
                shared_ptr<Database> db = client.CreateDatabase(L"FamilyDB");
                wcout << "\nCreating database:\n" << db->id();
                // Create a collection inside database
                shared_ptr<Collection> coll = db->CreateCollection(L"FamilyColl");
                wcout << "\n\nCreating collection:\n" << coll->id();
                value document_family;
                document_family[L"id"] = value::string(L"AndersenFamily");
                document_family[L"FirstName"] = value::string(L"Thomas");
                document_family[L"LastName"] = value::string(L"Andersen");
                shared_ptr<Document> doc =
                    coll->CreateDocumentAsync(document_family).get();
                wcout << "\n\nCreating document:\n" << doc->id();
                document_family[L"id"] = value::string(L"WakefieldFamily");
                document_family[L"FirstName"] = value::string(L"Lucy");
                document_family[L"LastName"] = value::string(L"Wakefield");
                doc = coll->CreateDocumentAsync(document_family).get();
                wcout << "\n\nCreating document:\n" << doc->id();
                executesimplequery(client, db->resource_id(), coll->resource_id());
                replacedocument(client, db->resource_id(), coll->resource_id(),
                    doc->resource_id());
                wcout << "\n\nReplaced document:\n" << doc->id();
                executesimplequery(client, db->resource_id(), coll->resource_id());
                deletedocument(client, db->resource_id(), coll->resource_id(),
                    doc->resource_id());
                wcout << "\n\nDeleted document:\n" << doc->id();
                deletedb(client, db->resource_id());
                wcout << "\n\nDeleted db:\n" << db->id();
                cin.get();
            }
            catch (ResourceAlreadyExistsException ex) {
                wcout << ex.message();
            }
        }
        catch (DocumentDBRuntimeException ex) {
            wcout << ex.message();
        }
        cin.get();
    }

Teď už by mělo být možné vytvořit a spustit kód v sadě Visual Studio stisknutím klávesy F5, případně v okně terminálu vyhledáním aplikace a otevřením spustitelného souboru. 

Měl by se zobrazit výstup počáteční aplikace. Tento výstup by se měl shodovat se snímkem obrazovky níže.

![Výstup aplikace DocumentDB v jazyce C++](media/documentdb-cpp-get-started/docdbconsole.png)

Blahopřejeme! Dokončili jste kurz k C++ a máte svou první konzolovou aplikaci DocumentDB!

## <a id="GetSolution"></a>Získání úplného řešení kurzu k C++
Abyste mohli sestavit řešení GetStarted, které obsahuje všechny ukázky tohoto článku, budete potřebovat následující:

* [Účet DocumentDB][documentdb-create-account]
* Řešení [GetStarted](https://github.com/stalker314314/DocumentDBCpp) dostupné na GitHubu

## <a name="next-steps"></a>Další kroky
* Naučte se [monitorovat účet DocumentDB](documentdb-monitor-accounts.md).
* Spouštějte dotazy proti ukázkovým datovým sadám v [Query Playground](https://www.documentdb.com/sql/demo).
* Přečtěte si více o tomto programovacím modelu v části Vyvíjejte na [stránce dokumentace DocumentDB](https://azure.microsoft.com/documentation/services/documentdb/).

[documentdb-create-account]: documentdb-create-account.md



