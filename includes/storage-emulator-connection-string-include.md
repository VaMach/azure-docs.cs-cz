Emulátor úložiště podporuje jeden pevný účet a dobře známé ověřovací klíč pro ověření sdíleným klíčem. Tento účet a klíč jsou pouze povolené pro použití s emulátor úložiště pověření sdílený klíč. Jsou:

```
Account name: devstoreaccount1
Account key: Eby8vdM02xNOcqFlqUwJPLlmEtlCDXJ1OUzFT50uSRZ6IFsuFq2UVErCz4I6tq/K1SZFPTOtr/KBHBeksoGMGw==
```

> [!NOTE]
> Ověřovací klíč nepodporuje emulátor úložiště je určena pouze pro testování funkce kód pro ověřování klienta. Neslouží jakýkoli účel zabezpečení. Produkční účtu úložiště a klíč nelze použít s emulátor úložiště. Vývoj pro účet byste neměli používat s provozními daty.
> 
> Emulátor úložiště podporuje pouze připojení prostřednictvím protokolu HTTP. Ale HTTPS je protokol doporučené pro přístup k prostředkům v produkční účtu úložiště Azure.
> 

#### <a name="connect-to-the-emulator-account-using-a-shortcut"></a>Připojení k účtu emulátoru pomocí zástupce
Nejjednodušší způsob, jak připojit k emulátor úložiště z vaší aplikace je nakonfigurovat připojovací řetězec v konfiguračním souboru aplikace, který odkazuje na zástupce `UseDevelopmentStorage=true`. Tady je příklad připojovacího řetězce pro emulátor úložiště v *app.config* souboru: 

```xml
<appSettings>
  <add key="StorageConnectionString" value="UseDevelopmentStorage=true" />
</appSettings>
```

#### <a name="connect-to-the-emulator-account-using-the-well-known-account-name-and-key"></a>Připojení k účtu emulátor používá známý účet název a klíč
Chcete-li vytvořit připojovací řetězec, který odkazuje na emulátoru název účtu a klíč, musíte zadat koncové body pro jednotlivé služby, které chcete použít z emulátoru serveru v připojovacím řetězci. To je nezbytné, aby připojovací řetězec bude odkazovat emulátoru koncových bodů, které se liší od zásad pro účet úložiště produkční. Například hodnota připojovací řetězec bude vypadat takto:

```
DefaultEndpointsProtocol=http;AccountName=devstoreaccount1;
AccountKey=Eby8vdM02xNOcqFlqUwJPLlmEtlCDXJ1OUzFT50uSRZ6IFsuFq2UVErCz4I6tq/K1SZFPTOtr/KBHBeksoGMGw==;
BlobEndpoint=http://127.0.0.1:10000/devstoreaccount1;
TableEndpoint=http://127.0.0.1:10002/devstoreaccount1;
QueueEndpoint=http://127.0.0.1:10001/devstoreaccount1;
```

Tato hodnota je stejná jako zástupce uvedené výše, `UseDevelopmentStorage=true`.

#### <a name="specify-an-http-proxy"></a>Zadejte proxy serveru HTTP
Můžete také zadat proxy server HTTP použít při testování služby emulátoru úložiště. To může být užitečné pro sledování požadavků a odpovědí HTTP při ladění operace u služby storage. Chcete-li zadat proxy server, přidejte `DevelopmentStorageProxyUri` možnost připojovací řetězec a jeho hodnotu nastavte identifikátor URI proxy serveru. Zde je například připojovací řetězec, který odkazuje na emulátor úložiště a nakonfiguruje server proxy protokolu HTTP:

```
UseDevelopmentStorage=true;DevelopmentStorageProxyUri=http://myProxyUri
```

