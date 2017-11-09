| Prostředek | Basic | Standard | Premium |
|---|---|---|---|---|
| Úložiště | 10 giB | 100 giB| 500 giB |
| ReadOps za minutu<sup>1, 2</sup> | 1 kB | 300 kB | 10 000 kB |
| WriteOps za minutu<sup>1, 3</sup> | 100 | 500 | 2k |
| Stáhnout MB/s šířky pásma<sup>1</sup> | 30 | 60 | 100 |
| Nahrát MB/s šířky pásma<sup>1</sup> | 10 | 20 | 50 |
| Webhooky | 2 | 10 | 100 |
| Geografická replikace | Není k dispozici | Není k dispozici | [Podporované *(preview)*](https://docs.microsoft.com/azure/container-registry/container-registry-geo-replication) |

<sup>1</sup> *ReadOps*, *WriteOps*, a *šířky pásma* jsou odhady minimální. ACR se snaží zlepšit výkon, protože vyžaduje použití.

<sup>2</sup> [docker vyžádání](https://docs.docker.com/registry/spec/api/#pulling-an-image) překládá do více operací čtení na základě počtu vrstev v bitovou kopii a načtení manifestu.

<sup>3</sup> [docker nabízené](https://docs.docker.com/registry/spec/api/#pushing-an-image) překládá do více operací zápisu na základě počtu vrstvy, které musí být posunuta. A `docker push` zahrnuje *ReadOps* k načtení manifestu pro stávající image.