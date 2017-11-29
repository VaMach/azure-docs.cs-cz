---
title: "Naplánované události pro virtuální počítače Windows v Azure | Microsoft Docs"
description: "Naplánované události pomocí služby Azure Metadata pro na virtuálních počítačích s Windows."
services: virtual-machines-windows, virtual-machines-linux, cloud-services
documentationcenter: 
author: zivraf
manager: timlt
editor: 
tags: 
ms.assetid: 28d8e1f2-8e61-4fbe-bfe8-80a68443baba
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 08/14/2017
ms.author: zivr
ms.openlocfilehash: f872972135f43efd1fbfdedcf9697c3e8100ebde
ms.sourcegitcommit: 310748b6d66dc0445e682c8c904ae4c71352fef2
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/28/2017
---
# <a name="azure-metadata-service-scheduled-events-preview-for-windows-vms"></a>Služba Azure Metadata: Naplánované události (Preview) pro virtuální počítače Windows

> [!NOTE] 
> Verze Preview jsou k dispozici pro vás, za předpokladu, že souhlasíte s podmínkami použití. Další informace najdete v [dodatečných podmínkách použití systémů Microsoft Azure Preview](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).
>

Naplánované události je služba Azure Metadata, která dává vaší aplikace času Příprava pro virtuální počítač údržby. Poskytuje informace o události nadcházející údržby (například restartování) tak, aby vaše aplikace můžete připravit pro ně a omezit přerušení. Je k dispozici pro všechny typy virtuálního počítače Azure, včetně PaaS a IaaS v systému Windows a Linux. 

Informace o naplánované události v systému Linux najdete v tématu [naplánované události pro virtuální počítače s Linuxem](../linux/scheduled-events.md).

## <a name="why-scheduled-events"></a>Proč naplánované události?

Mnoho aplikací můžete těžit z času na přípravu údržby virtuálního počítače. Čas slouží k provádění specifických úloh aplikace, které zlepšení dostupnosti, spolehlivosti a použitelnost včetně: 

- Kontrolní bod a obnovení
- Připojení vyprazdňování
- Převzetí služeb při selhání primární repliky 
- Odebrání z fondu vyrovnávání zatížení
- Protokolování událostí
- Řádné vypnutí 

Pomocí naplánované události aplikace může zjistit, kdy bude údržby dojít a aktivuje úlohy omezit její vliv.  

Naplánované události poskytuje události v následujících případech použití:
- Platforma iniciované údržby (např. hostitele operačního systému aktualizace)
- Uživatel spustil údržby (například uživatele restartuje nebo opětovně nasadí virtuální počítač)

## <a name="the-basics"></a>Základy  

Služba Azure Metadata zpřístupní informace o spouštění virtuálních počítačů pomocí koncový bod REST, která je přístupná z virtuálního počítače. Informace k dispozici prostřednictvím směrovat IP, takže není nezveřejní virtuálního počítače.

### <a name="scope"></a>Rozsah
Naplánované události budou doručeny do:
- Všechny virtuální počítače v cloudové službě
- Všechny virtuální počítače v nastavení dostupnosti
- Všechny virtuální počítače ve skupině umístění sady škálování. 

V důsledku toho byste měli zkontrolovat `Resources` pole v události zjistit, jaké virtuální počítače budou mít vliv. 

### <a name="discovering-the-endpoint"></a>Koncový bod zjišťování
V případě, kde se má vytvořit virtuální počítač v rámci virtuální sítě (VNet), je k dispozici ze statické IP adresy směrovat, služba metadat `169.254.169.254`.
Pokud virtuální počítač není vytvořen v rámci virtuální sítě, výchozí případů pro cloudové služby a klasické virtuální počítače, je potřeba další logiku zjistit koncový bod používat. Najdete v této ukázce další postup [zjistit koncový bod hostitele](https://github.com/azure-samples/virtual-machines-python-scheduled-events-discover-endpoint-for-non-vnet-vm).

### <a name="versioning"></a>Správa verzí 
Služba Instance metadat je verzí. Verze jsou povinné a aktuální verze je `2017-03-01`.

> [!NOTE] 
> Předchozí verze preview naplánované událostí podporovaných {nejnovější} jako verze rozhraní api. Tento formát se už nepodporuje a bude v budoucnu zastaralá.

### <a name="using-headers"></a>Používání hlaviček
Při dotazu Metadata služby, je nutné zadat hlavičku `Metadata: true` zajistit požadavek nebyl přesměrován náhodně.

### <a name="enabling-scheduled-events"></a>Povolení naplánované události
Při prvním může požádat o naplánované události Azure implicitně povolí funkci na virtuálním počítači. V důsledku toho byste měli očekávat zpožděné odpovědi v první volání až dvě minuty.

### <a name="user-initiated-maintenance"></a>Údržby iniciované uživatelem
Uživatel spustil údržby virtuálního počítače prostřednictvím portálu Azure, rozhraní API, rozhraní příkazového řádku, nebo prostředí PowerShell, které jsou výsledkem plánovaná událost. To umožňuje otestovat logiku přípravy údržby v aplikaci a umožňuje aplikaci připravit pro údržbu inicializované uživatelem.

Restartování virtuálního počítače plány událost s typem `Reboot`. Opětovné nasazení virtuálního počítače plány událost s typem `Redeploy`.

> [!NOTE] 
> Údržby iniciované uživatelem, což vede k naplánované události se momentálně nedá konfigurovat. Možnosti konfigurace: je plánovaná pro budoucí použití.

## <a name="using-the-api"></a>Pomocí rozhraní API

### <a name="query-for-events"></a>Dotaz pro události
Jednoduše tak, že toto volání se můžete dotazovat pro naplánované události:

```
curl -H Metadata:true http://169.254.169.254/metadata/scheduledevents?api-version=2017-03-01
```

Odpověď obsahuje pole naplánované události. Prázdné pole znamená, že aktuálně neexistují žádné události naplánované.
V případě, kde je naplánované události, odpověď obsahuje řadu událostí: 
```
{
    "DocumentIncarnation": {IncarnationID},
    "Events": [
        {
            "EventId": {eventID},
            "EventType": "Reboot" | "Redeploy" | "Freeze",
            "ResourceType": "VirtualMachine",
            "Resources": [{resourceName}],
            "EventStatus": "Scheduled" | "Started",
            "NotBefore": {timeInUTC},              
        }
    ]
}
```

### <a name="event-properties"></a>Vlastnosti události
|Vlastnost  |  Popis |
| - | - |
| ID události | Globálně jedinečný identifikátor pro tuto událost. <br><br> Příklad: <br><ul><li>602d9444-d2cd-49c7-8624-8643e7171297  |
| Typ události | Dopad, který způsobí, že tato událost. <br><br> Hodnoty: <br><ul><li> `Freeze`: Oprava virtuální počítač se pozastavit několik sekund. Procesor je pozastavená, ale neexistuje žádný vliv na paměti, otevřených souborů nebo připojení k síti. <li>`Reboot`: Restartování je naplánováno virtuálního počítače (dočasnou paměti dojde ke ztrátě). <li>`Redeploy`: Je naplánován virtuální počítač přesunout do jiného uzlu (dočasné disky jsou ztraceny). |
| ResourceType | Typ prostředku, který má dopad na tuto událost. <br><br> Hodnoty: <ul><li>`VirtualMachine`|
| Zdroje| Seznam prostředků, které má dopad na tuto událost. Představuje záruku obsahovat maximálně jeden počítače [aktualizace domény](manage-availability.md), ale nemusí obsahovat všechny počítače ve UD. <br><br> Příklad: <br><ul><li> ["FrontEnd_IN_0", "BackEnd_IN_0"] |
| Stav události. | Stav této události. <br><br> Hodnoty: <ul><li>`Scheduled`: Tato událost je naplánováno spuštění po dobu uvedenou v `NotBefore` vlastnost.<li>`Started`: Tato událost byla spuštěna.</ul> Ne `Completed` nebo se někdy poskytuje podobné stav, události se nelze vrátit už po dokončení události.
| Neplatí před| Doba, po jejímž uplynutí může spustit tuto událost. <br><br> Příklad: <br><ul><li> 2016-09-19T18:29:47Z  |

### <a name="event-scheduling"></a>Plánování událostí
Každá událost je naplánováno minimální množství času v budoucnu podle typu události. Tentokrát se odrazí v události `NotBefore` vlastnost. 

|Typ události  | Minimální oznámení |
| - | - |
| Zablokování| 15 minut |
| Restartování | 15 minut |
| Opětovné nasazení | 10 minut |

### <a name="starting-an-event"></a>Událost spuštění 

Jakmile jste se naučili nadcházející události a dokončit logika pro řádné vypnutí, můžete schválit nevyřízené události tak, že `POST` volání na metadata služby s `EventId`. To znamená do Azure, aby se zkrátil minimální oznámení čas (Pokud je to možné). 

```
curl -H Metadata:true -X POST -d '{"DocumentIncarnation":"5", "StartRequests": [{"EventId": "f020ba2e-3bc0-4c40-a10b-86575a9eabd5"}]}' http://169.254.169.254/metadata/scheduledevents?api-version=2017-03-01
```

> [!NOTE] 
> To v úvahu událost umožňuje událostí, aby bylo možné pokračovat pro všechny `Resources` v případě, že, ne jenom virtuální počítač, který uznává události. Proto můžete zvolit vedoucí ke koordinaci potvrzení, který může být stejně jednoduché jako první počítač v `Resources` pole.


## <a name="powershell-sample"></a>Ukázkové prostředí PowerShell 

Následující příklad dotazu na metadata službu pro naplánované události a schválí všechny nevyřízené události.

```PowerShell
# How to get scheduled events 
function GetScheduledEvents($uri)
{
    $scheduledEvents = Invoke-RestMethod -Headers @{"Metadata"="true"} -URI $uri -Method get
    $json = ConvertTo-Json $scheduledEvents
    Write-Host "Received following events: `n" $json
    return $scheduledEvents
}

# How to approve a scheduled event
function ApproveScheduledEvent($eventId, $docIncarnation, $uri)
{    
    # Create the Scheduled Events Approval Document
    $startRequests = [array]@{"EventId" = $eventId}
    $scheduledEventsApproval = @{"StartRequests" = $startRequests; "DocumentIncarnation" = $docIncarnation} 
    
    # Convert to JSON string
    $approvalString = ConvertTo-Json $scheduledEventsApproval

    Write-Host "Approving with the following: `n" $approvalString

    # Post approval string to scheduled events endpoint
    Invoke-RestMethod -Uri $uri -Headers @{"Metadata"="true"} -Method POST -Body $approvalString
}

function HandleScheduledEvents($scheduledEvents)
{
    # Add logic for handling events here
}

######### Sample Scheduled Events Interaction #########

# Set up the scheduled events URI for a VNET-enabled VM
$localHostIP = "169.254.169.254"
$scheduledEventURI = 'http://{0}/metadata/scheduledevents?api-version=2017-03-01' -f $localHostIP 

# Get events
$scheduledEvents = GetScheduledEvents $scheduledEventURI

# Handle events however is best for your service
HandleScheduledEvents $scheduledEvents

# Approve events when ready (optional)
foreach($event in $scheduledEvents.Events)
{
    Write-Host "Current Event: `n" $event
    $entry = Read-Host "`nApprove event? Y/N"
    if($entry -eq "Y" -or $entry -eq "y")
    {
        ApproveScheduledEvent $event.EventId $scheduledEvents.DocumentIncarnation $scheduledEventURI 
    }
}
``` 


## <a name="c-sample"></a>C\# vzorku 

Následující příklad je jednoduchý klient, který komunikuje se službou metadat.

```csharp
public class ScheduledEventsClient
{
    private readonly string scheduledEventsEndpoint;
    private readonly string defaultIpAddress = "169.254.169.254"; 

    // Set up the scheduled events URI for a VNET-enabled VM
    public ScheduledEventsClient()
    {
        scheduledEventsEndpoint = string.Format("http://{0}/metadata/scheduledevents?api-version=2017-03-01", defaultIpAddress);
    }

    // Get events
    public string GetScheduledEvents()
    {
        Uri cloudControlUri = new Uri(scheduledEventsEndpoint);
        using (var webClient = new WebClient())
        {
            webClient.Headers.Add("Metadata", "true");
            return webClient.DownloadString(cloudControlUri);
        }   
    }

    // Approve events
    public void ApproveScheduledEvents(string jsonPost)
    {
        using (var webClient = new WebClient())
        {
            webClient.Headers.Add("Content-Type", "application/json");
            webClient.UploadString(scheduledEventsEndpoint, jsonPost);
        }
    }
}
```

Naplánované události může být reprezentován pomocí následující datové struktury:

```csharp
public class ScheduledEventsDocument
{
    public string DocumentIncarnation;
    public List<CloudControlEvent> Events { get; set; }
}

public class CloudControlEvent
{
    public string EventId { get; set; }
    public string EventStatus { get; set; }
    public string EventType { get; set; }
    public string ResourceType { get; set; }
    public List<string> Resources { get; set; }
    public DateTime? NotBefore { get; set; }
}

public class ScheduledEventsApproval
{
    public string DocumentIncarnation;
    public List<StartRequest> StartRequests = new List<StartRequest>();
}

public class StartRequest
{
    [JsonProperty("EventId")]
    private string eventId;

    public StartRequest(string eventId)
    {
        this.eventId = eventId;
    }
}
```

Následující příklad dotazu na metadata službu pro naplánované události a schválí všechny nevyřízené události.

```csharp
public class Program
{
    static ScheduledEventsClient client;

    static void Main(string[] args)
    {
        client = new ScheduledEventsClient();

        while (true)
        {
            string json = client.GetDocument();
            ScheduledEventsDocument scheduledEventsDocument = JsonConvert.DeserializeObject<ScheduledEventsDocument>(json);

            HandleEvents(scheduledEventsDocument.Events);

            // Wait for user response
            Console.WriteLine("Press Enter to approve executing events\n");
            Console.ReadLine();

            // Approve events
            ScheduledEventsApproval scheduledEventsApprovalDocument = new ScheduledEventsApproval()
            {
                DocumentIncarnation = scheduledEventsDocument.DocumentIncarnation
            };
        
            foreach (CloudControlEvent event in scheduledEventsDocument.Events)
            {
                scheduledEventsApprovalDocument.StartRequests.Add(new StartRequest(event.EventId));
            }

            if (scheduledEventsApprovalDocument.StartRequests.Count > 0)
            {
                // Serialize using Newtonsoft.Json
                string approveEventsJsonDocument =
                    JsonConvert.SerializeObject(scheduledEventsApprovalDocument);

                Console.WriteLine($"Approving events with json: {approveEventsJsonDocument}\n");
                client.ApproveScheduledEvents(approveEventsJsonDocument);
            }

            Console.WriteLine("Complete. Press enter to repeat\n\n");
            Console.ReadLine();
            Console.Clear();
        }
    }

    private static void HandleEvents(List<CloudControlEvent> events)
    {
        // Add logic for handling events here
    }
}
```

## <a name="python-sample"></a>Ukázka Pythonu 

Následující příklad dotazu na metadata službu pro naplánované události a schválí všechny nevyřízené události.

```python
#!/usr/bin/python

import json
import urllib2
import socket
import sys

metadata_url = "http://169.254.169.254/metadata/scheduledevents?api-version=2017-03-01"
headers = "{Metadata:true}"
this_host = socket.gethostname()

def get_scheduled_events():
   req = urllib2.Request(metadata_url)
   req.add_header('Metadata', 'true')
   resp = urllib2.urlopen(req)
   data = json.loads(resp.read())
   return data

def handle_scheduled_events(data):
    for evt in data['Events']:
        eventid = evt['EventId']
        status = evt['EventStatus']
        resources = evt['Resources']
        eventtype = evt['EventType']
        resourcetype = evt['ResourceType']
        notbefore = evt['NotBefore'].replace(" ","_")
        if this_host in resources:
            print "+ Scheduled Event. This host is scheduled for " + eventype + " not before " + notbefore
            # Add logic for handling events here

def main():
   data = get_scheduled_events()
   handle_scheduled_events(data)
   
if __name__ == '__main__':
  main()
  sys.exit(0)
```

## <a name="next-steps"></a>Další kroky 

- Další informace o rozhraní API dostupná v [Instance Metadata služby](instance-metadata-service.md).
- Další informace o [plánované údržby pro virtuální počítače s Windows v Azure](planned-maintenance.md).

