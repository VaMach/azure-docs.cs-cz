---
title: 'Kurz k NoSQL: DocumentDB .NET SDK | Dokumentace Microsoftu'
description: "Kurz k NoSQL, v rámci kterého se vytváří online databáze a konzolová aplikace v jazyce C# pomocí sady DocumentDB .NET SDK. DocumentDB je databáze NoSQL pro JSON."
keywords: "kurz nosql, online databáze konzolová aplikace jazyka c#"
services: documentdb
documentationcenter: .net
author: AndrewHoh
manager: jhubbard
editor: monicar
ms.assetid: bf08e031-718a-4a2a-89d6-91e12ff8797d
ms.service: documentdb
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: hero-article
ms.date: 12/16/2016
ms.author: anhoh
translationtype: Human Translation
ms.sourcegitcommit: 72b2d9142479f9ba0380c5bd2dd82734e370dee7
ms.openlocfilehash: 2ca704e3ef14589b5a0c44c9b6857445e3e62dd7
ms.lasthandoff: 03/08/2017


---
# <a name="nosql-tutorial-build-a-documentdb-c-console-application"></a>Kurz k NoSQL: Vytvoření konzolové aplikace DocumentDB v jazyce C#
> [!div class="op_single_selector"]
> * [.NET](documentdb-get-started.md)
> * [.NET Core](documentdb-dotnetcore-get-started.md)
> * [Node.js pro MongoDB](documentdb-mongodb-samples.md)
> * [Node.js](documentdb-nodejs-get-started.md)
> * [Java](documentdb-java-get-started.md)
> * [C++](documentdb-cpp-get-started.md)
>  
> 

Vítejte v kurzu k NoSQL pro sadu Azure DocumentDB .NET SDK! Až projdete tímto kurzem, budete mít konzolovou aplikaci, která vytváří prostředky DocumentDB a dotazuje se na ně.

Budeme se zabývat těmito tématy:

* Vytvoření a připojení k účtu DocumentDB
* Konfigurace řešení v nástroji Visual Studio
* Vytvoření online databáze
* Vytvoření kolekce
* Vytvoření dokumentů JSON
* Dotazování na kolekci
* Nahrazení dokumentu
* Odstranění dokumentu
* Odstranění databáze

Nemáte čas? Nevadí! Úplné řešení je k dispozici na [GitHubu](https://github.com/Azure-Samples/documentdb-dotnet-getting-started). Pro rychlé pokyny přeskočte na [oddíl Získání úplného řešení kurzu NoSQL](#GetSolution).

Potom prosím použijte hlasovací tlačítka v horní nebo dolní části stránky, abychom získali zpětnou vazbu. Pokud chcete, abychom vás kontaktovali přímo, můžete nám nechat e-mailovou adresu v komentářích.

Můžeme začít!

## <a name="prerequisites"></a>Předpoklady
Ujistěte se prosím, že máte následující:

* Aktivní účet Azure. Pokud žádný nemáte, můžete si zaregistrovat [bezplatný účet](https://azure.microsoft.com/free/). 
    * Alternativně můžete pro tento kurz použít [emulátor Azure DocumentDB](documentdb-nosql-local-emulator.md).
* [Visual Studio 2013 / Visual Studio 2015](http://www.visualstudio.com/)
* .NET Framework 4.6

## <a name="step-1-create-a-documentdb-account"></a>Krok 1: Vytvoření účtu DocumentDB
Vytvořme účet DocumentDB. Pokud již máte účet, který chcete použít, můžete přeskočit na [Nastavení řešení v nástroji Visual Studio](#SetupVS). Pokud používáte emulátor DocumentDB, postupujte prosím podle kroků v tématu [Emulátor Azure DocumentDB](documentdb-nosql-local-emulator.md), abyste nastavili emulátor, a přeskočte k části [Nastavení řešení v sadě Visual Studio](#SetupVS).

[!INCLUDE [documentdb-create-dbaccount](../../includes/documentdb-create-dbaccount.md)]

## <a id="SetupVS"></a>Krok 2: Nastavení řešení v sadě Visual Studio
1. Otevřete v počítači **Visual Studio 2015**.
2. V nabídce **Soubor** vyberte **Nový** a zvolte **Projekt**.
3. V dialogovém okně **Nový projekt** vyberte **Šablony** / **Visual C#** / **Konzolová aplikace**, pojmenujte svůj projekt a klikněte na **OK**.
   ![Snímek obrazovky okna Nový projekt](./media/documentdb-get-started/nosql-tutorial-new-project-2.png)
4. V **Průzkumníku řešení** klikněte pravým tlačítkem na novou konzolovou aplikaci v rámci řešení sady Visual Studio a pak klikněte na **Spravovat balíčky NuGet**.
    
    ![Snímek obrazovky místní nabídky projektu](./media/documentdb-get-started/nosql-tutorial-manage-nuget-pacakges.png)
5. Na kartě **NuGet** klikněte na **Procházet** a do vyhledávacího pole zadejte **azure documentdb**.
6. Najděte ve výsledcích **Microsoft.Azure.DocumentDB** a klikněte na **Nainstalovat**.
   ID balíčku klientské knihovny DocumentDB je [Microsoft.Azure.DocumentDB](https://www.nuget.org/packages/Microsoft.Azure.DocumentDB).
   ![Snímek obrazovky nabídky Nuget pro vyhledání sady DocumentDB Client SDK](./media/documentdb-get-started/nosql-tutorial-manage-nuget-pacakges-2.png)

    Pokud se vám zobrazí zpráva týkající se kontroly změn řešení, klikněte na **OK**. Pokud se vám zobrazí zpráva týkající se přijetí licence, klikněte na **Souhlasím**.

Výborně! Teď když jsme dokončili nastavování, napišme nějaký kód. Úplný projekt s kódem pro tento kurz najdete na [GitHubu](https://github.com/Azure-Samples/documentdb-dotnet-getting-started/blob/master/src/Program.cs).

## <a id="Connect"></a>Krok 3: Připojení k účtu DocumentDB
Nejprve přidejte na začátek aplikace C# do souboru Program.cs tyto reference:

    using System;
    using System.Linq;
    using System.Threading.Tasks;

    // ADD THIS PART TO YOUR CODE
    using System.Net;
    using Microsoft.Azure.Documents;
    using Microsoft.Azure.Documents.Client;
    using Newtonsoft.Json;

> [!IMPORTANT]
> Přidání závislostí uvedených výše je nezbytné pro dokončení tohoto kurzu NoSQL.
> 
> 

Nyní přidejte tyto dvě konstanty a proměnnou *client* pod veřejnou třídu *Program*.

    public class Program
    {
        // ADD THIS PART TO YOUR CODE
        private const string EndpointUri = "<your endpoint URI>";
        private const string PrimaryKey = "<your key>";
        private DocumentClient client;

Dále přejděte zpět na [Azure Portal](https://portal.azure.com) a získejte identifikátor URI a primární klíč. Identifikátor URI a primární klíč pro DocumentDB jsou potřeba k tomu, aby aplikace věděla, kam se připojit, a aby databáze DocumentDB důvěřovala připojení aplikace.

Na webu Azure Portal přejděte na účet DocumentDB a klikněte na **Klíče**.

Zkopírujte identifikátor URI z portálu a vložte ho do `<your endpoint URI>` v souboru program.cs. Poté zkopírujte PRIMÁRNÍ KLÍČ z portálu a vložte ho do `<your key>`.

![Snímek obrazovky Portálu Azure, který se v kurzu NoSQL používá k vytvoření konzolové aplikace v C#. Zobrazuje účet DocumentDB se zvýrazněným aktivním centrem, zvýrazněným tlačítkem Klíče v okně účtu DocumentDB a zvýrazněnými hodnotami URI, PRIMÁRNÍ KLÍČ a SEKUNDÁRNÍ KLÍČ v okně Klíče.][keys]

Potom vytvořením nové instance **DocumentClient** spustíme aplikaci.

Pod metodu **Main** přidejte tento nový asynchronní úkol pojmenovaný **GetStartedDemo**, který vytvoří instanci našeho nového klienta **DocumentClient**.

    static void Main(string[] args)
    {
    }

    // ADD THIS PART TO YOUR CODE
    private async Task GetStartedDemo()
    {
        this.client = new DocumentClient(new Uri(EndpointUri), PrimaryKey);
    }

Přidejte následující kód, aby se asynchronní úkol spustil z metody **Main**. Metoda **Main** zachytí výjimky a vypíše je do konzoly.

    static void Main(string[] args)
    {
            // ADD THIS PART TO YOUR CODE
            try
            {
                    Program p = new Program();
                    p.GetStartedDemo().Wait();
            }
            catch (DocumentClientException de)
            {
                    Exception baseException = de.GetBaseException();
                    Console.WriteLine("{0} error occurred: {1}, Message: {2}", de.StatusCode, de.Message, baseException.Message);
            }
            catch (Exception e)
            {
                    Exception baseException = e.GetBaseException();
                    Console.WriteLine("Error: {0}, Message: {1}", e.Message, baseException.Message);
            }
            finally
            {
                    Console.WriteLine("End of demo, press any key to exit.");
                    Console.ReadKey();
            }

Stisknutím klávesy **F5** spusťte aplikaci. Výstup okna konzoly zobrazuje zprávu `End of demo, press any key to exit.`, která potvrzuje vytvoření připojení.  Potom můžete okno konzoly zavřít. 

Blahopřejeme! Úspěšně jste se připojili k účtu DocumentDB. Nyní se podívejme, jak se pracuje s prostředky DocumentDB.  

## <a name="step-4-create-a-database"></a>Krok 4: Vytvoření databáze
Než přidáte kód pro vytvoření databáze, přidejte pomocnou metodu pro výpis do konzoly.

Zkopírujte a vložte metodu **WriteToConsoleAndPromptToContinue** za metodu **GetStartedDemo**.

    // ADD THIS PART TO YOUR CODE
    private void WriteToConsoleAndPromptToContinue(string format, params object[] args)
    {
            Console.WriteLine(format, args);
            Console.WriteLine("Press any key to continue ...");
            Console.ReadKey();
    }

[Databázi](documentdb-resources.md#databases) DocumentDB je možné vytvořit pomocí metody [CreateDatabaseAsync](https://msdn.microsoft.com/library/microsoft.azure.documents.client.documentclient.createdatabaseasync.aspx) třídy **DocumentClient**. Databáze je logický kontejner úložiště dokumentů JSON rozděleného mezi kolekcemi.

Zkopírujte a vložte metodu **CreateDatabaseIfNotExists** za metodu **WriteToConsoleAndPromptToContinue**.

    // ADD THIS PART TO YOUR CODE
    private async Task CreateDatabaseIfNotExists(string databaseName)
    {
            // Check to verify a database with the id=FamilyDB does not exist
            try
            {
                    await this.client.ReadDatabaseAsync(UriFactory.CreateDatabaseUri(databaseName));
                    this.WriteToConsoleAndPromptToContinue("Found {0}", databaseName);
            }
            catch (DocumentClientException de)
            {
                    // If the database does not exist, create a new database
                    if (de.StatusCode == HttpStatusCode.NotFound)
                    {
                            await this.client.CreateDatabaseAsync(new Database { Id = databaseName });
                            this.WriteToConsoleAndPromptToContinue("Created {0}", databaseName);
                    }
                    else
                    {
                            throw;
                    }
            }
    }

Zkopírujte a vložte následující kód do metody **GetStartedDemo** za vytvoření klienta. Tím se vytvoří databáze s názvem *FamilyDB*.

    private async Task GetStartedDemo()
    {
        this.client = new DocumentClient(new Uri(EndpointUri), PrimaryKey);

        // ADD THIS PART TO YOUR CODE
        await this.CreateDatabaseIfNotExists("FamilyDB_oa");

Stisknutím klávesy **F5** spusťte aplikaci.

Blahopřejeme! Úspěšně jste vytvořili databázi DocumentDB.  

## <a id="CreateColl"></a>Krok 5: Vytvoření kolekce
> [!WARNING]
> **CreateDocumentCollectionAsync** vytvoří novou kolekci s vyhrazenou propustností, za kterou se hradí poplatky. Další podrobnosti najdete na [stránce s cenami](https://azure.microsoft.com/pricing/details/documentdb/).
> 
> 

[Kolekci](documentdb-resources.md#collections) je možné vytvořit pomocí metody [CreateDocumentCollectionAsync](https://msdn.microsoft.com/library/microsoft.azure.documents.client.documentclient.createdocumentcollectionasync.aspx) třídy **DocumentClient**. Kolekce je kontejner dokumentů JSON a přidružené logiky javascriptové aplikace.

Zkopírujte a vložte metodu **CreateDocumentCollectionIfNotExists** za metodu **CreateDatabaseIfNotExists**.

    // ADD THIS PART TO YOUR CODE
    private async Task CreateDocumentCollectionIfNotExists(string databaseName, string collectionName)
    {
        try
        {
            await this.client.ReadDocumentCollectionAsync(UriFactory.CreateDocumentCollectionUri(databaseName, collectionName));
            this.WriteToConsoleAndPromptToContinue("Found {0}", collectionName);
        }
        catch (DocumentClientException de)
        {
            // If the document collection does not exist, create a new collection
            if (de.StatusCode == HttpStatusCode.NotFound)
            {
                DocumentCollection collectionInfo = new DocumentCollection();
                collectionInfo.Id = collectionName;

                // Configure collections for maximum query flexibility including string range queries.
                collectionInfo.IndexingPolicy = new IndexingPolicy(new RangeIndex(DataType.String) { Precision = -1 });

                // Here we create a collection with 400 RU/s.
                await this.client.CreateDocumentCollectionAsync(
                    UriFactory.CreateDatabaseUri(databaseName),
                    collectionInfo,
                    new RequestOptions { OfferThroughput = 400 });

                this.WriteToConsoleAndPromptToContinue("Created {0}", collectionName);
            }
            else
            {
                throw;
            }
        }
    }

Zkopírujte a vložte následující kód do metody **GetStartedDemo** za vytvoření databáze. Tím se vytvoří kolekce dokumentů s názvem *FamilyCollection_oa*.

        this.client = new DocumentClient(new Uri(EndpointUri), PrimaryKey);

        await this.CreateDatabaseIfNotExists("FamilyDB_oa");

        // ADD THIS PART TO YOUR CODE
        await this.CreateDocumentCollectionIfNotExists("FamilyDB_oa", "FamilyCollection_oa");

Stisknutím klávesy **F5** spusťte aplikaci.

Blahopřejeme! Úspěšně jste vytvořili kolekci dokumentů DocumentDB.  

## <a id="CreateDoc"></a>Krok 6: Vytvoření dokumentů JSON
[Dokument](documentdb-resources.md#documents) je možné vytvořit pomocí metody [CreateDocumentAsync](https://msdn.microsoft.com/library/microsoft.azure.documents.client.documentclient.createdocumentasync.aspx) třídy **DocumentClient**. Dokumenty představují uživatelem definovaný (libovolný) obsah JSON. Nyní můžete vložit jeden nebo více dokumentů. Pokud již máte data, která chcete uložit do databáze, můžete použít [nástroj pro migraci dat](documentdb-import-data.md) služby DocumentDB a importovat tato data do databáze.

Nejprve musíme vytvořit třídu **Family**, která bude v této ukázce představovat objekty uložené v DocumentDB. Kromě toho vytvoříme i podtřídy **Parent**, **Child**, **Pet** a **Address**, které se použijí v rámci **Family**. Povšimněte si, že dokumenty musí mít vlastnost **Id** serializovanou jako **id** ve formátu JSON. Vytvořte tyto třídy tak, že za metodu **GetStartedDemo** přidáte následující vnitřní podtřídy.

Zkopírujte a vložte třídy **Family**, **Parent**, **Child**, **Pet** a **Address** za metodu **WriteToConsoleAndPromptToContinue**.

    private void WriteToConsoleAndPromptToContinue(string format, params object[] args)
    {
        Console.WriteLine(format, args);
        Console.WriteLine("Press any key to continue ...");
        Console.ReadKey();
    }

    // ADD THIS PART TO YOUR CODE
    public class Family
    {
        [JsonProperty(PropertyName = "id")]
        public string Id { get; set; }
        public string LastName { get; set; }
        public Parent[] Parents { get; set; }
        public Child[] Children { get; set; }
        public Address Address { get; set; }
        public bool IsRegistered { get; set; }
        public override string ToString()
        {
                return JsonConvert.SerializeObject(this);
        }
    }

    public class Parent
    {
        public string FamilyName { get; set; }
        public string FirstName { get; set; }
    }

    public class Child
    {
        public string FamilyName { get; set; }
        public string FirstName { get; set; }
        public string Gender { get; set; }
        public int Grade { get; set; }
        public Pet[] Pets { get; set; }
    }

    public class Pet
    {
        public string GivenName { get; set; }
    }

    public class Address
    {
        public string State { get; set; }
        public string County { get; set; }
        public string City { get; set; }
    }

Zkopírujte a vložte metodu **CreateFamilyDocumentIfNotExists** za metodu **CreateDocumentCollectionIfNotExists**.

    // ADD THIS PART TO YOUR CODE
    private async Task CreateFamilyDocumentIfNotExists(string databaseName, string collectionName, Family family)
    {
        try
        {
            await this.client.ReadDocumentAsync(UriFactory.CreateDocumentUri(databaseName, collectionName, family.Id));
            this.WriteToConsoleAndPromptToContinue("Found {0}", family.Id);
        }
        catch (DocumentClientException de)
        {
            if (de.StatusCode == HttpStatusCode.NotFound)
            {
                await this.client.CreateDocumentAsync(UriFactory.CreateDocumentCollectionUri(databaseName, collectionName), family);
                this.WriteToConsoleAndPromptToContinue("Created Family {0}", family.Id);
            }
            else
            {
                throw;
            }
        }
    }

Vložte dva dokumenty, jeden pro rodinu Andersenů a druhý pro rodinu Wakefieldů.

Zkopírujte a vložte následující kód do metody **GetStartedDemo** za vytvoření kolekce dokumentů.

    await this.CreateDatabaseIfNotExists("FamilyDB_oa");

    await this.CreateDocumentCollectionIfNotExists("FamilyDB_oa", "FamilyCollection_oa");

    // ADD THIS PART TO YOUR CODE
    Family andersenFamily = new Family
    {
            Id = "Andersen.1",
            LastName = "Andersen",
            Parents = new Parent[]
            {
                    new Parent { FirstName = "Thomas" },
                    new Parent { FirstName = "Mary Kay" }
            },
            Children = new Child[]
            {
                    new Child
                    {
                            FirstName = "Henriette Thaulow",
                            Gender = "female",
                            Grade = 5,
                            Pets = new Pet[]
                            {
                                    new Pet { GivenName = "Fluffy" }
                            }
                    }
            },
            Address = new Address { State = "WA", County = "King", City = "Seattle" },
            IsRegistered = true
    };

    await this.CreateFamilyDocumentIfNotExists("FamilyDB_oa", "FamilyCollection_oa", andersenFamily);

    Family wakefieldFamily = new Family
    {
            Id = "Wakefield.7",
            LastName = "Wakefield",
            Parents = new Parent[]
            {
                    new Parent { FamilyName = "Wakefield", FirstName = "Robin" },
                    new Parent { FamilyName = "Miller", FirstName = "Ben" }
            },
            Children = new Child[]
            {
                    new Child
                    {
                            FamilyName = "Merriam",
                            FirstName = "Jesse",
                            Gender = "female",
                            Grade = 8,
                            Pets = new Pet[]
                            {
                                    new Pet { GivenName = "Goofy" },
                                    new Pet { GivenName = "Shadow" }
                            }
                    },
                    new Child
                    {
                            FamilyName = "Miller",
                            FirstName = "Lisa",
                            Gender = "female",
                            Grade = 1
                    }
            },
            Address = new Address { State = "NY", County = "Manhattan", City = "NY" },
            IsRegistered = false
    };

    await this.CreateFamilyDocumentIfNotExists("FamilyDB_oa", "FamilyCollection_oa", wakefieldFamily);

Stisknutím klávesy **F5** spusťte aplikaci.

Blahopřejeme! Úspěšně jste vytvořili dva dokumenty DocumentDB.  

![Diagram ilustrující hierarchický vztah mezi účtem, online databází, kolekcí a dokumenty používanými v kurzu NoSQL k vytvoření konzolové aplikace v jazyce C#](./media/documentdb-get-started/nosql-tutorial-account-database.png)

## <a id="Query"></a>Krok 7: Dotazování prostředků DocumentDB
DocumentDB podporuje bohaté [dotazy](documentdb-sql-query.md) na dokumenty JSON uložené v každé z kolekcí.  Následující ukázkový kód ukazuje různé dotazy – používající jak syntaxi DocumentDB SQL, tak LINQ – které spouštíme oproti dokumentům vloženým v předchozím kroku.

Zkopírujte a vložte metodu **ExecuteSimpleQuery** za metodu **CreateFamilyDocumentIfNotExists**.

    // ADD THIS PART TO YOUR CODE
    private void ExecuteSimpleQuery(string databaseName, string collectionName)
    {
        // Set some common query options
        FeedOptions queryOptions = new FeedOptions { MaxItemCount = -1 };

            // Here we find the Andersen family via its LastName
            IQueryable<Family> familyQuery = this.client.CreateDocumentQuery<Family>(
                    UriFactory.CreateDocumentCollectionUri(databaseName, collectionName), queryOptions)
                    .Where(f => f.LastName == "Andersen");

            // The query is executed synchronously here, but can also be executed asynchronously via the IDocumentQuery<T> interface
            Console.WriteLine("Running LINQ query...");
            foreach (Family family in familyQuery)
            {
                    Console.WriteLine("\tRead {0}", family);
            }

            // Now execute the same query via direct SQL
            IQueryable<Family> familyQueryInSql = this.client.CreateDocumentQuery<Family>(
                    UriFactory.CreateDocumentCollectionUri(databaseName, collectionName),
                    "SELECT * FROM Family WHERE Family.LastName = 'Andersen'",
                    queryOptions);

            Console.WriteLine("Running direct SQL query...");
            foreach (Family family in familyQueryInSql)
            {
                    Console.WriteLine("\tRead {0}", family);
            }

            Console.WriteLine("Press any key to continue ...");
            Console.ReadKey();
    }

Zkopírujte a vložte následující kód do metody **GetStartedDemo** za vytvoření druhého dokumentu.

    await this.CreateFamilyDocumentIfNotExists("FamilyDB_oa", "FamilyCollection_oa", wakefieldFamily);

    // ADD THIS PART TO YOUR CODE
    this.ExecuteSimpleQuery("FamilyDB_oa", "FamilyCollection_oa");

Než spustíte aplikaci, klikněte v nabídce **Projekt** na *Název projektu*, **Vlastnosti...** a potom klikněte na **Sestavit**. Zrušte zaškrtnutí políčka **Preferovat 32bitovou verzi**, protože informace o směrování oddílů se při spuštěném 32bitovém procesu nedají z dotazu extrahovat.

Stisknutím klávesy **F5** spusťte aplikaci.

Blahopřejeme! Úspěšně jste provedli dotaz proti kolekci DocumentDB.

Následující diagram ilustruje, jak se volá syntaxe dotazu DocumentDB SQL proti kolekci, kterou jste vytvořili. Stejná logika platí také pro dotaz LINQ.

![Diagram ilustrující obor a význam dotazu použitého v kurzu NoSQL k vytvoření konzolové aplikace v jazyce C#](./media/documentdb-get-started/nosql-tutorial-collection-documents.png)

Klíčové slovo [FROM](documentdb-sql-query.md#FromClause) je v dotazu volitelné, protože dotazy DocumentDB již mají obor nastaven na jedinou kolekci. Proto je možné příkaz „FROM Families f“ vyměnit za „FROM root r“ nebo jakoukoli jinou proměnnou, kterou si zvolíte. DocumentDB standardně vyvodí, že Families, root nebo zvolený název proměnné odkazují na aktuální kolekci.

## <a id="ReplaceDocument"></a>Krok 8: Nahrazení dokumentu JSON
DocumentDB podporuje nahrazování dokumentů JSON.  

Zkopírujte a vložte metodu **ReplaceFamilyDocument** za metodu **ExecuteSimpleQuery**.

    // ADD THIS PART TO YOUR CODE
    private async Task ReplaceFamilyDocument(string databaseName, string collectionName, string familyName, Family updatedFamily)
    {
        try
        {
            await this.client.ReplaceDocumentAsync(UriFactory.CreateDocumentUri(databaseName, collectionName, familyName), updatedFamily);
            this.WriteToConsoleAndPromptToContinue("Replaced Family {0}", familyName);
        }
        catch (DocumentClientException de)
        {
            throw;
        }
    }

Zkopírujte a vložte následující kód do metody **GetStartedDemo** za spuštění dotazu na konec metody. Po nahrazení dokumentu tento kód spustí stejný dotaz znovu, aby se zobrazil změněný dokument.

    await this.CreateFamilyDocumentIfNotExists("FamilyDB_oa", "FamilyCollection_oa", wakefieldFamily);

    this.ExecuteSimpleQuery("FamilyDB_oa", "FamilyCollection_oa");

    // ADD THIS PART TO YOUR CODE
    // Update the Grade of the Andersen Family child
    andersenFamily.Children[0].Grade = 6;

    await this.ReplaceFamilyDocument("FamilyDB_oa", "FamilyCollection_oa", "Andersen.1", andersenFamily);

    this.ExecuteSimpleQuery("FamilyDB_oa", "FamilyCollection_oa");

Stisknutím klávesy **F5** spusťte aplikaci.

Blahopřejeme! Úspěšně jste nahradili dokument DocumentDB.

## <a id="DeleteDocument"></a>Krok 9: Odstranění dokumentu JSON
DocumentDB podporuje odstraňování dokumentů JSON.  

Zkopírujte a vložte metodu **DeleteFamilyDocument** za metodu **ReplaceFamilyDocument**.

    // ADD THIS PART TO YOUR CODE
    private async Task DeleteFamilyDocument(string databaseName, string collectionName, string documentName)
    {
        try
        {
            await this.client.DeleteDocumentAsync(UriFactory.CreateDocumentUri(databaseName, collectionName, documentName));
            Console.WriteLine("Deleted Family {0}", documentName);
        }
        catch (DocumentClientException de)
        {
            throw;
        }
    }

Zkopírujte a vložte následující kód do metody **GetStartedDemo** za spuštění druhého dotazu na konec metody.

    await this.ReplaceFamilyDocument("FamilyDB_oa", "FamilyCollection_oa", "Andersen.1", andersenFamily);

    this.ExecuteSimpleQuery("FamilyDB_oa", "FamilyCollection_oa");

    // ADD THIS PART TO CODE
    await this.DeleteFamilyDocument("FamilyDB_oa", "FamilyCollection_oa", "Andersen.1");

Stisknutím klávesy **F5** spusťte aplikaci.

Blahopřejeme! Úspěšně jste odstranili dokument DocumentDB.

## <a id="DeleteDatabase"></a>Krok 10: Odstranění databáze
Odstraněním vytvořené databáze dojde k odstranění databáze a všech jejích podřízených prostředků (kolekcí, dokumentů atd.).

Pokud chcete odstranit celou databázi a její podřízené prostředky, zkopírujte a vložte následující kód do metody **GetStartedDemo** za odstranění dokumentu.

    this.ExecuteSimpleQuery("FamilyDB_oa", "FamilyCollection_oa");

    await this.DeleteFamilyDocument("FamilyDB_oa", "FamilyCollection_oa", "Andersen.1");

    // ADD THIS PART TO CODE
    // Clean up/delete the database
    await this.client.DeleteDatabaseAsync(UriFactory.CreateDatabaseUri("FamilyDB_oa"));

Stisknutím klávesy **F5** spusťte aplikaci.

Blahopřejeme! Úspěšně jste odstranili databázi DocumentDB.

## <a id="Run"></a>Krok 11: Spuštění celé konzolové aplikace jazyka C#
Stiskněte v nástroji Visual Studio klávesu F5 – aplikace se sestaví v režimu ladění.

Měl by se zobrazit výstup počáteční aplikace. Výstup bude zobrazovat výsledky dotazů, které jsme přidali, a měl by odpovídat ukázkovému textu níže.

    Created FamilyDB_oa
    Press any key to continue ...
    Created FamilyCollection_oa
    Press any key to continue ...
    Created Family Andersen.1
    Press any key to continue ...
    Created Family Wakefield.7
    Press any key to continue ...
    Running LINQ query...
        Read {"id":"Andersen.1","LastName":"Andersen","District":"WA5","Parents":[{"FamilyName":null,"FirstName":"Thomas"},{"FamilyName":null,"FirstName":"Mary Kay"}],"Children":[{"FamilyName":null,"FirstName":"Henriette Thaulow","Gender":"female","Grade":5,"Pets":[{"GivenName":"Fluffy"}]}],"Address":{"State":"WA","County":"King","City":"Seattle"},"IsRegistered":true}
    Running direct SQL query...
        Read {"id":"Andersen.1","LastName":"Andersen","District":"WA5","Parents":[{"FamilyName":null,"FirstName":"Thomas"},{"FamilyName":null,"FirstName":"Mary Kay"}],"Children":[{"FamilyName":null,"FirstName":"Henriette Thaulow","Gender":"female","Grade":5,"Pets":[{"GivenName":"Fluffy"}]}],"Address":{"State":"WA","County":"King","City":"Seattle"},"IsRegistered":true}
    Replaced Family Andersen.1
    Press any key to continue ...
    Running LINQ query...
        Read {"id":"Andersen.1","LastName":"Andersen","District":"WA5","Parents":[{"FamilyName":null,"FirstName":"Thomas"},{"FamilyName":null,"FirstName":"Mary Kay"}],"Children":[{"FamilyName":null,"FirstName":"Henriette Thaulow","Gender":"female","Grade":6,"Pets":[{"GivenName":"Fluffy"}]}],"Address":{"State":"WA","County":"King","City":"Seattle"},"IsRegistered":true}
    Running direct SQL query...
        Read {"id":"Andersen.1","LastName":"Andersen","District":"WA5","Parents":[{"FamilyName":null,"FirstName":"Thomas"},{"FamilyName":null,"FirstName":"Mary Kay"}],"Children":[{"FamilyName":null,"FirstName":"Henriette Thaulow","Gender":"female","Grade":6,"Pets":[{"GivenName":"Fluffy"}]}],"Address":{"State":"WA","County":"King","City":"Seattle"},"IsRegistered":true}
    Deleted Family Andersen.1
    End of demo, press any key to exit.

Blahopřejeme! Dokončili jste tento kurz NoSQL a máte funkční konzolovou aplikaci jazyka C#!

## <a id="GetSolution"></a>Získání úplného řešení kurzu NoSQL
Pokud jste neměli dostatek času k dokončení kroků v tomto kurzu, nebo si jen chcete stáhnout ukázky kódu, můžete je získat z [Githubu](https://github.com/Azure-Samples/documentdb-dotnet-getting-started). 

K vytvoření řešení GetStarted budete potřebovat toto:

* Aktivní účet Azure. Pokud žádný nemáte, můžete si zaregistrovat [bezplatný účet](https://azure.microsoft.com/free/).
* [Účet DocumentDB][documentdb-create-account].
* Řešení [GetStarted](https://github.com/Azure-Samples/documentdb-dotnet-getting-started) dostupné na GitHubu

Pokud chcete obnovit reference na sadu DocumentDB .NET SDK v nástroji Visual Studio, klikněte v Průzkumníkovi řešení pravým tlačítkem na řešení **GetStarted** a pak levým na **Povolit obnovení balíčků NuGet**. Dále v souboru App.config aktualizujte hodnoty EndpointUrl a AuthorizationKey tak, jak je popsáno v části [Připojení k účtu DocumentDB](#Connect).

Než spustíte aplikaci, klikněte v nabídce **Projekt** na *Název projektu*, **Vlastnosti...** a potom klikněte na **Sestavit**. Zrušte zaškrtnutí políčka **Preferovat 32bitovou verzi**.

A to je vše, stačí sestavit a máte hotovo.


## <a name="next-steps"></a>Další kroky
* Chcete složitější kurz NoSQL pro ASP.NET MVC? Přečtěte si o [vytvoření webové aplikace pomocí ASP.NET MVC a DocumentDB](documentdb-dotnet-application.md).
* Chcete testovat škálování a výkon s DocumentDB? Přečtěte si o [testování výkonu a škálování s Azure DocumentDB](documentdb-performance-testing.md).
* Naučte se [monitorovat účet DocumentDB](documentdb-monitor-accounts.md).
* Spouštějte dotazy proti ukázkovým datovým sadám v [Query Playground](https://www.documentdb.com/sql/demo).
* Přečtěte si více o tomto programovacím modelu v části Vyvíjejte na [stránce dokumentace DocumentDB](https://azure.microsoft.com/documentation/services/documentdb/).

[documentdb-create-account]: documentdb-create-account.md
[keys]: media/documentdb-get-started/nosql-tutorial-keys.png

