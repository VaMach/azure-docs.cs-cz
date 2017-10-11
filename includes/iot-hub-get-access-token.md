## <a name="obtain-an-azure-resource-manager-token"></a>Získání tokenu Azure Resource Manager
Azure Active Directory musí ověřovat všechny úlohy, které můžete provádět na prostředků pomocí Azure Resource Manager. Z příkladu používá ověřování hesla, naleznete v dalších přístupy [požadavků ověřování Azure Resource Manager][lnk-authenticate-arm].

1. Přidejte následující kód, který **hlavní** metoda do souboru Program.cs k načtení tokenu z Azure AD pomocí id aplikace a hesla.
   
    ```
    var authContext = new AuthenticationContext(string.Format  
      ("https://login.microsoftonline.com/{0}", tenantId));
    var credential = new ClientCredential(applicationId, password);
    AuthenticationResult token = authContext.AcquireTokenAsync
      ("https://management.core.windows.net/", credential).Result;
   
    if (token == null)
    {
      Console.WriteLine("Failed to obtain the token");
      return;
    }
    ```
2. Vytvoření **ResourceManagementClient** objekt, který používá token přidáním následující kód do konce **hlavní** metoda:
   
    ```
    var creds = new TokenCredentials(token.AccessToken);
    var client = new ResourceManagementClient(creds);
    client.SubscriptionId = subscriptionId;
    ```
3. Vytvořit nebo získat odkaz na skupinu prostředků, kterou používáte:
   
    ```
    var rgResponse = client.ResourceGroups.CreateOrUpdate(rgName,
        new ResourceGroup("East US"));
    if (rgResponse.Properties.ProvisioningState != "Succeeded")
    {
      Console.WriteLine("Problem creating resource group");
      return;
    }
    ```

[lnk-authenticate-arm]: https://msdn.microsoft.com/library/azure/dn790557.aspx