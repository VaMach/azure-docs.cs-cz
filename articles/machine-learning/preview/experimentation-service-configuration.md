---
title: "Konfigurace služby experimenty Azure Machine Learning | Microsoft Docs"
description: "Tento článek obsahuje přehled služby Azure Machine Learning experimenty s pokyny o tom, jak ho nakonfigurovat."
services: machine-learning
author: gokhanuluderya-msft
ms.author: gokhanu
manager: haining
ms.reviewer: garyericson, jasonwhowell, mldocs
ms.service: machine-learning
ms.workload: data-services
ms.topic: article
ms.date: 09/28/2017
ms.openlocfilehash: 5635ef890537a2f37d9d6e9066d0258fc0cb346e
ms.sourcegitcommit: 3e3a5e01a5629e017de2289a6abebbb798cec736
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/27/2017
---
# <a name="configuring-azure-machine-learning-experimentation-service"></a>Konfigurace služby experimenty Azure Machine Learning

## <a name="overview"></a>Přehled
Služba experimenty Azure Machine Learning umožňuje datových vědců ke spouštění jejich experimenty pomocí Azure Machine Learning spuštění a spuštění možnosti správy. Poskytuje rozhraní pro agilní experimenty s rychlé opakování. Azure Machine Learning Workbench vám umožní spustit s místní spuštění v počítači a poskytuje snadnou cestu pro škálování nahoru i vně do dalších prostředí, jako je vzdálené vědecké účely virtuálních počítačů dat s grafickým Procesorem nebo clustery HDInsight se systémem Spark.

Služba experimentování je sestavená pro zajištění izolované, reprodukovatelnou a konzistentní postupnými experimentů. Pomáhá spravovat výpočetní cíle, provádění prostředí, a spusťte konfigurace. Pomocí Azure Machine Learning Workbench spuštění a možnosti spuštění správy, můžete snadno přesouvat mezi různých prostředích. 

V projektu Workbench můžete spustit skript Pythonu nebo PySpark, místně nebo ve velkém měřítku v cloudu. 

Skripty můžete spustit na: 

* Prostředí Python (3.5.2) v místním počítači nainstalovat Workbench.
* Prostředí Conda Python uvnitř kontejner Docker na místním počítači
* Prostředí Conda Python uvnitř kontejner Docker ve vzdáleném počítači systému Linux. Například [na základě Ubuntu DSVM v Azure](https://azuremarketplace.microsoft.com/marketplace/apps/microsoft-ads.linux-data-science-vm-ubuntu)
* [HDInsight pro Spark](https://azure.microsoft.com/services/hdinsight/apache-spark/) v Azure

>[!IMPORTANT]
>Služba experimenty Azure Machine Learning teď podporuje Python 3.5.2 a Spark 2.1.11 jako Python a Spark verze modulu runtime v uvedeném pořadí. 


### <a name="key-concepts-in-experimentation-service"></a>Klíčové koncepty služby experimentování
Je důležité si uvědomit následující koncepty při provádění experimentu Azure Machine Learning. V následujících částech probereme, jak používat tyto koncepty podrobně. 

#### <a name="compute-target"></a>Výpočetní cíl
A _výpočetní cíl_ Určuje, kde spustit váš program například vaše pracovní plocha, vzdálenou Docker na virtuální počítač nebo cluster. Výpočetní cíl musí být adresovatelné a je přístupný. Workbench vám dává možnost vytvořit výpočetní cíle a spravovat pomocí aplikace Workbench a rozhraní příkazového řádku. 

_Připojte az ml computetarget_ příkaz v rozhraní příkazového řádku můžete vytvořit výpočetní cíl, který můžete použít v vaší spustí.

Jsou podporované výpočetní cíle:
* Místní prostředí Python (3.5.2) v počítači nainstalován pomocí Workbench.
* Místní Docker ve vašem počítači
* Vzdálené Docker na virtuálních počítačích Linux Ubuntu. Například [na základě Ubuntu DSVM v Azure](https://azuremarketplace.microsoft.com/marketplace/apps/microsoft-ads.linux-data-science-vm-ubuntu)
* [HDInsight pro Spark cluster](https://azure.microsoft.com/services/hdinsight/apache-spark/) v Azure

Služba experimentování aktuálně podporuje Python 3.5.2 a Spark 2.1.11 jako Python a Spark verze modulu runtime, v uvedeném pořadí. 

>[!IMPORTANT]
> Spuštění virtuálních počítačů Windows jsou Docker **není** podporovány jako vzdálené výpočetní cíle.

#### <a name="execution-environment"></a>Prostředí pro spuštění
_Prostředí pro spuštění_ definuje konfiguraci běhu a závislosti nutné ke spuštění programu v Workbench.

Můžete spravovat místní spuštění prostředí pomocí oblíbených nástrojů a správce balíčku, pokud je spuštěn na výchozí runtime Workbench. 

Conda slouží ke správě Docker místní a vzdálené spuštění Docker, jakož i na základě HDInsight spuštěních. Pro tyto výpočty cíle, konfigurace prostředí provádění se spravuje přes **Conda_dependencies.yml** a **Spark_dependencies.yml soubory**. Tyto soubory jsou v **aml_config** složky uvnitř projektu.

**Jsou podporované moduly runtime pro spuštění prostředí:**
* Python 3.5.2
* Spark 2.1.11

### <a name="run-configuration"></a>Spuštění nástroje Konfigurace
Kromě výpočetním prostředí cíle a provádění Azure Machine Learning poskytuje rozhraní k definování a změnit *spustit konfigurace*. Různých spuštěních experimentu může vyžadovat jako součást opakovaných experimentů jinou konfiguraci. Může být komínů jiným parametrem rozsahy pomocí různých zdrojů a ladění spark parametry. Služba experimentování poskytuje rozhraní pro správu spusťte konfigurací.

Spuštění _az ml computetarget připojit_ příkaz vytvoří dva soubory ve vaší **aml_config** složku ve vašem projektu: .compute a .runconfig, následující touto konvencí: _< your_ computetarget_name > .compute_ a _< your_computetarget_name > .runconfig_. Soubor .runconfig se automaticky vytvoří pro usnadnění vaší práce, když vytvoříte výpočetní cíl. Můžete vytvořit a spravovat další spuštění konfigurace pomocí _az ml runconfigurations_ příkaz v rozhraní příkazového řádku. Můžete také vytvořit a upravit je v systému souborů.

Spuštění konfigurace v Workbench můžete také zadat proměnné prostředí. Můžete zadat proměnné prostředí a používat je ve vašem kódu přidáním následující části v souboru .runconfig. 

```
EnvironmentVariables:
"EXAMPLE_ENV_VAR1": "Example Value1"
"EXAMPLE_ENV_VAR2": "Example Value2"
```

Tyto proměnné prostředí je přístupná ve vašem kódu. Například tento fragment kódu phyton vytiskne proměnnou prostředí s názvem "EXAMPLE_ENV_VAR1"
```
print(os.environ.get("EXAMPLE_ENV_VAR1"))
```

_**Následující obrázek znázorňuje podrobný postup pro počáteční běh experimentu.**_
![](media/experimentation-service-configuration/experiment-execution-flow.png)

## <a name="experiment-execution-scenarios"></a>Experiment spuštění scénáře
V této části jsme ponořte do scénáře spouštění a další informace o tom, jak Azure Machine Learning spouští experimenty, konkrétně běží experimentu místně, na vzdálený počítač a na clusteru HDInsight. Tato část se od vytvoření cíl výpočetní do prováděných experimentů návod.

>[!NOTE]
>Pro zbývající část tohoto článku používáme příkazy rozhraní příkazového řádku (rozhraní příkazového řádku) pro zobrazení konceptů a funkcí. Možnosti, které jsou zde popsané můžete taky použít z Workbench.

## <a name="launching-the-cli"></a>Spuštění rozhraní příkazového řádku
Snadný způsob, jak spustit rozhraní příkazového řádku je otevření projektu v Workbench a přejdete na **souboru--> spusťte příkazový řádek**.

![](media/experimentation-service-configuration/opening-cli.png)

Tento příkaz spustí okno terminálu, ve kterém můžete zadat příkazů pro spuštění skriptů v aktuální složce projektu. Toto okno terminálu je nakonfigurovaný s prostředí Python 3.5.2, která je nainstalovaná v rámci Workbench.

>[!NOTE]
> Při spuštění žádné _az ml_ příkaz z příkazového řádku, budete muset ověřovány proti Azure. Rozhraní příkazového řádku používá ověřování nezávislé mezipaměti desktopová aplikace, a proto přihlašují k Workbench neznamená, že jste se ověřili ve vašem prostředí rozhraní příkazového řádku. K ověření, postupujte podle následujících kroků. Ověřovací token do místní mezipaměti dobu, stačí tento postup opakujte, když vyprší platnost tokenu. Když vyprší platnost tokenu nebo pokud vidíte chyby ověřování, spusťte následující příkazy:

```
# to authenticate 
$ az login

# to list subscriptions
$ az account list -o table

# to set current subscription to a particular subscription ID 
$ az account set -s <subscription_id>

# to verify your current Azure subscription
$ az account show
```

>[!NOTE] 
>Při spuštění _az ml_ příkaz ve složce projektu, ujistěte se, že projekt patří k účtu Azure Machine Learning experimentování na _aktuální_ předplatného Azure. V opačném případě se může vyskytnout chyby spuštění.


## <a name="running-scripts-and-experiments"></a>Spouštění skriptů a experimentů
S Workbench, můžete provést vaší Python a skripty PySpark na různé výpočetní cílů pomocí _odeslání az ml experimentu_ příkaz. Tento příkaz vyžaduje definice spuštění konfigurace. 

Workbench vytvoří odpovídající soubor .runconfig, když vytvoříte výpočetní cíl, ale můžete vytvořit další spuštění konfigurace pomocí _vytvořit az ml runconfiguration_ příkaz. Můžete také ručně upravit spuštění konfigurační soubory.

Spuštění konfigurace zobrazí jako součást experiment spustit prostředí v Workbench. 

>[!NOTE]
>Další informace o spuštění konfiguračního souboru v [referenci na konfigurační spuštění experimentu](experimentation-service-configuration-reference.md) části.

## <a name="running-a-script-locally-on-workbench-installed-runtime"></a>Spuštění skriptu místně na Workbench nainstalován modul runtime
Workbench umožňuje spustit skripty přímo na Workbench nainstalovaná Python 3.5.2 runtime. Tento modul runtime výchozí je nainstalován v době nastavení Workbench a zahrnuje knihovny Azure Machine Learning a závislosti. Spuštění výsledky a artefaktů pro místního spuštění jsou stále uloženy v spustit historie služba v cloudu.

Na rozdíl od na základě Docker spuštěních, tato konfigurace je _není_ spravuje Conda. Budete muset ručně zřídit závislosti balíčků pro místní prostředí Workbench Python.

Můžete spustit následující příkaz ke spuštění skriptu místně v prostředí Python Workbench nainstalovaná. 

```
$az ml experiment submit -c local myscript.py
```

Zadáním následujícího příkazu v okně Workbench rozhraní příkazového řádku můžete najít cestu k výchozí prostředí Python.
```
$ conda env list
```

>[!NOTE]
>Spuštění místně PySpark přímo na místní Spark prostředí je aktuálně **není** podporována. Workbench podpora PySpark skriptů spuštěných v místní Docker. Obrázek Docker základní služby Azure Machine Learning se dodává s předinstalovaným 2.1.11 Spark. 

_**Přehled místní spuštění pro skript v jazyce Python:**_
![](media/experimentation-service-configuration/local-native-run.png)

## <a name="running-a-script-on-local-docker"></a>Spuštění skriptu na místní Docker
Projekty můžete také spustit na kontejner Docker na místním počítači prostřednictvím služby experimenty. Workbench poskytuje základní image Docker, která se dodává s knihovnami Azure Machine Learning a také jako Spark 2.1.11 runtime tak, aby místní Spark spuštěních snadno. Docker vyžaduje, aby byl již spuštěn v místním počítači.

Pro spouštění vašeho Python nebo PySpark skriptu na místní Docker, můžete spustit následující příkazy v rozhraní příkazového řádku.

```
$az ml experiment submit -c docker myscript.py
```
nebo
```
az ml experiment submit --run-configuration docker myscript.py
```

Prostředí pro spuštění na místní Docker připravena pomocí Azure Machine Learning základní Docker image. Při spuštění pro první čas a překryvy s zadaný v souboru conda_dependencies.yml balíčky, stáhne Workbench tuto bitovou kopii. Tato operace provede počáteční spuštění něco pomalejší, ale při dalším spuštění jsou podstatně rychleji díky Workbench opětovné použití v mezipaměti vrstvy. 

>[!IMPORTANT]
>Budete muset spustit _az ml experimentu Příprava - c docker_ příkazu nejdřív připravit bitovou kopii Docker pro vaše první práce. Můžete také nastavit **PrepareEnvironment** parametr na hodnotu true v souboru docker.runconfig. Tato akce automaticky připraví prostředí jako součást vaší spuštění provádění.  

>[!NOTE]
>Pokud spuštění skriptu PySpark, Spark, používá se také spark_dependencies.yml kromě conda_dependencies.yml.

Skripty a systémem bitovou kopii Docker nabízí následující výhody:

1. Zajišťuje, že váš skript může spolehlivě provést v jiných prostředích provádění. Systémem kontejner Docker pomáhá zjišťovat a vyhnout se všechny místní odkazy, které můžou mít vliv přenositelnost. 

2. Umožňuje rychle testování kódu na moduly runtime a rozhraní, které jsou komplexní instalace a konfigurace, jako je například Apache Spark, aniž byste museli instalovat sami.


_**Přehled místní spuštění Docker skript v jazyce Python:**_
![](media/experimentation-service-configuration/local-docker-run.png)

## <a name="running-a-script-on-a-remote-docker"></a>Spuštění skriptu na vzdálené Docker
V některých případech nemusí být na místním počítači k dispozici prostředky dost pro trénování modelu požadované. V takovém případě experimentování Service umožňuje snadný způsob, jak spustit skripty Pythonu nebo PySpark na výkonnější virtuální počítače pomocí vzdálené spuštění Docker. 

Vzdálený počítač by měl splňovat následující požadavky:
* Vzdálený počítač musí běžet Linux Ubuntu a musí být přístupné prostřednictvím SSH. 
* Vzdálený počítač musí mít Docker systémem.

>[!IMPORTANT]
> Spuštění virtuálních počítačů Windows je Docker **není** podporovány jako vzdálené výpočetní cíle


Můžete vytvořit oba definici výpočetní cíl a spustit konfigurace pro vzdálené spuštění na základě Docker následující příkaz.

```
az ml computetarget attach --name "remotevm" --address "remotevm_IP_address" --username "sshuser" --password "sshpassword" --type remotedocker
```

Jakmile nakonfigurujete výpočetní cíl, můžete spustit skript následující příkaz.
```
$ az ml experiment submit -c remotevm myscript.py
```
>[!NOTE]
>Mějte na paměti této spuštění prostředí je konfigurován pomocí specifikace v conda_dependencies.yml. spark_dependencies.yml se také používá, pokud je v souboru .runconfig zadán PySpark framework. 

Proces vytváření Docker pro vzdálené virtuální počítače je přesně stejný proces pro místní Docker běží, takže byste měli očekávat na podobném principu provádění.

>[!TIP]
>Pokud dáváte přednost, aby se zabránilo latence zaváděné vytváření bitové kopie Docker pro vaše první práce, můžete připravit cílový výpočetní před provedením vašeho skriptu následující příkaz. Příprava experimentu ml az - c<remotedocker>


_**Přehled virtuálních počítačů vzdálené spuštění skript v jazyce Python:**_
![](media/experimentation-service-configuration/remote-vm-run.png)


## <a name="running-a-script-on-an-hdinsight-cluster"></a>Spuštění skriptu v clusteru HDInsight
HDInsight je Oblíbené platforma pro analýzu velkých objemů dat podpora Apache Spark. Workbench umožňuje experimentování velkých objemů dat pomocí clusterů HDInsight Spark. 

Můžete vytvořit cíl výpočetní a spustit konfigurace clusteru služby HDInsight Spark pomocí následujícího příkazu:

```
$ az ml computetarget attach --name "myhdi" --address "<FQDN or IP address>" --username "sshuser" --password "sshpassword" --type cluster 
```

>[!NOTE]
>Pokud používáte plně kvalifikovaný název domény místo IP adresy a jmenuje váš cluster HDI Spark _foo_, koncový bod SSH je na uzlu ovladače s názvem _foo-ssh.azurehdinsight.net_. Nezapomeňte **-ssh** přípony v názvu serveru, pokud používáte plně kvalifikovaný název domény pro _– adresa_ parametr.


Až budete mít kontext výpočetní, můžete spustit následující příkaz k provedení skriptu PySpark.

```
$ az ml experiment submit -c myhdi myscript.py
```

Workbench připraví a spravuje prostředí pro spuštění na clusteru HDInsight pomocí Conda. Spravuje konfiguraci _conda_dependencies.yml_ a _spark_dependencies.yml_ konfigurační soubory. 

Je nutné použít SSH do clusteru HDInsight provádění experimenty v tomto režimu. 

>[!NOTE]
>Podporované konfigurace je, že clusterů HDInsight Spark Linux spuštěné (Ubuntu s Python nebo PySpark 3.5.2 a Spark 2.1.11).

_**Přehled HDInsight na základě spuštění skriptu PySpark**_
![](media/experimentation-service-configuration/hdinsight-run.png)


## <a name="running-a-script-on-gpu"></a>Spuštění skriptu na GPU
Pokud chcete spustit skripty na grafický procesor, můžete podle pokynů uvedených v tomto článku:[použití GPU v Azure Machine Learning](how-to-use-gpu.md)


## <a name="next-steps"></a>Další kroky
* [Vytvořit a nainstalovat Azure Machine Learning](quickstart-installation.md)
* [Model správy](model-management-overview.md)
