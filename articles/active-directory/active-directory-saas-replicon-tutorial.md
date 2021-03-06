<properties 
    pageTitle="Учебник. Интеграция Azure Active Directory с Replicon | Microsoft Azure" 
    description="Узнайте, как использовать Replicon с Azure Active Directory для реализации единого входа, автоматической подготовки пользователей и выполнения других задач." 
    services="active-directory" 
    authors="jeevansd"  
    documentationCenter="na" 
    manager="femila"/>
<tags 
    ms.service="active-directory" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.tgt_pltfrm="na" 
    ms.workload="identity" 
    ms.date="07/07/2016" 
    ms.author="jeedes" />

#Учебник. Интеграция Azure Active Directory с Replicon
  
Цель данного учебника — показать интеграцию Azure и Replicon. Сценарий, описанный в этом учебнике, предполагает, что у вас уже имеется:

-   Действующая подписка на Azure
-   Клиент Replicon.
  
По завершении работы с этим руководством пользователи Azure AD, назначенные в Replicon, смогут выполнять единый вход в приложение на веб-сайте Replicon компании (вход, инициированный поставщиком услуг) или следуя указаниям в статье [Общие сведения о панели доступа](active-directory-saas-access-panel-introduction.md).
  
Сценарий, описанный в этом учебнике, состоит из следующих блоков:

1.  Включение интеграции приложений для Replicon
2.  Настройка единого входа
3.  Настройка подготовки учетных записей пользователей
4.  Назначение пользователей

![Сценарий](./media/active-directory-saas-replicon-tutorial/IC777798.png "Сценарий")
##Включение интеграции приложений для Replicon
  
В этом разделе показано, как включить интеграцию приложений для Replicon.

###Чтобы включить интеграцию приложений для Replicon, выполните следующие действия:

1.  На классическом портале Azure в области навигации слева щелкните **Active Directory**.

    ![Active Directory](./media/active-directory-saas-replicon-tutorial/IC700993.png "Active Directory")

2.  Из списка **Каталог** выберите каталог, для которого нужно включить интеграцию каталогов.

3.  Чтобы открыть представление приложений, в представлении каталога нажмите **Приложения** в верхнем меню.

    ![Приложения](./media/active-directory-saas-replicon-tutorial/IC700994.png "Приложения")

4.  В нижней части страницы нажмите кнопку **Добавить**.

    ![Добавление приложения](./media/active-directory-saas-replicon-tutorial/IC749321.png "Добавление приложения")

5.  В диалоговом окне **Что необходимо сделать?** нажмите **Добавить приложение из коллекции**.

    ![Добавить приложение из коллекции](./media/active-directory-saas-replicon-tutorial/IC749322.png "Добавить приложение из коллекции")

6.  В **поле поиска** введите **Replicon**.

    ![Коллекция приложений](./media/active-directory-saas-replicon-tutorial/IC777799.png "Коллекция приложений")

7.  В области результатов выберите **Replicon** и нажмите кнопку **Завершить**, чтобы добавить приложение.

    ![Replicon](./media/active-directory-saas-replicon-tutorial/IC777800.png "Replicon")
##Настройка единого входа
  
В этом разделе показано, как разрешить пользователям проходить аутентификацию в Replicon со своей учетной записью Azure AD, используя федерацию на основе протокола SAML.

###Чтобы настроить единый вход, выполните следующие действия.

1.  На странице интеграции с приложением **Replicon** классического портала Azure щелкните **Настройка единого входа**, чтобы открыть диалоговое окно **Настройка единого входа**.

    ![Настройка единого входа](./media/active-directory-saas-replicon-tutorial/IC777801.png "Настройка единого входа")

2.  На странице **Как пользователи должны входить в Replicon** выберите **Единый вход Microsoft Azure AD** и нажмите кнопку **Далее**.

    ![Настройка единого входа](./media/active-directory-saas-replicon-tutorial/IC777802.png "Настройка единого входа")

3.  На странице **Настройка URL-адреса приложения** выполните следующие действия.

    ![Настройка URL-адреса приложения](./media/active-directory-saas-replicon-tutorial/IC777803.png "Настройка URL-адреса приложения")

    1.  В текстовом поле **URL-адрес входа в Replicon** введите URL-адрес клиента Replicon (например, *https://na2.replicon.com/company/saml2/sp-sso/post*).
    2.  В текстовом поле **URL-адрес ответа Replicon** введите URL-адрес службы Replicon **AssertionConsumerService** (например, *https://global.replicon.com/!/saml2/company/sso/post*).

        >[AZURE.NOTE] URL-адрес можно получить из метаданных Replicon по адресу: 
	**https://global.replicon.com/!/saml2/\<ключ\_вашей\_компании>**.

    3.  Нажмите кнопку **Далее**.

4.  На странице **Настройка единого входа в Replicon** нажмите кнопку **Скачать метаданные**, чтобы скачать их, а затем сохраните файл данных на свой компьютер.

    ![Настройка единого входа](./media/active-directory-saas-replicon-tutorial/IC777804.png "Настройка единого входа")

5.  В другом окне веб-браузера войдите на сайт Replicon своей компании в качестве администратора.

6.  Для настройки SAML 2.0 выполните следующие действия:

    ![Включение проверки подлинности SAML](./media/active-directory-saas-replicon-tutorial/IC777805.png "Включение проверки подлинности SAML")

    1.  Чтобы отобразить диалоговое окно **EnableSAMLAuthentication2**, добавьте следующую строку в URL-адрес после ключа компании: **/services/SecurityService1.svc/help/test/EnableSAMLAuthentication2**. Ниже показана схема полного URL-адреса: **https://na2.replicon.com/\<ключ\_вашей\_компании>/services/SecurityService1.svc/help/test/EnableSAMLAuthentication2**.
    2.  Щелкните **+**, чтобы развернуть раздел **v20Configuration**.
    3.  Щелкните **+**, чтобы развернуть раздел **metaDataConfiguration**.
    4.  Щелкните **Выбрать файл**, чтобы выбрать XML-файл метаданных поставщика удостоверений, и нажмите кнопку **Отправить**.

7.  На классическом портале Azure выберите подтверждение конфигурации единого входа, а затем нажмите кнопку **Завершить**, чтобы закрыть диалоговое окно **Настройка единого входа**.

    ![Настройка единого входа](./media/active-directory-saas-replicon-tutorial/IC778418.png "Настройка единого входа")
##Настройка подготовки учетных записей пользователей
  
Чтобы пользователи Azure AD могли выполнять вход в Replicon, они должны быть подготовлены для Replicon. В случае с Replicon подготовка выполняется вручную.

###Чтобы настроить подготовку учетных записей пользователей, выполните следующие действия.

1.  В окне веб-браузера войдите на сайт Replicon своей компании в качестве администратора.

2.  Выберите **Администрирование > Пользователи**.

    ![Пользователи](./media/active-directory-saas-replicon-tutorial/IC777806.png "Пользователи")

3.  Нажмите кнопку **+ Добавить пользователя**.

    ![Добавить пользователя](./media/active-directory-saas-replicon-tutorial/IC777807.png "Добавить пользователя")

4.  В разделе **Профиль пользователя** сделайте следующее:

    ![Профиль пользователя](./media/active-directory-saas-replicon-tutorial/IC777808.png "Профиль пользователя")

    1.  В текстовом поле **Имя для входа** введите электронный адрес пользователя Azure AD, которого хотите подготовить.
    2.  Для параметра **Тип аутентификации** выберите значение **Единый вход**.
    3.  В текстовом поле **Отдел** введите отдел пользователя.
    4.  Для параметра **Тип сотрудника** выберите значение **Администратор**.
    5.  Нажмите кнопку **Сохранить профиль пользователя**.

>[AZURE.NOTE]Вы можете использовать любые другие инструменты создания учетных записей пользователя Replicon или API, предоставляемые Replicon для подготовки учетных записей пользователя AAD.

##Назначение пользователей
  
Чтобы проверить свою конфигурацию, предоставьте доступ пользователям Azure AD, которые должны использовать приложение, назначив их.

###Чтобы назначить пользователей Replicon, выполните следующие действия:

1.  На классическом портале Azure создайте тестовую учетную запись.

2.  На странице интеграции с приложением **Replicon** щелкните **Назначить пользователей**.

    ![Назначение пользователей](./media/active-directory-saas-replicon-tutorial/IC777809.png "Назначить пользователей")

3.  Выберите тестового пользователя, нажмите кнопку **Назначить**, а затем — **Да**, чтобы подтвердить назначение.

    ![Да](./media/active-directory-saas-replicon-tutorial/IC767830.png "Да")
  
Если вы хотите проверить параметры единого входа, откройте панель доступа. Дополнительные сведения о панели доступа см. в статье [Общие сведения о панели доступа](active-directory-saas-access-panel-introduction.md).

<!---HONumber=AcomDC_0713_2016-->