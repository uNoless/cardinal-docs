---
title: Хаб
---
# Рекомендуется начать отсюда [[Introduction]]

# FunPay Cardinal: Справочник разработчика плагинов

## Анатомия плагина и жизненный цикл
- Обязательные метаданные в шапке файла плагина (NAME, VERSION, DESCRIPTION, CREDITS, UUID, SETTINGS_PAGE).
- Точки связывания и хуки: списки BIND_TO_* в [[Cardinal#Cardinal| cardinal]]
- Архитектура получения событий: как фоновый [[Runner]] опрашивает FunPay и передает данные в диспетчер [[Handlers]].
- Доступ к ядру [[Cardinal#Cardinal|Cardinal]], сессии [[Аccount#Account|Account]]

## Шина событий
Оформи список связок BIND_TO со ссылками на события и типы (подробное описание самих событий смотри в [[Events]]):
- **Событие нового заказа**
  - Хук: BIND_TO_NEW_ORDER в [[Cardinal]]
  - Событие: [[Events#NewOrderEvent|NewOrderEvent]]
  - Полезная нагрузка: [[Types#Order|Order]]
  - Диспетчер: [[Handlers#new_order_handler|new_order_handler]]
- **Событие изменения статуса заказа**
  - Хук: BIND_TO_ORDER_STATUS_CHANGED в [[Cardinal]]
  - Событие: [[Events#OrderStatusChangedEvent|OrderStatusChangedEvent]]
  - Полезная нагрузка: [[Types#Order|Order]]
  - Диспетчер: [[Handlers#order_status_changed_handler|order_status_changed_handler]]
- **Событие входящего сообщения**
  - Хук: BIND_TO_NEW_MESSAGE в [[Cardinal]]
  - Событие: [[Events#NewMessageEvent|NewMessageEvent]]
  - Полезная нагрузка: [[Types#Message|Message]]
  - Диспетчер: [[Handlers#new_message_handler|new_message_handler]]
- **Событие автоподъема лотов**
  - Хук: BIND_TO_POST_LOTS_RAISE в [[Cardinal]]
  - Событие: [[Events#LotsRaiseEvent|LotsRaiseEvent]]

## 3. Рецепты и сценарии разработки
Пошаговые списки методов для частых задач:
- **Отправка сообщений и файлов**:
  - Быстрая отправка через ядро: [[Cardinal#send_message|cardinal.send_message()]]
  - Отправка изображений в чат: [[Аccount#send_image|account.send_image()]]
- **Обработка и сопровождение заказов**:
  - Получение полной карточки заказа: [[Аccount#get_order|account.get_order()]]
  - Проверка статуса через перечисления: [[Enums#OrderStatuses|OrderStatuses]]
  - Оформление возврата средств: [[Аccount#refund|account.refund()]]
- **Управление лотами и ценами**:
  - Получение полей лота для редактирования: [[Аccount#get_lot_fields|account.get_lot_fields()]]
  - Сохранение измененного лота на FunPay: [[Аccount#save_lot|account.save_lot()]]
  - Получение лотов подкатегории: [[Аccount#get_my_subcategory_lots|account.get_my_subcategory_lots()]]
- **Telegram UI и админ-панель**:
  - Регистрация команд плагина в меню: [[Cardinal#add_telegram_commands|cardinal.add_telegram_commands()]]
  - Регистрация хэндлеров команд и сообщений: методы [[Bot]]
  - Фильтрация и префиксы колбэков: константы из [[CBT]]

## Слой данных и перечисления
- **Модели данных**: список основных классов из [[Types]] со ссылками ([[Types#Order|Order]], [[Types#UserProfile|UserProfile]], [[Types#Message|Message]], [[Types#LotFields|LotFields]], [[Types#Chat|Chat]], [[Types#Lot|Lot]]).
- **Системные перечисления**: список enum-классов из [[Enums]] со ссылками ([[Enums#OrderStatuses|OrderStatuses]], [[Enums#SubCategoryTypes|SubCategoryTypes]], [[Enums#ChatTypes|ChatTypes]], [[Enums#MessageTypes|MessageTypes]]).

## Исключения и служебные утилиты
- **Иерархия ошибок**:
  - Внутренние ошибки ядра: список исключений из [[Exceptions]] со ссылками [[Exceptions#ИмяОшибки|ИмяОшибки]].
  - Ошибки API и сети FunPay: список исключений из [[FP_exceptions]] со ссылками [[FP_exceptions#ИмяОшибки|ИмяОшибки]].
- **Логирование**: вывод сообщений через систему [[Logger#Logger|logger]].
- **Вспомогательные функции**: хелперы бота из [[Utils]] и утилиты парсинга из [[FP_utils]].
