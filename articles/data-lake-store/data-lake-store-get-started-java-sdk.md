---
title: "Použití sady Java SDK pro Data Lake Store k vývoji aplikací | Dokumentace Microsoftu"
description: "Použití sady Java SDK pro Azure Data Lake Store k vývoji aplikací"
services: data-lake-store
documentationcenter: 
author: nitinme
manager: jhubbard
editor: cgronlun
ms.assetid: d10e09db-5232-4e84-bb50-52efc2c21887
ms.service: data-lake-store
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 12/23/2016
ms.author: nitinme
translationtype: Human Translation
ms.sourcegitcommit: c157da7bf53e2d0762624e8e71e56e956db04a24
ms.openlocfilehash: a80da95328a6f3c47edf6e9be9e786437a8c316e


---
# <a name="get-started-with-azure-data-lake-store-using-java"></a>Začínáme s Azure Data Lake Store pomocí jazyka Java
> [!div class="op_single_selector"]
> * [Azure Portal](data-lake-store-get-started-portal.md)
> * [PowerShell](data-lake-store-get-started-powershell.md)
> * [.NET SDK](data-lake-store-get-started-net-sdk.md)
> * [Java SDK](data-lake-store-get-started-java-sdk.md)
> * [REST API](data-lake-store-get-started-rest-api.md)
> * [Azure CLI](data-lake-store-get-started-cli.md)
> * [Node.js](data-lake-store-manage-use-nodejs.md)
> * [Python](data-lake-store-get-started-python.md)
>
> 

Naučte se používat sadu Java SDK pro Azure Data Lake Store k provádění základních operací, jako je vytváření složek, nahrávání a stahování datových souborů atd. Další informace týkající se Data Lake najdete v tématu [Azure Data Lake Store](data-lake-store-overview.md).

Dokumentaci rozhraní API sady Java SDK pro Azure Data Lake Store najdete v tématu [Dokumentace rozhraní API sady Java SDK pro Azure Data Lake Store](https://azure.github.io/azure-data-lake-store-java/javadoc/).

## <a name="prerequisites"></a>Požadavky
* Java Development Kit (JDK 7 nebo vyšší s využitím Java verze 1.7 nebo vyšší)
* Účet Azure Data Lake Store. Postupujte podle pokynů v tématu [Začínáme s Azure Data Lake Store s použitím webu Azure Portal](data-lake-store-get-started-portal.md).
* [Maven](https://maven.apache.org/install.html). V tomto kurzu se používá Maven pro závislosti sestavení a projektu. I když je možné sestavení vytvářet bez použití systému pro sestavení, jako je Maven a Gradle, tyto systémy podstatně usnadňují správu závislostí.
* (Volitelné) Rozhraní IDE, jako je například [IntelliJ IDEA](https://www.jetbrains.com/idea/download/) nebo [Eclipse](https://www.eclipse.org/downloads/) nebo podobné.

## <a name="how-do-i-authenticate-using-azure-active-directory"></a>Jak můžu ověřovat pomocí služby Azure Active Directory?
V tomto kurzu používáme tajný kód klienta aplikace Azure AD k načtení tokenu Azure Active Directory (ověřování mezi službami). Tento token používáme k vytvoření objektu klienta služby Data Lake Store k provádění operací operačních souborů a adresářů. Abychom mohli ukázat postup ověřování pomocí služby Azure Data Lake Store za použití sdíleného tajného kódu klienta, provedeme následující základní kroky:

1. Vytvoření webové aplikace Azure AD
2. Načtení ID klienta, tajného kódu klienta a koncového bodu tokenu pro webovou aplikaci Azure AD
3. Konfigurace přístupu pro webovou aplikaci Azure AD pro soubor nebo složku služby Data Lake Store, ke kterým chcete získat přístup z aplikace Java, kterou vytváříte

Pokyny k provedení těchto kroků najdete v tématu [Vytvoření aplikace služby Active Directory](data-lake-store-authenticate-using-active-directory.md#create-an-active-directory-application).

V Azure Active Directory jsou také další možnosti, jak načíst token. Můžete vybírat z mnoha různých ověřovacích mechanismů vhodných pro váš scénář, jako je například spuštění aplikace v prohlížeči, distribuce aplikace jako desktopové aplikace nebo spuštění serverové aplikace místně nebo na virtuálním počítači Azure. Vybírat můžete také z různých typů přihlašovacích údajů, jako jsou hesla, certifikáty, dvojúrovňové ověřování atd. Kromě toho umožňuje Azure Active Directory synchronizaci uživatelů místní služby Active Directory s cloudem. Podrobnosti najdete v tématu [Scénáře ověřování pro Azure Active Directory](../active-directory/active-directory-authentication-scenarios.md). 

## <a name="create-a-java-application"></a>Vytvoření aplikace Java
Ukázka kódu, která je k dispozici [na GitHubu](https://azure.microsoft.com/documentation/samples/data-lake-store-java-upload-download-get-started/), vás provede procesem vytvoření souborů v úložišti, zřetězení souborů, stažení souboru a nakonec odstranění některých souborů v úložišti. V této části článku se dozvíte o hlavních částech kódu.

1. Vytvořte projekt Maven pomocí příkazu [mvn archetype](https://maven.apache.org/guides/getting-started/maven-in-five-minutes.html) z příkazového řádku nebo pomocí rozhraní IDE. Pokyny k vytvoření projektu jazyka Java s použitím IntelliJ najdete [zde](https://www.jetbrains.com/help/idea/2016.1/creating-and-running-your-first-java-application.html). Pokyny k vytvoření projektu s použitím Eclipse najdete [zde](http://help.eclipse.org/mars/index.jsp?topic=%2Forg.eclipse.jdt.doc.user%2FgettingStarted%2Fqs-3.htm). 
2. Přidejte k souboru Maven **pom.xml** následující závislosti. Přidejte mezi značky **\</version>** a **\</project>** následující fragment textu:
   
        <dependencies>
          <dependency>
            <groupId>com.microsoft.azure</groupId>
            <artifactId>azure-data-lake-store-sdk</artifactId>
            <version>2.1.1</version>
          </dependency>
          <dependency>
            <groupId>org.slf4j</groupId>
            <artifactId>slf4j-nop</artifactId>
            <version>1.7.21</version>
          </dependency>
        </dependencies>
   
    První závislostí je použití sady SDK pro Data Lake Store (`azure-datalake-store`) z úložiště maven. Druhou závislostí (`slf4j-nop`) je zadání protokolovacího rozhraní, které se pro tuto aplikaci použije. Sada SDK pro službu Data Lake Store používá při protokolování [slf4j](http://www.slf4j.org/), takže máte možnost si vybrat z řady oblíbených protokolovacích rozhraní, jako je log4j, Java, logback atd., nebo nemusíte použít žádné protokolování. Pro tento příklad zakážeme protokolování a použijeme tedy vazbu **slf4j-nop**. Pokud chcete ve své aplikaci použít jiné možnosti protokolování, přečtěte si informace [zde](http://www.slf4j.org/manual.html#projectDep).

### <a name="add-the-application-code"></a>Přidání kódu aplikace
Kód má tři hlavní části.

1. Získání tokenu Azure Active Directory
2. Použití tokenu k vytvoření klienta služby Data Lake Store
3. Použití klienta služby Data Lake Store k provedení operací

#### <a name="step-1-obtain-an-azure-active-directory-token"></a>Krok 1: Získání tokenu Azure Active Directory
V sadě SDK pro Data Lake Store jsou vhodné metody, které umožňují získat tokeny zabezpečení potřebné ke komunikaci s účtem služby Data Lake Store. Není ale povinné použít tuto sadu SDK a tyto metody. Můžete použít také jakýkoliv jiný způsob získání tokenu, například sadu [Azure Active Directory SDK](https://github.com/AzureAD/azure-activedirectory-library-for-java) nebo vlastní kód.

Pokud chcete k získání tokenu pro webovou aplikaci služby Active Directory, kterou jste vytvořili dříve, použít sadu SDK pro Data Lake Store, použijte statické metody ve třídě `AzureADAuthenticator`. Místo **FILL-IN-HERE** zadejte skutečné hodnoty pro webovou aplikaci Azure Active Directory.

    private static String clientId = "FILL-IN-HERE";
    private static String authTokenEndpoint = "FILL-IN-HERE";
    private static String clientKey = "FILL-IN-HERE";

    AzureADToken token = AzureADAuthenticator.getTokenUsingClientCreds(authTokenEndpoint, clientId, clientKey);

#### <a name="step-2-create-an-azure-data-lake-store-client-adlstoreclient-object"></a>Krok 2: Vytvoření objektu klienta služby Azure Data Lake Store (ADLStoreClient)
Pokud chcete vytvořit objekt [ADLStoreClient](https://azure.github.io/azure-data-lake-store-java/javadoc/), musíte zadat název účtu služby Data Lake Store a token služby Azure Active Directory vygenerovaný v posledním kroku. Název účtu služby Data Lake Store musí být plně kvalifikovaný název domény. Například **FILL-IN-HERE** nahraďte něčím jako **mydatalakestore.azuredatalakestore.net**.

    private static String accountFQDN = "FILL-IN-HERE";  // full account FQDN, not just the account name
    ADLStoreClient client = ADLStoreClient.createClient(accountFQDN, token);

### <a name="step-3-use-the-adlstoreclient-to-perform-file-and-directory-operations"></a>Krok 3: Použití objektu ADLStoreClient k provedení operací souborů a adresářů
Kód níže obsahuje příklady fragmentů některých běžných operací. Na další operace se můžete podívat v kompletní [Dokumentaci rozhraní API sady Java SDK pro Azure Data Lake Store](https://azure.github.io/azure-data-lake-store-java/javadoc/) k objektu **ADLStoreClient**.

Poznámka: Soubory se čtou a do souborů se zapisuje pomocí standardních streamů Java. Znamená to, že kterýkoliv stream Java můžete umístit do vrstvy nad streamy služby Data Lake Store. Díky tomu budete moct využít možností standardních funkcí Java (například streamů tisku pro formátovaný výstup nebo kteréhokoliv ze streamů komprese nebo šifrování pro získání dalších funkcí atd.).

    // set file permission
    client.setPermission(filename, "744");

    // append to file
    stream = client.getAppendStream(filename);
    stream.write(getSampleContent());
    stream.close();

    // Read File
    InputStream in = client.getReadStream(filename);
    byte[] b = new byte[64000];
    while (in.read(b) != -1) {
        System.out.write(b);
    }
    in.close();

    // concatenate the two files into one
    List<String> fileList = Arrays.asList("/a/b/c.txt", "/a/b/d.txt");
    client.concatenateFiles("/a/b/f.txt", fileList);

    //rename the file
    client.rename("/a/b/f.txt", "/a/b/g.txt");

    // list directory contents
    List<DirectoryEntry> list = client.enumerateDirectory("/a/b", 2000);
    System.out.println("Directory listing for directory /a/b:");
    for (DirectoryEntry entry : list) {
        printDirectoryInfo(entry);
    }

    // delete directory along with all the subdirectories and files in it
    client.deleteRecursive("/a");

#### <a name="step-4-build-and-run-the-application"></a>Krok 4: Sestavení a spuštění aplikace
1. Pokud chcete aplikaci spustit z rozhraní IDE, stiskněte tlačítko **Spustit**. Pokud ji chcete spustit z Mavenu, použijte příkaz [exec:exec](http://www.mojohaus.org/exec-maven-plugin/exec-mojo.html).
2. Jestliže chcete vytvořit samostatný soubor jar, který budete moct spustit z příkazového řádku, vytvořte ho se všemi závislostmi s použitím [modulu plug-in sestavení Maven](http://maven.apache.org/plugins/maven-assembly-plugin/usage.html). U pom.xml v [ukázkovém zdrojovém kódu na githubu](https://github.com/Azure-Samples/data-lake-store-java-upload-download-get-started/blob/master/pom.xml) najdete příklad, jak to provést.

## <a name="next-steps"></a>Další kroky
* [Zabezpečení dat ve službě Data Lake Store](data-lake-store-secure-data.md)
* [Použití Azure Data Lake Analytics se službou Data Lake Store](../data-lake-analytics/data-lake-analytics-get-started-portal.md)
* [Použití Azure HDInsight se službou Data Lake Store](data-lake-store-hdinsight-hadoop-use-portal.md)




<!--HONumber=Nov16_HO4-->


