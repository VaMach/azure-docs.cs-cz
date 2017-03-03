# Přehled
## [Co je Site Recovery?](site-recovery-overview.md)
## [Jak funguje Site Recovery?](site-recovery-components.md)
## [Jaké úlohy můžete chránit?](site-recovery-workload.md)
## [Matice podpory Site Recovery](site-recovery-support-matrix-to-azure.md)
## [Nejčastější dotazy](site-recovery-faq.md)
## [Podívejte se na základní přehled](https://www.youtube.com/watch?v=eOOwMQPBKfM)

# Začínáme
## [Replikace virtuálních počítačů VMware do Azure](site-recovery-vmware-to-azure.md)
## [Replikace virtuálních počítačů VMware do Azure ve víceklientském nasazení (CSP)](site-recovery-multi-tenant-support-vmware-using-csp.md)
## [Replikace virtuálních počítačů Hyper-V do Azure (s VMM)](site-recovery-vmm-to-azure.md)
## [Replikace virtuálních počítačů Hyper-V do Azure](site-recovery-hyper-v-site-to-azure.md)
## [Replikace fyzických serverů a virtuálních počítačů VMware do sekundární lokality](site-recovery-vmware-to-vmware.md)
## [Replikace virtuálních počítačů Hyper-V do sekundární lokality (s VMM)](site-recovery-vmm-to-vmm.md)

# Postup
## Plánování
### [Požadavky nasazení](site-recovery-prereq.md)
### [Důležité informace o síťové infrastruktuře](site-recovery-network-design.md)
### [Použití nástroje Capacity Planner služby Site Recovery](site-recovery-capacity-planner.md)
### [Plánování kapacity a škálování replikace VMware do Azure](site-recovery-plan-capacity-vmware.md)
### [Spuštění plánovače nasazení pro replikaci VMware do Azure](site-recovery-deployment-planner.md)
## Konfigurace
### [Nastavení zdrojového prostředí](site-recovery-set-up-vmware-to-azure.md)
### [Nastavení cílového prostředí](site-recovery-prepare-target-vmware-to-azure.md)
### [Konfigurace nastavení replikace](site-recovery-setup-replication-settings-vmware.md)
### [Nasazení služby Mobility pro replikaci VMware](site-recovery-vmware-to-azure-install-mob-svc.md)
#### [Nasazení služby Mobility s využitím System Center Configuration Manageru](site-recovery-install-mobility-service-using-sccm.md)
#### [Nasazení služby Mobility s využitím Azure Automation DSC](site-recovery-automate-mobility-service-install.md)
## Převzetí služeb při selhání a navrácení služeb po obnovení
### [Převzetí služeb při selhání ve službě Site Recovery](site-recovery-failover.md)
### [Nastavení plánů obnovení](site-recovery-create-recovery-plans.md)
#### [Přidání runbooků Azure do plánů obnovení](site-recovery-runbook-automation.md)
### [Spuštění testovacího převzetí služeb při selhání do Azure](site-recovery-test-failover-to-azure.md)
### [Spuštění testovacího převzetí služeb při selhání mezi dvěma umístěními VMM](site-recovery-test-failover-vmm-to-vmm.md)
### [Navrácení služeb fyzických serverů a virtuálních počítačů VMware z Azure po obnovení](site-recovery-how-to-failback-azure-to-vmware.md)
### [Navrácení služeb virtuálních počítačů Hyper-V z Azure po obnovení](site-recovery-failback-from-azure-to-hyper-v.md)

## Migrace
### [Migrace do Azure](site-recovery-migrate-to-azure.md)
### [Migrace mezi oblastmi Azure](site-recovery-migrate-azure-to-azure.md)
### [Migrace instancí Windows AWS do Azure](site-recovery-migrate-aws-to-azure.md)
## Úlohy
### [Active Directory a DNS](site-recovery-active-directory.md)
### [SQL Server](site-recovery-sql.md)
### [SharePoint](site-recovery-workload.md#protect-sharepoint)
### [Dynamics AX](site-recovery-workload.md#protect-dynamics-ax)
### [Vzdálená plocha](site-recovery-workload.md#protect-rds)
### [Výměna](site-recovery-workload.md#protect-exchange)
### [SAP](site-recovery-workload.md#protect-sap)
### [Další úlohy](site-recovery-workload.md#workload-summary)
## Automatizace replikace
### [Automatizace replikace Hyper-V do Azure (bez VMM)](site-recovery-deploy-with-powershell-resource-manager.md)
### [Automatizace replikace Hyper-V do Azure (s VMM)](site-recovery-vmm-to-azure-powershell-resource-manager.md)
### [Automatizace replikace Hyper-V do sekundární lokality (s VMM)](site-recovery-vmm-to-vmm-powershell-resource-manager.md)
## Spravovat
### [Odebrání serverů a zakázání ochrany](site-recovery-manage-registration-and-protection.md)
### [Horizontální navýšení kapacity procesových serverů](site-recovery-vmware-to-azure-manage-scaleout-process-server.md)
### [Monitorování a odstraňování potíží](site-recovery-monitoring-and-troubleshooting.md)

# Referenční informace
## [PowerShell](/powershell/resourcemanager/azurerm.siterecovery/v3.2.0/azurerm.siterecovery)
## [PowerShell – Classic](/powershell/servicemanagement/azure.siterecovery/v3.1.0/azure.siterecovery)
## [REST](https://msdn.microsoft.com/en-us/library/mt750497)

# Související
## [Azure Automation](/azure/automation/)

# Zdroje a prostředky
## [Postup výuky](https://azure.microsoft.com/documentation/learning-paths/site-recovery/)
## [Fórum](https://social.msdn.microsoft.com/Forums/azure/en-US/home?forum=hypervrecovmgr)
## [Blog](http://azure.microsoft.com/blog/tag/azure-site-recovery/)
## [Ceny](https://azure.microsoft.com/pricing/details/site-recovery/)
## [Aktualizace služeb](https://azure.microsoft.com/updates/?product=site-recovery)
