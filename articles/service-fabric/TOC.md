# [Dokumentace k Service Fabric](/azure/service-fabric)
# Přehled
## [Co je Service Fabric?](service-fabric-overview.md)

# Šablony Rychlý start
## [Vytvoření aplikace .NET](service-fabric-quickstart-dotnet.md)
## [Nasazení aplikace typu kontejner pro Windows](service-fabric-quickstart-containers.md)
## [Nasazení aplikace typu kontejner pro Linux](service-fabric-quickstart-containers-linux.md)

# Kurzy
## Nasazení aplikace .NET
### [1. Sestavení aplikace .NET](service-fabric-tutorial-create-dotnet-app.md)
### [2. Nasazení aplikace](service-fabric-tutorial-deploy-app-to-party-cluster.md)
### [3. Konfigurace CI/CD](service-fabric-tutorial-deploy-app-with-cicd-vsts.md)

## Migrace aplikace metodou „lift and shift“
### [1. Vytvoření zabezpečeného clusteru v Azure](service-fabric-tutorial-create-cluster-azure-ps.md)
### [2. Nasazení aplikace .NET pomocí Docker Compose](service-fabric-host-app-in-a-container.md)

# Ukázky
## [Ukázky kódu](https://azure.microsoft.com/en-us/resources/samples/?service=service-fabric)
## [PowerShell](service-fabric-powershell-samples.md)
## [Service Fabric CLI](samples-cli.md)
# Koncepty
## [Principy mikroslužeb](service-fabric-overview-microservices.md)
## [Celkový přehled](service-fabric-content-roadmap.md)
## [Scénáře aplikací](service-fabric-application-scenarios.md)
## [Vzory a scénáře](service-fabric-patterns-and-scenarios.md)
## [Architektura](service-fabric-architecture.md)
## [Terminologie](service-fabric-technical-overview.md)

## Sestavení aplikací a služeb
### Podporované programovací modely
#### [Přehled](service-fabric-choose-framework.md)
#### Kontejnery
##### [Přehled](service-fabric-containers-overview.md)
##### [Docker Compose (Preview)](service-fabric-docker-compose.md)
##### [Zásady správného řízení prostředků](service-fabric-resource-governance.md)
#### Reliable Services
##### [Přehled](service-fabric-reliable-services-introduction.md)
##### [Životní cyklus Reliable Services – C#](service-fabric-reliable-services-lifecycle.md)
##### [Životní cyklus Reliable Services – Java](service-fabric-reliable-services-lifecycle-java.md)
##### [Reliable Collections](service-fabric-reliable-services-reliable-collections.md)
##### [Pokyny a doporučení k Reliable Collection](service-fabric-reliable-services-reliable-collections-guidelines.md)
##### [Práce s Reliable Collections](service-fabric-work-with-reliable-collections.md)
##### [Transakce a zámky](service-fabric-reliable-services-reliable-collections-transactions-locks.md)
##### [Spolehlivá souběžná fronta](service-fabric-reliable-services-reliable-concurrent-queue.md)
##### [Spolehlivá serializace kolekcí](service-fabric-reliable-services-reliable-collections-serialization.md)
##### [Interní informace Reliable State Manager a Reliable Collection](service-fabric-reliable-services-reliable-collections-internals.md)
##### [Pokročilé využití](service-fabric-reliable-services-advanced-usage.md)

#### Reliable Actors
##### [Přehled](service-fabric-reliable-actors-introduction.md)
##### [Architektura](service-fabric-reliable-actors-platform.md)
##### [Životní cyklus a uvolňování paměti](service-fabric-reliable-actors-lifecycle.md)
##### [Správa stavu](service-fabric-reliable-actors-state-management.md)
##### [Polymorfismus](service-fabric-reliable-actors-polymorphism.md)
##### [Vícenásobný přístup](service-fabric-reliable-actors-reentrancy.md)
##### [Serializace typu](service-fabric-reliable-actors-notes-on-actor-type-serialization.md)

### [Aplikační model](service-fabric-application-model.md)
### [Model hostingu](service-fabric-hosting-model.md)

### Služby
#### [Prostředky služby](service-fabric-service-manifest-resources.md)
#### [Stav služby](service-fabric-concepts-state.md)
#### [Dělení služby](service-fabric-concepts-partitioning.md)
#### [Dostupnost služeb](service-fabric-availability-services.md)
#### Komunikace služeb
##### [Přehled](service-fabric-connect-and-communicate-with-services.md)
##### [Služba DNS](service-fabric-dnsservice.md)
##### [Reverzní proxy server](service-fabric-reverseproxy.md)
##### [Konfigurace reverzního proxy serveru pro zabezpečenou komunikaci](service-fabric-reverseproxy-configure-secure-communication.md)
##### [Diagnostika reverzního proxy serveru](service-fabric-reverse-proxy-diagnostics.md)
### [Škálovatelnost aplikací](service-fabric-concepts-scalability.md)
### [Jádro ASP.NET](service-fabric-reliable-services-communication-aspnetcore.md)

### [Plánování kapacity aplikace](service-fabric-capacity-planning.md)

## Správa aplikací
### [Přehled](service-fabric-application-lifecycle.md)
### [Nastavení ImageStoreConnectionString](service-fabric-image-store-connection-string.md)
### Upgrade aplikací
#### [Přehled](service-fabric-application-upgrade.md)
#### [Konfigurace](service-fabric-visualstudio-configure-upgrade.md)
#### [Parametry upgradu aplikace](service-fabric-application-upgrade-parameters.md)
#### [Serializace dat v upgradech aplikací](service-fabric-application-upgrade-data-serialization.md)
#### [Pokročilá témata týkající se upgradů aplikací](service-fabric-application-upgrade-advanced.md)
### [Přehled analýzy chyb](service-fabric-testability-overview.md)

## Vytvoření a správa clusterů
### [Přehled](service-fabric-deploy-anywhere.md)
### [Service Fabric v Linuxu](service-fabric-linux-overview.md)
### Plánování a příprava
#### [Plánování kapacity](service-fabric-cluster-capacity.md)
#### [Zotavení po havárii](service-fabric-disaster-recovery.md)
### [Popis clusteru](service-fabric-cluster-resource-manager-cluster-description.md)
### [Zabezpečení clusteru](service-fabric-cluster-security.md)
### [Rozdíly ve funkcích mezi systémy Linux a Windows](service-fabric-linux-windows-differences.md)
### Clustery v Azure
#### [Typy uzlů a škálovací sady virtuálních počítačů](service-fabric-cluster-nodetypes.md)
#### [Vzory zapojení clusteru do sítě](service-fabric-patterns-networking.md)
### Cluster Resource Manager
#### [Přehled](service-fabric-cluster-resource-manager-introduction.md)
#### [Architektura](service-fabric-cluster-resource-manager-architecture.md)
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

## [Integrace se službou API Management](service-fabric-api-management-overview.md)

## Monitorování a diagnóza
### [Přehled](service-fabric-diagnostics-overview.md)
### [Model stavu](service-fabric-health-introduction.md)
### [Diagnostika ve stavovém modelu Reliable Services](service-fabric-reliable-services-diagnostics.md)
### [Diagnostika v Reliable Actors](service-fabric-reliable-actors-diagnostics.md)
### [Čítače výkonu pro vzdálenou komunikaci modelu Reliable Service](service-fabric-reliable-serviceremoting-diagnostics.md)

# Návody
## Nastavení vývojového prostředí
### [Windows](service-fabric-get-started.md)
### [Linux](service-fabric-get-started-linux.md)
### [Mac OS](service-fabric-get-started-mac.md)

## Sestavení aplikace
### [Vytvoření první aplikace C# v sadě Visual Studio](service-fabric-create-your-first-application-in-visual-studio.md)
### Sestavení služby spustitelné hostem
#### [Hostování aplikace Node.js ve Windows](quickstart-guest-app.md)
#### [Nasazení hostujícího spustitelného souboru](service-fabric-deploy-existing-app.md)
#### [Nasazení několika hostujících spustitelných souborů](service-fabric-deploy-multiple-apps.md)
### Sestavení služby kontejneru
#### [Vytvoření aplikace typu kontejner pro Windows](service-fabric-get-started-containers.md)
#### [Vytvoření aplikace typu kontejner pro Linux](service-fabric-get-started-containers-linux.md)
#### [Zabezpečení kontejneru](service-fabric-securing-containers.md)
#### [Docker Compose (Preview)](service-fabric-docker-compose.md)
#### [Zásady správného řízení prostředků pro kontejnery a služby](service-fabric-resource-governance.md)
#### [Svazek a ovladače protokolování](service-fabric-containers-volume-logging-drivers.md)
#### [Služby uvnitř kontejnerů](service-fabric-services-inside-containers.md)
#### [Síťové režimy kontejneru](service-fabric-networking-modes.md)

### Sestavení služby s modelem Reliable Services
#### [Přehled](service-fabric-reliable-services-introduction.md)
#### Koncepty
##### [Životní cyklus Reliable Services – C#](service-fabric-reliable-services-lifecycle.md)
##### [Životní cyklus Reliable Services – Java](service-fabric-reliable-services-lifecycle-java.md)

#### Reliable Collections
##### [Reliable Collections](service-fabric-reliable-services-reliable-collections.md)
##### [Pokyny a doporučení k Reliable Collection](service-fabric-reliable-services-reliable-collections-guidelines.md)
##### [Práce s Reliable Collections](service-fabric-work-with-reliable-collections.md)
##### [Transakce a zámky](service-fabric-reliable-services-reliable-collections-transactions-locks.md)
##### [Spolehlivá souběžná fronta](service-fabric-reliable-services-reliable-concurrent-queue.md)
##### [Spolehlivá serializace kolekcí](service-fabric-reliable-services-reliable-collections-serialization.md)
##### [Interní informace Reliable State Manager a Reliable Collection](service-fabric-reliable-services-reliable-collections-internals.md)

#### Začínáme
##### [C# v systému Windows](service-fabric-reliable-services-quick-start.md)
##### [Java v Linuxu](service-fabric-reliable-services-quick-start-java.md)
##### [Vytvoření aplikace C# v Linuxu](service-fabric-create-your-first-linux-application-with-csharp.md)
#### Komunikace se službami
##### [Komunikace s Reliable Services](service-fabric-reliable-services-communication.md)

##### [Vzdálená komunikace služby – C#](service-fabric-reliable-services-communication-remoting.md)
##### [Vzdálená komunikace služby – Java](service-fabric-reliable-services-communication-remoting-java.md)
##### [WCF](service-fabric-reliable-services-communication-wcf.md)
##### [Zabezpečená komunikace – C#](service-fabric-reliable-services-secure-communication.md)
##### [Zabezpečená komunikace – Java](service-fabric-reliable-services-secure-communication-java.md)

#### [Konfigurace](service-fabric-reliable-services-configuration.md)
#### [Odeslání oznámení](service-fabric-reliable-services-notifications.md)
#### [Zálohování a obnovení](service-fabric-reliable-services-backup-restore.md)

### Sestavení služby s modelem Reliable Actors
#### Začínáme
##### [C# v systému Windows](service-fabric-reliable-actors-get-started.md)
##### [Java v Linuxu](service-fabric-reliable-actors-get-started-java.md)
##### [Java Actor v Linuxu](service-fabric-create-your-first-linux-application-with-java.md)
#### [Odeslání oznámení](service-fabric-reliable-actors-events.md)
#### [Nastavení časovačů a připomenutí](service-fabric-reliable-actors-timers-reminders.md)
#### [Konfigurace KvsActorStateProvider](service-fabric-reliable-actors-kvsactorstateprovider-configuration.md)
#### [Konfigurace nastavení komunikace](service-fabric-reliable-actors-fabrictransportsettings.md)
#### [Konfigurace ReliableDictionaryActorStateProvider](service-fabric-reliable-actors-reliabledictionarystateprovider-configuration.md)

### [Migrace staré aplikace v Javě pro podporu Mavenu](service-fabric-migrate-old-javaapp-to-use-maven.md)

### [Konfigurace reverzního proxy serveru pro zabezpečenou komunikaci](service-fabric-reverseproxy-configure-secure-communication.md)

### [Sestavení služby ASP.NET Core](service-fabric-add-a-web-frontend.md)

### Konfigurace zabezpečení
#### [Správa tajných klíčů aplikací](service-fabric-application-secret-management.md)  
#### [Konfigurace zásad zabezpečení pro aplikaci](service-fabric-application-runas-security.md)

## Práce ve vývojovém prostředí Windows
### [Správa aplikací v sadě Visual Studio](service-fabric-manage-application-in-visual-studio.md)
### [Konfigurace zabezpečených připojení v sadě Visual Studio](service-fabric-visualstudio-configure-secure-connections.md)
### [Konfigurace aplikace pro více prostředí](service-fabric-manage-multiple-environment-app-configuration.md)
### [Ladění služby .NET ve VS](service-fabric-debugging-your-application.md)
### [Běžné chyby a výjimky](service-fabric-errors-and-exceptions.md)
### [Místní monitorování a diagnostika](service-fabric-diagnostics-how-to-monitor-and-diagnose-services-locally.md)

## Práce v linuxovém vývojovém prostředí
### [Začínáme s modulem plug-in Eclipse pro Java Development](service-fabric-get-started-eclipse.md)
### [Ladění služby Java v Eclipse](service-fabric-debugging-your-application-java.md)
### [Místní monitorování a diagnostika](service-fabric-diagnostics-how-to-monitor-and-diagnose-services-locally-linux.md)

## [Nasazení API Managementu a Service Fabric do Azure](service-fabric-api-management-quick-start.md)

## Migrace ze služby Cloud Services
### [Porovnání služby Cloud Services se Service Fabric](service-fabric-cloud-services-migration-differences.md)
### [Migrace do Service Fabric](service-fabric-cloud-services-migration-worker-role-stateless-service.md)
### [Doporučené postupy](/azure/architecture/service-fabric/migrate-from-cloud-services)

## Správa životního cyklu aplikace
### [Balení aplikace](service-fabric-package-apps.md)

### Nasazení nebo odebrání aplikací
#### [Nasazení aplikací v místním clusteru](service-fabric-get-started-with-a-local-cluster.md)
#### [PowerShell](service-fabric-deploy-remove-applications.md)
#### [Service Fabric CLI](service-fabric-application-lifecycle-sfctl.md)
#### [Visual Studio](service-fabric-publish-app-remote-cluster.md)
#### [Rozhraní API FabricClient](service-fabric-deploy-remove-applications-fabricclient.md)

### Upgrade aplikací
#### [Upgrade s využitím PowerShellu](service-fabric-application-upgrade-tutorial-powershell.md)
#### [Upgrade s využitím sady Visual Studio](service-fabric-application-upgrade-tutorial.md)
#### [Řešení potíží s upgrady aplikací](service-fabric-application-upgrade-troubleshooting.md)

### Testování aplikací a služeb
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
### Clustery v Azure
#### Vytvořit
##### [Vytvoření vašeho prvního clusteru v Azure](service-fabric-get-started-azure-cluster.md)
##### [Azure Portal](service-fabric-cluster-creation-via-portal.md)
##### [Azure Resource Manager](service-fabric-cluster-creation-via-arm.md)
#### Měřítko
##### [Ručně](service-fabric-cluster-scale-up-down.md)
##### [Prostřednictvím kódu programu](service-fabric-cluster-programmatic-scaling.md)
#### [Upgrade](service-fabric-cluster-upgrade.md)
#### [Nastavení řízení přístupu](service-fabric-cluster-security-roles.md)
#### [Konfigurace](service-fabric-cluster-fabric-settings.md)
#### [Otevření portu v nástroji pro vyrovnávání zatížení](create-load-balancer-rule.md)
#### [Správa certifikátů clusteru](service-fabric-cluster-security-update-certs-azure.md)
#### [Odstranění](service-fabric-cluster-delete.md)

### Samostatné clustery
#### [Plánování a příprava nasazení](service-fabric-cluster-standalone-deployment-preparation.md)
#### Vytvořit
##### [Vytvoření vašeho prvního samostatného clusteru](service-fabric-get-started-standalone-cluster.md)
##### [Místní vytvoření](service-fabric-cluster-creation-for-windows-server.md)
##### [Zabezpečení pomocí certifikátů](service-fabric-windows-cluster-x509-security.md)  
##### [Zabezpečení pomocí zabezpečení systému Windows](service-fabric-windows-cluster-windows-security.md)
##### [Obsah samostatného balíčku](service-fabric-cluster-standalone-package-contents.md)
#### [Škálování](service-fabric-cluster-windows-server-add-remove-nodes.md)
#### [Nastavení řízení přístupu](service-fabric-cluster-security-roles.md)
#### [Konfigurace](service-fabric-cluster-manifest.md)
#### [Upgrade](service-fabric-cluster-upgrade-windows-server.md)

### [Vizualizace clusteru](service-fabric-visualizing-your-cluster.md)
### [Připojení k zabezpečenému clusteru](service-fabric-connect-to-secure-cluster.md)

### [Správa clusteru pomocí rozhraní příkazového řádku Service Fabric](service-fabric-cli.md)
### [Oprava uzlů clusteru](service-fabric-patch-orchestration-application.md)

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

## Monitorování a diagnóza
### [Monitorování a diagnostika aplikací](service-fabric-diagnostics-overview.md)
### Generování událostí
#### [Generování událostí na úrovni platformy](service-fabric-diagnostics-event-generation-infra.md)
##### [Provozní kanál](service-fabric-diagnostics-event-generation-operational.md)
##### [Události Reliable Services](service-fabric-reliable-services-diagnostics.md)
##### [Události Reliable Actors](service-fabric-reliable-actors-diagnostics.md)
##### [Metriky výkonu](service-fabric-diagnostics-event-generation-perf.md)
#### [Generování událostí na úrovni aplikace](service-fabric-diagnostics-event-generation-app.md)
### Kontrola stavu aplikace a clusteru
#### [Monitorování stavu Service Fabric](service-fabric-health-introduction.md)
#### [Hlášení a kontrola stavu služeb](service-fabric-diagnostics-how-to-report-and-check-service-health.md)
#### [Přidání vlastních stavových sestav](service-fabric-report-health.md)
#### [Řešení potíží se systémovými stavovými sestavami](service-fabric-understand-and-troubleshoot-with-system-health-reports.md)
#### [Zobrazení stavových sestav](service-fabric-view-entities-aggregated-health.md)
#### [Monitorování kontejnerů Windows Serveru](service-fabric-diagnostics-containers-windowsserver.md)
### Agregace událostí
#### [Agregace událostí s využitím EventFlow](service-fabric-diagnostics-event-aggregation-eventflow.md)
#### Agregace událostí s využitím Azure Diagnostics
##### [Windows](service-fabric-diagnostics-event-aggregation-wad.md)
##### [Linux](service-fabric-diagnostics-event-aggregation-lad.md)
### Analýza událostí
#### [Analýza událostí s využitím Application Insights](service-fabric-diagnostics-event-analysis-appinsights.md)
#### [Analýza událostí s využitím OMS](service-fabric-diagnostics-event-analysis-oms.md)
### [Řešení potíží s místním clusterem](service-fabric-troubleshoot-local-cluster-setup.md)

# Referenční informace
## [PowerShell (Azure)](/powershell/module/azurerm.servicefabric/)
## [PowerShell](/powershell/module/servicefabric/?view=azureservicefabricps)
## [Azure CLI](/cli/azure/sf)
## [Rozhraní API Java](/java/api/overview/azure/servicefabric)
## [.NET](/dotnet/api/overview/azure/service-fabric?view=azure-dotnet)
## [REST](/rest/api/servicefabric)

# Zdroje a prostředky
## [Plány Azure do budoucna](https://azure.microsoft.com/roadmap/)
## [Časté dotazy](service-fabric-common-questions.md)
## [Postup výuky](https://azure.microsoft.com/documentation/learning-paths/service-fabric/)
## [Fórum MSDN](https://social.msdn.microsoft.com/Forums/home?forum=AzureServiceFabric)
## [Ceny](https://azure.microsoft.com/pricing/details/service-fabric/)
## [ Cenová kalkulačka](https://azure.microsoft.com/pricing/calculator/)
## [Ukázka kódu](http://aka.ms/servicefabricsamples)
## [Možnosti podpory](service-fabric-support.md)
## [Aktualizace služby](https://azure.microsoft.com/updates/?product=service-fabric)
## [Videa](https://azure.microsoft.com/documentation/videos/index/?services=service-fabric)
