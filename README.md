## Диаграмма процесса публикации товара

```mermaid
flowchart TD
    Start([Начало: продавец в ЛК]) --> Select[Выбрать товар в Мои товары]
    Select --> Click[Нажать кнопку Опубликовать]
    Click --> CheckFields{Обязательные поля\nзаполнены?}
    
    CheckFields -- Нет --> ErrorFields[Показать ошибку:\nЗаполните все поля]
    ErrorFields --> End([Конец, статус не изменён])
    
    CheckFields -- Да --> CheckPrice{Цена > 0?}
    CheckPrice -- Нет --> ErrorPrice[Показать ошибку:\nЦена должна быть > 0]
    ErrorPrice --> End
    
    CheckPrice -- Да --> CheckName{Название 3–120 симв.?}
    CheckName -- Нет --> ErrorName[Показать ошибку:\nНекорректная длина названия]
    ErrorName --> End
    
    CheckName -- Да --> CheckArt{Sku уникален?}
    CheckArt -- Нет --> ErrorArt[Показать ошибку:\nАртикул уже существует]
    ErrorArt --> End
    
    CheckArt -- Да --> CheckStatus{Товар уже\nопубликован?}
    CheckStatus -- Да --> WarnPublished[Показать предупреждение:\nТовар уже опубликован]
    WarnPublished --> End
    
    CheckStatus -- Нет --> UpdateDB[Изменить статус в БД\nна Опубликован]
    UpdateDB --> SendEvent[Отправить событие на витрину\n(асинхронно)]
    SendEvent --> Success[Показать сообщение:\nТовар успешно опубликован]
    Success --> End
```
