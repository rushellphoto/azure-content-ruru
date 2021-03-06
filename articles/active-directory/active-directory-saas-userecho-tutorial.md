<properties
	pageTitle="Руководство. Интеграция Azure Active Directory с UserEcho | Microsoft Azure"
	description="Узнайте, как настроить единый вход Azure Active Directory в приложении UserEcho."
	services="active-directory"
	documentationCenter=""
	authors="jeevansd"
	manager="stevenpo"
	editor=""/>

<tags
	ms.service="active-directory"
	ms.workload="identity"
	ms.tgt_pltfrm="na"
	ms.devlang="na"
	ms.topic="article"
	ms.date="08/05/2016"
	ms.author="jeedes"/>


# Руководство. Интеграция Azure Active Directory с UserEcho

Цель этого учебника — показать, как интегрировать Azure Active Directory (Azure AD) с приложением UserEcho. Интеграция Azure AD с приложением UserEcho обеспечивает следующие преимущества.

- С помощью Azure AD вы можете контролировать доступ к приложению UserEcho.
- Вы можете включить автоматический вход пользователей в UserEcho (единый вход) с учетной записью Azure AD.
- Вы можете управлять учетными записями централизованно — через классический портал Azure.

Подробнее узнать об интеграции приложений SaaS с Azure AD можно в статье [Что такое доступ к приложениям и единый вход с помощью Azure Active Directory?](active-directory-appssoaccess-whatis.md).

## Предварительные требования 

Чтобы настроить интеграцию Azure AD с приложением UserEcho, вам потребуется следующее:

- подписка Azure AD;
- подписка UserEcho с поддержкой единого входа.


> [AZURE.NOTE] Мы не рекомендуем использовать рабочую среду для проверки действий в этом учебнике.


При проверке действий в этом учебнике соблюдайте следующие рекомендации:

- Не следует использовать рабочую среду при отсутствии необходимости.
- Если у вас нет пробной среды Azure AD, вы можете получить пробную версию на один месяц по [этой ссылке](https://azure.microsoft.com/pricing/free-trial/).

 
## Описание сценария
Цель этого учебника — научить вас проверять единый вход в Azure AD в пробной среде. Сценарий, описанный в этом учебнике, состоит из двух основных блоков:

1. Добавление UserEcho из коллекции.
2. Настройка и проверка единого входа в Azure AD


## Добавление UserEcho из коллекции
Чтобы настроить интеграцию UserEcho с Azure AD, необходимо добавить UserEcho из коллекции в список управляемых приложений SaaS.

**Чтобы добавить UserEcho из коллекции, выполните следующие действия.**

1. На **классическом портале Azure** в области навигации слева щелкните **Active Directory**.

	.![Active Directory][1]

2. Из списка **Каталог** выберите каталог, для которого нужно включить интеграцию каталогов.

3. Чтобы открыть представление приложений, в представлении каталога нажмите **Приложения** в верхнем меню.

	.![Приложения][2]

4. В нижней части страницы нажмите кнопку **Добавить**.

	.![Приложения][3]

5. В диалоговом окне **Что необходимо сделать?** нажмите **Добавить приложение из коллекции**.

	![Приложения][4]

6. В поле поиска введите **UserEcho**.

	.![Создание тестового пользователя Azure AD](./media/active-directory-saas-userecho-tutorial/tutorial_userecho_01.png)

7. В области результатов выберите **UserEcho** и нажмите кнопку **Завершить**, чтобы добавить приложение.

	.![Создание тестового пользователя Azure AD](./media/active-directory-saas-userecho-tutorial/tutorial_userecho_02.png)

##  Настройка и проверка единого входа в Azure AD
Цель этого раздела — показать, как настроить и проверить единый вход Azure AD в UserEcho с использованием тестового пользователя Britta Simon.

Для настройки единого входа в Azure AD необходимо знать, какой пользователь в UserEcho соответствует пользователю в Azure AD. Иными словами, необходимо установить связь между пользователем Azure AD и соответствующим пользователем в UserEcho. Чтобы установить эту связь, следует указать **имя пользователя** в Azure AD в качестве значения **имени пользователя** в UserEcho.
 
Чтобы настроить и проверить единый вход Azure AD в UserEcho, вам потребуется выполнить действия в следующих стандартных блоках.

1. **[Настройка единого входа Azure AD](#configuring-azure-ad-single-single-sign-on)**. Необходима, чтобы пользователи могли использовать эту функцию.
2. **[Создание тестового пользователя Azure AD](#creating-an-azure-ad-test-user)**. Требуется для проверки работы единого входа Azure AD от имени пользователя Britta Simon.
4. **[Создание тестового пользователя UserEcho](#creating-a-userecho-test-user)** требуется для создания в UserEcho пользователя Britta Simon, связанного с соответствующим пользователем в Azure AD.
5. **[Назначение тестового пользователя Azure AD](#assigning-the-azure-ad-test-user)** необходимо, чтобы позволить Britta Simon использовать единый вход в Azure AD.
5. **[Проверка единого входа](#testing-single-sign-on)** необходима, чтобы убедиться в корректной работе конфигурации.

### Настройка единого входа в Azure AD

Цель этого раздела — включить единый вход Azure AD на классическом портале Azure и настроить единый вход в приложение UserEcho.




**Чтобы настроить единый вход Azure AD в UserEcho, выполните следующие действия.**

1. На классическом портале Azure на странице интеграции с приложением **UserEcho** щелкните **Настройка единого входа**, чтобы открыть диалоговое окно **Настройка единого входа**.

	.![Настройка единого входа][6]

2. На странице **Как пользователи должны входить в UserEcho?** выберите **Единый вход Azure AD** и нажмите кнопку **Далее**.

	.![Настройка единого входа](./media/active-directory-saas-userecho-tutorial/tutorial_userecho_03.png)

3. В диалоговом окне на странице **Настройка параметров приложения** выполните следующие действия.

	![Настройка единого входа](./media/active-directory-saas-userecho-tutorial/tutorial_userecho_04.png)

    а. В текстовом поле **URL-адрес для входа** введите URL-адрес, используемый для входа в приложение UserEcho (например, *https://fabrikam.UserEcho.com/*).

    b. Нажмите кнопку **Далее**.
 
 
4. На странице **Настройка единого входа в UserEcho** выполните следующие действия.

	![Настройка единого входа](./media/active-directory-saas-userecho-tutorial/tutorial_userecho_05.png)

    а. Нажмите **Загрузить сертификат** и сохраните файл сертификата на свой компьютер.

    b. Нажмите кнопку **Далее**.


1. В другом окне браузера войдите на корпоративный сайт UserEcho с правами администратора.

1. На панели инструментов вверху щелкните имя пользователя, чтобы развернуть меню, а затем щелкните **Setup** (Настройка).

	.![Настройка единого входа](./media/active-directory-saas-userecho-tutorial/tutorial_userecho_06.png)

1. Щелкните **Integrations** (Интеграция).

	.![Настройка единого входа](./media/active-directory-saas-userecho-tutorial/tutorial_userecho_07.png)


1. Щелкните **Website** (Веб-сайт), а затем — **Single sign-on (SAML2)** (Единый вход (SAML2)).

	.![Настройка единого входа](./media/active-directory-saas-userecho-tutorial/tutorial_userecho_08.png)


1. На странице **Single sign-on (SAML)** (Единый вход (SAML)) выполните следующие действия.

	![Настройка единого входа](./media/active-directory-saas-userecho-tutorial/tutorial_userecho_09.png)

    а. В поле **SAML-enabled** (Включить SAML) выберите **Yes** (Да).

    b. На классическом портале Azure на странице диалогового окна "Настройка единого входа в UserEcho" скопируйте значение поля **URL-адрес службы единого входа** и вставьте его в текстовое поле **URL-адрес единого входа SAML**.

    c. На классическом портале Azure на странице диалогового окна "Настройка единого входа в UserEcho" скопируйте значение поля **URL-адрес удаленного выхода** и вставьте его в текстовое поле **URL-адрес удаленного выхода**.

    г) Откройте скачанный сертификат в блокноте, а затем скопируйте и вставьте его содержимое в текстовое поле **X.509 Certificate** (Сертификат X.509).

    д. Щелкните **Сохранить**.


6. На классическом портале Azure выберите подтверждение конфигурации единого входа и нажмите кнопку **Далее**.

	.![Единый вход в Azure AD][10]

7. На странице **Подтверждение единого входа** нажмите кнопку **Завершить**.

	.![Единый вход в Azure AD][11]




### Создание тестового пользователя Azure AD
Цель этого раздела — создать на классическом портале Azure тестового пользователя с именем Britta Simon.

![Создание пользователя Azure AD][20]

**Чтобы создать тестового пользователя в Azure AD, выполните следующие действия:**

1. На **классическом портале Azure** в области навигации слева щелкните **Active Directory**.

	.![Создание тестового пользователя Azure AD](./media/active-directory-saas-userecho-tutorial/create_aaduser_09.png)

2. Из списка **Каталог** выберите каталог, для которого нужно включить интеграцию каталогов.

3. Чтобы вывести на экран список пользователей, в меню вверху выберите **Пользователи**.

	.![Создание тестового пользователя Azure AD](./media/active-directory-saas-userecho-tutorial/create_aaduser_03.png)
 
4. Чтобы открыть диалоговое окно **Добавление пользователя**, на панели инструментов внизу щелкните **Добавить пользователя**.

	![Создание тестового пользователя Azure AD](./media/active-directory-saas-userecho-tutorial/create_aaduser_04.png)

5. На странице диалогового окна **Тип учетной записи пользователя** выполните следующие действия.

	.![Создание тестового пользователя Azure AD](./media/active-directory-saas-userecho-tutorial/create_aaduser_05.png)

    а. В поле «Тип пользователя» выберите значение «Новый пользователь в вашей организации».

    b. В текстовое поле **Имя пользователя** введите **BrittaSimon**.

    c. Нажмите кнопку **Далее**.

6.  На странице диалогового окна **Профиль пользователя** выполните следующие действия.

	.![Создание тестового пользователя Azure AD](./media/active-directory-saas-userecho-tutorial/create_aaduser_06.png)
 
    а. В текстовом поле **Имя** введите **Britta**.

    b. В текстовое поле **Фамилия** введите **Simon**.

    c. В текстовое поле **Отображаемое имя** введите **Britta Simon**.

    г) В списке **Роль** выберите **Пользователь**. e. Нажмите кнопку **Далее**.

7. На странице диалогового окна **Получить временный пароль** нажмите кнопку **Создать**.

	![Создание тестового пользователя Azure AD](./media/active-directory-saas-userecho-tutorial/create_aaduser_07.png)
 
8. На диалоговой странице **Получение временного пароля** выполните следующие действия:

	.![Создание тестового пользователя Azure AD](./media/active-directory-saas-userecho-tutorial/create_aaduser_08.png)
  
    а. Запишите значение поля **Новый пароль**.

    b. Нажмите **Завершено**.

  
 
### Создание тестового пользователя UserEcho

Цель этого раздела — создать в приложении UserEcho пользователя с именем Britta Simon.

**Чтобы создать пользователя с именем Britta Simon в UserEcho, выполните следующие действия.**

1. Войдите на корпоративный сайт UserEcho с правами администратора.

1. На панели инструментов вверху щелкните имя пользователя, чтобы развернуть меню, а затем щелкните **Setup** (Настройка).

	.![Настройка единого входа](./media/active-directory-saas-userecho-tutorial/tutorial_userecho_06.png)

1. Щелкните **Users** (Пользователи), чтобы развернуть раздел **Users** (Пользователи).

	.![Настройка единого входа](./media/active-directory-saas-userecho-tutorial/tutorial_userecho_10.png)

1. Выберите раздел **Пользователи**.

	.![Настройка единого входа](./media/active-directory-saas-userecho-tutorial/tutorial_userecho_11.png)

1. Щелкните **Invite a new user** (Пригласить нового пользователя).

	![Настройка единого входа](./media/active-directory-saas-userecho-tutorial/tutorial_userecho_12.png)


1. В диалоговом окне **Invite a new user** (Приглашение нового пользователя) выполните следующие действия.

	.![Настройка единого входа](./media/active-directory-saas-userecho-tutorial/tutorial_userecho_13.png)

    а. В текстовом поле **Name** (Имя) введите **Britta Simon**.

    b. В текстовое поле **Электронный адрес** введите электронный адрес пользователя Britta Simon на классическом портале Azure.

    c. Щелкните **Invite** (Пригласить).

Пользователю Britta будет отправлено приглашение, с помощью которого можно начать работу с приложением UserEcho.



### Назначение тестового пользователя Azure AD

Цель этого раздела — разрешить пользователю Britta Simon использовать единый вход Azure, предоставив ей доступ к UserEcho.

![Назначение пользователя][200]

**Чтобы назначить пользователя Britta Simon приложению UserEcho, выполните следующие действия.**

1. Чтобы открыть представление приложений, на классическом портале Azure в представлении каталога щелкните **Приложения** в меню вверху.

	.![Назначение пользователя][201]

2. Из списка приложений выберите **UserEcho**.

	.![Настройка единого входа](./media/active-directory-saas-userecho-tutorial/tutorial_userecho_50.png)

1. В меню в верхней части страницы щелкните **Пользователи**.

	.![Назначение пользователя][203]

1. В списке пользователей выберите **Britta Simon**.

2. На панели инструментов внизу щелкните **Назначить**.

	.![Назначение пользователя][205]



### Проверка единого входа

Цель этого раздела — проверить конфигурацию единого входа Azure AD с помощью панели доступа. Щелкнув элемент UserEcho на панели доступа, вы автоматически войдете в приложение UserEcho.


## дополнительные ресурсы.

* [Список учебников по интеграции приложений SaaS с Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Что такое доступ к приложениям и единый вход с помощью Azure Active Directory?](active-directory-appssoaccess-whatis.md)


.<!--Image references-->

[1]: ./media/active-directory-saas-userecho-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-userecho-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-userecho-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-userecho-tutorial/tutorial_general_04.png

[6]: ./media/active-directory-saas-userecho-tutorial/tutorial_general_05.png
[10]: ./media/active-directory-saas-userecho-tutorial/tutorial_general_06.png
[11]: ./media/active-directory-saas-userecho-tutorial/tutorial_general_07.png
[20]: ./media/active-directory-saas-userecho-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-userecho-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-userecho-tutorial/tutorial_general_201.png
[203]: ./media/active-directory-saas-userecho-tutorial/tutorial_general_203.png
[204]: ./media/active-directory-saas-userecho-tutorial/tutorial_general_204.png
[205]: ./media/active-directory-saas-userecho-tutorial/tutorial_general_205.png

<!---HONumber=AcomDC_0810_2016-->