
# Přehled
## [Co je Service Fabric?](service-fabric-overview.md)
## [Principy mikroslužeb](service-fabric-overview-microservices.md)
## [Scénáře aplikací](service-fabric-application-scenarios.md)
## [Vzory a scénáře](service-fabric-patterns-and-scenarios.md)
## [Architektura](service-fabric-architecture.md)
## [Terminologie](service-fabric-technical-overview.md)
## [Základy](service-fabric-content-roadmap.md)

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
## [Vytvoření vašeho prvního clusteru v Azure](service-fabric-get-started-azure-cluster.md)
## [Vytvoření vašeho prvního samostatného clusteru](service-fabric-get-started-standalone-cluster.md)

# Postup
## Sestavení aplikace
  
### Koncepty
#### [Podporované programovací modely](service-fabric-choose-framework.md)
#### [Aplikační model](service-fabric-application-model.md)
#### [Prostředky manifestu služby](service-fabric-service-manifest-resources.md)
#### [Stav služby](service-fabric-concepts-state.md)
#### [Dělení služby](service-fabric-concepts-partitioning.md)
#### [Dostupnost služeb](service-fabric-availability-services.md)
#### [Škálovatelnost aplikací](service-fabric-concepts-scalability.md)
#### [Jádro ASP.NET](service-fabric-reliable-services-communication-aspnetcore.md)

### [Plánování kapacity aplikací](service-fabric-capacity-planning.md)

### Sestavení služby spustitelné hostem
#### [Nasazení hostujícího spustitelného souboru](service-fabric-deploy-existing-app.md)
#### [Nasazení několika hostujících spustitelných souborů](service-fabric-deploy-multiple-apps.md)

### Sestavení služby kontejneru
#### [Přehled](service-fabric-containers-overview.md)
#### [Nasazení kontejneru s Windows](service-fabric-deploy-container.md)
#### [Nasazení kontejneru Linuxu](service-fabric-deploy-container-linux.md)

### Sestavení služby s modelem Reliable Service
#### [Přehled](service-fabric-reliable-services-introduction.md)
#### Koncepty
##### [Životní cyklus Reliable Services – C#](service-fabric-reliable-services-lifecycle.md)
##### [Životní cyklus Reliable Services – Java](service-fabric-reliable-services-lifecycle-java.md)
##### [Reliable Collections](service-fabric-reliable-services-reliable-collections.md)

#### Začínáme
##### [C# v systému Windows](service-fabric-reliable-services-quick-start.md)
##### [Java v Linuxu](service-fabric-reliable-services-quick-start-java.md)

#### Životní cyklus Reliable Services
#### [Použití Reliable Collections](service-fabric-work-with-reliable-collections.md)
#### [Konfigurace](service-fabric-reliable-services-configuration.md)
#### [Odeslání oznámení](service-fabric-reliable-services-notifications.md)
#### [Zálohování a obnovení](service-fabric-reliable-services-backup-restore.md)

#### Komunikace se službami
##### [Komunikace s Reliable Services](service-fabric-reliable-services-communication.md)
##### [Vzdálená komunikace služby – C#](service-fabric-reliable-services-communication-remoting.md)
##### [Vzdálená komunikace služby – Java](service-fabric-reliable-services-communication-remoting-java.md)
##### [WCF](service-fabric-reliable-services-communication-wcf.md)
##### [Zabezpečená komunikace – C#](service-fabric-reliable-services-secure-communication.md)
##### [Zabezpečená komunikace – Java](service-fabric-reliable-services-secure-communication-java.md)

#### [Pokročilé využití](service-fabric-reliable-services-advanced-usage.md)

### Sestavení služby s objekty Reliable Actor
#### [Přehled](service-fabric-reliable-actors-introduction.md)
#### Koncepty
#### [Architektura](service-fabric-reliable-actors-platform.md)
#### [Životní cyklus a uvolňování paměti](service-fabric-reliable-actors-lifecycle.md)
#### [Správa stavu](service-fabric-reliable-actors-state-management.md)
#### [Polymorfismus](service-fabric-reliable-actors-polymorphism.md)
#### [Vícenásobný přístup](service-fabric-reliable-actors-reentrancy.md)
#### [Serializace typu](service-fabric-reliable-actors-notes-on-actor-type-serialization.md)

#### Začínáme
##### [C# v systému Windows](service-fabric-reliable-actors-get-started.md)
##### [Java v Linuxu](service-fabric-reliable-actors-get-started-java.md)

#### [Odeslání oznámení](service-fabric-reliable-actors-events.md) 
#### [Nastavení časovačů a připomenutí](service-fabric-reliable-actors-timers-reminders.md)
#### [Konfigurace KvsActorStateProvider](service-fabric-reliable-actors-kvsactorstateprovider-configuration.md)
#### [Konfigurace nastavení komunikace](service-fabric-reliable-actors-fabrictransportsettings.md) 
#### [Konfigurace ReliableDictionaryActorStateProvider](service-fabric-reliable-actors-reliabledictionarystateprovider-configuration.md)

### Komunikace se službami
#### [Komunikace služeb](service-fabric-connect-and-communicate-with-services.md)
#### [Reverzní proxy server](service-fabric-reverseproxy.md)

### [Přidání webového front-endu](service-fabric-add-a-web-frontend.md)

### Práce v integrovaném vývojovém prostředí (IDE)
#### [Začínáme s modulem plug-in Eclipse pro Java Development](service-fabric-get-started-eclipse.md)
#### [Správa aplikací v sadě Visual Studio](service-fabric-manage-application-in-visual-studio.md)
#### [Konfigurace zabezpečených připojení v sadě Visual Studio](service-fabric-visualstudio-configure-secure-connections.md)
#### [Konfigurace aplikace pro více prostředí](service-fabric-manage-multiple-environment-app-configuration.md)

### Konfigurace zabezpečení
#### [Správa tajných klíčů aplikací](service-fabric-application-secret-management.md)  
#### [Konfigurace zásad zabezpečení pro aplikaci](service-fabric-application-runas-security.md)

### Ladění
#### [Ladění služby C# ve VS](service-fabric-debugging-your-application.md)
#### [Ladění služby Java v Eclipse](service-fabric-debugging-your-application-java.md)
#### [Běžné chyby a výjimky](service-fabric-errors-and-exceptions.md)

### Místní monitorování a diagnostika
#### [Windows](service-fabric-diagnostics-how-to-monitor-and-diagnose-services-locally.md)
#### [Linux](service-fabric-diagnostics-how-to-monitor-and-diagnose-services-locally-linux.md)

### Migrace ze služby Cloud Services
#### [Porovnání služby Cloud Services se Service Fabric](service-fabric-cloud-services-migration-differences.md)
#### [Migrace do Service Fabric](service-fabric-cloud-services-migration-worker-role-stateless-service.md)
#### [Doporučené postupy](/azure/architecture/service-fabric/migrate-from-cloud-services)

## Správa životního cyklu aplikace
### [Přehled](service-fabric-application-lifecycle.md)
### [Balení aplikace](service-fabric-package-apps.md)
### [Principy nastavení ImageStoreConnectionString](service-fabric-image-store-connection-string.md)
### Nasazení nebo odebrání aplikací
#### [PowerShell](service-fabric-deploy-remove-applications.md)
#### [Visual Studio](service-fabric-publish-app-remote-cluster.md)
#### [Rozhraní API FabricClient](service-fabric-deploy-remove-applications-fabricclient.md)
### Upgrade aplikace
#### [Přehled](service-fabric-application-upgrade.md)
#### [Konfigurace upgradu aplikace](service-fabric-visualstudio-configure-upgrade.md)
#### [Parametry upgradu aplikace](service-fabric-application-upgrade-parameters.md)
#### Upgrade
##### [PowerShell](service-fabric-application-upgrade-tutorial-powershell.md)
##### [Visual Studio](service-fabric-application-upgrade-tutorial.md)
#### [Řešení potíží s upgrady aplikací](service-fabric-application-upgrade-troubleshooting.md)
#### [Serializace dat v upgradech aplikací](service-fabric-application-upgrade-data-serialization.md)
#### [Pokročilá témata týkající se upgradů aplikací](service-fabric-application-upgrade-advanced.md)

### Testování aplikací a služeb
#### [Přehled analýzy chyb](service-fabric-testability-overview.md)
#### [Testování komunikace mezi službami](service-fabric-testability-scenarios-service-communication.md)
#### Simulace chyb
##### [Pomocí řízeného chaosu](service-fabric-controlled-chaos.md)
##### [Pomocí testovacích akcí](service-fabric-testability-actions.md)
##### [Během úloh](service-fabric-testability-workload-tests.md)
##### [Pomocí testovacích scénářů](service-fabric-testability-scenarios.md)
##### [Pomocí rozhraní API pro přechod uzlů](service-fabric-node-transition-apis.md)
#### [Zátěžový test aplikace](service-fabric-vso-load-test.md)

### Nastavení průběžné integrace
#### [Nastavení průběžné integrace pomocí VSTS](service-fabric-set-up-continuous-integration.md)
#### [Nasazení linuxové aplikace v Javě pomocí Jenkinse](service-fabric-cicd-your-linux-java-application-with-jenkins.md)

## Vytvoření a správa clusterů

### [Přehled](service-fabric-deploy-anywhere.md)
### Koncepty
#### [Popis clusteru](service-fabric-cluster-resource-manager-cluster-description.md)
#### [Zabezpečení clusteru](service-fabric-cluster-security.md)
#### [Rozdíly ve funkcích mezi systémy Linux a Windows](service-fabric-linux-windows-differences.md)

### Plánování a příprava
#### [Plánování kapacity](service-fabric-cluster-capacity.md)
#### [Zotavení po havárii](service-fabric-disaster-recovery.md)

### Clustery v Azure
#### Koncepty
##### [Typy uzlů a škálovací sady virtuálních počítačů](service-fabric-cluster-nodetypes.md)
##### [Vzory zapojení clusteru do sítě](service-fabric-patterns-networking.md)
#### Vytvořit 
##### [Azure Portal](service-fabric-cluster-creation-via-portal.md)
##### [Azure Resource Manager](service-fabric-cluster-creation-via-arm.md)
##### [Visual Studio a Azure Resource Manager](service-fabric-cluster-creation-via-visual-studio.md)
#### Měřítko 
##### [Ručně](service-fabric-cluster-scale-up-down.md)
##### [Prostřednictvím kódu programu](service-fabric-cluster-programmatic-scaling.md)
#### [Upgrade](service-fabric-cluster-upgrade.md)
#### [Nastavení řízení přístupu](service-fabric-cluster-security-roles.md)
#### [Konfigurace](service-fabric-cluster-fabric-settings.md)
#### [Správa certifikátů clusteru](service-fabric-cluster-security-update-certs-azure.md) 
#### [Odstranění](service-fabric-cluster-delete.md)

### Samostatné clustery
#### [Obsah samostatného balíčku](service-fabric-cluster-standalone-package-contents.md)
#### [Plánování a příprava nasazení](service-fabric-cluster-standalone-deployment-preparation.md)
#### Vytvořit
##### [Místní vytvoření](service-fabric-cluster-creation-for-windows-server.md)
##### [Vytvoření na virtuálních počítačích Azure](service-fabric-cluster-creation-with-windows-azure-vms.md)
##### [Zabezpečení pomocí certifikátů](service-fabric-windows-cluster-x509-security.md)  
##### [Zabezpečení pomocí zabezpečení systému Windows](service-fabric-windows-cluster-windows-security.md)
#### [Škálování](service-fabric-cluster-windows-server-add-remove-nodes.md)
#### [Nastavení řízení přístupu](service-fabric-cluster-security-roles.md)
#### [Konfigurace](service-fabric-cluster-manifest.md)
#### [Upgrade](service-fabric-cluster-upgrade-windows-server.md) 

### [Vizualizace clusteru](service-fabric-visualizing-your-cluster.md)
### [Připojení k zabezpečenému clusteru](service-fabric-connect-to-secure-cluster.md)

### [Správa clusteru pomocí rozhraní příkazového řádku Azure](service-fabric-azure-cli.md)

### Správa a orchestrace prostředků clusteru
#### [Přehled Cluster Resource Manageru](service-fabric-cluster-resource-manager-introduction.md)
#### [Architektura Cluster Resource Manageru](service-fabric-cluster-resource-manager-architecture.md)
#### [Popis clusteru](service-fabric-cluster-resource-manager-cluster-description.md)
#### [Přehled skupin aplikací](service-fabric-cluster-resource-manager-application-groups.md)
#### [Konfigurace nastavení Cluster Resource Manageru](service-fabric-cluster-resource-manager-configure-services.md)
#### [Metriky spotřeby prostředků](service-fabric-cluster-resource-manager-metrics.md)
#### [Použití spřažení služeb](service-fabric-cluster-resource-manager-advanced-placement-rules-affinity.md)
#### [Zásady umístění služeb](service-fabric-cluster-resource-manager-advanced-placement-rules-placement-policies.md)
#### [Správa clusteru](service-fabric-cluster-resource-manager-management-integration.md)
#### [Defragmentace clusteru](service-fabric-cluster-resource-manager-defragmentation-metrics.md)
#### [Vyvážení clusteru](service-fabric-cluster-resource-manager-balancing.md)
#### [Omezování](service-fabric-cluster-resource-manager-advanced-throttling.md)
#### [Přesun služeb](service-fabric-cluster-resource-manager-movement-cost.md)

### [Připojení k zabezpečenému clusteru](service-fabric-connect-to-secure-cluster.md)

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

# Referenční informace
## [PowerShell](/powershell/azure/overview?view=azureservicefabricps)
## [Rozhraní API Java](/java/api/)
## [.NET](/dotnet/api/)
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
