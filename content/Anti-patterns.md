
## 1) Блокировка Event-loop

**Проблема**: Хэндлеры `BIND_TO_*` вызываются синхронно в основном потоке раннера. Если вызвать `time.sleep(10)` или долгий сетевой запрос к API, **весь бот перестанет принимать сообщения и заказы на 10 секунд**.

 ❌ **Плохо**:
```python
  def new_order_handler(cardinal: Cardinal, event: NewOrderEvent):
      time.sleep(15) # ЗАМОРОЗИТ ВСЕХ!
      requests.post("https://slow-api.com")
```

✅ **Правильно**:
``` python
import threading

def new_order_handler(cardinal: Cardinal, event: NewOrderEvent):
    # Выносим долгую работу в отдельный тред-демон
    threading.Thread(
        target=process_order_worker, 
        args=(cardinal, event), 
        daemon=True
    ).start()
```

## 2) Зацикливание авто ответов

- **Проблема**: Бот отвечает на свое собственное сообщение или сообщение другого плагина, входя в бесконечный цикл спама.
```python
def new_message_handler(cardinal: Cardinal, event: NewMessageEvent):
    # Игнорируем свои сообщения, автоответы и сообщения ботов
    if event.message.author_id == cardinal.account.id:
        return
    if getattr(event.message, "by_bot", False) or getattr(event.message, "is_autoreply", False):
        return
```

## 3) Потеря данных при одновременной записи в json

- **Проблема**: Два потока одновременно вызвали save_orders(), и один перезаписал файл поверх другого.
-  ✅ Решение:
``` python
_DB_LOCK = threading.RLock()

def save_orders(data):
    with _DB_LOCK:
        with open("orders.json", "w", encoding="utf-8") as f:
            json.dump(data, f, indent=4)

```

## 4) Спам методом `account.get_lot_fields()`
* **В чём ошибка:** Вызов [[Аccount#get_lot_fields|account.get_lot_fields()]] в цикле по всем лотам при каждом новом заказе или сообщении.
* **Почему ломается:** Каждый вызов загружает и парсит тяжелую HTML-страницу редактирования лота. Серия из десятков таких запросов замораживает процесс Cardinal на 1–2 минуты и приводит к временной блокировке IP со стороны FunPay (HTTP `429 Too Many Requests`).
* **Как исправить:** 
  1. Загружать поля лотов только точечно (для конкретного `lot_id`).
  2. Массовое сканирование лотов делать в фоновом потоке [[threading#Thread|threading.Thread]] с паузами `time.sleep(0.3)`.
  3. Хранить результат в словаре кэша в памяти и обновлять его по таймеру (раз в 15–30 минут) или по команде из Telegram.
