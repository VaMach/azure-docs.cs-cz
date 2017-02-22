---
title: "Konfigurace nastavení replikace pro Azure Site Recovery | Dokumentace Microsoftu"
description: "Popisuje způsob nasazení Site Recovery za účelem orchestrace replikace, převzetí služeb při selhání a obnovení virtuálních počítačů Hyper-V v cloudech VMM do Azure."
services: site-recovery
documentationcenter: 
author: sujayt
manager: rochakm
editor: rayne-wiselman
ms.assetid: 8e7d868e-00f3-4e8b-9a9e-f23365abf6ac
ms.service: site-recovery
ms.workload: backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: hero-article
ms.date: 01/19/2017
ms.author: sutalasi
translationtype: Human Translation
ms.sourcegitcommit: 74d51269cdd55e39eaa2963fec2b409decb47d0a
ms.openlocfilehash: a279fdc35c62c76e6f0858d52c64240032669f15


---
# <a name="manage-replication-policy-for-vmware-to-azure"></a>Správa zásad replikace pro VMware do Azure


## <a name="create-a-new-replication-policy"></a>Vytvoření nové zásady replikace

1. V nabídce na levé straně klikněte na Spravovat-> Infrastruktura Site Recovery. 
2. V části Pro VMware a fyzické počítače vyberte Zásady replikace.
3. Klikněte nahoře na +Zásada replikace.

    ![Vytvoření zásady replikace](./media/site-recovery-setup-replication-settings-vmware/createpolicy.png)

4. Zadejte název zásady.

5. V části Prahová hodnota cíle bodu obnovení (RPO) zadejte omezení RPO. Když toto omezení průběžná replikace překročí, vygenerují se upozornění.
6. V části Uchování bodu obnovení zadejte (v hodinách), jak dlouhý bude interval uchovávání dat pro jednotlivé body obnovení. Chráněné počítače je možné obnovit do libovolného bodu v rámci tohoto intervalu. 

    > [!NOTE] 
    > Pro počítače, které se replikují do Storage úrovně Premium, se podporuje uchování až 24 hodin a pro počítače, které se replikují do Storage úrovně Standard, se podporuje uchování až 72 hodin.
    
    > [!NOTE] 
    > Zásada replikace pro navrácení služeb po obnovení se vytvoří automaticky.

7. V nastavení Frekvence snímků konzistentní vzhledem k aplikacím určete, jak často (v minutách) se mají vytvářet body obnovení obsahující snímky konzistentní vzhledem k aplikacím.

8. Klikněte na OK. Zásada by se měla vytvořit během přibližně 30 sekund až 1 minuty.

![Vytvoření zásady replikace](./media/site-recovery-setup-replication-settings-vmware/Creating-Policy.png)

## <a name="associate-configuration-server-with-replication-policy"></a>Přidružení konfiguračního serveru k zásadě replikace
1. Klikněte na zásadu replikace, ke které chcete přidružit konfigurační server.
2. Klikněte v horní části na Přidružit.
![Vytvoření zásady replikace](./media/site-recovery-setup-replication-settings-vmware/Associate-CS-1.PNG)

3. Ze seznamu serverů vyberte konfigurační server.
4. Klikněte na tlačítko OK. Konfigurační server by se měl přidružit během přibližně 1 až 2 minut.

![Vytvoření zásady replikace](./media/site-recovery-setup-replication-settings-vmware/Associate-CS-2.png)

## <a name="edit-replication-policy"></a>Úprava zásady replikace
1. Klikněte na zásadu replikace, pro kterou chcete upravit nastavení replikace.
![Vytvoření zásady replikace](./media/site-recovery-setup-replication-settings-vmware/Select-Policy.png)

2. Klikněte nahoře na Upravit nastavení.
![Vytvoření zásady replikace](./media/site-recovery-setup-replication-settings-vmware/Edit-Policy.png)

3. Změňte nastavení podle vašich potřeb.
4. Klikněte nahoře na Uložit. Zásady by se měla uložit během přibližně 2 až 5 minut v závislosti na tom, kolik virtuálních počítačů tuto zásadu replikace používá.

![Vytvoření zásady replikace](./media/site-recovery-setup-replication-settings-vmware/Save-Policy.png)

## <a name="dissociate-configuration-server-from-replication-policy"></a>Zrušení přidružení konfiguračního serveru k zásadě replikace
1. Klikněte na zásadu replikace, ke které chcete přidružit konfigurační server.
2. Klikněte v horní části na Zrušit přidružení.
3. Ze seznamu serverů vyberte konfigurační server.
4. Klikněte na tlačítko OK. Přidružení konfiguračního serveru by se mělo zrušit během přibližně 1 až 2 minut.
    
    > [!NOTE] 
    > Přidružení konfiguračního serveru nejde zrušit, pokud existuje alespoň jedna replikovaná položka, která zásadu používá. Před zrušením přidružení konfiguračního serveru zkontrolujte, že žádné replikované položky zásadu nepoužívají.

## <a name="delete-replication-policy"></a>Odstranění zásady replikace 

1. Klikněte na zásadu replikace, kterou chcete odstranit.
2. Klikněte na Odstranit. Zásada by se měla odstranit během přibližně 30 sekund až 1 minuty.

    > [!NOTE] 
    > Zásadu replikace nejde odstranit, pokud má přidružený aspoň jeden konfigurační server. Zkontrolujte, že neexistují žádné replikované položky, které zásadu používají, a před odstraněním zásady odstraňte všechny přidružené konfigurační servery.


<!--HONumber=Jan17_HO4-->


