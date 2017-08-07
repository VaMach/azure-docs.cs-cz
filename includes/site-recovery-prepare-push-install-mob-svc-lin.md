### <a name="prepare-for-a-push-installation-on-a-linux-server"></a>Příprava nabízené instalace na serveru s Linuxem

1. Zkontrolujte, že existuje síťové připojení mezi počítačem s Linuxem a procesovým serverem.
2. Vytvořte účet, pomocí kterého bude procesový server moct přistupovat k počítači. Účet musí být na zdrojovém serveru s Linuxem uživatelem **root**. (Používejte tento účet pouze pro nabízenou instalaci a aktualizace.)
3. Zkontrolujte, že soubor /etc/hosts na zdrojovém serveru s Linuxem obsahuje položky, které mapují místní název hostitele na IP adresy přidružené ke všem síťovým adaptérům.
4. Na počítači, který chcete replikovat, nainstalujte nejnovější balíčky openssh, openssh-server a openssl.
5. Ujistěte se, že je povolený Secure Shell (SSH) a že běží na portu 22.
6. V souboru sshd_config povolte subsystém SFTP a ověřování heslem:
  1.  Přihlaste se jako uživatel **root**.
  2.  V souboru /etc/ssh/sshd_config vyhledejte řádek, který začíná na **PasswordAuthentication**.
  3.  Zrušte na řádku komentář a změňte hodnotu na **yes**.
  4.  Vyhledejte řádek, který začíná na **Subsystem** a zrušte na něm komentář.

     ![Linux](./media/site-recovery-prepare-push-install-mob-svc-lin/mobility2.png)
  5. Restartujte službu **sshd**.

7. V nástroji CSPSConfigtool přidejte účet, který jste vytvořili.
    1.  Přihlaste se ke konfiguračnímu serveru.
    2.  Otevřete **cspsconfigtool.exe**. (Je k dispozici jako zástupce na ploše a ve složce %ProgramData%\home\svsystems\bin.)
    3.  Na kartě **Správa účtů** klikněte na **Přidat účet**.
    4.  Přidejte účet, který jste vytvořili. 
    5.  Zadejte přihlašovací údaje, které používáte při povolení replikace počítače.
