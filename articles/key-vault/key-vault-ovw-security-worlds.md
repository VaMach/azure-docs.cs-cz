---
ms.assetid: 
title: Azure Key Vault architektury security World | Microsoft Docs
ms.service: key-vault
author: BrucePerlerMS
ms.author: bruceper
manager: mbaldwin
ms.date: 07/03/2017
ms.openlocfilehash: 921bbd109c9ea98d8b5c286a7512bed026412d26
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/11/2017
---
# <a name="azure-key-vault-security-worlds-and-geographic-boundaries"></a>Azure Key Vault architektury security World a geografické hranice.

Azure Key Vault je víceklientské služby a používá fond modulů hardwarového zabezpečení (HSM) v každém umístění Azure. 

Všechny moduly hardwarového zabezpečení v Azure umístěních ve stejné geografické oblasti sdílet stejnou hranici kryptografických (Thales Security World). Východ USA a západní USA sdílet stejnou architektury security world, protože náleží do US geografické umístění. Podobně všech Azure umístění v Japonsku sdílet stejnou architektury security world a všechny Azure umístění v Austrálii, Indie a tak dále. 

## <a name="backup-and-restore-behavior"></a>Zálohování a obnovení chování

Zálohy klíče z trezoru klíčů v jednom místě, Azure možné obnovit do trezoru klíčů v jiném umístění Azure, tak dlouho, dokud jsou splněny obě tyto podmínky:

- Z Azure umístění patřit do stejného geografického umístění
- Obě trezorů klíčů patří do stejného předplatného Azure

Například zálohy podle konkrétního předplatného klíče v trezoru klíčů v Indie – Západ, lze obnovit pouze na jiné trezoru klíčů ve stejném předplatném a umístění geograficky; Indie – Západ, Indie centrální nebo – Jih, Indie.

## <a name="regions-and-products"></a>Oblasti a produkty

- [Oblasti Azure](https://azure.microsoft.com/regions/)
- [Produkty společnosti Microsoft podle oblasti](https://azure.microsoft.com/regions/services/)

Oblasti jsou namapované na architektury security World, zobrazené jako hlavní záhlaví v tabulkách:

V produktech článkem oblast, například **Americas** karta obsahuje VÝCHOD USA, STŘED USA, ZÁPADNÍ USA veškerá mapování Americas oblasti. 

>[!NOTE]
>Výjimkou je, že nám DOD VÝCHOD a nám DOD centrální mají své vlastní architektury security World. 

Podobně na **Evropa** kartě, severní EVROPA a ZÁPADNÍ EVROPA obě namapovány na oblasti Evropa. Totéž platí také na **Asie a Tichomoří** kartě.



