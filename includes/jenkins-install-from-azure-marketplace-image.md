1. V prohlížeči otevřete [Azure Marketplace bitovou kopii pro volaných](https://azuremarketplace.microsoft.com/marketplace/apps/azure-oss.jenkins?tab=Overview).

1. Vyberte **získat IT teď**.

    ![Tuto možnost vyberte GIT IT teď ke spuštění procesu instalace bitové kopie volaných Marketplace.](./media/jenkins-install-from-azure-marketplace-image/jenkins-install-get-it-now.png)

1. Po zkontrolování informací cenovou podrobnosti a podmínky, vyberte **pokračovat**.

    ![Ceny a podmínky informace z webu Marketplace volaných bitové kopie.](./media/jenkins-install-from-azure-marketplace-image/jenkins-install-pricing-and-terms.png)

1. Vyberte **vytvořit** konfigurace serveru volaných na portálu Azure. 

    ![Nainstalujte bitovou kopii volaných Marketplace.](./media/jenkins-install-from-azure-marketplace-image/jenkins-install-create.png)

1. V **Základy** kartě, zadejte následující hodnoty:

    - **Název** -zadejte `Jenkins`.
    - **Uživatel** -zadejte uživatelské jméno pro použití při přihlašování do virtuálního počítače, na kterém je spuštěn volaných.
    - **Typ ověřování** – vyberte **heslo**.
    - **Heslo** -zadejte heslo pro použití při přihlašování do virtuálního počítače, na kterém je spuštěn volaných.
    - **Potvrzení hesla** -znovu zadejte heslo pro použití při přihlašování do virtuálního počítače, na kterém je spuštěn volaných.
    - **Typ verze volaných** – vyberte **LTS**.
    - **Předplatné** -vyberte předplatné Azure, do kterého chcete nainstalovat volaných.
    - **Skupina prostředků** – vyberte **vytvořit nový**a zadejte název pro skupinu prostředků, která slouží jako logický kontejner pro kolekci prostředků, které tvoří vaši volaných instalaci.
    - **Umístění** – vyberte **východní USA**.

    ![Zadejte skupiny informace o ověřování a prostředků pro volaných na kartě Základní.](./media/jenkins-install-from-azure-marketplace-image/jenkins-configure-basic.png)

1. Vyberte **OK** Chcete-li pokračovat **nastavení** kartě. 

1. V **nastavení** kartě, zadejte následující hodnoty:

    - **Velikost** -vyberte možnost odpovídající velikosti vašeho volaných virtuálního počítače.
    - **Typ disku virtuálního počítače** – zadejte buď HDD (jednotku pevného disku) nebo SSD (jednotka SSD) k označení disku typů úložiště, které je povoleno pro virtuální počítač volaných.
    - **Veřejná IP adresa** -výchozí název adresy IP na volaných název, který jste zadali na předchozí stránce s příponou - IP. Můžete vybrat možnost změnit tuto výchozí nastavení.
    - **Popisek názvu domény** – zadejte hodnotu pro plně kvalifikovanou adresu URL pro virtuální počítač volaných.

    ![Zadejte nastavení virtuálního počítače pro volaných na kartě nastavení.](./media/jenkins-install-from-azure-marketplace-image/jenkins-configure-settings.png)

1. Vyberte **OK** Chcete-li pokračovat **Souhrn** kartě.

1. Když **Souhrn** karta zobrazuje, se ověří zadané informace. Jakmile se zobrazí **ověření proběhlo úspěšně.** zprávy, vyberte **OK**. 

    ![Souhrn zobrazí a ověří vybrané možnosti.](./media/jenkins-install-from-azure-marketplace-image/jenkins-configure-summary.png)

1. Když **vytvořit** zobrazí kartu, vyberte **vytvořit** k vytvoření virtuálního počítače volaných. Pokud váš server je připraven, zobrazí se oznámení na portálu Azure.

    ![Volaných je připraven oznámení.](./media/jenkins-install-from-azure-marketplace-image/jenkins-install-notification.png)