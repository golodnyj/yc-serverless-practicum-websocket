# Service Account
## Создание аккаунтов для разных ресурсов

Для работы нашего приложения нам потребуется создать специализированные сервисные аккаунты, 
чтобы только от их имени запускать и управлять разного рода ресурсами. 

Поэтому создадим следующие сервисные аккаунты:

* `functions-sa` с правами `editor`, чтобы оперировать всем приложением
* `triggers-sa` с правами `serverless.functions.invoker`, чтобы запускать триггеры и функции к ним привязанные 
* `yds-reader-sa` с правами `yds.admin`, чтобы оперировать Yandex Data Stream
* `yds-writer-sa` с правами `yds.writer`, чтобы осуществлять чтение из Yandex Data Stream
* `ymq-reader-sa` с правами `ymq.reader`, чтобы осуществлять запись в Yandex Message Queue
* `ymq-writer-sa` с правами `ymq.writer`, чтобы осуществлять чтение из Yandex Message Queue

Создаем все сервисные аккаунты:

    yc iam service-account create --name functions-sa
    yc iam service-account create --name triggers-sa
    yc iam service-account create --name yds-reader-sa 
    yc iam service-account create --name yds-writer-sa
    yc iam service-account create --name ymq-reader-sa
    yc iam service-account create --name ymq-writer-sa

    yc iam service-account list

Выдаем им права, согласно ранее представленного списка:

    echo "export FUNCTIONS_SA_ID=$(jq -r <<<  \
    "$(yc iam service-account list --format json | jq '.[]' -c | grep functions-sa)" .id)"  \
    >> ~/.bashrc && . ~/.bashrc

    yc resource-manager folder add-access-binding $YC_FOLDER_ID \
    --subject serviceAccount:$FUNCTIONS_SA_ID \
    --role editor

    echo "export TRIGGERS_SA_ID=$(jq -r <<<  \
    "$(yc iam service-account list --format json | jq '.[]' -c | grep triggers-sa)" .id)"  \
    >> ~/.bashrc && . ~/.bashrc

    yc resource-manager folder add-access-binding $YC_FOLDER_ID \
    --subject serviceAccount:$TRIGGERS_SA_ID \
    --role serverless.functions.invoker

    echo "export YDS_READER_SA_ID=$(jq -r <<<  \
    "$(yc iam service-account list --format json | jq '.[]' -c | grep yds-reader-sa)" .id)"  \
    >> ~/.bashrc && . ~/.bashrc

    yc resource-manager folder add-access-binding $YC_FOLDER_ID \
    --subject serviceAccount:$YDS_READER_SA_ID \
    --role yds.admin

    echo "export YDS_WRITER_SA_ID=$(jq -r <<<  \
    "$(yc iam service-account list --format json | jq '.[]' -c | grep yds-writer-sa)" .id)"  \
    >> ~/.bashrc && . ~/.bashrc

    yc resource-manager folder add-access-binding $YC_FOLDER_ID \
    --subject serviceAccount:$YDS_WRITER_SA_ID \
    --role yds.admin

    echo "export YMQ_READER_SA_ID=$(jq -r <<<  \
    "$(yc iam service-account list --format json | jq '.[]' -c | grep ymq-reader-sa)" .id)"  \
    >> ~/.bashrc && . ~/.bashrc

    yc resource-manager folder add-access-binding $YC_FOLDER_ID \
    --subject serviceAccount:$YMQ_READER_SA_ID \
    --role ymq.reader

    echo "export YMQ_WRITER_SA_ID=$(jq -r <<<  \
    "$(yc iam service-account list --format json | jq '.[]' -c | grep ymq-writer-sa)" .id)"  \
    >> ~/.bashrc && . ~/.bashrc

    yc resource-manager folder add-access-binding $YC_FOLDER_ID \
    --subject serviceAccount:$YMQ_WRITER_SA_ID \
    --role ymq.writer

# [cледующий этап >>>](../3-create-ydb/README.md)
