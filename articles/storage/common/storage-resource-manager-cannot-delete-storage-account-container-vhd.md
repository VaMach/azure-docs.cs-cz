---
title: "Řešení chyb při odstranění účtů úložiště Azure, kontejnery nebo virtuální pevné disky | Microsoft Docs"
description: "Řešení chyb při odstranění účtů úložiště Azure, kontejnery nebo virtuální pevné disky"
services: storage
documentationcenter: 
author: passaree
manager: cshepard
editor: na
tags: storage
ms.assetid: 17403aa1-fe8d-45ec-bc33-2c0b61126286
ms.service: storage
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: troubleshooting
ms.date: 06/13/2017
ms.author: genli
ms.openlocfilehash: 174ab97ac14f4c05690306691df5ee4b6d33dd93
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/11/2017
---
# <a name="troubleshoot-storage-delete-errors-in-resource-manager-deployment"></a>Řešení chyb odstranit úložiště v nasazení Resource Manager
Tento článek obsahuje pokyny k odstraňování problémů, když jeden z těchto chyb dojít při pokusu o odstranění účtu úložiště Azure, kontejner nebo objekt blob v nasazení Azure Resource Manager.

>**Odstranění účtu úložiště 'StorageAccountName' se nezdařilo. Chyba: Účet úložiště nelze odstranit, protože některé jeho artefakty se používají.**

>**Nepodařilo se odstranit # mimo kontejnery #:<br>virtuální pevné disky: v současné době není zapůjčení na kontejneru a žádné ID zapůjčení zadaná v žádosti.**

>**Nepodařilo se odstranit # mimo # objekty BLOB:<br>BlobName.vhd: u objektu blob je momentálně zapůjčení a žádné ID zapůjčení zadaná v žádosti.**

Virtuální pevné disky používat ve virtuálních počítačích Azure jsou soubory VHD uložené jako objekty BLOB stránky v účtu úložiště standard nebo premium v Azure.  Můžete najít další informace o Azure disky [zde](../../virtual-machines/windows/about-disks-and-vhds.md). Azure zabraňuje odstranění disku, který je připojen k virtuálnímu počítači zabránit poškození. Rovněž zamezí odstranění kontejnerů a účty úložiště, které mají objekt blob stránky, který je připojen k virtuálnímu počítači. 

## <a name="solution"></a>Řešení
Postup při přijímání jedné z výše uvedených chyb odstranit účet úložiště, kontejner nebo objekt blob je: 
1. [Identifikovat objekty BLOB, které jsou připojené k virtuálnímu počítači](#step-1-identify-blobs-attached-to-a-vm)
2. [Připojené odstranění virtuálních počítačů s **disk operačního systému**](#step-2-delete-vm-to-detach-os-disk)
3. [Odpojte všechny **datové disky** z zbývající virtuální počítače](#step-3-detach-data-disk-from-the-vm)

Po dokončení výše uvedené kroky opakujte odstraňování účtu úložiště, kontejner nebo objekt blob.

### <a name="step-1-identify-blob-attached-to-a-vm"></a>Krok 1: Identifikace objektů blob, které jsou připojené k virtuálnímu počítači

#### <a name="scenario-1-deleting-a-blob--identify-attached-vm"></a>Scénář 1: Odstranění objektu blob – identifikovat připojených virtuálních počítačů
1. Přihlaste se k webu [Azure Portal](https://portal.azure.com).
2. V nabídce centra vyberte **všechny prostředky**. Přejděte na účet úložiště, v části **služby objektů Blob** vyberte **kontejnery** a přejděte do objektu blob pro odstranění.
3. Pokud objekt blob **stavu zapůjčení** je **pronajatých**, klikněte pravým tlačítkem a vyberte **upravit Metadata** otevřete podokno metadata objektu Blob. 

    ![Snímek obrazovky portálu, s úložištěm objektů BLOB účtu a klikněte pravým tlačítkem na > hilighted "Upravit Metadata"](./media/storage-resource-manager-cannot-delete-storage-account-container-vhd/utd-edit-metadata-sm.png)

4. V podokně metadata objektu Blob, zkontrolujte a hodnoty pro tuto hodnotu zaznamenejte **MicrosoftAzureCompute_VMName**. Tato hodnota je název virtuálního počítače, virtuální pevný disk připojený k. (Viz **důležité** Pokud toto pole neexistuje.)
5. V podokně metadata objektu Blob, zkontrolujte a zaznamenejte hodnotu **MicrosoftAzureCompute_DiskType**. To označuje, jestli je připojený disk operačního systému nebo data (viz **důležité** Pokud toto pole neexistuje). 

     ![Snímek obrazovky portálu open podokně "Metadata objektu Blob" úložiště](./media/storage-resource-manager-cannot-delete-storage-account-container-vhd/utd-blob-metadata-sm.png)

6. Pokud je typ objektu blob disku **OSDisk** podle [krok 2: odstranění virtuálního počítače odpojit disk s operačním systémem](#step-2-delete-vm-to-detach-os-disk). Jinak, pokud je typ objektu blob disku **DataDisk** postupujte podle kroků v [krok 3: odpojit datový disk z virtuálního počítače](#step-3-detach-data-disk-from-the-vm). 

> [!IMPORTANT]
> Pokud **MicrosoftAzureCompute_VMName** a **MicrosoftAzureCompute_DiskType** se nezobrazí v metadata objektu blob, označuje, že objektu blob je explicitně pronajatý a není připojen k virtuálnímu počítači. Zapůjčených objekty BLOB nelze odstranit, aniž by vás první zapůjčení. Rozdělit zapůjčení, klikněte pravým tlačítkem na objekt blob a vyberte **zalomení zapůjčení**. Zapůjčených objekty BLOB, které nejsou připojeny k virtuálnímu počítači zabránit odstranění objektu blob ale nebudou bránit odstranění účtu kontejneru nebo úložiště.

#### <a name="scenario-2-deleting-a-container---identify-all-blobs-within-container-that-are-attached-to-vms"></a>Scénář 2: Odstranění kontejner - identifikovat všechny blob(s) v rámci kontejneru, které jsou připojené k virtuálním počítačům
1. Přihlaste se k webu [Azure Portal](https://portal.azure.com).
2. V nabídce centra vyberte **všechny prostředky**. Přejděte na účet úložiště, v části **služby objektů Blob** vyberte **kontejnery** a najít kontejner, aby ji odstranit.
3. Kliknutím otevřete kontejner a zobrazí se seznam objektů blob je uvnitř. Identifikujte všechny objekty BLOB s typem objektu Blob = **objekt blob stránky** a stavu zapůjčení = **pronajatých** z tohoto seznamu. Postupujte podle [scénáři 1](#step-1-identify-blobs-attached-to-a-vm) k identifikaci virtuálních počítačů spojené s každou z těchto objektů BLOB.

    ![Snímek obrazovky portálu s objekty BLOB účtu úložiště a "Zapůjčení stav" s "Pronajatých" zvýrazněná](./media/storage-resource-manager-cannot-delete-storage-account-container-vhd/utd-disks-sm.png)

4. Postupujte podle [kroku 2](#step-2-delete-vm-to-detach-os-disk) a [krok 3](#step-3-detach-data-disk-from-the-vm) odstranit virtuální počítače s **OSDisk** a odpojit **DataDisk**. 

#### <a name="scenario-3-deleting-storage-account---identify-all-blobs-within-storage-account-that-are-attached-to-vms"></a>Scénář 3: Odstranění úložiště účet – identifikace blob(s) v rámci účtu úložiště, které jsou připojené k virtuálním počítačům
1. Přihlaste se k webu [Azure Portal](https://portal.azure.com).
2. V nabídce centra vyberte **všechny prostředky**. Přejděte na účet úložiště, v části **služby objektů Blob** vyberte **kontejnery**.

    ![Snímek obrazovky portálu s kontejnery účtu úložiště a "Zapůjčení stavu" s "Pronajatých" zvýrazněná](./media/storage-resource-manager-cannot-delete-storage-account-container-vhd/utd-containers-sm.png)

3. V **kontejnery** okně identifikovat všechny kontejnery kde **stavu zapůjčení** je **pronajatých** a postupujte podle pokynů [scénáři 2](#scenario-2-deleting-a-container---identify-all-blobs-within-container-that-are-attached-to-vms) pro každou  **Pronajaté** kontejneru.
4. Postupujte podle [kroku 2](#step-2-delete-vm-to-detach-os-disk) a [krok 3](#step-3-detach-data-disk-from-the-vm) odstranit virtuální počítače s **OSDisk** a odpojit **DataDisk**. 

### <a name="step-2-delete-vm-to-detach-os-disk"></a>Krok 2: Odstranění virtuálního počítače odpojit disk operačního systému
Pokud virtuální pevný disk, disk operačního systému, je nutné odstranit virtuální počítač, než mohl být odstraněn připojený soubor VHD. Žádná další akce nutné pro datové disky připojené na stejný virtuální počítač po dokončení těchto kroků:

1. Přihlaste se k webu [Azure Portal](https://portal.azure.com).
2. V nabídce centra vyberte **virtuální počítače**.
3. Vyberte virtuální počítač, virtuální pevný disk připojený k.
4. Ujistěte se, že nic aktivně používá virtuální počítač a virtuální počítač už nepotřebujete.
5. V horní části **podrobnosti virtuálního počítače** vyberte **odstranit**a potom klikněte na **Ano** k potvrzení.
6. Virtuální počítač by měl být odstraněn, ale uchovávání může být virtuální pevný disk. Virtuální pevný disk však by už připojený k virtuálnímu počítači nebo mít zapůjčení na jeho. To může trvat několik minut, než zapůjčení k uvolnění. Pokud chcete ověřit, že zapůjčení vydání, přejděte do umístění objektu blob a v **Blob vlastnosti** podokně **zapůjčení stav** by měla být **dostupné**.

### <a name="step-3-detach-data-disk-from-the-vm"></a>Krok 3: Odpojte datový disk z virtuálního počítače
Pokud virtuální pevný disk je datový disk, odpojte virtuální pevný disk z virtuálního počítače odebrat zapůjčení:

1. Přihlaste se k webu [Azure Portal](https://portal.azure.com).
2. V nabídce centra vyberte **virtuální počítače**.
3. Vyberte virtuální počítač, virtuální pevný disk připojený k.
4. Vyberte **disky** na **podrobnosti virtuálního počítače** okno.
5. Vyberte datový disk k odstranění virtuální pevný disk je připojen k. Můžete určit, které objekt blob je připojen na disku kontrolou adresu URL virtuálního pevného disku.
6. Umístění objektu blob můžete ověřit kliknutím na disk, který Zkontrolujte cestu **URI virtuálního pevného disku** pole.
7. Vyberte **upravit** na **disky** okno.
8. Klikněte na tlačítko **odpojit ikonu** z datový disk k odstranění.

     ![Snímek obrazovky portálu open podokně "Metadata objektu Blob" úložiště](./media/storage-resource-manager-cannot-delete-storage-account-container-vhd/utd-vm-disks-edit.png)

9. Vyberte **Uložit**. Z virtuálního počítače, by měl nyní odpojit disk a virtuální pevný disk na něm už měli zapůjčení. To může trvat několik minut, než zapůjčení k uvolnění. Pokud chcete ověřit, že byla vydána zapůjčení, přejděte do umístění objektu blob a v **Blob vlastnosti** podokně **zapůjčení stav** hodnota by měla být **Odemknutý** nebo **k dispozici**.

## <a name="next-steps"></a>Další kroky
Opakujte odstranění objektu úložiště, který měl předtím selhal.

