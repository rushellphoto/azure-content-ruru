Пиринг виртуальных сетей — это механизм подключения между двумя виртуальными сетями, находящимися в одном регионе, через магистральную сеть Azure. После создания пиринга две виртуальные сети будут выглядеть как одна виртуальная сеть при любом подключении. Если вы не знакомы с пирингом виртуальных сетей, см. [общие сведения об этой функции](../articles/virtual-network/virtual-network-peering-overview.md).

Пиринг виртуальных сетей работает в режиме общедоступной предварительной версии. Для его использования необходимо зарегистрироваться с помощью таких команд:

    Register-AzureRmProviderFeature -FeatureName AllowVnetPeering -ProviderNamespace Microsoft.Network

    Register-AzureRmResourceProvider -ProviderNamespace Microsoft.Network
 

<!---HONumber=AcomDC_0810_2016-->