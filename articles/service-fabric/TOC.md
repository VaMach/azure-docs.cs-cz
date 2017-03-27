# Přehled
## [Co je Service Fabric?](service-fabric-overview.md)
## [Principy mikroslužeb](service-fabric-overview-microservices.md)
## [Scénáře aplikací](service-fabric-application-scenarios.md)
## [Architektura](service-fabric-architecture.md)
## [Terminologie](service-fabric-technical-overview.md)
## [Mapa obsahu](service-fabric-content-roadmap.md)

# Začínáme
## Nastavení vývojového prostředí
### [Windows](service-fabric-get-started.md)
### [Linux](service-fabric-get-started-linux.md)
### [Mac OS](service-fabric-get-started-mac.md)
## Vytvoření první aplikace
### [C# v systému Windows](service-fabric-create-your-first-application-in-visual-studio.md)
### [Java v Linuxu](service-fabric-create-your-first-linux-application-with-java.md)
### [C# v Linuxu](service-fabric-create-your-first-linux-application-with-csharp.md)
## [Nasazení aplikací v místním clusteru](service-fabric-get-started-with-a-local-cluster.md)

# Postup
## Sestavení aplikace
### [Vzory a scénáře](service-fabric-patterns-and-scenarios.md)
### Základy
#### [Aplikační model](service-fabric-application-model.md)
#### [Podporovaný programovací model](service-fabric-choose-framework.md)
#### [Stav služby](service-fabric-concepts-state.md)
#### [Komunikace služeb](service-fabric-connect-and-communicate-with-services.md)
#### [Přidání webového front-endu](service-fabric-add-a-web-frontend.md)
#### [Prostředky manifestu služby](service-fabric-service-manifest-resources.md)
#### [Správa aplikací v sadě Visual Studio](service-fabric-manage-application-in-visual-studio.md)
#### [Konfigurace zabezpečených připojení v sadě Visual Studio](service-fabric-visualstudio-configure-secure-connections.md)
#### Ladění
##### [Ladění služby C# ve VS](service-fabric-debugging-your-application.md)
##### [Ladění služby Java v Eclipse](service-fabric-debugging-your-application-java.md)
#### Monitorování a diagnóza
##### [Windows](service-fabric-diagnostics-how-to-monitor-and-diagnose-services-locally.md)
##### [Linux](service-fabric-diagnostics-how-to-monitor-and-diagnose-services-locally-linux.md)
#### [Správa tajných klíčů aplikací](service-fabric-application-secret-management.md)  
#### [Konfigurace zásad zabezpečení pro aplikaci](service-fabric-application-runas-security.md)  
#### [Konfigurace aplikace pro více prostředí](service-fabric-manage-multiple-environment-app-configuration.md)  
#### [Běžné chyby a výjimky](service-fabric-errors-and-exceptions.md) 

### Aplikace spustitelná hostem
#### [Nasazení hostujícího spustitelného souboru](service-fabric-deploy-existing-app.md)
#### [Nasazení několika hostujících spustitelných souborů](service-fabric-deploy-multiple-apps.md)

### Aplikace typu kontejner
#### [Přehled](service-fabric-containers-overview.md)
#### [Nasazení kontejneru s Windows](service-fabric-deploy-container.md)
#### [Nasazení kontejneru s Dockerem](service-fabric-deploy-container-linux.md)

### Aplikace Reliable Services
#### [Přehled](service-fabric-reliable-services-introduction.md)
#### Začínáme
##### [C# v systému Windows](service-fabric-reliable-services-quick-start.md)
##### [Java v Linuxu](service-fabric-reliable-services-quick-start-java.md)
#### [Životní cyklus Reliable Services](service-fabric-reliable-services-lifecycle.md)
#### [Reliable Collections](service-fabric-reliable-services-reliable-collections.md)
#### [Použití Reliable Collections](service-fabric-work-with-reliable-collections.md)
#### [Konfigurace](service-fabric-reliable-services-configuration.md)
#### [Oznámení](service-fabric-reliable-services-notifications.md)
#### [Zálohování a obnovení](service-fabric-reliable-services-backup-restore.md)
#### [Komunikace s Reliable Services](service-fabric-reliable-services-communication.md)
#### [Zabezpečená komunikace s Reliable Services](service-fabric-reliable-services-secure-communication.md)
##### [ASP.NET](service-fabric-reliable-services-communication-webapi.md)
##### [Vzdálená komunikace služby](service-fabric-reliable-services-communication-remoting.md)
##### [WCF](service-fabric-reliable-services-communication-wcf.md)
##### [Reverzní proxy server](service-fabric-reverseproxy.md)
#### [Pokročilé využití](service-fabric-reliable-services-advanced-usage.md)

### Aplikace Reliable Actors
#### [Přehled](service-fabric-reliable-actors-introduction.md)
#### Začínáme
##### [C# v systému Windows](service-fabric-reliable-actors-get-started.md)
##### [Java v Linuxu](service-fabric-reliable-actors-get-started-java.md)
#### [Architektura](service-fabric-reliable-actors-platform.md)
#### [Životní cyklus a uvolňování paměti](service-fabric-reliable-actors-lifecycle.md)
#### [Polymorfismus](service-fabric-reliable-actors-polymorphism.md)
#### [Vícenásobný přístup](service-fabric-reliable-actors-reentrancy.md)
#### [Události](service-fabric-reliable-actors-events.md) 
#### [Časovače a připomenutí](service-fabric-reliable-actors-timers-reminders.md)
#### [Správa stavu](service-fabric-reliable-actors-state-management.md)
#### [Konfigurace KvsActorStateProvider](service-fabric-reliable-actors-kvsactorstateprovider-configuration.md)
#### [Serializace typu](service-fabric-reliable-actors-notes-on-actor-type-serialization.md)
#### [Konfigurace nastavení komunikace](service-fabric-reliable-actors-fabrictransportsettings.md) 
#### [Konfigurace ReliableDictionaryActorStateProvider](service-fabric-reliable-actors-reliabledictionarystateprovider-configuration.md)

## Migrace ze služby Cloud Services
### [Porovnání služby Cloud Services se Service Fabric](service-fabric-cloud-services-migration-differences.md)
### [Migrace do Service Fabric](service-fabric-cloud-services-migration-worker-role-stateless-service.md)

## Vytvoření a správa clusterů

### Základy
#### [Přehled](service-fabric-deploy-anywhere.md)
#### [Popis clusteru](service-fabric-cluster-resource-manager-cluster-description.md)
#### [Plánování kapacity](service-fabric-cluster-capacity.md)
#### [Vizualizace clusteru](service-fabric-visualizing-your-cluster.md)
#### [Připojení k zabezpečenému clusteru](service-fabric-connect-to-secure-cluster.md)
#### [Správa clusteru pomocí rozhraní příkazového řádku Azure](service-fabric-azure-cli.md) 
#### [Zabezpečení clusteru](service-fabric-cluster-security.md)
#### [Zotavení po havárii](service-fabric-disaster-recovery.md)

### Clustery v Azure
#### Vytvoření clusteru v Azure
##### [Azure Portal](service-fabric-cluster-creation-via-portal.md)
##### [Azure Resource Manager](service-fabric-cluster-creation-via-arm.md)
##### [Visual Studio a Azure Resource Manager](service-fabric-cluster-creation-via-visual-studio.md)
#### [Vzory zapojení clusteru do sítě](service-fabric-patterns-networking.md)
#### [Typy uzlů a škálovací sady virtuálních počítačů](service-fabric-cluster-nodetypes.md)
#### [Škálování clusteru](service-fabric-cluster-scale-up-down.md)
#### [Škálování clusteru prostřednictvím kódu programu](service-fabric-cluster-programmatic-scaling.md)
#### [Upgrade clusteru](service-fabric-cluster-upgrade.md)
#### [Odstranění clusteru](service-fabric-cluster-delete.md)
#### [Řízení přístupu](service-fabric-cluster-security-roles.md)
#### [Konfigurace clusteru](service-fabric-cluster-fabric-settings.md)
#### [Přidání nebo výměna certifikátů clusteru](service-fabric-cluster-security-update-certs-azure.md) 
#### [Bezplatné vyzkoušení Party Clusteru](http://aka.ms/tryservicefabric)

### Samostatné clustery
#### [Plánování a příprava nasazení](service-fabric-cluster-standalone-deployment-preparation.md)
#### [Obsah samostatného balíčku Service Fabric](service-fabric-cluster-standalone-package-contents.md)
#### [Vytvoření samostatného clusteru](service-fabric-cluster-creation-for-windows-server.md)
#### [Vytvoření samostatného clusteru v Azure Virtual Machines](service-fabric-cluster-creation-with-windows-azure-vms.md)
#### [Škálování clusteru](service-fabric-cluster-windows-server-add-remove-nodes.md)
#### [Upgrade clusteru](service-fabric-cluster-upgrade-windows-server.md)
#### [Řízení přístupu](service-fabric-cluster-security-roles.md)
#### [Konfigurace clusteru](service-fabric-cluster-manifest.md)
#### [Zabezpečení clusteru pomocí certifikátů](service-fabric-windows-cluster-x509-security.md)  
#### [Zabezpečení clusteru pomocí zabezpečení systému Windows](service-fabric-windows-cluster-windows-security.md) 

## Správa životního cyklu aplikace
### [Přehled](service-fabric-application-lifecycle.md)
### [Nastavení průběžné integrace pomocí VSTS](service-fabric-set-up-continuous-integration.md)
### [Nasazení linuxové aplikace v Javě pomocí Jenkinse](service-fabric-cicd-your-linux-java-application-with-jenkins.md)
### [Principy nastavení ImageStoreConnectionString](service-fabric-image-store-connection-string.md)
### Nasazení nebo odebrání aplikací
#### [PowerShell](service-fabric-deploy-remove-applications.md)
#### [Visual Studio](service-fabric-publish-app-remote-cluster.md)
### [Přehled upgradu aplikace](service-fabric-application-upgrade.md)
### [Konfigurace upgradu aplikace](service-fabric-visualstudio-configure-upgrade.md)
### [Parametry upgradu aplikace](service-fabric-application-upgrade-parameters.md)
### Upgrade aplikace
#### [PowerShell](service-fabric-application-upgrade-tutorial-powershell.md)
#### [Visual Studio](service-fabric-application-upgrade-tutorial.md)
### [Řešení potíží s upgrady aplikací](service-fabric-application-upgrade-troubleshooting.md)
### [Serializace dat v upgradech aplikací](service-fabric-application-upgrade-data-serialization.md)
### [Pokročilá témata týkající se upgradů aplikací](service-fabric-application-upgrade-advanced.md)

## Kontrola stavu aplikace a clusteru
### [Monitorování stavu Service Fabric](service-fabric-health-introduction.md)
### [Hlášení a kontrola stavu služeb](service-fabric-diagnostics-how-to-report-and-check-service-health.md)
### [Přidání vlastních stavových sestav](service-fabric-report-health.md)
### [Řešení potíží se systémovými stavovými sestavami](service-fabric-understand-and-troubleshoot-with-system-health-reports.md)
### [Zobrazení stavových sestav](service-fabric-view-entities-aggregated-health.md)

## Monitorování a diagnóza
### [Monitorování a diagnostika aplikací](service-fabric-diagnostics-overview.md)
### Místní monitorování a diagnostika služeb
#### [Windows](service-fabric-diagnostics-how-to-monitor-and-diagnose-services-locally.md)
#### [Linux](service-fabric-diagnostics-how-to-monitor-and-diagnose-services-locally-linux.md)
### Protokoly diagnostiky Azure
#### [Windows](service-fabric-diagnostics-how-to-setup-wad.md)
#### [Linux](service-fabric-diagnostics-how-to-setup-lad.md)
### [Shromažďování protokolů z procesu služeb](service-fabric-diagnostic-collect-logs-without-an-agent.md)
### [Diagnostika ve stavovém modelu Reliable Services](service-fabric-reliable-services-diagnostics.md)
### [Diagnostika v Reliable Actors](service-fabric-reliable-actors-diagnostics.md)
### [Řešení potíží s místním clusterem](service-fabric-troubleshoot-local-cluster-setup.md)

## Škálování aplikací
### [Dělení Reliable Services](service-fabric-concepts-partitioning.md)
### [Dostupnost služeb](service-fabric-availability-services.md)
### [Škálování aplikací](service-fabric-concepts-scalability.md)
### [Plánování kapacity aplikací](service-fabric-capacity-planning.md)

## Testování aplikací a služeb
### [Přehled analýzy chyb](service-fabric-testability-overview.md)
### [Testování komunikace mezi službami](service-fabric-testability-scenarios-service-communication.md)
### Simulace chyb
#### [Pomocí řízeného chaosu](service-fabric-controlled-chaos.md)
#### [Pomocí testovacích akcí](service-fabric-testability-actions.md)
#### [Během úloh](service-fabric-testability-workload-tests.md)
#### [Pomocí testovacích scénářů](service-fabric-testability-scenarios.md)
#### [Pomocí rozhraní API pro přechod uzlů](service-fabric-node-transition-apis.md)
### [Zátěžový test aplikace](service-fabric-vso-load-test.md)

## Správa a orchestrace prostředků clusteru
### [Přehled Cluster Resource Manageru](service-fabric-cluster-resource-manager-introduction.md)
### [Architektura Cluster Resource Manageru](service-fabric-cluster-resource-manager-architecture.md)
### [Popis clusteru](service-fabric-cluster-resource-manager-cluster-description.md)
### [Přehled skupin aplikací](service-fabric-cluster-resource-manager-application-groups.md)
### [Konfigurace nastavení Cluster Resource Manageru](service-fabric-cluster-resource-manager-configure-services.md)
### [Metriky spotřeby prostředků](service-fabric-cluster-resource-manager-metrics.md)
### [Použití spřažení služeb](service-fabric-cluster-resource-manager-advanced-placement-rules-affinity.md)
### [Zásady umístění služeb](service-fabric-cluster-resource-manager-advanced-placement-rules-placement-policies.md)
### [Správa clusteru](service-fabric-cluster-resource-manager-management-integration.md)
### [Defragmentace clusteru](service-fabric-cluster-resource-manager-defragmentation-metrics.md)
### [Vyvážení clusteru](service-fabric-cluster-resource-manager-balancing.md)
### [Omezování](service-fabric-cluster-resource-manager-advanced-throttling.md)
### [Přesun služeb](service-fabric-cluster-resource-manager-movement-cost.md)

# Referenční informace
## [PowerShell](//powershell/servicefabric/vlatest/servicefabric)
## [Rozhraní API Java](/java/api/microsoft.servicefabric.services)
## [.NET](/dotnet/api/microsoft.servicefabric.services)
## [REST](/rest/api/servicefabric)

# Zdroje a prostředky
## [Běžné otázky týkající se Service Fabric](service-fabric-common-questions.md)
## [Možnosti podpory pro Service Fabric](service-fabric-support.md)
## [Ukázka kódu](http://aka.ms/servicefabricsamples)
## [Postup výuky](https://azure.microsoft.com/documentation/learning-paths/service-fabric/)
## [Ceny](https://azure.microsoft.com/pricing/details/service-fabric/)
## [Aktualizace služby](https://azure.microsoft.com/updates/?product=service-fabric)
## [Fórum MSDN](https://social.msdn.microsoft.com/Forums/home?forum=AzureServiceFabric)
## [Videa](https://azure.microsoft.com/documentation/videos/index/?services=service-fabric)
