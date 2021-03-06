Вы можете создавать виртуальные машины Azure с помощью обозревателя серверов в Visual Studio.

## Создание виртуальной машины Azure в обозревателе серверов.

Вы можете создавать виртуальные машины не только на [портале управления Azure](http://go.microsoft.com/fwlink/?LinkID=253103), но также с помощью команд в обозревателе серверов. Виртуальные машины можно использовать в качестве внешнего интерфейса для общедоступной конечной точки с балансировкой нагрузки.

### Создание новой виртуальной машины.

1. В обозревателе серверов откройте узел **Azure** и выберите элемент **Виртуальные машины**.

1. В контекстном меню выберите команду **Создать виртуальную машину**.

    Откроется мастер **Создание новой виртуальной машины**.

    ![Команда создания виртуальной машины.](./media/virtual-machines-common-classic-create-manage-visual-studio/IC718342.png)

1. На странице **Выбор подписки** выберите подписку, которая будет использоваться при создании виртуальной машины, и нажмите кнопку **Далее**.

    Если вы не выполнили вход в Azure, нажмите кнопку **Войти**. Затем в раскрывающемся списке выберите подписку Azure, если она еще не выбрана.

1. На странице **Выбор образа виртуальной машины** в раскрывающемся списке **Тип образа** выберите тип образа, а затем выберите образ виртуальной машины в списке **Имя образа**. Закончив, нажмите кнопку **Далее**.

    ![Страница выбора образа виртуальной машины.](./media/virtual-machines-common-classic-create-manage-visual-studio/IC744137.png)

    Для выбора доступны следующие типы образов.

    - **Общедоступные образы** содержат образы виртуальных машин операционных систем и серверного программного обеспечения, таких как Windows Server и SQL Server.

    - **Образы MSDN** содержат образы виртуальных машин программного обеспечения, доступного подписчикам MSDN, такого как Visual Studio и Microsoft Dynamics.

    - **Частные образы** содержат образы специальных и обобщенных виртуальных машин, созданных вами.

    Дополнительные сведения о специальных и обобщенных виртуальных машинах см. в статье [Образ виртуальной машины](https://azure.microsoft.com/blog/2014/04/14/vm-image-blog-post/). Сведения о включении виртуальной машины в шаблон, который можно использовать для быстрого создания новых предварительно настроенных виртуальных машин, см. в статье [Создание образа виртуальной машины Windows для использования в качестве шаблона](https://azure.microsoft.com/documentation/articles/virtual-machines-capture-image-windows-server/).

    Если щелкнуть имя образа виртуальной машины, то в правой части страницы отобразится информация о выбранном образе.

    >[AZURE.NOTE] Добавить новые образы в списки **Общедоступные образы** и **Образы MSDN** нельзя, так как эти списки доступны только для чтения. Все создаваемые вами виртуальные машины добавляются в список **Частные образы**.

    Если вы являетесь подписчиком MSDN уровня Visual Studio, вы можете создавать предварительно настроенные виртуальные машины Azure, которые содержат Visual Studio. Также вам доступно несколько других типов образов. Дополнительные сведения см. в статьях [Создание виртуальной машины в Visual Studio с помощью образа из галереи образов Visual Studio 2013, доступной подписчикам MSDN](http://visualstudio2013msdngalleryimage.azurewebsites.net) и [Подписки MSDN](https://www.visualstudio.com/products/msdn-subscriptions-vs).|

1. На странице **Основные параметры виртуальной машины** введите имя компьютера и укажите характеристики виртуальной машины, такие как размер, имя пользователя и пароль. Закончив, нажмите кнопку **Далее**.

    Для доступа к машине с помощью удаленного рабочего стола вам потребуется новое имя пользователя и пароль, поэтому рекомендуется сохранить эти данные, чтобы не забыть их. После создания виртуальной машины Azure в Visual Studio вы сможете изменить ее размер и другие параметры на [портале управления Azure](http://go.microsoft.com/fwlink/?LinkID=253103).

    >[AZURE.NOTE] За увеличение размера виртуальной машины может взиматься дополнительная плата. Дополнительные сведения см. в статье [Сведения о ценах — виртуальные машины](https://azure.microsoft.com/pricing/details/virtual-machines/).

1. Виртуальные машины, созданные в Visual Studio, требуют наличия облачной службы. На странице **Параметры облачной службы** в раскрывающемся списке выберите облачную службу для виртуальной машины или выберите **<Создать...>**, если облачной службы еще нет или вы хотите создать новую. Кроме того, вам потребуется учетная запись хранения. Для этого в раскрывающемся списке **Учетная запись хранения** выберите учетную запись (или создайте новую). Дополнительную информацию см. в статье [Общие сведения о службе хранилища Microsoft Azure](../articles/storage/storage-introduction/) .

1. Если вы хотите указать виртуальную сеть (необязательно), выберите ее в раскрывающихся списках "Виртуальная сеть" и "Подсеть".

    Виртуальные машины, входящие в группу доступности, развертываются в разные домены сбоя. Дополнительные сведения см. в статье [Виртуальная сеть Azure](https://azure.microsoft.com/services/virtual-network/).

1. Если вы хотите включить виртуальную машину в группу доступности (необязательно), установите флажок **Указать группу доступности** и выберите группу в раскрывающемся списке. По завершении нажмите кнопку **Далее**.

    Добавление виртуальной машины в группу доступности помогает обеспечить доступность приложения в случае сбоев сети, локальных жестких дисков, а также при плановых простоях. Для создания виртуальных сетей, подсетей и групп доступности вам потребуется использовать [портал управления Azure](http://go.microsoft.com/fwlink/?LinkID=253103). Дополнительные сведения см. в статье [Управление доступностью виртуальных машин](https://azure.microsoft.com/documentation/articles/manage-availability-virtual-machines/).

1. На странице **Конечные точки** выберите общедоступные конечные точки, которые должны быть доступны пользователям вашей виртуальной машины. Например, кроме конечных точек удаленного рабочего стола и PowerShell, которые включены по умолчанию, вы можете включить HTTP (порт 80). Чтобы добавить конечную точку, выберите ее в раскрывающемся списке **Имя порта** и нажмите кнопку **Добавить**. Чтобы удалить конечную точку, щелкните красный значок **X** рядом с именем в списке конечных точек.

    ![Страница "Конечные точки" мастера виртуальных машин.](./media/virtual-machines-common-classic-create-manage-visual-studio/IC718351.png)

    Доступность той или иной конечной точки зависит от облачной службы, выбранной для вашей виртуальной машины. Дополнительные сведения см. в статье [Конечные точки службы Azure](https://azure.microsoft.com/documentation/articles/virtual-machines-set-up-endpoints/).

    >[AZURE.NOTE] Включение общедоступных конечных точек позволяет открыть доступ к вашей виртуальной машине из Интернета. Не забудьте установить и правильно настроить конечные точки и службы на виртуальной машине, такие как списки управления доступом (ACL) для конечных точек. Дополнительные сведения см. в статье [Настройка конечных точек виртуальной машины](https://azure.microsoft.com/documentation/articles/virtual-machines-set-up-endpoints/).

1. Завершив настройку параметров виртуальной машины, нажмите кнопку **Создать**, чтобы создать виртуальную машину.

    Пока Azure создает виртуальную машину, в разделе **Журнал действий Azure** отображается ход создания виртуальной машины.

    ![Журнал действий виртуальной машины — выполняется.](./media/virtual-machines-common-classic-create-manage-visual-studio/IC744138.png)

    Чтобы отобразить только сведения о виртуальной машине, в разделе **Журнал действий Azure** выберите вкладку **Виртуальные машины**.

    ![Журнал действий виртуальной машины — завершено.](./media/virtual-machines-common-classic-create-manage-visual-studio/IC744139.png)

    Если операция будет завершена успешно, в узел **Виртуальные машины** обозревателя серверов будет добавлена новая виртуальная машина. Для входа в нее выберите ярлык **Подключение с помощью удаленного рабочего стола**.

    ![Отображение виртуальной машины в обозревателе серверов.](./media/virtual-machines-common-classic-create-manage-visual-studio/IC744140.png)

## Управление виртуальными машинами

На странице конфигурации виртуальной машины, помимо завершения работы, соединения, обновления и добавления контрольных точек, вы также можете просматривать и изменять параметры виртуальной машины. Вы можете:

- изменить размер виртуальной машины;

- выбрать группу доступности для виртуальной машины;

- добавить, удалить и изменить настройки общедоступных конечных точек;

- добавить, удалить и настроить расширения для виртуальной машины;

- просмотреть сведения о дисках, связанных с виртуальной машиной;

### просмотреть и изменить параметры виртуальной машины.

1. В обозревателе серверов выберите виртуальную машину, развернув узел **Виртуальные машины Azure**.

1. В контекстном меню выберите элемент **Настройка**, чтобы открыть страницу конфигурации виртуальной машины.

    ![Страница настройки виртуальной машины Azure.](./media/virtual-machines-common-classic-create-manage-visual-studio/IC744141.png)

1. Просмотрите информацию о виртуальной машине или измените параметры.

### Сохранение или восстановление состояния виртуальной машины.

По мере настройки виртуальной машины и установки программного обеспечения рекомендуется регулярно сохранять ее состояние путем создания контрольных точек. Контрольная точка представляет собой моментальный снимок (или образ) текущего состояния вашей виртуальной машины. Если с виртуальной машиной что-то случится или вы захотите заново ее настроить, то для экономии времени вы можете восстановить предыдущее состояние из контрольной точки, а не начинать все заново.

### Создание контрольной точки виртуальной машины.

1. В обозревателе серверов выберите виртуальную машину, развернув узел **Виртуальные машины Azure**.

1. В контекстном меню выберите элемент **Настройка**, чтобы открыть страницу конфигурации виртуальной машины.

1. На странице конфигурации нажмите кнопку **Записать образ**.

    ![Кнопка записи на странице настройки Azure.](./media/virtual-machines-common-classic-create-manage-visual-studio/IC744142.png)

    Отображается диалоговое окно **Запись виртуальной машины**.

    ![Диалоговое окно записи виртуальной машины Azure.](./media/virtual-machines-common-classic-create-manage-visual-studio/IC744143.png)

1. Укажите название образа и описание. Поля названия и описания будут заполнены значениями по умолчанию, но вы можете ввести собственные значения.

1. Если на виртуальной машине уже запущена программа Sysprep, установите флажок **Программа Sysprep запущена на виртуальной машине**.

    Sysprep — это средство, которое, помимо прочего, позволяет удалить информацию, относящуюся к конкретной версии Windows на виртуальной машине. В результате такая система может использоваться в качестве шаблона для установки на других машинах. Дополнительные сведения см. в статье [Создание образа виртуальной машины Windows для использования в качестве шаблона](https://azure.microsoft.com/documentation/articles/virtual-machines-capture-image-windows-server/).

1. Когда параметры создания образа будут настроены, нажмите кнопку **Записать**, чтобы создать контрольную точку.

    Во время создания контрольной точки в разделе **Журнал действий Azure** будет отображаться ход выполнения операции.

    ![Контрольная точка записи виртуальной машины.](./media/virtual-machines-common-classic-create-manage-visual-studio/IC744144.png)

    После завершения создания контрольной точки вы увидите сообщение в разделе **Журнал действий Azure**.

    ![Операция контрольной точки завершена.](./media/virtual-machines-common-classic-create-manage-visual-studio/IC744145.png)

## Управление контрольными точками виртуальной машины.

### Восстановление виртуальной машины до ранее сохраненного состояния.

- Выполните действия, описанные в статье [Пошаговое руководство по восстановлению виртуальных машин Microsoft Azure в облаке с помощью PowerShell, часть 2](http://blogs.technet.com/b/keithmayer/archive/2014/02/04/step-by-step-perform-cloud-restores-of-windows-azure-virtual-machines-using-powershell-part-2.aspx).

### Удаление контрольной точки.

1. Перейдите на [портал управления Azure](http://go.microsoft.com/fwlink/?LinkID=253103).

1. На странице конфигурации виртуальной машины в верхней части страницы выберите вкладку **Образы**.

1. Выберите контрольную точку, которую необходимо удалить, и нажмите кнопку **Удалить** в нижней части страницы.

## Завершение работы виртуальной машины.

1. В обозревателе серверов раскройте узел **Виртуальные машины Azure** и выберите виртуальную машину, работу которой необходимо завершить.

1. В контекстном меню выберите команду **Завершение работы** или **Настройка**, чтобы открыть страницу конфигурации виртуальной машины. На этой странице нажмите кнопку **Завершение работы**.

## Дальнейшие действия

Дополнительные сведения о создании виртуальных машин см. в статьях [Создание виртуальной машины под управлением Linux](../articles/virtual-machines/virtual-machines-linux-quick-create-cli.md) и [Создание виртуальной машины под управлением Windows на портале предварительной версии Azure](../articles/virtual-machines/virtual-machines-windows-hero-tutorial.md).
