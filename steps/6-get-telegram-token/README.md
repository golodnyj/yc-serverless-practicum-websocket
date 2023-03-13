## Получите token для бота с помощью BotFather

В телеграме найдите `BotFather`, вызовите команду `/newbot`, задайте имя новому боту. 
В нашем случае для — `Serverless Game With WebSockets`, зададим username — `ServerlessGameWithWebSocketsBot` 
(он должен быть уникальным и заканчиваться на Bot), 
в результате получим адрес  `t.me/ServerlessGameWithWebSocketsBot` и `token`, 
запомним его, он потребуется на следующих этапах:

    echo "export TG_BOT_TOKEN=<token>" >> ~/.bashrc && . ~/.bashrc  
    echo $TG_BOT_TOKEN

# [cледующий этап >>>](../7-create-lockbox/README.md)
