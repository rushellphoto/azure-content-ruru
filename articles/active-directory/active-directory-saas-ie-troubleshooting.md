<properties
    pageTitle="Устранение неполадок, связанных с расширением панели доступа для Internet Explorer | Microsoft Azure"
    description="Как применить групповую политику для развертывания надстройки Internet Explorer для работы с порталом «Мои приложения»."
    services="active-directory"
    documentationCenter=""
    authors="MarkusVi"
    manager="stevenpo"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="identity"
    ms.date="05/19/2016"
    ms.author="markvi;liviodlc"/>

#Устранение неполадок, связанных с расширением панели доступа для Internet Explorer

Эта статья поможет устранить следующие проблемы.

- Не удается получить доступ к приложениям через портал «Мои приложения» при использовании Internet Explorer.
- Отображается сообщение «Установка программного обеспечения», хотя программное обеспечение уже установлено.

Если вы являетесь администратором, также прочтите статью [Развертывание расширения панели доступа для Internet Explorer с помощью групповой политики](active-directory-saas-ie-group-policy.md).

##Запуск средства диагностики

Для диагностики проблем с установкой расширения панели доступа загрузите и запустите средство диагностики панели доступа.

1. [Щелкните здесь, чтобы загрузить средство диагностики.](https://account.activedirectory.windowsazure.com/applications/AccessPanelExtensionDiagnosticTool/AccessPanelExtensionDiagnosticTool.zip)

2. Откройте файл и нажмите кнопку **Извлечь все**.

	![Нажмите кнопку «Извлечь все».](./media/active-directory-saas-ie-troubleshooting/extract1.png)

3. Нажмите кнопку **Извлечь**, чтобы продолжить.

	![Нажмите кнопку «Извлечь».](./media/active-directory-saas-ie-troubleshooting/extract2.png)

4. Чтобы запустить средство, щелкните правой кнопкой мыши файл с именем **AccessPanelExtensionDiagnosticTool**, а затем выберите пункты **Открыть с помощью > Сервер сценариев на базе Microsoft Windows**.

	![Открыть с помощью > Сервер сценариев на базе Microsoft Windows](./media/active-directory-saas-ie-troubleshooting/open_tool.png)

5. Откроется следующее окно диагностики, описывающее возможные проблемы с вашей установкой.

	![Образец окна диагностики](./media/active-directory-saas-ie-troubleshooting/tool_preview.png)

6. Нажмите кнопку **ДА**, чтобы программа могла решить обнаруженные проблемы.

7. Чтобы сохранить эти изменения, закройте все окна Internet Explorer и откройте браузер снова.<br />Если приложения по-прежнему недоступны, попробуйте выполнить описанные ниже действия.

##Убедитесь, что расширение панели доступа включено.

Чтобы убедиться, что расширение панели доступа в Internet Explorer включено, выполните описанные ниже действия.

1. В правом верхнем углу Internet Explorer щелкните **значок шестеренки**. Выберите **параметры обозревателя**.<br />(В более ранних версиях Internet Explorer они находятся в разделе **Сервис > Параметры Интернета**.)

	![Выберите пункты Сервис > Параметры Интернета.](./media/active-directory-saas-ie-troubleshooting/internetoptions.png)

2. Откройте вкладку **Программы** и нажмите кнопку **Управление надстройками**.

	![Щелкните «Управление надстройками».](./media/active-directory-saas-ie-troubleshooting/internetoptions_programs.png)

3. В этом диалоговом окне выберите **Расширение панели доступа** и нажмите кнопку **Включить**.

	![Щелкните «Включить».](./media/active-directory-saas-ie-troubleshooting/enableaddon.png)

4. Чтобы сохранить эти изменения, закройте все окна Internet Explorer и откройте браузер снова.

##Включение расширений для просмотра InPrivate

Если используется режим просмотра InPrivate, выполните следующие действия.

1. В правом верхнем углу Internet Explorer щелкните **значок шестеренки**. Выберите **параметры обозревателя**.<br />(В более ранних версиях Internet Explorer они находятся в разделе **Сервис > Параметры Интернета**.)

	![Образец окна диагностики](./media/active-directory-saas-ie-troubleshooting/inprivateoptions.png)

2. Откройте вкладку **Конфиденциальность**, **снимите** флажок **Отключить панели инструментов и расширения при запуске просмотра InPrivate**</p>.

	![Снимите флажок «Отключить панели инструментов и расширения при запуске просмотра InPrivate».](./media/active-directory-saas-ie-troubleshooting/enabletoolbars.png)

3. Чтобы сохранить эти изменения, закройте все окна Internet Explorer и откройте браузер снова.

##Удаление расширения панели доступа

Чтобы удалить расширение панели доступа с компьютера, выполните следующие действия.

1. Нажмите на клавиатуре **клавишу Windows**, чтобы открыть меню «Пуск». Открыв меню, введите параметры поиска. Введите «Панель управления», а затем откройте **Панель управления**, когда она появится в результатах поиска.

	![Поиск панели управления](./media/active-directory-saas-ie-troubleshooting/search_sm.png)

2. В правом верхнем углу панели управления выберите для параметра **Просмотр** значение **Крупные значки**. Затем найдите и нажмите кнопку **Программы и компоненты**.

	![Измените представление для отображения крупных значков.](./media/active-directory-saas-ie-troubleshooting/control_panel.png)

3. Выберите в списке **Расширение панели доступа** и нажмите кнопку **Удалить**.

	![Нажмите кнопку удаления.](./media/active-directory-saas-ie-troubleshooting/uninstall.png)

4. Затем попробуйте установить расширение еще раз, чтобы проверить, удалось ли решить проблему.

Если при удалении расширения возникнут проблемы, удалите его с помощью инструмента [Microsoft Fix It](https://go.microsoft.com/?linkid=9779673).

## Связанные статьи

- [Указатель статьей по управлению приложениями в Azure Active Directory](active-directory-apps-index.md)
- [Доступ к приложениям и единый вход с помощью Azure Active Directory](active-directory-appssoaccess-whatis.md)
- [Развертывание расширения панели доступа для Internet Explorer с помощью групповой политики](active-directory-saas-ie-group-policy.md)

<!---HONumber=AcomDC_0525_2016-->