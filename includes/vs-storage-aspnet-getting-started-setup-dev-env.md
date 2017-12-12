## <a name="set-up-the-development-environment"></a>Nastavení vývojového prostředí

Tato část vás provede nastavení vývojového prostředí, včetně vytvoření aplikace ASP.NET MVC, přidává se připojení připojené služby, přidáním řadiče a zadání direktivy požadované oboru názvů.

### <a name="create-an-aspnet-mvc-app-project"></a>Vytvoření projektu aplikace ASP.NET MVC

1. Otevřete sadu Visual Studio.

1. Vyberte **souboru -> Nový -> projekt** z hlavní nabídky

1. Na **nový projekt** dialogové okno, zadejte možnosti, jak je znázorněno na následujícím obrázku:

    ![Vytvoření projektu ASP.NET](./media/vs-storage-aspnet-getting-started-setup-dev-env/vs-storage-aspnet-getting-started-setup-dev-env-1.png)

1. Vyberte **OK**.

1. Na **nový projekt ASP.NET** dialogové okno, zadejte možnosti, jak je znázorněno na následujícím obrázku:

    ![Zadejte MVC](./media/vs-storage-aspnet-getting-started-setup-dev-env/vs-storage-aspnet-getting-started-setup-dev-env-2.png)

1. Vyberte **OK**.

### <a name="use-connected-services-to-connect-to-an-azure-storage-account"></a>Používat připojení služby pro připojení k účtu úložiště Azure

1. V **Průzkumníku řešení**, klikněte pravým tlačítkem na projekt a v místní nabídce vyberte **Přidat -> připojené služby**.

1. Na **přidat připojení službě** dialogovém okně, vyberte **úložiště v cloudu s Azure Storage**a potom vyberte **konfigurace**.

    ![Dialogové okno připojené služby](./media/vs-storage-aspnet-getting-started-setup-dev-env/vs-storage-aspnet-getting-started-setup-dev-env-3.png)

1. Na **Azure Storage** dialogovém okně, vyberte účet úložiště Azure má být použit pro tento kurz.  Chcete-li vytvořit nový účet úložiště Azure, klikněte na tlačítko **vytvořit nový účet úložiště** a vyplňte formulář.  Po výběru buď existující účet úložiště nebo vytvořit nový, klikněte na tlačítko **přidat**.  Visual Studio se nainstalujte balíček NuGet pro Azure Storage a připojovací řetězec úložiště do **Web.config**.

> [!TIP]
> Další informace o vytvoření účtu úložiště s [portál Azure](https://portal.azure.com), najdete v části [vytvořit účet úložiště](../articles/storage/common/storage-create-storage-account.md#create-a-storage-account).
>
> Účet úložiště Azure můžete vytvořit také pomocí [prostředí Azure PowerShell](../articles/storage/common/storage-powershell-guide-full.md), [rozhraní příkazového řádku Azure](../articles/storage/common/storage-azure-cli.md), nebo [prostředí cloudu Azure](../articles/cloud-shell/overview.md).

