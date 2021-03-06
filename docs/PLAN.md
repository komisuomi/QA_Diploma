# План автоматизации тестирования сценария покупки тура через веб-сервис "Путешествие дня", взаимодействующий с СУБД и API Банка.

## 1. Перечень автоматизируемых сценариев
***ВАЖНО:** Перед началом работы необходимо запустить SUT. Инструкция приведена в файле [README.md](https://github.com/komisuomi/QA_Diploma/blob/master/README.md)*.

### Позитивные сценарии.
Валидные данные для заполнения поля "Владелец" генерируются с помощью Faker, "Месяц", "Год", "CVC/CVV" - стандартными методами Java для работы с датами и случайными числами, "Номер карты" - тестовыми данными из набора.

**Сценарий №1. Позитивный сценарий покупки тура по карте.**
1. Открыть веб-сервис ["Путешествие дня"](http://localhost:8080/).
2. Кликнуть по кнопке «Купить».
3. Заполнить поле «Номер карты» валидными данными.
4. Протестировать поле «Номер карты» по сценариям (см. сценарии для тестирования поля «Номер карты»).
5. Заполнить поле «Месяц» и поле «Год» валидными данными.
6. Протестировать поле «Месяц» по сценариям (см. сценарии для тестирования поле «Месяц»).
7. Протестировать поле «Год» по сценариям (см. сценарии для тестирования поле «Год»).
8. Заполнить поле «Владелец» валидными данными.
9. Протестировать поле «Владелец» по сценариям (см. сценарии для тестирования поля «Владелец»).
10. Заполнить поле «CVC/CVV» валидными данными.
11. Протестировать поле «CVC/CVV» по сценариям (см. сценарии для тестирования поля «CVC/CVV»).
12. Нажать кнопку «Продолжить».
13. Отправить JSON запрос с данной картой и получить статус  транзакциии из ответа от симулятора.
14. Проверить создание записи в БД в таблицу "order_entity" и получить из нее код payment_id, выборка записи с самой поздней датой и времени создания.
15. Проверить создание записи в БД в таблицу "payment_entity". Выборка записи с самой поздней датой и времени создания. Проверка значений полей transaction_id = payment_id (из таблицы "order_entity"), status равен статусу транзакции из ответа от симулятора, amount равно сумме покупки тура в копейках (4 500 000), полученного со стартовой страницы сервиса.
16. Проверить вывод сообщения от сервиса пользователю об успешно выполненной операции (если статус транзакции APPROVED) или выводе сообщения об отказе в выполнении операции банком (если статус транзакции DECLINED).

**Сценарий №2. Позитивный сценарий покупки тура по карте в кредит.**
1. Открыть веб-сервис ["Путешествие дня"](http://localhost:8080/).
2. Кликнуть по кнопке «Купить в кредит».
3. Заполнить поле «Номер карты» валидными данными.
4. Протестировать поле «Номер карты» по сценариям (см. сценарии для тестирования поля «Номер карты»).
5. Заполнить поле «Месяц» и поле «Год» валидными данными.
6. Протестировать поле «Месяц» по сценариям (см. сценарии для тестирования поле «Месяц»).
7. Протестировать поле «Год» по сценариям (см. Сценарии для тестирования поле «Год»).
8. Заполнить поле «Владелец» валидными данными.
9. Протестировать поле «Владелец» по сценариям (см. Сценарии для тестирования поля «Владелец»).
10. Заполнить поле «CVC/CVV» валидными данными.
11. Протестировать поле «CVC/CVV» по сценариям см. Сценарии для тестирования поля «CVC/CVV»).
12. Нажать кнопку «Продолжить».
13. Отправить JSON запрос с данной картой и получить статус транзакции из ответа от симулятора.
14. Проверить создание записи в БД в таблицу "order_entity" и получить из нее код credit_id, выборка записи с самой поздней датой и времени создания.
15. Проверить создание записи в БД в таблицу "credit_request_entity". Выборка записи с самой поздней датой и времени создания. Проверка значений полей bank_id = credit_id (из таблицы "order_entity"), status равен статусу транзакции из ответа от симулятора.
16. Проверить вывод сообщения от сервиса пользователю об успешно выполненной операции (если статус транзакции APPROVED) или выводе сообщения об отказе в выполнении операции банком (если статус транзакции DECLINED).

#### Негативные сценарии

##### ВАЖНО: при проверке одного поля на негативные сценарии, все остальные поля должны быть заполнены валидными данными. 
Поле "номер карты". В данном сервисе используется формат карты из 16 цифр:
- пустое поле;
- использование букв в поле;
- спецсимволы !"№;%:? и т.д.;
- номер карты, содержащий менее 16 цифр;
- номер карты, содержащий более 16 цифр.

Поле "месяц":
- пустое поле;
- использование букв в поле;
- спецсимволы !"№;%:? и т.д.;
- граничные значения 00 и 13.

Поле "год":
- пустое поле;
- использование букв в поле;
- спецсимволы !"№;%:? и т.д.;
- год истекшего срока карты;
- год превышающий более 5 лет от текущей даты.

Поле "владелец":
- пустое поле;
- использование цифр в поле;
- спецсимволы !"№;%:? и т.д.;
- использование русских символов.

Поле "CVC/CVV":
- пустое поле;
- спецсимволы !"№;%:? и т.д.;
- использование букв;
- код, состоящий из менее чем трёх цифр;
- код, состоящий из более чем трёх цифр.

## 2. Перечень используемых инструментов с обоснованием выбора

**1. Браузер Google Chrome v.87.0.4280.141**

Самый распространённый на сегодняшний день браузер.

**2. Java 11**

Популярная версия Java, содержащая все необходимые функции, которые позволяют повысить производительность при разработке и запуске программ Java.

**3. IntelliJ IDEA (Community или Ultimate Edition)**

Интегрированная среда разработки программного обеспечения для многих языков программирования. Обращает внимание на существующие ошибки и самостоятельно устраняет их, предоставляет варианты автоматического дополнения кода, избавляет от повседневной рутины и позволяет сконцентрироваться на более важных задачах.

**4. Junit 5**

Библиотека для модульного тестирования программного обеспечения на языке Java, применяется для написания авто-тестов и их запуска. Плюсы: широта возможностей и стабильность.

**5. Selenide**

Удобный инструмент для автотестов, построенный на базе Selenium. Он позволяет автоматически управлять браузером. При падении тестов автоматически делает скриншот. Плюсы: удобство и простота.

**6. Gradle.**

Система автоматической сборки, позволяет автоматизировать процесс сборки проекта и управлять зависимостями. Плюсы: производительность.

**7. Lombok.**

Плагин для аннотаций, значительно упрощает работу (автоматически генерирует конструкции, позволяет не указывать конкретный тип переменных).

**8. Docker**

Программное обеспечение для автоматизации развёртывания и управления приложениями в средах с поддержкой контейнеризации.

**9. Faker**

Плагин для генерации случайных тестовых данных. Легко настраивается, большое количество типов генерируемых данных.

**10. Appveyor**

Система CI. Быстро подключается, интегрируется с GitHub, наглядно можно посмотреть StackTrace.

**11. Allure**

Система подготовки отчётов о тестировании. Хорошая визуализация и информативность.

## 3. Перечень и описание возможных рисков при автоматизации

1. Сложность нахождения необходимых селекторов в структуре сайта.

2. В ситуации если графический интерфейс части сайта, отвечающего за создание заявки, претерпит изменения, то возникают большие риски обрушения всех созданных ранее тестов. Следовательно, при любых изменениях UI сайта, необходимо актуализировать все автотесты.

3. Риск выявления в ходе тестирования необходимости реализации дополнительных сценариев.

4. Большое количество обнаруженных багов - в этом случае потребуется дополнительное время на составление отчётов.

## 4. Интервальная оценка с учётом рисков 
№пп | Этап  |Интервальная оценка (час)
--- | --- | ---
1.|Авто-тесты, результаты их прогона| 70
2.|Отчётные документы по итогам тестирования| 20
3.|Отчётные документы по итогам автоматизации| 20

## 5. План сдачи работ 
1. Планирование автоматизации тестирования до 21.01.2021 г.	
2. Написание и выполнение тестов до 05.02.2021 г.
3. Подготовке отчётных документов по итогам автоматизированного тестирования до 12.02.2021 г.
4. Подготовка отчётных документов по итогам автоматизации до 16.02.2021 г.
