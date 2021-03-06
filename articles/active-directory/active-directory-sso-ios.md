<properties
	pageTitle="Включение единого входа в нескольких приложениях в iOS с помощью ADAL | Microsoft Azure"
	description="Использование функций ADAL SDK для включения единого входа в приложениях."
	services="active-directory"
	documentationCenter=""
	authors="brandwe"
	manager="mbaldwin"
	editor=""/>

<tags
	ms.service="active-directory"
	ms.workload="identity"
	ms.tgt_pltfrm="ios"
	ms.devlang="objective-c"
	ms.topic="article"
	ms.date="05/31/2016"
	ms.author="brandwe"/>


# Включение единого входа в нескольких приложениях iOS с помощью ADAL


Современные клиенты ожидают предоставления возможности единого входа, которая позволяет пользователям ввести учетные данные только один раз, после чего они смогут автоматически входить в разные приложения. Сложность ввода имени пользователя и пароля на маленьком экране, часто с дополнительной проверкой подлинности, например посредством звонка по телефону или кода в SMS, приводит к неудовлетворенности пользователя, если ему приходится это делать больше одного раза.

Кроме того, если вы работаете с платформой удостоверений, которую могут использовать другие приложения, включая учетные записи Майкрософт и рабочую учетную запись Office 365, пользователи ожидают, что эти учетные данные будут доступны во всех приложениях независимо от поставщика.

Платформа удостоверений (Майкрософт), а также пакет SDK для этой платформы делают всю сложную работу за вас, предоставляя пользователям возможность единого входа либо во все используемые ими приложения, либо (с помощью брокера и приложений проверки подлинности) на всем устройстве.

В этом пошаговом руководстве описывается, как настроить пакет SDK в приложении, чтобы ваши пользователи смогли воспользоваться этими преимуществами.

Руководство применимо при работе с такими службами:

* Azure Active Directory
* Azure Active Directory B2C
* Azure Active Directory B2B


Обратите внимание, что в документе ниже предполагается, что вы знаете о том, как [подготовить приложения на устаревшем портале для Azure Active Directory](active-directory-how-to-integrate.md), а также что вы интегрировали приложение в [Microsoft Identity iOS SDK](https://github.com/AzureAD/azure-activedirectory-library-for-objc).

## Концепции единого входа на платформе Microsoft Identity

### Брокеры Microsoft Identity

Корпорация Майкрософт предоставляет приложения для всех мобильных платформ, которые позволяют привязывать учетные данные к приложениям от разных поставщиков, а также разрешают использовать специальные расширенные компоненты, требующие единого безопасного расположения для проверки учетных данных. Они называются **брокеры**. В iOS и Android брокеры доступны в скачиваемых приложениях. Эти приложения пользователи устанавливают самостоятельно, либо их может передавать на устройство сотрудника компания, частично или полностью управляющая такими устройствами. Брокеры поддерживают управление безопасностью только для некоторых приложений или для всего устройства в зависимости от требований ИТ-администраторов. В Windows этот компонент предоставляется средством выбора учетной записи, встроенным в операционную систему (его техническое название — брокер веб-проверки подлинности).

Чтобы узнать, как мы используем эти брокеры и как клиенты могут увидеть их в потоке входа с помощью платформы Microsoft Identity, ознакомьтесь с дополнительными сведениями.

### Способы входа на мобильные устройства

Доступ к учетным данным на устройствах с поддержкой платформы Microsoft Identity осуществляется в рамках двух основных подходов:

* вход без брокера;
* вход с брокером.

#### Вход без брокера

Вход без брокера — это встроенная возможность входа в приложение с использованием локального хранилища на устройстве. Хотя доступ к этом хранилищу может осуществляться из других приложений, учетные данные строго привязаны к использующему их приложению или набору приложений. Такой подход вы, вероятно, использовали во многих мобильных приложениях, в которых имя пользователя и пароль вводятся непосредственно в приложении.

Такой способ входа отличается следующими преимуществами:

-  пользовательский интерфейс полностью реализован в приложении;
-  учетные данные можно передавать в приложения, подписанные с использованием одного и того же сертификата, что создает возможность единого входа во все ваши приложения; 
-  управление процедурой входа в рамках приложения возможно как до, так и после входа.

Такой способ входа имеет следующие недостатки:

- пользователь не может выполнять единый вход во всех приложениях, в которых используется компонент Microsoft Identity. Единый вход доступен только в приложениях с собственными настроенными компонентами Microsoft Identity;
- в приложении не удается использовать расширенные бизнес-функции и компоненты, включая условный доступ или набор продуктов InTune;
- приложение не поддерживает проверку подлинности бизнес-пользователей на основе сертификатов.

Ниже показано, как с помощью пакетов SDK для Microsoft Identity можно включить единый вход, используя общее хранилище приложений.

```
+------------+ +------------+  +-------------+
|            | |            |  |             |
|   App 1    | |   App 2    |  |   App 3     |
|            | |            |  |             |
|            | |            |  |             |
+------------+ +------------+  +-------------+
| Azure SDK  | | Azure SDK  |  | Azure SDK   |
+------------+-+------------+--+-------------+
|                                            |
|            App Shared Storage              |
+--------------------------------------------+
```

#### Вход с брокером

Вход с брокером — это вход, выполняемый в приложении брокера с использованием хранилища и системы безопасности брокера для предоставления учетных данных всем приложениям на устройстве, которые используют платформу Microsoft Identity. Это означает, что ваши приложения используют брокер для входа пользователей в систему. В iOS и Android брокеры доступны в скачиваемых приложениях. Эти приложения пользователи устанавливают самостоятельно, либо их может передавать на устройство сотрудника компания, управляющая такими устройствами. Пример этого типа приложений — приложение Azure Authenticator в iOS. В Windows этот компонент предоставляется средством выбора учетной записи, встроенным в операционную систему (его техническое название — брокер веб-проверки подлинности). Этот способ входа зависит от платформы. При неправильном управлении он может мешать работе пользователей. Возможно, вы знакомы с этим способом входа, если у вас установлено приложение Facebook, с помощью которого вы входите в другие приложения. Платформа Microsoft Identity использует этот же принцип.

В iOS это реализовано в виде анимированного "перехода", в рамках которого ваше приложение переходит на задний план, а приложения Azure Authenticator — на передний, что позволяет пользователю выбрать учетную запись для входа.

В Android и Windows для удобства пользователя средство выбора учетной записи отображается поверх приложения.

#### Способы вызова брокера

При установке совместимого брокера на устройство, например приложения Azure Authenticator, пакеты SDK Microsoft Identity автоматически будут выполнять вызов брокера, когда пользователь укажет, что хочет войти с помощью любой учетной записи платформы Microsoft Identity. Это может быть персональной учетной записью Майкрософт, рабочей или учебной учетной записью либо учетной записью, предоставленной и размещенной в Azure с использованием продуктов B2C и B2B. Надежные алгоритмы защиты и шифрование гарантируют, что учетные данные запрашиваются и возвращаются безопасным способом. Точные технические сведения об этих механизмах не публикуются, но в их разработке участвовали компании Apple и Google.

**Разработчик может выбрать, будет ли SDK Microsoft Identity вызывать брокера или использовать поток без помощи брокера.** Если разработчик решит не использовать поток с брокером, он не сможет предоставить пользователям преимущества единого входа с помощью учетных данных, уже добавленных на устройство. Кроме того, он не сможет включить в приложение такие бизнес-функции Майкрософт, как условный доступ, возможности управления InTune и проверка подлинности на основе сертификатов.

Такой способ входа отличается следующими преимуществами:

-  пользователь выполняет единый вход во всех приложениях независимо от поставщика;
-  в приложении можно использовать расширенные бизнес-функции и компоненты, включая условный доступ или набор продуктов InTune;
-  приложение может поддерживать проверку подлинности бизнес-пользователей на основе сертификатов;
- более безопасная процедура входа — удостоверение приложения и пользователь проверяются приложением брокера с использованием шифрования и дополнительных алгоритмов безопасности.

Такой способ входа имеет следующие недостатки:

- в iOS пользователь выходит из приложения во время выбора учетных данных;
- невозможность управления процедурой входа пользователей в приложении.



Ниже показано, как с помощью пакетов SDK для Microsoft Identity можно включить единый вход, используя приложение брокера.

```
+------------+ +------------+   +-------------+
|            | |            |   |             |
|   App 1    | |   App 2    |   |   Someone   |
|            | |            |   |    Else's   |
|            | |            |   |     App     |
+------------+ +------------+   +-------------+
| Azure SDK  | | Azure SDK  |   | Azure SDK   |
+-----+------+-+-----+------+-  +-------+-----+
      |              |                  |
      |       +------v------+           |
      |       |             |           |
      |       | Microsoft   |           |
      +-------> Broker      |^----------+
              | Application
              |             |
              +-------------+
              |             |
              |   Broker    |
              |   Storage   |
              |             |
              +-------------+
```
              
Теперь, вооружившись базовыми сведениями, вы сможете лучше понимать возможности единого входа и эффективнее реализовывать их в своем приложении с помощью платформы Microsoft Identity и пакетов SDK.


## Включение единого входа в нескольких приложениях с помощью ADAL

Здесь SDK ADAL iOS будет использоваться в следующих целях:

- включение единого входа без помощи брокера для набора приложений;
- включить поддержку единого входа с помощью брокера.

### Включение единого входа без брокера

Когда для приложений включен единый вход без брокера, большей частью связанных с этой функцией процессов управляют пакеты SDK для Microsoft Identity. Сюда входит поиск нужного пользователя в кэше, а также хранение списка вошедших пользователей для выполнения запросов.

Включить единый вход для своих приложений можно так:

1. Убедитесь, что все приложения используют один идентификатор клиента или приложения. 
* Убедитесь, что все приложения имеют один и тот же самозаверяющий сертификат от Apple для общего доступа к цепочкам ключей.
* Запросите одинаковое назначение цепочки ключей для каждого из приложений.
* Сообщите SDK Microsoft Identity об общей цепочке ключей, которую вы хотите использовать.

#### Использование одного идентификатора клиента или приложения для всех приложений в наборе

Чтобы платформа Microsoft Identity получила разрешение на использование общих токенов в приложениях, каждому из приложений потребуется предоставить один идентификатор клиента или приложения. Это уникальный идентификатор, предоставленный вам при регистрации первого приложения на портале.

Возможно, вы еще не знаете, как службе Microsoft Identity различать разные приложения, ведь она использует только один идентификатор приложения. Все дело в **URI перенаправления**. Каждое приложение может иметь несколько кодов URI перенаправления, зарегистрированных на портале подключения. Каждое приложение в наборе получит уникальный код URI перенаправления. Вот как это выглядит:

URI перенаправления App1: `x-msauth-mytestiosapp://com.myapp.mytestapp`

URI перенаправления App2: `x-msauth-mytestiosapp://com.myapp.mytestapp2`

URI перенаправления App3: `x-msauth-mytestiosapp://com.myapp.mytestapp3`

....

Эти элементы вложены в один и тот же идентификатор клиента или приложения. Найти их можно по коду URI перенаправления, возвращаемого нам в конфигурации пакета SDK.

```
+-------------------+
|                   |
|  Client ID        |
+---------+---------+
          |
          |           +-----------------------------------+
          |           |  App 1 Redirect URI               |
          +----------^+                                   |
          |           +-----------------------------------+
          |
          |           +-----------------------------------+
          +----------^+  App 2 Redirect URI               |
          |           |                                   |
          |           +-----------------------------------+
          |
          +----------^+-----------------------------------+
                      |  App 3 Redirect URI               |
                      |                                   |
                      +-----------------------------------+

```


*Обратите внимание, что формат URI перенаправления представлен ниже. Вы можете использовать любой URI перенаправления, если не хотите поддерживать брокера: в этом случае он будет выглядеть, как показано выше*



#### Создание общей цепочки ключей между приложениями

Включение общей цепочки ключей находится за пределами области этого документа и охватывается Apple в документе [Добавление возможностей](https://developer.apple.com/library/ios/documentation/IDEs/Conceptual/AppDistributionGuide/AddingCapabilities/AddingCapabilities.html). Важно, что именно вы решаете, как следует назвать цепочку ключей, и добавляете эту возможность во всех приложениях.

Если у вас правильно настроены назначения, в каталоге проекта появится файл `entitlements.plist`, содержимое которого аналогично следующему:

```
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE plist PUBLIC "-//Apple//DTD PLIST 1.0//EN" "http://www.apple.com/DTDs/PropertyList-1.0.dtd">
<plist version="1.0">
<dict>
	<key>keychain-access-groups</key>
	<array>
		<string>$(AppIdentifierPrefix)com.myapp.mytestapp</string>
		<string>$(AppIdentifierPrefix)com.myapp.mycache</string>
	</array>
</dict>
</plist>
```

После включения назначения цепочки ключей в каждом из приложений, если вы готовы использовать единый вход, используйте цепочку ключей в пакете SDK Microsoft Identity в `ADAuthenticationSettings` с помощью следующего параметра:

```
defaultKeychainSharingGroup=@"com.myapp.mycache";
```

> [AZURE.WARNING] 
При предоставлении цепочки ключей в приложениях любое приложение может удалить пользователей или даже все токены в приложении. Это особенно катастрофично, если фоновая работа некоторых приложений зависит от токенов. Общий доступ к цепочке ключей означает, что вы должны быть очень аккуратны при операциях удаления, выполняемых через SDK Microsoft Identity.

Вот и все! Теперь пакет SDK для Microsoft Identity передаст учетные данные во все приложения. Кроме того, во все экземпляры приложений будет отправлен список пользователей.

### Включение единого входа с брокером

Возможность использовать любого брокера, который установлен на устройстве, **по умолчанию выключена** в приложении. Чтобы пользоваться приложением с брокером, необходимо выполнить дополнительную настройку и добавить код в приложение.

Шаги для выполнения:

1. Включите режим брокера в вызове MS SDK кода приложения.
2. Установите новый URI перенаправления и предоставьте его приложению и для регистрации приложения.
3. Регистрация схемы URL-адреса.
4. Поддержка iOS9: добавление разрешения в файл info.plist.


#### Шаг 1. Включение режима брокера в приложении
Возможность для приложения использовать брокера включена при создании "контекста" и первоначальной настройке объекта проверки подлинности. Она выполняется путем настройки типа учетных данных в коде.

```
/*! See the ADCredentialsType enumeration definition for details */
@propertyADCredentialsType credentialsType;
```
Настройка `AD_CREDENTIALS_AUTO` позволит пакету SDK Microsoft Identity попробовать вызвать брокера, `AD_CREDENTIALS_EMBEDDED` предотвратит вызов брокера из SDK Microsoft Identity.

#### Шаг 2. Регистрация схемы URL-адреса
Платформа Microsoft Identity использует URL-адреса для вызова брокера, а затем возвращает элемент управления в приложение. Чтобы завершить этот цикл, требуется схема URL-адреса, зарегистрированная для приложения, о котором платформа Microsoft Identity узнает. Она может быть дополнением к другим схемам безопасности, которые вы зарегистрировали в приложении.

> [AZURE.WARNING] 
Рекомендуется сделать схему URL-адреса уникальной, чтобы свести к минимуму вероятность использования той же схемы URL-адреса другим приложением. Apple не обеспечивает уникальность схем URL-адреса, зарегистрированных в магазине приложений.

Ниже представлен пример того, как это показано в конфигурации проекта. Также это можно сделать в XCode:

```
<key>CFBundleURLTypes</key>
<array>
    <dict>
        <key>CFBundleTypeRole</key>
        <string>Editor</string>
        <key>CFBundleURLName</key>
        <string>com.myapp.mytestapp</string>
        <key>CFBundleURLSchemes</key>
        <array>
            <string>x-msauth-mytestiosapp</string>
        </array>
    </dict>
</array>
```

#### Шаг 3. Установка нового URI перенаправления в схеме URL-адреса

Чтобы обеспечить обязательный возврат токенов учетных данных в нужное приложение, необходимо убедиться, что мы выполняем обратный вызов в приложение таким способом, который операционная система iOS может проверить. Операционная система iOS передает идентификатор набора вызывающего приложения в приложения брокера Майкрософт. Стороннее приложение не может подделать его. Следовательно, мы будем использовать его с кодом URI приложения брокера, обеспечивая тем самым возврат маркеров в нужное приложение. Вам нужно указать этот уникальный код URI перенаправления в приложении, а также выбрать его в качестве URI перенаправления на нашем портале разработчиков.

Формат кода URI перенаправления должен быть таким:

`<app-scheme>://<your.bundle.id>`

Пример: *x-msauth-mytestiosapp://com.myapp.mytestapp*

Этот URI перенаправления должен быть указан при регистрации приложения на [классическом портале Azure](https://manage.windowsazure.com/). Дополнительные сведения о регистрации приложения Azure AD см. в статье [Интеграция в Azure Active Directory](active-directory-how-to-integrate.md).


##### Шаг 3а. Добавление URI перенаправления в приложение и на портал разработчика для поддержки проверки подлинности на основе сертификатов

Чтобы поддержать проверку подлинности на основе сертификатов, необходимо зарегистрировать второй "msauth" в приложении и на [классическом портале Azure](https://manage.windowsazure.com/) для обработки проверки подлинности сертификатов, если вы хотите добавить эту поддержку в приложение.

`msauth://code/<broker-redirect-uri-in-url-encoded-form>`

Пример: * *msauth://code/x-msauth-mytestiosapp%3A%2F%2Fcom.myapp.mytestapp*


#### Шаг 4. iOS9: добавление параметра конфигурации в приложение

ADAL использует –canOpenURL для проверки того, установлен ли брокер на устройстве. В iOS 9 Apple заблокированы схемы, которые может запрашивать приложение. Вам потребуется вручную добавить "msauth" в раздел LSApplicationQueriesSchemes `info.plist file`.

<key>LSApplicationQueriesSchemes</key> <array> <string>msauth</string> </array>

### Вы настроили единый вход!

Теперь пакет SDK для Microsoft Identity будет автоматически предоставлять учетные данные в приложения и вызывать брокера, если он есть на устройстве.

<!---HONumber=AcomDC_0608_2016-->