# Создание Yandex Data Streams
## Создание ключа доступа для сервисного аккаунта

Ранее были созданы сервисные аккаунты:
* `sls-deploy` с правами `editor`, мы используем, чтобы создать экземпляр Yandex Message Queue;
* `yds-reader-sa` с правами `yds.admin`, чтобы оперировать Yandex Data Stream
* `yds-writer-sa` с правами `yds.writer`, чтобы осуществлять запись в Yandex Data Stream

Этот этап нужен для получения идентификатора ключа доступа и секретного ключа для сервисного аккаунта `yds-writer-sa`.
Для создания ключа доступа необходимо вызвать следующую команду:

    yc iam access-key create --service-account-id $YDS_WRITER_SA_ID

Подставьте свои значения и сохраните переменные:

    echo "export YDS_WRITER_KEY_ID=<key_id>" >> ~/.bashrc && . ~/.bashrc
    echo $YDS_WRITER_KEY_ID

    echo "export YDS_WRITER_KEY_SECRET=<secret>" >> ~/.bashrc && . ~/.bashrc
    echo $YDS_WRITER_KEY_SECRET

## Создание экземпляра YDB для Yandex Data Streams

Для создания экземпляра Yandex Data Streams потребуется отдельный экземпляр YDB.
Создадим базу данных YDB с именем `data-streams` и типом serverless используя для этого флаг `--serverless`:

    yc ydb database create data-streams --serverless --folder-id $YC_FOLDER_ID
    yc ydb database list

Сразу получим и сохраним значение `endpoint` и `database` в значение переменной `YDB_DATA_STREAMS_ENDPOINT` и `YDB_DATA_STREAMS_DATABASE`.

    yc ydb database get --name data-streams

    echo "export YDB_DATA_STREAMS_ENDPOINT=<YDB_ENDPOINT>" >> ~/.bashrc && . ~/.bashrc
    echo $YDB_DATA_STREAMS_ENDPOINT

    echo "export YDB_DATA_STREAMS_DATABASE=<YDB_DATABASE>" >> ~/.bashrc && . ~/.bashrc
    echo $YDB_DATA_STREAMS_DATABASE

## Создание экземпляра Yandex Data Streams

Используем уже настроенную утилиту aws для создания Yandex Data Streams: 

    aws kinesis create-stream \
    --endpoint https://yds.serverless.yandexcloud.net \
    --stream-name $YDB_DATA_STREAMS_DATABASE/notify-state-change \
    --shard-count 1

# [cледующий этап >>>](../6-get-telegram-token/README.md)
