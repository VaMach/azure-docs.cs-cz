## <a name="supported-distributions-and-drivers"></a>Podporované distribuce a ovladače


### <a name="nc-ncv2-and-nd-instances---nvidia-cuda-drivers"></a>NC, NCv2 a ND instancí - NVIDIA CUDA ovladače
| Distribuce | Ovladač |
| --- | --- | 
| Ubuntu 16.04 LTS<br/><br/> Red Hat Enterprise Linux 7.3 nebo 7.4<br/><br/> CentOS 7.3 nebo 7.4 | NVIDIA CUDA 9.1, ovladač větve R390 |

> [!IMPORTANT]
> Ujistěte se, že instalaci nebo upgrade na nejnovější ovladače CUDA pro distribuční. Ovladače, které jsou starší než verze R390 mohou mít problémy s aktualizované jádra systému Linux.
>

### <a name="nv-instances---nvidia-grid-drivers"></a>Instance NV – ovladače NVIDIA GRID


| Distribuce | Ovladač |
| --- | --- | 
| Ubuntu 16.04 LTS<br/><br/>Red Hat Enterprise Linux 7.3<br/><br/>Distribuce založené na CentOS 7.3 | 5.2 NVIDIA mřížky, ovladač větve R384|

> [!NOTE]
> Microsoft znovu distribuuje NVIDIA mřížky ovladač instalačních programů pro virtuální počítače vs. Nainstalujte pouze tyto ovladače mřížky na virtuálních počítačích Azure vs. Tyto ovladače zahrnují licencování pro mřížky virtuální grafický procesor Software v Azure.
>

> [!WARNING] 
> Instalace softwaru třetích stran na produkty Red Hat může mít vliv na podmínky podpory Red Hat. Informace najdete v [článku znalostní báze Red Hat](https://access.redhat.com/articles/1067).
>
