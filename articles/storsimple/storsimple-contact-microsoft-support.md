---
title: "Přihlaste se lístek podpory pro řady StorSimple 8000 | Microsoft Docs"
description: "Naučte se vytvořit žádost o podporu a spustit relaci podpory zařízení StorSimple."
services: storsimple
documentationcenter: 
author: alkohli
manager: timlt
editor: 
ms.assetid: 2ebc20fe-f490-4749-8e43-c9fac86f1676
ms.service: storsimple
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 02/27/2017
ms.author: alkohli;anbacker
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: cecc2566b432e897b5eff0c12e66598f0518ed80
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/11/2017
---
# <a name="contact-microsoft-support-for-your-storsimple"></a>Obraťte se na podporu společnosti Microsoft pro vaše zařízení StorSimple
Pokud narazíte na potíže s vaším řešením Microsoft Azure StorSimple, můžete vytvořit žádost o službu pro technickou podporu. V relaci online s svého specialistu technické podpory může také muset spustit relaci podpory zařízení StorSimple. Tento článek vás provede procesem:

* Postup vytvoření žádosti o podporu.
* Jak spustit relaci podpora v rozhraní Windows PowerShell zařízení StorSimple.

Zkontrolujte [StorSimple 8000 řady podporu SLA a informace o](https://msdn.microsoft.com/library/mt433077.aspx) předtím, než vytvoříte žádost o podporu.

## <a name="create-a-support-request"></a>Vytvořit žádost o podporu
Proveďte následující kroky k vytvoření žádosti o podporu:

#### <a name="to-create-a-support-request"></a>Chcete-li vytvořit žádost o podporu
1. V [portál Azure classic](https://manage.windowsazure.com/), v pravém horním rohu klikněte na název účtu a pak klikněte na tlačítko **obraťte se na podporu společnosti Microsoft**.
   
    ![MS obraťte se na podporu přes ManagementPortal](./media/storsimple-contact-microsoft-support/Ibiza1.png)
2. Budete přesměrováni na nový portál Azure (portal.azure.com). Klikněte **nová žádost o podporu** dlaždici.
   
    ![MS obraťte se na podporu prostřednictvím nového portálu](./media/storsimple-contact-microsoft-support/Ibiza2.png)
   
    Na pravé straně obrazovky **nová žádost o podporu** podokně se zobrazí. 
   
    ![Nové podokno žádosti o podporu](./media/storsimple-contact-microsoft-support/Ibiza3a.png)
3. V **Základy** dialogové okno pole, proveďte následující kroky:                                
   
   1. Z **vydat typ** rozevíracího seznamu vyberte **technické**.
   2. Vyberte **předplatné** z rozevíracího seznamu.
   3. Z **služby** rozevíracího seznamu vyberte **StorSimple**. 
   4. Vyberte **plán podpory** z rozevíracího seznamu. Je nutné placený plán podpory povolit technická podpora.
4. Klikněte na **Další**. **Problém** zobrazí se dialogové okno.
   
    ![Nové podokno žádosti o podporu](./media/storsimple-contact-microsoft-support/Ibiza5a.png) 
5. V **problém** dialogové okno pole, proveďte následující kroky:
   
   1. Vyberte **závažnost** úrovně z rozevíracího seznamu.
   2. Vyberte **typ problému** z rozevíracího seznamu.
   3. Vyberte **kategorie** z rozevíracího seznamu. 
   4. V **podrobnosti** pole Krátce popište svůj problém.
   5. V **časového rámce** pole, zda datum, čas a časové pásmo, které odpovídá nejaktuálnějšího výskytu vašeho problému.
   6. V části **nahrávání souborů**, klikněte na ikonu složky a přejděte do balíčku pro podporu.
   7. Vyberte **sdílet diagnostické informace** zaškrtávací políčko.
6. Klikněte na **Další**. **Kontaktní informace** zobrazí se dialogové okno.
   
    ![Nové podokno žádosti o podporu](./media/storsimple-contact-microsoft-support/Ibiza6a.png) 
7. Zadejte své kontaktní informace a vyberte způsob kontaktu (telefon nebo e-mail). 
8. Vyberte **uložit změny kontaktů pro budoucí požadavky na podporu** zaškrtávací políčko.
9. Klikněte na možnost **Vytvořit**.

Po odeslání vaši žádost o vás bude kontaktovat pracovníka podpory vám co nejdříve pokračovat k žádosti.

## <a name="start-a-support-session-in-windows-powershell-for-storsimple"></a>Spusťte relaci podpory v prostředí Windows PowerShell pro StorSimple
Chcete-li vyřešit potíže, které se můžete setkat s zařízení StorSimple, musíte použít s týmem Microsoft Support. Microsoft Support možná muset použít relaci podporu pro přihlášení k zařízení. 

Proveďte následující kroky pro spuštění relace podpory:

#### <a name="to-start-a-support-session"></a>Chcete-li spustit relaci podpory
1. Přístup k zařízení přímo pomocí konzoly sériového portu nebo prostřednictvím relace Telnetu ze vzdáleného počítače. Chcete-li to provést, postupujte podle kroků v [použití klienta PuTTY k připojení ke konzole sériového portu zařízení](storsimple-deployment-walkthrough.md#use-putty-to-connect-to-the-device-serial-console).
2. V této relaci, které se otevře, stiskněte **Enter** klíč k získání příkazového řádku.
3. V nabídce konzoly sériového portu, vyberte možnost 1, **přihlásit úplný přístup**.
4. Do příkazového řádku zadejte následující heslo: 
   
    `Password1`
5. Do příkazového řádku zadejte následující příkaz:
   
    `Enable-HcsSupportAccess`
6. Zobrazí se vám zašifrovaný řetězec. Zkopírujte tento řetězec do textového editoru, například Poznámkový blok.
7. Uložte tento řetězec a odeslání v e-mailovou zprávu na Microsoft Support. 

> [!IMPORTANT]
> Podpora přístupu můžete vypnout spuštěním `Disable-HcsSupportAccess`. Zařízení StorSimple se také pokusí zakázat přístup podporu 8 hodin po relace byl inicializován. Je osvědčeným postupem změnit svoje přihlašovací údaje zařízení StorSimple po inicializaci relaci podpory.
> 
> 

