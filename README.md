# Инструкция для тестирования Альфа-Линк - 1С:Директбанк

## Оглавление

[1. Особенности работы с 1С:Директбанк](#1. Особенности работы с 1С:Директбанк)

[2. Настройка среды для тестирования через 1С](#2. Настройка среды для тестирования через 1С)

[3. Тестирование отправки документов](#3. Тестирование отправки документов)

[4. Справочная информация и поддержка](#4. Справочная информация и поддержка )



## 1. Особенности работы с 1С:Директбанк

Технология Директбанк, позволяет отправлять документы напрямую в банк и получать их непосредственно из программы 1С

- Для запроса выписки не требуется подпись,
- При отправке платежа подпись обязательна,
- Версия формата документов = 2.2.2,
- Для подключения необходимо будет предоставить «документы для подключения Альфа-Линк» и единоразово оплатить его. Оплата отправки платежей (если требуется) производится отдельно, ежемесячно. 

Вы также можете рассмотреть способ работы в режиме API, описанный в [разделе](https://github.com/Host-to-Host-Instructions/alfalink-1c-directbank-via-api) 

## 2. Настройка среды для тестирования через 1С

Для тестирования необходимо настроить в 1С тестовую компанию, загрузить файл настроек. Все операции при тестировании проводятся на тестовых данных.

### 2.1. Скачивание архива со всеми необходимыми файлами

По [ссылке](https://github.com/Host-to-Host-Instructions/alfalink-1c-directbank-via-1c/archive/refs/heads/master.zip) можно скачать архив с необходимыми для тестирования файлами.

**Архив содержит:**

- Данную инструкцию по подключению (**README.md и в формате pdf**)
- Файл с тестовыми настройками **directBank_settings.xml**
- Файл с тестовыми настройками для Зарплатного проекта **directBank_SP_settings.xml**
- Файл сертификата для тестовой организации **directBank_cert_psw_123456.pfx**
- Файл с корневыми сертификатами УЦ Альфа-Банка **cacerts.p7b**
- Список отзыва сертификатов **certificate_revocation_list.crl** (обновляется **каждый** месяц 14-го числа).
- Папка с примерами (**examples**)



### 2.2. Настройка тестовой компании 

Для тестирования используется тестовая компания, которую необходимо добавить в вашу 1С. Все операции при тестировании проводятся с помощью данных, приведённых в таблицах ниже. 

[Инструкция по добавлению тестовой компании](http://testdirectbank.1c.ru/docdirectbank.htm#_Toc10219664)

**Реквизиты компании:**

| Параметр | Значение |
| ------------- |-------------|
| Краткое наименование | ООО "Тест Директ Банк" |
| Полное наименование | Общество с ограниченной ответственностью "Тест Директ Банк" |
| ИНН | 0329156629 |
| КПП | 043360081 |
| Номер счета | 40702810201300509144 |
| Банк | АО "АЛЬФА-БАНК" |
| Корсчет банка | 30101810200000000593 |
| БИК банка | 044525593 |

**Реквизиты компании получателя (для тестирования платежей):**

| Параметр             | Значение                                                     |
| -------------------- | ------------------------------------------------------------ |
| Краткое наименование | ООО "Тест Директ банк Получатель"                            |
| Полное наименование  | Общество с ограниченной ответственностью "Тест Директ банк Получатель" |
| ИНН                  | 0329156636                                                   |
| КПП                  | 770101999                                                    |
| Номер счета          | 40702810300000019796                                         |
| Банк                 | АО "АЛЬФА-БАНК"                                              |
| Корсчет банка        | 30101810200000000593                                         |
| БИК банка            | 044525593                                                    |

**Реквизиты для тестирования зарплатного проекта (зарплатная ведомость и заявка на открытие лицевых счетов):**

| Параметр                            | Значение                                        |
| ----------------------------------- | ----------------------------------------------- |
| Краткое наименование                | ООО "МАНИ"                                      |
| Полное наименование                 | Общество с ограниченной ответственностью "МАНИ" |
| ИНН                                 | 0551666943                                      |
| Номер счета                         | 40702810300000003333                            |
| Банк                                | АО "АЛЬФА-БАНК"                                 |
| Корсчет банка                       | 30101810200000000593                            |
| БИК банка                           | 044525593                                       |
| Номер зарплатного договора          | 00GSFA                                         |
| ФИО сотрудника                      | Семенов Семен Семенович                         |
| Отделение банка                     | 0000                                            |
| Лицевой счёт сотрудника             | 40817810904050000075                           |
| Серия паспорта                      | 99 89                                           |
| Номер паспорта                      | 898989                                          |
| Дата выдачи паспорта                | 2017-07-14                                      |
| Кем выдан паспорт                   | Уфмс                                            |
| Код подразделения                   | 090-909                                         |
| Дата рождения                       | 1970-07-23                                      |
| Место рождения (полное/сокращённое) | Москва/Москва                                   |
| Эмбоссированный текст               | Semenov / Semen / MR                            |



### 2.3. Настройка обмена с банком

Для настройки технологии 1С:Директбанк необходимо файл настроек directBank_settings.xml из архива загрузить в программу 1С.

В разделе 1С "Настройка обмена с банками" есть команда "Загрузить из файла" (либо опция "Еще" → "Загрузить из файла").

Программа запросит выбор файла с диска и создаст настройку обмена (подробнее тут: [Инструкция по настройке 1С Директбанк](https://its.1c.ru/db/metod81#content:5375:hdoc@2fdb754e)).

Для тестирования **зарплатного проекта** используется свой файл настроек: directBank_SP_settings.xml, который позволяет совершать обмен с банком тестовой организации ООО "МАНИ". 

Данные для авторизации: 

| Параметр                | Значение                                            |
| ----------------------- | --------------------------------------------------- |
| Сервер                  | https://alfa-link-int.alfabank.ru/API/v1/directbank |
| Логин                   | directBank                                          |
| Пароль                  | 123456                                              |
| Логин (для ЗП проекта)  | azon_no_sign                                        |
| Пароль (для ЗП проекта) | 123456                                              |
| ОГРН УЦ Альфа-Банка     | 1027700067328                                       |

После загрузки настроек проверьте, что на странице «Настройки обмена с банком» для Альфа-банка указано:

- Идентификатор организации: **40702810201300509144** (для ЗП проекта: **40702810300000003333**)
- Адрес сервера банка: https://alfa-link-int.alfabank.ru/API/v1/directbank
- Логин: **directBank** (для ЗП проекта **azon_no_sign**)

![Пример](./examples/Настройки%20обмена%20с%20банком%20после%20загрузки%20файла.png "Настройки обмена с банком после загрузки файла")

![Пример](./examples/Настройки%20обмена%20с%20банком%20после%20загрузки%20файла.png "Настройки обмена с банком после загрузки файла")

Если все поля заполнены верно, значит можно продолжать настройку. Иначе необходимо повторить загрузку файла настроек и проверить, нет ли ошибок при загрузке.

Если файл настроек не загружается корректно, необходимо обратиться в поддержку 1С.



### 2.4. Сертификат для подписания платежа 

**Тестовая среда**

Перед отправкой на исполнение тестовый платеж необходимо подписать сертификатом квалифицированной подписи (файл directBank_cert_psw_123456.pfx).

Установите сертификат на свой компьютер. 
Пароль от сертификата: 123456

В случае необходимости установите **корневые сертификаты** (файл cacerts.p7b) и **список отзыва** (файл certificate_revocation_list.crl) вручную.

**Если при подписании документа вы получаете в 1С ошибку: "Электронная подпись не верна. Причина: Сертификат неквалифицированный, так как выпущен удостоверяющим центром ТЕСТ УЦ 2.0 АО "АЛЬФА-БАНК", не аккредитованным на момент выпуска сертификата."**, добавьте, пожалуйста, для ваших пользователей ОГРН УЦ Альфа-Банка в форме "Разрешенные неакредитованные УЦ", где указывается список ОГРН удостоверяющих центров без аккредитации, подпись в этом случае будет считаться верной. (Администрирование - Общие настройки - Электронная подпись и шифрование - Разрешенные неакредитованные УЦ"). ОГРН УЦ Альфа-Банка: **1027700067328**.



**Промышленная среда**


При подключении Альфа-линк в промышленной среде необходимо предоставить в банк публичный (открытый) ключ квалифицированного сертификата (алгоритм подписи ГОСТ-2012) для подписания платежей.

Сертификат должен быть выпущен на уполномоченное лицо, которое подключается к Альфа-линк (сертификат на руководителя выпускается в ФНС, на сотрудника (как на физ. лицо) – в любом [аккредитованном удостоверяющем центре](https://digital.gov.ru/ru/activity/govservices/certification_authority/)).

Для подписания документов с помощью сертификата, выпущенного на физ. лицо, также нужна машиночитаемая доверенность (МЧД). Информацию о формате МЧД и кодах полномочий уточняйте по эл. почту alfa-link@alfabank.ru

Если не планируется направлять платежи из 1С, а только запрашивать выписку по счету, то сертификат предоставлять не нужно, в заявлении следует отметить роль "Оператор".



### 2.5. Настройка выписки по-расписанию

Для тестирования функции получения выписки по-расписанию необходимо на странице «Настройки обмена с банком» для Альфа-банка в разделе «Основное» нажать кнопку «Включить» (выделено красным на изображении ниже).

![Выписка по расписанию](./examples/Выписка%20по%20расписанию%201.png "Выписка по расписанию")

После этого нужно ввести пароль для пользователя, осуществляющего запрос выписки по-расписанию.

<div style="border: 1px solid #dbdbd6;
    margin-bottom: 1.25em;
    padding: 1.25em;
    background: #f3f3f2;
    border-radius: 4px;">
  <p><b>Логин изменять не нужно, он загружается автоматически из файла настроек.</b></p>
  <p><b>Пароль для тестового пользователя: 123456</b></p>
</div>

**В промышленной среде пароль приходит в смс на номер, указанный в заявлении на подключение.**

![Выписка по расписанию](./examples/Выписка%20по%20расписанию%202.png "Выписка по расписанию")

Далее нажать кнопку «Запустить автоматическое получение выписки банка».

После проверки работы обмена выписка начнёт запрашиваться по-расписанию.

**<img src="./examples/Выписка по расписанию 3.png" alt="Выписка по расписанию" style="zoom:67%;" />**

**Запросы выписки делаются по всем счетам организации, настроенным в 1С**.

Запрошенные выписки можно найти в разделе «Электронные документы».

![Выписка по расписанию](./examples/Выписка%20по%20расписанию%204.png "Выписка по расписанию")

<div class="content" style="border: 1px solid #dbdbd6;
    margin-bottom: 1.25em;
    padding: 1.25em;
    background: #f3f3f2;
    border-radius: 4px;">
<div class="title" style="font-size: 19pt; color: #7a2518; margin-top: 0; text-align: center;">Обратите внимание!</div>
<div class="paragraph">
<p><b>Подробнее о параметрах/частоте запросов выписки по-расписанию, о возможностях выгрузки выписок конкретно для вашей конфигурации вы можете узнать у службы поддержки 1С.</b></p>
</div>
</div>



## 3. Тестирование отправки документов

**Реквизиты, используемые на скриншотах, являются демонстрационными. Не следует использовать их при тестировании!**

Быстрый переход:

- [Выписки](#запрос-выписки-в-интерфейсе-1С)
- [Платежи](#отправка-платежа-в-интерфейсе-1С)
- [Зарплатная ведомость](#создание-ведомости-в-банк-в-интерфейсе-1С)
- [Заявка на открытие лицевого счёта](#создание-заявки-на-открытие-лицевых-счетов)

### Запрос выписки в интерфейсе 1С

1.1. Первый способ: На форме выбрать "**Банковские выписки**". Заполнить поля "**Банковский счёт**" и "**Организация**"

1.2. Нажать кнопку "**Загрузить**"

![Пример](./examples/Запрос%20выписки%201.png "Пример запроса выписки")

2.1. Второй способ: запросить выписку за конкретную дату/период. На вкладке "**Банковские выписки**" нажать кнопку "**Ещё**", пролистать вниз, выбрать "**Обмен с банком**"

![Пример](./examples/Запрос%20выписки%202.png "Пример запроса выписки")

2.2. Заполнить поля "**Банковский счёт**", "**Организация**" и "**Период**". Нажать кнопку "**Запросить выписку**" 

![Пример](./examples/Запрос%20выписки%203.png "Пример запроса выписки")



### Отправка платежа в интерфейсе 1С

1. Перейти на вкладку **"Платежные документы".** Нажать кнопку "**Создать**"

![Пример](./examples/Отправка%20платежа%201.png "Пример отправки платежа")

2. Заполнить все необходимые поля (поле "**Номер"** заполнится автоматически)

![Пример](./examples/Отправка%20платежа%202.png "Пример отправки платежа")

3. Нажать "**Записать**", после "**Провести и закрыть**"

4. В списке документов найти свой, нажать "**Сформировать**" и "**Подписать**"

![Пример](./examples/Отправка%20платежа%203.png "Пример отправки платежа")

5. Выбрать сертификат, ввести пароль

![Пример](./examples/Отправка%20платежа%204.png "Пример отправки платежа")



### Создание ведомости в банк в интерфейсе 1С

<div class="content" style="border: 1px solid #dbdbd6;
    margin-bottom: 1.25em;
    padding: 1.25em;
    background: #f3f3f2;
    border-radius: 4px;">
<div class="title" style="font-size: 19pt; color: #7a2518; margin-top: 0; text-align: center;">Обратите внимание!</div>
<div class="paragraph">
<p><b>Для настройки отправки зарплатных ведомостей и заявок на открытие лицевых счетов используются реквизиты для тестирования зарплатного проекта (п.2.2). Остальные поля, данных для которых нет в таблице с реквизитами, можно заполнять также, как на скриншотах.</b></p>
</div>
</div>

1. Создать организацию в справочнике "**Организации**" (**Настройка → блок "Предприятие" → Организации → Создать**) и заполнить обязательные поля

![Пример](./examples/Отправка%20зп%20ведомости%201.png "Пример отправки зарплатной ведомости")

2. Создать зарплатный проект (**Выплаты → зарплатные проекты → Создать**) и заполнить обязательные поля

![Пример](./examples/Отправка%20зп%20ведомости%202.png "Пример отправки зарплатной ведомости")

В поле "Номер" договора записывается значение "**orid**" (идентификатор зарплатного проекта)

3. Создать должность (**Кадры → блок "Штатное расписание" → Должности**) и заполнить обязательные поля

![Пример](./examples/Отправка%20зп%20ведомости%204.png "Пример отправки зарплатной ведомости")

4. Создать подразделение организации (**Кадры → блок "Штатное расписание" → Подразделения → Создать**) и заполнить обязательные поля

![Пример](./examples/Отправка%20зп%20ведомости%2014.png "Пример отправки зарплатной ведомости")

5. Создать сотрудника (**Кадры → Сотрудник**) и заполнить обязательные поля

![Пример](./examples/Отправка%20зп%20ведомости%203.png "Пример отправки зарплатной ведомости")

6. Создать документ "**Прием на работу**" (**Кадры → Прием на работу**) и заполнить обязательные поля (на вкладках "Главное" и "Оплата труда")

![Пример](./examples/Отправка%20зп%20ведомости%2011.png "Пример отправки зарплатной ведомости")

![Пример](./examples/Отправка%20зп%20ведомости%2012.png "Пример отправки зарплатной ведомости")

7. Создать штатное расписание для должности (**Кадры → блок "Штатное расписание" → Штатное расписание → Создать**) и заполнить обязательные поля

![Пример](./examples/Отправка%20зп%20ведомости%205.png "Пример отправки зарплатной ведомости")

8. Создать документ "**Ведомость в банк**" (**Выплаты → Ведомость в банк → Создать**)

![Пример](./examples/Отправка%20зп%20ведомости%2010.png "Пример отправки зарплатной ведомости")

Заполнить ранее созданными объектами (Организация, Зарплатный проект, Сотрудник) и указать сумму "К выплате"

9. Отравить документ через 1С:ДиректБанк (**1С:ДиректБанк → Отравить электронный документ**) с указанием логина/пароля.

### Создание запроса статуса ведомости в банк

1. Перейти в ведомость в банк для которой требуется узнать статус

2. 1С: ДиректБанк → Открыть электронные документы → Cинхронизировать с банком

3. Выбрать электронный документ

![Пример](./examples/Создание%20запроса%20статуса%20зп%20ведомости1.png "Пример создания запроса статуса зарплатной ведомости")

4*. Альтернативный способ. **1С: ДиректБанк → Просмотреть электронные документы → Запросить состояние**

![Пример](./examples/Создание%20запроса%20статуса%20зп%20ведомости2.png "Пример создания запроса статуса зарплатной ведомости")



### Создание заявки на открытие лицевых счетов

1. Создать организацию в справочнике **"Организации" (Настройка → блок "Предприятие" → Организации → Создать)** и заполнить обязательные поля

![Пример](./examples/Отправка%20зп%20ведомости%201.png "Пример отправки заявки на открытие лицевых счетов")

2. Создать зарплатный проект **(Выплаты → зарплатные проекты → Создать)** и заполнить обязательные поля

![Пример](./examples/Отправка%20зп%20ведомости%202.png "Пример отправки заявки на открытие лицевых счетов")

В поле "Номер" договора записывается значение "orid" (идентификатор зарплатного проекта, получаемый из АЗОН)

3. Создать должность **(Кадры → блок "Штатное расписание" → Должности)** и заполнить обязательные поля

![Пример](./examples/Отправка%20зп%20ведомости%204.png "Пример отправки заявки на открытие лицевых счетов")

4. Создать подразделение организации **(Кадры → блок "Штатное расписание" → Подразделения → Создать)** и заполнить обязательные поля

![Пример](./examples/Отправка%20зп%20ведомости%2014.png "Пример отправки заявки на открытие лицевых счетов")

5. Создать сотрудника **(Кадры → Сотрудник)** и заполнить обязательные поля

![Пример](./examples/Отправка%20зп%20ведомости%203.png "Пример отправки заявки на открытие лицевых счетов")

6. Создать документ **"Прием на работу" (Кадры → Прием на работу)** и заполнить обязательные поля (на вкладках "Главное" и "Оплата труда")

![Пример](./examples/Отправка%20зп%20ведомости%2011.png "Пример отправки заявки на открытие лицевых счетов")

![Пример](./examples/Отправка%20зп%20ведомости%2012.png "Пример отправки заявки на открытие лицевых счетов")

7. Создать штатное расписание для должности **(Кадры → блок "Штатное расписание" → Штатное расписание → Создать)** и заполнить обязательные поля

![Пример](./examples/Отправка%20зп%20ведомости%205.png "Пример отправки заявки на открытие лицевых счетов")

8. Создать документ "Заявка на открытие лицевых счетов" и заполнить все обязательные поля на каждой вкладке

![Пример](./examples/Отправка%20зп%20ведомости%2015.png "Пример отправки заявки на открытие лицевых счетов")



## 4. Справочная информация и поддержка 

В случае возникновения вопросов/проблем вы можете:

- Найти решение своего вопроса на [странице вопросов к документации](https://github.com/Host-to-Host-Instructions/alfalink-1c-directbank-via-1c/issues)

- Создать тикет на [странице документации](https://github.com/Host-to-Host-Instructions/alfalink-1c-directbank-via-1c/issues). Для этого нужно нажать на кнопку New issue, указать в заголовке краткое описание проблемы, а потом описать проблему полностью, при необходимости, приложив скриншоты.

- Отправить свой вопрос по почте на адрес: 
    - ASKhramtsova@alfabank.ru (Анастасия Храмцова) 
    - в копию (обязательно):
        - AKopyltsova@alfabank.ru (Анна Копыльцова)
        - alfa-link@alfabank.ru 
