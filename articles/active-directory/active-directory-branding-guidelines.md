<properties
   pageTitle="Рекомендации по фирменной символике для приложений | Microsoft Azure"
   description="Полное руководство по ориентированным на разработчиков ресурсам для Azure Active Directory"
   services="active-directory"
   documentationCenter="dev-center-name"
   authors="msmbaldwin"
   manager="mbaldwin"
   editor=""/>

<tags
   ms.service="active-directory"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="identity"
   ms.date="06/23/2016"
   ms.author="mbaldwin"/>


# Рекомендации по фирменной символике для приложений


В этом разделе рассматриваются рекомендации по фирменной символике, которыми следует пользоваться при разработке приложений в среде Azure Active Directory (Azure AD). Эти рекомендации позволят управлять пользователями, работающими со своей рабочей или учебной учетной записью, управляемой в Azure AD, или личной учетной записью для регистрации или входа в ваше приложение.

## Личные учетные записи и рабочие учетные записи Майкрософт

Корпорация Майкрософт управляет двумя платформами идентификации:

- **Личные учетные записи** (ранее известные как Windows Live ID) Эти учетные записи представляют взаимоотношения между *отдельными* пользователями и корпорацией Майкрософт и используются для доступа к службам и устройствам Майкрософт. Они предназначены для личного пользования.

- **Рабочие учетные записи** Управление этими учетными записями осуществляется корпорацией Майкрософт от имени организаций, использующих Azure Active Directory. Эти учетные записи используются для входа в Office 365 и другие бизнес-службы, предоставляемые корпорацией Майкрософт.

Рабочие учетные записи Майкрософт обычно присваиваются конечным пользователям (сотрудникам, студентам, федеральным служащим) их организациями (компанией, школой, государственным учреждением). Эти учетные записи можно создавать напрямую в облаке, Azure AD или синхронизировать из локального каталога, например Windows Server Active Directory. Корпорация Майкрософт является *хранителем* учетных записей, размещенных в Azure AD, но право владения и управления принадлежит организации.

## Ссылки на учетные записи Azure AD в приложении

Майкрософт не показывает конечным пользователям торговое наименование Active Directory, и вам тоже не следует этого делать.

- После входа пользователя следует как можно больше использовать наименование и логотип организации. Это предпочтительнее, чем использовать такие общие термины, как «ваша организация».

- Пока вход не выполнен, следует обозначить учетные записи пользователей как «рабочие учетные записи» и использовать логотип Майкрософт, чтобы указать на то, что эти учетные записи управляются корпорацией Майкрософт. Не используйте такие фразы, как “учетная запись предприятия”, “деловая учетная запись” или “корпоративная учетная запись”, так как возможна путаница.

## Пиктограмма учетной записи пользователя
В более ранней версии данного руководства рекомендуется использование пиктограммы «синего значка». На основании отзывов пользователей и разработчиков теперь вместо него рекомендуется использовать логотип Майкрософт. Таким образом пользователи поймут, что можно использовать имеющуюся учетную запись Office 365 или других бизнес-служб Майкрософт.

## Регистрация и вход в Azure AD

Приложение может предлагать отдельные способы регистрации и входа, и в следующем разделе наглядно показаны оба сценария.

**Если в приложении поддерживается регистрация пользователей (например, бесплатная пробная или условно-бесплатная версия)**: можно отобразить кнопку **Вход**. С ее помощью пользователи смогут получить доступ к приложению, используя свою рабочую или личную учетную запись. Azure AD отобразит запрос на продолжение при первом доступе к приложению.

**Если приложению требуются разрешения, которые может предоставить только администратор или необходимо лицензирование организацией**: следует разделять получение администраторских прав и вход пользователя в систему. Кнопка **«получить приложение»** будет перенаправлять администратора на вход, а затем потребуется предоставить разрешение от имени пользователя организации. Дополнительное преимущество такого подхода — это препятствие выводу запросов на продолжение со стороны конечных пользователей приложения.

## Визуальная подсказка для регистрации

Ссылка «получить приложение» должна перенаправлять пользователя страницу предоставления доступа (авторизации) Azure AD, чтобы администратор организации мог авторизовать приложение для получения доступа к данным организации, размещенным у Майкрософт. Сведения о том, как запросить доступ см. в статье [Интеграция приложений с Azure Active Directory](active-directory-integrating-applications.md).

После получения в приложении разрешения от администраторов его могут добавить в средство запуска приложений Office 365 (доступно из средства запуска приложений и [https://portal.office.com/myapps](https://portal.office.com/myapps)). Если требуется привлечь внимание к этой возможности, можно использовать описание наподобие «Добавить приложение для моей организации» и отобразить кнопку следующим образом:

![Типы приложений и сценарии](./media/active-directory-branding-guidelines/add-to-my-org.png)

Тем не менее рекомендуется написать пояснение, а не полагаться на кнопки. Например:
> *Если вы уже используете Office 365 или другие бизнес-службы Майкрософт, можно просто предоставить для <имя\_вашего\_приложения> доступ к корпоративным данным. Это позволит пользователям получить доступ к <имя\_вашего\_приложения> с помощью существующих рабочих учетных записей.*


## Визуальная подсказка для входа
В приложении должна отображаться кнопка входа, которая перенаправляет пользователей на конченную точку входа, соответствующую протоколу, который можно использовать для интеграции с Azure AD. Следующий раздел содержит сведения о том, как эта кнопка должна выглядеть.

### Пиктограмма и элемент управления "Войти в Майкрософт"
Именно комбинация логотипа Майкрософт и текста "Войти в Майкрософт" отличает Azure AD от других поставщиков удостоверений, которые могут поддерживаться в вашем приложении. Если недостаточно места для фразы "Войти в Майкрософт", можно сократить текст до одного слова "Войти".

![Типы приложений и сценарии](./media/active-directory-branding-guidelines/sign-in-with-microsoft-light.png)

![Типы приложений и сценарии](./media/active-directory-branding-guidelines/sign-in-light.png)

Также для кнопок можно использовать темную цветовую схему.

![Типы приложений и сценарии](./media/active-directory-branding-guidelines/sign-in-with-microsoft-dark.png)

![Типы приложений и сценарии](./media/active-directory-branding-guidelines/sign-in-dark.png)

## Что следует и что не следует делать с фирменно символикой

**Следует** использовать фразу "рабочая или учебная учетная запись" в комбинации с кнопкой "Войти в Майкрософт", чтобы помочь пользователям понять, смогут ли они воспользоваться этим элементом управления. **Не следует** использовать такие фразы, как "учетная запись предприятия", "деловая учетная запись" или "корпоративная учетная запись".

**Не** используйте фразы «Идентификатор Office 365» или «Идентификатор Azure». Office 365 также является наименованием предложения для клиентов от корпорации Майкрософт, которое не использует Azure AD для проверки подлинности.

**Не** изменяйте логотип Майкрософт.

**Не** отображайте для конечных пользователей торговые наименования Azure или Active Directory. Однако допускается использование этого термина в общении с разработчикам, ИТ-специалистами и администраторами.

## Что следует и что не следует делать с навигацией

**Следует** предоставить пользователям способ выйти из системы и переключиться на другую учетную запись. Хотя многие имеют только одну личную учетную запись в Майкрософт/Facebook/Google/Twitter, зачастую пользователи связаны с несколькими организациями. Поддержка одновременного входа нескольких пользователей скоро появится.

<!---HONumber=AcomDC_0629_2016-->