Pokud budete mít adresu URL sdílený přístupový podpis (SAS), který uděluje přístup k prostředkům v účtu úložiště, můžete použít SAS v připojovacím řetězci. Protože SAS obsahuje informace potřebné k ověření požadavku, připojovací řetězec s SAS poskytuje protokol, koncový bod služby a potřebné pověření pro přístup k prostředku.

Pokud chcete vytvořit připojovací řetězec, který zahrnuje sdílený přístupový podpis, zadejte řetězec ve formátu:

```
BlobEndpoint=myBlobEndpoint;
QueueEndpoint=myQueueEndpoint;
TableEndpoint=myTableEndpoint;
FileEndpoint=myFileEndpoint;
SharedAccessSignature=sasToken
```

Každý koncový bod služby je volitelný, i když připojovací řetězec musí obsahovat alespoň jeden.

> [!NOTE]
> Jako osvědčený postup se doporučuje HTTPS pomocí SAS.
>
> Pokud zadáte SAS v připojovacím řetězci v konfiguračním souboru, můžete kódovat speciální znaky v adrese URL.
>
>

### <a name="service-sas-example"></a>Příklad SAS služby
Tady je příklad připojovacího řetězce, který obsahuje službu SAS pro úložiště objektů Blob:

```
BlobEndpoint=https://storagesample.blob.core.windows.net;
SharedAccessSignature=sv=2015-04-05&sr=b&si=tutorial-policy-635959936145100803&sig=9aCzs76n0E7y5BpEi2GvsSv433BZa22leDOZXX%2BXXIU%3D
```

A tady je příklad stejný připojovací řetězec s kódováním speciálních znaků:

```
BlobEndpoint=https://storagesample.blob.core.windows.net;
SharedAccessSignature=sv=2015-04-05&amp;sr=b&amp;si=tutorial-policy-635959936145100803&amp;sig=9aCzs76n0E7y5BpEi2GvsSv433BZa22leDOZXX%2BXXIU%3D
```

### <a name="account-sas-example"></a>Příklad SAS účtu
Tady je příklad připojovacího řetězce, který zahrnuje SAS účtu pro úložiště Blob a souboru. Všimněte si, že jsou zadané koncové body pro obě služby:

```
BlobEndpoint=https://storagesample.blob.core.windows.net;
FileEndpoint=https://storagesample.file.core.windows.net;
SharedAccessSignature=sv=2015-07-08&sig=iCvQmdZngZNW%2F4vw43j6%2BVz6fndHF5LI639QJba4r8o%3D&spr=https&st=2016-04-12T03%3A24%3A31Z&se=2016-04-13T03%3A29%3A31Z&srt=s&ss=bf&sp=rwl
```

A tady je příklad stejný připojovací řetězec s kódováním adresy URL:

```
BlobEndpoint=https://storagesample.blob.core.windows.net;
FileEndpoint=https://storagesample.file.core.windows.net;
SharedAccessSignature=sv=2015-07-08&amp;sig=iCvQmdZngZNW%2F4vw43j6%2BVz6fndHF5LI639QJba4r8o%3D&amp;spr=https&amp;st=2016-04-12T03%3A24%3A31Z&amp;se=2016-04-13T03%3A29%3A31Z&amp;srt=s&amp;ss=bf&amp;sp=rwl
```

