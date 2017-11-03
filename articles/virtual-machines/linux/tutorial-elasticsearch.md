---
title: "Nasazení ElasticSearch na vývoj pro virtuální počítač v Azure"
description: "Kurz – instalace elastické zásobníku na vývojovém virtuálního počítače s Linuxem v Azure"
services: virtual-machines-linux
documentationcenter: virtual-machines
author: rloutlaw
manager: justhe
tags: azure-resource-manager
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: azurecli
ms.topic: tutorial
ms.date: 10/11/2017
ms.author: routlaw
ms.openlocfilehash: 5b0b51504478cc0d501a89760ccd60808a69ccbd
ms.sourcegitcommit: b979d446ccbe0224109f71b3948d6235eb04a967
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/25/2017
---
# <a name="install-the-elastic-stack-on-an-azure-vm"></a>Azure virtuálního počítače nainstalujte elastické zásobníku

Tento článek vás provede procesem nasazení [Elasticsearch](https://www.elastic.co/products/elasticsearch), [Logstash](https://www.elastic.co/products/logstash), a [Kibana](https://www.elastic.co/products/kibana), na virtuálního počítače s Ubuntu v Azure. Pokud chcete zobrazit elastické zásobníku v akci, můžete volitelně připojit k Kibana a pracovat s ukázkami protokolování dat. 

V tomto kurzu se naučíte:

> [!div class="checklist"]
> * Vytvoření virtuálního počítače s Ubuntu ve skupině prostředků Azure.
> * Do virtuálního počítače nainstalujte Elasticsearch, Logstash a Kibana
> * Poslat Elasticsearch s Logstash ukázková data 
> * Otevřete porty a pracovat s daty v konzole Kibana


 Toto nasazení je vhodná pro základní vývoj s elastické zásobníku. Další informace o elastické zásobníku, včetně doporučení pro produkční prostředí, najdete v článku [elastické dokumentaci](https://www.elastic.co/guide/index.html) a [Azure architektura Center](/azure/architecture/elasticsearch/).

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

Pokud si zvolíte instalaci a použití rozhraní příkazového řádku místně, tento kurz vyžaduje, že používáte Azure CLI verze verze 2.0.4 nebo novější. Verzi zjistíte spuštěním příkazu `az --version`. Pokud potřebujete instalaci nebo upgrade, přečtěte si téma [Instalace Azure CLI 2.0]( /cli/azure/install-azure-cli). 

## <a name="create-a-resource-group"></a>Vytvoření skupiny prostředků

Vytvořte skupinu prostředků pomocí příkazu [az group create](/cli/azure/group#create). Skupina prostředků Azure je logický kontejner, ve kterém se nasazují a spravují prostředky Azure. 

Následující příklad vytvoří skupinu prostředků *myResourceGroup* v umístění *eastus*.

```azurecli-interactive 
az group create --name myResourceGroup --location eastus
```

## <a name="create-a-virtual-machine"></a>Vytvoření virtuálního počítače

Vytvořte virtuální počítač pomocí příkazu [az vm create](/cli/azure/vm#create). 

Následující příklad vytvoří virtuální počítač *myVM*, a pokud ve výchozím umístění klíčů ještě neexistují klíče SSH, vytvoří je. Chcete-li použít konkrétní sadu klíčů, použijte možnost `--ssh-key-value`.  

```azurecli-interactive 
az vm create \
    --resource-group myResourceGroup \
    --name myVM \
    --image UbuntuLTS \
    --admin-username azureuser \
    --generate-ssh-keys
```

Po vytvoření virtuálního počítače se v Azure CLI zobrazí podobné informace jako v následujícím příkladu. Poznamenejte si `publicIpAddress`. Tato adresa se používá pro přístup k virtuálnímu počítači.

```azurecli-interactive 
{
  "fqdns": "",
  "id": "/subscriptions/<subscription ID>/resourceGroups/myResourceGroup/providers/Microsoft.Compute/virtualMachines/myVM",
  "location": "eastus",
  "macAddress": "00-0D-3A-23-9A-49",
  "powerState": "VM running",
  "privateIpAddress": "10.0.0.4",
  "publicIpAddress": "40.68.254.142",
  "resourceGroup": "myResourceGroup"
}
```

## <a name="ssh-into-your-vm"></a>Připojení SSH k virtuálnímu počítači

Pokud si nejste jisti již veřejnou IP adresu vašeho virtuálního počítače, spusťte [seznam veřejné ip sítě az](/cli/azure/network/public-ip#list) příkaz:

```azurecli-interactive
az network public-ip list --resource-group myResourceGroup --query [].ipAddress
```

Pomocí následujícího příkazu vytvořte s virtuálním počítačem relaci SSH. Nahraďte správné veřejnou IP adresu virtuálního počítače. V tomto příkladu IP adresa je *40.68.254.142*.

```bash
ssh azureuser@40.68.254.142
```

## <a name="install-the-elastic-stack"></a>Nainstalujte elastické zásobníku

Import podpisový klíč Elasticsearch a aktualizace vašeho seznamu VÝSTIŽNÝ zdroje zahrnout úložiště elastické balíčků:

```bash
wget -qO - https://artifacts.elastic.co/GPG-KEY-elasticsearch | sudo apt-key add -
echo "deb https://artifacts.elastic.co/packages/5.x/apt stable main" | sudo tee -a /etc/apt/sources.list.d/elastic-5.x.list
```

Nainstalujte na virtuální počítač virtuální Java a nakonfigurujte JAVA_HOME Tato proměnná je nezbytné pro elastické zásobníku součásti, které chcete spustit.

```bash
sudo apt update && sudo apt install openjdk-8-jre-headless
export JAVA_HOME=/usr/lib/jvm/java-8-openjdk-amd64
```

Spusťte následující příkazy k aktualizaci zdroje balíčků Ubuntu a nainstalujte Elasticsearch, Kibana a Logstash.

```bash
sudo apt update && sudo apt install elasticsearch kibana logstash   
```

> [!NOTE]
> Podrobné pokyny k instalaci, včetně rozložení adresáře a počáteční konfigurace, jsou zachována ve [Elastická na dokumentaci](https://www.elastic.co/guide/en/elastic-stack/current/installing-elastic-stack.html)

## <a name="start-elasticsearch"></a>Spustit Elasticsearch 

Spusťte Elasticsearch na vašem virtuálním počítači pomocí následujícího příkazu:

```bash
sudo systemctl start elasticsearch.service
```

Tento příkaz neprodukuje žádný výstup, tak ověřte, zda je na virtuálním počítači s tímto spuštěna Elasticsearch `curl` příkaz:

```bash
curl -XGET 'localhost:9200/'
```

Pokud Elasticsearch běží, zobrazí se výstup takto:

```json
{
  "name" : "w6Z4NwR",
  "cluster_name" : "elasticsearch",
  "cluster_uuid" : "SDzCajBoSK2EkXmHvJVaDQ",
  "version" : {
    "number" : "5.6.3",
    "build_hash" : "1a2f265",
    "build_date" : "2017-10-06T20:33:39.012Z",
    "build_snapshot" : false,
    "lucene_version" : "6.6.1"
  },
  "tagline" : "You Know, for Search"
}
```

## <a name="start-logstash-and-add-data-to-elasticsearch"></a>Spustit Logstash a přidejte data do Elasticsearch

Spusťte Logstash pomocí následujícího příkazu:

```bash 
sudo systemctl start logstash.service
```

Testovací Logstash v interaktivním režimu a ujistěte se, že funguje správně:

```bash
sudo /usr/share/logstash/bin/logstash -e 'input { stdin { } } output { stdout {} }'
```

Toto je základní logstash [kanálu](https://www.elastic.co/guide/en/logstash/5.6/pipeline.html) , vrátí standardní vstup standardním výstupu. 

```output
The stdin plugin is now waiting for input:
hello azure
2017-10-11T20:01:08.904Z myVM hello azure
```

Nastavte Logstash předávání zprávy jádra z tohoto virtuálního počítače do Elasticsearch. Vytvořte nový soubor v prázdný adresář s názvem `vm-syslog-logstash.conf` a vložte následující konfigurace Logstash:

```Logstash
input {
    stdin {
        type => "stdin-type"
    }

    file {
        type => "syslog"
        path => [ "/var/log/*.log", "/var/log/*/*.log", "/var/log/messages", "/var/log/syslog" ]
        start_position => "beginning"
    }
}

output {

    stdout {
        codec => rubydebug
    }
    elasticsearch {
        hosts  => "localhost:9200"
    }
}
```

Testování této konfigurace a jak odesílat syslog data Elasticsearch:

```bash
sudo /usr/share/logstash/bin/logstash -f vm-syslog-logstash.conf
```

Zobrazí položky syslogu v terminálu opakována při odesílání do Elasticsearch. Použití `CTRL+C` ukončíte mimo Logstash po odeslali některá data.

## <a name="start-kibana-and-visualize-the-data-in-elasticsearch"></a>Spusťte Kibana a znázorňovat data ve Elasticsearch

Upravit `/etc/kibana/kibana.yml` a změňte IP adresu Kibana naslouchá na, takže je přístupný z webového prohlížeče.

```bash
server.host:"0.0.0.0"
```

Spusťte Kibana pomocí následujícího příkazu:

```bash
sudo systemctl start kibana.service
```

Otevřete port 5601 z příkazového řádku Azure a povolil vzdálený přístup ke konzole Kibana:

```azurecli-interactive
az vm open-port --port 5601 --resource-group myResourceGroup --name myVM
```

Otevře konzolu Kibana a vyberte **vytvořit** vygenerujte výchozí index na základě dat syslog jste poslali do Elasticsearch dříve. 

![Procházet události procesu Syslog v Kibana](media/elasticsearch-install/kibana-index.png)

Vyberte **Discover** v konzole Kibana pro vyhledávání, procházení a filtrovat prostřednictvím události procesu syslog.

![Procházet události procesu Syslog v Kibana](media/elasticsearch-install/kibana-search-filter.png)

## <a name="next-steps"></a>Další kroky

V tomto kurzu jste nasadili elastické zásobníku do vývoj virtuálního počítače v Azure. Naučili jste se tyto postupy:

> [!div class="checklist"]
> * Vytvoření virtuálního počítače s Ubuntu ve skupině prostředků Azure.
> * Do virtuálního počítače nainstalujte Elasticsearch, Logstash a Kibana
> * Ukázková data poslat Elasticsearch z Logstash 
> * Otevřete porty a pracovat s daty v konzole Kibana