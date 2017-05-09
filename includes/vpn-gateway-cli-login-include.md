Přihlaste se k předplatnému Azure pomocí příkazu [az login](/cli/azure/#login) a postupujte podle pokynů na obrazovce. Další informace o přihlašování najdete v tématu [Začínáme s Azure CLI 2.0](/cli/azure/get-started-with-azure-cli).

```azurecli
az login
```

Pokud máte více než jedno předplatné Azure, vypište předplatná pro daný účet.

```azurecli
az account list --all
```

Určete předplatné, které chcete použít.

```azurecli
az account set --subscription <replace_with_your_subscription_id>
```

