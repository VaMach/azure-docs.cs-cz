V předchozích krocích jste vytvořili prostředky Azure ve skupině prostředků. Pokud předpokládáte, že už tyto prostředky nebudete potřebovat, můžete je odstranit tak, že odstraníte skupinu prostředků.
 
1. Spusťte následující příkaz a ujistěte se, že skupina prostředků neobsahuje žádné prostředky, které chcete uložit:

  ```azurecli
  az group show --name myResourceGroup
  ```

2. Pokud chcete všechny zobrazené prostředky odstranit, spusťte následující příkaz:
 
  ```azurecli
  az group delete --name myResourceGroup
  ```
