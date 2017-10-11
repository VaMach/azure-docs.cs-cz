1. Přihlaste se na [web Azure Portal].
2. Klikněte na **+NOVÝ** > **Web + mobilní zařízení** > **Mobilní aplikace** a potom zadejte název back-endu mobilní aplikace .
3. V případě **skupiny prostředků** vyberte existující skupinu prostředků nebo vytvořte novou (použijte stejný název, jaký má aplikace). 
   
    Můžete buď vybrat jiný plán služby App Service nebo vytvořit nový. Další informace o plánech služby App Service a vytvoření nového plánu v různých cenových úrovních a požadovaném umístění najdete v článku [Podrobný přehled plánů služby Azure App Service](../articles/app-service/azure-web-sites-web-hosting-plans-in-depth-overview.md).
4. V případě **plánu služby App Service** je vybraný výchozí plán (na [úrovni Standard](https://azure.microsoft.com/pricing/details/app-service/)). Můžete také vybrat jiný plán nebo [vytvořte novou](../articles/app-service/azure-web-sites-web-hosting-plans-in-depth-overview.md#create-an-app-service-plan). Nastavení plánu služby App Service určují [umístění, funkce a nákladové a výpočetní prostředky](https://azure.microsoft.com/pricing/details/app-service/) přidružené k vaší aplikaci. 
   
    Až se rozhodnete pro konkrétní plán, klikněte na **Vytvořit**. Tím vytvoříte back-end mobilní aplikace. 
5. V okně **Nastavení** nového back-endu mobilní aplikace klikněte na **Rychlý start** > aplikační platforma vašeho klienta > **Připojit databázi**. 
   
    ![](./media/app-service-mobile-dotnet-backend-create-new-service/dotnet-backend-create-data-connection.png)
6. V okně **Přidat datové připojení** klikněte na **Databáze SQL** > **Vytvořit novou databázi**, zadejte **Název** databáze, zvolte cenovou úroveň a potom klikněte na **Server**.  Novou databázi můžete použít opakovaně. Pokud už ve stejném umístění databázi máte, můžete místo toho vybrat možnost **Použít existující databázi**. Nedoporučujeme používat databázi v jiném umístění, protože vám to zvýší náklady na šířku pásma a zhorší latenci.
   
    ![](./media/app-service-mobile-dotnet-backend-create-new-service/dotnet-backend-create-db.png)
7. V okně **Nový server** zadejte do pole **Název serveru** jedinečný název serveru, zadejte přihlašovací jméno a heslo, zaškrtněte políčko **Povolit službám Azure přístup k serveru** a klikněte na tlačítko **OK**. Tím vytvoříte novou databázi.
8. Zpátky v okně **Přidat datové připojení** klikněte na **Připojovací řetězec**, zadejte přihlašovací jméno a heslo svojí databáze a klikněte na tlačítko **OK**. Počkejte několik minut na úspěšné nasazení databáze a teprve potom pokračujte.

<!-- URLs. -->
[web Azure Portal]: https://portal.azure.com/
