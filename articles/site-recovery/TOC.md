# Přehled
## [Co je Site Recovery?](site-recovery-overview.md)
## [Jak funguje Site Recovery?](site-recovery-components.md)
## [Jak funguje replikace Hyper-V do Azure?](site-recovery-hyper-v-azure-architecture.md)
## [Jaké úlohy můžete chránit?](site-recovery-workload.md)
## [Matice podpory Site Recovery](site-recovery-support-matrix-to-azure.md)
## [Nejčastější dotazy](site-recovery-faq.md)
## [Podívejte se na základní přehled](https://azure.microsoft.com/resources/videos/index/?services=site-recovery)

# Začínáme
## [Replikace virtuálních počítačů VMware do Azure](site-recovery-vmware-to-azure.md)
## [Replikace fyzických serverů do Azure](site-recovery-physical-servers-to-azure.md) 
## [Replikace virtuálních počítačů Hyper-V do Azure (s VMM)](site-recovery-vmm-to-azure.md)
## [Replikace virtuálních počítačů Hyper-V do Azure](site-recovery-hyper-v-site-to-azure.md)
## [Replikace virtuálních počítačů Hyper-V do sekundární lokality (s VMM)](site-recovery-vmm-to-vmm.md)
## [Replikace fyzických serverů a virtuálních počítačů VMware do sekundární lokality](site-recovery-vmware-to-vmware.md)
## [Replikace virtuálních počítačů VMware do Azure ve víceklientském nasazení (CSP)](site-recovery-multi-tenant-support-vmware-using-csp.md)

# Postup
## Plánování
### [Požadavky nasazení](site-recovery-prereq.md)
### [Plánování síťové infrastruktury](site-recovery-network-design.md)
### [Plánování kapacity a škálování replikace VMware do Azure](site-recovery-plan-capacity-vmware.md)
### [Plánovač nasazení pro replikaci VMware do Azure](site-recovery-deployment-planner.md)
### [Capacity Planner pro replikaci Hyper-V](site-recovery-capacity-planner.md)

## Konfigurace
### [Nastavení zdrojového prostředí](site-recovery-set-up-vmware-to-azure.md)
### [Nastavení cílového prostředí](site-recovery-prepare-target-vmware-to-azure.md)
### [Konfigurace nastavení replikace](site-recovery-setup-replication-settings-vmware.md)
### [Nasazení služby Mobility pro replikaci VMware](site-recovery-vmware-to-azure-install-mob-svc.md)
#### [Nasazení služby Mobility s využitím System Center Configuration Manageru](site-recovery-install-mobility-service-using-sccm.md)
#### [Nasazení služby Mobility s využitím Azure Automation DSC](site-recovery-automate-mobility-service-install.md)
### [Povolení replikace](site-recovery-replicate-vmware-to-azure.md)
## Převzetí služeb při selhání a navrácení služeb po obnovení
### [Převzetí služeb při selhání u chráněných počítačů](site-recovery-failover.md)
### [Nastavení plánů obnovení](site-recovery-create-recovery-plans.md)
#### [Přidání runbooků Azure do plánů obnovení](site-recovery-runbook-automation.md)
### [Spuštění testovacího převzetí služeb při selhání](site-recovery-test-failover-to-azure.md)
### [Znovunastavení ochrany počítačů po převzetí služeb při selhání](site-recovery-how-to-reprotect.md)
### [Navrácení služeb po obnovení z Azure](site-recovery-failback-azure-to-vmware.md)

## Migrace
### [Migrace do Azure](site-recovery-migrate-to-azure.md)
### [Migrace mezi oblastmi Azure](site-recovery-migrate-azure-to-azure.md)
### [Migrace instancí Windows AWS do Azure](site-recovery-migrate-aws-to-azure.md)
## Úlohy
### [Active Directory a DNS](site-recovery-active-directory.md)
### [SQL Server](site-recovery-sql.md)
### [SharePoint](site-recovery-sharepoint.md)
### [Dynamics AX](site-recovery-dynamicsax.md)
### [Vzdálená plocha](site-recovery-workload.md#protect-rds)
### [Výměna](site-recovery-workload.md#protect-exchange)
### [SAP](site-recovery-workload.md#protect-sap)
### [Další úlohy](site-recovery-workload.md#workload-summary)
## Automatizace replikace
### [Automatizace replikace Hyper-V do Azure (bez VMM)](site-recovery-deploy-with-powershell-resource-manager.md)
### [Automatizace replikace Hyper-V do Azure (s VMM)](site-recovery-vmm-to-azure-powershell-resource-manager.md)
### [Automatizace replikace Hyper-V do sekundární lokality (s VMM)](site-recovery-vmm-to-vmm-powershell-resource-manager.md)
## Spravovat
### [Úprava nastavení replikace](site-recovery-setup-replication-settings-vmware.md#edit-replication-policy.md)
### [Správa procesových serverů v Azure](site-recovery-vmware-setup-azure-ps-resource-manager.md)
### [Správa konfiguračního serveru](site-recovery-vmware-to-azure-manage-configuration-server.md)
### [Správa procesových serverů s horizontálním navýšením kapacity](site-recovery-vmware-to-azure-manage-scaleout-process-server.md)
### [Správa serverů vCenter](site-recovery-vmware-to-azure-manage-vCenter.md)
### [Odebrání serverů a zakázání ochrany](site-recovery-manage-registration-and-protection.md)
## [Monitorování a odstraňování potíží](site-recovery-monitoring-and-troubleshooting.md)

# Referenční informace
## [PowerShell](/powershell/module/azurerm.siterecovery)
## [PowerShell – Classic](/powershell/module/azure/?view=azuresmps-3.7.0)
## [REST](https://msdn.microsoft.com/en-us/library/mt750497)

# Související
## [Azure Automation](/azure/automation/)

# Zdroje a prostředky
## [Postup výuky](https://azure.microsoft.com/documentation/learning-paths/site-recovery/)
## [Fórum](https://social.msdn.microsoft.com/Forums/azure/en-US/home?forum=hypervrecovmgr)
## [Blog](http://azure.microsoft.com/blog/tag/azure-site-recovery/)
## [Ceny](https://azure.microsoft.com/pricing/details/site-recovery/)
## [Aktualizace služeb](https://azure.microsoft.com/updates/?product=site-recovery)
