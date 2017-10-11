---
title: "Přidat do plánů obnovení portálu classic runbooky služby Azure automation | Microsoft Docs"
description: "Tento článek popisuje, jak Azure Site Recovery teď můžete rozšířit plány obnovení pro dokončení složité úlohy během obnovení do Azure pomocí Azure Automation."
services: site-recovery
documentationcenter: 
author: ruturaj
manager: gauravd
editor: 
ms.assetid: f24eaa62-9dea-4fce-92e1-a72513ca0289
ms.service: site-recovery
ms.devlang: powershell
ms.tgt_pltfrm: na
ms.topic: article
ms.workload: storage-backup-recovery
ms.date: 08/11/2017
ms.author: ruturajd
ms.openlocfilehash: 0a248e7c3f39a35ac10dc6ac64e5cef7d152e033
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/18/2017
---
# <a name="add-azure-automation-runbooks-to-recovery-plans-in-the-classic-portal"></a>Přidat do plánů obnovení portálu classic runbooky služby Azure automation
Tento kurz popisuje, jak Azure Site Recovery integruje se službou Azure Automation zajistí možnosti rozšíření do plánů obnovení. Plány obnovení můžete orchestraci obnovení virtuální počítače chráněné pomocí Azure Site Recovery pro replikace do sekundární cloudu a replikaci do Azure scénáře. Také pomoci při obnovení **přesné**, **repeatable**, a **automatizované**. Pokud jsou selhání virtuálních počítačů do Azure, integraci s Azure Automation rozšiřuje plány obnovy a nabízí možnost spuštění sady runbook, což umožňuje efektivní automatizace úloh.

Pokud jste ještě se dozvěděli o službě Azure Automation ještě, zaregistrujte si [sem](https://azure.microsoft.com/services/automation/) a stáhnout jejich ukázkové skripty [zde](https://azure.microsoft.com/documentation/scripts/). Další informace o [Azure Site Recovery](https://azure.microsoft.com/services/site-recovery/) a orchestraci obnovení do Azure pomocí plánů obnovení [zde](https://azure.microsoft.com/blog/?p=166264).

V tomto kurzu krátké se podíváme na tom, jak můžete integrovat Azure Automation runbook do plánů obnovení. Nemůžeme se automatizovat jednoduché úkolů, které dříve vyžadují ruční zásah a zjistit, jak převést více krok obnovení do akce obnovení jedním kliknutím. Podíváme se také na řešení k jednoduchého skriptu, pokud se nepovede.

## <a name="protect-the-application-to-azure"></a>Chránit aplikaci do Azure
Dejte nám začněte jednoduché aplikace, která obsahuje dva virtuální počítače. Zde máme aplikace HRweb ze společnosti Fabrikam. Společnost Fabrikam. HRweb front-end a back-Fabrikam-Hrweb end jsou dva virtuální počítače chráněné na Azure pomocí Azure Site Recovery. K ochraně virtuálních počítačů pomocí Azure Site Recovery, postupujte podle následujících kroků.

1. Povolení ochrany pro virtuální počítače.
2. Zajistěte, aby virtuální počítače dokončit počáteční replikaci a jsou replikace.
3. Počkejte na dokončení počáteční replikace a stavu replikace uvádí chráněné.

## ![](media/site-recovery-runbook-automation/01.png)
V tomto kurzu vytvoříme plán obnovení pro aplikace Fabrikam HRweb převzetí služeb při selhání aplikaci do Azure. Pak budeme ji integrovat s sadu runbook, která vytvoří koncový bod na neúspěšný přes virtuální počítač Azure k obsluze webových stránek na portu 80.

Nejdříve vytvoříme plán obnovení pro naši aplikaci.

## <a name="create-the-recovery-plan"></a>Vytvoření plánu obnovení
Chcete-li obnovit aplikaci do Azure, vytvořte plán obnovení.
Pomocí plán obnovení, že které lze nastavit pořadí obnovení virtuálních počítačů. Virtuální počítač ve skupině 1 umístit bude obnovit a spusťte první a postupujte virtuálního počítače ve skupině 2.

Vytvořte do plánu obnovení, který může vypadat níže.

![](media/site-recovery-runbook-automation/12.png)

Další informace o plánech obnovení, přečtěte si dokumentaci [sem](https://msdn.microsoft.com/library/azure/dn788799.aspx "zde").

V dalším kroku vytvoříme nezbytné artefakty ve službě Azure Automation.

## <a name="create-the-automation-account-and-its-assets"></a>Vytvoření účtu automation a její prostředky
Potřebujete účet Azure Automation pro vytváření runbooků. Pokud již účet nemáte, přejděte na kartu Azure Automation označený jako ![](media/site-recovery-runbook-automation/02.png)a vytvořit nový účet.

1. Pojmenujte účet můžete identifikovat se.
2. Zadejte geografické oblasti, ve které chcete umístit účet.

Doporučujeme umístit účet ve stejné oblasti jako trezor automatické obnovení systému.

![](media/site-recovery-runbook-automation/03.png)

Dále vytvořte následující prostředky v účtu.

### <a name="add-a-subscription-name-as-asset"></a>Přidejte název odběru jako prostředek
1. Přidejte nové nastavení ![](media/site-recovery-runbook-automation/04.png) v Azure Automation prostředky a vyberte k![](media/site-recovery-runbook-automation/05.png)
2. Vyberte typ proměnné jako **řetězec**
3. Zadejte název proměnné jako **AzureSubscriptionName**

   ![](media/site-recovery-runbook-automation/06.png)
4. Jako hodnotu proměnné, zadejte skutečný název předplatné Azure.

   ![](media/site-recovery-runbook-automation/07_1.png)

Můžete určit název vaše předplatné na stránce nastavení svého účtu na portálu Azure.

### <a name="add-an-azure-login-credential-as-asset"></a>Přidat jako asset přihlašovacích údajů přihlášení k Azure
Automatizace Azure používá pro připojení k předplatnému Azure PowerShell a funguje na artefakty existuje. V takovém případě budete muset ověřit pomocí účtu Microsoft nebo pracovní nebo školní účet.
Přihlašovací údaje účtu můžete uložit ve prostředek má být bezpečně používána sady runbook.

1. Přidejte nové nastavení ![](media/site-recovery-runbook-automation/04.png) v Azure Automation prostředky a vyberte![](media/site-recovery-runbook-automation/09.png)
2. Vyberte typ přihlašovacích údajů jako **přihlašovací údaje Windows PowerShell**
3. Zadejte název **AzureCredential**

   ![](media/site-recovery-runbook-automation/10.png)
4. Zadejte uživatelské jméno a heslo k přihlášení s.

Obě tato nastavení jsou nyní k dispozici v vaše prostředky.

![](media/site-recovery-runbook-automation/11.png)

Další informace o tom, jak se připojit k předplatnému pomocí prostředí PowerShell jsou uvedeny [zde](/powershell/azure/overview).

V dalším kroku vytvoříte sady runbook ve službě Azure Automation, které můžete přidat koncový bod pro front-endu virtuální počítač po převzetí služeb při selhání.

## <a name="azure-automation-context"></a>Kontext služby Azure automation
Automatické obnovení systému předá kontextové proměnné do runbooku usnadňuje psaní skriptů deterministický. Jeden může uvádějí, že jsou předvídatelný názvy cloudové služby a virtuální počítač, ale se stane, že není vždy případ vyplývající z určité scénáře, jako je ta, kde název název virtuálního počítače může změnily kvůli nepodporované znaky v Azure. Proto se předá tyto informace do plánu obnovení automatické obnovení systému jako součást *kontextu*.

Níže je příklad, jak vypadá kontextové proměnné.

        {"RecoveryPlanName":"hrweb-recovery",

        "FailoverType":"Test",

        "FailoverDirection":"PrimaryToSecondary",

        "GroupId":"1",

        "VmMap":{"7a1069c6-c1d6-49c5-8c5d-33bfce8dd183":

                {"CloudServiceName":"pod02hrweb-Chicago-test",

                "RoleName":"Fabrikam-Hrweb-frontend-test"}

                }

        }


Následující tabulka obsahuje název a popis pro každou proměnnou v kontextu.

| **Název proměnné** | **Popis** |
| --- | --- |
| RecoveryPlanName |Název plánu spuštěn. Umožňuje provést akci na základě názvu pomocí stejného skriptu |
| FailoverType |Určuje, zda je převzetí služeb při selhání otestovat, plánovaná nebo neplánovaná. |
| FailoverDirection |Určete, zda je primární nebo sekundární obnovení |
| GroupID |Identifikovat číslo skupiny v rámci plánu obnovení, když běží plánu |
| VmMap |Pole všech virtuálních počítačů ve skupině |
| Klíč VMMap |Jedinečný klíč (GUID) pro každý virtuální počítač. Případně je stejný jako Identifikátor VMM virtuálního počítače. |
| RoleName |Název virtuálního počítače Azure, který obnovuje |
| CloudServiceName |Azure název cloudové služby, pod kterou je vytvořen virtuální počítač. |

K identifikaci VmMap klíče v kontextu, můžete také přejít na stránku vlastností virtuálního počítače v nástroji automatické obnovení systému a podívejte se na vlastnost identifikátor GUID virtuálního počítače.

![](media/site-recovery-runbook-automation/13.png)

## <a name="author-an-automation-runbook"></a>Autor runbook služby Automation.
Teď vytvoříte runbook otevřít port 80 na front-endu virtuálním počítači.

1. Vytvořit novou sadu runbook v účtu Azure Automation s názvem **OpenPort80**

   ![](media/site-recovery-runbook-automation/14.png)
2. Přejděte do zobrazení Autor sady runbook a zadejte režimu konceptu.
3. Nejdřív zadejte proměnnou použít jako kontext plánu obnovení

   ```
       param (
           [Object]$RecoveryPlanContext
       )

   ```
4. Další připojení k předplatnému pomocí přihlašovacích údajů a předplatné názvu

   ```
       $Cred = Get-AutomationPSCredential -Name 'AzureCredential'

       # Connect to Azure
       $AzureAccount = Add-AzureAccount -Credential $Cred
       $AzureSubscriptionName = Get-AutomationVariable –Name ‘AzureSubscriptionName’
       Select-AzureSubscription -SubscriptionName $AzureSubscriptionName
   ```

   Všimněte si, že používáte Azure prostředky – **AzureCredential** a **AzureSubscriptionName** sem.
5. Teď určete koncový bod podrobnosti a identifikátor GUID virtuálního počítače, pro kterou chcete vystavit koncový bod. V tomto případě front-endu virtuálního počítače.

   ```
       # Specify the parameters to be used by the script
       $AEProtocol = "TCP"
       $AELocalPort = 80
       $AEPublicPort = 80
       $AEName = "Port 80 for HTTP"
       $VMGUID = "7a1069c6-c1d6-49c5-8c5d-33bfce8dd183"
   ```

   Určuje protokol koncového bodu Azure, místní port ve virtuálním počítači a jeho namapované veřejný port. Tyto proměnné jsou parametry, které vyžadují Azure příkazy, které přidat koncové body k virtuálním počítačům. VMGUID obsahuje identifikátor GUID virtuálního počítače, které potřebujete pracovat.
6. Skript se teď extrahování kontext pro daný identifikátor GUID virtuálního počítače a vytvořit koncový bod na virtuálním počítači, který se odkazuje.

   ```
       #Read the VM GUID from the context
       $VM = $RecoveryPlanContext.VmMap.$VMGUID

       if ($VM -ne $null)
       {
           # Invoke pipeline commands within an InlineScript

           $EndpointStatus = InlineScript {
               # Invoke the necessary pipeline commands to add a Azure Endpoint to a specified Virtual Machine
               # Commands include: Get-AzureVM | Add-AzureEndpoint | Update-AzureVM (including parameters)

               $Status = Get-AzureVM -ServiceName $Using:VM.CloudServiceName -Name $Using:VM.RoleName | `
                   Add-AzureEndpoint -Name $Using:AEName -Protocol $Using:AEProtocol -PublicPort $Using:AEPublicPort -LocalPort $Using:AELocalPort | `
                   Update-AzureVM
               Write-Output $Status
           }
       }
   ```
7. Po dokončení, stiskněte tlačítko Publikovat ![](media/site-recovery-runbook-automation/20.png) umožňující váš skript k dispozici pro spuštění.

Dále je pro vaši informaci uveden dokončení skriptu

```
  workflow OpenPort80
  {
    param (
        [Object]$RecoveryPlanContext
    )

    $Cred = Get-AutomationPSCredential -Name 'AzureCredential'

    # Connect to Azure
    $AzureAccount = Add-AzureAccount -Credential $Cred
    $AzureSubscriptionName = Get-AutomationVariable –Name ‘AzureSubscriptionName’
    Select-AzureSubscription -SubscriptionName $AzureSubscriptionName

    # Specify the parameters to be used by the script
    $AEProtocol = "TCP"
    $AELocalPort = 80
    $AEPublicPort = 80
    $AEName = "Port 80 for HTTP"
    $VMGUID = "7a1069c6-c1d6-49c5-8c5d-33bfce8dd183"

    #Read the VM GUID from the context
    $VM = $RecoveryPlanContext.VmMap.$VMGUID

    if ($VM -ne $null)
    {
        # Invoke pipeline commands within an InlineScript

        $EndpointStatus = InlineScript {
            # Invoke the necessary pipeline commands to add an Azure Endpoint to a specified Virtual Machine
            # This set of commands includes: Get-AzureVM | Add-AzureEndpoint | Update-AzureVM (including necessary parameters)

            $Status = Get-AzureVM -ServiceName $Using:VM.CloudServiceName -Name $Using:VM.RoleName | `
                Add-AzureEndpoint -Name $Using:AEName -Protocol $Using:AEProtocol -PublicPort $Using:AEPublicPort -LocalPort $Using:AELocalPort | `
                Update-AzureVM
            Write-Output $Status
        }
    }
  }
```

## <a name="add-the-script-to-the-recovery-plan"></a>Přidejte skript do plánu obnovení
Až tento skript je připraven, můžete ho přidat do plánu obnovení, který jste vytvořili dříve.

1. V plánu obnovení, kterou jste vytvořili vyberte skript přidáte tak po skupiny 2. ![](media/site-recovery-runbook-automation/15.png)
2. Zadejte název skriptu. Toto je právě popisný název pro tento skript pro zobrazení v rámci plánu obnovení.
3. V převzetí služeb při selhání do Azure skriptu – vyberte název účet Azure Automation.
4. V sadách Runbook Azure vyberte sadu runbook, vámi vytvořený.

![](media/site-recovery-runbook-automation/16.png)

## <a name="primary-side-scripts"></a>Primární straně skripty
Když jsou prováděny převzetí služeb při selhání do Azure, můžete také spustit skripty primární straně. Tyto skripty se spustí na serveru VMM během převzetí služeb při selhání.
Primární straně skriptů jsou k dispozici pouze pro před vypnutím pouze a post vypnutí fázích. Toto je očekávané být obvykle není k dispozici při havárii narazilo primární lokality.
Během neplánované převzetí služeb při selhání pouze v případě, že můžete vyjádřit výslovný souhlas pro operace primární lokality, pokusí se spustit skripty primární straně. Pokud nejsou dosažitelné nebo vypršel časový limit, bude pokračovat převzetí služeb při selhání obnovení virtuálních počítačů.
Primární straně skripty jsou zrušení dostupnou pro servery VMware nebo fyzických/Hyper-v bez VMM chráněné na Azure - při můžete převzetí služeb při selhání do Azure.
Ale když můžete navrácení služeb po obnovení z Azure na primární straně skripty (sady Runbook) na místě slouží pro všechny cíle kromě VMware.

## <a name="test-the-recovery-plan"></a>Testovací plán obnovení
Po přidání sady runbook k plánu, můžete zahájit testovací převzetí služeb a pozorování v akci. Doporučujeme vždy spustit převzetí služeb při selhání pro testování vaší aplikace a plán obnovení tak, ujistěte se, že nejsou žádné chyby.

1. Vyberte plán obnovení a spustit testovací převzetí služeb.
2. Během provádění plánu můžete zjistit, zda sada runbook má provést, nebo nikoli prostřednictvím jeho stav.

   ![](media/site-recovery-runbook-automation/17.png)
3. Stav provádění podrobné runbook můžete zobrazit také na stránce úlohy automatizace Azure pro sadu runbook.

   ![](media/site-recovery-runbook-automation/18.png)
4. Po dokončení převzetí kromě výsledku spuštění runbooku, uvidíte, zda je úspěšné provedení nebo není na stránce virtuální počítač Azure a prohlížení koncových bodů.

![](media/site-recovery-runbook-automation/19.png)

## <a name="sample-scripts"></a>Ukázkové skripty
Když jsme projít automatizaci jeden běžně používat úkolu přidávání koncového bodu pro virtuální počítač Azure v tomto kurzu, můžete to udělat několika další výkonné automatizované úlohy pomocí služby Azure automation. Microsoft a její komunitě Azure Automation poskytují ukázkové sady runbook, které vám pomůžou začít vytvářet vlastní řešení a sady runbook nástroj, který můžete použít jako stavební bloky pro větší úlohy automatizace. Začít používat je z galerie a vytvářet plány obnovení výkonné jedním kliknutím pro vaše aplikace pomocí Azure Site Recovery.

## <a name="additional-resources"></a>Další zdroje
[Přehled služby Azure Automation](http://msdn.microsoft.com/library/azure/dn643629.aspx "Přehled služby Azure Automation")

[Ukázkové skripty pro automatizaci Azure](http://gallery.technet.microsoft.com/scriptcenter/site/search?f\[0\].Type=User&f\[0\].Value=SC%20Automation%20Product%20Team&f\[0\].Text=SC%20Automation%20Product%20Team "ukázkové skripty služby Azure Automation")
