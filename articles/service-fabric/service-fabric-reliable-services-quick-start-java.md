---
title: "Vytvoření vaší první mikroslužbu spolehlivé Azure v jazyce Java | Microsoft Docs"
description: "Úvod do vytváření aplikace Microsoft Azure Service Fabric s bezzstavovými i stavovými službami."
services: service-fabric
documentationcenter: java
author: suhuruli
manager: timlt
editor: 
ms.assetid: 7831886f-7ec4-4aef-95c5-b2469a5b7b5d
ms.service: service-fabric
ms.devlang: java
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/02/2017
ms.author: suhuruli
ms.openlocfilehash: 59b58e9d9bdb044c81261fd19338c3f95bd409b3
ms.sourcegitcommit: 3df3fcec9ac9e56a3f5282f6c65e5a9bc1b5ba22
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/04/2017
---
# <a name="get-started-with-reliable-services"></a>Začínáme s Reliable Services
> [!div class="op_single_selector"]
> * [C# v systému Windows](service-fabric-reliable-services-quick-start.md)
> * [Java v Linuxu](service-fabric-reliable-services-quick-start-java.md)
>
>

Tento článek vysvětluje základy služby Azure Service Fabric spolehlivé a provede vás vytvořením a nasazením jednoduchou spolehlivá služba aplikaci napsanou v jazyce Java. Video tento Microsoft Virtual Academy také ukazuje postup vytvoření bezstavové spolehlivé služby:<center><a target="_blank" href="https://mva.microsoft.com/en-US/training-courses/building-microservices-applications-on-azure-service-fabric-16747?l=DOX8K86yC_206218965">  
<img src="./media/service-fabric-reliable-services-quick-start-java/ReliableServicesJavaVid.png" WIDTH="360" HEIGHT="244">  
</a></center>

## <a name="installation-and-setup"></a>Instalace a nastavení
Než začnete, ujistěte se, že máte vývojového prostředí Service Fabric na váš počítač.
Pokud potřebujete nastavit tak, přejděte na [Začínáme v systému Mac](service-fabric-get-started-mac.md) nebo [Začínáme v systému Linux](service-fabric-get-started-linux.md).

## <a name="basic-concepts"></a>Základní koncepty
Pokud chcete začít se službami Reliable Services, potřebujete jenom pochopit několik základní koncepty:

* **Typ služby**: Toto je implementace služby. Je definována v třídě napíšete, který rozšiřuje `StatelessService` a ostatní kódu nebo v něm, použít název a číslo verze závislosti.
* **S názvem instance služby**: ke spuštění služby, vytvoříte pojmenované instance typu služby mnohem jako vytvoření instance objektu typu třídy. Instance služby, které jsou ve skutečnosti instancí objektu možnosti vaší služby třídu, která můžete zapsat.
* **Hostitel služby**: pojmenované instance vytvoříte muset spustit uvnitř hostitele. Hostitel služby je právě proces, kde můžete spustit instance služby.
* **Registrace služby**: registrace soustřeďuje všechny informace dohromady. Typ služby musí být zaregistrován u modulu runtime Service Fabric v hostitele služby umožňující Service Fabric k vytvoření instance ho spustit.  

## <a name="create-a-stateless-service"></a>Vytvoření bezstavové služby
Začněte vytvořením aplikace Service Fabric. Sada Service Fabric SDK pro Linux zahrnuje Yeoman generátor zajistit generování uživatelského rozhraní pro aplikace Service Fabric pomocí bezstavové služby. Spusťte následující Yeoman spuštěním příkazu:

```bash
$ yo azuresfjava
```

Postupujte podle pokynů vytvořte **spolehlivé bezstavové služby**. V tomto kurzu, název aplikace "HelloWorldApplication" a "HelloWorld" service. Výsledek obsahuje adresářů pro `HelloWorldApplication` a `HelloWorld`.

```bash
HelloWorldApplication/
├── build.gradle
├── HelloWorld
│   ├── build.gradle
│   └── src
│       └── statelessservice
│           ├── HelloWorldServiceHost.java
│           └── HelloWorldService.java
├── HelloWorldApplication
│   ├── ApplicationManifest.xml
│   └── HelloWorldPkg
│       ├── Code
│       │   ├── entryPoint.sh
│       │   └── _readme.txt
│       ├── Config
│       │   └── _readme.txt
│       ├── Data
│       │   └── _readme.txt
│       └── ServiceManifest.xml
├── install.sh
├── settings.gradle
└── uninstall.sh
```

## <a name="implement-the-service"></a>Tuto službu implementovat
Otevřete **HelloWorldApplication/HelloWorld/src/statelessservice/HelloWorldService.java**. Tato třída definuje typ služby a můžete spustit kód. Rozhraní API služby obsahuje dvě vstupní body pro váš kód:

* Metodu zprostředkovává vstupního bodu, názvem `runAsync()`, kde můžete začít provádění jakékoli úlohy, včetně dlouho běžící výpočetních úloh.

```java
@Override
protected CompletableFuture<?> runAsync(CancellationToken cancellationToken) {
    ...
}
```

* Ke komunikaci vstupnímu bodu kde zařaďte vaší komunikačního balíku výběru. Toto je, kde můžete spustit přijímá žádosti od uživatelů a dalších služeb.

```java
@Override
protected List<ServiceInstanceListener> createServiceInstanceListeners() {
    ...
}
```

V tomto kurzu budeme soustředit se na `runAsync()` metody vstupní bod. Toto je, kde můžete okamžitě začít kód spuštěný.

### <a name="runasync"></a>RunAsync
Platforma volá tuto metodu, když je umístěný a připravené ke spuštění instance služby. Bezstavové služby, která jednoduše znamená, když je otevřen v instanci služby. Token zrušení je k dispozici pro koordinaci při instanci služby musí být uzavřen. V Service Fabric tento cyklus otevření nebo uzavření instance služby může docházet k tolikrát, kolikrát za dobu existence služby jako celek. Toto může nastat z různých důvodů, včetně:

* Systém přesune vaší instance služby pro vyrovnávání prostředků.
* K chybám dochází v kódu.
* Aplikace nebo systému byla upgradována.
* Základní hardware dojde k výpadku.

Tato orchestration je spravován pomocí Service Fabric a udržovat služby je vysoce dostupný a správně vyrovnáváním.

`runAsync()`by neměly blokovat synchronně. Implementaci runAsync by měl vrátit CompletableFuture umožňující pokračovat modulu runtime. Pokud vaše úlohy musí implementovat dlouhotrvající úlohu, která se má provést uvnitř CompletableFuture.

#### <a name="cancellation"></a>Zrušení
Zrušení úlohy je spolupráci úsilí řízená token poskytnutý zrušení. Systém čeká na ukončení (podle úspěšné dokončení, zrušení nebo selhání), než ji přesune vaše úlohy. Je důležité respektovat token zrušení, Dokončit veškerou práci a ukončete `runAsync()` provést co nejrychleji, pokud systém požadavky zrušení. Následující příklad ukazuje, jak se zpracovat událost zrušení:

```java
    @Override
    protected CompletableFuture<?> runAsync(CancellationToken cancellationToken) {

        // TODO: Replace the following sample code with your own logic
        // or remove this runAsync override if it's not needed in your service.

        CompletableFuture.runAsync(() -> {
          long iterations = 0;
          while(true)
          {
            cancellationToken.throwIfCancellationRequested();
            logger.log(Level.INFO, "Working-{0}", ++iterations);

            try
            {
              Thread.sleep(1000);
            }
            catch (IOException ex) {}
          }
        });
    }
```

### <a name="service-registration"></a>Registrace služby
Typů služeb musí být zaregistrované v modulu runtime Service Fabric. Typ služby je definována v `ServiceManifest.xml` a třídě služby, který implementuje `StatelessService`. Registrace služby se provádí v hlavní vstupního bodu procesu. V tomto příkladu je hlavní vstupního bodu procesu `HelloWorldServiceHost.java`:

```java
public static void main(String[] args) throws Exception {
    try {
        ServiceRuntime.registerStatelessServiceAsync("HelloWorldType", (context) -> new HelloWorldService(), Duration.ofSeconds(10));
        logger.log(Level.INFO, "Registered stateless service type HelloWorldType.");
        Thread.sleep(Long.MAX_VALUE);
    }
    catch (Exception ex) {
        logger.log(Level.SEVERE, "Exception in registration:", ex);
        throw ex;
    }
}
```

## <a name="run-the-application"></a>Spuštění aplikace

Yeoman generování uživatelského rozhraní obsahuje skript gradle sestavení aplikace a skripty pro nasazení a odeberte aplikaci bash. Ke spuštění aplikace, nejprve sestavte aplikaci s gradlem:

```bash
$ gradle
```

Tímto se vytvoří balíček aplikace Service Fabric, které se dá nasadit pomocí Service Fabric rozhraní příkazového řádku.

### <a name="deploy-with-service-fabric-cli"></a>Nasazení pomocí Service Fabric rozhraní příkazového řádku

Install.sh skript obsahuje potřebné příkazy Service Fabric rozhraní příkazového řádku pro nasazení balíčku aplikace. Spusťte skript install.sh k nasazení aplikace.

```bash
$ ./install.sh
```

## <a name="next-steps"></a>Další kroky

* [Začínáme se Service Fabric CLI](service-fabric-cli.md)
