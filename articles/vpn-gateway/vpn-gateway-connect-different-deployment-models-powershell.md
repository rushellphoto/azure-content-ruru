<properties 
   pageTitle="Подключение классических виртуальных сетей к виртуальным сетям Resource Manager с помощью PowerShell | Microsoft Azure"
   description="Узнайте, как создать VPN-подключение между классическими виртуальными сетями и виртуальными сетями Resource Manager с помощью VPN-шлюза и PowerShell"
   services="vpn-gateway"
   documentationCenter="na"
   authors="cherylmc"
   manager="carmonm"
   editor=""
   tags="azure-service-management,azure-resource-manager"/>
<tags 
   ms.service="vpn-gateway"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="08/09/2016"
   ms.author="cherylmc" />

# Подключение виртуальных сетей из различных моделей развертывания с использованием PowerShell

> [AZURE.SELECTOR]
- [Портал](vpn-gateway-connect-different-deployment-models-portal.md)
- [PowerShell](vpn-gateway-connect-different-deployment-models-powershell.md)

В настоящее время в Azure есть два режима управления: классический и Resource Manager (RM). Если вы использовали Azure какое-то время, вероятно, у вас есть виртуальные машины и роли экземпляров Azure, выполняемые в классической виртуальной сети. А более новые виртуальные машины и экземпляры ролей могут выполняться в виртуальной сети, созданной Resource Manager. В этой статье описываются этапы настройки подключения между классическими виртуальными сетями и виртуальными сетями Resource Manager, чтобы ресурсы, размещенные в разных моделях развертывания, могли взаимодействовать между собой через шлюз.

Подключение можно создать между виртуальными сетями, которые находятся в разных подписках, расположены в разных регионах и даже используются в разных режимах развертывания. Вы также можете создать подключение между виртуальными сетями, которые уже подключены к локальным сетям. В этом случае для них должен быть настроен динамический шлюз или шлюз на основе маршрутов. Дополнительные сведения о подключениях типа "виртуальная сеть — виртуальная сеть" см. в разделе [Часто задаваемые вопросы о подключениях типа "виртуальная сеть — виртуальная сеть"](#faq) в конце этой статьи. [AZURE.INCLUDE [vpn-gateway-vnetpeeringlink](../../includes/vpn-gateway-vnetpeeringlink-include.md)]

## Подготовка

Далее приведены инструкции, которые помогут вам настроить динамический шлюз или шлюз на основе маршрутов для каждой виртуальной сети и создать VPN-подключение между шлюзами. Эта конфигурация не поддерживает статические шлюзы и шлюзы на основе политик.

Прежде чем начать, проверьте, выполнены ли следующие условия.

 - Обе виртуальные сети уже созданы.
 - Диапазоны адресов для виртуальных сетей не перекрываются между собой или с другими диапазонами подключений, к которым могут быть подключены шлюзы.
 - В системе установлены последние командлеты Azure PowerShell (версии 1.0.2 или более поздней). Дополнительные сведения см. в статье [Установка и настройка Azure PowerShell](../powershell-install-configure.md). Командлеты управления службой и Resource Manager должны быть установлены.

### <a name="exampleref"></a>Примеры настроек

При выполнении командлетов PowerShell на следующих шагах можно воспользоваться приведенными ниже примерами настроек.

**Параметры классической виртуальной сети**

Имя виртуальной сети: ClassicVNet. <br> Расположение: западная часть США. <br> Адресное пространство виртуальной сети: 10.0.0.0/24. <br> Подсеть-1: 10.0.0.0/27. <br> Подсеть шлюза: 10.0.0.32/29. <br> Имя локальной сети: RMVNetLocal. <br> Тип шлюза: DynamicRouting.

**Параметры виртуальной сети Resource Manager**

Имя виртуальной сети: RMVNet. <br> Группа ресурсов: RG1. <br> Пространства IP-адресов виртуальной сети: 192.168.1.0/16. <br> Подсеть-1: 192.168.1.0/24. <br> Подсеть шлюза: 192.168.0.0/26. <br> Расположение: восточная часть США. <br> Имя общедоступного IP-адреса шлюза: gwpip.<br> Локальный сетевой шлюз: ClassicVNetLocal. <br> Имя шлюза виртуальной сети: RMGateway. <br> Конфигурация IP-адресации шлюза: gwipconfig.


## <a name="createsmgw"></a>Раздел 1. Настройка классической виртуальной сети

### Часть 1. Скачивание файла конфигурации сети

1. В консоли PowerShell войдите в свою учетную запись Azure. Командлет запрашивает учетные данные входа для вашей учетной записи Azure. После выполнения входа он скачивает параметры учетной записи, чтобы они были доступны в Azure PowerShell. Для выполнения этой части конфигурации будут использоваться командлеты PowerShell управления службой.

		Add-AzureAccount

2. Экспортируйте файл конфигурации сети Azure, выполнив приведенную ниже команду. При необходимости можно изменить расположение файла для экспорта. Затем нужно отредактировать этот файл и импортировать его в Azure.

		Get-AzureVNetConfig -ExportToFile c:\AzureNet\NetworkConfig.xml

3. Откройте скачанный XML-файл, чтобы внести изменения. Пример файла конфигурации сети см. в [схеме конфигурации сети](https://msdn.microsoft.com/library/jj157100.aspx).
 

### Часть 2. Проверка подсети шлюза

В элементе **VirtualNetworkSites** добавьте подсеть шлюза в виртуальную сеть, если она еще не создана. При работе с файлом конфигурации сети для подсети шлюза необходимо задать имя GatewaySubnet. В противном случае Azure не удастся распознать и использовать ее в качестве подсети шлюза.
	
    <VirtualNetworkSites>
      <VirtualNetworkSite name="ClassicVNet" Location="West US">
        <AddressSpace>
          <AddressPrefix>10.0.0.0/24</AddressPrefix>
        </AddressSpace>
        <Subnets>
          <Subnet name="Subnet-1">
            <AddressPrefix>10.0.0.0/27</AddressPrefix>
          </Subnet>
          <Subnet name="GatewaySubnet">
            <AddressPrefix>10.0.0.32/29</AddressPrefix>
          </Subnet>
        </Subnets>
      </VirtualNetworkSite>
    </VirtualNetworkSites>
       
### Часть 3. Добавление сайта локальной сети

Добавляемый сайт локальной сети представляет собой виртуальную сеть RM, к которой необходимо подключиться. Возможно, в файл потребуется добавить элемент **LocalNetworkSites**, если он еще не создан. На этом этапе настройки значение VPNGatewayAddress может быть любым допустимым общедоступным IP-адресом, так как шлюз для виртуальной сети RM еще не создан. После создания шлюза этот заполнитель необходимо заменить правильным общедоступным IP-адресом, назначенным шлюзу RM.

    <LocalNetworkSites>
      <LocalNetworkSite name="RMVNetLocal">
        <AddressSpace>
          <AddressPrefix>192.168.1.0/16</AddressPrefix>
        </AddressSpace>
        <VPNGatewayAddress>13.68.210.16</VPNGatewayAddress>
      </LocalNetworkSite>
    </LocalNetworkSites>

### Часть 4. Связывание виртуальной сети с сайтом локальной сети

В этом разделе необходимо указать сайт локальной сети, который требуется подключить к виртуальной сети. В нашем случае это виртуальная сеть RM, указанная ранее. Убедитесь, что имена совпадают. На этом этапе шлюз еще не создается. Пока только указывается локальная сеть, к которой будет подключен шлюз.

		<Gateway>
          <ConnectionsToLocalNetwork>
            <LocalNetworkSiteRef name="RMVNetLocal">
              <Connection type="IPsec" />
            </LocalNetworkSiteRef>
          </ConnectionsToLocalNetwork>
        </Gateway>

### Часть 5. Сохранение и передача файла

Сохраните файл, а затем импортируйте его в Azure, выполнив приведенную ниже команду. Обязательно измените путь к файлу в соответствии с вашей средой.

		Set-AzureVNetConfig -ConfigurationPath C:\AzureNet\ClassicVNet.xml

Вы увидите примерно такие выходные данные, указывающие, что импорт выполнен успешно:

		OperationDescription        OperationId                      OperationStatus                                                
		--------------------        -----------                      ---------------                                                
		Set-AzureVNetConfig        e0ee6e66-9167-cfa7-a746-7casb9    Succeeded 

### Часть 6. Создание шлюза виртуальной сети 

Шлюз виртуальной сети можно создать на классическом портале или с помощью PowerShell. Сейчас создать классический шлюз на портале Azure невозможно.

#### Создание шлюза на классическом портале

1. На [классическом портале](https://manage.windowsazure.com) перейдите к элементу **Сети** и щелкните классическую виртуальную сеть, для которой необходимо создать шлюз виртуальной сети. Откроется главная страница для виртуальной сети.
2. В верхней части страницы щелкните **Панель мониторинга**, чтобы перейти к странице "Панель мониторинга".
3. В нижней части страницы "Панель мониторинга" щелкните **Создать шлюз**, а затем — **Динамическая маршрутизация**.
4. Нажмите кнопку **Да**, чтобы начать создание шлюза.
5. Подождите, пока шлюз не будет создан. Этот процесс может занять 45 минут или больше.
6. После создания шлюза его IP-адрес можно просмотреть на странице **Панель мониторинга**. Это общедоступный IP-адрес вашего шлюза. Запишите или скопируйте это значение, так как оно понадобится позже при создании локальной сети для настройки виртуальной сети Resource Manager.
7. Чтобы получить параметры шлюза и проверить его состояние, можно использовать командлет `Get-AzureVirtualNetworkGateway`.

#### Создание шлюза с помощью PowerShell

Чтобы создать шлюз с помощью PowerShell, используйте приведенный ниже пример.

	New-AzureVNetGateway -VNetName ClassicVNet -GatewayType DynamicRouting


## <a name="creatermgw"></a>Раздел 2. Настройка шлюза виртуальной сети RM

Чтобы создать VPN-шлюз для виртуальной сети RM, следуйте приведенным ниже инструкциям. Не приступайте к выполнению следующих действий до получения общедоступного IP-адреса шлюза классической виртуальной сети.

1. В консоли PowerShell **войдите в свою учетную запись Azure**. Следующий командлет запрашивает учетные данные входа для вашей учетной записи Azure. После выполнения входа он скачивает параметры учетной записи, чтобы они были доступны в Azure PowerShell.

		Login-AzureRmAccount 

 	Если у вас есть несколько подписок, запросите их список.

		Get-AzureRmSubscription

	Укажите подписку, которую нужно использовать.

		Select-AzureRmSubscription -SubscriptionName "Name of subscription"


2.  **Создайте шлюз локальной сети**. В виртуальной сети термин "шлюз локальной сети" обычно означает локальное расположение. В этом случае термин "шлюз локальной сети" указывает на классическую виртуальную сеть. Присвойте этому расположению имя, с помощью которого служба Azure сможет ссылаться на него, а также укажите префикс адресного пространства. С помощью указанного префикса IP-адреса служба Azure может определить, какой трафик отправлять в локальное расположение. Если позже понадобится изменить эти сведения, перед созданием шлюза можно изменить значения и снова запустить пример.<br><br>
	- `-Name` — это имя, которое будет использоваться, чтобы ссылаться на шлюз локальной сети.<br>
	- `-AddressPrefix` — это адресное пространство для классической виртуальной сети.<br>
	- `-GatewayIpAddress` — это общедоступный IP-адрес шлюза классической виртуальной сети. Укажите в следующем примере правильный IP-адрес.
	
			New-AzureRmLocalNetworkGateway -Name ClassicVNetLocal `
			-Location "West US" -AddressPrefix "10.0.0.0/24" `
			-GatewayIpAddress "n.n.n.n" -ResourceGroupName RG1

3. **Запросите общедоступный IP-адрес**, который нужно выделить для VPN-шлюза виртуальной сети Azure Resource Manager. Указать необходимый IP-адрес нельзя. Он выделяется для шлюза динамически. Однако это не значит, что IP-адрес изменится. IP-адрес VPN-шлюза Azure изменяется только после его удаления и повторного создания. При изменении размера, выполнении сброса или других внутренних операций обслуживания или обновления VPN-шлюз Azure не изменяется.<br>На этом этапе также задается переменная, которая будет использоваться позже.


		$ipaddress = New-AzureRmPublicIpAddress -Name gwpip `
		-ResourceGroupName RG1 -Location 'EastUS' `
		-AllocationMethod Dynamic

4. **Проверьте наличие подсети шлюза в виртуальной сети**. Если подсети шлюза нет, добавьте ее. Присвойте подсети шлюза имя *GatewaySubnet*.

5. **Получите подсеть**, используемую для шлюза, выполнив следующую команду. На этом этапе мы также зададим переменную, которая будет использоваться на следующем этапе.
	- `-Name` — это имя виртуальной сети Resource Manager.
	- `-ResourceGroupName` — это группа ресурсов, с которой связана виртуальная сеть. Подсеть шлюза для этой виртуальной сети уже должна существовать и иметь имя *GatewaySubnet*.


			$subnet = Get-AzureRmVirtualNetworkSubnetConfig -Name GatewaySubnet `
			-VirtualNetwork (Get-AzureRmVirtualNetwork -Name RMVNet -ResourceGroupName RG1) 

6. **Создайте конфигурацию IP-адресации шлюза**. Конфигурация шлюза определяет используемые подсеть и общедоступный IP-адрес. Используйте приведенный ниже пример, чтобы создать конфигурацию шлюза.<br>На этом этапе в параметры `-SubnetId` и `-PublicIpAddressId` необходимо передать свойство идентификатора подсети и IP-адреса соответственно. Нельзя использовать простую строку. Эти переменные задаются на этапе запроса общедоступного IP-адреса и этапе получения подсети.

		$gwipconfig = New-AzureRmVirtualNetworkGatewayIpConfig `
		-Name gwipconfig -SubnetId $subnet.id `
		-PublicIpAddressId $ipaddress.id
	
7. **Создайте шлюз виртуальной сети Resource Manager**, выполнив приведенную ниже команду. `-VpnType` должен иметь значение *RouteBased*.

		New-AzureRmVirtualNetworkGateway -Name RMGateway -ResourceGroupName RG1 `
		-Location "EastUS" -GatewaySKU Standard -GatewayType Vpn `
		-IpConfigurations $gwipconfig `
		-EnableBgp $false -VpnType RouteBased

8. После создания VPN-шлюза **скопируйте общедоступный IP-адрес**. Он понадобится при настройке параметров локальной сети для классической виртуальной сети. Для получения общедоступного IP-адреса можно использовать следующий командлет. В возврате должен быть указан общедоступный IP-адрес *IpAddress*.

		Get-AzureRmPublicIpAddress -Name gwpip -ResourceGroupName RG1

## Раздел 3. Изменение параметров локальной сети для классической виртуальной сети

Чтобы выполнить этот шаг, нужно экспортировать, редактировать и сохранить файл конфигурации, а затем импортировать его обратно в Azure. Этот параметр можно также изменить на классическом портале.

### Изменение на портале

1. Откройте [классический портал](https://manage.windowsazure.com).

2. На классическом портале прокрутите вниз и в левой части выберите **Сети**. В верхней части страницы **Сети** щелкните **Локальные сети**.

3. На странице **Локальные сети** выберите локальную сеть, настроенную в части 1, перейдите к нижней части страницы, а затем щелкните **Изменить**.

4. На странице **Укажите сведения о локальной сети** замените заполнитель общедоступным IP-адресом шлюза Resource Manager, созданным в предыдущем разделе. Нажмите кнопку со стрелкой, чтобы перейти к следующему разделу. Убедитесь, что для параметра **Адресное пространство** задано правильное значение, а затем щелкните значок флажка, чтобы применить изменения.

### Изменение с использованием файла конфигурации сети

1. Экспортируйте файл конфигурации сети.
2. В текстовом редакторе измените IP-адрес VPN-шлюза.

	    <VPNGatewayAddress>13.68.210.16</VPNGatewayAddress>

3. Сохраните изменения и импортируйте измененный файл обратно в Azure.


## <a name="connect"></a>Раздел 4. Создание подключения между шлюзами

Для создания подключения между шлюзами требуется PowerShell. Возможно, потребуется добавить учетную запись Azure, чтобы использовать классические командлеты PowerShell. Используйте для этого следующий командлет.
		
	Add-AzureAccount

1. **Установите общий ключ**, выполнив приведенную ниже команду. В этом примере `-VNetName` — это имя классической виртуальной сети, а `-LocalNetworkSiteName` — имя локальной сети, определенное на классическом портале. `-SharedKey` — это значение, которое можно создать и задать. Заданное здесь значение должно совпадать со значением, которое вы укажите на следующем шаге при создании подключения. В результате выполнения этого примера должно отобразиться состояние "Успешно".

		Set-AzureVNetGatewayKey -VNetName ClassicVNet `
		-LocalNetworkSiteName RMVNetLocal -SharedKey abc123

2. Создайте VPN-подключение, выполнив следующие команды.

	**Задание переменных**

		$vnet01gateway = Get-AzureRMLocalNetworkGateway -Name ClassicVNetLocal -ResourceGroupName RG1
		$vnet02gateway = Get-AzureRmVirtualNetworkGateway -Name RMGateway -ResourceGroupName RG1

	**Создание подключения**<br> Обратите внимание, что для параметра `-ConnectionType` используется значение IPsec, а не Vnet2Vnet.
		
		New-AzureRmVirtualNetworkGatewayConnection -Name RM-Classic -ResourceGroupName RG1 `
		-Location "East US" -VirtualNetworkGateway1 `
		$vnet02gateway -LocalNetworkGateway2 `
		$vnet01gateway -ConnectionType IPsec -RoutingWeight 10 -SharedKey 'abc123'

3. Подключение можно просмотреть на портале или с помощью командлета `Get-AzureRmVirtualNetworkGatewayConnection`.

		Get-AzureRmVirtualNetworkGatewayConnection -Name RM-Classic -ResourceGroupName RG1

## <a name="faq"></a>Часто задаваемые вопросы о подключениях типа "виртуальная сеть — виртуальная сеть"

Дополнительные сведения см. в ответах на часто задаваемые вопросы о подключениях типа "виртуальная сеть — виртуальная сеть".

[AZURE.INCLUDE [vpn-gateway-vnet-vnet-faq](../../includes/vpn-gateway-vnet-vnet-faq-include.md)]

<!---HONumber=AcomDC_0810_2016-->