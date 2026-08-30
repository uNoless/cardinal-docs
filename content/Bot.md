
## Архитектурная роль
Модуль **bot** отвечает за взаимодействие с Telegram API и управление состояниями пользователей. Он предоставляет методы для отправки уведомлений, обработки команд и настроек, а также интегрируется с другими компонентами системы FunPayCardinal через экземпляр [[Cardinal#Cardinal|Cardinal]].

## Компоненты и логика
### `TGBot`
* **Назначение:** Класс, представляющий бота для взаимодействия с Telegram API и управлением состояниями пользователей.
* **Аргументы и связи:**
 - **cardinal** ([[Cardinal#Cardinal|Cardinal]]) — доступ к функциям ядра и сессии.
### __init__(self, cardinal: [[Cardinal#Cardinal|Cardinal]])
* **Назначение:** Инициализирует экземпляр бота.
* **Аргументы и связи:**
 * **cardinal** ([[Cardinal#Cardinal|Cardinal]]): предоставляющий доступ к основным функциям системы.
- Методы:
  - bot: Доступ к боту (telebot)
  - authorized_users: Cписок авторизованных юзеров получаемый с помощью [[Utils#load_authorized_users| utils.load_authorized_users()]]

### `get_state(self, chat_id: int, user_id: int) -> dict | None`
* **Назначение:** Получает текущее состояние пользователя.
* **Аргументы и связи:**
 * **chat_id** (`int`): ID чата.
 * **user_id** (`int`): ID пользователя.

### `set_state(self, chat_id: int, message_id: int, user_id: int, state: str, data: dict | None=None)`
* **Назначение:** Устанавливает состояние для пользователя.
* **Аргументы и связи:**
 * **chat_id** (`int`): ID чата.
 * **message_id** (`int`): ID сообщения, после которого устанавливается данное состояние.
 * **user_id** (`int`): ID пользователя.
 * **state** (`str`): состояние.
 * **data** (`dict | None`): дополнительные данные.

### `clear_state(self, chat_id: int, user_id: int, del_msg: bool=False) -> int | None`
* **Назначение:** Очищает состояние пользователя.
* **Аргументы и связи:**
 * **chat_id** (`int`): ID чата.
 * **user_id** (`int`): ID пользователя.
 * **del_msg** (`bool`): удалять ли сообщение, после которого было обозначено текущее состояние.

### `check_state(self, chat_id: int, user_id: int, state: str) -> bool`
* **Назначение:** Проверяет, является ли состояние указанным.
* **Аргументы и связи:**
 * **chat_id** (`int`): ID чата.
 * user_id** (`int`): ID пользователя.
 * **state** (`str`): состояние.

### `is_notification_enabled(self, chat_id: int | str, notification_type: str) -> bool`
* **Назначение:** Проверяет, включен ли указанный тип уведомлений в указанном чате.
* **Аргументы и связи:**
 * **chat_id** (`int | str`): ID Telegram чата.
 * **notification_type** (`str`): тип уведомлений.

### `toggle_notification(self, chat_id: int, notification_type: str) -> bool`
* **Назначение:** Переключает указанный тип уведомлений в указанном чате и сохраняет настройки уведомлений.
* **Аргументы и связи:**
 * **chat_id** (`int`): ID Telegram чата.
 * **notification_type** (`str`): тип уведомлений.

### `is_file_handler(self, m: [[types#Message|Message]])`
* **Назначение:** Определяет, является ли сообщение файлом.
* **Аргументы и связи:**
 * **m** ([[Types#Message|Message]]): сообщение.

### `file_handler(self, state, handler)`
* **Назначение:** Обрабатывает файлы.
* **Аргументы и связи:**
 * **state**: состояние.
 * **handler**: обработчик.

### `run_file_handlers(self, m: [[types#Message|Message]])`
* **Назначение:** Запускает обработчики файлов.
* **Аргументы и связи:**
 * **m** ([[Types#Message|Message]]): сообщение.

### `msg_handler(self, handler, **kwargs)`
* **Назначение:** Регистрирует хэндлер, срабатывающий при новом сообщении.
* **Аргументы и связи:**
 * **handler**: хэндлер.
 * **kwargs**: аргументы для хэндлера.

### `cbq_handler(self, handler, func, **kwargs)`
* **Назначение:** Регистрирует хэндлер, срабатывающий при новом callback'е.
* **Аргументы и связи:**
 * **handler**: хэндлер.
 * **func**: функция-фильтр.
 * **kwargs**: аргументы для хэндлера.

### `mdw_handler(self, handler, **kwargs)`
* **Назначение:** Регистрирует промежуточный хэндлер.
* **Аргументы и связи:**
 * **handler**: хэндлер.
 * **kwargs**: аргументы для хэндлера.

### `setup_chat_notifications(self, bot: TGBot, m: [[types#Message|Message]])`
* **Назначение:** Устанавливает настройки уведомлений по умолчанию в новом чате.
* **Аргументы и связи:**
 * **bot** (`TGBot`): * **m** ([[Types#Message|Message]]): сообщение.

### `reg_admin(self, m: [[types#Message|Message]])`
* **Назначение:** Проверяет, есть ли пользователь в списке пользователей с доступом к ПУ TG.
* **Аргументы и связи:**
 * **m** ([[Types#Message|Message]]): сообщение.

### `ignore_unauthorized_users(self, c: `CallbackQuery`)`
* **Назначение:** Игнорирует callback'и от не авторизированных пользователей.
* **Аргументы и связи:**
 * **c** (`CallbackQuery`): callback.

### `send_settings_menu(self, m: [[types#Message|Message]])`
* **Назначение:** Отправляет основное меню настроек (новым сообщением).
* **Аргументы и связи:**
 * **m** ([[Types#Message|Message]]): сообщение.

### `send_profile(self, m: [[types#Message|Message]])`
* **Назначение:** Отправляет статистику аккаунта.
* **Аргументы и связи:**
 * **m** ([[Types#Message|Message]]): сообщение.

### `act_change_cookie(self, m: [[types#Message|Message]])`
* **Назначение:** Активирует режим ввода golden_key.
* **Аргументы и связи:**
 * **m** ([[Types#Message|Message]]): сообщение.

### `change_cookie(self, m: [[types#Message|Message]])`
* **Назначение:** Меняет golden_key аккаунта FunPay.
* **Аргументы и связи:**
 * **m** ([[Types#Message|Message]]): сообщение.

### `update_profile(self, c: `CallbackQuery`)`
* **Назначение:** Обновляет профиль.
* **Аргументы и связи:**
 * **c** (`CallbackQuery`): callback.

### `act_manual_delivery_test(self, m: [[types#Message|Message]])`
* **Назначение:** Активирует режим ввода названия лота для ручной генерации ключа теста автовыдачи.
* **Аргументы и связи:**
 * **m** ([[Types#Message|Message]]): сообщение.

### `manual_delivery_text(self, m: [[types#Message|Message]])`
* **Назначение:** Генерирует ключ теста автовыдачи (ручной режим).
* **Аргументы и связи:**
 * **m** ([[Types#Message|Message]]): сообщение.

### `act_ban(self, m: [[types#Message|Message]])`
* **Назначение:** Активирует режим ввода никнейма пользователя, которого нужно добавить в ЧС.
* **Аргументы и связи:**
 * **m** ([[Types#Message|Message]]): сообщение.

### `ban(self, m: [[types#Message|Message]])`
* **Назначение:** Добавляет пользователя в ЧС.
* **Аргументы и связи:**
 * **m** ([[Types#Message|Message]]): сообщение.

### `act_unban(self, m: [[types#Message|Message]])`
* **Назначение:** Активирует режим ввода никнейма пользователя, которого нужно удалить из ЧС.
* **Аргументы и связи:**
 * **m** ([[Types#Message|Message]]): сообщение.

### `unban(self, m: [[types#Message|Message]])`
* **Назначение:** Удаляет пользователя из ЧС.
* **Аргументы и связи:**
 * **m** ([[Types#Message|Message]]): сообщение.

### `send_ban_list(self, m: [[types#Message|Message]])`
* **Назначение:** Отправляет ЧС.
* **Аргументы и связи:**
 * **m** ([[Types#Message|Message]]): сообщение.

### `act_edit_watermark(self, m: [[types#Message|Message]])`
* **Назначение:** Активирует режим ввода вотемарки сообщений.
* **Аргументы и связи:**
 * **m** ([[Types#Message|Message]]): сообщение.

### `edit_watermark(self, m: [[types#Message|Message]])`
* **Назначение:** Изменяет вотемарку сообщений.
* **Аргументы и связи:**
 * **m** ([[Types#Message|Message]]): сообщение.

### `send_logs(self, m: [[types#Message|Message]])`
* **Назначение:** Отправляет файл логов.
* **Аргументы и связи:**
 * **m** ([[Types#Message|Message]]): сообщение.

### `del_logs(self, m: [[types#Message|Message]])`
* **Назначение:** Удаляет старые лог-файлы.
* **Аргументы и связи:**
 * **m** ([[Types#Message|Message]]): сообщение.

### `about(self, m: [[types#Message|Message]])`
* **Назначение:** Отправляет информацию о текущей версии бота.
* **Аргументы и связи:**
 * **m** ([[Types#Message|Message]]): сообщение.

### `check_updates(self, m: [[types#Message|Message]])`
* **Назначение:** Проверяет наличие обновлений.
* **Аргументы и связи:**
 * **m** ([[Types#Message|Message]]): сообщение.

### `get_backup(self, m: [[types#Message|Message]])`
* **Назначение:** Получает бэкап.
* **Аргументы и связи:**
 * **m** ([[Types#Message|Message]]): сообщение.

### `create_backup(self, m: [[types#Message|Message]])`
* **Назначение:** Создает бэкап.
* **Аргументы и связи:**
 * **m** ([[Types#Message|Message]]): сообщение.

### `update(self, m: [[types#Message|Message]])`
* **Назначение:** Обновляет бота.
* **Аргументы и связи:**
 * **m** ([[Types#Message|Message]]): сообщение.

### `send_system_info(self, m: [[types#Message|Message]])`
* **Назначение:** Отправляет информацию о нагрузке на систему.
* **Аргументы и связи:**
 * **m** ([[Types#Message|Message]]): сообщение.

### `restart_cardinal(self, m: [[types#Message|Message]])`
* **Назначение:** Перезапускает кардинал.
* **Аргументы и связи:**
 * **m** ([[Types#Message|Message]]): сообщение.

### `ask_power_off(self, m: [[types#Message|Message]])`
* **Назначение:** Просит подтверждение на отключение FPC.
* **Аргументы и связи:**
 * **m** ([[Types#Message|Message]]): сообщение.

### `cancel_power_off(self, c: `CallbackQuery`)`
* **Назначение:** Отменяет выключение (удаляет клавиатуру с кнопками подтверждения).
* **Аргументы и связи:**
 * **c** (`CallbackQuery`): callback.

### `power_off(self, c: `CallbackQuery`)`
* **Назначение:** Отключает FPC.
* **Аргументы и связи:**
 * **c** (`CallbackQuery`): callback.

### `act_send_funpay_message(self, c: `CallbackQuery`)`
* **Назначение:** Активирует режим ввода сообщения для отправки его в чат FunPay.
* **Аргументы и связи:**
 * **c** (`CallbackQuery`): callback.

### `send_funpay_message(self, message: [[types#Message|Message]])`
* **Назначение:** Отправляет сообщение в чат FunPay.
* **Аргументы и связи:**
 * **message** ([[Types#Message|Message]]): сообщение.

### `act_upload_image(self, m: [[types#Message|Message]])`
* **Назначение:** Активирует режим ожидания изображения для последующей выгрузки на FunPay.
* **Аргументы и связи:**
 * **m** ([[Types#Message|Message]]): сообщение.

### `act_upload_backup(self, m: [[types#Message|Message]])`
* **Назначение:** Активирует режим ожидания бэкапа.
* **Аргументы и связи:**
 * **m** ([[Types#Message|Message]]): сообщение.

### `act_edit_greetings_text(self, c: `CallbackQuery`)`
* **Назначение:** Активирует режим редактирования текста приветствия.
* **Аргументы и связи:**
 * **c** (`CallbackQuery`): callback.

### `edit_greetings_text(self, m: [[types#Message|Message]])`
* **Назначение:** Изменяет текст приветствия.
* **Аргументы и связи:**
 * **m** ([[Types#Message|Message]]): сообщение.

### `act_edit_greetings_cooldown(self, c: `CallbackQuery`)`
* **Назначение:** Активирует режим редактирования таймаута приветствия.
* **Аргументы и связи:**
 * **c** (`CallbackQuery`): callback.

### `edit_greetings_cooldown(self, m: [[types#Message|Message]])`
* **Назначение:** Изменяет таймаут приветствия.
* **Аргументы и связи:**
 * **m** ([[Types#Message|Message]]): сообщение.

### `act_edit_order_confirm_reply_text(self, c: `CallbackQuery`)`
* **Назначение:** Активирует режим редактирования текста подтверждения заказа.
* **Аргументы и связи:**
 * **c** (`CallbackQuery`): callback.

### `edit_order_confirm_reply_text(self, m: [[types#Message|Message]])`
* **Назначение:** Изменяет текст подтверждения заказа.
* **Аргументы и связи:**
 * **m** ([[Types#Message|Message]]): сообщение.

### `act_edit_review_reply_text(self, c: `CallbackQuery`)`
* **Назначение:** Активирует режим редактирования текста отзыва.
* **Аргументы и связи:**
 * **c** (`CallbackQuery`): callback.

### `edit_review_reply_text(self, m: [[types#Message|Message]])`
* **Назначение:** Изменяет текст отзыва.
* **Аргументы и связи:**
 * **m** ([[Types#Message|Message]]): сообщение.

### `open_reply_menu(self, c: `CallbackQuery`)`
* **Назначение:** Открывает меню ответа на сообщение (callback используется в кнопках "назад").
* **Аргументы и связи:**
 * **c** (`CallbackQuery`): callback.

### `extend_new_message_notification(self, c: `CallbackQuery`)`
* **Назначение:** "Расширяет" уведомление о новом сообщении.
* **Аргументы и связи:**
 * **c** (`CallbackQuery`): callback.

### `ask_confirm_refund(self, call: `CallbackQuery`)`
* **Назначение:** Просит подтвердить возврат денег.
* **Аргументы и связи:**
 * **call** (`CallbackQuery`): callback.

### `cancel_refund(self, call: `CallbackQuery`)`
* **Назначение:** Отменяет возврат.
* **Аргументы и связи:**
 * **call** (`CallbackQuery`): callback.

### `refund(self, c: `CallbackQuery`)`
* **Назначение:** Оформляет возврат за заказ.
* **Аргументы и связи:**
 * **c** (`CallbackQuery`): callback.

### `open_order_menu(self, c: `CallbackQuery`)`
* **Назначение:** Открывает меню заказа.
* **Аргументы и связи:**
 * **c** (`CallbackQuery`): callback.

### `open_cp(self, c: `CallbackQuery`)`
* **Назначение:** Открывает основное меню настроек (редактирует сообщение).
* **Аргументы и связи:**
 * **c** (`CallbackQuery`): callback.

### `open_cp2(self, c: `CallbackQuery`)`
* **Назначение:** Открывает 2 страницу основного меню настроек (редактирует сообщение).
* **Аргументы и связи:**
 * **c** (`CallbackQuery`): callback.

### `switch_param(self, c: `CallbackQuery`)`
* **Назначение:** Переключает переключаемые настройки FPC.
* **Аргументы и связи:**
 * **c** (`CallbackQuery`): callback.

### `switch_chat_notification(self, c: `CallbackQuery`)`
* **Назначение:** Переключает уведомления чата.
* **Аргументы и связи:**
 * **c** (`CallbackQuery`): callback.

### `open_settings_section(self, c: `CallbackQuery`)`
* **Назначение:** Открывает выбранную категорию настроек.
* **Аргументы и связи:**
 * **c** (`CallbackQuery`): callback.

### `cancel_action(self, call: `CallbackQuery`)`
* **Назначение:** Обнуляет состояние пользователя по кнопке "Отмена" (CBT.CLEAR_STATE).
* **Аргументы и связи:**
 * **call** (`CallbackQuery`): callback.

### `param_disabled(self, c: `CallbackQuery`)`
* **Назначение:** Отправляет сообщение о том, что параметр отключен в глобальных переключателях.
* **Аргументы и связи:**
 * **c** (`CallbackQuery`): callback.

### `send_announcements_kb(self, m: [[types#Message|Message]])`
* **Назначение:** Отправляет сообщение с клавиатурой управления уведомлениями о новых объявлениях.
* **Аргументы и связи:**
 * **m** ([[Types#Message|Message]]): сообщение.

### `send_review_reply_text(self, c: `CallbackQuery`)`
* **Назначение:** Отправляет текст ответа на отзыв.
* **Аргументы и связи:**
 * **c** (`CallbackQuery`): callback.

### `send_old_mode_help_text(self, c: `CallbackQuery`)`
* **Назначение:** Отправляет текст справки для старого режима.
* **Аргументы и связи:**
 * **c** (`CallbackQuery`): callback.

### `empty_callback(self, c: `CallbackQuery`)`
* **Назначение:** Обрабатывает пустые callback'и.
* **Аргументы и связи:**
 * **c** (`CallbackQuery`): callback.

### `switch_lang(self, c: `CallbackQuery`)`
* **Назначение:** Переключает язык.
* **Аргументы и связи:**
 * **c** (`CallbackQuery`): callback.

### `__register_handlers(self)`
* **Назначение:** Регистрирует хэндлеры всех команд.

### `send_notification(self, text: str | None, keyboard: K | None=None, notification_type: str=utils.[[utils#NotificationTypes|NotificationTypes]].other, photo: bytes | None=None, pin: bool=False)`
* **Назначение:** Отправляет сообщение во все чаты для уведомлений из self.notification_settings.
* **Аргументы и связи:**
 * **text** (`str | None`): текст уведомления.
 * **keyboard** (`K | None`): * **notification_type** (`str`): тип уведомления.
 * **photo** (`bytes | None`): фотография (если нужна).
 * **pin** (`bool`): закреплять ли сообщение.

### `add_command_to_menu(self, command: str, help_text: str) -> None`
* **Назначение:** Добавляет команду в список команд (в кнопке menu).
* **Аргументы и связи:**
 * **command** (`str`): текст команды.
 * **help_text** (`str`): текст справки.

### `setup_commands(self)`
* **Назначение:** Устанавливает меню команд.

### `edit_bot(self)`
* **Назначение:** Изменяет описания и название бота.

### `init(self)`
* **Назначение:** Инициализирует бота.

### `run(self)`
* **Назначение:** Запускает поллинг.

## Граф зависимостей
  - **Входящие зависимости:** [[Cardinal]]
  - **Исходящие зависимости:** [[CBT]], [[Аccount]], [[Cardinal]], [[Types]], [[Utils]]