## <a name="create-a-self-hosted-ir"></a>Vytvoření místního prostředí IR

V této části můžete vytvořit místní prostředí Integration Runtime a přidružit ho k místnímu počítači s databází SQL Serveru. Místní prostředí Integration Runtime místně je komponenta, která zkopíruje data z SQL Serveru ve vašem počítači do úložiště objektů blob v Azure. 

1. Vytvořte proměnnou pro název prostředí Integration Runtime. Použijte jedinečný název, který si poznamenejte. Použijete ho později v tomto kurzu. 

    ```powershell
   $integrationRuntimeName = "ADFTutorialIR"
    ```
1. Vytvořte místní prostředí Integration Runtime. 

   ```powershell
   Set-AzureRmDataFactoryV2IntegrationRuntime -Name $integrationRuntimeName -Type SelfHosted -DataFactoryName $dataFactoryName -ResourceGroupName $resourceGroupName
   ```

   Zde je ukázkový výstup:

   ```json
    Id                : /subscriptions/<subscription ID>/resourceGroups/ADFTutorialResourceGroup/providers/Microsoft.DataFactory/factories/onpremdf0914/integrationruntimes/myonpremirsp0914
    Type              : SelfHosted
    ResourceGroupName : ADFTutorialResourceGroup
    DataFactoryName   : onpremdf0914
    Name              : myonpremirsp0914
    Description       :
    ```
 

2. Spusťte následující příkaz, který načte stav vytvořeného prostředí Integration Runtime. Potvrďte, že hodnota vlastnosti **State** je nastavena na **NeedRegistration**. 

   ```powershell
   Get-AzureRmDataFactoryV2IntegrationRuntime -name $integrationRuntimeName -ResourceGroupName $resourceGroupName -DataFactoryName $dataFactoryName -Status
   ```

   Zde je ukázkový výstup:

   ```json
   Nodes                     : {}
   CreateTime                : 9/14/2017 10:01:21 AM
   InternalChannelEncryption :
   Version                   :
   Capabilities              : {}
   ScheduledUpdateDate       :
   UpdateDelayOffset         :
   LocalTimeZoneOffset       :
   AutoUpdate                :
   ServiceUrls               : {eu.frontend.clouddatahub.net, *.servicebus.windows.net}
   ResourceGroupName         : <ResourceGroup name>
   DataFactoryName           : <DataFactory name>
   Name                      : <Integration Runtime name>
   State                     : NeedRegistration
   ```

3. Spusťte následující příkaz, který načte **ověřovací klíče** pro registraci místního prostředí Integration Runtime ve službě Data Factory v cloudu. 

   ```powershell
   Get-AzureRmDataFactoryV2IntegrationRuntimeKey -Name $integrationRuntimeName -DataFactoryName $dataFactoryName -ResourceGroupName $resourceGroupName | ConvertTo-Json
   ```

   Zde je ukázkový výstup:

   ```json
   {
       "AuthKey1":  "IR@0000000000-0000-0000-0000-000000000000@xy0@xy@xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx=",
       "AuthKey2":  "IR@0000000000-0000-0000-0000-000000000000@xy0@xy@yyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyy="
   }
   ```    
4. Pro registraci místního prostředí Integration Runtime, který nainstalujete na počítači v dalším kroku, zkopírujte jeden z klíčů (bez dvojitých uvozovek).  

## <a name="install-integration-runtime"></a>Instalace prostředí Integration Runtime
1. Pokud již na počítači máte **Microsoft Integration Runtime**, odinstalujte ho pomocí panelu **Přidat nebo odebrat programy**. 
2. Na místním počítači s Windows [stáhněte](https://www.microsoft.com/download/details.aspx?id=39717) místní prostředí Integration Runtime a spusťte instalaci. 
3. Na stránce **Vítá vás průvodce instalací prostředí Microsoft Integration Runtime** klikněte na **Další**.  
4. Na stránce **Licenční smlouva s koncovým uživatelem (EULA)** přijměte podmínky a licenční smlouvu a klikněte na **Další**. 
5. Na stránce **Cílová složka** klikněte na **Další**. 
6. Na stránce **Připraveno k instalaci prostředí Microsoft Integration Runtime** klikněte na **Nainstalovat**. 
7. Pokud se zobrazí zpráva upozorňující na konfiguraci přechodu počítače do režimu spánku nebo hibernace, když se zrovna nepoužívá, klikněte na **OK**. 
8. Pokud se zobrazí okno **Možnosti napájení**, zavřete ho a přepněte do okna nastavení. 
9. Na stránce **Dokončení průvodce instalací prostředí Microsoft Integration Runtime** klikněte na **Dokončit**.
10. Na stránce **Registrace prostředí Integration Runtime (v místním prostředí)** vložte klíč, který jste uložili v předchozí části, a klikněte na **Zaregistrovat**. 

   ![Registrace prostředí Integration Runtime](media/data-factory-create-install-integration-runtime/register-integration-runtime.png)
2. Po úspěšném dokončení registrace místního prostředí Integration Runtime se zobrazí následující zpráva:

   ![Úspěšně zaregistrováno](media/data-factory-create-install-integration-runtime/registered-successfully.png)

3. Na stránce **Nový uzel prostředí Integration Runtime (v místním prostředí)** klikněte na **Další**. 

    ![Stránka Nový uzel prostředí Integration Runtime](media/data-factory-create-install-integration-runtime/new-integration-runtime-node-page.png)
4. Na stránce **Komunikační kanál intranetu** klikněte na **Přeskočit**. V prostředí Integration Runtime s více uzly můžete pro zabezpečení komunikace v rámci uzlů vybrat certifikaci TLS/SSL. 

    ![Stránka Komunikační kanál intranetu](media/data-factory-create-install-integration-runtime/intranet-communication-channel-page.png)
5. Na stránce **Registrace prostředí Integration Runtime (v místním prostředí)** klikněte na **Spustit Správce konfigurace**. 
6. Jakmile se uzel připojí ke cloudové službě, zobrazí se následující stránka:

   ![Uzel je připojen](media/data-factory-create-install-integration-runtime/node-is-connected.png)
7. Teď otestujte připojení k databázi SQL Serveru.

    ![Karta Diagnostika](media/data-factory-create-install-integration-runtime/config-manager-diagnostics-tab.png)   

    - V okně **Configuration Manager** přepněte na kartu **Diagnostika**.
    - V poli **Typ zdroje dat** vyberte **SqlServer**.
    - Zadejte název **serveru**.
    - Zadejte název **databáze**. 
    - Vyberte režim **ověřování**. 
    - Zadejte **uživatelské jméno**. 
    - Zadejte **heslo** pro toto uživatelské jméno.
    - Kliknutím na **Test** potvrďte, že Integration Runtime může připojit k SQL Serveru. Pokud je připojení úspěšné, zobrazí se zelená značka zaškrtnutí. V opačném případě se zobrazí chybová zpráva přidružená k tomuto selhání. Opravte všechny problémy a ověřte, že se Integration Runtime může připojit k SQL Serveru.    

    > [!NOTE]
    > Poznamenejte si tyto hodnoty (typ ověřování, server, databáze, uživatel, heslo). Použijete je později v tomto kurzu. 
    
