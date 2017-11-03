---
title: "Řešení potíží s nasazením OpenShift v Azure | Microsoft Docs"
description: "Řešení potíží s nasazením OpenShift v Azure"
services: virtual-machines-linux
documentationcenter: virtual-machines
author: haroldw
manager: najoshi
editor: 
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 
ms.author: haroldw
ms.openlocfilehash: ea3664870480f6ed170972a5f52940dc4a852219
ms.sourcegitcommit: b979d446ccbe0224109f71b3948d6235eb04a967
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/25/2017
---
# <a name="troubleshooting-openshift-deployment-in-azure"></a>Řešení potíží s nasazením OpenShift v Azure

Pokud OpenShift cluster není úspěšně nasazena, nejsou některé úloh odstraňování potíží, které lze provést zúžit zaměření problém. Zobrazení stavu nasazení a porovnán v následujícím seznamu ukončovacích kódů.

- Ukončovací kód 3: Red Hat předplatné uživatelské jméno / heslo nebo ID organizace aktivační klíč je nesprávný
- Ukončovací kód 4: Vaše ID fondu Red Hat je nesprávné, nebo nejsou k dispozici žádné oprávnění
- Ukončovací kód 5: Nedá zřídit svazek fondu dynamické Docker
- Ukončovací kód 6: OpenShift clusteru instalace se nezdařila.
- Ukončovací kód 7: Instalace OpenShift clusteru bylo úspěšné, ale konfigurace poskytovatele cloudových služeb Azure se nezdařila - hlavní konfigurace na potíže hlavní uzel
- Ukončovací kód 8: OpenShift clusteru instalace proběhla úspěšně, ale Azure Cloud zprostředkovatele konfigurace se nezdařila - konfigurace uzlu na potíže hlavní uzel
- Ukončovací kód 9: OpenShift clusteru instalace proběhla úspěšně, ale konfigurace poskytovatele cloudových služeb Azure se nezdařila - konfigurace uzlu na uzel aplikace nebo Infra problém
- Ukončovací kód 10: OpenShift clusteru instalace úspěšně, ale Azure Cloud zprostředkovatele konfigurace se nezdařila - opravách hlavní uzly nebo nebylo možné nastavit hlavní jako unschedulable
- Ukončovací kód 11: Nasazení metriky se nezdařilo.
- Ukončovací kód 12: Nasazení protokolování se nezdařilo.

Pro kódy ukončení 7 – 10 nainstalovat clusteru OpenShift ale Azure Cloud zprostředkovatele konfigurace se nezdařilo. Můžete SSH do Bastionu uzel (platforma kontejneru) nebo hlavní uzel (původ) a z tam SSH na všech uzlech v clusteru a opravy problémů.

Obvyklou příčinou selhání ukončovací kódy 7 - 9 je, že objekt služby nemá příslušná oprávnění pro předplatné nebo skupinu prostředků. Pokud je to skutečně problém, pak přiřadit správná oprávnění a ručně znovu spusťte skript, který se nezdařilo všechny následné skripty.
Nezapomeňte restartovat službu, která se nezdařila (například systemctl restartování atomic-openshift-node.service) před spuštěním skriptů na znovu.

Pro další řešení potíží, SSH do vaší hlavního uzlu na portu 2200 (původ) nebo uzel Bastionu na port 22 (platforma kontejneru). Budete muset být kořenový (sudo su-) a potom přejděte na následující adresář: /var/lib/waagent/custom-script/download

Měli byste vidět složku s názvem '0' a '1'. V každém z těchto složek se zobrazí dva soubory a stdout a stderr. Můžete zobrazit prostřednictvím těchto souborů k určení, kde došlo k chybě.
