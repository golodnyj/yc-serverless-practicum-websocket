# Service Account
## Настройка yc CLI, создание профайла

Для работы с облаком необходимо настроить утилиту `yc`, 
рекомендуется создать профиль, в моем случае профиль называется `golodnyj`.
В случае подготовки к практикуму, вы уже выполнили это ранее и он называется, допустим `sls350`. Давайте проверим:

    yc config profile list
    yc config profile get golodnyj

В выводе последней команды если все хорошо вы увидите значение `cloud-id` и `folder-id` они нам понадобятся далее.

## Создание токена

У обычного облачного пользователя вы бы использовали `OAUTH_TOKEN`:

    echo "export OAUTH_TOKEN=$(yc config get token)" >> ~/.bashrc && . ~/.bashrc
    echo $OAUTH_TOKEN

Для федерального пользователя можно получить IAM-токена, укажите свой профиль и выполните:

    echo "export YC_IAM_TOKEN=$(yc iam create-token --profile golodnyj)" >> ~/.bashrc && . ~/.bashrc
    echo $YC_IAM_TOKEN

## Создание аккаунта

Создайте сервисный аккаунт с именем `sls-deploy` для организации деплоя нашего приложения:

    export SERVICE_ACCOUNT_GAME=$(yc iam service-account create --name sls-deploy \
    --description "service account for serverless game" \
    --format json | jq -r .)

    echo $SERVICE_ACCOUNT_GAME

Проверьте текущий список сервисных аккаунтов:

    yc iam service-account list

После проверки запишите ID, созданного сервисного аккаунта, в переменную `SERVICE_ACCOUNT_GAME_ID`:

    echo "export SERVICE_ACCOUNT_GAME_ID=<ID>" >> ~/.bashrc && . ~/.bashrc  
    echo $SERVICE_ACCOUNT_GAME_ID

## Назначение роли сервисному аккаунту

Добавим вновь созданному сервисному аккаунту роль `editor`:

    echo "export YC_FOLDER_ID=$(yc config get folder-id)" >> ~/.bashrc && . ~/.bashrc
    echo $YC_FOLDER_ID

    echo "export YC_CLOUD_ID=$(yc config get cloud-id)" >> ~/.bashrc && . ~/.bashrc
    echo $YC_CLOUD_ID

    yc resource-manager folder add-access-binding $YC_FOLDER_ID \
    --subject serviceAccount:$SERVICE_ACCOUNT_GAME_ID \
    --role editor

## Переменная для выкатки кода

    echo "export APP_ENV=production" >> ~/.bashrc && . ~/.bashrc
    echo $APP_ENV

## Видео

https://youtu.be/gtRmEaJvHEA

# [cледующий этап >>>](../2-create-key/README.md)
