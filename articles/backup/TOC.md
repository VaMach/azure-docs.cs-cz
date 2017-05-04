
# Přehled
## [Co je Azure Backup?](backup-introduction-to-azure-backup.md)

# Začínáme
## [Zálohování souborů a složek](backup-try-azure-backup-in-10-mins.md)
## [Zálohování virtuálních počítačů Azure](backup-azure-vms-first-look.md)
## [Ochrana virtuálních počítačů Azure](backup-azure-vms-first-look-arm.md)

# Postup
## Použití prostředí PowerShell
### [Virtuální počítače Azure na webu Azure Portal](backup-azure-vms-automation.md)
### [Virtuální počítače Azure na portálu Azure Classic](backup-azure-vms-classic-automation.md)
### [DPM na webu Azure Portal](backup-dpm-automation.md)
### [DPM na portálu Azure Classic](backup-dpm-automation-classic.md)
### [Windows Server na webu Azure Portal](backup-client-automation.md)
### [Windows Server na portálu Azure Classic](backup-client-automation-classic.md)

## Server Azure Backup
### [Příprava úloh Azure Backup Serveru na webu Azure Portal](backup-azure-microsoft-azure-backup.md)
### [Příprava úloh Azure Backup Serveru na portálu Azure Classic](backup-azure-microsoft-azure-backup-classic.md)
### [Použití Azure Backup Serveru k zálohování serveru VMware](backup-azure-backup-server-vmware.md)
### [Použití Azure Backup Serveru k zálohování Exchange](backup-azure-exchange-mabs.md)
### [Použití Azure Backup Serveru k zálohování sharepointové farmy](backup-azure-backup-sharepoint-mabs.md)
### [Použití Azure Backup Serveru k zálohování SQL](backup-azure-sql-mabs.md)

## Data Protection Manager
### [Příprava úloh DPM na webu Azure Portal](backup-azure-dpm-introduction.md)
### [Příprava úloh DPM na portálu Azure Classic](backup-azure-dpm-introduction-classic.md)
### [Použití nástroje System Center DPM k zálohování serveru Exchange](backup-azure-backup-exchange-server.md)
### [Obnovení dat v úložišti záloh na alternativní server DPM](backup-azure-alternate-dpm-server.md)
### [Použití DPM k zálohování úloh SQL Serveru](backup-azure-backup-sql.md)
### [Použití DPM k zálohování sharepointové farmy](backup-azure-backup-sharepoint.md)

## Virtuální počítače Azure
### [Příprava virtuálních počítačů Azure](backup-azure-vms-prepare.md)
### [Příprava virtuálních počítačů nasazených Resource Managerem](backup-azure-arm-vms-prepare.md)
### [Plánování infrastruktury zálohování virtuálních počítačů](backup-azure-vms-introduction.md)
### [Zálohování virtuálních počítačů Azure do úložiště záloh](backup-azure-vms.md)
### [Zálohování virtuálních počítačů Azure do trezoru služby Recovery Services](backup-azure-arm-vms.md)
### [Zálohování a obnovení šifrovaných virtuálních počítačů](backup-azure-vms-encryption.md)
### [Správa a sledování záloh virtuálních Počítačů Azure na klasickém portálu](backup-azure-manage-vms-classic.md)
### [Správa záloh virtuálních počítačů Azure na webu Azure Portal](backup-azure-manage-vms.md)
### [Monitorování výstrah pro zálohy virtuálních počítačů Azure na webu Azure Portal](backup-azure-monitor-vms.md)
### [Obnovení souborů ze záloh virtuálních počítačů Azure](backup-azure-restore-files-from-vm.md)
### [Obnovení virtuálních počítačů v Azure](backup-azure-restore-vms.md)
### [Obnovení virtuálních počítačů nasazených Azure Resource Managerem na webu Azure Portal](backup-azure-arm-restore-vms.md)
### [Obnovení klíče a tajného kódu Key Vault pro šifrované virtuální počítače pomocí Azure Backup](backup-azure-restore-key-secret.md)

## Azure SQL Database
### [Konfigurace dlouhodobého uchovávání záloh](../sql-database/sql-database-configure-long-term-retention.md?toc=%2fazure%2fbackup%2ftoc.json)
### [Zobrazení záloh v trezoru služby Recovery Services](../sql-database/sql-database-view-backups-in-vault.md?toc=%2fazure%2fbackup%2ftoc.json)
### [Obnovení z dlouhodobě uchovávaných záloh](../sql-database/sql-database-restore-from-long-term-retention.md?toc=%2fazure%2fbackup%2ftoc.json)
### [Odstranění dlouhodobých záloh Azure SQL](../sql-database/sql-database-long-term-retention-delete.md?toc=%2fazure%2fbackup%2ftoc.json)

## Soubory a složky ve Windows
### [Windows Server s využitím klasického modelu nasazení](backup-configure-vault-classic.md)
### [Windows Server s využitím modelu nasazení Resource Manageru](backup-configure-vault.md)
### [Správa trezorů služby Backup s využitím klasického modelu nasazení](backup-azure-manage-windows-server-classic.md)
### [Monitorování a správa trezorů služby Recovery Services](backup-azure-manage-windows-server.md)
### [Obnovení souborů na Windows Serveru s využitím modelu nasazení Resource Manager](backup-azure-restore-windows-server.md)
### [Obnovení souborů na Windows Serveru s využitím klasického modelu nasazení](backup-azure-restore-windows-server-classic.md)

## [Nejčastější dotazy](backup-azure-backup-faq.md)

## Řešení potíží
### [Potíže se zálohami virtuálních počítačů Azure na webu Azure Portal](backup-azure-vms-troubleshoot.md)
### [Potíže se zálohami virtuálních počítačů Azure na portálu Azure Classic](backup-azure-vms-troubleshoot-classic.md)
### [Azure Backup Server](backup-azure-mabs-troubleshoot.md)
### [Selhání zálohování virtuálních počítačů Azure: S agentem virtuálního počítače nebylo možné komunikovat – Dílčí úloze snímku virtuálního počítače vypršel časový limit](backup-azure-troubleshoot-vm-backup-fails-snapshot-timeout.md)
### [Pomalé zálohování souborů a složek ve službě Azure Backup](backup-azure-troubleshoot-slow-backup-performance-issue.md)

# Koncepty
## [Řízení přístupu na základě rolí](backup-rbac-rs-vault.md)
## [Zabezpečení pro hybridní zálohy](backup-azure-security-feature.md)
## [Odstranění trezoru služby Azure Backup](backup-azure-delete-vault.md)
## [Konfigurace zálohování offline](backup-azure-backup-import-export.md)
## [Náhrada páskové knihovny](backup-azure-backup-cloud-as-tape.md)
## [Aplikačně konzistentní zálohování virtuálních počítačů s Linuxem](backup-azure-linux-app-consistent.md)

# Referenční informace
## [PowerShell](/powershell/module/azurerm.recoveryservices.backup)
## [.NET](/dotnet/api/microsoft.azure.management.recoveryservices.backup)

# Zdroje a prostředky
## [Ceny](https://azure.microsoft.com/pricing/details/backup/)
## [Fórum MSDN](https://social.msdn.microsoft.com/Forums/en-US/home?forum=windowsazureonlinebackup)
## [Videa](https://azure.microsoft.com/documentation/videos/index/?services=backup)
## [Aktualizace služeb](https://azure.microsoft.com/updates/?product=backup)
