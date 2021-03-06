
<properties
	pageTitle="Покупка личного доменного имени в веб-приложениях службы приложений Azure"
	description="Информация о приобретении личного доменного имени для веб-приложения в службе приложение Azure."
	services="app-service\web"
	documentationCenter=""
	authors="rmcmurray"
	manager="wpickett"
	editor=""/>

<tags
	ms.service="app-service-web"
	ms.workload="web"
	ms.tgt_pltfrm="na"
	ms.devlang="na"
	ms.topic="article"
	ms.date="06/24/2016"
	ms.author="robmcm"/>

# Приобретение и настройка личного домена для службы приложений Azure.

> [AZURE.SELECTOR]
- [Приобретение домена для веб-приложений](custom-dns-web-site-buydomains-web-app.md)
- [Веб-приложения со внешними доменами](web-sites-custom-domain-name.md)
- [Веб-приложения с диспетчером трафика](web-sites-traffic-manager-custom-domain-name.md)
- [GoDaddy](web-sites-godaddy-custom-domain-name.md)




[AZURE.INCLUDE [websites-cloud-services-css-guided-walkthrough](../../includes/websites-cloud-services-css-guided-walkthrough.md)]

При создании веб-приложения Azure назначает ему поддомен azurewebsites.net. Например, если веб-приложение называется **contoso**, оно будет иметь URL-адрес **contoso.azurewebsites.net**. Azure также назначает виртуальный IP-адрес.

Для рабочего веб-приложения, возможно, потребуется, чтобы пользователи видели имя пользовательского домена. В этой статье объясняется, как приобрести и настроить личный домен для [Веб-приложений службы приложений](http://go.microsoft.com/fwlink/?LinkId=529714).

[AZURE.INCLUDE [нижний колонтитул](../../includes/custom-dns-web-site-intro-notes.md)]


## Обзор

> [AZURE.NOTE] Пожалуйста, не пытайтесь приобрести домен с помощью подписки без связанной активной кредитной карты. Это может привести к отключению подписки.

Если у вас нет доменного имени для веб-приложения, вы можете легко приобрести его на [портале Azure](https://portal.azure.com/). Во время покупки домена можно автоматически сопоставить WWW и записи DNS корневого домена с вашим веб-приложением. Также можно управлять правами домена на портале Azure.


Чтобы приобрести доменные имена и назначить их своему веб-приложению, сделайте вот что:

1. В браузере откройте [портал Azure](https://portal.azure.com/).

2. На вкладке **Веб-приложения** щелкните имя своего веб-приложения, выберите **Параметры**, а затем — **Личные домены и SSL**.

	![](./media/custom-dns-web-site-buydomains-web-app/dncmntask-cname-6.png)

3. В колонке **Личные домены и SSL** щелкните **Купить домены**.

	![](./media/custom-dns-web-site-buydomains-web-app/dncmntask-cname-buydomains-1.png)

4. В текстовом поле колонки **Купить домены** введите доменное имя, которое вы хотите приобрести, и нажмите клавишу Enter. Под текстовым полем появятся доступные домены. Выберите домен, который хотите приобрести. Вы можете купить сразу несколько доменов.

  ![](./media/custom-dns-web-site-buydomains-web-app/dncmntask-cname-buydomains-2.png)

5. Щелкните **Контактные данные** и заполните форму контактных данных домена.

  ![](./media/custom-dns-web-site-buydomains-web-app/dncmntask-cname-buydomains-3.png)

> [AZURE.NOTE] Очень важно ввести правильные данные в обязательные поля, особенно адрес электронной почты. При приобретении домена без «Защиты конфиденциальности» может быть предложено проверить адрес электронной почты, прежде чем домен станет активным. В некоторых случаях неправильные контактные данные могут препятствовать приобретению доменов.

6. Теперь можно выбрать следующие параметры.

	а) «Автоматическое обновление» домена каждый год.
	
	б) «Защиту личных данных», которая включена в цену покупки БЕСПЛАТНО.
	
	в) «Назначение имен узлов по умолчанию» для WWW и корневого домена для текущего веб-приложения.

  ![](./media/custom-dns-web-site-buydomains-web-app/dncmntask-cname-buydomains-2.5.png)
  
> [AZURE.NOTE] Вариант В настраивает привязки DNS и имя узла автоматически. Так, в веб-приложение можно войти с помощью личного домена сразу же после покупки (кроме редких случаев задержки распространения DNS). Если для веб-приложения используется диспетчер трафика Azure, у вас не будет возможности назначить корневой домен, так как А-записи не работают с диспетчером трафика.
>
>Домены или поддомены, приобретенные через одно веб-приложение, можно назначать другому веб-приложению. Дополнительную информацию см. в шаге 8.

	
7. Щелкните **Выбрать** в колонке **Купить домены**, после чего появятся сведения о приобретении в колонке **Подтверждение покупки**. Если принять условия использования и нажать кнопку **Купить**, ваш заказ будет отправлен. Процесс покупки можно отслеживать в разделе **Уведомление**. Покупка домена может занять несколько минут.

  ![](./media/custom-dns-web-site-buydomains-web-app/dncmntask-cname-buydomains-4.png)

  ![](./media/custom-dns-web-site-buydomains-web-app/dncmntask-cname-buydomains-5.png)

8. Если вам удалось заказать домен, вы можете управлять им и назначить его своему веб-приложению. Щелкните **...** справа от своего домена. Вы можете выбрать **Отменить покупку** или **Управление доменом**. Щелкните **Управление доменом**. Теперь можно привязать **поддомен** к веб-приложению в колонке **Управление доменом**. Если вы хотите привязать **поддомен** к другому веб-приложению, выполните этот шаг, используя соответствующее веб-приложение. Здесь можно назначить домен конечной точке диспетчера трафика (если для веб-приложения используется диспетчер трафика). Для этого выберите имя диспетчера трафика из раскрывающегося меню. Так домен или поддомен автоматически назначается всем веб-приложениям за этой конечной точкой диспетчера трафика.

	![](./media/custom-dns-web-site-buydomains-web-app/dncmntask-cname-buydomains-6.png)

> [AZURE.NOTE] Вы можете отменить покупку в течение 5 дней и полностью вернуть свои деньги. Через 5 дней вы уже не сможете это сделать, и вместо параметра «Отменить покупку» появится возможность «Удалить» домен. Удаление домена приведет к его выключению из подписки без возврата денег, и он станет доступным для других.

После завершения настройки имя личного домена появится в вашем веб-приложении в разделе **Привязки к имени узла**.

На этом этапе можно ввести личное доменное имя в браузере и удостовериться, что оно успешно принято вашим веб-приложением.
 
## Что происходит с приобретенным вами пользовательским доменом

Пользовательский домен, который вы приобрели в колонке **Пользовательские домены и SSL**, привязывается к подписке Azure. Как ресурс Azure, такой пользовательский домен является отдельным и независимым от приложений службы приложений, для которых вы приобретаете домен изначально. Это означает следующее:

- На портале Azure вы можете использовать приобретенный пользовательский домен для нескольких приложений службы приложений, а не только для приложения, для которого вы приобрели его изначально.
- Вы можете управлять всеми пользовательскими доменами, приобретенными в рамках подписки Azure, в колонке **Пользовательские домены и SSL** *любого* приложения службы приложений в этой подписке.
- Вы также можете назначить любое приложение службы приложений из той же подписки Azure поддомену в рамках этого пользовательского домена.
- Если вам потребуется удалить приложение службы приложений, можно не удалять пользовательский домен, к которому оно привязано, чтобы продолжать использовать его для других приложений.

## Что делать, если приобретенный пользовательский домен не отображается

Если вы приобрели пользовательский домен в колонке **Пользовательские домены и SSL** и он недоступен в разделе **Управляемые домены**, причины могут быть следующими.

- Возможно, создание пользовательского домена не завершено. Проверьте хода выполнения в разделе уведомлений (значок "колокольчик") в верхней части портала Azure.
- Возможно, по какой-либо причине возник сбой создания пользовательского домена. Проверьте хода выполнения в разделе уведомлений (значок "колокольчик") в верхней части портала Azure.
- Возможно, пользовательский домен успешно создан, однако колонка еще не обновлена. Попробуйте еще раз открыть колонку **Пользовательские домены и SSL**.
- Возможно, вы удалили пользовательский домен. Проверьте журналы аудита, щелкнув **Параметры** > **Журналы аудита** в главной колонке приложения.
- Колонка **Пользовательские домены и SSL**, в которой вы пытаетесь найти домен, может принадлежать приложению, созданному в другой подписке Azure. Перейдите в другое приложение в другой подписке и проверьте его колонку **Пользовательские домены и SSL**. В рамках портала вы не можете просматривать пользовательские домены, созданные в другой подписке Azure, и управлять ими. Но если щелкнуть элемент **Расширенное управление** в колонке **Управление доменом** нужного домена, вы перейдете на веб-сайт поставщика домена, где можно [вручную настроить этот пользовательский домен как любой внешний пользовательский домен](web-sites-custom-domain-name.md) для приложений, созданных в другой подписке Azure.

<!---HONumber=AcomDC_0629_2016-->