# Создание Yandex Data Streams
## Проверка ключа и конфигурации AWS CLI

Ранее был создан сервисный аккаунт `sls-deploy` с правами `editor`, мы используем его, 
чтобы создать экземпляр Yandex Message Queue.
Мы уже получили идентификатор ключа доступа и секретный ключ для сервисного аккаунта `sls-deploy`.

    echo $AWS_ACCESS_KEY_ID
    echo $AWS_SECRET_ACCESS_KEY
    aws configure list

## Создание экземпляра Yandex Data Streams

Используем уже настроенную утилиту aws для создания Yandex Data Streams: 

    echo $YDB_DATA_STREAMS_DATABASE

    aws kinesis create-stream \
    --endpoint https://yds.serverless.yandexcloud.net \
    --stream-name $YDB_DATA_STREAMS_DATABASE/notify-state-change \
    --shard-count 1

# [cледующий этап >>>](../5-get-telegram-token/README.md)
