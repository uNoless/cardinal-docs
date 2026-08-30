## Плагин

- Каждый плагин для FunPay Cardinal представляет собой отдельный `.py` файл, помещаемый в папку `plugins/`.
## Обязательные константы для плагина

-  **NAME** – Ваше название плагина | str
-  **VERSION** – Версия плагина | str
-  **DESCRIPTION** – Описание плагина | str
-  **CREDITS** – Для связи с автором плагина | str
-  **UUID** – Уникальный uuid v4 | str
-  **SETTINGS_PAGE** – Управление плагином с меню загрузки | bool
#### А также в конце плагина

- Глобальные переменные из [[Cardinal#`Cardinal`| cardinal]]
## Рекомендованная интеграция плагина с ботом

-  В кардинале для этого есть state машина, которая каждой кнопке дает стейт. Т.е. юзер жмет кнопку отправить ключ, на этой кнопке весит state = “sending_key”, то автоматически к следующему сообщению будет применен этот state. ***Но это долго муторно и не удобно.***
- В библиотеке для telegram бота, существует прекрасный способ декорировать любую функцию под ваши нужды:

``` Python
from telebot.types import InlineKeyboardMarkup as K, InlineKeyboardButton as B
import tg_bot.CBT as CBT

def init_cardinal(cardinal: Cardinal):
    bot = cardinal.telegram.bot

    # 1. Регистрация команд в меню /help Telegram-бота
    cardinal.add_telegram_commands(UUID, [
        ("mycmd", "Описание команды плагина", True)
    ])

    # 2. Обработчик текстовой команды
    @bot.message_handler(commands=["mycmd"])
    def cmd_mycmd(m):
        kb = K()
        # ВАЖНО: всегда добавляйте префикс UUID в callback_data, чтобы не пересекаться с другими плагинами!
        kb.add(B("🟢 Нажми меня", callback_data=f"{UUID}:my_action"))
        bot.send_message(m.chat.id, "Привет! Это меню моего плагина.", reply_markup=kb)

    # 3. Обработчик callback-кнопок плагина
    @bot.callback_query_handler(func=lambda c: c.data.startswith(f"{UUID}:"))
    def handle_callbacks(c):
        if c.data == f"{UUID}:my_action":
            bot.answer_callback_query(c.id, "Кнопка нажата!")
            bot.send_message(c.message.chat.id, "Действие выполнено ✅")

    # 4. Обработчик кнопки «Настройки» в общем списке плагинов бота
    @bot.callback_query_handler(func=lambda c: c.data.startswith(f"{CBT.PLUGIN_SETTINGS}:{UUID}") or c.data == f"{UUID}:0")
    def open_plugin_settings(c):
        bot.send_message(c.message.chat.id, "Главное меню плагина:")
```

## Полная информация о заказе

``` Python
(Весь объект заказа со всеми полями ниже)
order = cardinal.account.get_order(order_id="ABC12345")

print(order.price)          # Цена (float)
print(order.buyer_username) # Ник покупателя (str)
print(order.status)         # OrderStatus.PAID / CLOSED / REFUNDED
print(order.chat_id)        # ID чата с покупателем

```
## Возврат средств покупателю

``` Python
try:
    cardinal.account.refund(order_id="ABC12345")
    cardinal.send_message(chat_id, "✅ Средства успешно возвращены.")
except Exception as e:
    logger.error(f"Не удалось сделать возврат: {e}")
    
```

## Управление лотами

``` Python
# 1. Получаем текущие поля лота
lot_fields = cardinal.account.get_lot_fields(lot_id=12345678)

# 2. Меняем нужные свойства
lot_fields.active = False   # True = включить, False = выключить лот
lot_fields.price = 199.0    # Новая цена лота

# 3. Сохраняем изменения на FunPay
cardinal.account.save_lot(lot_fields)
```

## Получение списка лотов в категории

``` Python
# Получить все свои активные/неактивные лоты конкретной подкатегории (например, Telegram Stars):
lots = cardinal.account.get_my_subcategory_lots(subcategory_id=2418)
for lot in lots:
    print(lot.id, lot.title, lot.price, lot.active)
```

# Анти-паттерны, частые ошибки → [[Anti-patterns| Анти-Паттерны]]