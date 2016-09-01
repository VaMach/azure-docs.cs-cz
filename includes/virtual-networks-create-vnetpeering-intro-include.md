Partnerský vztah virtuálních sítí je mechanismus spojující dvě virtuální sítě ve stejné oblasti prostřednictvím páteřní sítě Azure. Po navázání partnerského vztahu se tyto dvě virtuální sítě pro všechny účely připojení jeví jako jedna virtuální síť. Pokud je pro vás přehled partnerských vztahů virtuálních sítí novinkou, můžete si přečíst [Přehled partnerských vztahů virtuálních sítí](../articles/virtual-network/virtual-network-peering-overview.md).

Přehled partnerských vztahů virtuálních sítí je ve verzi public preview. Abyste ho mohli použít, je nutné se zaregistrovat pomocí příkazů níže:

    Register-AzureRmProviderFeature -FeatureName AllowVnetPeering -ProviderNamespace Microsoft.Network

    Register-AzureRmResourceProvider -ProviderNamespace Microsoft.Network
 


<!---HONumber=Aug16_HO4-->


