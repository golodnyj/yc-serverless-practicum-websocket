# Создание Yandex Message Queue
## Создание ключа доступа для сервисного аккаунта

Ранее были созданы сервисные аккаунты:
* `sls-deploy` с правами `editor`, мы используем, чтобы создать экземпляр Yandex Message Queue;
* `ymq-reader-sa` с правами `ymq.reader`, чтобы осуществлять чтение в Yandex Message Queue;
* `ymq-writer-sa` с правами `ymq.writer`, чтобы осуществлять запись из Yandex Message Queue.

Этот этап нужен для получения идентификатора ключа доступа и секретного ключа. 
Для создания ключа доступа необходимо вызвать следующую команду:

    yc iam access-key create --service-account-id $SERVICE_ACCOUNT_GAME_ID

В результате вы получите примерно следующее

    access_key:
        id: ajeibet32197n869t0lu
        service_account_id: ajehr6tv2eoo3isdbv0e
        created_at: "2023-03-13T10:20:58.471421425Z"
        key_id: YCASD3CT9mPCVFh3KRmB4JDx9
    secret: YCNhBcdvfDdssIuBa-FDl6zZz0MSky6ujSjACX

Сохраните идентификатор `key_id` в переменную `AWS_ACCESS_KEY_ID`
и секретный ключ `secret` в переменную `AWS_SECRET_ACCESS_KEY`.
Получить значение ключа снова будет невозможно. Подставьте свои значения и сохраните переменные:

    echo "export AWS_ACCESS_KEY_ID=<key_id>" >> ~/.bashrc && . ~/.bashrc
    echo $AWS_ACCESS_KEY_ID

    echo "export AWS_SECRET_ACCESS_KEY=<secret>" >> ~/.bashrc && . ~/.bashrc
    echo $AWS_SECRET_ACCESS_KEY


    yc iam access-key create --service-account-id $SERVICE_ACCOUNT_GAME_ID 

Аналогичную процедуру проведем для сервисного аккаунта `ymq-writer-sa`, 
итоговые переменные будут использованы для работы функций с очередями:

    yc iam access-key create --service-account-id $YMQ_WRITER_SA_ID

    echo "export YMQ_WRITER_KEY_ID=<key_id>" >> ~/.bashrc && . ~/.bashrc
    echo $YMQ_WRITER_KEY_ID

    echo "export YMQ_WRITER_KEY_SECRET=<secret>" >> ~/.bashrc && . ~/.bashrc
    echo $YMQ_WRITER_KEY_SECRET

## Создание очереди Yandex Message Queue
Для создания очереди Yandex Message Queue вы можете использовать три разных способа:
* UI;
* консольную утилиту aws;
* terraform.

## Создание очереди с помощью UI

1. Откройте раздел Message Queue.
2. Нажмите кнопку `Создать очередь`.
3. Укажите имя очереди `capturing-queue`.
4. Выберите тип очереди `Стандартная`.
5. Нажмите кнопку `Создать`.

## Создание очереди с помощью утилиты aws

Для альтернативного способа создания очереди можете воспользоваться  AWS CLI. 
Для начала, задайте конфигурацию с помощью команды `aws configure`. При этом от вас потребуется ввести:
* `AWS Access Key ID` — это идентификатор ключа доступа `key_id` сервисного аккаунта, полученный ранее.
* `AWS Secret Access Key` — это секретный ключ `secret` сервисного аккаунта, полученный ранее.
* `Default region name` — используйте значение `ru-central1`.
* `Default output format` — оставьте пустым.

По завершению конфигурации вы сможете создать очередь, нам таких очередей нужно две `capturing-queue` и `notify-state-change`, создадим первую:
    
    echo $AWS_ACCESS_KEY_ID
    echo $AWS_SECRET_ACCESS_KEY
    aws configure
    aws sqs create-queue --queue-name capturing-queue --endpoint https://message-queue.api.cloud.yandex.net/

В результате успешного выполнения предыдущей команды в ответ вы получите URL, который нужно сохранить.
Вывод будет примерно такой:

    {
        "QueueUrl": "https://message-queue.api.cloud.yandex.net/b1gvr958gmn7ibs9vfib/dj600000000bt70t02vv/capturing-queue"
    }

Аналогичную команду нужно повторить для очереди `notify-state-change`:

    aws sqs create-queue --queue-name notify-state-change --endpoint https://message-queue.api.cloud.yandex.net/

Полученные адреса необходимо сохранить в переменные: 

    echo "export YMQ_CAPTURE_QUEUE_URL=<URL>" >> ~/.bashrc && . ~/.bashrc
    echo $YMQ_CAPTURE_QUEUE_URL

    echo "export YMQ_STATE_CHANGE_QUEUE_URL=<URL>" >> ~/.bashrc && . ~/.bashrc
    echo $YMQ_STATE_CHANGE_QUEUE_URL

# [cледующий этап >>>](../5-create-yds/README.md)
