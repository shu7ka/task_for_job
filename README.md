```mermaid
flowchart TD
    Start([Начало: продавец в ЛК]) --> Select[Выбрать товар в Мои товары]
    Select --> Click[Нажать кнопку Опубликовать]
    Click --> CheckFields{Обязательные поля заполнены?}
    
    CheckFields -- Нет --> ErrorFields[Показать ошибку: Заполните все поля]
    ErrorFields --> End([Конец, статус не изменён])
    
    CheckFields -- Да --> CheckPrice{Цена > 0?}
    CheckPrice -- Нет --> ErrorPrice[Показать ошибку: Цена должна быть > 0]
    ErrorPrice --> End
    
    CheckPrice -- Да --> CheckName{Название 3-120 символов?}
    CheckName -- Нет --> ErrorName[Показать ошибку: Некорректная длина названия]
    ErrorName --> End
    
    CheckName -- Да --> CheckArt{Артикул уникален?}
    CheckArt -- Нет --> ErrorArt[Показать ошибку: Артикул уже существует]
    ErrorArt --> End
    
    CheckArt -- Да --> CheckStatus{Товар уже опубликован?}
    CheckStatus -- Да --> WarnPublished[Показать предупреждение: Товар уже опубликован]
    WarnPublished --> End
    
    CheckStatus -- Нет --> UpdateDB[Изменить статус в БД на Опубликован]
    UpdateDB --> SendEvent[Отправить событие на витрину асинхронно]
    SendEvent --> Success[Показать сообщение: Товар успешно опубликован]
    Success --> End
