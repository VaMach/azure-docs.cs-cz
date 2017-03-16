# Přehled
## [Co je Azure Stack?](azure-stack-poc.md)
## [Co je nového](azure-stack-whats-new.md)
## [Klíčové funkce a koncepty](azure-stack-key-features.md)
## [Architektura POC](azure-stack-architecture.md)

# Vyhodnocení a nasazení
## Začínáme
### [Požadavky nasazení](azure-stack-deploy.md)
### [Nasazení](azure-stack-run-powershell-script.md)
### [Registrace](azure-stack-register.md)
## Postup
### [Připojení k Azure Stack POC](azure-stack-connect-azure-stack.md)
### [Přidání výchozí image](azure-stack-add-default-image.md)
### [Zřízení virtuálního počítače](azure-stack-provision-vm.md)
### [Vytvoření účtu úložiště](azure-stack-provision-storage-account.md)
### [Přidání tenanta Azure Stacku](azure-stack-add-new-user-aad.md)
### [Opětovné nasazení Azure Stack POC](azure-stack-redeploy.md)

# Spravovat
## Přehled
### [Správa oblastí](azure-stack-region-management.md)
### [Použití portálů](azure-stack-manage-portals.md)
## Začínáme
### [Připojení ke službě Azure Stack](azure-stack-connect-azure-stack.md)
### Nastavení prostředí pro správu
#### [Instalace PowerShellu](azure-stack-powershell-install.md)
#### [Stažení nástrojů](azure-stack-powershell-download.md)
#### [Konfigurace PowerShellu](azure-stack-powershell-configure.md)
## Postup
### [Správa aktualizací](azure-stack-updates.md)
### [Monitorování stavu a upozornění](azure-stack-monitor-health.md)
### [Správa síťových prostředků](azure-stack-viewing-public-ip-address-consumption-in-tp2.md)
### [Správa prostředků úložiště](azure-stack-manage-storage-accounts.md)

# Zabezpečení a dodržování předpisů
## Postup
### [Správa uživatelských oprávnění](azure-stack-manage-permissions.md)
### [Přidání uživatelů pro AD FS](azure-stack-add-users-adfs.md)

# Nabídka služeb
## Začínáme
### Vytvoření plánů, nabídek a kvót
#### [Nastavení kvót](azure-stack-setting-quotas.md)
#### [Vytvoření plánu](azure-stack-create-plan.md)
#### [Vytvoření nabídky](azure-stack-create-offer.md)
#### [Přihlášení k odběru nabídky](azure-stack-subscribe-plan-provision-vm.md)
#### [Delegování nabídek v Azure Stacku](azure-stack-delegated-provider.md)
## Postup
### Nabídka SQL nebo MySQL jako PaaS
#### [Nasazení poskytovatele prostředků MySQL](azure-stack-mysql-resource-provider-deploy.md)
#### [Nasazení poskytovatele prostředků SQL](azure-stack-sql-resource-provider-deploy.md)
### Nabídka App Service jako PaaS
#### [Přehled App Service v Azure Stacku](azure-stack-app-service-overview.md)
#### [Než začnete](azure-stack-app-service-before-you-get-started.md)
#### [Nasazení poskytovatele prostředků App Service](azure-stack-app-service-deploy.md)
#### [Přidání dalších webových rolí pracovního procesu](azure-stack-app-service-add-worker-roles.md)
#### [Konfigurace zdrojů nasazení](azure-stack-app-service-configure-deployment-sources.md)
#### [Povolení FTP ve službě App Service v Azure Stacku](azure-stack-app-service-enable-ftp.md)
### Naplnění webu Marketplace
#### [Přehled webu Marketplace](azure-stack-marketplace.md)
#### [Stažení položek z Marketplace](azure-stack-download-azure-marketplace-item.md)
#### [Vytvoření a publikování položky Marketplace](azure-stack-create-and-publish-marketplace-item.md)
#### [Přidání vlastní image virtuálního počítače](azure-stack-add-vm-image.md)
#### [Nasazení virtuálních počítačů s Linuxem](azure-stack-linux.md)
### Vyúčtování a vratka
#### [Vyúčtování a vratka – přehled](azure-stack-billing-and-chargeback.md)
#### [Rozhraní API využití prostředků poskytovatele](azure-stack-provider-resource-api.md)
#### [Rozhraní API využití prostředků tenanta](azure-stack-tenant-resource-usage-api.md)
#### [Nejčastější dotazy k využití](azure-stack-usage-related-faq.md)

# Použití služeb
## Compute
### [Přidání image virtuálního počítače](azure-stack-add-vm-image.md)
### [Použití hostů s Linuxem](azure-stack-linux.md)
## Úložiště
### [Přehled](azure-stack-storage-overview.md)
### [Rozdíly a aspekty](azure-stack-acs-differences-tp2.md)
## Síť
### [iDNS pro Azure Stack](azure-stack-understanding-dns-in-tp2.md)
### [DNS v Azure Stacku](azure-stack-dns.md)
### [Principy připojení k sítím Site-to-Site VPN](azure-stack-create-vpn-connection-one-node-tp2.md)
## Key Vault
### [Přehled](azure-stack-kv-intro.md)
### Začínáme
#### [Správa pomocí portálu](azure-stack-kv-manage-portal.md)
#### [Správa pomocí PowerShellu](azure-stack-kv-manage-powershell.md)
### Postup
#### [Nasazení virtuálního počítače s heslem](azure-stack-kv-deploy-vm-with-secret.md)
#### [Vytvoření virtuálního počítače s certifikátem](azure-stack-kv-push-secret-into-vm.md)
#### [Ukázková aplikace Key Vaultu](azure-stack-kv-sample-app.md)

# Vytváření aplikací
## Přehled
### [Vývoj pro Azure Stack](azure-stack-developer.md)
## Začínáme
### [Připojení ke službě Azure Stack](azure-stack-connect-azure-stack.md)
### Nastavení vývojového prostředí
#### [Instalace PowerShellu](azure-stack-powershell-install.md)
#### [Stažení nástrojů](azure-stack-powershell-download.md)
#### [Konfigurace PowerShellu](azure-stack-powershell-configure.md)
#### [Instalace sady Visual Studio](azure-stack-install-visual-studio.md)
## Postup
### [Použití modulu zásad](azure-stack-policy-module.md)
### Použití šablon
#### [Přehled šablon](azure-stack-arm-templates.md)
#### [Vývoj šablon](azure-stack-develop-templates.md)
#### [Kontrola šablon](azure-stack-validate-templates.md)
#### [Nasazení s použitím portálu](azure-stack-deploy-template-portal.md)
#### [Nasazení s použitím PowerShellu](azure-stack-deploy-template-powershell.md)
#### [Nasazení s použitím sady Visual Studio](azure-stack-deploy-template-visual-studio.md)

# Řešení potíží
## [Známé problémy](azure-stack-troubleshooting.md)
## [Diagnostika v Azure Stacku](azure-stack-diagnostics.md)

# Zdroje a prostředky
## [Fórum MSDN](https://social.msdn.microsoft.com/Forums/azure/home?forum=AzureStack)  
## [Stack Overflow](http://stackoverflow.com/questions/tagged/azure-stack)

