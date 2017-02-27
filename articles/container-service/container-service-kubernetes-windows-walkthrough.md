---
title: Cluster Azure Kubernetes pro Windows | Dokumentace Microsoftu
description: "Nasazení clusteru Kubernetes pro kontejnery Windows ve službě Azure Container Service a zahájení práce"
services: container-service
documentationcenter: 
author: dlepow
manager: timlt
editor: 
tags: acs, azure-container-service, kubernetes
keywords: 
ms.assetid: 
ms.service: container-service
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 02/21/2017
ms.author: dlepow
translationtype: Human Translation
ms.sourcegitcommit: 2a381431acb6436ddd8e13c69b05423a33cd4fa6
ms.openlocfilehash: 010a9a4a9ad0f6f7584b1c9a54e665557078d25b


---

# <a name="get-started-with-windows-containers-in-a-kubernetes-cluster"></a>Začínáme s kontejnery Windows v clusteru Kubernetes


Tento článek ukazuje, jak vytvořit cluster Kubernetes ve službě Azure Container Service, který obsahuje uzly Windows pro spouštění kontejnerů Windows. 

> [!NOTE]
> Podpora pro kontejnery Windows s Kubernetes ve službě Azure Container Service je ve verzi Preview. K vytvoření clusteru Kubernetes s uzly Windows použijte Azure Portal nebo šablonu Resource Manageru. Azure CLI 2.0 v současné době tuto funkci nepodporuje.



Následující obrázek ukazuje architekturu clusteru Kubernetes ve službě Azure Container Service s jedním hlavním linuxovým uzlem a dvěma agentskými uzly Windows. 

* Hlavní linuxový uzel obsluhuje pro Kubernetes rozhraní REST API a je přístupný prostřednictvím SSH na portu 22 nebo `kubectl` na portu 443. 
* Agentské uzly Windows jsou seskupené ve skupině dostupnosti Azure a spouští vaše kontejnery. Uzly Windows jsou přístupné prostřednictvím tunelu RDP SSH přes hlavní uzel. Pravidla nástroje pro vyrovnávání zatížení Azure se dynamicky přidávají do clusteru v závislosti na vystavených službách.


![Obrázek clusteru Kubernetes v Azure](media/container-service-kubernetes-windows-walkthrough/kubernetes-windows.png)

Všechny virtuální počítače jsou ve stejné privátní virtuální síti a jsou vzájemně plně přístupné. Na všech virtuálních počítačích běží kubelet, Docker a proxy server.

## <a name="prerequisites"></a>Požadavky


* **Veřejný klíč SSH RSA:** Při nasazení pomocí portálu nebo některé ze šablon Azure pro rychlý start budete muset zadat veřejný klíč SSH RSA pro ověření s virtuálními počítači Azure Container Service. Pokud chcete vytvořit klíče SSH (Secure Shell) RSA, postupujte podle pokynů pro systémy [OS X a Linux](../virtual-machines/virtual-machines-linux-mac-create-ssh-keys.md) nebo [Windows](../virtual-machines/virtual-machines-linux-ssh-from-windows.md). 

* **ID a tajný klíč klienta instančního objektu:** Další informace a pokyny najdete v tématu [O instančním objektu pro cluster Kubernetes](container-service-kubernetes-service-principal.md).




## <a name="create-the-cluster"></a>Vytvoření clusteru

K [vytvoření clusteru Kubernetes](container-service-deployment.md#create-a-cluster-by-using-the-azure-portal) s agentskými uzly Windows můžete použít Azure Portal. 

V podokně **Konfigurace rozhraní** v části **Konfigurace orchestrátoru** vyberte **Kubernetes – Windows**. 

## <a name="connect-to-the-cluster"></a>Připojení ke clusteru

Pomocí nástroje příkazového řádku `kubectl` se můžete připojit z místního počítače k hlavnímu uzlu clusteru Kubernetes. Postup instalace a nastavení `kubectl` najdete v části [Připojení ke clusteru Azure Container Service](container-service-connect.md#connect-to-a-kubernetes-cluster). Pomocí příkazů `kubectl` můžete přistupovat k webovému uživatelskému rozhraní Kubernetes a vytvářet a spravovat úlohy kontejneru Windows.

## <a name="create-your-first-kubernetes-service"></a>Vytvoření první služby Kubernetes

Po vytvoření clusteru a připojení pomocí `kubectl` můžete vyzkoušet spuštění základní webové aplikace Windows a její zpřístupnění na internetu. V tomto příkladu určíte prostředky kontejneru pomocí souboru YAML a následně kontejner vytvoříte pomocí příkazu `kubctl apply`.

1. Pokud chcete zobrazit seznam uzlů, zadejte `kubectl get nodes`.  Pokud chcete zobrazit úplné podrobnosti o uzlech, zadejte:  

  ```
  kubectl get nodes -o yaml
  ```

2. Vytvořte soubor `simpleweb.yaml` a zkopírujte do něj následující. Tento soubor nastaví webovou aplikaci pomocí základní image jádra operačního systému Windows Server 2016 z [Docker Hubu](https://hub.docker.com/r/microsoft/windowsservercore/).  

  ```yaml
  apiVersion: v1
  kind: Service
  metadata:
    name: win-webserver
    labels:
      app: win-webserver
  spec:
    ports:
      # the port that this service should serve on
    - port: 80
      targetPort: 80
    selector:
      app: win-webserver
    type: LoadBalancer
  ---
  apiVersion: extensions/v1beta1
  kind: Deployment
  metadata:
    labels:
      app: win-webserver
    name: win-webserver
  spec:
    replicas: 1
    template:
      metadata:
        labels:
          app: win-webserver
        name: win-webserver
      spec:
        containers:
        - name: windowswebserver
          image: microsoft/windowsservercore
          command:
          - powershell.exe
          - -command
          - "<#code used from https://gist.github.com/wagnerandrade/5424431#> ; $$ip = (Get-NetIPAddress | where {$$_.IPAddress -Like '*.*.*.*'})[0].IPAddress ; $$url = 'http://'+$$ip+':80/' ; $$listener = New-Object System.Net.HttpListener ; $$listener.Prefixes.Add($$url) ; $$listener.Start() ; $$callerCounts = @{} ; Write-Host('Listening at {0}...' -f $$url) ; while ($$listener.IsListening) { ;$$context = $$listener.GetContext() ;$$requestUrl = $$context.Request.Url ;$$clientIP = $$context.Request.RemoteEndPoint.Address ;$$response = $$context.Response ;Write-Host '' ;Write-Host('> {0}' -f $$requestUrl) ;  ;$$count = 1 ;$$k=$$callerCounts.Get_Item($$clientIP) ;if ($$k -ne $$null) { $$count += $$k } ;$$callerCounts.Set_Item($$clientIP, $$count) ;$$header='<html><body><H1>Windows Container Web Server</H1>' ;$$callerCountsString='' ;$$callerCounts.Keys | % { $$callerCountsString+='<p>IP {0} callerCount {1} ' -f $$_,$$callerCounts.Item($$_) } ;$$footer='</body></html>' ;$$content='{0}{1}{2}' -f $$header,$$callerCountsString,$$footer ;Write-Output $$content ;$$buffer = [System.Text.Encoding]::UTF8.GetBytes($$content) ;$$response.ContentLength64 = $$buffer.Length ;$$response.OutputStream.Write($$buffer, 0, $$buffer.Length) ;$$response.Close() ;$$responseStatus = $$response.StatusCode ;Write-Host('< {0}' -f $$responseStatus)  } ; "
        nodeSelector:
          beta.kubernetes.io/os: windows
  ```

3. Chcete-li spustit aplikaci, zadejte:

  ```
  kubectl apply -f simpleweb.yaml
  ```
  
  > [!NOTE] 
  > Konfigurace zahrnuje `type: LoadBalancer`. Toto nastavení způsobí, že služba bude na internetu zpřístupněna prostřednictvím nástroje pro vyrovnávání zatížení Azure. Další informace najdete v tématu [Kontejnery pro vyrovnávání zatížení v clusteru Kubernetes v Azure Container Service](container-service-kubernetes-load-balancing.md).
  
4. Pokud chcete ověřit nasazení služby (které trvá přibližně 30 sekund), zadejte:

  ```
  kubectl get pods
  ```

5. Jakmile je služba spuštěná, můžete zobrazit interní a externí IP adresy zadáním:

  ```
  kubectl get svc
  ``` 

  ![IP adresy služby pro Windows](media/container-service-kubernetes-windows-walkthrough/externalipa.png)

  Přidání externí IP adresy trvá několik minut. Předtím, než nástroj pro vyrovnávání zatížení externí IP adresu nakonfiguruje, bude zobrazena se stavem `<pending>` (čekající).


6. Jakmile je externí IP adresa dostupná, můžete ke službě přistupovat z webového prohlížeče.

  ![Aplikace pro Windows Server v prohlížeči](media/container-service-kubernetes-windows-walkthrough/wincontainerwebserver.png)


## <a name="access-the-windows-nodes"></a>Přístup k uzlům Windows
K uzlům Windows můžete přistupovat z místního počítače s Windows prostřednictvím Připojení ke vzdálené ploše. Doporučujeme použít tunel RDP SSH přes hlavní uzel. 

Tunely SSH je ve Windows možné vytvořit několika způsoby. Tato část popisuje, jak pomocí PuTTY vytvořit tunel.

1. [Stáhněte si PuTTY](http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html) do svého počítače s Windows.

2. Spusťte aplikaci.

3. Zadejte název hostitele, který se skládá z uživatelského jména správce clusteru a veřejného názvu DNS prvního hlavního uzlu v clusteru. **Název hostitele** vypadá podobně jako `adminuser@PublicDNSName`. Jako **Port** zadejte 22.

    ![Konfigurace PuTTY 1](media/container-service-kubernetes-windows-walkthrough/putty1.png)

4. Vyberte **SSH > Ověřování**. Pro ověření přidejte cestu ke svému souboru privátního klíče (formát .ppk). Můžete použít nástroj typu [PuTTYgen](http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html) k vygenerování tohoto souboru z klíč SSH použitého při vytváření clusteru.

    ![Konfigurace PuTTY 2](media/container-service-kubernetes-windows-walkthrough/putty2.png)

5. Vyberte **SSH > Tunely** a nakonfigurujte přesměrované porty. Vzhledem k tomu, že váš počítač s Windows již používá port 3389, doporučujeme pro přístup k uzlu Windows 0 a uzlu Windows 1 použít následující nastavení. (Tento vzor použijte i pro další uzly Windows.)

  **Uzel Windows 0**

  * **Zdrojový port:** 3390
  * **Cíl:** 10.240.245.5:3389

  **Uzel Windows 1**

  * **Zdrojový port:** 3391
  * **Cíl:** 10.240.245.6:3389

  ![Obrázek tunelů Windows RDP](media/container-service-kubernetes-windows-walkthrough/rdptunnels.png)

6. Po dokončení uložte konfiguraci připojení kliknutím na **Relace > Uložit**.

7. K relaci PuTTY se připojíte kliknutím na **Otevřít**. Dokončete připojení k hlavnímu uzlu.

8. Spusťte Připojení ke vzdálené ploše. Pokud se chcete připojit k prvnímu uzlu Windows, jako **Počítač** zadejte `localhost:3390` a klikněte na **Připojit**. (Pro připojení k druhému zadejte `localhost:3390` atd.) Pro dokončení připojení zadejte heslo místního správce Windows, které jste nakonfigurovali během nasazení.








## <a name="next-steps"></a>Další kroky

Zde jsou doporučené odkazy na další informace o Kubernetes:

* [Kubernetes Bootcamp](https://kubernetesbootcamp.github.io/kubernetes-bootcamp/index.html) – ukazuje, jak nasadit, škálovat, aktualizovat a ladit kontejnerizované aplikace.
* [Uživatelská příručka Kubernetes](http://kubernetes.io/docs/user-guide/) – poskytuje informace o spouštění programů v existujícím clusteru Kubernetes.
* [Příklady Kubernetes](https://github.com/kubernetes/kubernetes/tree/master/examples) – poskytuje příklady spouštění skutečných aplikací s využitím Kubernetes.


<!--HONumber=Feb17_HO4-->


