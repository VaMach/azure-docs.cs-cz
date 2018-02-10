---
title: Zahrnout soubor
description: Zahrnout soubor
services: backup
author: markgalioto
ms.service: backup
ms.topic: include
ms.date: 2/7/2018
ms.author: trinadhk
ms.custom: include file
ms.openlocfilehash: 7ca5b34961b4d0e3d4fcecb8175e3e0901d7049d
ms.sourcegitcommit: 059dae3d8a0e716adc95ad2296843a45745a415d
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 02/09/2018
---
Následující omezení platí pro zálohování Azure.

| Identifikátor limit | Výchozí omezení |
| --- | --- |
| Počet serverů/počítačů, které lze registrovat k trezoru |50 pro Windows Server nebo klienta nebo SCDPM <br/> 200 pro virtuální počítače IaaS |
| Velikost zdroje dat pro data uložená v úložišti Azure trezoru |54400 GB max<sup>1</sup> |
| Počet trezorů záloh, které lze vytvořit v rámci předplatného Azure |25 trezorů služeb zotavení podle oblastí |
| Počet pokusů, které lze naplánovat zálohování za den |3 za den pro Windows Server nebo klienta <br/> 2 na každý den pro SCDPM <br/> Jednou denně pro virtuální počítače IaaS |
| Datových disků připojených k virtuální počítač pro zálohování Azure |16 |
| Velikost jednotlivých datový disk připojen k virtuální počítač pro zálohování Azure| 1024 GB <sup>2</sup>|

* <sup>1</sup>54400 GB omezení se nevztahuje na zálohování virtuálních počítačů IaaS.
* <sup>2</sup> máme [privátní Preview verzi](https://gallery.technet.microsoft.com/Instant-recovery-point-and-25fe398a?redir=0) pro podporu disků až 4 TB. 

