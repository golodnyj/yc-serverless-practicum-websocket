# Обновление Lockbox

Ранее мы создали секрет с именем `game-secrets` и поместили в него несколько переменных, 
сейчас мы можем обновить их значение. Для начала обновим переменную  

Чтобы задать значение переменной `YMQ_CAPTURE_QUEUE_URL` необходимо 
в веб-консоли зайти в созданную очередь `capturing-queue` и скопировать значение `URL`, 
подставьте ваше значение в следующую команду: 

    echo "export YMQ_CAPTURE_QUEUE_URL=<URL>" >> ~/.bashrc && . ~/.bashrc

Проверим что у нас есть все переменные для обновления значений секрета в LOCKBOX:

    echo $LOCKBOX_SECRET_ID
    echo $YMQ_WRITER_KEY_ID
    echo $YMQ_WRITER_KEY_SECRET
    echo $YDS_WRITER_KEY_ID
    echo $YDS_WRITER_KEY_SECRET
    echo $YMQ_CAPTURE_QUEUE_URL

    yc lockbox secret add-version --id $LOCKBOX_SECRET_ID \
    --payload "[{'key': 'ymq_writer_key_id', 'text_value': '$YMQ_WRITER_KEY_ID'},\
    {'key': 'ymq_writer_key_secret', 'text_value': '$YMQ_WRITER_KEY_SECRET'},\
    {'key': 'ymq_capture_queue_url', 'text_value': '$YMQ_CAPTURE_QUEUE_URL'},\
    {'key': 'yds_writer_key_id', 'text_value': '$YDS_WRITER_KEY_ID'},\
    {'key': 'yds_writer_key_secret', 'text_value': '$YDS_WRITER_KEY_SECRET'}]"

# Передеплой приложения

Находясь в корне клонированного проекта выполните передеплой приложения:

    npm run deploy

## Видео

https://youtu.be/R5sPKaFXfyE

# [cледующий этап >>>](../10-create-api-gw/README.md)
