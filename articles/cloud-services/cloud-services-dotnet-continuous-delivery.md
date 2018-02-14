---
title: "Nastavené průběžné doručování pro cloudové služby se sadou TFS v Azure | Microsoft Docs"
description: "Zjistěte, jak nastavit průběžné doručování Azure cloudových aplikací. Ukázky kódu pro MSBuild příkazy příkazového řádku a skripty prostředí PowerShell."
services: cloud-services
documentationcenter: 
author: kraigb
manager: ghogen
editor: 
ms.assetid: 4f3c93c6-5c82-4779-9d19-7404a01e82ca
ms.service: cloud-services
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 06/12/2017
ms.author: kraigb
ms.openlocfilehash: 470fda7722e6a22e50ed66a7bc193fc7c9f71536
ms.sourcegitcommit: cc03e42cffdec775515f489fa8e02edd35fd83dc
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 12/07/2017
---
# <a name="continuous-delivery-for-cloud-services-in-azure"></a>Nastavené průběžné doručování pro cloudové služby v Azure
Proces popsaný v tomto článku se dozvíte, jak nastavit průběžné doručování Azure cloudových aplikací. Po každém vrácení kódu se změnami vám tento proces umožní automaticky vytvořit balíčky a nasadit balíček do Azure. Proces sestavení balíčku, který je popsaný v tomto článku je ekvivalentní **balíček** příkaz v sadě Visual Studio a publikování kroky jsou rovnocenná **publikovat** příkazu v sadě Visual Studio.
Článek vysvětluje metod, které byste použili k vytvoření serveru sestavení s příkazy příkazového řádku nástroje MSBuild a skriptů prostředí Windows PowerShell a také ukazuje, jak Volitelně lze konfigurovat Visual Studio Team Foundation Server – definice sestavení Team použití nástroje MSBuild příkazy a skripty prostředí PowerShell. Proces je přizpůsobitelné prostředí pro sestavení a prostředí Azure cíl.

Můžete také použít Visual Studio Team Services, verzi sady TFS, která je hostovaná v Azure k tomu snadněji. 

Než začnete, byste měli publikovat aplikace ze sady Visual Studio.
To zajistí, že všechny prostředky, které jsou dostupné a inicializovaného při pokusu automatizovat proces publikace.

## <a name="1-configure-the-build-server"></a>1: Konfigurace serveru sestavení
Než vytvoříte balíček Azure pomocí nástroje MSBuild je na serveru sestavení nainstalovat požadovaný software a nástroje.

Visual Studio není musí být nainstalovaný na serveru sestavení. Pokud chcete použít ke správě serveru sestavení služby sestavení Team Foundation, postupujte podle pokynů [služby sestavení Team Foundation] [ Team Foundation Build Service] dokumentaci.

1. Na serveru, sestavení, nainstalujte [rozhraní .NET Framework 4.5.2][.NET Framework 4.5.2], což zahrnuje MSBuild.
2. Nainstalujte si nejnovější verzi [nástroje pro tvorbu Azure pro .NET](https://azure.microsoft.com/develop/net/).
3. Nainstalujte [knihovny Azure pro .NET](http://go.microsoft.com/fwlink/?LinkId=623519).
4. Zkopírujte soubor Microsoft.WebApplication.targets z instalace sady Visual Studio k serveru sestavení.

   Na počítači s nainstalovanou sadu Visual Studio, tento soubor je umístěný v adresáři C:\\Program Files(x86)\\MSBuild\\Microsoft\\Visual Studio\\v14.0\\WebApplications. Měli byste ho zkopírovat do stejného adresáře na serveru sestavení.
5. Nainstalujte [nástroje Azure pro sadu Visual Studio](https://www.visualstudio.com/features/azure-tools-vs.aspx).

## <a name="2-build-a-package-using-msbuild-commands"></a>2: vytvoření balíčku pomocí příkazů nástroje MSBuild
Tato část popisuje, jak můžete vytvořit pomocí nástroje MSBuild příkazu, který vytvoří balíček Azure. Tento krok spusťte na serveru sestavení ověření všechno správně nakonfigurován a že příkaz MSBuild provést požadované na práci. Tento příkaz můžete přidat buď existující sestavení skriptů na serveru sestavení nebo příkazového řádku v definici sestavení TFS můžete použít, jak je popsáno v následující části. Další informace o parametry příkazového řádku a nástroje MSBuild najdete v tématu [příkazového řádku MSBuild – Reference](https://msdn.microsoft.com/library/ms164311%28v=vs.140%29.aspx).

1. Pokud Visual Studio je nainstalovaná na serveru sestavení, vyhledejte a vyberte **Visual Studio – příkazový řádek** v **nástroje sady Visual Studio** složky v systému Windows.

   Pokud Visual Studio není nainstalována na serveru sestavení, otevřete příkazový řádek a ujistěte se, že je dostupný na cestě MSBuild.exe. MSBuild je nainstalován s rozhraním .NET Framework v cestě % WINDIR %\\Microsoft.NET\\Framework\\*verze*. Například MSBuild.exe přidat do proměnné prostředí PATH v případě, že máte nainstalované rozhraní .NET Framework 4, zadejte na příkazovém řádku následující příkaz:

       set PATH=%PATH%;"C:\Windows\Microsoft.NET\Framework\v4.0.30319"
2. Na příkazovém řádku přejděte do složky obsahující soubor projektu Azure, který chcete vytvořit.
3. Spuštění nástroje MSBuild pomocí / target: publikování možnost jako v následujícím příkladu:

       MSBuild /target:Publish

   Tuto možnost lze zkrátit na /t: publikování. Možnost /t:Publish v nástroji MSBuild Nezaměňovat s příkazy publikování, který je k dispozici v sadě Visual Studio Pokud máte sadu Azure SDK nainstalována. /T: publikovat pouze sestavení možnost Azure balíčky. Stejně jako příkazy Publikovat v sadě Visual Studio Nenasazuje balíčky.

   Volitelně můžete zadat název projektu jako parametr MSBuild. Pokud není zadaný, použije se aktuální adresář. Další informace o možnostech příkazového řádku nástroje MSBuild najdete v tématu [příkazového řádku MSBuild – Reference](https://msdn.microsoft.com/library/ms164311%28v=vs.140%29.aspx).
4. Vyhledejte výstup. Ve výchozím nastavení, tento příkaz vytvoří adresář ve vztahu k kořenové složce projektu, například *ProjectDir*\\bin\\*konfigurace*\\app.publish\\. Při sestavování projektu Azure generovat dva soubory, samotný soubor balíčku a doprovodné konfigurační soubor:

   * Project.cspkg
   * Objekt ServiceConfiguration. *TargetProfile*.cscfg

   Ve výchozím nastavení každý projekt, Azure zahrnuje jednu konfigurační soubor služby (soubor .cscfg) pro místní sestavení (ladění) a druhou pro sestavení cloudu (pracovním nebo produkčním), ale můžete přidat nebo odebrat soubory konfigurace služby podle potřeby. Když vytvoříte balíček Visual Studia, zobrazí se výzva které konfigurační soubor služby zahrnout společně se balíček.
5. Zadejte konfigurační soubor služby. Když vytvoříte balíček pomocí nástroje MSBuild, místní služby konfigurační soubor je zahrnuté ve výchozím nastavení. K zahrnutí konfigurační soubor různé služby, nastavte vlastnost TargetProfile MSBuild příkazu, jako v následujícím příkladu:

       MSBuild /t:Publish /p:TargetProfile=Cloud
6. Zadejte umístění pro výstup. Nastavit cestu, pomocí /p:PublishDir =*Directory* \\ možnost, včetně koncové oddělovače zpětné lomítko, jako v následujícím příkladu:

       MSBuild /target:Publish /p:PublishDir=\\myserver\drops\

   Jakmile jste sestavený a otestovat příslušné nástroje MSBuild příkazového řádku k vytvoření vašich projektů a zkombinovat do balíčku s Azure, můžete přidat tento příkazový řádek pro skripty sestavení. Pokud váš server sestavení používá vlastní skripty, tento proces bude záviset na jaké jsou specifikace svůj vlastní sestavovací proces. Pokud používáte jako prostředí pro sestavování sady TFS, můžete podle pokynů v dalším kroku přidejte balíček Azure sestavení do vašeho procesu sestavení.

## <a name="3-build-a-package-using-tfs-team-build"></a>3: vytvoření balíčku pomocí TFS Team Build
Pokud máte nastavený jako řadič sestavení a sestavení serveru Team Foundation Server (TFS) nastavit jako počítač sestavení sady TFS a potom automatizované sestavení Volitelně můžete nastavit pro balíčku Azure. Informace o tom, jak nastavit a používat jako systém sestavení Team Foundation server najdete v tématu [škálování systém sestavení][Scale out your build system]. Zejména následující postup předpokládá, že jste nakonfigurovali vašem serveru sestavení, jak je popsáno v [nasadit a nakonfigurovat server sestavení][Deploy and configure a build server], a že jste vytvořili týmového projektu, vytvoří projekt cloudové služby v týmový projekt.

Pokud chcete konfigurovat TFS sestavovat balíčky, Azure, proveďte následující kroky:

1. V sadě Visual Studio ve svém vývojovém počítači, v nabídce Zobrazit zvolte **Team Explorer**, nebo zvolte Ctrl +\\, Ctrl + M. V okně Team Explorer rozbalte **sestavení** uzlu, nebo zvolte **sestavení** stránce a vyberte **novou definici sestavení**.

   ![Nová možnost definice sestavení][0]
2. Vyberte **aktivační událost** a zadejte požadované podmínek, když chcete, aby balíček, který má být sestaven. Můžete například zadat **průběžnou integraci** dojde k vytvoření balíčku vždy, když zdroj řídí změnami.
3. Vyberte **nastavení zdroje** kartě a zajistěte, aby složky projektu, je uvedena ve **zdrojové složky řízení** sloupce a stav je **Active**.
4. Vyberte **sestavení výchozí** a v části sestavení řadič, ověřte název serveru sestavení.  Také, zvolte možnost **kopie sestavení výstup do následující složky, vyřaďte** a zadejte požadované rozevírací umístění.
5. Vyberte **proces** kartě. Na kartě procesu zvolit výchozí šablonu, v části **sestavení**, zvolte projekt, pokud není vybrána a rozbalte **Upřesnit** tématu **sestavení** části mřížky.
6. Zvolte **argumenty MSBuild**a nastavte příslušné argumenty příkazového řádku nástroje MSBuild, jak je popsáno v kroku 2 výše. Zadejte například **/t: publikování /p:PublishDir =\\\\myserver\\zahodí\\**  k vytvoření balíčku a zkopírujte soubory balíčku do umístění \\ \\myserver\\zahodí\\:

   ![Argumenty MSBuild][2]

   > [!NOTE]
   > Kopírování souborů do veřejného umístění usnadňuje ručně nasadit balíčky z vývojovém počítači.
7. Kontrola v změn do projektu test úspěch kroku sestavení nebo fronty, nové sestavení. Fronty, nové sestavení, v Team Exploreru kliknete pravým tlačítkem na **všechny definice sestavení,** a potom zvolte **fronty nové sestavení**.

## <a name="4-publish-a-package-using-a-powershell-script"></a>4: publikování balíčku pomocí skriptu prostředí PowerShell
Tato část popisuje, jak vytvořit skript prostředí Windows PowerShell, která bude publikovat balíček výstup cloudové aplikace do Azure pomocí volitelné parametry. Tento skript je možné volat po kroku sestavení ve vaší vlastní sestavovací automation. Také může být volána z aktivit pracovního postupu šablonu procesu v sestavení Team TFS s Visual Studio.

1. Nainstalujte [rutin prostředí Azure PowerShell][Azure PowerShell cmdlets] (v0.6.1 nebo vyšší).
   Během fáze instalace rutiny zvolte instalaci jako modul snap-in. Všimněte si, že tato oficiálně podporované verze nahrazuje starší verze nabízeným přes webu CodePlex, i když bylo předchozí verze číslované 2.x.x.
2. Otevřete prostředí Azure PowerShell pomocí nabídky Start nebo úvodní stránka. Pokud spustíte tímto způsobem, budou načteny rutin prostředí Azure PowerShell.
3. Do příkazového řádku Powershellu, ověřte, zda jsou načteny rutin prostředí PowerShell zadáním příkazu částečné `Get-Azure` a stisknutím klávesy tabulátor pro dokončování.

   Pokud opakovaně stiskněte klávesu Tabulátor, měli byste vidět různé příkazy prostředí Azure PowerShell.
4. Ověřte, že se můžete připojit k předplatnému Azure pomocí Import ze souboru .publishsettings informace o vašem předplatném.

   `Import-AzurePublishSettingsFile c:\scripts\WindowsAzure\default.publishsettings`

   Potom zadejte příkaz

   `Get-AzureSubscription`

   Ukazuje to informace o vašem předplatném. Ověřte, že je vše v pořádku.
5. Uložit šablonu skript zadaná na konci tohoto článku do složky skriptů jako c:\\skripty\\WindowsAzure\\**PublishCloudService.ps1**.
6. Projděte si část parametry skriptu. Přidat nebo upravit všechny výchozí hodnoty. Tyto hodnoty mohou být přepsány vždy explicitní parametrů.
7. Zajistí existuje, platný cloudové služby a úložiště účtů v rámci vašeho předplatného, které mohou být zaměřeny skriptem publikovat vytvoření. Účet úložiště (úložiště objektů blob) se použije k nahrání a nasazení balíčku a konfigurační soubor dočasně uložit, je při vytváření nasazení.

   * Pokud chcete vytvořit novou cloudovou službu, můžete volat tento skript nebo použít [portál Azure](https://portal.azure.com). Název cloudové služby se použije jako předponu v platný plně kvalifikovaný název domény, a proto musí být jedinečný.

         New-AzureService -ServiceName "mytestcloudservice" -Location "North Central US" -Label "mytestcloudservice"
   * Pokud chcete vytvořit nový účet úložiště, můžete volat tento skript nebo použít [portál Azure](https://portal.azure.com). Název účtu úložiště se použije jako předponu v platný plně kvalifikovaný název domény, a proto musí být jedinečný. Můžete se pokusit použít stejný název jako cloudová služba.

         New-AzureStorageAccount -ServiceName "mytestcloudservice" -Location "North Central US" -Label "mytestcloudservice"
8. Volání skript přímo z prostředí Azure PowerShell nebo propojit se tento skript k vaší automatizace sestavení hostitele dochází po sestavení balíčku.

   > [!IMPORTANT]
   > Skript bude vždy odstranit nebo nahradit existující nasazení ve výchozím nastavení, pokud jsou zjišťovány. Toto je nutná pro povolení průběžné doručování z automatizace, kde je možné použít bez vyzvání uživatele.
   >
   >

   **Příklad scénáře 1:** průběžné nasazování pro pracovní prostředí služby:

       PowerShell c:\scripts\windowsazure\PublishCloudService.ps1 -environment Staging -serviceName mycloudservice -storageAccountName mystoragesaccount -packageLocation c:\drops\app.publish\ContactManager.Azure.cspkg -cloudConfigLocation c:\drops\app.publish\ServiceConfiguration.Cloud.cscfg -subscriptionDataFile c:\scripts\default.publishsettings

   To je obvykle následuje test spusťte ověření a prohození virtuálních IP adres. Prohození virtuálních IP adres, můžete to udělat pomocí [portál Azure](https://portal.azure.com) nebo pomocí rutiny přesunutí nasazení.

   **Příklad scénáře 2:** průběžné nasazování do produkčního prostředí služby vyhrazené testu

       PowerShell c:\scripts\windowsazure\PublishCloudService.ps1 -environment Production -enableDeploymentUpgrade 1 -serviceName mycloudservice -storageAccountName mystorageaccount -packageLocation c:\drops\app.publish\ContactManager.Azure.cspkg -cloudConfigLocation c:\drops\app.publish\ServiceConfiguration.Cloud.cscfg -subscriptionDataFile c:\scripts\default.publishsettings

   **Vzdálená plocha:**

   Pokud v projektu Azure, budete muset provést další kroky jednorázové zajistit, že je načtený správný certifikát služby Cloud ke všem cloudovým službám, které jsou cílem tento skript je povolené vzdálené plochy.

   Vyhledejte hodnoty kryptografického otisku certifikátu očekávanou role. Kryptografický otisk hodnoty jsou viditelné v části certifikáty do cloudu konfiguračního souboru (tj. ServiceConfiguration.Cloud.cscfg). Je také viditelné v dialogovém okně Konfigurace vzdálené plochy v sadě Visual Studio při zobrazení možností a zobrazení vybraný certifikát.

       <Certificates>
             <Certificate name="Microsoft.WindowsAzure.Plugins.RemoteAccess.PasswordEncryption" thumbprint="C33B6C432C25581601B84C80F86EC2809DC224E8" thumbprintAlgorithm="sha1" />
       </Certificates>

   Nahrání certifikátů vzdálené plochy jako krok jednorázové instalace pomocí následující rutiny skriptu:

       Add-AzureCertificate -serviceName <CLOUDSERVICENAME> -certToDeploy (get-item cert:\CurrentUser\MY\<THUMBPRINT>)

   Například:

       Add-AzureCertificate -serviceName 'mytestcloudservice' -certToDeploy (get-item cert:\CurrentUser\MY\C33B6C432C25581601B84C80F86EC2809DC224E8

   Případně můžete exportovat soubor PFX certifikátu s privátním klíčem a nahrát certifikáty na každý cílový cloud služby pomocí [portál Azure](https://portal.azure.com).

   <!---
   Fixing broken links for Azure content migration from ACOM to DOCS. I'm unable to find a replacement links, so I'm commenting out this reference for now. The author can investigate in the future. "Read the following article to learn more: http://msdn.microsoft.com/library/windowsazure/gg443832.aspx.
   -->
   **Upgrade a nasazení. Odstranění nasazení -\> nové nasazení**

   Skript bude ve výchozím nastavení provádět nasazení upgradu ($enableDeploymentUpgrade = 1) Pokud žádný parametr se předává v nebo hodnota 1, je předaná explicitně. Pro jeden instance to nabízí výhodu v podobě trvá kratší dobu, než úplné nasazení. Pro instance, které vyžadují vysokou dostupnost, to také nabízí výhodu v podobě ponechat některé instancí spuštěných zatímco jiné jsou upgradovány (proti vaší doméně aktualizace), plus vaše virtuální IP adresy se neodstraní.

   Nasazení upgradu lze zakázat ve skriptu ($enableDeploymentUpgrade = 0) nebo pomocí předání *- enableDeploymentUpgrade 0* jako parametr, který mění chování skriptu nejprve odstranit všechna stávající nasazení a pak vytvořte nové nasazení.

   > [!IMPORTANT]
   > Skript bude vždy odstranit nebo nahradit existující nasazení ve výchozím nastavení, pokud jsou zjišťovány. Toto je nutná pro povolení nastavené průběžné doručování ze služby automation, kde je možné použít bez vyzvání uživatele / – operátor.
   >
   >

## <a name="5-publish-a-package-using-tfs-team-build"></a>5: publikování balíčku pomocí TFS Team Build
Tento volitelný krok připojí TFS Team od sestavení k skript vytvořený v kroku 4, která zpracovává publikování sestavení balíčku do Azure. To zahrnuje, úprava šablony procesu, který používá svou definici sestavení tak, aby běžel aktivitu publikovat na konci pracovního postupu. Aktivita publikování provede příkazu prostředí PowerShell předávání v parametry z buildu. Výstup MSBuild cílí a publikovat skriptu se přesměruje do výstupu standardní sestavení.

1. Upravit definici sestavení zodpovědná za průběžné nasazování.
2. Vyberte **proces** kartě.
3. Postupujte podle [tyto pokyny](http://msdn.microsoft.com/library/dd647551.aspx) Přidání aktivity projektu pro šablonu procesu sestavení, stáhnout výchozí šablonu, přidejte do projektu a vrácení se změnami. Zadejte nový název, jako je například AzureBuildProcessTemplate šablony procesu sestavení.
4. Vraťte se do **proces** kartě a používat **zobrazit podrobnosti** zobrazíte seznam šablony procesu sestavení k dispozici. Vyberte **nové...**  tlačítko a přejděte do projektu, stačí přidat a vrátit se změnami. Vyhledejte šablonu, kterou jste právě vytvořili a zvolte **OK**.
5. Otevřete vybrané šablony procesu pro úpravy. Můžete otevřít přímo v Návrháři pracovních postupů, nebo v editoru XML pro práci s XAML.
6. Následující seznam argumentů, nové přidáte jako samostatný řádek položky na kartě argumenty v Návrháři pracovních postupů. Všechny argumenty by měl mít směr = v a typ = řetězec. Tyto se použije tok parametry z definice sestavení do pracovního postupu, které pak získat používá k volání skriptu publikovat.

       SubscriptionName
       StorageAccountName
       CloudConfigLocation
       PackageLocation
       Environment
       SubscriptionDataFileLocation
       PublishScriptLocation
       ServiceName

   ![Seznam argumentů][3]

   Odpovídající XAML vypadá takto:

       <Activity  _ />
         <x:Members>
           <x:Property Name="BuildSettings" Type="InArgument(mtbwa:BuildSettings)" />
           <x:Property Name="TestSpecs" Type="InArgument(mtbwa:TestSpecList)" />
           <x:Property Name="BuildNumberFormat" Type="InArgument(x:String)" />
           <x:Property Name="CleanWorkspace" Type="InArgument(mtbwa:CleanWorkspaceOption)" />
           <x:Property Name="RunCodeAnalysis" Type="InArgument(mtbwa:CodeAnalysisOption)" />
           <x:Property Name="SourceAndSymbolServerSettings" Type="InArgument(mtbwa:SourceAndSymbolServerSettings)" />
           <x:Property Name="AgentSettings" Type="InArgument(mtbwa:AgentSettings)" />
           <x:Property Name="AssociateChangesetsAndWorkItems" Type="InArgument(x:Boolean)" />
           <x:Property Name="CreateWorkItem" Type="InArgument(x:Boolean)" />
           <x:Property Name="DropBuild" Type="InArgument(x:Boolean)" />
           <x:Property Name="MSBuildArguments" Type="InArgument(x:String)" />
           <x:Property Name="MSBuildPlatform" Type="InArgument(mtbwa:ToolPlatform)" />
           <x:Property Name="PerformTestImpactAnalysis" Type="InArgument(x:Boolean)" />
           <x:Property Name="CreateLabel" Type="InArgument(x:Boolean)" />
           <x:Property Name="DisableTests" Type="InArgument(x:Boolean)" />
           <x:Property Name="GetVersion" Type="InArgument(x:String)" />
           <x:Property Name="PrivateDropLocation" Type="InArgument(x:String)" />
           <x:Property Name="Verbosity" Type="InArgument(mtbw:BuildVerbosity)" />
           <x:Property Name="Metadata" Type="mtbw:ProcessParameterMetadataCollection" />
           <x:Property Name="SupportedReasons" Type="mtbc:BuildReason" />
           <x:Property Name="SubscriptionName" Type="InArgument(x:String)" />
           <x:Property Name="StorageAccountName" Type="InArgument(x:String)" />
           <x:Property Name="CloudConfigLocation" Type="InArgument(x:String)" />
           <x:Property Name="PackageLocation" Type="InArgument(x:String)" />
           <x:Property Name="Environment" Type="InArgument(x:String)" />
           <x:Property Name="SubscriptionDataFileLocation" Type="InArgument(x:String)" />
           <x:Property Name="PublishScriptLocation" Type="InArgument(x:String)" />
           <x:Property Name="ServiceName" Type="InArgument(x:String)" />
         </x:Members>

         <this:Process.MSBuildArguments>
7. Přidejte nové pořadí na konci spustit na agenta:

   1. Začněte přidáním aktivitu Pokud příkaz zkontrolujte soubor platný skriptu. Podmínka nastavená na tuto hodnotu:

          Not String.IsNullOrEmpty(PublishScriptLocation)
   2. Pak případ Pokud příkaz Přidat novou aktivitu pořadí. Nastavit počáteční publikovat zobrazovaný název
   3. S spuštění publikování pořadí při výběru, přidejte následující seznam nové proměnné jako samostatné řádku položky v kartě proměnné v Návrháři pracovních postupů. Všechny proměnné, měl by být proměnné typ = řetězec a obor = počáteční publikovat. Tyto se použije tok parametry z definice sestavení do pracovního postupu, které pak získat používá k volání skriptu publikovat.

      * SubscriptionDataFilePath typu řetězec.
      * PublishScriptFilePath typu řetězec.

        ![Nové proměnné][4]
   4. Pokud používáte sady TFS 2012 nebo starším, přidejte aktivitu ConvertWorkspaceItem na začátku nového pořadí. Pokud používáte sady TFS 2013 nebo novější, přidejte aktivitu GetLocalPath na začátku nového pořadí. Pro ConvertWorkspaceItem, nastavte vlastnosti takto: směr = ServerToLocal, DisplayName = název souboru skriptu publikovat převést, vstup = PublishScriptLocation, výsledek = PublishScriptFilePath, pracovní prostor = 'Prostoru'. Pro aktivitu GetLocalPath nastavte vlastnost IncomingPath na 'PublishScriptLocation' a výsledek, který má 'PublishScriptFilePath'. Tato aktivita převede cestu do skriptu publikování z TFS umístění serveru (Pokud je k dispozici) na místní disk standardní cestu.
   5. Pokud používáte sady TFS 2012 nebo starším, přidejte další aktivitu ConvertWorkspaceItem na konci tohoto nového pořadí. Směr ServerToLocal, DisplayName = = převést předplatné filename, vstup = SubscriptionDataFileLocation, výsledek = SubscriptionDataFilePath, pracovní prostor = 'Prostoru'. Pokud používáte sady TFS 2013 nebo novější, přidejte další GetLocalPath. IncomingPath = 'SubscriptionDataFileLocation' a výsledek = "SubscriptionDataFilePath."
   6. Přidejte aktivitu, InvokeProcess na konci tohoto nového pořadí.
      Tato aktivita volá PowerShell.exe s argumenty předaná v definici sestavení.

      + Argumenty = String.Format ("-""{0}" "- serviceName {1} - storageAccountName {2} - packageLocation""{3}" "– cloudConfigLocation""{4}" "– subscriptionDataFile""{5}" "- selectedSubscription {6} souboru-prostředí""{7}" "", PublishScriptFilePath, ServiceName, StorageAccountName, PackageLocation, CloudConfigLocation, SubscriptionDataFilePath, Název_předplatného, prostředí)
      + DisplayName = Execute publikování skriptu
      + Název souboru = "PowerShell" (použít uvozovky)
      + OutputEncoding = System.Text.Encoding.GetEncoding(System.Globalization.CultureInfo.InstalledUICulture.TextInfo.OEMCodePage)
   7. V **zpracování standardní výstup** část textového InvokeProcess, nastavte hodnotu textové pole na "data". Toto je proměnnou pro uložení standardní výstupní data.
   8. Přidat aktivitu WriteBuildMessage právě níže **zpracování standardní výstup** části. Nastavit význam = 'Microsoft.TeamFoundation.Build.Client.BuildMessageImportance.High' a zpráva = "data". Tím se zajistí ve standardním výstupu skriptu získat zapíšou do výstupu sestavení.
   9. V **zpracovávat chyby výstupu** část textového InvokeProcess, nastavte hodnotu textové pole na "data". Toto je proměnnou pro uložení dat standardní chyba.
   10. Přidat aktivitu WriteBuildError právě níže **zpracovávat chyby výstupu** části. Nastavit zprávu = "data". Tím se zajistí, že standard chyby skriptu získat zapíšou do výstupu chyby sestavení.
   11. Opravte všechny chyby, indikován blue vykřičníku značky. Najeďte myší značky vykřičníku získat nápovědu o této chybě. Uložení pracovního postupu vymazat chyby.

   Konečný výsledek aktivit pracovního postupu publikovat bude v Návrháři vypadat například takto:

   ![Aktivity pracovního postupu][5]

   Konečný výsledek aktivit pracovního postupu publikovat bude vypadat takto v jazyce XAML:

       <If Condition="[Not String.IsNullOrEmpty(PublishScriptLocation)]" sap2010:WorkflowViewState.IdRef="If_1">
           <If.Then>
             <Sequence DisplayName="Start Publish" sap2010:WorkflowViewState.IdRef="Sequence_4">
               <Sequence.Variables>
                 <Variable x:TypeArguments="x:String" Name="SubscriptionDataFilePath" />
                 <Variable x:TypeArguments="x:String" Name="PublishScriptFilePath" />
               </Sequence.Variables>
               <mtbwa:ConvertWorkspaceItem DisplayName="Convert publish script filename" sap2010:WorkflowViewState.IdRef="ConvertWorkspaceItem_1" Input="[PublishScriptLocation]" Result="[PublishScriptFilePath]" Workspace="[Workspace]" />
               <mtbwa:ConvertWorkspaceItem DisplayName="Convert subscription filename" sap2010:WorkflowViewState.IdRef="ConvertWorkspaceItem_2" Input="[SubscriptionDataFileLocation]" Result="[SubscriptionDataFilePath]" Workspace="[Workspace]" />
               <mtbwa:InvokeProcess Arguments="[String.Format(&quot; -File &quot;&quot;{0}&quot;&quot; -serviceName {1}&#xD;&#xA;            -storageAccountName {2} -packageLocation &quot;&quot;{3}&quot;&quot;&#xD;&#xA;            -cloudConfigLocation &quot;&quot;{4}&quot;&quot; -subscriptionDataFile &quot;&quot;{5}&quot;&quot;&#xD;&#xA;            -selectedSubscription {6} -environment &quot;&quot;{7}&quot;&quot;&quot;,&#xD;&#xA;            PublishScriptFilePath, ServiceName, StorageAccountName,&#xD;&#xA;            PackageLocation, CloudConfigLocation,&#xD;&#xA;            SubscriptionDataFilePath, SubscriptionName, Environment)]" DisplayName="'Execute Publish Script'" FileName="[PowerShell]" sap2010:WorkflowViewState.IdRef="InvokeProcess_1">
                 <mtbwa:InvokeProcess.ErrorDataReceived>
                   <ActivityAction x:TypeArguments="x:String">
                     <ActivityAction.Argument>
                       <DelegateInArgument x:TypeArguments="x:String" Name="data" />
                     </ActivityAction.Argument>
                     <mtbwa:WriteBuildError Message="{x:Null}" sap2010:WorkflowViewState.IdRef="WriteBuildError_1" />
                   </ActivityAction>
                 </mtbwa:InvokeProcess.ErrorDataReceived>
                 <mtbwa:InvokeProcess.OutputDataReceived>
                   <ActivityAction x:TypeArguments="x:String">
                     <ActivityAction.Argument>
                       <DelegateInArgument x:TypeArguments="x:String" Name="data" />
                     </ActivityAction.Argument>
                     <mtbwa:WriteBuildMessage sap2010:WorkflowViewState.IdRef="WriteBuildMessage_2" Importance="[Microsoft.TeamFoundation.Build.Client.BuildMessageImportance.High]" Message="[data]" mva:VisualBasic.Settings="Assembly references and imported namespaces serialized as XML namespaces" />
                   </ActivityAction>
                 </mtbwa:InvokeProcess.OutputDataReceived>
               </mtbwa:InvokeProcess>
             </Sequence>
           </If.Then>
         </If>
       </Sequence>
8. Uložte tento soubor pracovní postup šablony procesu sestavení a vrátit se změnami.
9. Upravit definici sestavení (zavřete ho Pokud už je otevřený) a vyberte **nový** tlačítko, pokud ještě nevidíte nové šablony v seznamu šablon procesů.
10. Nastavte parametr hodnoty vlastností v části různé takto:

    1. CloudConfigLocation = "c:\\zahodí\\app.publish\\ServiceConfiguration.Cloud.cscfg' *tato hodnota se odvozuje od: ($PublishDir)ServiceConfiguration.Cloud.cscfg*
    2. PackageLocation = "c:\\zahodí\\app.publish\\ContactManager.Azure.cspkg' *tato hodnota se odvozuje od: ($PublishDir)($ProjectName) .cspkg*
    3. PublishScriptLocation = "c:\\skripty\\WindowsAzure\\PublishCloudService.ps1.
    4. ServiceName = 'mycloudservicename' *zde použít název příslušné cloudové služby*
    5. Prostředí = "pracovní.
    6. StorageAccountName = 'mystorageaccountname' *zde použít název účtu příslušné úložiště*
    7. SubscriptionDataFileLocation = "c:\\skripty\\WindowsAzure\\Subscription.xml.
    8. Název_předplatného = "default"

    ![Vlastnost hodnoty parametru][6]
11. Uložte změny do definice sestavení.
12. Fronta sestavení ke spouštění balíčku sestavení a publikování. Pokud je nastavena na průběžnou integraci aktivační událost, provede toto chování na každý vrácení se změnami.

### <a name="publishcloudserviceps1-script-template"></a>Šablona PublishCloudService.ps1 skriptu
```powershell
Param(  $serviceName = "",
        $storageAccountName = "",
        $packageLocation = "",
        $cloudConfigLocation = "",
        $environment = "Staging",
        $deploymentLabel = "ContinuousDeploy to $servicename",
        $timeStampFormat = "g",
        $alwaysDeleteExistingDeployments = 1,
        $enableDeploymentUpgrade = 1,
        $selectedsubscription = "default",
        $subscriptionDataFile = ""
     )


function Publish()
{
    $deployment = Get-AzureDeployment -ServiceName $serviceName -Slot $slot -ErrorVariable a -ErrorAction silentlycontinue
    if ($a[0] -ne $null)
    {
        Write-Output "$(Get-Date -f $timeStampFormat) - No deployment is detected. Creating a new deployment. "
    }
    #check for existing deployment and then either upgrade, delete + deploy, or cancel according to $alwaysDeleteExistingDeployments and $enableDeploymentUpgrade boolean variables
    if ($deployment.Name -ne $null)
    {
        switch ($alwaysDeleteExistingDeployments)
        {
            1
            {
                switch ($enableDeploymentUpgrade)
                {
                    1  #Update deployment inplace (usually faster, cheaper, won't destroy VIP)
                    {
                        Write-Output "$(Get-Date -f $timeStampFormat) - Deployment exists in $servicename.  Upgrading deployment."
                        UpgradeDeployment
                    }
                    0  #Delete then create new deployment
                    {
                        Write-Output "$(Get-Date -f $timeStampFormat) - Deployment exists in $servicename.  Deleting deployment."
                        DeleteDeployment
                        CreateNewDeployment

                    }
                } # switch ($enableDeploymentUpgrade)
            }
            0
            {
                Write-Output "$(Get-Date -f $timeStampFormat) - ERROR: Deployment exists in $servicename.  Script execution cancelled."
                exit
            }
        } #switch ($alwaysDeleteExistingDeployments)
    } else {
            CreateNewDeployment
    }
}

function CreateNewDeployment()
{
    write-progress -id 3 -activity "Creating New Deployment" -Status "In progress"
    Write-Output "$(Get-Date -f $timeStampFormat) - Creating New Deployment: In progress"

    $opstat = New-AzureDeployment -Slot $slot -Package $packageLocation -Configuration $cloudConfigLocation -label $deploymentLabel -ServiceName $serviceName

    $completeDeployment = Get-AzureDeployment -ServiceName $serviceName -Slot $slot
    $completeDeploymentID = $completeDeployment.deploymentid

    write-progress -id 3 -activity "Creating New Deployment" -completed -Status "Complete"
    Write-Output "$(Get-Date -f $timeStampFormat) - Creating New Deployment: Complete, Deployment ID: $completeDeploymentID"

    StartInstances
}

function UpgradeDeployment()
{
    write-progress -id 3 -activity "Upgrading Deployment" -Status "In progress"
    Write-Output "$(Get-Date -f $timeStampFormat) - Upgrading Deployment: In progress"

    # perform Update-Deployment
    $setdeployment = Set-AzureDeployment -Upgrade -Slot $slot -Package $packageLocation -Configuration $cloudConfigLocation -label $deploymentLabel -ServiceName $serviceName -Force

    $completeDeployment = Get-AzureDeployment -ServiceName $serviceName -Slot $slot
    $completeDeploymentID = $completeDeployment.deploymentid

    write-progress -id 3 -activity "Upgrading Deployment" -completed -Status "Complete"
    Write-Output "$(Get-Date -f $timeStampFormat) - Upgrading Deployment: Complete, Deployment ID: $completeDeploymentID"
}

function DeleteDeployment()
{

    write-progress -id 2 -activity "Deleting Deployment" -Status "In progress"
    Write-Output "$(Get-Date -f $timeStampFormat) - Deleting Deployment: In progress"

    #WARNING - always deletes with force
    $removeDeployment = Remove-AzureDeployment -Slot $slot -ServiceName $serviceName -Force

    write-progress -id 2 -activity "Deleting Deployment: Complete" -completed -Status $removeDeployment
    Write-Output "$(Get-Date -f $timeStampFormat) - Deleting Deployment: Complete"

}

function StartInstances()
{
    write-progress -id 4 -activity "Starting Instances" -status "In progress"
    Write-Output "$(Get-Date -f $timeStampFormat) - Starting Instances: In progress"

    $deployment = Get-AzureDeployment -ServiceName $serviceName -Slot $slot
    $runstatus = $deployment.Status

    if ($runstatus -ne 'Running')
    {
        $run = Set-AzureDeployment -Slot $slot -ServiceName $serviceName -Status Running
    }
    $deployment = Get-AzureDeployment -ServiceName $serviceName -Slot $slot
    $oldStatusStr = @("") * $deployment.RoleInstanceList.Count

    while (-not(AllInstancesRunning($deployment.RoleInstanceList)))
    {
        $i = 1
        foreach ($roleInstance in $deployment.RoleInstanceList)
        {
            $instanceName = $roleInstance.InstanceName
            $instanceStatus = $roleInstance.InstanceStatus

            if ($oldStatusStr[$i - 1] -ne $roleInstance.InstanceStatus)
            {
                $oldStatusStr[$i - 1] = $roleInstance.InstanceStatus
                Write-Output "$(Get-Date -f $timeStampFormat) - Starting Instance '$instanceName': $instanceStatus"
            }

            write-progress -id (4 + $i) -activity "Starting Instance '$instanceName'" -status "$instanceStatus"
            $i = $i + 1
        }

        sleep -Seconds 1

        $deployment = Get-AzureDeployment -ServiceName $serviceName -Slot $slot
    }

    $i = 1
    foreach ($roleInstance in $deployment.RoleInstanceList)
    {
        $instanceName = $roleInstance.InstanceName
        $instanceStatus = $roleInstance.InstanceStatus

        if ($oldStatusStr[$i - 1] -ne $roleInstance.InstanceStatus)
        {
            $oldStatusStr[$i - 1] = $roleInstance.InstanceStatus
            Write-Output "$(Get-Date -f $timeStampFormat) - Starting Instance '$instanceName': $instanceStatus"
        }

        $i = $i + 1
    }

    $deployment = Get-AzureDeployment -ServiceName $serviceName -Slot $slot
    $opstat = $deployment.Status

    write-progress -id 4 -activity "Starting Instances" -completed -status $opstat
    Write-Output "$(Get-Date -f $timeStampFormat) - Starting Instances: $opstat"
}

function AllInstancesRunning($roleInstanceList)
{
    foreach ($roleInstance in $roleInstanceList)
    {
        if ($roleInstance.InstanceStatus -ne "ReadyRole")
        {
            return $false
        }
    }

    return $true
}

#configure powershell with Azure 1.7 modules
Import-Module Azure

#configure powershell with publishsettings for your subscription
$pubsettings = $subscriptionDataFile
Import-AzurePublishSettingsFile $pubsettings
Set-AzureSubscription -CurrentStorageAccountName $storageAccountName -SubscriptionName $selectedsubscription
Select-AzureSubscription $selectedsubscription

#set remaining environment variables for Azure cmdlets
$subscription = Get-AzureSubscription $selectedsubscription
$subscriptionname = $subscription.subscriptionname
$subscriptionid = $subscription.subscriptionid
$slot = $environment

#main driver - publish & write progress to activity log
Write-Output "$(Get-Date -f $timeStampFormat) - Azure Cloud Service deploy script started."
Write-Output "$(Get-Date -f $timeStampFormat) - Preparing deployment of $deploymentLabel for $subscriptionname with Subscription ID $subscriptionid."

Publish

$deployment = Get-AzureDeployment -slot $slot -serviceName $servicename
$deploymentUrl = $deployment.Url

Write-Output "$(Get-Date -f $timeStampFormat) - Created Cloud Service with URL $deploymentUrl."
Write-Output "$(Get-Date -f $timeStampFormat) - Azure Cloud Service deploy script finished."
```

## <a name="next-steps"></a>Další kroky
Chcete-li povolit vzdálené ladění při použití nastavené průběžné doručování, přečtěte si téma [povolení vzdáleného ladění při publikování v Azure pomocí nastavené průběžné doručování](cloud-services-virtual-machines-dotnet-continuous-delivery-remote-debugging.md).

[Team Foundation Build Service]: https://msdn.microsoft.com/library/ee259687.aspx
[.NET Framework 4]: https://www.microsoft.com/download/details.aspx?id=17851
[.NET Framework 4.5]: https://www.microsoft.com/download/details.aspx?id=30653
[.NET Framework 4.5.2]: https://www.microsoft.com/download/details.aspx?id=42643
[Scale out your build system]: https://msdn.microsoft.com/library/dd793166.aspx
[Deploy and configure a build server]: https://msdn.microsoft.com/library/ms181712.aspx
[Azure PowerShell cmdlets]: /powershell/azureps-cmdlets-docs
[0]: ./media/cloud-services-dotnet-continuous-delivery/tfs-01bc.png
[2]: ./media/cloud-services-dotnet-continuous-delivery/tfs-02.png
[3]: ./media/cloud-services-dotnet-continuous-delivery/common-task-tfs-03.png
[4]: ./media/cloud-services-dotnet-continuous-delivery/common-task-tfs-04.png
[5]: ./media/cloud-services-dotnet-continuous-delivery/common-task-tfs-05.png
[6]: ./media/cloud-services-dotnet-continuous-delivery/common-task-tfs-06.png
