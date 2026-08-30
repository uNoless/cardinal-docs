
## Архитектурная роль
Модуль **fp_exceptions** содержит набор классов исключений, которые используются для обработки и передачи ошибок, возникающих при взаимодействии с API сервиса FunPay. Эти исключения позволяют более детально описать типы ошибок и их причины, что упрощает отладку и обработку ошибок в приложениях, использующих FunPayAPI.

## Компоненты и логика

### `AccountNotInitiatedError`
* **Назначение:** Возбуждается, если предпринята попытка вызвать метод класса `FunPayAPI.account.Account` без предварительного получения данных аккаунта с помощью метода `FunPayAPI.account.Account.get`.
* **Аргументы и связи:** 
 * Нет аргументов.
* **Взаимодействие:** Нет.

### `RequestFailedError`
* **Назначение:** Возбуждается, если статус код ответа API не равен 200.
* **Аргументы и связи:** 
 * **response** (`requests.Response`): объект ответа.
* **Взаимодействие:** Наследуется классами [[FP_exceptions#UnauthorizedError|UnauthorizedError]], [[FP_exceptions#WithdrawError|WithdrawError]], [[FP_exceptions#RaiseError|RaiseError]], [[FP_exceptions#ImageUploadError|ImageUploadError]], [[FP_exceptions#MessageNotDeliveredError|MessageNotDeliveredError]], [[FP_exceptions#FeedbackEditingError|FeedbackEditingError]], [[FP_exceptions#LotParsingError|LotParsingError]], [[FP_exceptions#LotSavingError|LotSavingError]], [[FP_exceptions#RefundError|RefundError]].

### `UnauthorizedError`
* **Назначение:** Возбуждается, если не удалось найти идентифицирующий аккаунт элемент и / или произошло другое событие, указывающее на отсутствие авторизации.
* **Аргументы и связи:** 
 * **response** (`requests.Response`): объект ответа.
* **Взаимодействие:** Наследуется от [[FP_exceptions#RequestFailedError|RequestFailedError]].

### `WithdrawError`
* **Назначение:** Возбуждается, если произошла ошибка при попытке вывести средства с аккаунта.
* **Аргументы и связи:** 
 * **response** (`requests.Response`): объект ответа.
 * **error_message** (`str | None`): сообщение об ошибке.
* **Взаимодействие:** Наследуется от [[FP_exceptions#RequestFailedError|RequestFailedError]].

### `RaiseError`
* **Назначение:** Возбуждается, если произошла ошибка при попытке поднять лоты.
* **Аргументы и связи:** 
 * **response** (`requests.Response`): объект ответа.
 * **category** (`types.Category`): категория лотов.
 * **error_message** (`str | None`): сообщение об ошибке.
 * **wait_time** (`int | None`): время ожидания.
* **Взаимодействие:** Наследуется от [[FP_exceptions#RequestFailedError|RequestFailedError]].

### `ImageUploadError`
* **Назначение:** Возбуждается, если произошла ошибка при выгрузке изображения.
* **Аргументы и связи:** 
 * **response** (`requests.Response`): объект ответа.
 * **error_message** (`str | None`): сообщение об ошибке.
* **Взаимодействие:** Наследуется от [[FP_exceptions#RequestFailedError|RequestFailedError]].

### `MessageNotDeliveredError`
* **Назначение:** Возбуждается, если при отправке сообщения произошла ошибка.
* **Аргументы и связи:** 
 * **response** (`requests.Response`): объект ответа.
 * **error_message** (`str | None`): сообщение об ошибке.
 * **chat_id** (`int`): идентификатор чата.
* **Взаимодействие:** Наследуется от [[FP_exceptions#RequestFailedError|RequestFailedError]].

### `FeedbackEditingError`
* **Назначение:** Возбуждается, если при добавлении / редактировании / удалении отзыва / ответа на отзыв произошла ошибка.
* **Аргументы и связи:** 
 * **response** (`requests.Response`): объект ответа.
 * **error_message** (`str | None`): сообщение об ошибке.
 * **order_id** (`str`): идентификатор заказа.
* **Взаимодействие:** Наследуется от [[FP_exceptions#RequestFailedError|RequestFailedError]].

### `LotParsingError`
* **Назначение:** Возбуждается, если при получении полей лота произошла ошибка.
* **Аргументы и связи:** 
 * **response** (`requests.Response`): объект ответа.
 * **error_message** (`str | None`): сообщение об ошибке.
 * **lot_id** (`int`): идентификатор лота.
* **Взаимодействие:** Наследуется от [[FP_exceptions#RequestFailedError|RequestFailedError]].

### `LotSavingError`
* **Назначение:** Возбуждается, если при сохранении лота произошла ошибка.
* **Аргументы и связи:** 
 * **response** (`requests.Response`): объект ответа.
 * **error_message** (`str | None`): сообщение об ошибке.
 * **lot_id** (`int`): идентификатор лота.
 * **errors** (`dict[str, str]`): словарь ошибок.
* **Взаимодействие:** Наследуется от [[FP_exceptions#RequestFailedError|RequestFailedError]].

### `RefundError`
* **Назначение:** Возбуждается, если при возврате средств за заказ произошла ошибка.
* **Аргументы и связи:** 
 * **response** (`requests.Response`): объект ответа.
 * **error_message** (`str | None`): сообщение об ошибке.
 * **order_id** (`str`): идентификатор заказа.
* **Взаимодействие:** Наследуется от [[FP_exceptions#RequestFailedError|RequestFailedError]].

## Граф зависимостей
  - **Входящие зависимости:** [[Аccount]], [[Runner]]
  - **Исходящие зависимости:** [[Types]]