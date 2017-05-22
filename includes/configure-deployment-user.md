## <a name="configure-a-deployment-user"></a>Konfigurace uživatele nasazení  

Pro FTP a místní Git je potřeba mít na serveru nakonfigurovaného uživatele nasazení, aby bylo možné nasazení ověřit.

> [!NOTE]
> Uživatel nasazení je vyžadován pro nasazení pomocí FTP a místního Gitu do webové aplikace.
> Položky `username` a `password` jsou na úrovni účtu. To znamená, že se liší od přihlašovacích údajů předplatného Azure.
>

Pomocí příkazu [az appservice web deployment user set](/cli/azure/appservice/web/deployment/user#set) vytvořte své přihlašovací údaje pro nasazení.

```azurecli
az appservice web deployment user set --user-name <username> --password <password>
```

Uživatelské jméno musí být jedinečné a je potřeba zadat silné heslo. Pokud se zobrazí chyba ` 'Conflict'. Details: 409`, změňte uživatelské jméno. Pokud se zobrazí chyba ` 'Bad Request'. Details: 400`, použijte silnější heslo.

Tohoto uživatele nasazení je potřeba vytvořit jenom jednou a potom ho můžete použít pro všechna nasazení v Azure.

Uživatelské jméno a heslo si poznamenejte, budete je totiž potřebovat později při nasazování aplikace.