# Создание p2p счёта по API

Здесь рассматривается ситуация, когда нужно создать индивиуальную форму оплаты для p2p шлюза.

Для создания счёта необходимо отправить API запрос как указано здесь: [Форма регистрации платежа](README.md/#Форма-регистрации-платежа)

Дополнительно требуется указать параметр 'json' со значением 1 и 'p2p_type' со значением 'ANY' - перевод по номеру карты, 'SBP' - по системе быстрых переводов РФ или 'SBP_TG' - для трансграничного переводу через систему быстрых переводов.
Для значений p2p_type - может потребоваться передать имя и фамилию отправителя в параметре 'sender_name'.
В случае корректного создания счёта будет возвращён ответ следующего вида в формате json в зависимости от параметра p2p_type.

'ANY':

```json
{
  "amount": 600, // Сумма платежа
  "orderId": "1003", // Номер заказа
  "number": "007026-000271", // Номер созданного счета
  "until": "2023-07-12 01:57:48+0000", // Дата окончания приема оплат
  "cardNumber": "2201112223334455" // Номер карты для перевода 
} 
```

'SBP':

```json
{
  "amount": 600, // Сумма платежа
  "orderId": "1004", // Номер заказа
  "number": "007026-000272", // Номер созданного счета
  "until": "2023-07-12 02:00:53+0000", // Дата окончания приема оплат
  "receiverName": "Иванов Иван Иванович", // Получатель
  "receiverBank": "SBERBANK", // Банк получателя
  "receiverPhone": "+79998887766" // Номер телефона для перевода
} 
```
'SBP_TG':
```json
{
    "amount": 600, // Сумма платежа
    "orderId": "1005", // Номер заказа
    "number": "007026-000273", // Номер созданного счета
    "until": "2023-07-12 02:00:53+0000", // Дата окончания приема оплат
    "receiverName": "Иванов Шохрух", // Получатель
    "receiverBank": "БАНК ЭСХАТА", // Банк получателя
    "receiverPhone": "+992911223344" // Номер телефона для перевода
}
```

По полученным данным необходимо произвести перевод денежных средств не позднее даты 'until'. 

Информирование о статусе платежа описано здесь: [Проверка информации о платеже](README.md#проверка-информации-о-платеже)

Данный тип шлюза не позволяет производить оплату по уже существующему номеру заказа (orderId). В случае обнаружения такого платежа будет возвращена ошибка:

```json
{
  "status":false,
  "user_msg":"Error: Error! OrderId already exists."
}
```
