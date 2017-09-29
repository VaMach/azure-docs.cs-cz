Ve službě Cloud Shell vytvořte přihlašovací údaje nasazení pomocí příkazu [az webapp deployment user set](/cli/azure/webapp/deployment/user#set).

Uživatel nasazení je vyžadován pro nasazení pomocí FTP a místního Gitu do webové aplikace. Uživatelské jméno a heslo jsou na úrovni účtu. _To znamená, že se liší od přihlašovacích údajů předplatného Azure._

V následujícím příkazu nahraďte hodnoty *\<username>* a *\<password>* novým uživatelským jménem a heslem. Uživatelské jméno musí být jedinečné. Heslo musí obsahovat alespoň osm znaků a musí v něm být použity minimálně dva z následujících typů znaků: písmena, číslice a symboly. 

```azurecli-interactive
az webapp deployment user set --user-name <username> --password <password>
```

Pokud se zobrazí chyba ` 'Conflict'. Details: 409`, změňte uživatelské jméno. Pokud se zobrazí chyba ` 'Bad Request'. Details: 400`, použijte silnější heslo.

Tohoto uživatele nasazení vytvoříte jenom jednou a potom ho můžete použít pro všechna nasazení v Azure.

> [!NOTE]
> Uživatelské jméno a heslo si poznamenejte. Později je budete potřebovat k nasazení webové aplikace.
>
>