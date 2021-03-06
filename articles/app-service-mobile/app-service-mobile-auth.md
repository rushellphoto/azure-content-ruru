<properties
	pageTitle="Проверка подлинности и авторизация в мобильных приложениях Azure | Microsoft Azure"
	description="Справочник принципов и общие сведения о функции проверки подлинности и авторизации для мобильных приложений Azure."
	services="app-service\mobile"
	documentationCenter=""
	authors="mattchenderson"
	manager="erikref"
	editor=""/>

<tags
	ms.service="app-service-mobile"
	ms.workload="mobile"
	ms.tgt_pltfrm="na"
	ms.devlang="multiple"
	ms.topic="article"
	ms.date="05/05/2016"
	ms.author="mahender"/>

# Проверка подлинности и авторизация в мобильных приложениях Azure

## Что такое проверка подлинности и авторизация службы приложений?

> [AZURE.NOTE] Эта статья будет перенесена в консолидированную статью [Проверка подлинности и авторизация в службе приложений Azure](../app-service/app-service-authentication-overview.md), которая охватывает веб-приложения, мобильные приложения и приложения API.

Проверка подлинности и авторизация службы приложений — это функция, которая позволяет пользователям приложения выполнять вход без изменений кода, необходимых в серверной части приложения. Она является простым способом обеспечения защиты приложения и работы с данными пользователей.

Служба приложений использует федеративное удостоверение, когда сторонний **поставщик удостоверений** сохраняет учетные записи и проверяет подлинность пользователей, а приложение использует это удостоверение вместо своего. Служба приложений поддерживает пять поставщиков удостоверений по умолчанию: _Azure Active Directory_, _Facebook_, _Google_, _учетная запись Майкрософт_ и _Twitter_. Вы можете расширить этот список для своих приложений, интегрировав другого поставщика удостоверений или собственное настраиваемое решение идентификации.

Ваше приложение может поддерживать любое количество поставщиков удостоверений, предоставляя конечным пользователям возможность выбора способа входа.

Чтобы немедленно приступить к работе, ознакомьтесь с одним из следующих учебников.

- [Добавление проверки подлинности в приложение iOS]
- [Добавление проверки подлинности в приложение Xamarin.iOS]
- [Добавление проверки подлинности в приложение Xamarin.Android]
- [Добавление проверки подлинности в приложение Windows]

## Принцип действия проверки подлинности

Чтобы проверять подлинность с помощью одного из поставщиков удостоверений, сначала необходимо настроить поставщик удостоверений так, чтобы ему было известно о вашем приложении. Затем поставщик удостоверений предоставит вам коды и секреты, которые будут указаны в приложении. Это позволяет сформировать отношение доверия и дает службе приложений возможность проверять удостоверения.

Эти действия описаны в следующих разделах.

- [Настройка приложения для использования имени входа Azure Active Directory]
- [Настройка приложения для использования имени входа Facebook]
- [Настройка приложения для использования имени входа Google]
- [Настройка приложения для использования входа по учетной записи Майкрософт]
- [Настройка приложения для использования имени входа Twitter]

Выполнив все настройки на серверной стороне, можно изменить клиент для входа. Существует два подхода:

- использование одной строки кода для входа пользователей с помощью пакета SDK клиента для мобильных приложений;
- использование пакета SDK, опубликованного указанным поставщиком удостоверений, для определения удостоверения и последующего получения доступа к службе приложений.

>[AZURE.TIP] Многие приложения должны использовать пакет SDK поставщика для поддержки более естественной процедуры входа и использования возможности обновления и других преимуществ, предлагаемых конкретным поставщиком.

### Принцип действия проверки подлинности без пакета SDK поставщика

Если вы не хотите настраивать пакет SDK поставщика, можно разрешить мобильным приложениям выполнять за вас вход. Клиентский пакет SDK мобильных приложений будет открывать выбранному вами поставщику веб-представление и осуществлять процедуру входа. Иногда в блогах и на форумах это действие может называться "серверным потоком" или "управляемым сервером потоком", так как управление входом выполняет сервер, а клиентский пакет SDK никогда не получает маркер поставщика.

Код, необходимый для запуска этого потока, рассматривается в учебнике по проверке подлинности для каждой платформы. В конце потока у клиентского пакета SDK есть маркер службы приложений, и маркер автоматически присоединяется ко всем запросам к серверной части.

### Принцип действия проверки подлинности с пакетом SDK поставщика

При работе с пакетом SDK поставщика процедура входа очень тесно привязывается к ОС платформы, в которой выполняется приложение. При этом вы также получаете маркер поставщика и некоторые сведения о пользователях на стороне клиента, что упрощает использование Graph API и настройку пользовательского интерфейса. Иногда в блогах и на форумах это может называться "клиентским потоком" или "управляемым клиентом потоком", так как управление входом на клиенте выполняет код, и код клиента имеет доступ к маркеру поставщика.

Полученный маркер поставщика необходимо отправить в службу приложений для проверки. В конце потока у клиентского пакета SDK есть маркер службы приложений, и маркер автоматически присоединяется ко всем запросам к серверной части. Если необходимо, разработчик также может сохранить ссылку на маркер поставщика.

## Принцип действия авторизации

Проверка подлинности и авторизация службы приложений предоставляет несколько вариантов в качестве значения параметра **Действие, выполняемое, если запрос не прошел проверку подлинности**. Прежде чем код получит конкретный запрос, служба приложений может проверить, прошел ли запрос проверку подлинности. Если проверка подлинности не пройдена, запрос отклоняется, и перед повторением действия предпринимается попытка входа пользователя.

Один из вариантов — перенаправление запросов без проверки подлинности к одному из поставщиков удостоверений. В этом случае в веб-браузере пользователь будет перенаправлен на новую страницу. Однако мобильный клиент не может быть перенаправлен таким образом, и ответы без проверки подлинности будут получать код состояния HTTP _401 — Не санкционировано_. С учетом этого первый запрос, который делает ваш клиент, всегда должен быть направлен в конечную точку входа. После этого можно выполнять вызовы других API. Если вы пытаетесь вызвать другой API перед входом, ваш клиент получит ошибку.

Чтобы получить более строгий контроль над тем, какие конечные точки требуют проверку подлинности, можно выбрать параметр "Никаких действий (разрешить выполнение запроса)" для запросов без проверки подлинности. В этом случае все решения относительно проверки переносятся в код приложения. Это также позволяет разрешить доступ конкретным пользователям на основе правил настраиваемой авторизации.

## Документация

В следующих учебниках показано, как добавить проверку подлинности в мобильные клиенты с помощью службы приложений.

- [Добавление проверки подлинности в приложение iOS]
- [Добавление проверки подлинности в приложение Xamarin.iOS]
- [Добавление проверки подлинности в приложение Xamarin.Android]
- [Добавление проверки подлинности в приложение Windows]

В следующих учебниках содержатся сведения о настройке службы приложений для использования разных поставщиков проверки подлинности.

- [Настройка приложения для использования имени входа Azure Active Directory]
- [Настройка приложения для использования имени входа Facebook]
- [Настройка приложения для использования имени входа Google]
- [Настройка приложения для использования входа по учетной записи Майкрософт]
- [Настройка приложения для использования имени входа Twitter]

Если вы хотите использовать систему удостоверений, отличную от указанных здесь, можно также использовать [предварительную версию поддержки настраиваемой проверки подлинности в серверном пакете SDK .NET](app-service-mobile-dotnet-backend-how-to-use-server-sdk.md#custom-auth).

[Добавление проверки подлинности в приложение iOS]: app-service-mobile-ios-get-started-users.md
[Добавление проверки подлинности в приложение Xamarin.iOS]: app-service-mobile-xamarin-ios-get-started-users.md
[Добавление проверки подлинности в приложение Xamarin.Android]: app-service-mobile-xamarin-android-get-started-users.md
[Добавление проверки подлинности в приложение Windows]: app-service-mobile-windows-store-dotnet-get-started-users.md

[Настройка приложения для использования имени входа Azure Active Directory]: app-service-mobile-how-to-configure-active-directory-authentication.md
[Настройка приложения для использования имени входа Facebook]: app-service-mobile-how-to-configure-facebook-authentication.md
[Настройка приложения для использования имени входа Google]: app-service-mobile-how-to-configure-google-authentication.md
[Настройка приложения для использования входа по учетной записи Майкрософт]: app-service-mobile-how-to-configure-microsoft-authentication.md
[Настройка приложения для использования имени входа Twitter]: app-service-mobile-how-to-configure-twitter-authentication.md

<!---HONumber=AcomDC_0511_2016-->