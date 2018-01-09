 ---
Title: Import Postman prostředí pro Azure Media Services REST volá Popis: Toto téma obsahuje definici Postman prostředí pro Azure Media Services REST volání.
služby: documentationcenter služby media services:: Autor: Juliako správce: cfowler editor: "

MS.Service: ms.workload služby media services: média ms.tgt_pltfrm: na ms.devlang: na ms.topic: článek ms.date: ms.author 01/04/2017: juliako

---

# <a name="import-the-postman-environment"></a>Import prostředí Postman 

Tento článek obsahuje definice **Postman** proměnné prostředí, které se používají [Postman kolekce](postman-collection.md) obsahující seskupené požadavky HTTP, které volání rozhraní API REST Media Services. Prostředí a kolekce souborů jsou používány [Postman nakonfigurovat pro Media Services REST API volá](media-rest-apis-with-postman.md) kurzu.

```
{
  "id": "2dbce3ce-74c2-2ceb-0461-c4c2323f5b09",
  "name": "AzureMedia",
  "values": [
    {
      "enabled": true,
      "key": "TenantID",
      "value": "",
      "type": "text"
    },
    {
      "enabled": true,
      "key": "AzureADSTSEndpoint",
      "value": "",
      "type": "text"
    },
    {
      "enabled": true,
      "key": "RESTAPIEndpoint",
      "value": "",
      "type": "text"
    },
    {
      "enabled": true,
      "key": "ClientID",
      "value": "",
      "type": "text"
    },
    {
      "enabled": true,
      "key": "ClientSecret",
      "value": "",
      "type": "text"
    },
    {
      "enabled": true,
      "key": "AccessToken",
      "value": "",
      "type": "text"
    },
    {
      "enabled": true,
      "key": "LastAssetId",
      "value": "",
      "type": "text"
    },
    {
      "enabled": true,
      "key": "LastAccessPolicyId",
      "value": "",
      "type": "text"
    },
    {
      "enabled": true,
      "key": "UploadURL",
      "value": "",
      "type": "text"
    },
    {
      "enabled": true,
      "key": "MediaFileName",
      "value": "BigBuckBunny.mp4",
      "type": "text"
    },
    {
      "enabled": true,
      "key": "LastChannelId",
      "value": "",
      "type": "text"
    }
  ],
  "_postman_variable_scope": "environment",
  "_postman_exported_using": "Postman/5.5.0"
}
```
