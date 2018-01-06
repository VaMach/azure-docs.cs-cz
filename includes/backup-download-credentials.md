## <a name="using-vault-credentials-to-authenticate-with-the-azure-backup-service"></a>Pomocí přihlašovacích údajů trezoru k ověření pomocí služby Azure Backup
Předtím, než místním serveru (Windows klienta nebo serveru Windows Server nebo Data Protection Manager) můžete zálohovat do Azure, ověření serveru s trezoru služeb zotavení. Použijte soubor s přihlašovacími údaji k ověření serveru s Azure. Přihlašovací údaje úložiště jsou podobná koncepci "publikovat nastavení" soubor určený v prostředí Azure PowerShell.

### <a name="what-is-the-vault-credential-file"></a>Co je soubor s přihlašovacími údaji trezoru?
Soubor s přihlašovacími údaji trezoru je certifikát vytvořený portál pro každý trezor záloh. Portál poté odešle veřejný klíč do Access Control Service (ACS). Privátní klíč certifikátu je k dispozici pro uživatele v rámci pracovního postupu, které je zadána jako vstup v pracovním postupu registrace počítače. To ověřuje počítač pro odesílání zálohovaných dat do identifikovaného trezoru ve službě Azure Backup.

Přihlašovací údajů úložiště se používají pouze během pracovního postupu registrace. Je zodpovědností uživatele zajistit, že není ohrožená soubor s přihlašovacími údaji. Padne-li soubor s přihlašovacími údaji do rukou neautorizovaného uživatele, může být použitý k registraci dalších počítačů ke stejnému trezoru. Jako zálohovaná data šifrovaná pomocí hesla, který patří k zákazníka, však nemůže být ohrožena stávajících zálohovaných dat. Pro snížení tohoto rizika, je nastaveno vypršení platnosti v 48hrs přihlašovací údaje trezoru. Si můžete stáhnout přihlašovací údaje trezoru úložiště záloh libovolný počet časy – ale během pracovního postupu registrace lze použít pouze nejnovější soubor přihlašovacích údajů trezoru.

### <a name="download-the-vault-credential-file"></a>Stáhněte si soubor s přihlašovacími údaji trezoru
Soubor s přihlašovacími údaji trezoru je stažen prostřednictvím zabezpečeného kanálu z portálu Azure. Služba Azure Backup nezaznamená privátní klíč certifikátu, a privátní klíč není trvalý v portálu nebo službu. Pomocí následujících kroků stáhněte soubor s přihlašovacími údaji trezoru do místního počítače.

1. Otevřete [portálu Azure](https://ms.azure.portal.com/)
2. V levé nabídce vyberte **všechny služby** a v seznamu služeb zadejte **služeb zotavení**. Klikněte na tlačítko **trezory služeb zotavení**.

   ![Otevřený trezor služeb zotavení](../articles/backup/media/tutorial-backup-windows-server-to-azure/full-browser-open-rs-vault_2.png)
3. Na stránce Rychlý Start klikněte na **přihlašovací údaje trezoru Stáhnout**. Na portálu vygeneruje soubor pověření pro úložiště, který je k dispozici ke stažení.
   
   ![Ke stažení](./media/backup-download-credentials/downloadvc.png)
4. Na portálu vygeneruje přihlašovací údaje úložiště pomocí kombinace název trezoru a aktuální datum. Klikněte na tlačítko **Uložit** stáhnout přihlašovací údaje úložiště do složky se staženými soubory místní účet, nebo vyberte možnost Uložit jako v nabídce Uložit a zadejte umístění pro přihlašovací údaje trezoru.

### <a name="note"></a>Poznámka
* Ujistěte se, že přihlašovací údaje trezoru je uložen v umístění, které jsou přístupné z počítače. Pokud je uložený v souboru sdílené složky nebo SMB, zkontrolujte oprávnění k přístupu.
* Soubor s přihlašovacími údaji se používají pouze během pracovního postupu registrace.
* Soubor s přihlašovacími údaji trezoru vyprší po 48hrs a lze stáhnout z portálu.
* Ke službě Azure Backup naleznete [– nejčastější dotazy](../articles/backup/backup-azure-backup-faq.md) pro všechny dotazy na pracovní postup.

