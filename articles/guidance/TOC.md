# Architektura

## Základy práce s cloudy

### [Návrh odolných aplikací pro Azure](guidance-resiliency-overview.md)
#### [Kontrolní seznam k odolnosti](guidance-resiliency-checklist.md)
#### [Analýza režimu selhání](guidance-resiliency-failure-mode-analysis.md)

#### [Zotavení po havárii pro aplikace založené na Microsoft Azure](..\resiliency\resiliency-disaster-recovery-azure-applications.md)
#### [Zotavení po havárii a vysoká dostupnost pro aplikace založené na Microsoft Azure](..\resiliency\resiliency-disaster-recovery-high-availability-azure-applications.md)
#### [Vysoká dostupnost pro aplikace založené na Microsoft Azure](..\resiliency\resiliency-high-availability-azure-applications.md)
#### [Kontrolní seznam k vysoké dostupnosti](..\resiliency\resiliency-high-availability-checklist.md)
#### [Doprovodné materiály k zajištění odolnosti specifické pro služby Azure](..\resiliency\resiliency-service-guidance-index.md)
#### [Obnovení při poškození nebo nechtěném odstranění dat](..\resiliency\resiliency-technical-guidance-recovery-data-corruption.md)
#### [Obnovení při místních selháních v Azure](..\resiliency\resiliency-technical-guidance-recovery-local-failures.md)
#### [Obnovení při přerušení služeb na úrovni celé oblasti](..\resiliency\resiliency-technical-guidance-recovery-loss-azure-region.md)
#### [Obnovení z místního prostředí do Azure](..\resiliency\resiliency-technical-guidance-recovery-on-premises-azure.md)
#### [Technické pokyny k odolnosti Azure](..\resiliency\resiliency-technical-guidance.md)


## Referenční architektury

### Referenční architektura služby Compute
#### [Spuštění linuxového virtuálního počítače v Azure](guidance-compute-single-vm-linux.md)
#### [Spuštění virtuálního počítače s Windows v Azure](guidance-compute-single-vm.md)
#### [Spuštění několika virtuálních počítačů v Azure pro zajištění škálovatelnosti a dostupnosti](guidance-compute-multi-vm.md)
#### [Spuštění virtuálních počítačů s Linuxem pro n-vrstvou architekturu v Azure](guidance-compute-n-tier-vm-linux.md)
#### [Spuštění virtuálních počítačů s Windows pro n-vrstvou architekturu v Azure](guidance-compute-n-tier-vm.md)
#### [Spuštění virtuálních počítačů s Linuxem v několika oblastech pro zajištění vysoké dostupnosti](guidance-compute-multiple-datacenters-linux.md)
#### [Spuštění virtuálních počítačů s Windows v několika oblastech pro zajištění vysoké dostupnosti](guidance-compute-multiple-datacenters.md)

### [Připojení vaší místní sítě k Azure](guidance-connecting-your-on-premises-network-to-azure.md)
#### [Implementace hybridní síťové architektury s využitím Azure ExpressRoute](guidance-hybrid-network-expressroute.md)
#### [Implementace hybridní síťové architektury s využitím Azure a místní sítě VPN](guidance-hybrid-network-vpn.md)
#### [Implementace hybridní síťové architektury s vysokou dostupností](guidance-hybrid-network-expressroute-vpn-failover.md)

### Ochrana hranice cloudu v Azure
#### [Implementace zabezpečené hybridní síťové architektury v Azure](guidance-iaas-ra-secure-vnet-hybrid.md)
#### [Implementace DMZ mezi Azure a internetem](guidance-iaas-ra-secure-vnet-dmz.md)

### [Správa identit v Azure](guidance-ra-identity.md)
#### [Implementace služby Azure Active Directory](guidance-identity-aad.md)
#### [Rozšíření služby Active Directory Directory Services (ADDS) do Azure](guidance-identity-adds-extend-domain.md)
#### [Vytvoření doménové struktury prostředků Active Directory Directory Services (ADDS) v Azure](guidance-identity-adds-resource-forest.md)
#### [Implementace služby Active Directory Federation Services (ADFS) v Azure](guidance-identity-adfs.md)

### Referenční architektura pro webové aplikace PaaS
#### [Referenční architektura Azure: Základní webová aplikace](guidance-web-apps-basic.md)
#### [Referenční architektura Azure: Webová aplikace s vysokou dostupností](guidance-web-apps-multi-region.md)
#### [Vylepšení škálovatelnosti ve webové aplikaci](guidance-web-apps-scalability.md)


## Způsoby návrhu v cloudu

## Osvědčené postupy pro cloudové aplikace

### [Pokyny k návrhu rozhraní API](..\best-practices-api-design.md)
### [Pokyny k implementaci rozhraní API](..\best-practices-api-implementation.md)
### [Pokyny k automatickému škálování](..\best-practices-auto-scaling.md)
### [Kontrolní seznam k dostupnosti](..\best-practices-availability-checklist.md)
### [Pokyny k úlohám na pozadí](..\best-practices-background-jobs.md)
### [Provozní kontinuita a zotavení po havárii (BCDR): Spárované oblasti Azure](..\best-practices-availability-paired-regions.md)
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
#### [Ladění výkonu při přijímání dat pro Elasticsearch v Azure](guidance-elasticsearch-tuning-data-ingestion-performance.md)
#### [Ladění výkonu při zpracování dotazů a agregace dat pomocí Elasticsearch v Azure](guidance-elasticsearch-tuning-data-aggregation-and-query-performance.md)
#### [Konfigurace odolnosti a obnovení v Elasticsearch v Azure](guidance-elasticsearch-configuring-resilience-and-recovery.md)
#### [Vytvoření prostředí pro testování výkonnosti pro Elasticsearch v Azure](guidance-elasticsearch-creating-performance-testing-environment.md)
#### [Implementace testovacího plánu JMeter pro Elasticsearch](guidance-elasticsearch-implementing-jmeter-test-plan.md)
#### [Nasazení vzorkovníku JMeter JUnit pro testování výkonnosti Elasticsearch](guidance-elasticsearch-deploying-jmeter-junit-sampler.md)
#### [Spuštění automatizovaných testů odolnosti Elasticsearch](guidance-elasticsearch-running-automated-resilience-tests.md)
#### [Spuštění automatizovaných testů výkonnosti Elasticsearch](guidance-elasticsearch-running-automated-performance-tests.md)

### [Správa identit pro víceklientské aplikace v Microsoft Azure](guidance-multitenant-identity.md)
#### [Seznámení se správou identit pro víceklientské aplikace v Microsoft Azure](guidance-multitenant-identity-intro.md)
#### [Aplikace Tailspin Surveys](guidance-multitenant-identity-tailspin.md)
#### [Ověřování ve víceklientských aplikacích s využitím Azure AD a OpenID Connect](guidance-multitenant-identity-authenticate.md)
#### [Práce s identitami založenými na deklaracích ve víceklientských aplikacích](guidance-multitenant-identity-claims.md)
#### [Registrace a připojování tenantů ve víceklientské aplikaci](guidance-multitenant-identity-signup.md)
#### [Aplikační role ve víceklientských aplikacích](guidance-multitenant-identity-app-roles.md)
#### [Ověřování na základě rolí a prostředků ve víceklientských aplikacích](guidance-multitenant-identity-authorize.md)
#### [Zabezpečení webového rozhraní API back-endu ve víceklientské aplikaci](guidance-multitenant-identity-web-api.md)
#### [Ukládání přístupových tokenů ve víceklientské aplikaci](guidance-multitenant-identity-token-cache.md)
#### [Federování s AD FS zákazníka pro víceklientské aplikace Azure](guidance-multitenant-identity-adfs.md)
#### [Použití klientského kontrolního výrazu k získání přístupových tokenů z Azure AD](guidance-multitenant-identity-client-assertion.md)
#### [Použití Azure Key Vault k ochraně tajných klíčů aplikace](guidance-multitenant-identity-keyvault.md)

#### [Nasazení virtuálních zařízení v prostředí s vysokou dostupností](guidance-nva-ha.md)


<!--HONumber=Nov16_HO2-->


