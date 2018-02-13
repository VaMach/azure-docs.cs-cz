## <a name="rest"></a>Nasazení pomocí rozhraní REST API 
 
Můžete použít [službu pro nasazení rozhraní REST API](https://github.com/projectkudu/kudu/wiki/REST-API) k nasazení souboru ZIP do vaší aplikace v Azure. Https://<app_name>.scm.azurewebsites.net/api/zipdeploy právě odešlete požadavek POST. Požadavek POST musí obsahovat soubor .zip v textu zprávy. Přihlašovací údaje nasazení pro vaši aplikaci jsou uvedeny v požadavku pomocí ověřování HTTP BASIC. Další informace najdete v tématu [odkaz nasazení nabízené .zip](https://github.com/projectkudu/kudu/wiki/Deploying-from-a-zip-file). 

Následující příklad používá nástroj cURL k odeslání požadavku, která obsahuje soubor .zip. V počítači Mac nebo Linux nebo pomocí Bash ve Windows můžete spustit cURL z terminálu. Nahraďte `<zip_file_path>` zástupný symbol cestu k umístění souboru .zip projektu. Také nahraďte `<app_name>` s jedinečným názvem aplikace.

Nahraďte `<deployment_user>` zástupný symbol uživatelské jméno přihlašovací údaje nasazení. Po zobrazení výzvy cURL, zadejte heslo. Zjistěte, jak nastavit přihlašovací údaje nasazení pro aplikace, najdete v tématu [nastavit a resetovat přihlašovací údaje individuální](../articles/app-service/app-service-deployment-credentials.md#userscope).   

```bash
curl -X POST -u <deployment_user> --data-binary @"<zip_file_path>" https://<app_name>.scm.azurewebsites.net/api/zipdeploy
```

Tento požadavek se aktivuje nabízené nasazení z odeslaného souboru ZIP. Aktuální a starší nasazení můžete zkontrolovat pomocí https://<app_name>.scm.azurewebsites.net/api/deployments koncového bodu, jak je znázorněno v následujícím příkladu cURL. Znovu, nahraďte `<app_name>` s názvem vaší aplikace a `<deployment_user>` uživatelským jménem přihlašovací údaje nasazení.

```bash
curl -u <deployment_user> https://<app_name>.scm.azurewebsites.net/api/deployments
```