## <a name="push-to-azure-from-git"></a>Přenos z Gitu do Azure

Přidejte vzdálené úložiště Azure do místního úložiště Gitu.

```bash
git remote add azure <URI from previous step>
```

Nasaďte aplikaci do vzdáleného úložiště Azure. Zobrazí se výzva k zadání hesla, které jste zadali dříve při vytváření uživatele nasazení. Dejte pozor na to, abyste zadali heslo, které jste vytvořili v kroku [Konfigurace uživatele nasazení](#configure-a-deployment-user), ne heslo, se kterým se přihlašujete na web Azure Portal.

```bash
git push azure master
```

Předchozí příkaz zobrazí podobné informace jako v následujícím příkladu:
