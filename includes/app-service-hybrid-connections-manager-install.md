
1. V **hybridní připojení** okno, klikněte na tlačítko hybridní připojení, který jste právě vytvořili, klikněte **naslouchací proces instalace**.
   
    ![Klikněte na tlačítko naslouchací proces instalace](./media/app-service-hybrid-connections-manager-install/D04ClickListenerSetup.png)
2. **Vlastnosti hybridní připojení** otevře se okno. V části **místní správce hybridního připojení**, zvolte **stáhnout a nakonfigurovat ručně**, uložte stažený balíček HybridConnectionManager.msi a zkopírujte připojovací řetězec brány.
   
    ![Chcete-li nainstalovat, klikněte sem](./media/app-service-hybrid-connections-manager-install/D05ClickToInstallHCM.png)
3. Z příkazového řádku správce zadejte následující příkaz, který spouští instalační program:
   
        start HybridConnectionManager.msi
4. Po spuštění instalačního programu, klikněte na tlačítko **teď ne**, pak přejděte do složky %ProgramFiles%\Microsoft\HybridConnectionManager, spustí HCMConfigWizard.exe a klikněte na tlačítko **Ano** v **uživatelský účet Ovládací prvek** dialogové okno.
5. Vložte hybridní připojovací řetězec, který jste zkopírovali dříve a klikněte na tlačítko **OK**. 
   
    ![Instalace](./media/app-service-hybrid-connections-manager-install/D08aHCMInstallManual.png)
6. Po dokončení instalace klikněte na tlačítko **Zavřít**.
   
    ![Klikněte na tlačítko Zavřít](./media/app-service-hybrid-connections-manager-install/D09HCMInstallComplete.png)
   
    Na **hybridní připojení** okně **stav** teď zobrazuje sloupec **připojeno**. 
   
    ![Připojené stav](./media/app-service-hybrid-connections-manager-install/D10HCStatusConnected.png)

