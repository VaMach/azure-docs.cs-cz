---
title: "Šifrování v Azure Data Lake Store | Dokumentace Microsoftu"
description: "Pochopíte, jak funguje šifrování a obměna klíčů v Azure Data Lake Store"
services: data-lake-store
documentationcenter: 
author: yagupta
manager: 
editor: 
ms.assetid: 
ms.service: data-lake-store
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 4/14/2017
ms.author: yagupta
ms.translationtype: Human Translation
ms.sourcegitcommit: db034a8151495fbb431f3f6969c08cb3677daa3e
ms.openlocfilehash: 9bd9996a4a22b2f57510b6f3b6625e22b3183b1c
ms.contentlocale: cs-cz
ms.lasthandoff: 04/29/2017

---

# <a name="encryption-of-data-in-azure-data-lake-store"></a>Šifrování dat v Azure Data Lake Store

## <a name="overview-of-encryption-in-azure-data-lake-store"></a>Přehled šifrování v Azure Data Lake Store

Díky šifrování v Azure Data Lake Store (ADLS) můžete chránit svá data, implementovat zásady podnikového zabezpečení a splnit požadavky na dodržování legislativních předpisů. Tento článek poskytuje přehled návrhu a popisuje technické aspekty implementace šifrování dat ve službě Data Lake Store.

ADLS podporuje ve výchozím nastavení transparentní šifrování neaktivních uložených dat. Tady je podrobnější vysvětlení významu těchto termínů:

* Ve výchozím nastavení: Při vytváření nového účtu Azure Data Lake Store výchozí nastavení povolí šifrování. Následně se data ukládaná ve službě Data Lake Store budou šifrovat vždy před uložením na trvalé médium. Toto chování platí pro veškerá data a po vytvoření účtu nejde změnit.
* Transparentní: ADLS data automaticky šifruje před uložením a dešifruje před načtením. Konfiguraci a správu šifrování provádí správce na úrovni služby Data Lake Store. Rozhraní API pro přístup k datům se nemění a proto není nutné kvůli šifrování měnit aplikace a služby, které pracují se službou Data Lake Store.

Přenášená data se také vždy šifrují ve službě Data Lake Store. Kromě šifrování dat před uložením na trvalé médium se také vždy šifrují přenášená data pomocí protokolu HTTPS (Hypertext Transfer Protocol over Secure Sockets Layer). HTTPS je jediný protokol, který se podporuje v rozhraních REST služby Data Lake Store.

![Obrázek 1](./media/data-lake-store-encryption/fig1.png)


## <a name="setting-up-encryption-with-azure-data-lake-store"></a>Nastavení šifrování v Azure Data Lake Store

Šifrování se pro Azure Data Lake Store nastavuje během vytváření účtu a ve výchozím nastavení je vždy povoleno. Zákazníci mají možnost sami spravovat klíče nebo povolit službě Azure Data Lake Store, aby je spravovala za ně (výchozí nastavení).

Informace o nastavení šifrování v Azure Data Lake Store najdete v tématu [Začínáme](https://docs.microsoft.com/azure/data-lake-store/data-lake-store-get-started-portal).

## <a name="under-the-hood--how-encryption-works-in-azure-data-lake-store"></a>Pod pokličkou – Jak funguje šifrování v Azure Data Lake Store

### <a name="master-encryption-keys"></a>Hlavní šifrovací klíče

Azure Data Lake Store nabízí dva režimy pro správu hlavních šifrovacích klíčů. Použití hlavních šifrovacích klíčů je podrobněji popsáno níže. Prozatím předpokládejme, že hlavní šifrovací klíč je klíč nejvyšší úrovně. K dešifrování jakýchkoli dat uložených ve službě Data Lake Store se vyžaduje přístup k hlavnímu šifrovacímu klíči.

Pro správu hlavních šifrovacích klíčů existují tyto dva režimy:

1.    Klíče spravované službou
2.    Klíče spravované zákazníkem

V obou režimech je hlavní šifrovací klíč zabezpečen uložením ve službě Azure Key Vault. Azure Key Vault je plně spravovaná, vysoce zabezpečená služba v Azure, kterou můžete použít k bezpečnému ukládání kryptografických klíčů. Další informace o Azure Key Vault si můžete přečíst [zde](https://azure.microsoft.com/services/key-vault).

Tady je stručné porovnání možností, které nabízí dva režimy správy hlavních šifrovacích klíčů.

|  | Klíče spravované službou | Klíče spravované zákazníkem |
| --- | --- | --- |
|Jak se data ukládají?|Před uložením se vždy šifrují.|Před uložením se vždy šifrují.|
|Kde je uložený hlavní šifrovací klíč?|Azure Key Vault|Azure Key Vault|
|Jsou nějaké šifrovací klíče uložené v nezašifrované podobě mimo Azure Key Vault?|Ne|Ne|
|Je možné načíst hlavní šifrovací klíče z Azure Key Vault?|Ne. Po uložení ve službě Key Vault je lze použít pouze k šifrování a dešifrování.|Ne. Po uložení ve službě Key Vault je lze použít pouze k šifrování a dešifrování.|
|Kdo je vlastníkem služby Azure Key Vault a hlavního šifrovacího klíče?|Služba Azure Data Lake Store.|Zákazník je vlastníkem služby Azure Key Vault, která patří do jeho vlastního předplatného Azure. Hlavní šifrovací klíč ve službě Key Vault může být spravovaný softwarem nebo hardwarem (HSM).|
|Může zákazník službě Azure Data Lake Store odebrat přístup k hlavnímu šifrovacímu klíči?|Ne|Ano. Může spravovat seznamy řízení přístupu v Azure Key Vault a pro službu Azure Data Lake Store odebrat záznamy řízení přístupu k identitě služby.|
|Může zákazník trvale odstranit hlavní šifrovací klíč?|Ne|Ano. Pokud zákazník odstraní hlavní šifrovací klíč z Azure Key Vault, data v účtu ADLS nebude možné dešifrovat, a to ani službou Azure Data Lake Store. <br><br> Pokud zákazník hlavní šifrovací klíč před odstraněním z Azure Key Vault explicitně zazálohuje, bude možné ho obnovit a následně obnovit i data. Pokud ale zákazník hlavní šifrovací klíč před odstraněním z Azure Key Vault nezazálohuje, data v účtu ADLS pak již nikdy nebude možné dešifrovat.|


Kromě rozdílu na nejvyšší úrovni, tedy kdo spravuje hlavní šifrovací klíče a službu Key Vault, ve které se nachází, je zbytek návrhu pro oba režimy stejný.

S výběrem režimu pro hlavní šifrovací klíče souvisí několik důležitých aspektů, na které byste měli pamatovat.

1.    Možnost výběru, jestli chcete použít klíče spravované zákazníkem nebo klíče spravované službou ADLS, máte při zřizování účtu ADLS.
2.    Po zřízení účtu ADLS už režim nejde změnit.

### <a name="encryption-and-decryption-of-data"></a>Šifrování a dešifrování dat

V návrhu šifrování dat pro službu Azure Data Lake se používají tři typy klíčů. Následující tabulka shrnuje jejich význam:

| Klíč                   | Zkratka | Přidružený k | Umístění úložiště                             | Typ       | Poznámky                                                                                                   |
|-----------------------|--------------|-----------------|----------------------------------------------|------------|---------------------------------------------------------------------------------------------------------|
| Hlavní šifrovací klíč | MEK          | Účtu ADLS | Azure Key Vault                              | Asymetrický | Může být spravovaný službou ADLS nebo zákazníkem.                                                              |
| Šifrovací klíč dat   | DEK          | Účtu ADLS | Trvalé úložiště – spravované službou ADLS | Symetrický  | Klíč DEK je šifrovaný klíčem MEK a šifrovaný klíč DEK je to, co se ukládá na trvalé médium. |
| Šifrovací klíč bloku  | BEK          | Blokem dat | Žádný                                         | Symetrický  | Klíč BEK je odvozený od klíče DEK a příslušného datového bloku.                                                      |

Následující diagram znázorňuje tyto koncepty:

![Obrázek 2](./media/data-lake-store-encryption/fig2.png)

#### <a name="pseudo-algorithm-when-a-file-is-to-be-decrypted"></a>Pseudo algoritmus dešifrování souboru:
1.    Kontrola, jestli je klíč DEK pro účet ADLS uložený v mezipaměti a připravený k použití.
    * Pokud není, přečtení šifrovaného klíče DEK z trvalého úložiště a jeho odeslání do Azure Key Vault k dešifrování. Uložení dešifrovaného klíče DEK do mezipaměti – klíč je teď připravený k použití.
2.    Pro každý blok dat v souboru:
    * Načtení šifrovaného bloku dat z trvalého úložiště.
    * Vygenerování klíče BEK z klíče DEK a šifrovaného bloku dat.
    * Dešifrování dat pomocí klíče BEK.
#### <a name="pseudo-algorithm-when-a-block-of-data-is-to-be-encrypted"></a>Pseudo algoritmus šifrování bloku dat:
1.    Kontrola, jestli je klíč DEK pro účet ADLS uložený v mezipaměti a připravený k použití.
    * Pokud není, přečtení šifrovaného klíče DEK z trvalého úložiště a jeho odeslání do Azure Key Vault k dešifrování. Uložení dešifrovaného klíče DEK do mezipaměti – klíč je teď připravený k použití.
2.    Vygenerování jedinečného klíče BEK pro blok dat z klíče DEK.
3.    Šifrování datového bloku pomocí klíče BEK s použitím šifrování AES-256.
4.    Uložení šifrovaného datového bloku v trvalém úložišti.

> [!NOTE] 
> Z důvodu zajištění lepšího výkonu se šifrovací klíč dat v nezašifrované podobě uloží po velmi krátkou dobu do mezipaměti a po uplynutí této doby se okamžitě vymaže. Na trvalém médiu je vždy uložen zašifrovaný hlavním šifrovacím klíčem.

## <a name="key-rotation"></a>Obměna klíčů

Pokud používáte klíče spravované zákazníkem, Azure Data Lake Store umožňuje obměnu hlavního šifrovacího klíče. Informace o nastavení účtu ADLS s použitím klíčů spravovaných zákazníkem najdete na stránce [Začínáme](https://docs.microsoft.com/azure/data-lake-store/data-lake-store-get-started-portal).

### <a name="pre-requisites"></a>Požadavky

Při nastavování účtu Azure Data Lake zákazník zvolil možnost použití vlastních klíčů. Tuto možnost po vytvoření účtu nejde změnit. Pokud používáte výchozí možnosti šifrování, vaše data se vždy šifrují pomocí klíčů spravovaných službou Azure Data Lake. Při použití této možnosti zákazník nemá možnost obměňovat klíče, protože jsou spravované službou Azure Data Lake. Následující postup předpokládá, že používáte klíče spravované zákazníkem (tedy že jste zvolili vlastní klíče z vaší služby Key Vault).

### <a name="how-to-rotate-the-key-mek-in-azure-data-lake-store"></a>Obměna hlavního šifrovacího klíče v Azure Data Lake Store

1. Přihlaste se k novému webu [Azure Portal](https://portal.azure.com/).
2. Přejděte do služby Key Vault, ve které jsou uloženy klíče přidružené k vašemu účtu Azure Data Lake Store, a vyberte Klíče.

    ![Klíče](./media/data-lake-store-encryption/keyvault.png)

3.    Vyberte klíč přidružený k vašemu účtu Azure Data Lake Store a vytvořte novou verzi tohoto klíče.
  
   V tomto okamžiku služba Azure Data Lake podporuje pouze obměnu klíče za novou verzi klíče; obměna za jiný klíč se nepodporuje.

   ![Nová verze](./media/data-lake-store-encryption/keynewversion.png)

4.    Přejděte do účtu úložiště Azure Data Lake a vyberte Šifrování.

    ![Nová verze](./media/data-lake-store-encryption/select-encryption.png)

5.    Zobrazí se poznámka oznamující, že je k dispozici nová verze klíče, a tlačítko k obměně klíče za tuto novou verzi. Kliknutím na Obměnit klíč aktualizujte klíč na novou verzi.

    ![Hotovo](./media/data-lake-store-encryption/rotatekey.png)

6. Tato operace by neměla trvat déle než 2 minuty a nemělo by dojít k žádnému výpadku v důsledku obměny klíče. Po dokončení operace se začne používat nová verze klíče.

