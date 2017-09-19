V okně místního terminálu přidejte vzdálené úložiště Azure do místního úložiště Gitu.

```bash
git remote add azure <URI from previous step>
```

Nasaďte aplikaci do vzdáleného úložiště Azure pomocí následujícího příkazu. Po zobrazení výzvy k zadání hesla se ujistěte, že zadáváte heslo, které jste vytvořili v části [Konfigurace uživatele nasazení](#configure-a-deployment-user), ne heslo, se kterým se přihlašujete na web Azure Portal.

```bash
git push azure master
```

Předchozí příkaz zobrazí podobné informace jako v následujícím příkladu:
