
## Архитектурная роль
Модуль **handlers** отвечает за обработку событий, связанных с чатами, сообщениями и заказами, и взаимодействует с другими компонентами системы, такими как [[Cardinal#Cardinal|Cardinal]], для выполнения различных операций, таких как отправка уведомлений, логирование и обработка заказов.

## Компоненты и логика

### `LAST_STACK_ID`
* **Назначение:** Константа, используемая для хранения последнего стека сообщений.
* **Аргументы и связи:** Нет.
* **Взаимодействие:** Нет.

### `MSG_LOG_LAST_STACK_ID`
* **Назначение:** Константа, используемая для хранения последнего стека сообщений для логирования.
* **Аргументы и связи:** Нет.
* **Взаимодействие:** Нет.

### `ORDER_HTML_TEMPLATE`
* **Назначение:** Константа, используемая для хранения шаблона HTML для заказов.
* **Аргументы и связи:** Нет.
* **Взаимодействие:** Нет.

### `save_init_chats_handler`
* **Назначение:** Кэширует существующие чаты, чтобы не отправлять приветственные сообщения.
* **Аргументы и связи:**
 * **c** ([[Cardinal#Cardinal|Cardinal]]): Объект кардинала.
 * **e** ([[Events#InitialChatEvent|InitialChatEvent]]): Событие инициализации чата.
* **Взаимодействие:** [[Cardinal#Cardinal|Cardinal]], [[Events#InitialChatEvent|InitialChatEvent]].

### `update_threshold_on_initial_chat`
* **Назначение:** Обновляет пороговое значение для определения новых чатов.
* **Аргументы и связи:**
 * **c** ([[Cardinal#Cardinal|Cardinal]]): Объект кардинала.
 * **e** ([[Events#InitialChatEvent|InitialChatEvent]]): Событие инициализации чата.
* **Взаимодействие:** [[Cardinal#Cardinal|Cardinal]], [[Events#InitialChatEvent|InitialChatEvent]].

### `old_log_msg_handler`
* **Назначение:** Логирует полученное сообщение.
* **Аргументы и связи:**
 * **c** ([[Cardinal#Cardinal|Cardinal]]): Объект кардинала.
 * **e** ([[Events#LastChatMessageChangedEvent|LastChatMessageChangedEvent]]): Событие изменения последнего сообщения чата.
* **Взаимодействие:** [[Cardinal#Cardinal|Cardinal]], [[Events#LastChatMessageChangedEvent|LastChatMessageChangedEvent]].

### `log_msg_handler`
* **Назначение:** Логирует сообщение.
* **Аргументы и связи:**
 * **c** ([[Cardinal#Cardinal|Cardinal]]): Объект кардинала.
 * **e** ([[Events#NewMessageEvent|NewMessageEvent]]): Событие нового сообщения.
* **Взаимодействие:** [[Cardinal#Cardinal|Cardinal]], [[Events#NewMessageEvent|NewMessageEvent]].

### `update_threshold_on_last_message_change`
* **Назначение:** Обновляет пороговое значение для определения новых чатов.
* **Аргументы и связи:**
 * **c** ([[Cardinal#Cardinal|Cardinal]]): Объект кардинала.
 * **e** ([[Events#LastChatMessageChangedEvent|LastChatMessageChangedEvent]] | [[Events#NewMessageEvent|NewMessageEvent]]): Событие изменения последнего сообщения чата или нового сообщения.
* **Взаимодействие:** [[Cardinal#Cardinal|Cardinal]], [[Events#LastChatMessageChangedEvent|LastChatMessageChangedEvent]], [[Events#NewMessageEvent|NewMessageEvent]].

### `greetings_handler`
* **Назначение:** Отправляет приветственное сообщение.
* **Аргументы и связи:**
 * **c** ([[Cardinal#Cardinal|Cardinal]]): Объект кардинала.
 * **e** ([[Events#NewMessageEvent|NewMessageEvent]] | [[Events#LastChatMessageChangedEvent|LastChatMessageChangedEvent]]): Событие нового сообщения или изменения последнего сообщения чата.
* **Взаимодействие:** [[Cardinal#Cardinal|Cardinal]], [[Events#NewMessageEvent|NewMessageEvent]], [[Events#LastChatMessageChangedEvent|LastChatMessageChangedEvent]].

### `add_old_user_handler`
* **Назначение:** Добавляет пользователя в список написавших.
* **Аргументы и связи:**
 * **c** ([[Cardinal#Cardinal|Cardinal]]): Объект кардинала.
 * **e** ([[Events#NewMessageEvent|NewMessageEvent]] | [[Events#LastChatMessageChangedEvent|LastChatMessageChangedEvent]]): Событие нового сообщения или изменения последнего сообщения чата.
* **Взаимодействие:** [[Cardinal#Cardinal|Cardinal]], [[Events#NewMessageEvent|NewMessageEvent]], [[Events#LastChatMessageChangedEvent|LastChatMessageChangedEvent]].

### `send_response_handler`
* **Назначение:** Проверяет, является ли сообщение командой, и если да, отправляет ответ на данную команду.
* **Аргументы и связи:**
 * **c** ([[Cardinal#Cardinal|Cardinal]]): Объект кардинала.
 * **e** ([[Events#NewMessageEvent|NewMessageEvent]] | [[Events#LastChatMessageChangedEvent|LastChatMessageChangedEvent]]): Событие нового сообщения или изменения последнего сообщения чата.
* **Взаимодействие:** [[Cardinal#Cardinal|Cardinal]], [[Events#NewMessageEvent|NewMessageEvent]], [[Events#LastChatMessageChangedEvent|LastChatMessageChangedEvent]].

### `old_send_new_msg_notification_handler`
* **Назначение:** Отправляет уведомление о новом сообщении в телеграм.
* **Аргументы и связи:**
 * **c** ([[Cardinal#Cardinal|Cardinal]]): Объект кардинала.
 * **e** ([[Events#LastChatMessageChangedEvent|LastChatMessageChangedEvent]]): Событие изменения последнего сообщения чата.
* **Взаимодействие:** [[Cardinal#Cardinal|Cardinal]], [[Events#LastChatMessageChangedEvent|LastChatMessageChangedEvent]].

### `send_new_msg_notification_handler`
* **Назначение:** Отправляет уведомление о новом сообщении в телеграм.
* **Аргументы и связи:**
 * **c** ([[Cardinal#Cardinal|Cardinal]]): Объект кардинала.
 * **e** ([[Events#NewMessageEvent|NewMessageEvent]]): Событие нового сообщения.
* **Взаимодействие:** [[Cardinal#Cardinal|Cardinal]], [[Events#NewMessageEvent|NewMessageEvent]].

### `send_review_notification`
* **Назначение:** Отправляет уведомление о новом отзыве.
* **Аргументы и связи:**
 * **c** ([[Cardinal#Cardinal|Cardinal]]): Объект кардинала.
 * **order** ([[Types#Order|Order]]): Объект заказа.
 * **chat_id** (`int`): ID чата.
 * **reply_text** (`str | None`): Текст ответа на отзыв.
* **Взаимодействие:** [[Cardinal#Cardinal|Cardinal]], [[Types#Order|Order]].

### `process_review_handler`
* **Назначение:** Обрабатывает новый отзыв.
* **Аргументы и связи:**
 * **c** ([[Cardinal#Cardinal|Cardinal]]): Объект кардинала.
 * **e** ([[Events#NewMessageEvent|NewMessageEvent]] | [[Events#LastChatMessageChangedEvent|LastChatMessageChangedEvent]]): Событие нового сообщения или изменения последнего сообщения чата.
* **Взаимодействие:** [[Cardinal#Cardinal|Cardinal]], [[Events#NewMessageEvent|NewMessageEvent]], [[Events#LastChatMessageChangedEvent|LastChatMessageChangedEvent]].

### `send_command_notification_handler`
* **Назначение:** Отправляет уведомление о введенной команде в телеграм.
* **Аргументы и связи:**
 * **c** ([[Cardinal#Cardinal|Cardinal]]): Объект кардинала.
 * **e** ([[Events#NewMessageEvent|NewMessageEvent]] | [[Events#LastChatMessageChangedEvent|LastChatMessageChangedEvent]]): Событие нового сообщения или изменения последнего сообщения чата.
* **Взаимодействие:** [[Cardinal#Cardinal|Cardinal]], [[Events#NewMessageEvent|NewMessageEvent]], [[Events#LastChatMessageChangedEvent|LastChatMessageChangedEvent]].

### `test_auto_delivery_handler`
* **Назначение:** Выполняет тест автовыдачи.
* **Аргументы и связи:**
 * **c** ([[Cardinal#Cardinal|Cardinal]]): Объект кардинала.
 * **e** ([[Events#NewMessageEvent|NewMessageEvent]] | [[Events#LastChatMessageChangedEvent|LastChatMessageChangedEvent]]): Событие нового сообщения или изменения последнего сообщения чата.
* **Взаимодействие:** [[Cardinal#Cardinal|Cardinal]], [[Events#NewMessageEvent|NewMessageEvent]], [[Events#LastChatMessageChangedEvent|LastChatMessageChangedEvent]].

### `send_categories_raised_notification_handler`
* **Назначение:** Отправляет уведомление о поднятии лотов в Telegram.
* **Аргументы и связи:**
 * **c** ([[Cardinal#Cardinal|Cardinal]]): Объект кардинала.
 * **cat** ([[Types#Category|Category]]): Категория.
 * **error_text** (`str`): Текст ошибки.
* **Взаимодействие:** [[Cardinal#Cardinal|Cardinal]], [[Types#Category|Category]].

### `get_lot_config_by_name`
* **Назначение:** Ищет секцию лота в конфиге автовыдачи.
* **Аргументы и связи:**
 * **c** ([[Cardinal#Cardinal|Cardinal]]): Объект кардинала.
 * **name** (`str`): Название лота.
* **Возвращаемое значение:** Секция конфига или None.
* **Взаимодействие:** [[Cardinal#Cardinal|Cardinal]].

### `check_products_amount`
* **Назначение:** Проверяет количество продуктов.
* **Аргументы и связи:**
 * **config_obj** (`configparser.SectionProxy`): Объект секции конфига.
* **Возвращаемое значение:** Количество продуктов.
* **Взаимодействие:** Нет.

### `log_new_order_handler`
* **Назначение:** Логирует новый заказ.
* **Аргументы и связи:**
 * **c** ([[Cardinal#Cardinal|Cardinal]]): Объект кардинала.
 * **e** ([[Events#NewOrderEvent|NewOrderEvent]]): Событие нового заказа.
 * `*args`: Дополнительные аргументы.
* **Взаимодействие:** [[Cardinal#Cardinal|Cardinal]], [[Events#NewOrderEvent|NewOrderEvent]].

### `setup_event_attributes_handler`
* **Назначение:** Устанавливает атрибуты события.
* **Аргументы и связи:**
 * **c** ([[Cardinal#Cardinal|Cardinal]]): Объект кардинала.
 * **e** ([[Events#NewOrderEvent|NewOrderEvent]]): Событие нового заказа.
 * `*args`: Дополнительные аргументы.
* **Взаимодействие:** [[Cardinal#Cardinal|Cardinal]], [[Events#NewOrderEvent|NewOrderEvent]].

### `send_new_order_notification_handler`
* **Назначение:** Отправляет уведомления о новом заказе в телеграм.
* **Аргументы и связи:**
 * **c** ([[Cardinal#Cardinal|Cardinal]]): Объект кардинала.
 * **e** ([[Events#NewOrderEvent|NewOrderEvent]]): Событие нового заказа.
 * `*args`: Дополнительные аргументы.
* **Взаимодействие:** [[Cardinal#Cardinal|Cardinal]], [[Events#NewOrderEvent|NewOrderEvent]].

### `deliver_goods`
* **Назначение:** Доставляет товары.
* **Аргументы и связи:**
 * **c** ([[Cardinal#Cardinal|Cardinal]]): Объект кардинала.
 * **e** ([[Events#NewOrderEvent|NewOrderEvent]]): Событие нового заказа.
 * `*args`: Дополнительные аргументы.
* **Взаимодействие:** [[Cardinal#Cardinal|Cardinal]], [[Events#NewOrderEvent|NewOrderEvent]].

### `deliver_product_handler`
* **Назначение:** Обертка для `deliver_product()`, обрабатывающая ошибки.
* **Аргументы и связи:**
 * **c** ([[Cardinal#Cardinal|Cardinal]]): Объект кардинала.
 * **e** ([[Events#NewOrderEvent|NewOrderEvent]]): Событие нового заказа.
 * `*args`: Дополнительные аргументы.
* **Возвращаемое значение:** None.
* **Взаимодействие:** [[Cardinal#Cardinal|Cardinal]], [[Events#NewOrderEvent|NewOrderEvent]].

### `send_delivery_notification_handler`
* **Назначение:** Отправляет уведомление в телеграм об отправке товара.
* **Аргументы и связи:**
 * **c** ([[Cardinal#Cardinal|Cardinal]]): Объект кардинала.
 * **e** ([[Events#NewOrderEvent|NewOrderEvent]]): Событие нового заказа.
* **Взаимодействие:** [[Cardinal#Cardinal|Cardinal]], [[Events#NewOrderEvent|NewOrderEvent]].

### `update_current_lots`
* **Назначение:** Обновляет текущие лоты.
* **Аргументы и связи:**
 * **c** ([[Cardinal#Cardinal|Cardinal]]): Объект кардинала.
 * **e** ([[Events#NewOrderEvent|NewOrderEvent]]): Событие нового заказа.
* **Взаимодействие:** [[Cardinal#Cardinal|Cardinal]], [[Events#NewOrderEvent|NewOrderEvent]].

### `update_profile_lots`
* **Назначение:** Обновляет лоты в `c.profile`.
* **Аргументы и связи:**
 * **c** ([[Cardinal#Cardinal|Cardinal]]): Объект кардинала.
 * **e** ([[Events#NewOrderEvent|NewOrderEvent]]): Событие нового заказа.
* **Взаимодействие:** [[Cardinal#Cardinal|Cardinal]], [[Events#NewOrderEvent|NewOrderEvent]].

### `update_lot_state`
* **Назначение:** Обновляет состояние лота.
* **Аргументы и связи:**
 * **cardinal** ([[Cardinal#Cardinal|Cardinal]]): Объект кардинала.
 * **lot** ([[Types#LotShortcut|LotShortcut]]): Объект лота.
 * **task** (`int`): Задача (-1 - деактивировать лот, 1 - активировать лот).
* **Возвращаемое значение:** Результат выполнения.
* **Взаимодействие:** [[Cardinal#Cardinal|Cardinal]], [[Types#LotShortcut|LotShortcut]].

### `update_lots_states`
* **Назначение:** Обновляет состояния лотов.
* **Аргументы и связи:**
 * **cardinal** ([[Cardinal#Cardinal|Cardinal]]): Объект кардинала.
 * **event** ([[Events#NewOrderEvent|NewOrderEvent]]): Событие нового заказа.
* **Взаимодействие:** [[Cardinal#Cardinal|Cardinal]], [[Events#NewOrderEvent|NewOrderEvent]].

### `update_profiles_handler`
* **Назначение:** Обновляет информацию о профилях и состояния лотов в отдельном потоке.
* **Аргументы и связи:**
 * **cardinal** ([[Cardinal#Cardinal|Cardinal]]): Объект кардинала.
 * **event** ([[Events#NewOrderEvent|NewOrderEvent]] | [[Events#OrdersListChangedEvent|OrdersListChangedEvent]]): Событие нового заказа или изменения списка заказов.
 * `*args`: Дополнительные аргументы.
* **Взаимодействие:** [[Cardinal#Cardinal|Cardinal]], [[Events#NewOrderEvent|NewOrderEvent]], [[Events#OrdersListChangedEvent|OrdersListChangedEvent]].

### `send_thank_u_message_handler`
* **Назначение:** Отправляет ответное сообщение на подтверждение заказа.
* **Аргументы и связи:**
 * **cardinal** ([[Cardinal#Cardinal|Cardinal]]): Объект кардинала.
 * **event** ([[Events#OrderStatusChangedEvent|OrderStatusChangedEvent]]): Событие изменения статуса заказа.
* **Взаимодействие:** [[Cardinal#Cardinal|Cardinal]], [[Events#OrderStatusChangedEvent|OrderStatusChangedEvent]].

### `send_order_confirmed_notification_handler`
* **Назначение:** Отправляет уведомление о подтверждении заказа в Telegram.
* **Аргументы и связи:**
 * **cardinal** ([[Cardinal#Cardinal|Cardinal]]): Объект кардинала.
 * **event** ([[Events#OrderStatusChangedEvent|OrderStatusChangedEvent]]): Событие изменения статуса заказа.
* **Взаимодействие:** [[Cardinal#Cardinal|Cardinal]], [[Events#OrderStatusChangedEvent|OrderStatusChangedEvent]].

### `send_bot_started_notification_handler`
* **Назначение:** Отправляет уведомление о запуске бота в телеграм.
* **Аргументы и связи:**
 * **c** ([[Cardinal#Cardinal|Cardinal]]): Объект кардинала.
 * `*args`: Дополнительные аргументы.
* **Взаимодействие:** [[Cardinal#Cardinal|Cardinal]].

## Граф зависимостей
  - **Входящие зависимости:** [[Cardinal]], [[Logger]]
  - **Исходящие зависимости:** [[Cardinal]], [[Events]], [[Exceptions]], [[Types]], [[Utils]]