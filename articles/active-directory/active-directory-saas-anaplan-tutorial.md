<properties
	pageTitle="Руководство. Интеграция Azure Active Directory с Anaplan | Microsoft Azure"
	description="Узнайте, как настроить единый вход между Azure Active Directory и Anaplan."
	services="active-directory"
	documentationCenter=""
	authors="jeevansd"
	manager="femila"
	editor=""/>

<tags
	ms.service="active-directory"
	ms.workload="identity"
	ms.tgt_pltfrm="na"
	ms.devlang="na"
	ms.topic="article"
	ms.date="07/19/2016"
	ms.author="jeedes"/>


# Руководство. Интеграция Azure Active Directory с Anaplan

Цель этого руководства — показать, как интегрировать Azure Active Directory (Azure AD) с приложением Anaplan.

Интеграция Azure AD с приложением Anaplan обеспечивает следующие преимущества.

- С помощью Azure AD вы можете контролировать доступ к Anaplan.
- Вы можете включить автоматический вход пользователей в Anaplan (единый вход) с учетной записью Azure AD.
- Вы можете управлять учетными записями централизованно — через классический портал Azure.

Подробнее узнать об интеграции приложений SaaS с Azure AD можно в статье [Что такое доступ к приложениям и единый вход с помощью Azure Active Directory?](active-directory-appssoaccess-whatis.md).

## Предварительные требования

Чтобы настроить интеграцию Azure AD с Anaplan, вам потребуется:

- подписка Azure AD;
- подписка Anaplan с поддержкой единого входа.


> [AZURE.NOTE] Мы не рекомендуем использовать рабочую среду для проверки действий в этом учебнике.


При проверке действий в этом учебнике соблюдайте следующие рекомендации:

- Не следует использовать рабочую среду при отсутствии необходимости.
- Если у вас нет пробной среды Azure AD, вы можете получить пробную версию на один месяц по [этой ссылке](https://azure.microsoft.com/pricing/free-trial/).


## Описание сценария
Цель этого учебника — научить вас проверять единый вход в Azure AD в пробной среде.

Сценарий, описанный в этом учебнике, состоит из двух основных блоков:

1. Добавление Anaplan из коллекции.
2. Настройка и проверка единого входа в Azure AD


## Добавление Anaplan из коллекции
Чтобы настроить интеграцию Anaplan с Azure AD, необходимо добавить Anaplan из коллекции в список управляемых приложений SaaS.

**Чтобы добавить Anaplan из коллекции, сделайте следующее:**

1. На **классическом портале Azure** в области навигации слева щелкните **Active Directory**.

	![Active Directory][1]

2. Из списка **Каталог** выберите каталог, для которого нужно включить интеграцию каталогов.

3. Чтобы открыть представление приложений, в представлении каталога нажмите **Приложения** в верхнем меню.
	
	![Приложения][2]

4. В нижней части страницы нажмите кнопку **Добавить**.

	![Приложения][3]

5. В диалоговом окне **Что необходимо сделать?** нажмите **Добавить приложение из коллекции**.

	![Приложения][4]

6. В поле поиска введите **Anaplan**.

	![Создание тестового пользователя Azure AD](./media/active-directory-saas-anaplan-tutorial/tutorial_anaplan_01.png)
7. В области результатов выберите **Anaplan** и нажмите кнопку **Завершить**, чтобы добавить приложение.

	![Выбор приложения в коллекции](./media/active-directory-saas-anaplan-tutorial/tutorial_anaplan_001.png)


##  Настройка и проверка единого входа в Azure AD
Цель этого раздела — показать, как настроить и проверить единый вход Azure AD в Anaplan с использованием тестового пользователя Britta Simon.

Для работы единого входа в Azure AD необходимо знать, какой пользователь в Anaplan соответствует пользователю в Azure AD. Иными словами, необходимо установить связь между пользователем Azure AD и соответствующим пользователем в Anaplan.

Чтобы установить эту связь, следует назначить **имя пользователя** в Azure AD в качестве значения **имени пользователя** в Anaplan.

Чтобы настроить и проверить единый вход Azure AD в Anaplan, вам потребуется выполнить действия в следующих стандартных блоках.

1. **[Настройка единого входа Azure AD](#configuring-azure-ad-single-single-sign-on)** необходима, чтобы пользователи могли использовать эту функцию.
2. **[Создание тестового пользователя Azure AD](#creating-an-azure-ad-test-user)** требуется для проверки работы единого входа Azure AD от имени пользователя Britta Simon.
3. **[Создание тестового пользователя Anaplan](#creating-a-anaplan-test-user)** требуется для создания в Anaplan пользователя Britta Simon, связанного с представлением этого же пользователя в Azure AD.
4. **[Назначение тестового пользователя Azure AD](#assigning-the-azure-ad-test-user)** необходимо, чтобы позволить Britta Simon использовать единый вход в Azure AD.
5. **[Проверка единого входа](#testing-single-sign-on)** необходима, чтобы убедиться в корректной работе конфигурации.

### Настройка единого входа в Azure AD

В данном разделе описано, как включить единый вход Azure AD на классическом портале и настроить его в приложении Anaplan.

**Чтобы настроить единый вход Azure AD в Anaplan, сделайте следующее:**

1. На классическом портале на странице интеграции с приложением **Anaplan** щелкните **Настройка единого входа**, чтобы открыть диалоговое окно **Настройка единого входа**.
	 
	![Настройка единого входа][6]

2. На странице **How would you like users to sign on to Anaplan** (Как пользователи должны входить в Anaplan?) выберите **Единый вход Azure AD** и нажмите кнопку **Далее**.

	![Настройка единого входа](./media/active-directory-saas-anaplan-tutorial/tutorial_anaplan_03.png)

3. На диалоговой странице **Configure App Settings** (Настройка параметров приложения) выполните следующие действия, а затем нажмите кнопку **Далее**.

	![Настройка единого входа](./media/active-directory-saas-anaplan-tutorial/tutorial_anaplan_04.png)

    а. В текстовом поле **URL-адрес входа** введите URL-адрес в следующем формате: `https://sdp.anaplan.com/frontdoor/saml/<tenant name>`.

	b. Нажмите кнопку **Далее**.


	> [AZURE.NOTE] В этом руководстве значение URL-адреса входа — просто заполнитель. Чтобы получить фактическое значение для своей среды, обратитесь в службу поддержки Anaplan.


4. На странице **Configure single sign-on at Anaplan** (Настройка единого входа в Anaplan) выполните следующие действия и нажмите кнопку **Далее**.

	![Настройка единого входа](./media/active-directory-saas-anaplan-tutorial/tutorial_anaplan_05.png)

    а. Нажмите **Загрузить метаданные** и сохраните файл на свой компьютер.

    b. Нажмите кнопку **Далее**.




5. Чтобы настроить единый вход для своего приложения, обратитесь в службу поддержки Anaplan по адресу [support@anaplan.com](mailto:support@anaplan.com) и предоставьте следующие сведения:

    - скачанный файл метаданных;

    - **идентификатор сущности**;

    - **URL-адрес единого входа SAML**;

    - **URL-адрес службы единого выхода**.





6. На классическом портале выберите подтверждение конфигурации единого входа и нажмите кнопку **Далее**.

	![Единый вход в Azure AD][10]

7. На странице **Подтверждение единого входа** нажмите кнопку **Завершить**.

	![Единый вход в Azure AD][11]



### Создание тестового пользователя Azure AD
Цель этого раздела — создать на классическом портале тестового пользователя с именем Britta Simon.
	
![Создание пользователя Azure AD][20]

**Чтобы создать тестового пользователя в Azure AD, выполните следующие действия:**

1. На **классическом портале Azure** в области навигации слева щелкните **Active Directory**.

	![Создание тестового пользователя Azure AD](./media/active-directory-saas-anaplan-tutorial/create_aaduser_09.png)

2. Из списка **Каталог** выберите каталог, для которого нужно включить интеграцию каталогов.

3. Чтобы вывести на экран список пользователей, в меню вверху выберите **Пользователи**.
	
	![Создание тестового пользователя Azure AD](./media/active-directory-saas-anaplan-tutorial/create_aaduser_03.png)

4. Чтобы открыть диалоговое окно **Добавление пользователя**, на панели инструментов внизу щелкните **Добавить пользователя**.
	
	![Создание тестового пользователя Azure AD](./media/active-directory-saas-anaplan-tutorial/create_aaduser_04.png)

5. На диалоговой странице **Тип учетной записи пользователя** выполните следующие действия:

	![Создание тестового пользователя Azure AD](./media/active-directory-saas-anaplan-tutorial/create_aaduser_05.png)

    а. В поле «Тип пользователя» выберите значение «Новый пользователь в вашей организации».

    b. В текстовое поле **Имя пользователя** введите **BrittaSimon**.

    c. Нажмите кнопку **Далее**.

6.  На диалоговой странице **Профиль пользователя** выполните следующие действия:
	
	![Создание тестового пользователя Azure AD](./media/active-directory-saas-anaplan-tutorial/create_aaduser_06.png)

    а. В текстовом поле **Имя** введите **Britta**.

    b. В текстовое поле **Фамилия** введите **Simon**.

    c. В текстовое поле **Отображаемое имя** введите **Britta Simon**.

    г) В списке **Роль** выберите **Пользователь**.

    д. Нажмите кнопку **Далее**.

7. На диалоговой странице **Получить временный пароль** нажмите кнопку **Создать**.
	
	![Создание тестового пользователя Azure AD](./media/active-directory-saas-anaplan-tutorial/create_aaduser_07.png)

8. На диалоговой странице **Получить временный пароль** сделайте следующее:
	
	![Создание тестового пользователя Azure AD](./media/active-directory-saas-anaplan-tutorial/create_aaduser_08.png)

    а. Запишите значение поля **Новый пароль**.

    b. Нажмите **Завершено**.



### Создание тестового пользователя Anaplan

В этом разделе описано, как создать пользователя Britta Simon в приложении Anaplan. Обратитесь в службу поддержки Anaplan по адресу <mailto:support@anaplan.com>, чтобы добавить пользователей на платформу Anaplan.


### Назначение тестового пользователя Azure AD

Цель этого раздела — разрешить пользователю Britta Simon использовать единый вход Azure путем предоставления доступа к Anaplan.
	
![Назначение пользователя][200]

**Чтобы назначить пользователя Britta Simon в Anaplan, сделайте следующее:**

1. Чтобы открыть представление приложений, в представлении каталога на классическом портале щелкните **Приложения** в верхнем меню.

	![Назначение пользователя][201]

2. В списке приложений выберите **Anaplan**.

	![Настройка единого входа](./media/active-directory-saas-anaplan-tutorial/tutorial_anaplan_50.png)

3. В меню в верхней части страницы щелкните **Пользователи**.
	
	![Назначение пользователя][203]

4. В списке пользователей выберите **Britta Simon**.

5. На панели инструментов внизу щелкните **Назначить**.

	![Назначение пользователя][205]



### Проверка единого входа

Цель этого раздела — проверить конфигурацию единого входа Azure AD с помощью панели доступа.

Щелкнув элемент Anaplan на панели доступа, вы автоматически войдете в приложение Anaplan.


## Дополнительные ресурсы

* [Список учебников по интеграции приложений SaaS с Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Что такое доступ к приложениям и единый вход с помощью Azure Active Directory?](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-anaplan-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-anaplan-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-anaplan-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-anaplan-tutorial/tutorial_general_04.png

[6]: ./media/active-directory-saas-anaplan-tutorial/tutorial_general_05.png
[10]: ./media/active-directory-saas-anaplan-tutorial/tutorial_general_06.png
[11]: ./media/active-directory-saas-anaplan-tutorial/tutorial_general_07.png
[20]: ./media/active-directory-saas-anaplan-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-anaplan-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-anaplan-tutorial/tutorial_general_201.png
[203]: ./media/active-directory-saas-anaplan-tutorial/tutorial_general_203.png
[204]: ./media/active-directory-saas-anaplan-tutorial/tutorial_general_204.png
[205]: ./media/active-directory-saas-anaplan-tutorial/tutorial_general_205.png

<!---HONumber=AcomDC_0720_2016-->