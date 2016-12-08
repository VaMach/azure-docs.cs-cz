# Architektura

## Základy práce s cloudy

### [Navrhování odolných aplikací](guidance-resiliency-overview.md)
#### [Kontrolní seznam k odolnosti](guidance-resiliency-checklist.md)
#### [Analýza režimu selhání](guidance-resiliency-failure-mode-analysis.md)
#### [Zotavení po havárii](..\resiliency\resiliency-disaster-recovery-azure-applications.md)
#### [Zotavení po havárii a vysoká dostupnost](..\resiliency\resiliency-disaster-recovery-high-availability-azure-applications.md)
#### [Vysoká dostupnost](..\resiliency\resiliency-high-availability-azure-applications.md)
#### [Kontrolní seznam k vysoké dostupnosti](..\resiliency\resiliency-high-availability-checklist.md)
#### [Doprovodné materiály k zajištění odolnosti specifické pro služby Azure](..\resiliency\resiliency-service-guidance-index.md)
#### [Obnovení při poškození nebo nechtěném odstranění dat](..\resiliency\resiliency-technical-guidance-recovery-data-corruption.md)
#### [Obnovení při místních selháních](..\resiliency\resiliency-technical-guidance-recovery-local-failures.md)
#### [Obnovení při přerušení služeb na úrovni celé oblasti](..\resiliency\resiliency-technical-guidance-recovery-loss-azure-region.md)
#### [Obnovení z místního prostředí do Azure](..\resiliency\resiliency-technical-guidance-recovery-on-premises-azure.md)
#### [Technické pokyny k odolnosti Azure](..\resiliency\resiliency-technical-guidance.md)


## Referenční architektury

### [Spuštění úloh virtuálních počítačů v Azure](guidance-ra-compute.md)
#### [Spuštění linuxového virtuálního počítače v Azure](guidance-compute-single-vm-linux.md)
#### [Spuštění virtuálního počítače s Windows v Azure](guidance-compute-single-vm.md)
#### [Spuštění několika virtuálních počítačů v Azure pro zajištění škálovatelnosti a dostupnosti](guidance-compute-multi-vm.md)
#### [Spuštění linuxových virtuálních počítačů pro n-vrstvou architekturu](guidance-compute-n-tier-vm-linux.md)
#### [Spuštění virtuálních počítačů s Windows pro n-vrstvou architekturu](guidance-compute-n-tier-vm.md)
#### [Spuštění linuxových virtuálních počítačů v několika oblastech pro zajištění vysoké dostupnosti](guidance-compute-multiple-datacenters-linux.md)
#### [Spuštění virtuálních počítačů s Windows v několika oblastech pro zajištění vysoké dostupnosti](guidance-compute-multiple-datacenters.md)

### [Připojení místní sítě k Azure](guidance-ra-hybrid-networking.md)
#### [Hybridní síťová architektura s využitím Azure ExpressRoute](guidance-hybrid-network-expressroute.md)
#### [Hybridní síťová architektura s využitím Azure a místní sítě VPN](guidance-hybrid-network-vpn.md)
#### [Vysoce dostupná hybridní síťová architektura](guidance-hybrid-network-expressroute-vpn-failover.md)

### [Ochrana hranice cloudu v Azure](guidance-ra-network-security.md)
#### [Zabezpečení hybridní síťové architektury v Azure](guidance-iaas-ra-secure-vnet-hybrid.md)
#### [DMZ mezi Azure a internetem](guidance-iaas-ra-secure-vnet-dmz.md)

### [Správa identit v Azure](guidance-ra-identity.md)
#### [Implementace Azure Active Directory](guidance-identity-aad.md)
#### [Rozšíření ADDS do Azure](guidance-identity-adds-extend-domain.md)
#### [Vytvoření doménové struktury prostředku ADDS v Azure](guidance-identity-adds-resource-forest.md)
#### [Implementace služby AD FS v Azure](guidance-identity-adfs.md)

### [Architektury webových aplikací pro službu Azure App Service](guidance-ra-app-service.md)
#### [Základní webové aplikace](guidance-web-apps-basic.md)
#### [Webové aplikace s vysokou dostupností](guidance-web-apps-multi-region.md)
#### [Vylepšení škálovatelnosti ve webové aplikaci](guidance-web-apps-scalability.md)


## Osvědčené postupy pro cloudové aplikace

### [Pokyny k návrhu rozhraní API](..\best-practices-api-design.md)
### [Pokyny k implementaci rozhraní API](..\best-practices-api-implementation.md)
### [Pokyny k automatickému škálování](..\best-practices-auto-scaling.md)
### [Kontrolní seznam k dostupnosti](..\best-practices-availability-checklist.md)
### [Pokyny k úlohám na pozadí](..\best-practices-background-jobs.md)
### [Spárované oblasti Azure](..\best-practices-availability-paired-regions.md)
### [Pokyny k ukládání do mezipaměti](..\best-practices-caching.md)
### [Pokyny k síti pro doručování obsahu (CDN)](..\best-practices-cdn.md)
### [Pokyny k dělení dat](..\best-practices-data-partitioning.md)
### [Pokyny k monitorování a diagnostice](..\best-practices-monitoring.md)
### [Zabezpečení sítí a cloudových služeb Microsoftu](..\best-practices-network-security.md)
### [Vzory pro navrhování šablon Azure Resource Manageru](..\best-practices-resource-manager-design-templates.md)
### [Doporučené zásady vytváření názvů pro prostředky Azure](guidance-naming-conventions.md)
### [Aspekty zabezpečení pro Azure Resource Manager](..\best-practices-resource-manager-security.md)
### [Sdílení stavu v šablonách Azure Resource Manageru](..\best-practices-resource-manager-state.md)
### [Obecné pokyny k opakovaným pokusům](..\best-practices-retry-general.md)
### [Pokyny k opakovaným pokusům pro konkrétní služby](..\best-practices-retry-service-specific.md)
### [Kontrolní seznam ke škálovatelnosti](..\best-practices-scalability-checklist.md)


## Pokyny pro scénáře

### [Pokyny k Elasticsearch v Azure](guidance-elasticsearch.md)
#### [Spuštění Elasticsearch v Azure](guidance-elasticsearch-running-on-azure.md)
#### [Vyladění výkonu přijímání dat](guidance-elasticsearch-tuning-data-ingestion-performance.md)
#### [Vyladění výkonu dotazování a agregace dat](guidance-elasticsearch-tuning-data-aggregation-and-query-performance.md)
#### [Konfigurace odolnosti a obnovení](guidance-elasticsearch-configuring-resilience-and-recovery.md)
#### [Vytvoření prostředí pro testování výkonu](guidance-elasticsearch-creating-performance-testing-environment.md)
#### [Implementace testovacího plánu JMeter](guidance-elasticsearch-implementing-jmeter-test-plan.md)
#### [Nasazení vzorkovníku JMeter JUnit](guidance-elasticsearch-deploying-jmeter-junit-sampler.md)
#### [Spuštění automatizovaných testů odolnosti](guidance-elasticsearch-running-automated-resilience-tests.md)
#### [Spuštění automatizovaných testů výkonu](guidance-elasticsearch-running-automated-performance-tests.md)

### [Správa identit pro víceklientské aplikace](guidance-multitenant-identity.md)
#### [Přehled](guidance-multitenant-identity-intro.md)
#### [Aplikace Tailspin Surveys](guidance-multitenant-identity-tailspin.md)
#### [Ověřování s využitím Azure AD a OpenID Connect](guidance-multitenant-identity-authenticate.md)
#### [Identity založené na deklaracích](guidance-multitenant-identity-claims.md)
#### [Registrace a připojování tenantů](guidance-multitenant-identity-signup.md)
#### [Aplikační role](guidance-multitenant-identity-app-roles.md)
#### [Ověřování na základě rolí a prostředků](guidance-multitenant-identity-authorize.md)
#### [Zabezpečení webového rozhraní API back-endu](guidance-multitenant-identity-web-api.md)
#### [Tokeny pro přístup k mezipaměti](guidance-multitenant-identity-token-cache.md)
#### [Federace s využitím AD FS zákazníka](guidance-multitenant-identity-adfs.md)
#### [Použití klientského kontrolního výrazu k získání přístupových tokenů](guidance-multitenant-identity-client-assertion.md)
#### [Použití Azure Key Vault k ochraně tajných kódů aplikace](guidance-multitenant-identity-keyvault.md)
#### [Nasazení virtuálních zařízení v prostředí s vysokou dostupností](guidance-nva-ha.md)


<!--HONumber=Nov16_HO4-->


