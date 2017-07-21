Vytvořte přihlašovací údaje nasazení pomocí příkazu [az webapp deployment user set](/cli/azure/webapp/deployment/user#set).

Uživatel nasazení je vyžadován pro nasazení pomocí FTP a místního Gitu do webové aplikace. Uživatelské jméno a heslo jsou na úrovni účtu. To znamená, že se liší od přihlašovacích údajů předplatného Azure.

V následujícím příkazu nahraďte hodnoty *\<user-name>* a *\<password>* novým uživatelským jménem a heslem.

```azurecli-interactive
az webapp deployment user set --user-name <username> --password <password>
```

Uživatelské jméno musí být jedinečné. Heslo musí obsahovat aspoň osm znaků a musí v něm být použity minimálně dva z následujících typů znaků: písmena, čísla a symboly. Pokud se zobrazí chyba ` 'Conflict'. Details: 409`, změňte uživatelské jméno. Pokud se zobrazí chyba ` 'Bad Request'. Details: 400`, použijte silnější heslo.

Tohoto uživatele nasazení je potřeba vytvořit jenom jednou a potom ho můžete použít pro všechna nasazení v Azure.

> [!NOTE]
> Uživatelské jméno a heslo si poznamenejte. Později je budete potřebovat k nasazení webové aplikace.
>
>