# Создание ключа доступа для сервисного аккаунта

Ранее были созданы сервисные аккаунты:
* `yds-writer-sa` с правами `yds.writer`, чтобы осуществлять запись в запись в Yandex Data Stream;
* `ymq-writer-sa` с правами `ymq.writer`, чтобы осуществлять запись из Yandex Message Queue.

Этот этап нужен для получения идентификатора ключа доступа и секретного ключа.
Для создания ключа доступа необходимо вызвать следующую команду:

    echo "export YDS_WRITER_SA_ID=$(jq -r <<<  \
    "$(yc iam service-account list --format json | jq '.[]' -c | grep yds-writer-sa)" .id)"  \
    >> ~/.bashrc && . ~/.bashrc

    yc iam access-key create --service-account-id $YDS_WRITER_SA_ID

В результате вы получите примерно следующее

    access_key:
        id: ajeibet32197n869t0lu
        service_account_id: ajehr6tv2eoo3isdbv0e
        created_at: "2023-03-13T10:20:58.471421425Z"
        key_id: YCASD3CT9mPCVFh3KRmB4JDx9
    secret: YCNhBcdvfDdssIuBa-FDl6zZz0MSky6ujSjACX

Подставьте свои значения и сохраните переменные:

    echo "export YDS_WRITER_KEY_ID=<key_id>" >> ~/.bashrc && . ~/.bashrc
    echo $YDS_WRITER_KEY_ID

    echo "export YDS_WRITER_KEY_SECRET=<secret>" >> ~/.bashrc && . ~/.bashrc
    echo $YDS_WRITER_KEY_SECRET

Аналогичную процедуру проведем для сервисного аккаунта `ymq-writer-sa`,
итоговые переменные будут использованы для работы функций с очередями:

    echo "export YMQ_WRITER_SA_ID=$(jq -r <<<  \
    "$(yc iam service-account list --format json | jq '.[]' -c | grep ymq-writer-sa)" .id)"  \
    >> ~/.bashrc && . ~/.bashrc

    yc iam access-key create --service-account-id $YMQ_WRITER_SA_ID

    echo "export YMQ_WRITER_KEY_ID=<key_id>" >> ~/.bashrc && . ~/.bashrc
    echo $YMQ_WRITER_KEY_ID

    echo "export YMQ_WRITER_KEY_SECRET=<secret>" >> ~/.bashrc && . ~/.bashrc
    echo $YMQ_WRITER_KEY_SECRET

# [cледующий этап >>>](../9-update-and-redeploy/README.md)
