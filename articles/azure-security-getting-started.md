<properties
   pageTitle="Приступая к работе с безопасностью Microsoft Azure | Microsoft Azure"
   description="В этой статье приведен обзор возможностей безопасности Microsoft Azure и общие рекомендации для организаций, которые переносят ресурсы в поставщик облака."
   services="security"
   documentationCenter="na"
   authors="YuriDio"
   manager="swadhwa"
   editor="TomSh"/>

<tags
   ms.service="security"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="05/23/2016"
   ms.author="yuridio"/>

#Приступая к работе с безопасностью Microsoft Azure

При создании ИТ-ресурсов или их переносе в поставщик облачных служб вы полагаетесь на возможности этой организации защитить приложения и данные, доверенные службам, и средства управления безопасностью, предоставляемые для управления безопасностью облачных ресурсов.

Инфраструктура устройств и приложений Azure предназначена для размещения миллионов пользователей одновременно и обеспечения надежной основы, которая удовлетворяет требованиям организаций к защите. Кроме того, Azure предоставляет широкий набор настраиваемых решений безопасности, а также возможность управления ими. Все это позволит настроить безопасность в соответствии с уникальными требованиями для развертываний.

В этой статье, содержащей общие сведения о безопасности Azure, мы рассмотрим такие темы:

-   службы и функции Azure, которые позволяют защитить службы и данные в Azure;

-   как корпорация Майкрософт защищает инфраструктуру Azure для обеспечения защиты данных и приложений.

##Управление удостоверениями и доступом


Очень важно управлять доступом к ИТ-инфраструктуре, данным и приложениям. В Microsoft Azure эти возможности реализованы в службе Azure Active Directory, службе хранилища Azure, а также за счет поддержки различных стандартов и интерфейсов API.

[Azure Active Directory](./active-directory/active-directory-whatis.md) (Azure AD) — это репозиторий удостоверений и обработчик, который обеспечивает проверку подлинности, авторизацию и управление доступом для корпоративных пользователей, групп и объектов. Кроме того, Azure AD помогает разработчикам эффективно интегрировать функцию управления удостоверений в свои приложения. Благодаря использованию стандартных протоколов, таких как [SAML 2.0](https://en.wikipedia.org/wiki/SAML_2.0), [WS-Federation](https://msdn.microsoft.com/library/bb498017.aspx) и [OpenID Connect](http://openid.net/connect/), можно выполнить вход на различных платформах, например .NET, Java, Node.js и PHP.

API Graph на основе REST позволяет разработчикам выполнять чтение и запись в каталоге на любой платформе. Благодаря поддержке протокола [OAuth 2.0](http://oauth.net/2/) разработчики могут создавать мобильные и веб-приложения, которые интегрируются с веб-интерфейсами API корпорации Майкрософт и сторонних поставщиков, и создавать собственные безопасные веб-интерфейсы API. Для .NET, Windows Store, iOS и Android доступны клиентские библиотеки с открытым исходным кодом. Кроме того, доступны дополнительные библиотеки, находящиеся на стадии разработки.

### Как Azure обеспечивает управление удостоверениями и доступом

Azure AD можно использовать в качестве изолированного облачного каталога для организации или решения, интегрированного с существующей локальной средой Active Directory. Некоторые функции интеграции включают в себя синхронизацию каталогов и единый вход (SSO). Они расширяют возможности доступа существующих локальных идентификаторов к облаку и улучшают работу администраторов и пользователей.

Ниже представлены другие возможности управления удостоверениями и доступом.

-   Azure AD позволяет воспользоваться [единым входом](https://azure.microsoft.com/documentation/videos/overview-of-single-sign-on/) в приложениях SaaS независимо от того, где они размещены. Некоторые приложения служба Azure AD объединяет в федерацию, а другие используют единый вход с помощью пароля. Федеративные приложения могут, кроме того, поддерживать подготовку пользователя и хранение пароля в хранилище.

-   Доступ к данным в [службе хранилища Azure](https://azure.microsoft.com/services/storage/) осуществляется при условии проверки подлинности. Для каждой учетной записи хранения есть первичный ключ ([ключ учетной записи хранения](https://msdn.microsoft.com/library/azure/ee460785.aspx), или SAK) и вторичный секретный ключ (подписанный URL-адрес, или SAS).

-   Azure AD предоставляет удостоверение как услугу путем создания федерации (с помощью [служб федерации Active Directory](./active-directory/fundamentals-identity.md)), синхронизации и репликации локальных каталогов.

-   [Azure Multi-Factor Authentication (MFA)](./multi-factor-authentication/multi-factor-authentication.md) — это служба многофакторной проверки подлинности, которая требует от пользователей подтверждать вход с помощью мобильного приложения, телефонного звонка или текстового сообщения. Ее можно использовать с Azure AD, для защиты локальных ресурсов с помощью сервера Azure MFA, а также с пользовательскими приложениями и каталогами с помощью пакета SDK.

-   [Доменные службы Azure AD](https://azure.microsoft.com/services/active-directory-ds/) позволяют присоединить виртуальные машины к домену без необходимости развертывать контроллеры домена. Пользователи могут войти в эти виртуальные машины, используя корпоративные учетные данные Active Directory, и выполнять администрирование виртуальных машин, присоединенных к домену, с помощью групповой политики, чтобы применить базовые средства защиты на всех виртуальных машинах Azure.

-   [Azure Active Directory B2C](https://azure.microsoft.com/services/active-directory-b2c/) представляет собой глобальную службу управления удостоверениями для клиентских приложений, которая обеспечивает высокую доступность и масштабируется до сотен миллионов удостоверений. Служба интегрируется с мобильными и веб-платформами. Клиенты могут входить в ваши приложения через настраиваемый интерфейс с помощью существующих учетных записей социальных сетей или путем создания новой учетной записи.

##Управление доступом к данным и шифрование данных

В операциях Azure корпорация Майкрософт придерживается принципов разделения обязанностей и [предоставления минимальных прав](https://en.wikipedia.org/wiki/Principle_of_least_privilege). Для доступа к данным Azure персоналу технической поддержки требуется явное разрешение. Такой доступ предоставляется по принципу своевременности, т. е. специалист получает доступ к данным, проводит аудит, а после выполнения задачи доступ отзывается.

Кроме того, Azure предоставляет несколько возможностей для защиты передаваемых и хранящихся данных, включая шифрование данных, файлов, приложений, служб, сообщений и дисков. Перед тем как разместить данные в Azure, их можно зашифровать. Кроме того, можно хранить ключи в локальных центрах обработки данных.

![Антивредоносное ПО Майкрософт в Azure](./media/azure-security-getting-started\sec-azgsfig1.PNG)

### Технологии шифрования Azure

С помощью службы [Azure AD Reporting](./active-directory/active-directory-reporting-audit-events.md) можно собирать информацию об административном доступе к среде подписки. Можно настроить [шифрование дисков BitLocker](https://technet.microsoft.com/library/cc732774.aspx) для дисков VHD, содержащих конфиденциальные данные в Azure.

Ниже представлены другие возможности в Azure, которые обеспечат безопасность данных.

-   Разработчики приложений могут встраивать шифрование в приложения, развертываемые в Azure, с помощью Windows [CryptoAPI](https://msdn.microsoft.com/library/ms867086.aspx) и .NET Framework.

- Шифрование на стороне клиента для хранилища больших двоичных объектов Майкрософт позволяет полностью управлять ключами. Хранилище никогда не видит ключи и не может расшифровать данные.

-   [Azure RMS](https://technet.microsoft.com/library/jj585026.aspx) (с пакетом [SDK RMS](https://msdn.microsoft.com/library/dn758244.aspx)) обеспечивает шифрование на уровне файлов и данных, а также предотвращает утечку данных путем управления доступом на основе политик.

-   Azure поддерживает [шифрование на уровне таблиц (TDE) и столбцов (CLE)](http://blogs.msdn.com/b/sqlsecurity/archive/2015/05/12/recommendations-for-using-cell-level-encryption-in-azure-sql-database.aspx) в виртуальных машинах SQL Server и сторонние локальные серверы управления ключами в центрах обработки данных клиентов.

-   Ключи учетной записи хранения, подписанные URL-адреса, сертификаты управления и другие ключи уникальны для каждого клиента Azure.

-   Гибридное хранилище Azure [StorSimple](http://www.microsoft.com/server-cloud/products/storsimple/overview.aspx) шифрует данные с помощью 128-разрядного открытого и закрытого ключа, прежде чем передавать их в службу хранилища Azure.

-   Azure поддерживает и использует множество механизмов шифрования, включая SSL/TLS, IPsec и AES, в зависимости от типов данных, контейнеров и транспортных протоколов.

##Виртуализация

Платформа Azure использует виртуальную среду. Пользовательские экземпляры работают как автономные виртуальные машины, у которых нет доступа к физическому серверу узла. Такая изоляция принудительно применяется с помощью [уровней привилегий(кольцо 0/кольцо 3) физического процессора](https://en.wikipedia.org/wiki/Protection_ring).

Кольцо 0 обеспечивает максимальные привилегии, а кольцо 3 — минимальные. Гостевая ОС работает в кольце 1 с меньшими привилегиями, а приложения — в последнем кольце 3 с наименьшими привилегиями. Виртуализация физических ресурсов приводит к четкому разделению гостевой ОС и низкоуровневой оболочки, что приводит к дополнительному разделению по безопасности между ними.

Низкоуровневая оболочка Azure выполняет роль микроядра и передает все запросы на доступ к оборудованию из гостевых виртуальных машин узлам, чтобы выполнить обработку с помощью интерфейса общей памяти VMBus. Благодаря этому пользователь не может получить доступ RAW к системе на чтение, запись и выполнение. Таким образом снижается риск совместного доступа к системным ресурсам.

![Антивредоносное ПО Майкрософт в Azure](./media/azure-security-getting-started\sec-azgsfig2.PNG)

### Как Azure реализует виртуализацию

Azure использует брандмауэр низкоуровневой оболочки (фильтр пакетов), который внедрен в низкоуровневую оболочку и настраивается агентом контроллера структуры. Таким образом обеспечивается защита клиентов от несанкционированного доступа. По умолчанию при создании виртуальной машины весь трафик блокируется, а затем агент контроллера структуры настраивает фильтр пакетов, чтобы добавить *правила и исключения*, которые позволяют передавать авторизованный трафик.

Здесь запрограммированы две категории правил.

-   **Конфигурация компьютера или правила инфраструктуры**. По умолчанию весь обмен данными блокируется. Существуют исключения, которые позволяют виртуальной машине отправлять и получать трафик DHCP и DNS. Виртуальные машины также могут отправлять трафик в «общедоступный» Интернет и другим виртуальным машинам в кластере и на сервере активации ОС. Список разрешенных исходящих назначений виртуальных машин не включает в себя подсети маршрутизатора Azure, сервер управления Azure и другие свойства Майкрософт.

-   **Файл конфигурации ролей**. Он определяет входящие списки управления доступом на основе модели службы клиентов. Например, если клиент использует веб-интерфейс на порте 80 на определенной виртуальной машине, Azure открывает TCP-порт 80 для всех IP-адресов при настройке конечной точки в модели [управления службами Azure](resource-manager-deployment-model.md). Если на виртуальной машине запущены серверная часть или рабочая роль, рабочая роль открывается только для виртуальных машин в пределах одного клиента.

##Изоляция

Другое важное требование к безопасности облака — обеспечение разделения. Это помогает предотвратить несанкционированный случайный обмен данными между развертываниями в общей мультитенантной архитектуре.

С помощью изоляции виртуальной локальной сети, списков контроля доступа, балансировщиков нагрузки и IP-фильтров Azure реализует [управление доступом по сети](https://azure.microsoft.com/blog/network-isolation-options-for-machines-in-windows-azure-virtual-networks/) и разделение. Внешний трафик, входящий в виртуальные машины, ограничен портами и протоколами, определяемыми пользователем. Фильтрация сети реализуется, чтобы не пропускать ложный трафик и ограничивать входящий и исходящий трафик надежными компонентами платформ. Политики потока трафика применяются на устройствах защиты периметра, которые блокируют трафик по умолчанию.

![Антивредоносное ПО Майкрософт в Azure](./media/azure-security-getting-started\sec-azgsfig3.PNG)

Преобразование сетевых адресов (NAT) используется для разделения внутреннего и внешнего сетевого трафика. Внутренний трафик не маршрутизируется внешними устройствами. [Виртуальные IP-адреса](http://blogs.msdn.com/b/cloud_solution_architect/archive/2014/11/08/vips-dips-and-pips-in-microsoft-azure.aspx), которые маршрутизируются внешними устройствами, преобразуются во [внутренние динамические IP-адреса](http://blogs.msdn.com/b/cloud_solution_architect/archive/2014/11/08/vips-dips-and-pips-in-microsoft-azure.aspx), которые маршрутизируются только в пределах Azure.

Внешний трафик для виртуальных машин Azure защищает брандмауэр с применением списков управления доступом (ACL) на маршрутизаторах, балансировщиках нагрузки и коммутаторах уровня 3. Разрешено использовать только определенные известные протоколы. Списки управления доступом ограничивают трафик от гостевых виртуальных машин для других виртуальных локальных сетей, используемых для управления. Кроме того, трафик фильтруется с помощью IP-фильтров на ОС узла, за счет чего трафик ограничивается на уровне канала передачи данных и сетевом уровне.

### Как Azure реализует изоляцию

Контролер структуры Azure отвечает за распределение ресурсов инфраструктуры между рабочими нагрузками клиента и управляет односторонней связью между узлом и виртуальными машинами. Низкоуровневая оболочка Azure принудительно разделяет память и процессы между виртуальными машинами и обеспечивает безопасную передачу сетевого трафика клиентам гостевой ОС. Azure реализует также изоляцию для клиентов, хранилища и виртуальных сетей:

-   Каждый клиент Azure AD логически изолирован с помощью периметра безопасности.

-   Учетные записи хранения Azure уникальны для каждой подписки, а при доступе нужно выполнять проверку подлинности с помощью ключа учетной записи хранения.

-   Виртуальные сети логически изолированы благодаря сочетанию уникальных частных IP-адресов, брандмауэров и списков управления доступом по протоколу IP. Балансировщики нагрузки направляют трафик соответствующим клиентам на основе определений конечной точки.

##Виртуальная сеть и брандмауэр

[Распределенные и виртуальные сети](http://download.microsoft.com/download/4/3/9/43902EC9-410E-4875-8800-0788BE146A3D/Windows%20Azure%20Network%20Security%20Whitepaper%20-%20FINAL.docx) в Azure обеспечивают логическую изоляцию трафика частной сети от трафика в других виртуальных сетях Azure.

![Антивредоносное ПО Майкрософт в Azure](./media/azure-security-getting-started\sec-azgsfig4.PNG)

Подписка может содержать несколько изолированных частных сетей (и включать в себя брандмауэр, балансировку нагрузки и преобразование сетевых адресов).

Azure предоставляет три основных уровня разделения сети в каждом кластере Azure для логического разделения трафика. [Виртуальные локальные сети](https://azure.microsoft.com/services/virtual-network/) (VLAN) используются для отделения трафика клиента от остального сетевого трафика Azure. Балансировщики нагрузки ограничивают доступ к сети Azure за пределами кластера.

Входящий и исходящий сетевой трафик виртуальных машин должен проходить через виртуальный коммутатор низкоуровневой оболочки. IP-фильтр в корневой ОС изолирует корневую виртуальную машину от гостевых виртуальных машин и гостевые виртуальные машины друг от друга. Он фильтрует трафик для ограничения взаимодействия между узлами клиента и общедоступным Интернетом (на основе конфигурации службы клиента), отделяя их от других клиентов.

IP-фильтр позволяет запретить виртуальным машинам:

- создавать ложный трафик;

- получать трафик, не предназначенный им;

- направлять трафик защищенным конечным точкам инфраструктуры;

- отправлять или получать несоответствующий широковещательный трафик.

Виртуальные машины можно разместить в [виртуальных сетях Azure](https://azure.microsoft.com/documentation/services/virtual-network/). Эти виртуальные сети аналогичны сетям, настраиваемым в локальных средах, где они обычно связаны с виртуальным коммутатором. Виртуальные машины, подключенные к одной виртуальной сети Azure, могут взаимодействовать без дополнительной настройки. Кроме того, в виртуальной сети Azure вы можете настроить разные подсети.

Ниже представлены технологии виртуальной сети Azure, обеспечивающие защиту при обмене данными в этой сети.

-   [**Группы безопасности сети (NSG)**](./virtual-network/virtual-networks-nsg.md). С помощью группы безопасности сети вы можете контролировать входящий трафик экземпляров виртуальных машин в вашей виртуальной сети. Группа безопасности сети содержит правила контроля доступа, которые разрешают или запрещают трафик в зависимости от направления трафика, протокола, исходного и целевого адреса и порта.

-   [**Пользовательская маршрутизация**](./virtual-network/virtual-networks-udr-overview.md). Маршрутизацией пакетов можно управлять через виртуальное устройство. Для этого нужно создать пользовательские маршруты, которые указывают, чтобы при следующем прыжке пакеты, передаваемые в конкретную подсеть, передавались в виртуальное устройство безопасности сети.

-   [**IP-пересылка**](./virtual-network/virtual-networks-udr-overview.md). Виртуальное устройство безопасности сети должно иметь возможность получать входящий трафик, предназначенный не ему. Чтобы разрешить виртуальной машине получать трафик, направленный по другим адресам, для нее необходимо включить IP-пересылку.

-   [**Принудительное туннелирование**](./vpn-gateway/vpn-gateway-about-forced-tunneling.md). Принудительное туннелирование позволяет перенаправлять или «принудительно направлять» весь интернет-трафик, созданный виртуальными машинами в виртуальной сети Azure, обратно в локальное расположение через туннель VPN типа «сеть-сеть» для проверки и аудита.

-   [**Списки управления доступом** для конечной точки](./virtual-machines/virtual-machines-windows-classic-setup-endpoints.md). Определив списки управления доступом для конечной точки, можно управлять тем, на каких компьютерах разрешены входящие подключения из Интернета к виртуальной машине в виртуальной сети Azure.

-   [**Партнерские решения для сетевой безопасности**](https://azure.microsoft.com/marketplace/). Из Azure Marketplace можно получить доступ к ряду партнерских решений для безопасности сети.

### Как Azure реализует виртуальные сети и брандмауэр

Azure применяет брандмауэры с фильтрацией пакетов на всех виртуальных машинах узла и гостевых виртуальных машинах по умолчанию. Кроме того, в образах ОС Windows из коллекции Azure брандмауэр Windows включен по умолчанию. Балансировщики нагрузки, расположенные по периметру общедоступной сети Azure, управляют обменом данными на основе списков управления доступом по протоколу IP, которыми управляют администраторы клиента.

Azure перемещает данные клиента по закрытым зашифрованным каналам связи в ходе нормальной работы или при возникновении аварии. Ниже представлены другие возможности, применяемые Azure в виртуальных сетях и брандмауэре.

-   **Внутренний брандмауэр узла**. Структура и хранилище Azure работают в собственной операционной системе без низкоуровневой оболочки. Поэтому брандмауэр Windows настраивается с помощью двух наборов правил выше. Хранилища работают в собственной операционной системе. Это нужно для оптимизации производительности.

-   **Брандмауэр узла**. Брандмауэр узла используется для защиты операционной системы узла, в которой работает низкоуровневая оболочка. Правила программируются таким образом, чтобы с ОС узла могли взаимодействовать через конкретный порт только контролер структуры и поля перехода. Существуют другие исключения, разрешающие ответы DHCP и DNS. Azure использует файл конфигурации компьютера, который содержит шаблон правил брандмауэра для ОС узла. Узел защищен от внешних атак брандмауэром Windows, который настроен таким образом, чтобы разрешать взаимодействие только с известными источниками, прошедшими проверку подлинности.

-   **Гостевой брандмауэр**. Он реплицирует правила в фильтре пакетов коммутатора виртуальной машины и программируется в другом программном обеспечении (т. е. в брандмауэре Windows гостевой ОС). Гостевой брандмауэр виртуальной машины можно настроить для ограничения входящего и исходящего обмена данными гостевой виртуальной машины, даже если такой обмен данными разрешен в конфигурациях в IP-фильтре узла. Например, с помощью гостевого брандмауэра виртуальной машины можно ограничить обмен данными между двумя виртуальными сетями, для которых настроено взаимное подключение.

-   **Брандмауэр (FW) хранилища**. Брандмауэр в интерфейсе хранилища фильтрует трафик таким образом, чтобы он поступал только через порты 80 и 443 и другие необходимые порты служебных программ. Брандмауэр в серверной части хранилища ограничивает связь только входящей информацией, поступающей от серверов переднего плана хранилища.

-   **Шлюз виртуальной сети**. [Шлюзы виртуальной сети Azure](./vpn-gateway/virtual-networks-configure-vnet-to-vnet-connection.md) выступают в качестве межорганизационных шлюзов, которые соединяют рабочие нагрузки в виртуальной сети Azure с локальными сайтами. Он нужен для подключения к локальным сайтам через [туннели IPsec VPN типа «сеть — сеть»](./vpn-gateway/vpn-gateway-create-site-to-site-rm-powershell.md) или каналы [ExpressRoute](./expressroute/expressroute-introduction.md). Для туннелей VPN IPsec/IKE шлюзы выполняют подтверждение по протоколу IKE и устанавливают туннели VPN типа «сеть — сеть» с использованием IPsec между виртуальными сетями и локальными сайтами. Кроме того, шлюзы виртуальной сети закрывают [подключения VPN типа «точка — сеть»](./vpn-gateway/vpn-gateway-point-to-site-create.md).

##Безопасный удаленный доступ

Данные, хранящиеся в облаке, должны иметь достаточно механизмов защиты для предотвращения эксплойтов и обеспечения конфиденциальности и целостности при передаче. К таким механизмам относятся сетевые средства управления, которые связаны с корпоративными механизмами для управления удостоверениями и доступом на основе политик с возможностью аудита.

Встроенная технология шифрования позволяет шифровать данные в развертываниях и при передаче между развертываниями, между регионами Azure, а также данные, передаваемые из Azure в локальные центры обработки данных. Доступ администратора к виртуальным машинам через [сеансы удаленного рабочего стола](./virtual-machines/virtual-machines-windows-classic-connect-logon.md), [удаленный сеанс Windows PowerShell](http://blogs.technet.com/b/heyscriptingguy/archive/2013/09/07/weekend-scripter-remoting-the-cloud-with-windows-azure-and-powershell.aspx) и портал управления Azure всегда шифруется.

Чтобы безопасным образом расширить локальный центр обработки данных в облако, воспользуйтесь подключением Azure [VPN типа «сеть — сеть»](./vpn-gateway/vpn-gateway-create-site-to-site-rm-powershell.md) и [VPN типа «точка — сеть»](./vpn-gateway/vpn-gateway-point-to-site-create.md), а также выделенными каналами связи [ExpressRoute](./expressroute/expressroute-introduction.md) (подключения к виртуальным сетям Azure через VPN шифруются).

### Как Azure реализует безопасный удаленный доступ

Подключения к порталу Azure всегда должны проходить проверку подлинности и требуют протокол SSL/TLS. Для безопасного управления можно настроить сертификаты управления. Полностью поддерживаются стандартные безопасные протоколы, такие как [SSTP](https://technet.microsoft.com/magazine/2007.06.cableguy.aspx) и [IPsec](https://en.wikipedia.org/wiki/IPsec).

[Azure ExpressRoute](./expressroute/expressroute-introduction.md) позволяет создавать частные подключения между центрами обработки данных Azure и инфраструктурой вашей локальной среды или среды для совместного размещения. Подключения ExpressRoute не проходят через общедоступный Интернет. Они обеспечивают большую надежность, более высокую скорость, меньшую задержку и большую безопасность, чем обычные веб-каналы связи. В некоторых случаях использование подключений ExpressRoute для передачи данных между локальными сетями и Azure также дает возможность значительно сократить затраты.

##Ведение журналов и мониторинг

В Azure предусмотрена регистрация событий с проверкой подлинности, которые связаны с безопасностью. В ходе этой регистрации создается журнал аудита. Она разработана с учетом защиты от мошенничества. Сюда входит информация о системе, например журналы событий безопасности в виртуальных машинах инфраструктуры Azure и Azure AD. Мониторинг событий безопасности включает сбор событий, таких как изменения в IP-адресах DHCP-сервера или DNS-сервера, попытки доступа к заблокированным портам, протоколам или IP-адресам, изменения в параметрах политики безопасности или брандмауэра, создание учетной записи или группы, неожиданные процессы или установка драйвера.

![Антивредоносное ПО Майкрософт в Azure](./media/azure-security-getting-started\sec-azgsfig5.PNG)

Журналы аудита, которые записывают доступ и действия привилегированного пользователя, попытки авторизованного и несанкционированного доступа, исключения системы и события, связанные с безопасностью информации, сохраняются в течение определенного времени. За сохранение журналов отвечает пользователь, так как он настраивает сбор и сохранение журналов согласно своим требованиям.

### Как Azure реализует ведение журналов и мониторинг

Azure развертывает агенты управления (MA) и агенты монитора безопасности Azure (ASM) на каждом управляемом вычислительном узле, узле хранилища и узле структуры вне зависимости от его типа (внутренний или виртуальный). Каждый агент настроен таким образом, чтобы выполнять проверку подлинности в учетной записи хранения группы обслуживания с помощью сертификата, полученного из хранилища сертификатов Azure, и передавать предварительно настроенные диагностические данные и данные о событиях в учетную запись хранения. Эти агенты не развертываются на виртуальных машинах клиентов.

Администраторы Azure получают доступ к журналам через веб-портал для использования управляемого доступа с проверкой подлинности к журналам. Администратор может анализировать, фильтровать и коррелировать журналы. Учетные записи хранения группы обслуживания Azure для журналов защищены от прямого доступа администратора для предотвращения несанкционированного изменения данных журнала.

Корпорация Майкрософт собирает журналы из сетевых устройств, используя протокол Syslog, и из серверов узлов с помощью служб ACS Microsoft. Эти журналы помещаются в базу данных журналов, в которой генерируются оповещения при возникновении подозрительных событий, которые передаются непосредственно администратору корпорации Майкрософт. Администратор может получать доступ к этим журналам и анализировать их.

[Диагностика Azure](https://msdn.microsoft.com/library/azure/gg433048.aspx) — компонент Azure, который позволяет собирать диагностические данные приложений, запущенных на Azure. Это диагностические данные, используемые для отладки и устранения неполадок, измерения производительности, мониторинга использования ресурсов, анализа трафика и планирования загрузки, а также в целях аудита. После сбора диагностические данные можно передать в учетную запись хранения Azure для сохранения. Передача может быть запланированной или выполняться по требованию.

##Предотвращение угроз

Помимо изоляции, шифрования и фильтрации, Azure использует несколько механизмов и процессов предотвращения угроз для защиты инфраструктуры и служб. К ним относятся внутренние средства управления и технологии, используемые для обнаружения и устранения угроз (таких как DDoS), эскалация привилегий и [проект OWASP Топ-10](https://www.owasp.org/index.php/Category:OWASP_Top_Ten_Project).

Средства управления безопасностью и процессы по управлению рисками, предлагаемые корпорацией Майкрософт для защиты облачной инфраструктуры, снижают риск возникновения инцидентов безопасности. Однако если они возникли, группа управления инцидентами безопасности (SIM), которая входит в группу онлайн-служб безопасности и соответствия требованиям (OSSC) корпорации Майкрософт, готова реагировать на них без перерывов и выходных.

### Как Azure реализует предотвращение угроз

В Azure есть средства управления безопасностью, которые реализуют предотвращение угроз, а также позволяют пользователям предотвращать потенциальные угрозы в своих средах. В следующем списке перечислены возможности для устранения угроз, предлагаемые в Azure.

-   [Антивредоносное ПО Azure](azure-security-antimalware.md) включено по умолчанию на всех серверах инфраструктуры. При желании его можно включить в виртуальных машинах.

-   Корпорация Майкрософт обеспечивает постоянный мониторинг серверов, сетей и приложений для выявления угроз и предотвращения эксплойтов. Автоматические оповещения сообщают администраторам об аномальном поведении, позволяя им выполнить корректирующие действия по устранению внутренних и внешних угроз.

-   Можно развернуть решения безопасности стороннего производителя в подписках, например брандмауэры веб-приложения от компании [Barracuda](https://techlib.barracuda.com/ng54/deployonazure).

-   Подход корпорации Майкрософт к тестированию защиты от несанкционированного доступа включает в себя [тестирование на угрозу извне](http://download.microsoft.com/download/C/1/9/C1990DBA-502F-4C2A-848D-392B93D9B9C3/Microsoft_Enterprise_Cloud_Red_Teaming.pdf). В его рамках специалисты по защите корпорации Майкрософт симулируют атаку на реальные рабочие системы в Azure (не используемые клиентами) для тестирования защиты от реальных постоянных изощренных угроз.

-   Интегрированные развернутые системы управляют распространением и установкой исправлений безопасности на платформах Azure.

##Дальнейшие действия

[Центр управления безопасностью Azure](https://azure.microsoft.com/support/trust-center/)

[Блог группы безопасности Azure](http://blogs.msdn.com/b/azuresecurity/)

[Центр Microsoft Security Response Center](https://technet.microsoft.com/library/dn440717.aspx)

[Блог Active Directory](http://blogs.technet.com/b/ad/)

<!---HONumber=AcomDC_0525_2016-->