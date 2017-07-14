Nakonfigurujte nasazení do aplikace API pomocí místního Gitu – pomocí příkazu [az webapp source-control config-local-git](/cli/azure/appservice/web/source-control#config-local-git).   

App Service podporuje řadu způsobů nasazení obsahu do webové aplikace, jako například FTP, místní Git, GitHub, Visual Studio Team Services nebo Bitbucket. Pro účely tohoto rychlého zprovoznění provedete nasazení pomocí místního Gitu. To znamená, že nasazení provedete použitím příkazu Gitu, který přenese obsah z místního úložiště do úložiště v Azure.  

Pomocí skriptu níže nastavíte přihlašovací údaje pro nasazení na úrovni účtu, které budete používat při vkládání kódu. Nezapomeňte použít vlastní hodnoty uživatelského jména a hesla.   

```azurecli-interactive
az webapp deployment user set --user-name <desired user name> --password <desired password>
```
