---
title: "Azure šifrování služby úložiště pomocí zákazníka spravované klíče v Azure Key Vault | Microsoft Docs"
description: "Použít funkci šifrování služby úložiště Azure pro šifrování Azure Blob Storage na straně služby při ukládání dat a data dešifrovat při načítání dat pomocí zákazníka spravovaných klíčů."
services: storage
documentationcenter: .net
author: lakasa
manager: jahogg
editor: tysonn
ms.assetid: 
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/07/2017
ms.author: lakasa
ms.openlocfilehash: 6d1e6752fb631114f5be06cb27a63e40547bf6ca
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/11/2017
---
# <a name="storage-service-encryption-using-customer-managed-keys-in-azure-key-vault"></a>Šifrování služby úložiště pomocí klíčů zákazníků spravované v Azure Key Vault

Microsoft Azure se zaměřuje na pomáhá chránit a ochranu dat, aby splňovaly vaše organizace zabezpečení a dodržování předpisů závazky.  Jedním ze způsobů, budete moci chránit vaše data v klidovém stavu je použití šifrování služby úložiště (SSE), který automaticky při zápisu do úložiště data šifruje a dešifruje data při jeho načítání. Šifrování a dešifrování je automatická, naprosto transparentní a používá 256 bitů [šifrování AES](https://en.wikipedia.org/wiki/Advanced_Encryption_Standard), jednu z nejsilnějších bloku šifer k dispozici.

Spravované společností Microsoft šifrovací klíče můžete použít s SSE nebo můžete použít vlastní šifrovací klíče. Tento článek popisuje ty druhé. Další informace o používání klíčů spravovaný společností Microsoft nebo o SSE v obecné najdete v tématu [šifrování služby úložiště pro Data v klidovém stavu](../storage-service-encryption.md).

Pokud chcete zadat možnost používat vlastní šifrovací klíče, je integrovaná SSE pro úložiště objektů Blob trezoru službou klíč s Azure (AZURE). Můžete vytvořit vlastní šifrovací klíče a uložit je do službou AZURE nebo rozhraní API je službou AZURE můžete použít ke generování šifrovacích klíčů. Nejen službou AZURE umožňuje spravovat a řídit klíče, taky umožňuje auditovat vaše použití klíče. 

Proč byste se měli k vytvoření vlastních klíčů? Nabízí větší flexibilitu, což umožňuje vytvářet, otáčení, zakázat a definovat řízení přístupu. Také umožňuje audit šifrovací klíče umožňuje chránit vaše data.

## <a name="sse-with-customer-managed-keys-preview"></a>SSE se customer preview spravovaných klíčů

Tato funkce je aktuálně ve verzi Preview. Chcete-li tuto funkci používat, vytvořte nový účet úložiště. Můžete buď vytvořit nový trezor klíčů a klíč nebo můžete použít existující trezor klíčů a klíč. Účet úložiště a trezoru klíčů musí být ve stejné oblasti, ale mohou být v různých předplatných.

Obraťte se na k účasti ve verzi preview, [ ssediscussions@microsoft.com ](mailto:ssediscussions@microsoft.com). Poskytujeme speciálního odkazu k účasti ve verzi preview.

Další informace naleznete [– nejčastější dotazy](#frequently-asked-questions-about-storage-service-encryption-for-data-at-rest).

> [!IMPORTANT]
> Ve verzi Preview před podle kroků v tomto článku, musíte se přihlásit. Bez přístup s náhledem nebudete moci povolit tuto funkci na portálu.

## <a name="getting-started"></a>Začínáme
## <a name="step-1-create-a-new-storage-accountstorage-create-storage-accountmd"></a>Krok 1: [vytvořit nový účet úložiště](../storage-create-storage-account.md)

## <a name="step-2-enable-encryption"></a>Krok 2: Povolení šifrování
Můžete povolit SSE pro účet úložiště pomocí [portál Azure](https://portal.azure.com). V okně nastavení pro účet úložiště vyhledejte v části služby objektů Blob, jak je znázorněno na následujícím obrázku a klikněte na šifrování.

![Možnost šifrování portálu snímek obrazovky zobrazující](./media/storage-service-encryption-customer-managed-keys/ssecmk1.png)
<br/>*Povolit SSE pro služby objektů Blob*

Pokud chcete prostřednictvím kódu programu povolit nebo zakázat šifrování služby úložiště na účet úložiště, můžete použít [REST API služby Azure Storage prostředků zprostředkovatele](https://docs.microsoft.com/en-us/rest/api/storagerp/?redirectedfrom=MSDN), [prostředků zprostředkovatele Klientská knihovna pro úložiště pro .NET ](https://docs.microsoft.com/en-us/dotnet/api/?redirectedfrom=MSDN), [Prostředí azure PowerShell](https://docs.microsoft.com/en-us/powershell/azure/overview?view=azurermps-4.0.0), nebo [rozhraní příkazového řádku Azure](https://docs.microsoft.com/en-us/azure/storage/storage-azure-cli).

Na této obrazovce Pokud nevidíte zaškrtávací políčko "použití vlastní klíč", můžete nebylo schváleno ve verzi preview. Odeslat e-mail, který [ ssediscussions@microsoft.com ](mailto:ssediscussions@microsoft.com) a požádat o schválení.

![Portál snímek obrazovky zobrazující šifrování Preview](./media/storage-service-encryption-customer-managed-keys/ssecmk1.png)

Ve výchozím nastavení používá SSE spravovaný Microsoftem klíče. Pokud chcete používat vlastní klíče, zaškrtněte políčko. Potom můžete buď zadat klíč identifikátor URI, nebo vybrat klíč a Key Vault z nástroje pro výběr.

## <a name="step-3-select-your-key"></a>Krok 3: Vyberte klíč

![Portálu snímek obrazovky zobrazující šifrování použijte možnost vlastní klíče](./media/storage-service-encryption-customer-managed-keys/ssecmk2.png)

![Portál snímek obrazovky s šifrování s, zadejte možnost klíče uri](./media/storage-service-encryption-customer-managed-keys/ssecmk3.png)

Pokud účet úložiště nemá přístup do služby Key Vault, můžete spustit následující příkaz pomocí Azure Powershell k udělení přístupu k účtům úložiště do požadované trezoru klíčů.

![Portál snímek obrazovky ukazující, přístup k trezoru klíčů odepřen](./media/storage-service-encryption-customer-managed-keys/ssecmk4.png)

Můžete také udělit přístup prostřednictvím portálu Azure tak, že přejdete do Azure Key Vault na portálu Azure a udělení přístupu k účtu úložiště.

## <a name="step-4-copy-data-to-storage-account"></a>Krok 4: Kopírování dat do účtu úložiště
Pokud chcete k přenosu dat do nového účtu úložiště tak, aby se šifrují, podívejte se na [krok 3 z Začínáme v šifrování služby úložiště pro Data v klidovém stavu](https://docs.microsoft.com/en-us/azure/storage/storage-service-encryption#step-3-copy-data-to-storage-account).

## <a name="step-5-query-the-status-of-the-encrypted-data"></a>Krok 5: Dotaz na stav šifrovaná data
Dotaz na stav šifrovaná data, najdete v tématu [krok 4 z Začínáme v šifrování služby úložiště pro Data v klidovém stavu](https://docs.microsoft.com/en-us/azure/storage/storage-service-encryption#step-4-query-the-status-of-the-encrypted-data).

## <a name="frequently-asked-questions-about-storage-service-encryption-for-data-at-rest"></a>Nejčastější dotazy o šifrování služby úložiště pro Data v klidovém stavu
**Otázka: používám storage úrovně Premium; můžete použít SSE pomocí klíčů zákazníků spravovat?**

Odpověď: Ano SSE s spravovaný společností Microsoft a k zákaznickým spravované klíče je podporována v úložiště úrovně Standard a Premium storage. 

**Otázka: je možné vytvořit nové účty úložiště s SSE pomocí klíčů zákazníků spravované povolit pomocí Azure PowerShell a rozhraní příkazového řádku Azure?**

Odpověď: Ano.

**Otázka: jak mnohem víc náklady na úložiště Azure nepodporuje když SSE s zákazníka spravované klíče je povolen?**

Odpověď: je s náklady související pro použití Azure Key Vault. Další podrobnosti najdete [klíč trezoru ceny](https://azure.microsoft.com/en-us/pricing/details/key-vault/). Není k dispozici pro použití SSE bez dalších nákladů.

**Otázka: je možné odvolat přístup k šifrovacím klíčům?**

A: Ano, můžete kdykoli odvolat přístup. Existuje několik způsobů odvolat přístup k klíče. Odkazovat na [Azure Key Vault PowerShell](https://docs.microsoft.com/en-us/powershell/module/azurerm.keyvault/?view=azurermps-4.0.0) a [příkazového řádku Azure Key Vault](https://docs.microsoft.com/en-us/cli/azure/keyvault) další podrobnosti. Odvolání přístupu bude efektivně blokovat přístup k všech objektů BLOB v účtu úložiště, protože účet šifrovací klíč je pravděpodobně nepřístupný úložiště Azure.

**Otázka: je možné vytvořit účet úložiště a klíč v jiné oblasti?**

A: účet úložiště a klíč nebo klíč trezoru Ne, musí být ve stejné oblasti. 

**Otázka: je možné povolit SSE pomocí klíčů zákazníků spravované při vytváření účtu úložiště?**

Odpověď: Ne. Když povolíte SSE při vytváření účtu úložiště, můžete použít pouze spravovaný Microsoftem klíče. Pokud chcete používat klíče zákazníků spravovat, je nutné aktualizovat vlastnosti účtu úložiště. Můžete použít REST nebo jeden z knihovny klienta úložiště prostřednictvím kódu programu aktualizace účtu úložiště nebo aktualizovat vlastnosti účtu úložiště pomocí portálu Azure po vytvoření účtu.

**Otázka: je možné zakázat šifrování, zatímco SSE pomocí zákazníka spravované klíče?**

A: šifrování nelze zakázat Ne, zatímco SSE pomocí zákazníka spravované klíče. Můžete zakázat šifrování, je nutné přepnout pomocí klíče spravovaný Microsoftem. To provedete pomocí portálu Azure nebo Powershellu.

**Otázka: je SSE ve výchozím nastavení povolené, po vytvoření nového účtu úložiště?**

Odpověď: SSE není povoleno ve výchozím nastavení; na portálu Azure můžete ji povolit. Můžete také prostřednictvím kódu programu povolit tuto funkci pomocí rozhraní REST API poskytovatele prostředků úložiště. 

**Otázka: I nelze povolit šifrování na svůj účet úložiště.**

Odpověď: je účet správce prostředků úložiště? Klasické účty úložiště nejsou podporovány. SSE pomocí klíčů zákazníků spravovat lze také povolit pouze na nově vytvořené účty úložiště Resource Manager.

**Otázka: je SSE s zákazníka spravované klíče je povolena pouze v určitých oblastí?**

Odpověď: SSE je k dispozici v pouze určité oblasti pro úložiště objektů Blob pro tuto verzi Preview. E-mailu [ ssediscussions@microsoft.com ](mailto:ssediscussions@microsoft.com) ke kontrole dostupnosti a podrobnosti o verzi preview. 

**Otázka: jak I contact někdo po jakýchkoli problémů nebo chcete poskytnout zpětnou vazbu?**

Odpověď: kontaktní [ ssediscussions@microsoft.com ](mailto:ssediscussions@microsoft.com) pro veškeré problémy související s šifrování služby úložiště. 

## <a name="next-steps"></a>Další kroky

*   Další informace o komplexní sadu zabezpečení možnosti, které pomáhají vývojářům vytvářet aplikace, zabezpečení naleznete v tématu [Průvodce zabezpečením úložiště](https://docs.microsoft.com/en-us/azure/storage/storage-security-guide).
*   Souhrnné informace o Azure Key Vault naleznete v tématu [co je Azure Key Vault](https://docs.microsoft.com/en-us/azure/key-vault/key-vault-whatis)?
*   Začínáme v Azure Key Vault, najdete v části [Začínáme s Azure Key Vault](../../key-vault/key-vault-get-started.md).
