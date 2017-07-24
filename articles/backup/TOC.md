
# Přehled
## [Co je Azure Backup?](backup-introduction-to-azure-backup.md)

# Začínáme
## [Zálohování virtuálních počítačů Azure](backup-azure-vms-first-look-arm.md)
## [Zálohování souborů a složek](backup-try-azure-backup-in-10-mins.md)
## [Zálohování serverů VMware](backup-azure-backup-server-vmware.md)

# Postup
## Virtuální počítače Azure
### Příprava virtuálního počítače
#### [Příprava virtuálních počítačů nasazených Resource Managerem](backup-azure-arm-vms-prepare.md)
#### [Příprava virtuálních počítačů Azure](backup-azure-vms-prepare.md)
### Plánování prostředí
#### [Plánování infrastruktury zálohování virtuálních počítačů](backup-azure-vms-introduction.md)
### Zálohování virtuálních počítačů
#### [Zálohování virtuálních počítačů Azure do trezoru služby Recovery Services](backup-azure-arm-vms.md)
#### [Zálohování šifrovaných virtuálních počítačů](backup-azure-vms-encryption.md)
#### [Zálohování virtuálních počítačů Azure do úložiště záloh](backup-azure-vms.md)
### Správa a monitorování virtuálních počítačů
#### [Správa záloh virtuálních počítačů Azure na webu Azure Portal](backup-azure-manage-vms.md)
#### [Monitorování výstrah pro zálohy virtuálních počítačů Azure na webu Azure Portal](backup-azure-monitor-vms.md)
#### [Správa a sledování záloh virtuálních Počítačů Azure na klasickém portálu](backup-azure-manage-vms-classic.md)
### Obnovení dat z virtuálních počítačů
#### [Obnovení souborů ze záloh virtuálních počítačů Azure](backup-azure-restore-files-from-vm.md)
#### [Obnovení virtuálních počítačů nasazených Azure Resource Managerem na webu Azure Portal](backup-azure-arm-restore-vms.md)
#### [Obnovení šifrovaných virtuálních počítačů](backup-azure-vms-encryption.md)
#### [Obnovení virtuálních počítačů v Azure](backup-azure-restore-vms.md)
#### [Obnovení klíče a tajného kódu Key Vault pro šifrované virtuální počítače pomocí Azure Backup](backup-azure-restore-key-secret.md)


## Soubory a složky ve Windows
### [Windows Server s využitím modelu nasazení Resource Manageru](backup-configure-vault.md)
### [Windows Server s využitím klasického modelu nasazení](backup-configure-vault-classic.md)
### [Monitorování a správa trezorů služby Recovery Services](backup-azure-manage-windows-server.md)
### [Správa trezorů služby Backup s využitím klasického modelu nasazení](backup-azure-manage-windows-server-classic.md)
### [Obnovení souborů na Windows Serveru s využitím modelu nasazení Resource Manager](backup-azure-restore-windows-server.md)
### [Obnovení souborů na Windows Serveru s využitím klasického modelu nasazení](backup-azure-restore-windows-server-classic.md)

## Server Azure Backup
### [Systém ochrany Azure Backup Serveru](backup-mabs-protection-matrix.md)
### Instalace nebo upgrade
#### [Příprava úloh Azure Backup Serveru na webu Azure Portal](backup-azure-microsoft-azure-backup.md)
#### [Příprava úloh Azure Backup Serveru na portálu Azure Classic](backup-azure-microsoft-azure-backup-classic.md)
#### [Přidání úložiště do Azure Backup Serveru](backup-mabs-add-storage.md)
#### [Upgrade Azure Backup Serveru na v.2](backup-mabs-upgrade-to-v2.md)
#### [Bezobslužná instalace Azure Backup Serveru](backup-mabs-unattended-install.md)
### Ochrana úloh
#### [Použití Azure Backup Serveru k zálohování serveru VMware](backup-azure-backup-server-vmware.md)
#### [Použití Azure Backup Serveru k zálohování Exchange](backup-azure-exchange-mabs.md)
#### [Použití Azure Backup Serveru k zálohování sharepointové farmy](backup-azure-backup-sharepoint-mabs.md)
#### [Použití Azure Backup Serveru k zálohování SQL](backup-azure-sql-mabs.md)
#### [Ochrana stavu systému a úplné obnovení systému](backup-mabs-system-state-and-bmr.md)

## Data Protection Manager
### [Příprava úloh DPM na webu Azure Portal](backup-azure-dpm-introduction.md)
### [Příprava úloh DPM na portálu Azure Classic](backup-azure-dpm-introduction-classic.md)
### [Použití nástroje System Center DPM k zálohování serveru Exchange](backup-azure-backup-exchange-server.md)
### [Obnovení dat v úložišti záloh na alternativní server DPM](backup-azure-alternate-dpm-server.md)
### [Použití DPM k zálohování úloh SQL Serveru](backup-azure-backup-sql.md)
### [Použití DPM k zálohování sharepointové farmy](backup-azure-backup-sharepoint.md)

## Azure SQL Database
### [Konfigurace dlouhodobého uchovávání záloh](../sql-database/sql-database-configure-long-term-retention.md?toc=%2fazure%2fbackup%2ftoc.json)
### [Zobrazení záloh v trezoru služby Recovery Services](../sql-database/sql-database-view-backups-in-vault.md?toc=%2fazure%2fbackup%2ftoc.json)
### [Obnovení z dlouhodobě uchovávaných záloh](../sql-database/sql-database-restore-from-long-term-retention.md?toc=%2fazure%2fbackup%2ftoc.json)
### [Odstranění dlouhodobých záloh Azure SQL](../sql-database/sql-database-long-term-retention-delete.md?toc=%2fazure%2fbackup%2ftoc.json)

## Použití prostředí PowerShell
### [Virtuální počítače Azure na webu Azure Portal](backup-azure-vms-automation.md)
### [Virtuální počítače Azure na portálu Azure Classic](backup-azure-vms-classic-automation.md)
### [DPM na webu Azure Portal](backup-dpm-automation.md)
### [DPM na portálu Azure Classic](backup-dpm-automation-classic.md)
### [Windows Server na webu Azure Portal](backup-client-automation.md)
### [Windows Server na portálu Azure Classic](backup-client-automation-classic.md)

## Nejčastější dotazy
### [Nejčastější dotazy k trezorům služby Recovery Services](backup-azure-backup-faq.md)
### [Nejčastější dotazy k zálohování virtuálních počítačů Azure](backup-azure-vm-backup-faq.md)
### [Nejčastější dotazy k zálohování souborů a složek pomocí agenta Azure Backup](backup-azure-file-folder-backup-faq.md)

## Řešení potíží
### [Potíže se zálohami virtuálních počítačů Azure na webu Azure Portal](backup-azure-vms-troubleshoot.md)
### [Potíže se zálohami virtuálních počítačů Azure na portálu Azure Classic](backup-azure-vms-troubleshoot-classic.md)
### [Selhání zálohování virtuálních počítačů Azure: S agentem virtuálního počítače nebylo možné komunikovat – Dílčí úloze snímku virtuálního počítače vypršel časový limit](backup-azure-troubleshoot-vm-backup-fails-snapshot-timeout.md)
### [Pomalé zálohování souborů a složek ve službě Azure Backup](backup-azure-troubleshoot-slow-backup-performance-issue.md)
### [Odstraňování potíží Azure Backup Serveru](backup-azure-mabs-troubleshoot.md)

# Koncepty
## [Přehled trezorů služby Recovery Services](backup-azure-recovery-services-vault-overview.md)
## [Upgrade úložiště záloh do trezoru služby Recovery Services](backup-azure-upgrade-backup-to-recovery-services.md)
## [Odstranění trezoru služby Azure Backup](backup-azure-delete-vault.md)
## [Řízení přístupu na základě rolí](backup-rbac-rs-vault.md)
## [Zabezpečení pro hybridní zálohy](backup-azure-security-feature.md)
## [Konfigurace sestav Azure Backup](backup-azure-configure-reports.md)
## [Datový model pro sestavy Azure Backup](backup-azure-reports-data-model.md)
## [Konfigurace zálohování offline](backup-azure-backup-import-export.md)
## [Náhrada páskové knihovny](backup-azure-backup-cloud-as-tape.md)
## [Aplikačně konzistentní zálohování virtuálních počítačů s Linuxem](backup-azure-linux-app-consistent.md)

# Referenční informace
## [PowerShell](/powershell/module/azurerm.recoveryservices.backup)
## [.NET](/dotnet/api/microsoft.azure.management.recoveryservices.backup)

# Zdroje a prostředky
## [Plány Azure do budoucna](https://azure.microsoft.com/roadmap/)
## [Fórum MSDN](https://social.msdn.microsoft.com/Forums/en-US/home?forum=windowsazureonlinebackup)
## [Ceny](https://azure.microsoft.com/pricing/details/backup/)
## [Aktualizace služeb](https://azure.microsoft.com/updates/?product=backup)
## [Videa](https://azure.microsoft.com/documentation/videos/index/?services=backup)
