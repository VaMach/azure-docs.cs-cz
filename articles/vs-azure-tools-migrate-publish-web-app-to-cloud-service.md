---
title: "Jak migrovat a publikovat webovou aplikaci pro cloudové služby Azure ze sady Visual Studio | Microsoft Docs"
description: "Zjistěte, jak migrovat a publikovat webovou aplikaci na cloudové služby Azure pomocí sady Visual Studio."
services: visual-studio-online
documentationcenter: na
author: kraigb
manager: ghogen
editor: 
ms.assetid: 9394adfd-a645-4664-9354-dd5df08e8c91
ms.service: multiple
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: multiple
ms.date: 11/11/2016
ms.author: kraigb
ms.openlocfilehash: d5de4f5a7357cf5adde7773867356d47ad447bab
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/11/2017
---
# <a name="how-to-migrate-and-publish-a-web-application-to-an-azure-cloud-service-from-visual-studio"></a>Postupy: migrace a publikovat webovou aplikaci pro cloudové služby Azure ze sady Visual Studio
Pokud chcete využít výhod hostitelských služeb a škálovatelnost Azure, budete chtít migrovat a publikovat webovou aplikaci na cloudové služby Azure. Webovou aplikaci můžete spustit v Azure s minimálními změnami do vaší stávající aplikace.

> [!NOTE]
> Toto téma se věnuje nasazení do cloudové služby, není pro webové servery. Informace o nasazení do webové stránky najdete v tématu [nasazení webové aplikace v Azure App Service](app-service/app-service-deploy-local-git.md).
>
>

Seznam konkrétních šablon, které jsou podporovány pro obě Visual C# a Visual Basic, najdete v části **podporované šablony projektů** dál v tomto tématu.

Nejprve je nutné povolit webové aplikace pro Azure ze sady Visual Studio. Následující obrázek znázorňuje klíče postup, jak publikovat aplikace pro web přidáním projektu Azure má použít pro nasazení. Tento proces projektu Azure s požadované webovou roli přidá do vašeho řešení. Na základě typu webového projektu, který máte, vlastnosti projektu pro sestavení jsou také aktualizovat Pokud balíček vyžaduje další sestavení pro nasazení.

![Publikování webové aplikace do služby Microsoft Azure](./media/vs-azure-tools-migrate-publish-web-app-to-cloud-service/IC748917.png)

> [!NOTE]
> **Převést**, **převést na projekt cloudové služby Azure** příkazu se zobrazí pouze pro webový projekt ve vašem řešení. Například příkaz není k dispozici pro projekt Silverlight ve vašem řešení.
> Když vytvoříte balíček služby nebo publikování aplikace do Azure, může dojít k upozornění nebo chyby. Tyto chyby a varování můžete opravit problémy, před nasazením do Azure. Například může zobrazit upozornění na chybějící sestavení. Další informace o tom, jak považovat všechna upozornění, chyby najdete v tématu [konfigurace projektu Azure Cloud Service pomocí sady Visual Studio](vs-azure-tools-configuring-an-azure-project.md). Pokud sestavit aplikaci, spusťte ho místně pomocí emulátoru služby výpočty v nebo jej publikujte do Azure, může se zobrazit následující chyby v **seznam chyb** okno: **zadaná cesta a název souboru jsou příliš dlouhé**. K této chybě dojde, protože délka názvu plně kvalifikovaný projektu Azure je příliš dlouhý. Délka názvu projektu, včetně úplná cesta, nemůže být více než 146 znaků. To je třeba název úplné projektu včetně cesta k souboru pro Azure projekt, který je vytvořen pro aplikaci Silverlight: `c:\users\<user name>\documents\visual studio 2015\Projects\SilverlightApplication4\SilverlightApplication4.Web.Azure.ccproj`. Bude pravděpodobně nutné přesunout do jiného adresáře s kratší cestou k Zkraťte název plně kvalifikovaný projektu řešení.
>
>

K migraci a publikování webové aplikace do Azure ze sady Visual Studio, postupujte podle těchto kroků.

## <a name="enable-a-web-application-for-deployment-to-azure"></a>Povolit webovou aplikaci pro nasazení do Azure
### <a name="to-enable-a-web-application-for-deployment-to-azure"></a>Chcete-li povolit webovou aplikaci pro nasazení do Azure
1. Pokud chcete povolit webové aplikace pro nasazení do Azure, otevřete místní nabídku pro webový projekt ve vašem řešení a zvolte Přidat projekt nasazení Azure.

    Provedou se tyto akce:

   * Volá se projektu Azure `<name of the web project>.Azure` je přidán do řešení pro vaši aplikaci.
   * Webové role pro webový projekt se přidá k tomuto projektu Azure.
   * **Místní kopie** je nastavena na hodnotu true pro všechny sestavení, které jsou požadovány pro MVC MVC 2, MVC 3, 4 a Silverlight obchodních aplikací. Tento postup přidá tyto sestavení do balíčku služby, který se používá pro nasazení.

   > [!IMPORTANT]
   > Pokud máte další sestavení nebo soubory, které jsou požadovány pro tuto webovou aplikaci, je nutné ručně nastavit vlastnosti u těchto souborů. Informace o tom, jak nastavit tyto vlastnosti, najdete v části **souborů v balíčku služby** dále v tomto článku.
   >
   > [!NOTE]
   > Pokud pro konkrétní webový projekt webové role již existuje v projektu Azure v řešení, **převést**, **převést na projekt cloudové služby Azure** se nezobrazí v místní nabídce pro tento webový projekt.
   >
   >

   Pokud máte více webových projektů ve webové aplikaci a chcete vytvořit webové role pro každý webový projekt, musíte provést kroky v tomto postupu pro každý webový projekt. Tím se vytvoří samostatný Azure projekty pro každou webovou roli. Každý webový projekt můžete publikovat samostatně. Alternativně můžete ručně přidáte jiné webové role do existujícího projektu Azure ve webové aplikaci. To pokud chcete udělat, otevřete místní nabídku pro **role** v projektu Azure vyberte **přidat**, pak **projekt webové Role v řešení**, zvolte projekt, který má přidat jako webovou roli a Zvolte **OK** tlačítko.

## <a name="use-an-azure-sql-database-for-your-application"></a>Použít databázi Azure SQL pro vaši aplikaci
Pokud máte připojovací řetězec pro svoji webovou aplikaci, která používá databázi systému SQL Server, který je místní, musíte změnit tento připojovací řetězec k použití instance databáze SQL, který je hostitelem Azure místo.

> [!IMPORTANT]
> Vaše předplatné musí umožňují použít SQL Database. Pokud máte přístup k předplatnému z [portál Azure classic](http://go.microsoft.com/fwlink/?LinkID=213885), můžete určit, jaké služby poskytuje vašeho předplatného. Tyto pokyny platí pro vydaná [portál Azure classic](http://go.microsoft.com/fwlink/?LinkID=213885). Pokud používáte [portál Azure](http://portal.microsoft.com), přejděte k dalšímu postupu.
>
>

### <a name="to-use-a-sql-database-instance-in-your-web-role-for-your-connection-string"></a>Použít instanci SQL databáze vaší webové role pro připojovací řetězec
1. K vytvoření instance databáze SQL v [portál Azure classic](http://go.microsoft.com/fwlink/?LinkID=213885), postupujte podle kroků v následujícím článku: [vytvořit databázi serveru SQL](http://go.microsoft.com/fwlink/?LinkId=225109).

   > [!NOTE]
   > Při nastavování pravidel brány firewall pro instanci SQL databáze, je nutné vybrat **povolit jiným službám Azure přístup k tomuto serveru** zaškrtávací políčko.
   >
   >
2. Pokud chcete vytvořit instanci databáze SQL pro použití pro připojovací řetězec, postupujte podle kroků v další části v následujícím článku: [vytvoření databáze SQL](http://go.microsoft.com/fwlink/?LinkId=225110).
3. Pokud chcete zkopírovat připojovací řetězec ADO.NET k použití pro připojovací řetězec, proveďte následující kroky v [portál Azure classic](http://go.microsoft.com/fwlink/?LinkID=213885).  

   1. Vyberte **databáze** tlačítko a poté rozbalte uzel pro předplatné, které jste použili k vytvoření instanci databáze SQL.
   2. Chcete-li zobrazit dostupné instance SQL Database, zvolte **databází SQL** uzlu.
   3. Chcete-li zobrazit vlastnosti pro databázi, zvolte databázi. **Vlastnosti** zobrazení se zobrazí.

      > [!NOTE]
      > Pokud **vlastnosti** zobrazení se nezobrazí, možná budete muset otevřít pomocí dělicí.
      >
      >
   4. Chcete-li zobrazit připojovací řetězce, zvolte tlačítko se třemi tečkami (...) vedle položky zobrazení.

      **Připojovací řetězce** zobrazí se dialogové okno.
   5. Zkopírujte připojovací řetězec ADO.NET, označte text a vyberte klávesy Ctrl + C.
   6. Chcete-li zavřít dialogové okno, zvolte **zavřete** tlačítko.
4. Chcete-li nahradit připojovací řetězec v souboru web.config pro použití této instance systému SQL Database, otevřete soubor web.config, zvýrazněte existující připojovací řetězec a poté zvolte klávesy Ctrl + V. Připojovací řetězec ADO.NET pro instanci SQL databáze nahradí existující připojovací řetězec.
5. Musíte taky přidat parametr `MultipleActiveResultSets=True` připojovací řetězec. Připojovací řetězec musí mít následující formát:

    ```
    connectionString=”Server=tcp:<database_server>.database.windows.net,1433;Database=<database_name>;User ID=<user_name>@<database_server>;Password=<myPassword>;Trusted_Connection=False;Encrypt=True;MultipleActiveResultSets=True"
    ```
6. (Volitelné) Alternativní metodu změna připojovací řetězec přímo v souboru web.config je přidat oddíl do jednoho ze souborů web.config transformace, v závislosti na konfiguraci sestavení, který použijete k vytvoření vašeho balíčku služby. Otevřete soubor Web.Debug.Config nebo soubor Web. Do tohoto souboru přidejte následující části:

    ```
    XMLCopy<connectionStrings><addname="DefaultConnection"connectionString="Server=tcp:<database_server>.database.windows.net,1433;Database=<database_name>;User ID=<user_name>@<database_server>;Password=<myPassword>;Trusted_Connection=False;Encrypt=True;MultipleActiveResultSets=True"xdt:Transform="SetAttributes"xdt:Locator="Match(name)"/></connectionStrings>
    ```
7. Uložte soubor, který můžete upravit a znovu publikovat aplikace.

### <a name="to-use-an-instance-of-sql-database-by-using-the-azure-classic-portal"></a>Použití instance databáze SQL pomocí portálu Azure classic
1. V [portál Azure classic](http://go.microsoft.com/fwlink/?LinkID=213885), vyberte uzel databáze SQL.

   * Pokud se zobrazí instance systému SQL Database, která chcete použít, vyberte ho otevřete.
   * Pokud jste nevytvořili žádné instance, vyberte příslušný odkaz a pak vytvořit instanci.
2. Po otevření nebo vytvořit instanci databáze, klikněte **připojovací řetězce** odkaz.
3. V dolní části stránky vyberte odkaz nakonfigurovat nastavení brány firewall a přijměte výchozí hodnoty nebo nakonfigurovat hodnoty, které potřebujete.
4. Zkopírujte připojovací řetězec ADO.NET, vložte jej do souboru web.config přes staré připojovací řetězec pro místní databázi a nezapomeňte přidat `MultipleActiveResultSets=True`.

## <a name="publish-a-web-application-to-azure"></a>Publikování webové aplikace do Azure
### <a name="to-publish-a-web-application-to-azure"></a>Chcete-li publikovat webovou aplikaci do Azure
1. K testování aplikace pro místní vývoj prostředí pomocí Azure emulátor služby výpočty, otevřete místní nabídky projektu Azure pro webovou roli a vyberte **nastavit jako spouštěný projekt**. Zvolte **ladění**, **spustit ladění** (klávesové: **F5**).

    **Spuštění prostředí Azure ladění** otevře se dialogové okno a spuštění aplikace v prohlížeči. Pro konkrétní podrobnosti o tom, jak spustit každý typ webovou aplikaci v emulátoru služby výpočty v najdete v tabulce v této části.
2. K nastavení služby pro vaše aplikace a publikování v Azure, musíte mít účet Microsoft a předplatné Azure. Nastavit vašim službám pomocí kroků v následujícím tématu: [Příprava na publikování nebo nasadit aplikaci Azure ze sady Visual Studio](vs-azure-tools-cloud-service-publish-set-up-required-services-in-visual-studio.md).
3. Chcete-li publikovat webovou aplikaci do Azure, otevřete místní nabídku pro webový projekt a zvolte **publikovat do Azure**.

    **Publikování aplikaci Azure** otevře se dialogové okno a Visual Studio spustí proces nasazení. Další informace o tom, jak publikovat aplikaci, najdete v části **publikovat aplikaci Azure ze sady Visual Studio** v [publikování cloudové služby pomocí nástroje Azure](vs-azure-tools-publishing-a-cloud-service.md).

   > [!NOTE]
   > Můžete také publikovat webovou aplikaci z Azure projektu. K tomuto účelu otevřete místní nabídky pro Azure projekt a zvolte **publikovat**.
   >
   >
4. Chcete-li sledovat průběh nasazení, můžete se podívat **protokol činnosti Azure** okno. Tento protokol se automaticky zobrazí při spuštění procesu nasazení. Můžete rozbalit řádku položky v protokolu aktivity, který chcete zobrazit podrobné informace, jak je znázorněno na následujícím obrázku:

    ![VST_AzureActivityLog](./media/vs-azure-tools-migrate-publish-web-app-to-cloud-service/IC744149.png)
5. (Volitelné) Pokud chcete zrušit proces nasazení, otevřete místní nabídku pro položku řádku v protokolu aktivit a zvolte **zrušit a odeberte**. To zastaví proces nasazení a odstraní prostředí nasazení z Azure.

   > [!NOTE]
   > Chcete-li odebrat toto nasazení prostředí poté, co byla nasazena, je nutné použít [portál Azure classic](http://go.microsoft.com/fwlink/?LinkID=213885).
   >
   >
6. (Volitelné) Po spuštění instance role mít Visual Studio automaticky zobrazí prostředí nasazení v **Azure Compute** uzlu v **Průzkumník cloudu** nebo **Průzkumníka serveru**. Odtud můžete zobrazit stav instance jednotlivých rolí.

    Následující obrázek znázorňuje instancí rolí ve **Průzkumníka serveru** době, kdy jsou stále ve stavu Probíhá inicializace:

    ![VST_DeployComputeNode](./media/vs-azure-tools-migrate-publish-web-app-to-cloud-service/IC744134.png)
7. Pro přístup k aplikaci po nasazení, zvolte na šipku vedle vašeho nasazení, když stav **dokončeno** se zobrazí v **protokol činnosti Azure**. Zobrazí adresu URL pro webovou aplikaci v Azure. Najdete v následující tabulce najdete podrobnosti o tom, jak spustit určitý typ webové aplikace z Azure.

    V následující tabulce jsou uvedeny podrobnosti o způsobu spuštění konkrétní webové aplikace z Azure nebo ke spuštění nebo ladění webovou aplikaci místně pomocí emulátoru výpočetní Azure:

   | Typ webové aplikace | Spustit nebo ladění místně pomocí emulátoru služby výpočty v | spuštění v Azure |
   | --- | --- | --- |
   | Webová aplikace ASP.NET |Na řádku nabídek zvolte **ladění**, **spustit ladění** (klávesové: Zvolte **F5** klíč.). |Zvolte adresa URL hypertextový odkaz v zobrazí **nasazení** kartě pro **protokol činnosti Azure** načíst úvodní stránku v prohlížeči. |
   | Webová aplikace ASP.NET MVC 2 |Na řádku nabídek zvolte **ladění**, **spustit ladění** (klávesové: Zvolte **F5** klíč.). |Zvolte adresa URL hypertextový odkaz v zobrazí **nasazení** kartě pro **protokol činnosti Azure** načíst úvodní stránku v prohlížeči. |
   | Webové aplikace ASP.NET MVC 3 |Na řádku nabídek zvolte **ladění**, **spustit ladění** (klávesové: Zvolte **F5** klíč.). |Zvolte adresa URL hypertextový odkaz v zobrazí **nasazení** kartě pro **protokol činnosti Azure** načíst úvodní stránku v prohlížeči. |
   | Webové aplikace ASP.NET MVC 4 |Na řádku nabídek zvolte **ladění**, **spustit ladění** (klávesové: Zvolte **F5** klíč.). |Zvolte adresa URL hypertextový odkaz v zobrazí **nasazení** kartě pro **protokol činnosti Azure** načíst úvodní stránku v prohlížeči. |
   | Prázdná webová aplikace ASP.NET |Je nutné přidat stránku .aspx ve vaší aplikaci, která nastavíte jako úvodní stránky pro webový projekt. V řádku nabídek zvolte **ladění**, **spustit ladění** (klávesové: Zvolte **F5** klíč.). |Pokud máte výchozí stránku .aspx ve vaší aplikaci, vyberte adresa URL hypertextový odkaz v zobrazí **nasazení** kartě pro **protokol činnosti Azure** a načte tuto stránku v prohlížeči. Pokud máte jiný .aspx stránky, budete muset přejděte na tuto konkrétní stránku v následujícím formátu, pro adresu url:`<url for deployment>/<name of page>.aspx` |
   | Aplikace Silverlight |Na řádku nabídek zvolte **ladění**, **spustit ladění** (klávesové: Zvolte **F5** klíč.). |Je třeba přejít na stránku specifické pro vaši aplikaci v následujícím formátu, pro adresu url:`<url for deployment>/<name of page>.aspx` |
   | Obchodní aplikace Silverlight |Na řádku nabídek zvolte **ladění**, **spustit ladění** (klávesové: Zvolte **F5** klíč.). |Je třeba přejít na stránku specifické pro vaši aplikaci v následujícím formátu, pro adresu url:`<url for deployment>/<name of page>.aspx` |
   | Navigace aplikace Silverlight |Na řádku nabídek zvolte **ladění**, **spustit ladění** (klávesové: Zvolte **F5** klíč.). |Je třeba přejít na stránku specifické pro vaši aplikaci v následujícím formátu, pro adresu url:`<url for deployment>/<name of page>.aspx` |
   | Aplikace služby WCF |Soubor .svc musíte nastavit jako úvodní stránky pro svůj projekt služby WCF. V řádku nabídek zvolte **ladění**, **spustit ladění** (klávesové: Zvolte **F5** klíč.). |Je třeba přejděte k souboru svc pro vaši aplikaci v následujícím formátu, pro adresu url:`<url for deployment>/<name of service file>.svc` |
   | Aplikace služby pracovního postupu WCF |Soubor .svc musíte nastavit jako úvodní stránky pro svůj projekt služby WCF. V řádku nabídek zvolte **ladění**, **spustit ladění** (klávesové: Zvolte **F5** klíč.). |Je třeba přejděte k souboru svc pro vaši aplikaci v následujícím formátu, pro adresu url:`<url for deployment>/<name of service file>.svc` |
   | Dynamické ASP.NET Entity |Na řádku nabídek zvolte **ladění**, **spustit ladění** (klávesové: Zvolte **F5** klíč.). |Je nutné aktualizovat připojovací řetězec (viz další část). Také musíte přejít na stránku specifické pro vaši aplikaci v následujícím formátu, pro adresu url:`<url for deployment>/<name of page>.aspx` |
   | Linq to SQL ASP.NET dynamických dat |Na řádku nabídek zvolte **ladění**, **spustit ladění** (klávesové: Zvolte **F5** klíč.). |Je nutné postupovat podle kroků v tomto postupu: použít databázi SQL Azure pro aplikaci (viz výše uvedené části v tomto tématu). Také musíte přejít na stránku specifické pro vaši aplikaci v následujícím formátu, pro adresu url:`<url for deployment>/<name of page>.aspx` |

## <a name="update-a-connection-string-for-aspnet-dynamic-entities"></a>Aktualizovat připojovací řetězec pro ASP.NET dynamické entity
### <a name="to-update-a-connection-string-for-aspnet-dynamic-entities"></a>Chcete-li aktualizovat připojovací řetězec pro ASP.NET dynamické entity
1. Pokud chcete vytvořit databázi SQL Azure, která lze použít pro webovou aplikaci ASP.NET dynamické entity, postupujte podle kroků v postupu **použít databázi SQL Azure pro vaši aplikaci** výše v tomto tématu.
2. Přidání tabulek a polí, které je nutné pro tuto databázi z [portál Azure classic](http://go.microsoft.com/fwlink/?LinkID=213885).
3. Připojovací řetězec pro tento typ aplikace se v souboru web.config má následující formát:  

    ```
    <addname="tempdbEntities"connectionString="metadata=res://*/Model1.csdl|res://*/Model1.ssdl|res://*/Model1.msl;provider=System.Data.SqlClient;provider connection string=&quot;data source=<server name>\SQLEXPRESS;initial catalog=<database name>;integrated security=True;multipleactiveresultsets=True;App=EntityFramework&quot;"providerName="System.Data.EntityClient"/>
    ```

    Aktualizace *connectionString* hodnotu s připojovací řetězec ADO.NET pro vaši databázi SQL Azure následujícím způsobem:

    ```
    XMLCopy<addname="tempdbEntities"connectionString="metadata=res://*/Model1.csdl|res://*/Model1.ssdl|res://*/Model1.msl;provider=System.Data.SqlClient;provider connection string=&quot;Server=tcp:<SQL Azure server name>.database.windows.net,1433;Database=<database name>;User ID=<user name>;Password=<password>;Trusted_Connection=False;Encrypt=True;multipleactiveresultsets=True;App=EntityFramework&quot;"providerName="System.Data.EntityClient"/>
    ```
4. Pokud chcete uložit soubor web.config se změnami, které jste udělali připojovací řetězec, na řádku nabídek zvolte **soubor**, **uložit soubor web.config**.

## <a name="supported-project-templates"></a>Šablony projektů podporované
Pokud chcete publikovat webovou aplikaci do Azure, aplikace musí používat jednu z šablon projektu pro C# nebo Visual Basic, která je uvedena v následující tabulce.

| Skupina šablon projektu | Šablona projektu |
| --- | --- |
| Web |Webová aplikace ASP.NET |
| Web |Webová aplikace ASP.NET MVC 2 |
| Web |Webové aplikace ASP.NET MVC 3 |
| Web |Webové aplikace ASP.NET MVC4 |
| Web |Prázdná webová aplikace ASP.NET |
| Web |Prázdná webová aplikace ASP.NET MVC 2 |
| Web |Webovou aplikaci ASP.NET dynamické dat entity |
| Web |Linq to SQL webové aplikace ASP.NET dynamických dat |
| Silverlight |Aplikace Silverlight |
| Silverlight |Obchodní aplikace Silverlight |
| Silverlight |Navigace aplikace Silverlight |
| WCF |Aplikace služby WCF |
| WCF |Aplikace služby pracovního postupu WCF |
| Pracovní postup |Aplikace služby pracovního postupu WCF |

## <a name="next-steps"></a>Další kroky
Další informace o publikování najdete v tématu [Příprava na publikování nebo nasazení aplikace Azure ze sady Visual Studio](vs-azure-tools-cloud-service-publish-set-up-required-services-in-visual-studio.md). Také si přečtěte [nastavení až s názvem ověřování pověření](vs-azure-tools-setting-up-named-authentication-credentials.md).
