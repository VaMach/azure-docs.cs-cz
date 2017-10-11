## <a name="configure-your-application-to-access-azure-storage"></a>Konfigurace aplikace pro přístup k úložišti Azure
Existují dva způsoby, jak aplikaci pro přístup ke službám úložiště ověřovat:

* Sdílený klíč: Použít sdílený klíč jenom pro účely testování
* Sdílený přístupový podpis (SAS): Použití SAS pro výrobní aplikace

### <a name="shared-key"></a>Sdílený klíč
Ověření pomocí sdíleného klíče znamená, že vaše aplikace bude používat název účtu a klíč účtu pro přístup ke službám úložiště. Pro účely rychle znázorňující způsob použití této knihovny můžeme použít ověření sdíleným klíčem v této příručce Začínáme.

> [!WARNING] 
> **Používejte ověření sdíleným klíčem pouze pro účely testování!** Název účtu a klíč účtu, což dává přístup pro čtení/zápisu pro přidružený účet úložiště, budou distribuována do všech osob, který stahuje vaší aplikace. Toto je **není** dobrou praxi, protože riskujete, že má váš klíč nekompromitovali nedůvěryhodní klienti.
> 
> 

Pokud používáte ověření sdíleným klíčem, vytvoříte [připojovací řetězec](../articles/storage/common/storage-configure-connection-string.md). Připojovací řetězec se skládá z:  

* **DefaultEndpointsProtocol** – můžete protokolu HTTP nebo HTTPS. Však pomocí protokolu HTTPS důrazně doporučujeme.
* **Název účtu** – název účtu úložiště
* **Klíč účtu** – na [portálu Azure](https://portal.azure.com), přejděte na svůj účet úložiště a klikněte na tlačítko **klíče** ikonu najít tyto informace.
* (Volitelné) **EndpointSuffix** – používá se pro služby úložiště v oblasti s přípon jinému koncovému bodu, například Azure China nebo Azure zásad správného řízení.

Tady je příklad připojovacího řetězce, pomocí ověření sdíleným klíčem:

`"DefaultEndpointsProtocol=https;AccountName=your_account_name_here;AccountKey=your_account_key_here"`

### <a name="shared-access-signatures-sas"></a>Sdílené přístupové podpisy (SAS)
Mobilní aplikace je doporučené metody pro ověřování na požadavek klienta pro službu Azure Storage pomocí sdíleného přístupového podpisu (SAS). SAS můžete udělit klientský přístup k prostředku pro zadaný časový úsek, se zadanou sadou oprávnění.
Jako vlastník účtu úložiště budete potřebovat pro mobilní klienty využívat SAS můžete vygenerovat. Pokud chcete vygenerovat SAS, budete pravděpodobně chtít zápisu samostatnou službu, která vygeneruje SAS distribuována do klientů. Pro účely testování můžete použít [Microsoft Azure Storage Explorer](http://storageexplorer.com) nebo [portálu Azure](https://portal.azure.com) SAS můžete vygenerovat. Když vytvoříte SAS, můžete zadat časový interval, za které je platný SAS a oprávnění, která uděluje SAS klientovi.

Následující příklad ukazuje, jak používat Microsoft Azure Storage Explorer SAS můžete vygenerovat.

1. Pokud jste to ještě neudělali, [nainstalovat Microsoft Azure Storage Explorer](http://storageexplorer.com)
2. Připojte se ke svému předplatnému.
3. Klikněte na vašem účtu úložiště a klikněte na kartu "Akce" v dolní části vlevo. Klikněte na tlačítko "Získání sdíleného přístupového podpisu" generovat "připojovací řetězec" pro vaše SAS.
4. Tady je příklad připojovacího řetězce SAS, že uděluje oprávnění čtení a zápis na služby, kontejneru a na úrovni objektu služby objektů blob účtu úložiště.
   
   `"SharedAccessSignature=sv=2015-04-05&ss=b&srt=sco&sp=rw&se=2016-07-21T18%3A00%3A00Z&sig=3ABdLOJZosCp0o491T%2BqZGKIhafF1nlM3MzESDDD3Gg%3D;BlobEndpoint=https://youraccount.blob.core.windows.net"`

Jak můžete vidět, když pomocí SAS, nejsou vystavení klíč účtu v aplikaci. Další informace o přidružení zabezpečení a doporučené postupy pro používání SAS vyzkoušejte [sdílené přístupové podpisy: vysvětlení modelu SAS](../articles/storage/common/storage-dotnet-shared-access-signature-part-1.md).

