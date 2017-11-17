V okně místního terminálu přidejte vzdálené úložiště Azure do místního úložiště Gitu. Tato Azure vzdálené byl vytvořen pro vás v [vytvořit webovou aplikaci](#create-a-web-app).

```bash
git remote add azure <deploymentLocalGitUrl-from-create-step>
```

Nasaďte aplikaci do vzdáleného úložiště Azure pomocí následujícího příkazu. Po zobrazení výzvy k zadání hesla se ujistěte, že zadáváte heslo, které jste vytvořili v části [Konfigurace uživatele nasazení](#configure-a-deployment-user), ne heslo, se kterým se přihlašujete na web Azure Portal.

```bash
git push azure master
```

Tento příkaz může trvat několik minut. Při spuštění, zobrazuje informace podobně jako v následujícím příkladu:
