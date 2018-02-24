### <a name="prepare-for-a-push-installation-on-a-linux-server"></a>Příprava nabízené instalace na serveru s Linuxem

1. Zkontrolujte, zda je síťové připojení mezi počítače se systémem Linux a procesového serveru.
2. Vytvořte účet, pomocí kterého bude procesový server moct přistupovat k počítači. Účet musí být na zdrojovém serveru s Linuxem uživatelem **root**. Pouze pro nabízené instalace a aktualizace, použijte tento účet.
3. Zkontrolujte, že soubor /etc/hosts na zdrojovém serveru s Linuxem obsahuje položky, které mapují místní název hostitele na IP adresy přidružené ke všem síťovým adaptérům.
4. Na počítači, který chcete replikovat, nainstalujte nejnovější balíčky openssh, openssh-server a openssl.
5. Ujistěte se, že je povolený Secure Shell (SSH) a že běží na portu 22.
6. Povolte ověřování pomocí protokolu SFTP subsystém a heslo v souboru sshd_config. Postupujte následovně:

    a. Přihlaste se jako uživatel **root**.

    b. V **/etc/ssh/sshd_config** souboru, najít řádek, který začíná **PasswordAuthentication**.

    c. Zrušením komentáře u řádku a změňte hodnotu na **Ano**.

    d. Najít řádek, který začíná **subsystému**, a zrušte komentář u řádku.

      ![Linux](./media/site-recovery-prepare-push-install-mob-svc-lin/mobility2.png)

    e. Restartujte službu **sshd**.

7. V nástroji CSPSConfigtool přidejte účet, který jste vytvořili. Postupujte následovně:

    a. Přihlaste se ke konfiguračnímu serveru.

    b. Otevřete **cspsconfigtool.exe**. Je k dispozici jako zástupce na ploše a ve složce %ProgramData%\home\svsystems\bin.

    c. Na **Správa účtů** vyberte **přidat účet**.

    d. Přidejte účet, který jste vytvořili.

    d. Zadejte přihlašovací údaje, které používáte při povolení replikace počítače.
