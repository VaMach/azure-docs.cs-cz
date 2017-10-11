---
title: "Zálohování a obnovení pro SQL Server | Microsoft Docs"
description: "Popisuje aspekty zálohování a obnovení pro databáze systému SQL Server, které jsou spuštěné na virtuálních počítačích Azure."
services: virtual-machines-windows
documentationcenter: na
author: MikeRayMSFT
manager: jhubbard
editor: 
tags: azure-resource-management
ms.assetid: 95a89072-0edf-49b5-88ed-584891c0e066
ms.service: virtual-machines-sql
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows-sql-server
ms.workload: iaas-sql-server
ms.date: 11/15/2016
ms.author: mikeray
ms.openlocfilehash: 65557938673c5442758396a47873be1016e0f71b
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="backup-and-restore-for-sql-server-in-azure-virtual-machines"></a>Zálohování a obnovení pro SQL Server v Azure Virtual Machines
## <a name="overview"></a>Přehled
Úložiště Azure udržuje 3 kopie všechny disky virtuálního počítače Azure, pokud chcete zajistit ochranu proti ztrátě dat nebo poškození dat fyzické. Na rozdíl od místní, nepotřebujete tedy starat o těchto. Ale měli stále zálohování databáze SQL Server k ochraně proti chybám aplikaci nebo uživatele (např. vkládání chybná data nebo odstranění tabulky) a schopnost obnovit k určitému bodu v čase.

[!INCLUDE [learn-about-deployment-models](../../../../includes/learn-about-deployment-models-both-include.md)]

Pro SQL Server běžící ve virtuálních počítačích Azure můžete pomocí nativního zálohování a obnovení techniky pomocí připojených disků pro cíl zálohování souborů. Existuje ale omezení počtu disků můžete připojit k Azure virtuálnímu počítači, na základě [velikost virtuálního počítače](../sizes.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json). Je také režii správy disků vzít v úvahu.

Od verze SQL Server 2014, můžete zálohovat a obnovit do Microsoft Azure Blob storage. SQL Server 2016 také nabízí vylepšení pro tuto možnost. Kromě toho pro databázové soubory uložené v úložišti objektů Blob Microsoft Azure, SQL Server 2016 poskytuje možnost pro téměř okamžité zálohy a pro rychlé obnovení pomocí Azure snímků. Tento článek obsahuje přehled těchto možností a další informace naleznete na [zálohování systému SQL Server a obnovení službou Microsoft Azure Blob Storage](https://msdn.microsoft.com/library/jj919148.aspx).

> [!NOTE]
> Informace o možnosti pro zálohování velmi velké databáze, najdete v části [Víceterabajtové SQL Server databáze zálohování strategie pro virtuální počítače Azure](http://blogs.msdn.com/b/igorpag/archive/2015/07/28/multi-terabyte-sql-server-database-backup-strategies-for-azure-virtual-machines.aspx).
> 
> 

V níže uvedených částech obsahují informace, které jsou specifické pro různé verze systému SQL Server podporovanou ve virtuálním počítači Azure.

## <a name="sql-server-virtual-machines"></a>Virtuální počítače se systémem SQL Server
Pokud vaše instance systému SQL Server běží na virtuální počítač Azure, soubory databáze již nacházet v datových disků v Azure. Tyto disky za provozu v Azure Blob storage. Proto z důvodů pro zálohování databáze a přístupy trvat mírně změnit. Pamatujte na následující věci. 

* Již nepotřebujete provádět zálohování databáze k zajištění ochrany proti selhání hardwaru nebo média, protože Microsoft Azure poskytuje tuto ochranu jako součást služby Microsoft Azure.
* Stále je nutné provést zálohování databáze k zajištění ochrany proti chybám uživatele nebo pro archivační účely, regulačních důvodů nebo pro účely správy.
* Záložní soubor lze uložit přímo v Azure. Další informace najdete v následujících částech, které obsahují pokyny pro různé verze systému SQL Server.

## <a name="sql-server-2016"></a>SQL Server 2016
Podporuje Microsoft SQL Server 2016 [zálohování a obnovení s Azure blob](https://msdn.microsoft.com/library/jj919148.aspx) funkce nalézt v systému SQL Server 2014. Můžete ale také obsahuje následující vylepšení:

| Vylepšení 2016 | Podrobnosti |
| --- | --- |
| **Proložení** |Při zálohování do úložiště objektů blob Microsoft Azure, SQL Server 2016 podporuje zálohování na více objektů BLOB k povolení zálohování velké databáze, až do maximálního počtu 12,8 TB. |
| **Zálohy snímku** |Prostřednictvím Azure snímků zálohy snímku souboru SQL Server poskytuje téměř okamžité zálohy a rychlé obnovení pro soubory databáze uložené pomocí služby Azure Blob storage. Tato funkce umožňuje zjednodušit zálohování a obnovení zásad. Zálohy snímku souboru také podporuje bod v době obnovení. Další informace najdete v tématu [snímek zálohy pro soubory databáze v Azure](https://msdn.microsoft.com/library/mt169363%28v=sql.130%29.aspx). |
| **Spravované plánování zálohování** |Zálohování spravované systému SQL Server do Azure nyní podporuje vlastní plány. Další informace najdete v tématu [zálohování spravované systému SQL Server do služby Microsoft Azure](https://msdn.microsoft.com/library/dn449496.aspx). |

Možnosti systému SQL Server 2016 při použití Azure Blob storage, na adrese [kurz: služby Microsoft Azure Blob storage pomocí databáze SQL serveru 2016](https://msdn.microsoft.com/library/dn466438.aspx).

## <a name="sql-server-2014"></a>SQL Server 2014
SQL Server 2014 obsahuje následující vylepšení:

1. **Zálohování a obnovení do Azure**:
   
   * *Zálohování systému SQL Server na adresu URL* má nyní podpora v systému SQL Server Management Studio. Možnost zálohovat do Azure je nyní k dispozici při použití úlohu zálohování nebo obnovení, nebo Průvodce plánu údržby v aplikaci SQL Server Management Studio. Další informace najdete v tématu [zálohování systému SQL Server na adresu URL](https://msdn.microsoft.com/library/jj919148%28v=sql.120%29.aspx).
   * *SQL Server spravované zálohování do Azure* obsahuje nové funkce, která umožňuje automatické zálohování správy. To je užitečné zejména pro automatizaci správy zálohování pro SQL Server 2014 instancí spuštěných na počítači Azure. Další informace najdete v tématu [zálohování spravované systému SQL Server do služby Microsoft Azure](https://msdn.microsoft.com/library/dn449496%28v=sql.120%29.aspx).
   * *Automatizované zálohování* poskytuje další automatizace se automaticky povolit *zálohování spravované systému SQL Server do Azure* na všechny stávající a nové databáze pro virtuální počítač SQL Server v Azure. Další informace najdete v tématu [Automatizované zálohování pro SQL Server v Azure Virtual Machines](virtual-machines-windows-sql-automated-backup.md).
   * Přehled všech možností pro zálohování systému SQL Server 2014 do Azure najdete v tématu [zálohování systému SQL Server a obnovení službou Microsoft Azure Blob Storage](https://msdn.microsoft.com/library/jj919148%28v=sql.120%29.aspx).
2. **Šifrování**: SQL Server 2014 podporuje šifrování dat při vytváření zálohy. Podporuje několik šifrovacích algoritmů a použití osf certifikát nebo asymetrický klíč. Další informace najdete v tématu [šifrování zálohy](https://msdn.microsoft.com/library/dn449489%28v=sql.120%29.aspx).

## <a name="sql-server-2012"></a>SQL Server 2012
Podrobné informace o zálohování systému SQL Server a obnovení v systému SQL Server 2012 najdete v tématu [zálohování a obnovení databází SQL serveru (SQL Server 2012)](https://msdn.microsoft.com/library/ms187048%28v=sql.110%29.aspx).

Od verze SQL Server 2012 SP1 kumulativní aktualizaci 2, můžete zálohování a obnovení ze služby Azure Blob Storage. Toto vylepšení slouží k zálohování databází systému SQL Server na SQL serveru se systémem na virtuální počítač Azure nebo místní instance. Další informace najdete v tématu [zálohování systému SQL Server a obnovení služby úložiště objektů Blob Azure](https://msdn.microsoft.com/library/jj919148%28v=sql.110%29.aspx).

Některé z výhod používání služby Azure Blob storage patří možnost obejít limit 16 disků pro připojené disky, snadnou správu přímé dostupnost záložní soubor do jiné instance systému instance systému SQL Server spuštěna na virtuálním počítači Azure nebo místní instanci pro migraci nebo pro účely obnovení po havárii. Úplný seznam výhody služby Azure blob storage pro zálohování systému SQL Server, najdete v článku *výhody* kapitoly [zálohování systému SQL Server a obnovení služby úložiště objektů Blob Azure](https://msdn.microsoft.com/library/jj919148%28v=sql.110%29.aspx).

Doporučení osvědčených postupů a informace o řešení potíží najdete v tématu [zálohování a obnovení osvědčených postupů (služby úložiště objektů Blob Azure)](https://msdn.microsoft.com/library/jj919149%28v=sql.110%29.aspx).

## <a name="sql-server-2008"></a>SQL Server 2008
Zálohování systému SQL Server a obnovení v systému SQL Server 2008 R2 najdete v tématu [zálohování a obnovení databází v systému SQL Server (SQL Server 2008 R2)](https://msdn.microsoft.com/library/ms187048%28v=sql.105%29.aspx).

Zálohování systému SQL Server a obnovení v systému SQL Server 2008 naleznete v tématu [zálohování a obnovení databází v systému SQL Server (SQL Server 2008)](https://msdn.microsoft.com/library/ms187048%28v=sql.100%29.aspx).

## <a name="next-steps"></a>Další kroky
Pokud plánujete nasazení systému SQL Server ve virtuálním počítači Azure, můžete najít zřizování pokyny v tomto kurzu následující: [zřizování virtuálního počítače systému SQL Server na platformě Azure pomocí Azure Resource Manageru](virtual-machines-windows-portal-sql-server-provision.md).

Zálohování a obnovení lze pro migraci dat, ale jsou potenciálně jednodušší cesty migrace dat do systému SQL Server na virtuálním počítači Azure. Úplnou diskusi o možnostech migrace a doporučení, najdete v části [migrace databáze do systému SQL Server na virtuálním počítači Azure](virtual-machines-windows-migrate-sql.md).

Přečtěte si další [prostředky pro spuštění systému SQL Server v Azure Virtual Machines](virtual-machines-windows-sql-server-iaas-overview.md).

