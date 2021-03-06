<properties
	pageTitle="Защита идентификации Azure Active Directory | Microsoft Azure"
	description="Узнайте, как защита идентификации Azure AD позволяет помешать злоумышленникам воспользоваться скомпрометированными удостоверениями и устройствами, а также защитить удостоверение или устройство, которое ранее предположительно или фактически скомпрометировано."
	services="active-directory"
	keywords="защита удостоверений Azure Active Directory, Cloud App Discovery, управление приложениями, безопасность, риск, уровень риска, уязвимость, политика безопасности"
	documentationCenter=""
	authors="markusvi"
	manager="femila"
	editor=""/>

<tags
	ms.service="active-directory"
	ms.workload="identity"
	ms.tgt_pltfrm="na"
	ms.devlang="na"
	ms.topic="article"
	ms.date="07/25/2016"
	ms.author="markvi"/>

#Защита идентификации Azure Active Directory 

Служба защиты идентификации Azure Active Directory — это служба безопасности, которая обеспечивает единое представление о всех событиях риска и возможных уязвимостях, которые могут влиять на удостоверения организации. Корпорация Майкрософт более десяти лет обеспечивает безопасность облачной идентификации, а теперь благодаря защите идентификации Azure AD аналогичные системы защиты стали доступны корпоративным клиентам. Защита идентификации предусматривает возможности обнаружения аномалий Azure AD (они доступны в отчетах Azure AD об аномальных действиях) и новые типы событий риска, которые позволяют обнаруживать аномалии в реальном времени.

## Ограничения текущей предварительной версии
В этом разделе перечислены ограничения, относящиеся к текущей предварительной версии службы защиты идентификации Azure Active Directory.

### Ограничение по стране или региону

В настоящее время предварительная версия защиты службы идентификации Azure Active Directory доступна только для каталогов, у которых параметр **Страна или регион** имеет значение **США**. <br><br> ![Исправление](./media/active-directory-identityprotection/222.png "Исправление")


### Защита идентификации и федеративные домены

Предварительная версия службы защиты идентификации Azure Active Directory имеет следующие ограничения в отношении федеративных доменов:

- Для федеративных доменов работает только политика безопасности для риска, связанного со входом. В настоящее время политика безопасности для риска, связанного с пользователем, не работает для федеративных доменов

- События риска определяются только для приложений, имеющих федеративную связь с Azure Active Directory.


##Приступая к работе

Подавляющее большинство нарушений безопасности — это случаи, когда злоумышленники получают доступ к среде за счет кражи удостоверения пользователя. Злоумышленники стали еще эффективнее использовать бреши в сторонних системах, а фишинговые атаки стали еще изощреннее. Как только злоумышленник получит доступ к учетной записи пользователя с привилегиями даже низкого уровня, ему не составит особого труда получить доступ и к важным ресурсам компании. Поэтому важно защитить все удостоверения, а в случае компрометации предотвратить злонамеренное использование скомпрометированных удостоверений.

Обнаружение скомпрометированных удостоверений — непростая задача. К счастью, ее можно выполнить с помощью службы защиты идентификации. Она использует адаптивные алгоритмы машинного обучения и эвристические методы, чтобы обнаруживать аномалии и события риска, которые могут указывать на компрометацию удостоверения.
 
На основе этих данных служба защиты идентификации создает отчеты и оповещения, с помощью которых можно исследовать события риска, чтобы предпринять действия по исправлению и устранению рисков.
 
Тем не менее служба защиты идентификации Azure Active Directory — больше, чем просто инструмент для мониторинга и создания отчетности. Она позволяет рассчитать уровень риска для каждого пользователя на основе соответствующих событий и настроить политики рисков, чтобы обеспечить автоматическую защиту удостоверений в организации. Эти политики рисков, помимо других элементов управления доступом Azure Active Directory и EMS, позволяют обеспечить автоматическую блокировку или предусмотреть адаптивные действия по исправлению, в том числе сброс пароля и принудительную многофакторную проверку подлинности.

####Возможности защиты идентификации 

**Обнаружение событий риска и учетных записей, подверженных риску:**

- обнаружение событий риска 6 типов с помощью правил машинного обучения и эвристических правил;

- расчет уровня риска для пользователя;

- предоставление пользовательских рекомендаций для повышения общего уровня безопасности путем информирования об уязвимостях.

<br>

**Исследование событий риска:**

- отправка уведомлений о событиях риска;

- исследование событий риска на основе соответствующей и контекстной информации;

- базовые рабочие процессы для отслеживания исследований;

- легкий доступ к действиям по исправлению, таким как сброс пароля.

<br>

**Политики условного доступа на основе рисков:**

- политика уменьшения рисков при сомнительных входах путем блокировки входа или запроса многофакторной проверки подлинности;

- политика блокировки или защиты учетных записей с возможным риском;

- политика обязательной регистрации пользователей для многофакторной проверки подлинности.


## Обнаружение и риск

### События риска

События риска — это события, которые отмечаются в службе защиты идентификации как подозрительные и указывают на возможную компрометацию удостоверения. Полный список событий риска см. в статье [Типы событий риска, обнаруживаемые службой защитой идентификации Azure Active Directory](active-directory-identityprotection-risk-events-types.md).

Некоторые из этих событий можно просмотреть на портале управления Azure в отчетах Azure AD об аномальных действиях. В следующей таблице перечислены различные типы событий риска и соответствующие отчеты **Azure AD об аномальных действиях**. Корпорация Майкрософт продолжает инвестировать в эту сферу и планирует постоянно увеличивать точность обнаружения событий риска, а также добавлять новые типы таких событий.



| Тип события риска в службе защиты идентификации | Соответствующий отчет Azure AD об аномальных действиях |
| :-- | :-- |
| Утерянные учетные данные | Пользователи с утерянными учетными данными |
| Невозможно переместиться в нетипичные расположения |	Нестандартные действия при входе |
| Попытки входа с инфицированных устройств | Попытки входа с возможно инфицированных устройств |
| Попытки входа с анонимных IP-адресов | Попытки входа из неизвестных источников |
| Попытки входа с IP-адресов с подозрительными действиями |	Попытки входа с IP-адресов с подозрительными действиями |
| Попытки входа из неизвестных расположений | — | 
| События блокировки (кроме общедоступной предварительной версии) | — |

У следующих отчетов Azure AD об аномальных действиях нет соответствующих событий риска в службе защиты идентификации Azure AD, и, следовательно, они не будут доступны в этой службе. Эти отчеты по-прежнему будут доступны на портале управления Azure, но в скором будущем они станут устаревшими, так как их заменяют события риска в рамках защиты идентификации:

- "Операции входа после нескольких неудачных попыток";
- "Операции входа из нескольких географических регионов".

### Уровень риска

Уровень риска события обозначает его серьезность (высокую, среднюю или низкую). В службе защиты идентификации уровень риска можно использовать для расстановки приоритета действий, которые необходимо предпринимать, чтобы уменьшить риск для организации. Серьезность события риска — это интенсивность сигнала, характеризующего прогноз компрометации удостоверения, в сочетании с объемом ненужных сведений, которые обычно создает событие.

- **Высокий уровень** — у события риска высокая вероятность и высокая серьезность. Эти события являются достоверным показателем того, что удостоверение пользователя скомпрометировано, и для каждой такой учетной записи следует безотлагательно принять меры по исправлению.

- **Средний уровень** — у события высокая серьезность, но более низкая вероятность риска или наоборот. Эти события потенциально опасны, и для каждой затронутой учетной записи пользователя следует принять меры по исправлению.

- **Низкий уровень** — у события риска низкая вероятность и низкая серьезность. Это событие может не требовать срочных действий, но в сочетании с другими событиями оно может служить достоверным показателем компрометации удостоверения.


![Уровень риска](./media/active-directory-identityprotection/01.png "Уровень риска")

 

События риска определяются в **реальном времени** или на этапе последующей обработки, когда событие уже состоялось (вне сети). Сейчас большинство событий риска в рамках защиты идентификации вычисляются в автономном режиме и отображаются в службе в течение 2–4 часов. При оценке в режиме реального времени события риска отображаются в консоли защиты идентификации в течение 5–10 минут.

Для некоторых устаревших клиентов не поддерживается обнаружение и предотвращение событий риска в реальном времени. Поэтому попытки входа из этих клиентов нельзя обнаружить или предотвратить в реальном времени.


## Исследование
Обычно исследование защиты идентификации начинают с панели мониторинга соответствующей службы.

<br><br> ![Исправление](./media/active-directory-identityprotection/29.png "Исправление") <br>

Панель мониторинга позволяет использовать:
 
- отчеты, такие как **Пользователи, помеченные для события риска**, **События риска** и **Уязвимости**;
- параметры, такие как конфигурация **политик безопасности**, **уведомлений** и **регистрации с многофакторной проверкой подлинности**.
 

Обычно исследование начинают с проверки действий, журналов и других связанных сведений, относящихся к событию риска, для принятия решения о необходимости выполнения действий по исправлению или устранению рисков, получения представления о способе компрометации удостоверения и вариантах использования скомпрометированного удостоверения.

Действия по исследованию можно привязать к [уведомлениям](active-directory-identityprotection-notifications.md), отправляемым службой защиты идентификации Active Directory Azure по электронной почте.

Следующие разделы содержат дополнительные сведения и шаги, связанные с исследованием.



## Что такое уровень риска для пользователя?

Уровень риска для пользователя ("Высокий", "Средний" или "Низкий") означает степень вероятности, что его удостоверение скомпрометировано. Он рассчитывается на основе событий риска для пользователя, связанных с его удостоверением.

У события риска может быть состояние **Активно** или **Закрыто**. При расчете риска для пользователя учитываются только события с состоянием **Активно**.

При расчете риска для пользователя также используются следующие входные данные:

- активные события риска, влияющие на пользователя;
- уровень риска этих событий;
- факт выполнения или невыполнения действий по исправлению.

<br> ![Риск для пользователя](./media/active-directory-identityprotection/86.png "Риск для пользователя") <br>



Уровни риска для пользователя можно использовать, чтобы создать политики условного доступа для блокировки входа пользователей, представляющих риск, или принудительного изменения пароля.


## Закрытие событий риска вручную

В большинстве случаев, чтобы события риска закрывались автоматически, вы будете использовать такое действие по исправлению, как безопасный сброс пароля. Однако это действие не всегда можно выполнить. <br> Например, это происходит, если:

- был удален пользователь с активными событиями риска;
- исследование показывает, что выявленные события риска произошли в результате действий полномочного пользователя.

Так как при расчете риска для пользователя учитываются события с состоянием **Активно**, может потребоваться уменьшить уровень риска, вручную закрыв эти события. <br> В ходе исследования вы можете использовать любое из этих действий, чтобы изменить состояние события риска:

<br> ![Действия](./media/active-directory-identityprotection/34.png "Действия") <br>

- **Разрешить** — если после исследования события риска вы выполнили соответствующее действие по исправлению вне службы защиты идентификации и считаете, что событие следует закрыть, пометьте его как "Разрешенное". Разрешенным событиям риска будет присвоено состояние "Закрыто", и они больше не будут учитываться при расчете риска для пользователя.

- **Пометить как ложное срабатывание** — иногда в результате исследования событий риска может обнаружиться, что событие помечено по ошибке. Количество таких вхождений можно уменьшить, пометив события риска как "Ложное срабатывание". Это позволит усовершенствовать классификацию таких событий при использовании алгоритмов машинного обучения. Состояние ложно сработанных событий изменится на **Закрыто**, и они больше не будут учитываться при расчете риска для пользователя.

- **Пропустить** — если вы не предпринимали действий по исправлению, но нужно удалить события риска из списка активных, можно пометить их, щелкнув "Пропустить", чтобы изменить состояние на "Закрыто". Пропущенные события не влияют на риск для пользователя. Эту возможность следует использовать только при чрезвычайных обстоятельствах.

- **Повторно активировать** — события риска, закрытые вручную (если вы щелкнули **Разрешить**, **Ложное срабатывание** или **Пропустить**), можно активировать снова, установив для них состояние **Активно**. Повторно активированные события рисков учитываются при расчете уровня риска для пользователя. События рисков, закрытые вследствие исправления (например, безопасного сброса пароля), нельзя повторно активировать.

**Чтобы открыть соответствующее диалоговое окно настройки, выполните следующее**:

1. В колонке **Защита идентификации Azure AD** щелкните **Пользователи, помеченные для события риска**. <br><br> ![Сброс паролей вручную](./media/active-directory-identityprotection/408.png "Сброс паролей вручную") <br>

2. Щелкните правой кнопкой мыши затронутого пользователя. <br><br> ![Сброс паролей вручную](./media/active-directory-identityprotection/437.png "Сброс паролей вручную") <br>

## Исправление событий риска для пользователей

Исправление — это действие для защиты удостоверения или устройства, которое ранее предположительно или фактически скомпрометировано. Действие исправления восстанавливает безопасное состояние удостоверения или устройства и разрешает предыдущие события риска, связанные с таким удостоверением или устройством.

Чтобы устранить события риска для пользователя, можно сделать следующее:

- безопасно сбросить пароль, чтобы устранить событие вручную;

- настроить политику безопасности в отношении риска для пользователей, чтобы автоматически устранять события или уменьшать их риск;

- переустановить систему инфицированного устройства из образа.


### Безопасный сброс паролей вручную

Безопасный сброс паролей —это эффективный способ исправления многих событий риска. При таком сбросе события автоматически закрываются и происходит перерасчет уровня риска для пользователя. Чтобы инициировать сброс пароля для пользователя с риском, можно использовать панель мониторинга службы защиты идентификации.

Соответствующее диалоговое окно предлагает два метода для сброса пароля:

**Сброс пароля**. Выберите **Требовать от пользователя сбросить пароль**, чтобы разрешить самостоятельное восстановление, если пользователь зарегистрировался для многофакторной проверки подлинности. При следующем входе пользователю будет необходимо пройти многофакторную проверку подлинности по запросу и только потом произойдет принудительное изменения пароля. Этот параметр будет недоступен, если учетная запись пользователя еще не зарегистрирована для многофакторной проверки подлинности.

**Использование временного пароля**. Выберите **Создать временный пароль**, чтобы сразу же аннулировать существующий пароль пользователя и создать для него новый временный пароль. Отправьте новый временный пароль по альтернативному адресу электронной почты пользователя или руководителя пользователя. Так как пароль временный, после входа пользователю будет предложено изменить его.

<br> ![Политика](./media/active-directory-identityprotection/71.png "Политика") <br>

**Чтобы открыть соответствующее диалоговое окно настройки, выполните следующее**:

1. В колонке **Защита идентификации Azure AD** щелкните **Пользователи, помеченные для события риска**. <br><br> ![Сброс паролей вручную](./media/active-directory-identityprotection/408.png "Сброс паролей вручную") <br>

2. Щелкните затронутого пользователя. <br><br> ![Сброс паролей вручную](./media/active-directory-identityprotection/404.png "Сброс паролей вручную") <br>

2. Щелкните "Сброс пароля". <br><br> ![Сброс паролей вручную](./media/active-directory-identityprotection/420.png "Сброс паролей вручную") <br>





## Политика безопасности в отношении риска для пользователя

Политика безопасности в отношении риска для пользователя — это политика условного доступа, согласно которой вычисляется уровень риска для конкретного пользователя, а на основе ее предопределенных условий и правил предпринимаются действия по исправлению и устранению рисков. <br><br> ![Политика риска пользователей](./media/active-directory-identityprotection/500.png "Политика риска пользователей") <br>

Служба защиты идентификации Azure AD облегчает управление исправлением и устранением рисков для пользователей, помеченных для событий риска, и позволяет:

- задать пользователей и группы, к которым следует применять политику; <br><br> ![Политика риска пользователей](./media/active-directory-identityprotection/501.png "Политика риска пользователей") <br>

- задать пороговое значение уровня политики риска для пользователей (низкий, средний или высокий уровень), при достижении которого активируется изменение пароля; <br><br> ![Политика риска пользователей](./media/active-directory-identityprotection/502.png "Политика риска пользователей") <br>

- задать пороговое значение уровня политики риска для пользователей (низкий, средний или высокий уровень), при достижении которого активируется блокировка пользователя; <br><br> ![Политика риска пользователей](./media/active-directory-identityprotection/503.png "Политика риска пользователей") <br>

- переключить состояние политики; <br><br> ![Регистрация в MFA](./media/active-directory-identityprotection/403.png "Регистрация в MFA") <br>

- проверить и оценить влияние изменений до их активации. <br><br> ![Политика риска пользователей](./media/active-directory-identityprotection/504.png "Политика риска пользователей") <br>


Если выбрать пороговое значение уровня **Высокий**, уменьшится количество случаев активации политики и степень влияния на пользователей. Однако это исключит из политики пользователей, помеченных для события риска, с уровнем **Низкий** и **Средний**, оставив незащищенными удостоверения и устройства, которые были ранее предположительно или фактически скомпрометированы.

При настройке политики:

- исключите пользователей, из-за которых, скорее всего, создается большое количество ложных срабатываний (разработчиков, аналитиков системы безопасности);

- исключите пользователей, для языкового стандарта которых нецелесообразно включать политику (например, при его использовании недоступна техническая поддержка);

- используйте пороговое значение уровня **Высокий** при начальном развертывании политики, а также если требуется уменьшить количество запросов защиты для конечного пользователя;

- используйте пороговое значение уровня **Низкий**, если нужно повысить уровень безопасности для организации. Если вы выберете пороговое значение уровня **Низкий**, при входе для пользователей будет отображаться больше запросов защиты, но уровень безопасности повысится.

Большинству организаций мы рекомендуем настроить для правил пороговое значение уровня **Средний**, чтобы обеспечить оптимальное соотношение удобства использования и безопасности.

Общие сведения о соответствующем взаимодействии с пользователями см. в статьях:

- [Восстановление скомпрометированной учетной записи](active-directory-identityprotection-flows.md#compromised-account-recovery).

- [Блокировка скомпрометированной учетной записи](active-directory-identityprotection-flows.md#compromised-account-blocked).


**Чтобы открыть соответствующее диалоговое окно настройки, выполните следующее**:

1. В колонке **Защита идентификации Azure AD** щелкните **Параметры**. <br><br> ![Политика риска пользователя](./media/active-directory-identityprotection/401.png "Политика риска пользователя") <br>

2. В разделе **Политики безопасности** щелкните **Риск пользователя**. <br><br> ![Политика риска пользователя](./media/active-directory-identityprotection/500.png "Политика риска пользователя") <br>






## Устранение рисков событий для пользователей
При наличии прав администратора в политике безопасности в отношении риска пользователей можно установить блокировку входа на основе уровня риска.

Блокировка входа:
 
- предотвращает создание новых событий риска для каждого затронутого пользователя;

- позволяет администраторам вручную устранить события риска, которые распространяются на удостоверение пользователя, и восстанавливать безопасное состояние.



## Что такое уровень риска при входе?

Уровень риска при входе ("Высокий", "Средний" или "Низкий") обозначает вероятность того, что при конкретной попытке входа другое лицо попытается пройти проверку подлинности с помощью удостоверения пользователя. Уровень риска при входе оценивается во время входа и учитывает события риска и индикаторы, обнаруженные для этого входа в режиме реального времени.

## Устранение событий риска при входе 
Устранение рисков — это действие, позволяющее ограничить возможность использования злоумышленником скомпрометированного удостоверения или устройства без восстановления их безопасного состояния. Устранение рисков не разрешает предыдущие события риска при входе, связанные с удостоверением или устройством.

Чтобы автоматически устранять события риска при входе, в рамках защиты идентификации Azure AD можно использовать условный доступ. С помощью политик можно настроить уровень риска или условия входа, при которых будет блокироваться вход, если он представляет риск, или обязательную многофакторную проверку подлинности. Эти действия могут помешать злоумышленникам использовать украденное удостоверение в преступных целях и позволят выиграть время для защиты удостоверения.


## Политика безопасности в отношении риска входа

Политика риска при входе — это политика условного доступа, которая оценивает риск конкретной попытки входа и применяет действия для устранения рисков на основе предопределенных условий и правил. <br><br> ![Политика риска входа](./media/active-directory-identityprotection/700.png "Политика риска входа") <br>

Защита идентификации Azure AD облегчает управление устранением рисков при попытке входа и позволяет:

- задать пользователей и группы, к которым следует применять политику; <br><br> ![Политика риска входа](./media/active-directory-identityprotection/701.png "Политика риска входа") <br>

- задать пороговое значение для уровня политики риска при входе (низкий, средний или высокий уровень), при достижении которого активируется запрос на многофакторную проверку подлинности для соответствующих входов; <br><br> ![Политика риска входа](./media/active-directory-identityprotection/702.png "Политика риска входа") <br>

- задать пороговое значение для уровня политики риска при входе (низкий, средний или высокий уровень), при достижении которого происходит блокировка соответствующих входов; <br><br> ![Политика риска входа](./media/active-directory-identityprotection/703.png "Политика риска входа") <br>

- переключить состояние политики; <br><br> ![Регистрация в MFA](./media/active-directory-identityprotection/403.png "Регистрация в MFA") <br>

- проверить и оценить влияние изменений до их активации. <br><br> ![Политика риска входа](./media/active-directory-identityprotection/704.png "Политика риска входа") <br>

 
Если выбрать пороговое значение уровня **Высокий**, уменьшится количество случаев активации политики и степень влияния на пользователей.<br> Тем не менее это исключит из политики пользователей, помеченных для события риска с уровнем **Низкий** и **Средний**, что может предотвратить блокировку скомпрометированного удостоверения для злоумышленников.

При настройке политики:

- исключите пользователей, которые не используют или не могут использовать многофакторную проверку подлинности;

- исключите пользователей, для языкового стандарта которых нецелесообразно включать политику (например, при его использовании недоступна техническая поддержка);

- исключите пользователей, из-за которых, скорее всего, создается большое количество ложных срабатываний (разработчиков, аналитиков системы безопасности);

- используйте пороговое значение уровня **Высокий** при начальном развертывании политики, а также если требуется уменьшить количество запросов защиты для конечного пользователя;

- используйте пороговое значение уровня **Низкий**, если нужно повысить уровень безопасности для организации. Если вы выберете пороговое значение уровня **Низкий**, при входе для пользователей будет отображаться больше запросов защиты, но уровень безопасности повысится.

Большинству организаций мы рекомендуем настроить для правил пороговое значение уровня **Средний**, чтобы обеспечить оптимальное соотношение удобства использования и безопасности.

 
Политика риска входа:

- применяется ко всему трафику в браузере и при каждом входе с использованием современных способов проверки подлинности;
- не применяется к приложениям, которые используют протоколы безопасности старых версий, отключая конечную точку WS-Trust в федеративном IdP, таком как ADFS.

На странице **События риска** консоли защиты данных приведен список всех событий:

- к которым применена эта политика;
- что позволяет просмотреть действие и определить, выполнено ли оно соответствующим образом.

Общие сведения о соответствующем взаимодействии с пользователями см. в статьях:

- [Восстановление входа, представлявшего риск](active-directory-identityprotection-flows.md#risky-sign-in-recovery)

- [Блокировка входа, представляющего риск](active-directory-identityprotection-flows.md#risky-sign-in-blocked)

- [Регистрация с многофакторной проверкой подлинности во время входа, представляющего риск](active-directory-identityprotection-flows.md#multi-factor-authentication-registration-during-a-risky-sign-in)





**Чтобы открыть соответствующее диалоговое окно настройки, выполните следующее**:

1. В колонке **Защита идентификации Azure AD** щелкните **Параметры**. <br><br> ![Регистрация в MFA](./media/active-directory-identityprotection/401.png "Регистрация в MFA") <br>

1. В разделе **Политики безопасности** щелкните **Риск при входе**. <br><br> ![Регистрация в MFA](./media/active-directory-identityprotection/700.png "Регистрация в MFA") <br>




## Политика регистрации с многофакторной проверкой подлинности

Azure Multi-factor Authentication — это метод проверки идентичности пользователя, при котором используются дополнительные средства, а не только имя пользователя и пароль. Это второй уровень безопасности, который применяется для входа пользователя в систему и выполнения транзакций. <br> Мы рекомендуем настроить обязательную проверку Azure Multi-Factor Authentication при входе пользователей, так как она:

- обеспечивает строгую аутентификацию с помощью ряда простых параметров проверки;

- играет ключевую роль при подготовке защиты от компрометации, а также восстановления скомпрометированных учетных записей в организации.

Дополнительную информацию см. в статье [Что такое многофакторная проверка подлинности Azure?](../multi-factor-authentication/multi-factor-authentication.md).


Защита идентификации Azure AD облегчает управление развертыванием регистрации с многофакторной проверкой подлинности за счет настройки политики, которая позволяет:

- просматривать текущее состояние регистрации; <br><br> ![Регистрация в MFA](./media/active-directory-identityprotection/603.png "Регистрация в MFA") <br>

- задать пользователей и группы, к которым следует применять политику; <br><br> ![Регистрация в MFA](./media/active-directory-identityprotection/601.png "Регистрация в MFA") <br>

- определить, на протяжении какого времени они могут пропускать регистрацию; <br><br> ![Регистрация в MFA](./media/active-directory-identityprotection/602.png "Регистрация в MFA") <br>

- переключить состояние политики. <br><br> ![Регистрация в MFA](./media/active-directory-identityprotection/403.png "Регистрация в MFA") <br>

Общие сведения о соответствующем взаимодействии с пользователями см. в статьях:

- [Регистрация с многофакторной проверкой подлинности](active-directory-identityprotection-flows.md#multi-factor-authentication-registration).

- [Регистрация с многофакторной проверкой подлинности во время входа, представляющего риск](active-directory-identityprotection-flows.md#multi-factor-authentication-registration-during-a-risky-sign-in).





**Чтобы открыть соответствующее диалоговое окно настройки, выполните следующее**:

1. В колонке **Защита идентификации Azure AD** щелкните **Параметры**. <br><br> ![Регистрация в MFA](./media/active-directory-identityprotection/401.png "Регистрация в MFA") <br>

2. В **Multi-Factor Authentication** щелкните **Регистрация**. <br><br> ![Регистрация в MFA](./media/active-directory-identityprotection/402.png "Регистрация в MFA") <br>




## См. также

 - [Channel 9: Azure AD and Identity Show: Identity Protection Preview](https://channel9.msdn.com/Series/Azure-AD-Identity/Azure-AD-and-Identity-Show-Identity-Protection-Preview) (Канал 9. Azure Active Directory и идентификация. Предварительная версия защиты идентификации)
 - [Типы событий риска, обнаруживаемые защитой идентификации Azure Active Directory](active-directory-identityprotection-risk-events-types.md)
 - [Уязвимости, обнаруживаемые защитой идентификации Azure Active Directory](active-directory-identityprotection-vulnerabilities.md)
 - [Уведомления защиты идентификации Azure Active Directory](active-directory-identityprotection-notifications.md)
 - [Процессы защиты идентификации Azure Active Directory](active-directory-identityprotection-flows.md)
 - [Тренировочное задание по защите идентификации Azure Active Directory](active-directory-identityprotection-playbook.md)
 - [Глоссарий по защите идентификации Azure Active Directory](active-directory-identityprotection-glossary.md)
 - [Начало работы с защитой идентификации Azure Active Directory и Microsoft Graph](active-directory-identityprotection-graph-getting-started.md)

<!---HONumber=AcomDC_0727_2016-->