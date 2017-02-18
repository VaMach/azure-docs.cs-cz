1. Připojte se k virtuálnímu počítači procesového serveru pomocí Připojení ke vzdálené ploše.
2. Po přihlášení se automaticky spustí konfigurační nástroj procesového serveru a bude vyžadovat zadání následujících údajů:
  * Plně kvalifikovaný název (FQDN) nebo IP adresa konfiguračního serveru.
  * Port, na kterém konfigurační server naslouchá. Hodnota by měla být 443.
  * Přístupové heslo připojení pro připojení ke konfiguračnímu serveru.
  * Port pro přenos dat, který se má pro tento procesový server nakonfigurovat. Ponechte výchozí hodnotu, pokud jste nezměnili číslo portu ve svém prostředí.

    ![Registrace procesového serveru](./media/site-recovery-vmware-register-process-server/register-ps.png)
3. Kliknutím na tlačítko Uložit uložte konfiguraci.
4. Po dokončení registrace se procesový server začne zobrazovat pod vaším konfiguračním serverem a můžete jej použít k navrácení služeb po obnovení.

> [!TIP]
> Konfigurační nástroj procesového serveru spustíte dvojitým kliknutím na zástupce **cspsconfigtool**, který je k dispozici na ploše virtuálního počítače.


<!--HONumber=Feb17_HO1-->


