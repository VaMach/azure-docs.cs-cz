### <a name="prepare-for-a-push-installation-on-a-windows-computer"></a>Příprava pro nabízenou instalaci v počítači se systémem Windows

1. Zkontrolujte, zda je síťové připojení mezi počítači s Windows a procesového serveru.
2. Vytvořte účet, pomocí kterého bude procesový server moct přistupovat k počítači. Účet musí mít oprávnění správce (místní nebo doménový). (Pouze pro nabízené instalace a aktualizace agenta, použijte tento účet.)

   > [!NOTE]
   > Pokud nepoužíváte účet domény, zakažte řízení vzdáleného přístupu uživatele v místním počítači. Chcete-li zakázat řízení vzdáleného přístupu uživatele, v klíči registru HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion\Policies\System přidat novou hodnotu DWORD: **LocalAccountTokenFilterPolicy**. Nastavte hodnotu na **1**. K tomu na příkazovém řádku spusťte následující příkaz:  
   `REG ADD HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion\Policies\System /v LocalAccountTokenFilterPolicy /t REG_DWORD /d 1`
   >
   >
2. V bráně Windows Firewall v počítači, který chcete chránit, vyberte **povolit aplikace nebo funkci průchod bránou Firewall**. Povolit **sdílení souborů a tiskáren** a **Windows Management Instrumentation (WMI)**. Pro počítače, které patří k doméně můžete nakonfigurovat nastavení brány firewall pomocí objektu zásad skupiny (GPO).

   ![Nastavení brány firewall](./media/site-recovery-prepare-push-install-mob-svc-win/mobility1.png)

3. V nástroji CSPSConfigtool přidejte účet, který jste vytvořili.
    1.  Přihlaste se ke konfiguračnímu serveru.
    2.  Otevřete **cspsconfigtool.exe**. (Je k dispozici jako zástupce na ploše a ve složce %ProgramData%\home\svsystems\bin.)
    3.  Na **Správa účtů** vyberte **přidat účet**.
    4.  Přidejte účet, který jste vytvořili.
    5.  Zadejte přihlašovací údaje, které používáte při povolení replikace počítače.
