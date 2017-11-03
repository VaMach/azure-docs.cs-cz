## <a name="set-up-the-development-environment"></a>Nastavení vývojového prostředí

V této části najdete nastavení vašeho vývojového prostředí, včetně vytvoření aplikace ASP.NET MVC, přidává se připojení připojené služby, přidáním řadiče a zadání direktivy požadované oboru názvů.

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

1. Na **přidat připojení službě** dialogovém okně, vyberte **Azure Storage**a potom vyberte **konfigurace**.

    ![Dialogové okno připojené služby](./media/vs-storage-aspnet-getting-started-setup-dev-env/vs-storage-aspnet-getting-started-setup-dev-env-3.png)

1. Na **Azure Storage** dialogové okno, vyberte účet požadované úložiště Azure, se kterým chcete pracovat a vyberte **přidat**.
