# [Dokumentace ke službě Azure Security](/azure/security/)
# [Architektura a návrh](../azure-security.md)
## [Pokročilá detekce hrozeb](../azure-threat-detection.md)
## [Protokolování a auditování Azure](../azure-log-audit.md)
## [Zabezpečení sítě Azure](../azure-network-security.md)
## [Povolení provozního zabezpečení](../azure-operational-security.md)
## [Zásady správného řízení v Azure](../governance-in-azure.md)
## [Izolace v cloudu Azure](../azure-isolation.md)
## [Zabezpečení hybridní síťové architektury](https://docs.microsoft.com/en-us/azure/architecture/reference-architectures/dmz/secure-vnet-hybrid?toc=%2Fazure%2Fsecurity%2Ftoc.json)
## [Technické možnosti zabezpečení](../azure-security-technical-capabilities.md)

# [Zabezpečení a šifrování dat](../security-azure-encryption-overview.md)
## [Zabezpečení databází](../azure-database-security-overview.md)
### [Osvědčené postupy](../azure-database-security-best-practices.md)
### [Kontrolní seznam zabezpečení](../azure-database-security-checklist.md)
## Šifrování disku
### [Osvědčené postupy](../azure-security-data-encryption-best-practices.md)
### [Šifrování neaktivních dat](../azure-security-encryption-atrest.md)
### [Šifrování disků pro virtuální počítače IaaS](../azure-security-disk-encryption.md)
#### [Šifrování disků – nejčastější dotazy](../azure-security-disk-encryption-faq.md)
#### [Řešení potíží](../azure-security-disk-encryption-tsg.md)
### [Šifrování virtuálního počítače Azure](../../security-center/security-center-disk-encryption.md?toc=%2fazure%2fsecurity%2ftoc.json)
## [Zabezpečení služby Azure Storage](../security-storage-overview.md)
## [Příručka zabezpečení úložiště](../../storage/common/storage-security-guide.md?toc=%2fazure%2fsecurity%2ftoc.json)

# Platforma a infrastruktura
## [Antimalware od Microsoftu](../azure-security-antimalware.md)
## [Zabezpečení IaaS](../security-virtual-machines-overview.md)
### [Osvědčené postupy – virtuální počítače Azure](../azure-security-best-practices-vms.md)
### [Osvědčené postupy – úlohy IaaS](../azure-security-iaas.md)
### [Image na webu Azure Marketplace](../security-recommendations-azure-marketplace-images.md)
## [Správa identit](../security-identity-management-overview.md)
### [Osvědčené postupy](../azure-security-identity-management-best-practices.md)
## [Zabezpečení sítě](../security-network-overview.md)
### [Osvědčené postupy](../azure-security-network-security-best-practices.md)
### [Zabezpečení hranic](../../best-practices-network-security.md?toc=%2fazure%2fsecurity%2ftoc.json)

# Aplikace
##  [PaaS](../security-paas-deployments.md)
### [Azure App Service pro PaaS](../security-paas-applications-using-app-services.md)
### [Azure Storage pro PaaS](../security-paas-applications-using-storage.md)
### [Osvědčené postupy DB pro PaaS](../security-paas-applications-using-sql.md)
### [IoT](../security-internet-of-things-overview.md)
#### [Osvědčené postupy zabezpečení IoT](../../iot-suite/iot-security-best-practices.md)
#### [Zabezpečení IoT](../../iot-suite/iot-security-architecture.md#security-in-iot)
#### [Zabezpečení nasazení IoT](../../iot-suite/iot-suite-security-deployment.md)

## [Zabezpečení služby Service Fabric](../azure-service-fabric-security-overview.md)
### [Osvědčené postupy](../azure-service-fabric-security-best-practices.md)
### [Kontrolní seznam zabezpečení](../azure-service-fabric-security-checklist.md)

# [Monitorování, auditování a provoz](../security-management-and-monitoring-overview.md)
## Auditování a protokolování
### [Správa zabezpečení](../../security-center/security-center-intro.md?toc=%2fazure%2fsecurity%2ftoc.json)
### [Zabezpečení vzdálené správy](../azure-security-management.md)
### Integrace protokolů Azure
#### [Úvod](../security-azure-log-integration-overview.md)
#### [Začínáme](../security-azure-log-integration-get-started.md)
#### [Integrace protokolů auditu Azure AD](../security-azure-log-integration-ad.md)
#### [Integrace výstrah služby Security Center](../security-azure-log-integration-security-center.md)
#### [Integrace protokolů ze služby Key Vault](../security-azure-log-integration-keyvault-eventhub.md)
#### [Nejčastější dotazy](../security-azure-log-integration-faq.md)
## [Provozní zabezpečení](../azure-operational-security-overview.md)
### [Osvědčené postupy](../azure-operational-security-best-practices.md)
### [Kontrolní seznam zabezpečení](../azure-operational-security-checklist.md)

# Zásady správného řízení a dodržování předpisů
## [Finanční služby pro regulované úlohy](financial-services-regulated-workloads.md)
## GDPR
### [Správa osobních údajů](../manage-personal-data-azure.md)
### [Zjišťování, identifikace a klasifikace osobních údajů](../how-to-discover-classify-personal-data-azure.md)
### [Ochrana osobních údajů v Azure](../protect-personal-data-azure.md)
### [Ochrana osobních údajů pomocí služby Security Center](../protect-personal-data-azure-security-center.md)
### [Ochrana osobních údajů pomocí služby Application Gateway](../protect-personal-data-network-security.md)
### [Ochrana osobních údajů pomocí identit a řízení přístupu](../protect-personal-data-identity-access-controls.md)
### [Ochrana osobních údajů v klidovém stavu pomocí šifrování](../protect-personal-data-at-rest.md)
### [Ochrana osobních údajů při přenosu pomocí šifrování](../protect-personal-data-in-transit-encryption.md)
### [Ochrana osobních údajů pomocí nástrojů na vytváření sestav Azure](../protection-personal-data-azure-reporting-tools.md)
## [FEDRAMP](fedramp.md)
### [Přehled ovládacích prvků zabezpečení](fedramp-controls-overview.md)
### [Řízení přístupu](fedramp-access-controls.md)
### [Audit a zodpovědnost](fedramp-audit-accountability-controls.md)
### [Přehled a školení](fedramp-awareness-training-controls.md)
### [Správa konfigurace](fedramp-configuration-manager-controls.md)
### [Plánování řešení nepředvídaných událostí](fedramp-contingency-planning-controls.md)
### [Identifikace a ověřování](fedramp-identification-authentication-controls.md)
### [Reakce na incidenty](fedramp-incident-response-controls.md)
### [Údržba](fedramp-maintenance-controls.md)
### [Ochrana médií](fedramp-media-protection-controls.md)
### [Zabezpečení pracovníků](fedramp-personnel-security-controls.md)
### [Fyzická ochrana a ochrana prostředí](fedramp-physical-environmental-protection-controls.md)
### [Plánování](fedramp-planning-controls.md)
### [Posouzení rizik](fedramp-risk-assessment-controls.md)
### [Posouzení zabezpečení a autorizace](fedramp-security-assessment-authorization-controls.md)
### [Ochrana systémů a komunikací](fedramp-system-communications-protection-controls.md)
### [Integrita systémů a informací](fedramp-system-information-integrity-controls.md)
### [Získávání systémů a služeb](fedramp-system-services-acquisition-controls.md)
## [PCI DSS](payment-processing-blueprint.md)
### [Přehled požadavků](pci-dss-requirements-overview.md)
### [Požadavek 1 – brány firewall](pci-dss-requirement-1-firewall.md)
### [Požadavek 2 – hesla](pci-dss-requirement-2-password.md)
### [Požadavek 3 – CHD](pci-dss-requirement-3-chd.md)
### [Požadavek 4 – šifrování](pci-dss-requirement-4-encryption.md)
### [Požadavek 5 – malware](pci-dss-requirement-5-malware.md)
### [Požadavek 6 – zabezpečené systémy](pci-dss-requirement-6-secure-system.md)
### [Požadavek 7 – přístup](pci-dss-requirement-7-access.md)
### [Požadavek 8 – identita](pci-dss-requirement-8-identity.md)
### [Požadavek 9 – fyzický přístup](pci-dss-requirement-9-physical-access.md)
### [Požadavek 10 – monitorování](pci-dss-requirement-10-monitoring.md)
### [Požadavek 11 – testování](pci-dss-requirement-11-testing.md)
### [Požadavek 12 – zásady](pci-dss-requirement-12-policy.md)
## [UK-OFFICIAL](uk-official-three-tier-applications-overview.md)
### [Přehled principů](uk-official-three-tier-applications-principles-overview.md)
## [Data o stavu HIPAA/HITRUST a AI](azure-health.md)

# [Dokumenty white paper](../security-white-papers.md)
# [Technické přehledy](../security-overviews.md)
# [Osvědčené postupy](../security-best-practices-and-patterns.md)

# Zdroje a prostředky

## [Program MVP pro zabezpečení Azure](../azure-security-mvp.md)
## [Poradenství pro zabezpečení](../azure-security-cyber-services.md)
## [Zadání lístku podpory pro událost zabezpečení](../azure-security-event-support-ticket.md)
## [Testování průniku](../azure-security-pen-testing.md)
## [Nástroj Microsoftu pro modelování hrozeb](../azure-security-threat-modeling-tool.md)
### [Začínáme](../azure-security-threat-modeling-tool-getting-started.md)
### [Přehled funkcí](../azure-security-threat-modeling-tool-feature-overview.md)
### [Hrozby](../azure-security-threat-modeling-tool-threats.md)
### [Omezení rizik](../azure-security-threat-modeling-tool-mitigations.md)
#### [Auditování a protokolování](../azure-security-threat-modeling-tool-auditing-and-logging.md)
#### [Ověřování](../azure-security-threat-modeling-tool-authentication.md)
#### [Autorizace](../azure-security-threat-modeling-tool-authorization.md)
#### [Zabezpečení komunikace](../azure-security-threat-modeling-tool-communication-security.md)
#### [Správa konfigurace](../azure-security-threat-modeling-tool-configuration-management.md)
#### [Kryptografie](../azure-security-threat-modeling-tool-cryptography.md)
#### [Správa výjimek](../azure-security-threat-modeling-tool-exception-management.md)
#### [Ověření vstupu](../azure-security-threat-modeling-tool-input-validation.md)
#### [Citlivá data](../azure-security-threat-modeling-tool-sensitive-data.md)
#### [Správa relací](../azure-security-threat-modeling-tool-session-management.md)

