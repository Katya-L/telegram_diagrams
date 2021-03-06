sequenceDiagram

    actor U1 as Sender
    participant C as Client
    participant S as Smart-contract
    participant W as CryptoWallet
    actor U2 as Receiver 

    U1 ->> C: 1. Нажатие кнопки отправить
    C ->> C: 2. Запрос ввода имени пользователя получателя
    U1 ->> C: 3. Ввод имени пользователя
    C ->> S: 4. Запрос на подтверждение
    S ->> S: 5. Сопоставление имени пользователя адреса кошелька
    S ->> W: 6. Запрос на подтверждение адреса
    W ->> W: 7. Поиск кошелька
    W -->> S: 8. Подтверждение адреса
    S -->> C: 9. Подтверждение адреса

    C ->> S: 10. Запрос баланса пользователя
    S -->> C: 11. Баланс
    C ->> C: 12. Отображение баланса
    C ->> C: 13. Запрос ввода суммы перевода и валюты
    U1 ->> C: 14. Ввод суммы и валюты

    C ->> S: 15. Запрос размера газа
    S ->> W: 16. Запрос размера газа
    W -->> S: 17. Газ
    S -->> C: 18. Газ
    C ->> C: 19. Расчет итоговой суммы
    C ->> C: 20. Проверка достаточности средств с учетом оплаты газа

    alt Средств достаточно
        U1 ->> C: 21. Нажатие кнопки далее
        C ->> C: 22. Отображение газа и итоговой суммы
        U1 ->> C: 23. Подтверждение транзакции
        C ->> S: 24. Запрос на транзакцию
        S ->> S: 25. Проверка транзакции
        S ->> W: 26. Запрос на транзакцию
        W ->> W: 27. Проверка транзакции
        W ->> W: 28. Добавление транзакции в очередь
        W -->> S: 29. Статус: в ожидании
        S -->> C: 30. Статус: в ожидании
        C ->> C: 31. Отображение статуса

        W ->> W: 32. Блок с транзакцией добавлен в блокчейн
        W -->> S: 33. Транзакция
        S ->> S: 34. Транзакция добавлена в истории пользователей
        S ->> S: 35. Пересчет баланса пользователей
        S -->> C: 36. Транзакция

        par 
            C ->> U1: 37. Уведомление о транзакции
        and 
            C ->> U2: 37. Уведомление о транзакции
        end

    else Средств недостаточно
        C ->> C: 21a. Отображение сообщения ошибки
    end
    
