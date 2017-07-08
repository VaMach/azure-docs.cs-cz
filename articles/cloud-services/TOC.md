# Přehled
## [Co je služba Cloud Services?](cloud-services-choose-me.md)
## [Balíčky a konfigurační soubory cloudových služeb](cloud-services-model-and-package.md)

# Začínáme
## [Příklad cloudové služby v .NET](cloud-services-dotnet-get-started.md)
## [Příklad cloudové služby v Pythonu pro Visual Studio](cloud-services-python-ptvs.md)
## [Nastavení hybridního clusteru HPC pomocí sady Microsoft HPC Pack](cloud-services-setup-hybrid-hpcpack-cluster.md)

# Postup
## Plánování
### [Velikosti virtuálních počítačů](cloud-services-sizes-specs.md)
### [Aktualizace](cloud-services-update-azure-service.md)

## Vývoj
### [tvoření rolí pracovního procesu a webu PHP](../cloud-services-php-create-web-role.md)
### [Sestavení a nasazení aplikace Node.js do Azure](cloud-services-nodejs-develop-deploy-app.md)
### [Sestavení webové aplikace Node.js pomocí Expressu](cloud-services-nodejs-develop-deploy-express-app.md)
### Služba Storage a Visual Studio
#### [Blob Storage a propojené služby](../storage/vs-storage-cloud-services-getting-started-blobs.md)
#### [Queue Storage a propojené služby](../storage/vs-storage-cloud-services-getting-started-queues.md)
#### [Table Storage a propojené služby](../storage/vs-storage-cloud-services-getting-started-tables.md)
### Konfigurace balíčků pro nepřetržité sestavování a nasazování
#### [Visual Studio Team Services a Git](cloud-services-continuous-delivery-use-vso-git.md)
#### [Visual Studio Team Services](cloud-services-continuous-delivery-use-vso.md)
#### [TFS a týmové sestavení](cloud-services-dotnet-continuous-delivery.md)
### [Konfigurace pravidel přenosů pro roli](cloud-services-enable-communication-role-instances.md)
### [Zpracování událostí životního cyklu cloudových služeb](cloud-services-role-lifecycle-dotnet.md)
### [Socket.io (Node.js)](cloud-services-nodejs-chat-app-socketio.md)
### [Použití Twilia k telefonování (.NET)](../partner-twilio-cloud-services-dotnet-phone-call-web-role.md)
### [New Relic](../store-new-relic-cloud-services-dotnet-application-performance-management.md)

### Konfigurace úloh při spuštění
#### [Vytvoření úloh při spuštění](cloud-services-startup-tasks.md)
#### [Běžné úlohy při spuštění](cloud-services-startup-tasks-common.md)
#### [Použití úlohy pro instalaci .NET v roli cloudové služby](cloud-services-dotnet-install-dotnet.md)

### Konfigurace vzdálené plochy
#### [Azure Portal](cloud-services-role-enable-remote-desktop-new-portal.md)
#### [Portál Classic](cloud-services-role-enable-remote-desktop.md)
#### [PowerShell](cloud-services-role-enable-remote-desktop-powershell.md)

## Nasazení
### Vytvoření a nasazení cloudové služby na portálu
#### [Azure Portal](cloud-services-how-to-create-deploy-portal.md)
#### [Portál Classic](cloud-services-how-to-create-deploy.md)
### [Vytvoření prázdného kontejneru cloudové služby v PowerShellu](cloud-services-powershell-create-cloud-container.md)
### Konfigurace názvu vlastní domény
#### [Azure Portal](cloud-services-custom-domain-name-portal.md)
#### [Portál Classic](cloud-services-custom-domain-name.md)
### [Příprava nasazení cloudové služby (Node.js)](cloud-services-nodejs-stage-application.md)
### [Připojení k vlastnímu řadiči domény](cloud-services-connect-to-custom-domain.md)

## Správa služby
### Běžné úlohy správy
#### [Azure Portal](cloud-services-how-to-manage-portal.md)
#### [Portál Classic](cloud-services-how-to-manage.md)
### Konfigurace cloudové služby
#### [Azure Portal](cloud-services-how-to-configure-portal.md)
#### [Portál Classic](cloud-services-how-to-configure.md)
### [Správa cloudové služby pomocí Azure Automation](automation-manage-cloud-services.md)
### Konfigurace automatického škálování
#### [Azure Portal](cloud-services-how-to-scale-portal.md)
#### [Portál Classic](cloud-services-how-to-scale.md)
### [Použití Pythonu ke správě prostředků Azure](cloud-services-python-how-to-use-service-management.md)

### [Opravy hostovaného operačního systému](cloud-services-guestos-msrc-releases.md)
### Vyřazení hostovaného operačního systému
#### [Zásady vyřazení](cloud-services-guestos-retirement-policy.md)
#### [Oznámení o vyřazení řady 1 z provozu](cloud-services-guestos-family1-retirement.md)
### [Příspěvky k vydání hostovaného operačního systému](cloud-services-guestos-update-matrix.md)
### [Tahák Xpath pro konfigurace rolí Cloud Services](cloud-services-role-config-xpath.md)

## Správa certifikátů
### [Cloud Services a správa certifikátů](cloud-services-certs-create.md)
### Konfigurace SSL 
#### [Azure Portal](cloud-services-configure-ssl-certificate-portal.md)
#### [Portál Classic](cloud-services-configure-ssl-certificate.md)

## Monitorování
### [Monitorování cloudové služby](cloud-services-how-to-monitor.md)
### [Testování výkonu](../vs-azure-tools-performance-profiling-cloud-services.md)
#### [Testování pomocí profileru sady Visual Studio](cloud-services-performance-testing-visual-studio-profiler.md)
### Povolení diagnostiky
#### [PowerShell](cloud-services-diagnostics-powershell.md)
#### [.NET](cloud-services-dotnet-diagnostics.md)
#### [Visual Studio](../vs-azure-tools-diagnostics-for-cloud-services-and-virtual-machines.md)
### [Použití čítačů výkonu v Diagnostice Azure](cloud-services-dotnet-diagnostics-performance-counters.md)
### [Ukládání a zobrazení diagnostických dat v Azure Storage](cloud-services-dotnet-diagnostics-storage.md)
### [Trasování cloudové služby pomocí diagnostiky](cloud-services-dotnet-diagnostics-trace-flow.md)
### [Odesílání diagnostických dat do App Insights](cloud-services-dotnet-diagnostics-applicationinsights.md)

## Řešení potíží
### Ladění 
#### [Povolení vzdáleného ladění s průběžným doručováním](cloud-services-virtual-machines-dotnet-continuous-delivery-remote-debugging.md)
#### [Možnosti pro cloudovou službu](../vs-azure-tools-debugging-cloud-services-overview.md)
#### [Místní cloudová služba s využitím sady Visual Studio](../vs-azure-tools-debug-cloud-services-virtual-machines.md)
#### [Publikovaná cloudová služba s využitím sady Visual Studio](../vs-azure-tools-intellitrace-debug-published-cloud-services.md)
### [Chyba přidělení cloudové služby](cloud-services-allocation-failures.md)
### [Běžné příčiny recyklování rolí cloudové služby](cloud-services-troubleshoot-common-issues-which-cause-roles-recycle.md)
### [Výchozí velikost složky TEMP je pro roli příliš malá](cloud-services-troubleshoot-default-temp-folder-size-too-small-web-worker-role.md)
### [Běžné problémy s nasazením](cloud-services-troubleshoot-deployment-problems.md)
### [Role se nespustila](cloud-services-troubleshoot-roles-that-fail-start.md)
### [Pokyny k zotavení](cloud-services-disaster-recovery-guidance.md)
### Cloud Services – nejčastější dotazy
#### [Dostupnost aplikací a služeb – nejčastější dotazy](cloud-services-application-and-service-availability-faq.md)
#### [Konfigurace a správa – nejčastější dotazy](cloud-services-configuration-and-management-faq.md)
#### [Možnosti připojení a sítě – nejčastější dotazy](cloud-services-connectivity-and-networking-faq.md)
#### [Nasazení – nejčastější dotazy](cloud-services-deployment-faq.md)

# Referenční informace
## [.csdef XMLSchema](https://msdn.microsoft.com/library/azure/ee758711)
## [.cscfg XMLSchema](https://msdn.microsoft.com/library/azure/ee758710)
## [REST](https://msdn.microsoft.com/library/azure/ee460812)

# Zdroje a prostředky
## [Plány Azure do budoucna](https://azure.microsoft.com/roadmap/)
## [Postup výuky](https://azure.microsoft.com/documentation/learning-paths/cloud-services/)
## [Fórum MSDN](https://social.msdn.microsoft.com/Forums/en-us/home?forum=windowsazuredevelopment)
## [Ceny](https://azure.microsoft.com/pricing/details/cloud-services/)
## [Aktualizace služeb](https://azure.microsoft.com/updates/?product=cloud-services&updatetype=&platform=)
## [Videa](https://azure.microsoft.com/documentation/videos/index/?services=cloud-services)
