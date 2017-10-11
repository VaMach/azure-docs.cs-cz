## <a name="download-install-and-register-the-azure-backup-agent"></a>Stažení, instalace a registrace agenta Azure Backup
Po vytvoření trezoru zálohování Azure, je třeba nainstalovat agenta na všechny vaše Windows počítače (Windows Server, klient systému Windows, server System Center Data Protection Manager nebo počítače serveru Azure Backup), které povoluje zálohování dat a aplikací do Azure.

1. Přihlaste se k [portálu pro správu](https://manage.windowsazure.com/)
2. Klikněte na tlačítko **služeb zotavení**, zvolte úložiště záloh, které chcete zaregistrovat serveru. Zobrazí se stránka rychlý Start pro tento trezor záloh.
   
    ![Rychlý start](./media/backup-install-agent/quickstart.png)
3. Na stránce Rychlý Start klikněte na **klienta pro systém Windows Server nebo System Center Data Protection Manager nebo Windows** možnost pod **stáhnout agenta**. Klikněte na tlačítko **Uložit** a zkopírujte ho do místního počítače.
   
    ![Uložit agenta](./media/backup-install-agent/agent.png)
4. Po instalaci agenta, poklikejte na MARSAgentInstaller.exe spustíte instalaci agenta Azure Backup agent. Vyberte instalační složku a pomocné složky, které jsou potřebné pro agenta. Zadané umístění mezipaměti musí mít volné místo, který je nejméně 5 % zálohovaná data.
5. Pokud používáte proxy server pro připojení k Internetu, v **konfiguraci proxy serveru** obrazovky, zadejte podrobnosti o proxy serveru. Pokud používáte ověřený server proxy, zadejte uživatelské jméno a heslo podrobnosti na této obrazovce.
6. Agent Azure Backup nainstaluje rozhraní .NET Framework 4.5 a prostředí Windows PowerShell (Pokud není k dispozici již) pro dokončení instalace.
7. Po instalaci agenta, klikněte na **přejít k registraci** tlačítko pokračujte s pracovním postupem.
   
   ![Registrace](./media/backup-install-agent/register.png)
8. Na obrazovce přihlašovací údaje trezoru vyhledejte a vyberte soubor s přihlašovacími údaji trezoru, který byl dříve staženy.
   
    ![Přihlašovací údaje trezoru](./media/backup-install-agent/vc.png)
   
    Soubor s přihlašovacími údaji je platná pouze pro 48 hodin (po jeho stažení z portálu). Pokud dojde k chybě v této plánováním obrazovce (například "soubor s přihlašovacími údaji poskytuje vypršela"), přihlášení k portálu Azure a znovu stáhnout soubor s přihlašovacími údaji.
   
    Ujistěte se, že je soubor s přihlašovacími údaji k dispozici v umístění, která je přístupná instalační program aplikace. Pokud narazíte na přístup související chyby, zkopírujte soubor s přihlašovacími údaji do dočasného umístění v tomto počítači a operaci opakujte.
   
    Pokud dojde k chybě neplatný úložiště přihlašovacích údajů (např. "Neplatné přihlašovací údaje úložiště") soubor je buď poškozený nebo nemá mít poslední přihlašovací údaje související se službou obnovení. Opakujte operaci po stažení nový soubor s přihlašovacími údaji trezoru z portálu. Tato chyba je zpravidla se zobrazí, pokud uživatel klikne na **přihlašovací údaje trezoru Stáhnout** možnost na portálu Azure rychle po sobě. V takovém případě je platný pouze druhý soubor přihlašovacích údajů trezoru.
9. V **nastavení šifrování** obrazovky, můžete buď vygenerovat přístupové heslo nebo zadat přístupové heslo (minimálně 16 znaků). Nezapomeňte uložit heslo v zabezpečeném umístění.
   
    ![Šifrování](./media/backup-install-agent/encryption.png)
   
   > [!WARNING]
   > Pokud heslo ztratíte nebo zapomenete; Microsoft vám nemůže pomoci obnovit zálohovaná data. Koncový uživatel vlastní šifrovací přístupové heslo a Microsoft nemá přehled přístupové heslo používané koncovým uživatelem. Uložte soubor na bezpečné místo, jako je vyžadována během operace obnovení.
   > 
   > 
10. Po kliknutí **Dokončit** tlačítko, počítač se úspěšně registrovaný k úložišti a je nyní připraven pro spuštění zálohování na Microsoft Azure.
11. Při použití samostatných Microsoft Azure Backup můžete upravit nastavení určené během pracovního postupu registrace kliknutím na **změnit vlastnosti** možnost v Azure Backup konzoly mmc modul snap-in.
    
    ![Změnit vlastnosti](./media/backup-install-agent/change.png)
    
    Případně, pokud používáte Data Protection Manager, můžete upravit nastavení určené během pracovního postupu registrace kliknutím **konfigurace** možnost výběrem **Online** pod **Správu** kartě.
    
    ![Konfigurace služby Azure Backup](./media/backup-install-agent/configure.png)

