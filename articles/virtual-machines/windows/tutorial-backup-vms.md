---
title: "Zálohování virtuálních počítačů Azure Windows | Microsoft Docs"
description: "Virtuální počítače Windows Chraňte pomocí zálohování pomocí služby Azure Backup."
services: virtual-machines-windows
documentationcenter: virtual-machines
author: cynthn
manager: timlt
editor: tysonn
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 07/27/2017
ms.author: cynthn
ms.custom: mvc
ms.openlocfilehash: 8e58a2290e5034ef393f65cbcddb86e18cf4a6ec
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/11/2017
---
# <a name="back-up-windows-virtual-machines-in-azure"></a>Zálohovat virtuální počítače s Windows v Azure

Svá data můžete chránit prováděním záloh v pravidelných intervalech. Zálohování Azure vytvoří body obnovení, které jsou uložené v geograficky redundantní obnovení trezorů. Při obnovení z bodu obnovení můžete obnovit celý virtuální počítač nebo jenom určité soubory. Tento článek vysvětluje, jak obnovit jeden soubor k virtuálnímu počítači spuštěný Windows Server a službu IIS. Pokud ještě nemáte virtuální počítač používat, můžete vytvořit jeden pomocí [rychlé spuštění Windows](quick-create-portal.md). V tomto kurzu se naučíte:

> [!div class="checklist"]
> * Vytvoření zálohy virtuálního počítače
> * Naplánovat denní zálohování
> * Obnovte soubor ze zálohy




## <a name="backup-overview"></a>Přehled služby Backup

Když služba Azure Backup zahájí úlohu zálohování, aktivuje rozšíření zálohování k pořízení snímku v daném okamžiku. Použití služby Azure Backup _VMSnapshot_ rozšíření. Rozšíření je nainstalován během první zálohování virtuálních počítačů, pokud je virtuální počítač spuštěný. Pokud virtuální počítač není spuštěný, služba zálohování pořídí snímek podkladové úložiště (protože žádné aplikace zápisy dojít při zastavení virtuálního počítače).

Při pořizování snímku virtuálních počítačů Windows, služba zálohování koordinuje s Stínová kopie svazku Service (VSS) získat konzistentního snímku disky virtuálního počítače. Jakmile služba Azure Backup používá snímku, data se přenáší do trezoru. Pokud chcete maximalizovat efektivitu, služba identifikuje a přenáší pouze bloky dat, které se změnily od předchozí zálohy.

Po dokončení přenosu dat se odebere snímku a vytvoří bod obnovení.


## <a name="create-a-backup"></a>Vytvoření zálohy
Vytvořte jednoduché plánované denní zálohování do trezoru služby Recovery Services. 

1. Přihlaste se k webu [Azure Portal](https://portal.azure.com/).
2. V nabídce na levé straně vyberte **Virtuální počítače**. 
3. V seznamu vyberte virtuální počítač, který chcete zálohovat.
4. V okně virtuálního počítače v **nastavení** klikněte na tlačítko **zálohování**. **Povolit zálohování** otevře se okno.
5. V **trezor služeb zotavení**, klikněte na tlačítko **vytvořit nový** a zadejte název pro nový trezor. Nový trezor se vytvoří ve stejné skupině prostředků a umístění jako virtuální počítač.
6. Klikněte na tlačítko **zálohování zásad**. V tomto příkladu ponechejte výchozí hodnoty a klikněte na tlačítko **OK**.
7. Na **povolit zálohování** okně klikněte na tlačítko **povolit zálohování**. Tím se vytvoří denní zálohování podle plánu, výchozí.
10. Vytvořit bod obnovení počáteční na **zálohování** okně klikněte na tlačítko **zálohovat nyní**.
11. Na **zálohovat nyní** okně klikněte na ikonu Kalendář, pomocí ovládacího prvku Kalendář vyberte poslední den tohoto bodu obnovení se zachovává a klikněte na tlačítko **zálohování**.
12. V **zálohování** okno pro virtuální počítač, zobrazí se počet bodů obnovení, které jsou dokončeny.

    ![Body obnovení](./media/tutorial-backup-vms/backup-complete.png)
    
První zálohování trvá asi 20 minut. Po dokončení zálohování, přejděte k další části tohoto kurzu.

## <a name="recover-a-file"></a>Obnovit soubor

Pokud omylem odstraníte nebo provést změny do souboru, můžete obnovit soubor z trezoru zálohování obnovení souborů. Obnovení souborů používá skript, který běží na virtuální počítač, přípojný bod obnovení jako místní disk. Tyto disky zůstanou připojené 12 hodin, aby mohli zkopírovat soubory z bodu obnovení a obnovit je do virtuálního počítače.  

V tomto příkladu ukážeme, jak k obnovení souboru bitové kopie, který se používá v výchozí webové stránky pro službu IIS. 

1. Otevřete prohlížeč a připojte na adresu IP virtuálního počítače, který zobrazit výchozí stránka služby IIS.

    ![Výchozí služba IIS webové stránky](./media/tutorial-backup-vms/iis-working.png)

2. Připojte k virtuálnímu počítači.
3. Na virtuální počítač, otevřete **Průzkumníka souborů** , přejděte na \inetpub\wwwroot, odstraňte soubor **iisstart.png**.
4. V místním počítači aktualizujte prohlížeč a zjistíte, že je pryč bitovou kopii na výchozí stránka služby IIS.

    ![Výchozí služba IIS webové stránky](./media/tutorial-backup-vms/iis-broken.png)

5. V místním počítači, otevřete novou kartu a přejděte [portál Azure](https://portal.azure.com).
6. V nabídce na levé straně vyberte **virtuální počítače** a vyberte formuláře virtuálních počítačů v seznamu.
8. V okně virtuálního počítače v **nastavení** klikněte na tlačítko **zálohování**. **Zálohování** otevře se okno. 
9. V nabídce v horní části okna vyberte **obnovení souboru**. **Obnovení souboru** otevře se okno.
10. V **krok 1: Vyberte bod obnovení**, vyberte bod obnovení z rozevíracího seznamu.
11. V **krok 2: stáhnout skript a procházet a obnovit soubory**, klikněte **spustitelný soubor stáhnout** tlačítko. Uložení souboru do vaší **stáhne** složky.
12. V místním počítači, otevřete **Průzkumníka souborů** a přejděte do vaší **stáhne** složky a zkopírujte na stažený soubor .exe. Název souboru bude obsahovat předponu název vašeho virtuálního počítače. 
13. Na vašem virtuálním počítači (přes připojení RDP) vložte soubor .exe na ploše virtuálního počítače. 
14. Přejděte na ploše virtuálního počítače a dvakrát klikněte na .exe. Se spustí příkazový řádek a pak připojte jako sdílené složky, který je k dispozici bod obnovení. Po dokončení vytváření sdílené složky, zadejte **q** zavřete příkazovém řádku.
15. Na vašem virtuálním počítači otevřete **Průzkumníka souborů** a přejděte na písmeno jednotky, která byla použita pro sdílené složky.
16. Přejděte do \inetpub\wwwroot a zkopírujte **iisstart.png** ze souboru sdílet a vložte jej do \inetpub\wwwroot. Například F:\inetpub\wwwroot\iisstart.png zkopírujte a vložte jej do c:\inetpub\wwwroot k obnovení souboru.
17. V místním počítači otevřete kartu prohlížeče, kde jste připojeni k IP adresu virtuálního počítače zobrazující výchozí stránka služby IIS. Stisknutím kláves CTRL + F5 aktualizujte stránku prohlížeče. Teď byste měli vidět, že byla obnovena bitovou kopii.
18. V místním počítači, přejděte zpět na záložce prohlížeče pro portál Azure a v **krok 3: odpojení disky po obnovení** klikněte na tlačítko **odpojit disky** tlačítko. Pokud zapomenete tento krok, připojení k přípojný bod je automaticky zavřít po 12 hodinách. Po těchto 12 hodin budete muset stáhnout nový skript pro vytvoření nové přípojný bod.


## <a name="next-steps"></a>Další kroky

V tomto kurzu jste se naučili:

> [!div class="checklist"]
> * Vytvoření zálohy virtuálního počítače
> * Naplánovat denní zálohování
> * Obnovte soubor ze zálohy

Přechodu na v dalším kurzu se dozvíte o monitorování virtuálních počítačů.

> [!div class="nextstepaction"]
> [Monitorování virtuálních počítačů](tutorial-monitoring.md)









