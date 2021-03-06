<properties 
   pageTitle="Управление устройством StorSimple с помощью PowerShell | Microsoft Azure"
   description="Узнайте, как использовать Windows PowerShell для StorSimple для управления своим устройством StorSimple."
   services="storsimple"
   documentationCenter="NA"
   authors="alkohli"
   manager="carmonm"
   editor="" />
<tags 
   ms.service="storsimple"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="TBD"
   ms.date="05/24/2016"
   ms.author="alkohli@microsoft.com" />

# Администрирование устройства с помощью Windows PowerShell для StorSimple

## Общие сведения

Windows PowerShell для StorSimple — это интерфейс командной строки, который можно использовать для управления устройством Microsoft Azure StorSimple. Как видно из названия, данный интерфейс командной строки основан на Windows PowerShell и встроен в ограниченное пространство выполнения. Пользователь командной строки видит ограниченное пространство выполнения как сокращенную версию Windows PowerShell. Данный интерфейс не только поддерживает некоторые основные функции Windows PowerShell, но и включает дополнительные командлеты, предназначенные специально для управления устройством Microsoft Azure StorSimple.

В этой статье описываются функции Windows PowerShell для StorSimple и приводятся инструкции по подключению к этому интерфейсу, а также ссылки на пошаговые руководства по рабочим процессам, которые можно выполнять с помощью этого интерфейса. В число таких рабочих процессов входят: регистрация устройства, настройка сетевого интерфейса на устройстве, установка обновлений, для которых устройство необходимо перевести в режим обслуживания, изменение состояния устройства и устранение возможных неполадок.

Прочитав эту статью, вы сможете выполнять следующие действия.

- Подключиться к устройству StorSimple с помощью Windows PowerShell для StorSimple.

- Администрировать устройство StorSimple с помощью Windows PowerShell для StorSimple.

- Получить справку по Windows PowerShell для StorSimple.

>[AZURE.NOTE] 	

>- Командлеты Windows PowerShell для StorSimple позволяют управлять устройствами StorSimple через последовательную консоль или удаленно с помощью удаленного взаимодействия Windows PowerShell. Дополнительные сведения о каждом отдельном командлете, который можно использовать в этом интерфейсе, см. в [справочнике по командлетам Windows PowerShell для StorSimple](https://technet.microsoft.com/library/dn688168.aspx).

>- Командлеты Azure PowerShell для StorSimple — это другая коллекция командлетов, которые позволяют автоматизировать задачи обновления и миграции StorSimple с помощью командной строки. Дополнительную информацию о командлетах Azure PowerShell для StorSimple см. в [справочной документации по командлетам Azure StorSimple](https://msdn.microsoft.com/library/azure/dn920427.aspx).

Получить доступ к Windows PowerShell для StorSimple можно одним из следующих способов.

- [Подключение к последовательной консоли устройства StorSimple](#connect-to-windows-powershell-for-storsimple-via-the-device-serial-console)
- [Удаленное подключение к StorSimple с помощью Windows PowerShell](#connect-remotely-to-storsimple-using-windows-powershell-for-storsimple)
	

## Подключение к Windows PowerShell для StorSimple через последовательную консоль устройства

Для подключения к Windows PowerShell для StorSimple можно [загрузить PuTTY](http://www.putty.org/) или другую программу эмуляции терминала. Настройте PuTTY на доступ к устройству Microsoft Azure StorSimple. В следующих разделах содержатся подробные инструкции по настройке PuTTY и подключению к устройству, а также описаны различные параметры меню последовательной консоли.

### Параметры PuTTY

Для подключения к интерфейсу Windows PowerShell из последовательной консоли используйте указанные ниже параметры PuTTY.

#### Настройка PuTTY

1. На панели **Категория** диалогового окна **Перенастройка** PuTTY выберите пункт **Клавиатура**.

2. В данном разделе должны быть выбраны указанные ниже параметры (при создании нового сеанса они устанавливаются по умолчанию).

 	|Клавиша|Выберите пункт|
 	|---|---|
 	|BACKSPACE|Control-? (127)|
	|Клавиши HOME и END|Standard|
	|Функциональные клавиши и дополнительная клавиатура|ESC[n~|
	|Начальное состояние клавиш управления курсором|Обычный|
	|Начальное состояние числовой клавиатуры|Обычный|
	|Включение компонентов на дополнительной клавиатуре|Control-Alt отличается от AltGr|

	![Поддерживаемые параметры PuTTY](./media/storsimple-windows-powershell-administration/IC740877.png)

3. Нажмите кнопку **Применить**.

4. На панели **Категория** выберите пункт **Перевод**.

5. В поле **Удаленный набор символов** выберите пункт **UTF-8**.

6. В разделе **Обработка символов рисования линии** выберите пункт **Использовать кодовые точки графических объектов Юникод**. Правильный выбор параметр PuTTY показан ниже.

	![Параметры UTF PuTTY](./media/storsimple-windows-powershell-administration/IC740878.png)

7. Нажмите кнопку **Применить**.


Теперь PuTTY можно использовать для подключения к последовательной консоли устройства, выполнив указанные ниже действия.

[AZURE.INCLUDE [storsimple-use-putty](../../includes/storsimple-use-putty.md)]


### О последовательной консоли

При обращении к интерфейсу Windows PowerShell устройства StorSimple через последовательную консоль отображается заглавное сообщение, а затем параметры меню.

Заглавное сообщение содержит основные сведения об устройстве StorSimple, такие как модель, имя, версия установленного программного обеспечения и состояние контроллера, к которому производится доступ. Пример заглавного сообщения приведен ниже.

![Заглавное сообщение в последовательной консоли](./media/storsimple-windows-powershell-administration/IC741098.png)

>[AZURE.IMPORTANT] С помощью заглавного сообщения можно определить, активен или пассивен контроллер, к которому вы подключаетесь.

Ниже показаны различные параметры выполнения, доступные в меню последовательной консоли.

![Регистрация устройства 2](./media/storsimple-windows-powershell-administration/IC740906.png)

Вы можете выбрать один из следующих параметров.

1. **Войти с полным доступом**: позволяет подключиться к пространству выполнения **SSAdminConsole** на локальном контроллере (если указаны соответствующие учетные данные). (Локальный контроллер — это контроллер, к которому вы обращаетесь через последовательную консоль устройства StorSimple.) Этот параметр позволяет также разрешить службе поддержки Майкрософт доступ к неограниченному пространству выполнения (сеанс поддержки) с целью поиска и устранения возможных неполадок устройства. Если вход в систему был выполнен с данным параметром, вы можете разрешить специалисту службы поддержки Майкрософт доступ к неограниченному пространству выполнения, используя соответствующий командлет. Дополнительные сведения см. в статье [Запуск сеанса поддержки](storsimple-contact-microsoft-support.md#start-a-support-session-in-windows-powershell-for-storsimple).

2. **Войти на одноранговый контроллер с полным доступом**: работает как параметр 1, но (при условии, что указаны соответствующие учетные данные) позволяет подключиться к пространству выполнения **SSAdminConsole** на одноранговом контроллере. Поскольку устройство StorSimple является устройством высокой доступности с двумя контроллерами в активно-пассивной конфигурации, одноранговым считается контроллер устройства, к которому вы обращаетесь через последовательную консоль. Данный параметр, как и параметр 1, позволяет также предоставить службе поддержки Майкрософт доступ к неограниченному пространству выполнения на одноранговом контроллере.

3. **Подключиться с ограниченным доступом**: используется для доступа к интерфейсу Windows PowerShell в ограниченном режиме. Учетные данные для доступа не запрашиваются. Данный параметр подключается к более ограниченному пространству выполнения, чем параметры 1 и 2. Примеры задач, которые доступны при подключении с параметром 1 и не могут быть решены в таком пространстве выполнения:

	- сброс до заводских настроек;
	- изменение пароля;
	- включение или отключение доступа для службы поддержки;
	- установка обновлений;
	- установка исправлений. 
												

	>[AZURE.NOTE] **Этот параметр рекомендуется использовать, если вы забыли пароль администратора устройства и не можете подключиться с параметром 1 или 2.**

4. **Изменить язык**: позволяет изменить язык интерфейса Windows PowerShell. Поддерживаются следующие языки: английский, японский, русский, французский, южнокорейский, испанский, итальянский, немецкий, китайский и португальский (Бразилия).


## Удаленное подключение к StorSimple с помощью Windows PowerShell для StorSimple

Для подключения к устройству StorSimple можно использовать удаленное взаимодействие Windows PowerShell. При таком способе подключения меню не отображается. (Меню появляется только при подключении к устройству с помощью последовательной консоли. При удаленном подключении осуществляется переход к эквиваленту "Вариант 1 — полный доступ" на последовательной консоли.) При использовании удаленного взаимодействия Windows PowerShell вы подключаетесь к конкретному пространству выполнения. Также можно указать язык интерфейса.

Язык интерфейса не зависит от языка, выбранного с помощью параметра **Изменить язык** в меню последовательной консоли. Если язык не задан, удаленный сеанс PowerShell автоматически выберет языковой стандарт устройства, к которому вы подключаетесь.

>[AZURE.NOTE] При работе с виртуальными узлами Microsoft Azure и виртуальными устройствами StorSimple можно использовать удаленное взаимодействие Windows PowerShell и виртуальный узел для подключения к виртуальному устройству. Если вы настроили общую папку на главном компьютере, где будут храниться данные из сеанса Windows PowerShell, помните, что субъект "Все" включает только пользователей, прошедших проверку подлинности. Таким образом, если вы настроили общий ресурс, чтобы разрешить доступ всем пользователям, и подключились без указания учетных данных, программа использует анонимный субъект без проверки подлинности и выдаст сообщение об ошибке. Чтобы решить эту проблему, активируйте в общем ресурсе гостевую учетную запись и предоставьте ей полный доступ к общему ресурсу либо укажите действительные учетные данные вместе с соответствующим командлетом Windows PowerShell.

Для подключения с использованием удаленного взаимодействия Windows PowerShell можно использовать HTTP или HTTPS. Соответствующие инструкции см. в следующих учебниках.

- [Подключение по протоколу HTTP](storsimple-remote-connect.md#connect-through-http)
- [Подключение по протоколу HTTPS](storsimple-remote-connect.md#connect-through-https)

## Вопросы безопасности подключения

Выбирая способ подключения к Windows PowerShell для StorSimple, примите во внимание следующие аспекты.

- Прямое подключение к последовательной консоли устройства является безопасным, а подключение к последовательной консоли через сетевые коммутаторы — нет. При подключении к последовательной консоли устройства через сетевые коммутаторы учитывайте угрозы для безопасности.

- Подключение через сеанс HTTP может быть более безопасным, чем подключение через последовательную консоль по сети. Хотя это не самый безопасный метод, его можно использовать в доверенных сетях.

- Наиболее безопасным и рекомендуемым способом подключения является подключение через сеанс HTTPS.


## Администрировать устройство StorSimple с помощью Windows PowerShell для StorSimple.
В следующей таблице собраны все стандартные задачи управления и сложные рабочие процессы, которые могут быть выполнены в интерфейсе Windows PowerShell устройства StorSimple. Чтобы получить дополнительные сведения о каждом рабочем процессе, щелкните соответствующую запись в таблице.

#### Рабочие процессы Windows PowerShell для StorSimple

|Если вы хотите...|Используйте эту процедуру.|
|---|---|
|Зарегистрировать устройство|[Настройка и регистрация устройства средствами Windows PowerShell для StorSimple](storsimple-deployment-walkthrough.md#step-3-configure-and-register-the-device-through-windows-powershell-for-storsimple) |
|Настроить веб-прокси</br>Посмотреть параметры веб-прокси|[Настройка веб-прокси для устройства StorSimple](storsimple-configure-web-proxy.md)|
|Изменить параметры сетевого интерфейса DATA 0 на устройстве StorSimple|[Изменение сетевого интерфейса DATA 0 на устройстве StorSimple](storsimple-modify-data-0.md)|
|Остановить контроллер </br>Перезагрузить или выключить контроллер </br>Выключить устройство </br>Восстановить заводские настройки устройства|[Управление контроллерами устройства](storsimple-manage-device-controller.md)|
|Установка обновлений и исправлений в режиме обслуживания|[Обновление устройства](storsimple-update-device.md)|
|Перейти в режим обслуживания</br>Выйти из режима обслуживания|[Режимы устройств StorSimple](storsimple-device-modes.md)|
|Создать пакет поддержки</br>Расшифровать и изменить пакет поддержки|[Создание пакетов поддержки и управление ими](storsimple-create-manage-support-package.md)|
|Запустить сеанс поддержки</br>|[Запуск сеанса поддержки в Windows PowerShell для StorSimple](/storsimple-contact-microsoft-support.md#start-a-support-session-in-windows-powershell-for-storsimple)
 

## Получить справку по Windows PowerShell для StorSimple

В Windows PowerShell для StorSimple доступна справка по командлетам. Существует также актуальная онлайн-справка, которую можно использовать для обновления справки на компьютере.

Получить справку в этом интерфейсе можно точно так же, как в Windows PowerShell; большинство командлетов справки здесь работают. Справку по Windows PowerShell см. в библиотеке TechNet: [Работа со сценариями в Windows PowerShell](http://go.microsoft.com/fwlink/?LinkID=108518).

Ниже приводится краткое описание типов справки для данного интерфейса Windows PowerShell с указанием способов обновления.

#### Справка по командлетам

- Чтобы получить справку по какому-либо командлету или функции, используйте следующую команду: `Get-Help <cmdlet-name>`.

- Чтобы получить онлайн-справку по какому-либо командлету, используйте предыдущий командлет с таким параметром`-Online`: `Get-Help <cmdlet-name> -Online`.

- Для получения полной справки можно использовать параметр `–Full`, а для получения примеров — параметр `–Examples`.

#### Обновление справки

Справка легко обновляется через интерфейс Windows PowerShell. Для обновления справки в системе выполните указанные ниже действия.

#### Обновление справки по командлетам

1. Запустите Windows PowerShell с параметром **Запуск от имени администратора**.

1. В командной строке введите следующий текст: `Update-Help`.

1. Будут установлены обновленные файлы справки.

1. После установки файлов справки введите следующий текст: `Get-Help Get-Command`. Появится список командлетов, для которых доступна справка.


>[AZURE.NOTE] Чтобы получить список всех доступных командлетов в каком-либо пространстве выполнения, войдите в соответствующий пункт меню и выполните командлет `Get-Command`.

## Дальнейшие действия
Если при выполнении одного из указанных выше рабочих процессов возникнут проблемы с устройством StorSimple, см. раздел [Средства устранения неисправностей в развертываемых средах StorSimple](storsimple-troubleshoot-deployment.md#tools-for-troubleshooting-storsimple-deployments).

<!---HONumber=AcomDC_0615_2016-->