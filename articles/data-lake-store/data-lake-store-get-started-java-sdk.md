<properties
   pageTitle="Použití sady Java SDK pro Data Lake Store k vývoji aplikací | Azure"
   description="Použití sady Java SDK pro Azure Data Lake Store k vývoji aplikací"
   services="data-lake-store"
   documentationCenter=""
   authors="nitinme"
   manager="paulettm"
   editor="cgronlun"/>

<tags
   ms.service="data-lake-store"
   ms.devlang="na"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="na"
   ms.workload="big-data"
   ms.date="05/12/2016"
   ms.author="nitinme"/>

# Začínáme s Azure Data Lake Store pomocí jazyka Java

> [AZURE.SELECTOR]
- [Portál](data-lake-store-get-started-portal.md)
- [PowerShell](data-lake-store-get-started-powershell.md)
- [.NET SDK](data-lake-store-get-started-net-sdk.md)
- [Java SDK](data-lake-store-get-started-java-sdk.md)
- [REST API](data-lake-store-get-started-rest-api.md)
- [Azure CLI](data-lake-store-get-started-cli.md)
- [Node.js](data-lake-store-manage-use-nodejs.md)

Naučte se používat sadu Java SDK pro Azure Data Lake Store k vytvoření účtu Azure Data Lake a provádění základních operací, jako je vytváření složek, nahrávání a stahování datových souborů, odstranění účtu atd. Další informace týkající se Data Lake najdete v tématu [Azure Data Lake Store](data-lake-store-overview.md).

## Java SDK služby Azure Data Lake Store

Následující odkazy obsahují umístění ke stažení sady Java SDK pro Data Lake Store a referenční informace sady Java SDK. Pro účely tohoto kurzu není nutné stáhnout sadu SDK ani postupovat podle pokynů v referenčním dokumentu. Tyto odkazy jsou uvedeny jen pro informaci.

* Zdrojový kód sady Java SDK pro Data Lake Store je dostupný na webu [GitHub](https://github.com/Azure/azure-sdk-for-java).
* Referenční informace sady Java SDK pro Data Lake Store jsou dostupné na adrese [https://azure.github.io/azure-sdk-for-java/](https://azure.github.io/azure-sdk-for-java/).

## Předpoklady

* Java Development Kit (JDK) 8 (využívající jazyk Java verze 1.8).
* IntelliJ nebo jiné vhodné vývojové prostředí Java. Tato položka je nepovinná, ale doporučuje se. Níže uvedené pokyny používají IntelliJ.
* **Předplatné Azure**. Viz [Získání bezplatné zkušební verze Azure](https://azure.microsoft.com/pricing/free-trial/).
* **Aktivujte předplatné Azure** pro verzi Public Preview služby Data Lake Store. Viz [pokyny](data-lake-store-get-started-portal.md#signup).
* **Vytvoření aplikace Azure Active Directory**. Ověřování pomocí služby Azure Active Directory můžete provádět dvěma způsoby – **interaktivně** a **neinteraktivně**. Předpoklady se liší podle toho, jak chcete ověřovat.
    * **V případě interaktivního ověřování** – Ve službě Azure Active Directory je nutné vytvořit **nativní klientskou aplikaci**. Po vytvoření aplikace načtěte následující hodnoty týkající se aplikace.
        - Získejte **ID klienta** a **identifikátor URI přesměrování** aplikace.
        - Nastavte delegovaná oprávnění.

    * **V případě neinteraktivního ověřování** (používá se v tomto článku) – Ve službě Azure Active Directory je nutné vytvořit **webovou aplikaci**. Po vytvoření aplikace načtěte následující hodnoty týkající se aplikace.
        - Získejte **ID klienta**, **tajný klíč klienta** a **identifikátor URI přesměrování** aplikace.
        - Nastavte delegovaná oprávnění.
        - Přiřaďte aplikaci Azure Active Directory k roli. Tato role může být na úrovni rozsahu, ve kterém chcete aplikaci Azure Active Directory udělit oprávnění. Aplikaci můžete přiřadit například na úrovni předplatného nebo na úrovni skupiny prostředků. 

    Pokyny týkající se načtení těchto hodnot, nastavení oprávnění a přiřazování rolí najdete v tématu [Vytvoření aplikace Active Directory a objektu zabezpečení pomocí portálu](../resource-group-create-service-principal-portal.md).

## Jak můžu ověřovat pomocí služby Azure Active Directory?

Následující fragment kódu obsahuje kód pro **neinteraktivní** ověřování, kdy aplikace poskytuje svoje vlastní přihlašovací údaje.

Pro účely tohoto kurzu je nutné, abyste aplikaci udělili oprávnění k vytváření prostředků v Azure. **Důrazně doporučujeme**, abyste této aplikaci pro účely tohoto kurzu udělili oprávnění Přispěvatel jenom k nové, nepoužité a prázdné skupině prostředků v předplatném Azure.

## Vytvoření aplikace Java

1. Otevřete IntelliJ a pomocí šablony **aplikace příkazového řádku** vytvořte nový projekt v jazyce Java. Dokončete průvodce a vytvořte projekt.

2. Klikněte pravým tlačítkem na projekt na levé straně obrazovky a klikněte na možnost **Přidat podporu architektury**. Vyberte možnost **Maven** a klikněte na tlačítko **OK**.

3. Otevřete nově vytvořený soubor **pom.xml** a mezi značky **\</version>** a **\</project>** přidejte následující fragment textu:

    >[AZURE.NOTE] Tento krok je dočasný, dokud nebude sada Azure Data Lake Store SDK dostupná v nástroji Maven. Jakmile bude sada SDK dostupná v nástroji Maven, tento článek budeme aktualizovat. Všechny budoucí aktualizace této sady SDK budou dostupné prostřednictvím nástroje Maven.

        <repositories>
            <repository>
                <id>adx-snapshots</id>
                <name>Azure ADX Snapshots</name>
                <url>http://adxsnapshots.azurewebsites.net/</url>
                <layout>default</layout>
                <snapshots>
                    <enabled>true</enabled>
                </snapshots>
            </repository>
            <repository>
                <id>oss-snapshots</id>
                <name>Open Source Snapshots</name>
                <url>https://oss.sonatype.org/content/repositories/snapshots/</url>
                <layout>default</layout>
                <snapshots>
                    <enabled>true</enabled>
                    <updatePolicy>always</updatePolicy>
                </snapshots>
            </repository>
        </repositories>
        <dependencies>
            <dependency>
                <groupId>com.microsoft.azure</groupId>
                <artifactId>azure-client-authentication</artifactId>
                <version>1.0.0-SNAPSHOT</version>
            </dependency>
            <dependency>
                <groupId>com.microsoft.azure</groupId>
                <artifactId>azure-mgmt-datalake-store</artifactId>
                <version>1.0.0-SNAPSHOT</version>
            </dependency>
        </dependencies>


4. Přejděte k položce **Soubor**, **Nastavení** a potom **Sestavení, provádění a nasazení**. Rozbalte položku **Nástroje sestavení **, **Maven** a potom rozbalte položku **Import**. Zaškrtněte políčko u možnosti **Automaticky importovat projekty Maven**. Klikněte na tlačítko **Použít** a potom **OK**.

5. V levém podokně přejděte k položce **src**, **main**, **java**, **\<název balíčku>** a potom otevřete položku **Main.java** a stávající blok kódu nahraďte následujícím kódem. Zadejte také hodnoty parametrů použitých ve fragmentu kódu (například **localFolderPath**, **_adlsAccountName** a **_resourceGroupName**) a nahraďte zástupné symboly **CLIENT-ID**, **CLIENT-SECRET**, **TENANT-ID** a **SUBSCRIPTION-ID**.

    Tento kód projde procesem vytvoření účtu Data Lake Store, vytvoření souborů v úložišti, zřetězení souborů, stažení souboru a nakonec odstranění účtu.

        package com.company;
        
        import com.microsoft.azure.CloudException;
        import com.microsoft.azure.credentials.ApplicationTokenCredentials;
        import com.microsoft.azure.management.datalake.store.*;
        import com.microsoft.azure.management.datalake.store.models.*;
        import com.microsoft.rest.credentials.ServiceClientCredentials;
        import java.io.*;
        import java.nio.charset.Charset;
        import java.util.ArrayList;
        import java.util.List;
        
        public class Main {
            private static String _adlsAccountName;
            private static String _resourceGroupName;
            private static String _location;
        
            private static String _tenantId;
            private static String _subId;
            private static String _clientId;
            private static String _clientSecret;
        
            private static DataLakeStoreAccountManagementClient _adlsClient;
            private static DataLakeStoreFileSystemManagementClient _adlsFileSystemClient;
        
            public static void main(String[] args) throws Exception {
                _adlsAccountName = "<DATA-LAKE-STORE-NAME>";
                _resourceGroupName = "<RESOURCE-GROUP-NAME>";
                _location = "East US 2";
        
                _tenantId = "<TENANT-ID>";
                _subId =  "<SUBSCRIPTION-ID>";
                _clientId = "<CLIENT-ID>";
        
                _clientSecret = "<CLIENT-SECRET>"; // TODO: For production scenarios, we recommend that you replace this line with a more secure way of acquiring the application client secret, rather than hard-coding it in the source code.
        
                String localFolderPath = "C:\\local_path\\"; // TODO: Change this to any unused, new, empty folder on your local machine.
        
                // Authenticate
                ApplicationTokenCredentials creds = new ApplicationTokenCredentials(_clientId, _tenantId, _clientSecret, null);
                SetupClients(creds);
        
                // Create Data Lake Store account
                WaitForNewline("Authenticated.", "Creating NEW account.");
                CreateAccount();
                WaitForNewline("Account created.", "Displaying account(s).");
        
                // List Data Lake Store accounts that this app can access
                System.out.println(String.format("All ADL Store accounts that this app can access in subscription %s:", _subId));
                List<DataLakeStoreAccount> adlsListResult = _adlsClient.getAccountOperations().list().getBody();
                for (DataLakeStoreAccount acct : adlsListResult) {
                    System.out.println(acct.getName());
                }
                WaitForNewline("Account(s) displayed.", "Creating files.");
        
                // Create two files in Data Lake Store: file1.csv and file2.csv
                CreateFile("/file1.csv", "123,abc", true);
                CreateFile("/file2.csv", "456,def", true);
                WaitForNewline("Files created.", "Concatenating files.");
        
                // Concatenate two files in Data Lake Store
                List<String> srcFilePaths = new ArrayList<String>();
                srcFilePaths.add("/file1.csv");
                srcFilePaths.add("/file2.csv");
                ConcatenateFiles(srcFilePaths, "/input.csv");
                WaitForNewline("Files concatenated.", "Downloading file.");
        
                // Download file from Data Lake Store
                DownloadFile("/input.csv", localFolderPath + "input.csv");
                WaitForNewline("File downloaded.", "Deleting file.");
        
                // Delete file from Data Lake Store
                DeleteFile("/input.csv");
                WaitForNewline("File deleted.", "Deleting account.");
        
                // Delete account
                DeleteAccount();
                WaitForNewline("Account deleted.", "DONE.");
            }
        
            //Set up clients
            public static void SetupClients(ServiceClientCredentials creds)
            {
                _adlsClient = new DataLakeStoreAccountManagementClientImpl(creds);
                _adlsFileSystemClient = new DataLakeStoreFileSystemManagementClientImpl(creds);
        
                _adlsClient.setSubscriptionId(_subId);
            }
        
            // Helper function to show status and wait for user input
            public static void WaitForNewline(String reason, String nextAction)
            {
                if (nextAction == null)
                    nextAction = "";
                if (!nextAction.isEmpty())
                {
                    System.out.println(reason + "\r\nPress ENTER to continue...");
                    try{System.in.read();}
                    catch(Exception e){}
                    System.out.println(nextAction);
                }
                else
                {
                    System.out.println(reason + "\r\nPress ENTER to continue...");
                    try{System.in.read();}
                    catch(Exception e){}
                }
            }
        
            // Create Data Lake Store account
            public static void CreateAccount() throws InterruptedException, CloudException, IOException {
                DataLakeStoreAccount adlsParameters = new DataLakeStoreAccount();
                adlsParameters.setLocation(_location);
        
                _adlsClient.getAccountOperations().create(_resourceGroupName, _adlsAccountName, adlsParameters);
            }
        
            // Create file
            public static void CreateFile(String path) throws IOException, CloudException {
                _adlsFileSystemClient.getFileSystemOperations().create(_adlsAccountName, path);
            }
        
            // Create file with contents
            public static void CreateFile(String path, String contents, boolean force) throws IOException, CloudException {
                byte[] bytesContents = contents.getBytes();
        
                _adlsFileSystemClient.getFileSystemOperations().create(_adlsAccountName, path, bytesContents, force);
            }
        
            // Append to file
            public static void AppendToFile(String path, String contents) throws IOException, CloudException {
                byte[] bytesContents = contents.getBytes();
        
                _adlsFileSystemClient.getFileSystemOperations().append(_adlsAccountName, path, bytesContents);
            }
        
            // Concatenate files
            public static void ConcatenateFiles(List<String> srcFilePaths, String destFilePath) throws IOException, CloudException {
                _adlsFileSystemClient.getFileSystemOperations().concat(_adlsAccountName, destFilePath, srcFilePaths);
            }
        
            // Delete concatenated file
            public static void DeleteFile(String filePath) throws IOException, CloudException {
                _adlsFileSystemClient.getFileSystemOperations().delete(_adlsAccountName, filePath);
            }
        
            // Get file or directory info
            public static FileStatusProperties GetItemInfo(String path) throws IOException, CloudException {
                return _adlsFileSystemClient.getFileSystemOperations().getFileStatus(_adlsAccountName, path).getBody().getFileStatus();
            }
        
            // List files and directories
            public static List<FileStatusProperties> ListItems(String directoryPath) throws IOException, CloudException {
                return _adlsFileSystemClient.getFileSystemOperations().listFileStatus(_adlsAccountName, directoryPath).getBody().getFileStatuses().getFileStatus();
            }
        
            // Download file
            public static void DownloadFile(String srcPath, String destPath) throws IOException, CloudException {
                InputStream stream = _adlsFileSystemClient.getFileSystemOperations().open(_adlsAccountName, srcPath).getBody();
        
                PrintWriter pWriter = new PrintWriter(destPath, Charset.defaultCharset().name());
        
                String fileContents = "";
                if (stream != null) {
                    Writer writer = new StringWriter();
        
                    char[] buffer = new char[1024];
                    try {
                        Reader reader = new BufferedReader(
                                new InputStreamReader(stream, "UTF-8"));
                        int n;
                        while ((n = reader.read(buffer)) != -1) {
                            writer.write(buffer, 0, n);
                        }
                    } finally {
                        stream.close();
                    }
                    fileContents =  writer.toString();
                }
        
                pWriter.println(fileContents);
                pWriter.close();
            }
        
            // Delete account
            public static void DeleteAccount() throws InterruptedException, CloudException, IOException {
                _adlsClient.getAccountOperations().delete(_resourceGroupName, _adlsAccountName);
            }
        }


6. Spusťte aplikaci. Postupujte podle výzev a spusťte a dokončete aplikaci.

## Další kroky

- [Zabezpečení dat v Data Lake Store](data-lake-store-secure-data.md)
- [Použití Azure Data Lake Analytics s Data Lake Store](../data-lake-analytics/data-lake-analytics-get-started-portal.md)
- [Použití Azure HDInsight s Data Lake Store](data-lake-store-hdinsight-hadoop-use-portal.md)



<!--HONumber=Jun16_HO2-->


