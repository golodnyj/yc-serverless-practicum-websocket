# Создание Lockbox

Создадим секрет с именем `game-secrets` и поместим пару переменных со значениями `YDB_ENDPOINT` и `YDB_DATABASE`

    yc lockbox secret create --name game-secrets \
    --description "The secrets for the serverless game" \
    --payload "[{'key': 'ydb_endpoint', 'text_value': $YDB_ENDPOINT},{'key': 'ydb_db', 'text_value': $YDB_DATABASE}]" \
    --cloud-id $YC_CLOUD_ID \
    --folder-id $YC_FOLDER_ID 

Сохраним ID созданного секрета, и можно подсмотреть и вставить ручками:
    
    yc lockbox secret list
    echo "export LOCKBOX_SECRET_ID=<ID>" >> ~/.bashrc && . ~/.bashrc

Или автоматизированно:

    echo "export LOCKBOX_SECRET_ID=$(jq -r <<<  \
    "$(yc lockbox secret list --format json | jq '.[]' -c | grep game-secrets)" .id)"  \
    >> ~/.bashrc && . ~/.bashrc

    echo $LOCKBOX_SECRET_ID

Но нам необходимо добавить еще несколько переменных, обновим наш секрет:

    yc lockbox secret add-version --id $LOCKBOX_SECRET_ID \
    --payload "[{'key': 'tg_bot_token', 'text_value': '$TG_BOT_TOKEN'}]"

    yc lockbox secret add-version --id $LOCKBOX_SECRET_ID \
    --payload "[{'key': 'ymq_writer_key_id', 'text_value': '$YMQ_WRITER_KEY_ID'},\
    {'key': 'ymq_writer_key_secret', 'text_value': '$YMQ_WRITER_KEY_SECRET'},\
    {'key': 'ymq_capture_queue_url', 'text_value': '$YMQ_CAPTURE_QUEUE_URL'},\
    {'key': 'ymq_state_change_queue_url', 'text_value': 'YMQ_STATE_CHANGE_QUEUE_URL'},\
    {'key': 'yds_writer_key_id', 'text_value': '$YDS_WRITER_KEY_ID'},\
    {'key': 'yds_writer_key_secret', 'text_value': '$YDS_WRITER_KEY_SECRET'},\
    {'key': 'yds_state_change_stream', 'text_value': 'notify-state-change'},\
    {'key': 'yds_state_change_database', 'text_value': '$YDB_DATA_STREAMS_DATABASE'}]" 

# [cледующий этап >>>](../8-deploy-app/README.md)
