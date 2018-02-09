V okně místního terminálu přidejte vzdálené úložiště Azure do místního úložiště Gitu. Nahraďte _&lt;paste\_copied\_url\_here>_ adresou URL vzdáleného úložiště, kterou jste uložili při [Vytvoření webové aplikace](#create).

```bash
git remote add azure <deploymentLocalGitUrl-from-create-step>
```

Nasaďte aplikaci do vzdáleného úložiště Azure pomocí následujícího příkazu. Po zobrazení výzvy k zadání hesla se ujistěte, že zadáváte heslo, které jste vytvořili v části [Konfigurace uživatele nasazení](#configure-a-deployment-user), ne heslo, se kterým se přihlašujete na web Azure Portal.

```bash
git push azure master
```

Spuštění tohoto příkazu může trvat několik minut. Při spuštění příkaz zobrazí podobné informace jako v následujícím příkladu:
