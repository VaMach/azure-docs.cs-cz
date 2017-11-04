---
title: "Sestavení aplikace ASP.NET v Azure SQL Database | Microsoft Docs"
description: "Další informace o získání aplikace ASP.NET, v Azure, funguje s připojením k databázi SQL."
services: app-service\web
documentationcenter: nodejs
author: cephalin
manager: erikre
editor: 
ms.assetid: 03c584f1-a93c-4e3d-ac1b-c82b50c75d3e
ms.service: app-service-web
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: csharp
ms.topic: tutorial
ms.date: 06/09/2017
ms.author: cephalin
ms.custom: mvc, devcenter
ms.openlocfilehash: db3be8068ef9e560614daa0e7f0dcf62467fd338
ms.sourcegitcommit: 3e3a5e01a5629e017de2289a6abebbb798cec736
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/27/2017
---
# <a name="build-an-aspnet-app-in-azure-with-sql-database"></a>Sestavení aplikace ASP.NET v Azure SQL Database

[Azure Web Apps](app-service-web-overview.md) je vysoce škálovatelná služba s automatickými opravami pro hostování webů. V tomto kurzu se dozvíte, jak nasadit datové webové aplikace ASP.NET v Azure a propojte jej s [Azure SQL Database](../sql-database/sql-database-technical-overview.md). Jakmile budete hotovi, máte ASP.NET aplikace spuštěná v Azure a připojení k databázi SQL.

![Publikované aplikace ASP.NET ve službě Azure web app](./media/app-service-web-tutorial-dotnet-sqldatabase/azure-app-in-browser.png)

V tomto kurzu se naučíte:

> [!div class="checklist"]
> * Vytvoření databáze SQL v Azure
> * Připojení k databázi SQL aplikace ASP.NET
> * Nasazení aplikace do Azure
> * Aktualizovat datový model a aplikaci znovu nasaďte
> * Datový proud protokolů z Azure terminálu
> * Spravovat aplikaci na portálu Azure

## <a name="prerequisites"></a>Požadavky

K provedení kroků v tomto kurzu je potřeba:

* Nainstalovat [Visual Studio 2017](https://www.visualstudio.com/downloads/) s následujícími sadami funkcí:
  - **Vývoj pro ASP.NET a web**
  - **Azure – vývoj**

  ![Vývoj pro ASP.NET a Azure – vývoj (v části Web a cloud)](media/app-service-web-tutorial-dotnet-sqldatabase/workloads.png)

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

## <a name="download-the-sample"></a>Stažení ukázky

[Stáhněte si ukázkový projekt](https://github.com/Azure-Samples/dotnet-sqldb-tutorial/archive/master.zip).

Extrahování (rozbalte) *dotnet-sqldb kurzu master.zip* souboru.

Ukázkový projekt obsahuje základní [ASP.NET MVC](https://www.asp.net/mvc) CRUD (vytvoření čtení aktualizace odstranění) aplikace pomocí [Entity Framework Code First](/aspnet/mvc/overview/getting-started/getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application).

### <a name="run-the-app"></a>Spuštění aplikace

Otevřete *dotnet-sqldb – kurz – hlavní/DotNetAppSqlDb.sln* souborů v sadě Visual Studio. 

Typ `Ctrl+F5` a spusťte aplikaci bez ladění. Aplikace se zobrazí ve výchozím prohlížeči. Vyberte **vytvořit nový** propojit a vytvořit pár *úkolů* položky. 

![Dialogové okno Nový projekt ASP.NET](media/app-service-web-tutorial-dotnet-sqldatabase/local-app-in-browser.png)

Testovací **upravit**, **podrobnosti**, a **odstranit** odkazy.

Aplikace používá pro připojení k databázi kontext databáze. V této ukázce kontext databáze používá připojovací řetězec s názvem `MyDbConnection`. Připojovací řetězec je nastavena v *Web.config* souborů a v odkazuje *Models/MyDatabaseContext.cs* souboru. Název připojovacího řetězce se později v tomto kurzu používá pro připojení k databázi SQL Azure webové aplikace Azure. 

## <a name="publish-to-azure-with-sql-database"></a>Publikování v Azure SQL Database

V **Průzkumníku řešení**, klikněte pravým tlačítkem na vaše **DotNetAppSqlDb** projektu a vyberte **publikovat**.

![Publikování z Průzkumníka řešení](./media/app-service-web-tutorial-dotnet-sqldatabase/solution-explorer-publish.png)

Zkontrolujte, že je vybraná možnost **Microsoft Azure App Service** a klikněte na **Publikovat**.

![Publikování ze stránky přehledu projektu](./media/app-service-web-tutorial-dotnet-sqldatabase/publish-to-app-service.png)

Publikování se otevře **vytvořit službu App Service** dialog, který vám pomůže vytvořit všechny prostředky Azure budete muset spustit webové aplikace ASP.NET v Azure.

### <a name="sign-in-to-azure"></a>Přihlášení k Azure

V dialogovém okně **Vytvoření služby App Service** klikněte na **Přidat účet** a přihlaste se ke svému předplatnému Azure. Pokud jste již přihlášení k účtu Microsoft, ujistěte se, že odpovídá vašemu předplatnému Azure. Pokud jste přihlášeni k účtu Microsoft, který nemá přiřazené předplatné Azure, kliknutím na něj přidejte správný účet.
   
![Přihlášení k Azure](./media/app-service-web-tutorial-dotnet-sqldatabase/sign-in-azure.png)

Až se přihlásíte, můžete v tomto okně vytvořit všechny prostředky, které potřebujete pro vaši webovou aplikaci Azure.

### <a name="configure-the-web-app-name"></a>Nakonfigurujte název webové aplikace

Můžete ponechat název vygenerovaný webové aplikace, nebo ho změnit na jiný jedinečný název (platnými znaky jsou `a-z`, `0-9`, a `-`). Název webové aplikace se používá jako součást výchozí adresa URL pro aplikaci (`<app_name>.azurewebsites.net`, kde `<app_name>` je název vaší webové aplikace). Název webové aplikace musí být jedinečný v rámci všech aplikací v Azure. 

![Vytvoření dialogovém okně app service](media/app-service-web-tutorial-dotnet-sqldatabase/wan.png)

> [!NOTE]
> Neklikejte na **vytvořit**. Nejprve je třeba nastavit vytvářet databáze SQL v pozdější fázi.

### <a name="create-a-resource-group"></a>Vytvoření skupiny prostředků

[!INCLUDE [resource-group](../../includes/resource-group.md)]

Vedle pole **Skupina prostředků** klikněte na tlačítko **Nová**.

![Vedle skupinu prostředků klikněte na tlačítko Nový.](media/app-service-web-tutorial-dotnet-sqldatabase/new_rg2.png)

Název skupiny prostředků **myResourceGroup**.

### <a name="create-an-app-service-plan"></a>Vytvoření plánu služby App Service

[!INCLUDE [app-service-plan](../../includes/app-service-plan.md)]

Vedle pole **Plán služby App Service** klikněte na **Nový**. 

V dialogovém okně **Konfigurace plánu služby App Service** nastavte nový plán takto:

![Vytvoření plánu služby App Service](./media/app-service-web-tutorial-dotnet-sqldatabase/configure-app-service-plan.png)

| Nastavení  | Navrhovaná hodnota | Další informace |
| ----------------- | ------------ | ----|
|**Plán služby App Service**| myAppServicePlan | [Plány služby App Service](../app-service/azure-web-sites-web-hosting-plans-in-depth-overview.md) |
|**Umístění**| Západní Evropa | [Oblasti Azure](https://azure.microsoft.com/regions/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio) |
|**Velikost**| Free | [Cenové úrovně](https://azure.microsoft.com/pricing/details/app-service/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio)|

### <a name="create-a-sql-server-instance"></a>Vytvoření instance systému SQL Server

Před vytvořením databázi, budete potřebovat [logického serveru Azure SQL Database](../sql-database/sql-database-features.md). Logický server obsahuje soubor databází spravovaných jako skupina.

Vyberte **Objevte další služby Azure**.

![Nastavení názvu webové aplikace](media/app-service-web-tutorial-dotnet-sqldatabase/web-app-name.png)

V **služby** , klikněte na  **+**  ikonu vedle **SQL Database**. 

![Na kartě služby klikněte + Ikona vedle databáze SQL.](media/app-service-web-tutorial-dotnet-sqldatabase/sql.png)

V **nakonfigurovat databázi SQL** dialogové okno, klikněte na tlačítko **nový** vedle **systému SQL Server**. 

Generuje se název jedinečný serveru. Tento název se používá jako součást výchozí adresa URL logického serveru, `<server_name>.database.windows.net`. Musí být ve všech instancích logický server v Azure jedinečný. Můžete změnit název serveru, ale pro účely tohoto kurzu zachovat generované hodnoty.

Přidáte správce uživatelské jméno a heslo. Požadavky na složitost hesla, najdete v části [zásady hesel](/sql/relational-databases/security/password-policy).

Mějte na paměti Toto uživatelské jméno a heslo. Je potřebujete spravovat instanci logického serveru později.

![Vytvoření instance systému SQL Server](media/app-service-web-tutorial-dotnet-sqldatabase/configure-sql-database-server.png)

Klikněte na **OK**. Neuzavírejte **nakonfigurovat databázi SQL** zatím dialogové okno.

### <a name="create-a-sql-database"></a>Vytvoření databáze SQL

V **nakonfigurovat databázi SQL** dialogové okno: 

* Ponechat výchozí generovaný **název databáze**.
* V **název připojovacího řetězce**, typ *MyDbConnection*. Tento název musí odpovídat připojovací řetězec, který se odkazuje v *Models/MyDatabaseContext.cs*.
* Vyberte **OK**.

![Konfigurace databáze SQL](media/app-service-web-tutorial-dotnet-sqldatabase/configure-sql-database.png)

**Vytvořit službu App Service** dialog zobrazuje prostředky jste vytvořili. Klikněte na možnost **Vytvořit**. 

![prostředky, které jste vytvořili](media/app-service-web-tutorial-dotnet-sqldatabase/app_svc_plan_done.png)

Až průvodce dokončí vytváření prostředků Azure, publikuje aplikace ASP.NET do Azure. Spuštění výchozího prohlížeče s adresou URL nasazené aplikace. 

Přidejte několik položek úkolů.

![Publikované aplikace ASP.NET ve službě Azure web app](./media/app-service-web-tutorial-dotnet-sqldatabase/azure-app-in-browser.png)

Blahopřejeme! Vaše aplikace ASP.NET řízené daty je spuštěna za provozu v Azure App Service.

## <a name="access-the-sql-database-locally"></a>Přístup k databázi SQL místně

Visual Studio umožňuje prozkoumat a spravovat snadno v nové databáze SQL **Průzkumník objektů systému SQL Server**.

### <a name="create-a-database-connection"></a>Vytvoření připojení k databázi

Z **zobrazení** nabídce vyberte možnost **Průzkumník objektů systému SQL Server**.

V horní části **Průzkumník objektů systému SQL Server**, klikněte **přidat SQL Server** tlačítko.

### <a name="configure-the-database-connection"></a>Konfigurace připojení k databázi

V **připojit** dialogové okno, rozbalte **Azure** uzlu. Všechny instance databáze SQL v Azure jsou zde uvedeny.

Vyberte databázi SQL, který jste vytvořili dříve. V dolní části se automaticky vyplní připojení, které jste vytvořili dříve.

Zadejte heslo správce databáze, který jste dříve vytvořili a klikněte na tlačítko **Connect**.

![Konfigurace připojení k databázi ze sady Visual Studio](./media/app-service-web-tutorial-dotnet-sqldatabase/connect-to-sql-database.png)

### <a name="allow-client-connection-from-your-computer"></a>Povolit připojení klienta z vašeho počítače

**Vytvořit nové pravidlo brány firewall** se otevře dialogové okno. Ve výchozím nastavení umožňuje vaší instanci služby SQL Database pouze připojení ze služby Azure, jako je například Azure webové aplikace. Pro připojení k databázi, vytvořte pravidlo brány firewall v instanci databáze SQL. Pravidlo brány firewall umožňuje veřejnou IP adresu místního počítače.

Dialogové okno je již vyplněn veřejná IP adresa vašeho počítače.

Ujistěte se, že **přidat Moje IP adresa klienta** je vybrána a klikněte na tlačítko **OK**. 

![Nastavení brány firewall pro instanci databáze SQL](./media/app-service-web-tutorial-dotnet-sqldatabase/sql-set-firewall.png)

Jakmile sady Visual Studio dokončí vytváření nastavení brány firewall pro instanci SQL databáze, připojení se zobrazí v **Průzkumník objektů systému SQL Server**.

Zde můžete provádět nejběžnější databázových operací, jako je například spouštění dotazů, vytvořit zobrazení a uložených procedur a další. 

Rozbalte připojení > **databáze** > **&lt;databáze >** > **tabulky**. Klikněte pravým tlačítkem na `Todoes` tabulky a vyberte **Data zobrazení**. 

![Prozkoumejte objekty databáze SQL](./media/app-service-web-tutorial-dotnet-sqldatabase/explore-sql-database.png)

## <a name="update-app-with-code-first-migrations"></a>Aktualizovat aplikaci migrace Code First

Známých nástrojů v sadě Visual Studio slouží k aktualizaci databáze a webové aplikace v Azure. V tomto kroku použijete migrace Code First v Entity Framework k provedení změny svého schématu databáze a publikujete ho v Azure.

Další informace o použití migrace Entity Framework Code First najdete v tématu [Začínáme s Entity Framework 6 Code First pomocí MVC 5](https://docs.microsoft.com/aspnet/mvc/overview/getting-started/getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application).

### <a name="update-your-data-model"></a>Aktualizovat data modelu

Otevřete _Models\Todo.cs_ v editoru kódu. Přidejte následující vlastnosti, která má `ToDo` třídy:

```csharp
public bool Done { get; set; }
```

### <a name="run-code-first-migrations-locally"></a>Spustit migrace Code First místně

Spusťte několik příkazy provádění aktualizací místní databázi. 

Z **nástroje** nabídky, klikněte na tlačítko **Správce balíčků NuGet** > **Konzola správce balíčků**.

V okně konzoly Správce balíčků povolení migrace Code First:

```PowerShell
Enable-Migrations
```

Přidejte migrace:

```PowerShell
Add-Migration AddProperty
```

Aktualizace místní databázi:

```PowerShell
Update-Database
```

Typ `Ctrl+F5` a spusťte aplikaci. Upravit podrobnosti, otestovat a vytvořit odkazy.

Pokud aplikace načte bez chyb, proběhla úspěšně migrace Code First. Ale stránku stále vypadá stejně protože aplikační logiku ještě nepoužívá tuto novou vlastnost. 

### <a name="use-the-new-property"></a>Použít nové vlastnosti

Některé změny v kódu k použití `Done` vlastnost. Pro zjednodušení v tomto kurzu se pouze chystáte změnit `Index` a `Create` zobrazení naleznete ve vlastnosti v akci.

Otevřete _Controllers\TodosController.cs_.

Najít `Create()` metoda na řádek 52 a přidejte `Done` do seznamu vlastností v `Bind` atribut. Když jste hotovi, vaše `Create()` podpis metody vypadá jako následující kód:

```csharp
public ActionResult Create([Bind(Include = "Description,CreatedDate,Done")] Todo todo)
```

Otevřete _Views\Todos\Create.cshtml_.

V kódu Razor, měli byste vidět `<div class="form-group">` element, který používá `model.Description`a pak další `<div class="form-group">` element, který používá `model.CreatedDate`. Hned za tyto dva prvky, přidejte další `<div class="form-group">` element, který používá `model.Done`:

```csharp
<div class="form-group">
    @Html.LabelFor(model => model.Done, htmlAttributes: new { @class = "control-label col-md-2" })
    <div class="col-md-10">
        <div class="checkbox">
            @Html.EditorFor(model => model.Done)
            @Html.ValidationMessageFor(model => model.Done, "", new { @class = "text-danger" })
        </div>
    </div>
</div>
```

Otevřete _Views\Todos\Index.cshtml_.

Vyhledejte prázdné `<th></th>` elementu. Nad tento element přidejte následující kód Razor:

```csharp
<th>
    @Html.DisplayNameFor(model => model.Done)
</th>
```

Najít `<td>` elementu, který obsahuje `Html.ActionLink()` pomocné metody. _Výše_ to `<td>`, přidejte další `<td>` element s následujícím kódem Razor:

```csharp
<td>
    @Html.DisplayFor(modelItem => item.Done)
</td>
```

To je všechno potřebujete zobrazit změny v `Index` a `Create` zobrazení. 

Typ `Ctrl+F5` a spusťte aplikaci.

Teď můžete přidat položku seznamu úkolů a zkontrolujte **provádí**. Potom ho měli zobrazí v domovské stránce jako dokončené položka. Nezapomeňte, že `Edit` zobrazení nezobrazí `Done` pole, protože nebyla změní `Edit` zobrazení.

### <a name="enable-code-first-migrations-in-azure"></a>Povolení migrace Code First v Azure

Teď, když kód změnit funguje, včetně databáze migrace jej publikujte do vaší webové aplikace Azure a příliš aktualizace databáze SQL se migrace Code First.

Podobně jako před, klikněte pravým tlačítkem na projekt a vyberte **publikovat**.

Klikněte na tlačítko **nastavení** otevřete Průvodce Publikovat.

![Otevřete nastavení publikování](./media/app-service-web-tutorial-dotnet-sqldatabase/publish-settings.png)

V průvodci klikněte na tlačítko **Další**.

Ujistěte se, že je připojovací řetězec pro vaši databázi SQL vložené do **MyDatabaseContext (MyDbConnection)**. Je nutné vybrat **myToDoAppDb** databáze z rozevíracího seznamu. 

Vyberte **spustit migrace Code First (spuštěno při spuštění aplikace)**, pak klikněte na tlačítko **Uložit**.

![Povolení migrace Code First ve webové aplikace Azure](./media/app-service-web-tutorial-dotnet-sqldatabase/enable-migrations.png)

### <a name="publish-your-changes"></a>Publikování změn

Teď, když jste povolili migrace Code First ve vaší webové aplikace Azure, publikování změn kódu.

Na stránce publikování klikněte na **Publikovat**.

Přidejte položkami seznamu úkolů znovu a vyberte **provádí**, a měli objeví v domovské stránce jako dokončené položka.

![Webové aplikace Azure po migraci první kódu](./media/app-service-web-tutorial-dotnet-sqldatabase/this-one-is-done.png)

Všechny vaše existující položky seznamu úkolů se pořád zobrazí. Při opětovném aplikace ASP.NET, není stávající data v databázi SQL ztraceny. Migrace Code First také, pouze změní schéma dat a svoje existující data zůstanou zachovány.


## <a name="stream-application-logs"></a>Protokoly aplikací datového proudu

Trasování zprávy můžete přímo z vaší webové aplikace Azure datového proudu k sadě Visual Studio.

Otevřete _Controllers\TodosController.cs_.

Každá akce začíná `Trace.WriteLine()` metoda. Tento kód se přidá do ukazují, jak přidat trasování zprávy do vaší webové aplikace Azure.

### <a name="open-server-explorer"></a>V Průzkumníku serveru otevřete

Z **zobrazení** nabídce vyberte možnost **Průzkumníka serveru**. Můžete nakonfigurovat protokolování pro Azure webové aplikace v **Průzkumníka serveru**. 

### <a name="enable-log-streaming"></a>Povolení protokolu streamování

V **Průzkumníka serveru**, rozbalte položku **Azure** > **služby App Service**.

Rozbalte **myResourceGroup** skupinu prostředků, jste vytvořili při prvním vytvoření webové aplikace Azure.

Klikněte pravým tlačítkem na vaší webové aplikace Azure a vyberte **zobrazit protokoly streamování**.

![Povolení protokolu streamování](./media/app-service-web-tutorial-dotnet-sqldatabase/stream-logs.png)

Protokoly jsou nyní streamování do **výstup** okno. 

![V okně výstupu datový proud protokolu](./media/app-service-web-tutorial-dotnet-sqldatabase/log-streaming-pane.png)

Ale nezobrazí žádná trasování zpráv ještě. To je vzhledem k tomu, že pokud nejprve vybrat **zobrazit protokoly streamování**, vaší webové aplikace Azure nastaví na úrovni trasování `Error`, který jenom protokoluje události chyby (s `Trace.TraceError()` metoda).

### <a name="change-trace-levels"></a>Změna úrovně trasování

Pokud chcete změnit úrovně trasování do výstupního další zprávy trasování, přejděte zpět na **Průzkumníka serveru**.

Znovu klikněte pravým tlačítkem Azure webové aplikace a vyberte **nastavení zobrazení**.

V **protokolování aplikace (systém souborů)** rozevíracího seznamu vyberte **podrobné**. Klikněte na **Uložit**.

![Změňte úroveň trasování Verbose](./media/app-service-web-tutorial-dotnet-sqldatabase/trace-level-verbose.png)

> [!TIP]
> Můžete experimentovat s úrovněmi různých trasování chcete zobrazit, jaké typy zprávy se zobrazují pro každou úroveň. Například **informace** úroveň zahrnuje všechny protokoly, které jsou vytvořené `Trace.TraceInformation()`, `Trace.TraceWarning()`, a `Trace.TraceError()`, ale není protokoly vytvořené `Trace.WriteLine()`.
>
>

V prohlížeči přejděte na webovou aplikaci znovu na *http://&lt;název aplikace >. azurewebsites.net*, poté zkuste klepnout kolem aplikaci seznamu úkolů v Azure. Trasovací zprávy jsou nyní streamování na **výstup** oken v sadě Visual Studio.

```console
Application: 2017-04-06T23:30:41  PID[8132] Verbose     GET /Todos/Index
Application: 2017-04-06T23:30:43  PID[8132] Verbose     GET /Todos/Create
Application: 2017-04-06T23:30:53  PID[8132] Verbose     POST /Todos/Create
Application: 2017-04-06T23:30:54  PID[8132] Verbose     GET /Todos/Index
```



### <a name="stop-log-streaming"></a>Zastavit streamování protokolu

Zastavit službu protokolu streamování, klikněte na tlačítko **zastavit monitorování** v tlačítko **výstup** okno.

![Zastavit streamování protokolu](./media/app-service-web-tutorial-dotnet-sqldatabase/stop-streaming.png)

## <a name="manage-your-azure-web-app"></a>Správa Azure webové aplikace

Přejděte na [portál Azure](https://portal.azure.com) zobrazíte webové aplikace, které jste vytvořili. 



V levé nabídce klikněte na **App Service** a pak klikněte na název vaší webové aplikace Azure.

![Navigace portálem k webové aplikaci Azure](./media/app-service-web-tutorial-dotnet-sqldatabase/access-portal.png)

Dostali jste na stránce vaší webové aplikace. 

Ve výchozím nastavení, zobrazí na portálu **přehled** stránky. Tato stránka poskytuje přehled, jak si vaše aplikace stojí. Tady můžete také provést základní úlohy správy, jako je procházení, zastavení, spuštění, restartování a odstranění. Karty na levé straně stránky zobrazí stránek jinou konfiguraci, že můžete otevřít. 

![Stránka služby App Service na webu Azure Portal](./media/app-service-web-tutorial-dotnet-sqldatabase/web-app-blade.png)

[!INCLUDE [Clean up section](../../includes/clean-up-section-portal-web-app.md)]

<a name="next"></a>

## <a name="next-steps"></a>Další kroky

V tomto kurzu jste se naučili:

> [!div class="checklist"]
> * Vytvoření databáze SQL v Azure
> * Připojení k databázi SQL aplikace ASP.NET
> * Nasazení aplikace do Azure
> * Aktualizovat datový model a aplikaci znovu nasaďte
> * Datový proud protokolů z Azure terminálu
> * Spravovat aplikaci na portálu Azure

Přechodu na dalším kurzu se dozvíte, jak namapovat vlastní název DNS do webové aplikace.

> [!div class="nextstepaction"]
> [Mapování existujícího vlastního názvu DNS na Azure Web Apps](app-service-web-tutorial-custom-domain.md)
