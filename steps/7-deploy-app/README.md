# Деплой сервиса

Прежде чем выполнить деплой нашего сервиса, выполним несколько действий. 

## Чекаут проекта

Специально для этого практикума был подготовлен бранч проекта `yc-serverless-serverless-game` на который можно опираться. 
Перейдите в корень проекта и сделайте себе локальный экземпляр этого бранча:

    git clone --branch sls-demo-0323 https://github.com/yandex-cloud-examples/yc-serverless-serverless-game.git

В результате у вас будет новый каталог `yc-serverless-serverless-game` 
со всеми необходимыми файлами для запуска приложения.

## Изменения конфигурации для  Object Storage

Так как бакет должен быть уникальным правим его значение в нескольких местах нашего проекта. 
Чтобы получить уникальное значение `sls-game-files` заменили на `sls-game-files-golodnyj`. 
В вашем случае в конце можно использовать свой идентификатор профайла.

### Изменения конфигурации в файле serverless.yaml 

В директории клонированного проекта есть файл `serverless.yaml`. 
Найдите следующую строку:

    sls-game-files:
      type: yc::ObjectStorageBucket

Внесите изменение:

    sls-game-files-golodnyj:
      type: yc::ObjectStorageBucket

### Изменения конфигурации в файле загрузки 

В файл `upload-to-s3.ts` необходимо также внести правку, 
он расположен в поддиректории `/scripts` клонированного проекта.
Найдите следующую строку:

    Bucket: 'sls-game-files',

Внесите изменение:

    Bucket: 'sls-game-files-golodnyj',

## Создание токена 
Внимание! Если вы ранее делали этот пункт, то можете смело его пропустить.

У обычного облачного пользователя вы бы использовали `OAUTH_TOKEN`:

    echo "export OAUTH_TOKEN=$(yc config get token)" >> ~/.bashrc && . ~/.bashrc
    echo $OAUTH_TOKEN

Для федерального пользователя можно получить IAM-токена так:

    yc config profile list
    echo "export YC_IAM_TOKEN=$(yc iam create-token --profile golodnyj)" >> ~/.bashrc && . ~/.bashrc
    echo $YC_IAM_TOKEN

## Сборка и деплой проекта (дополнить)

Находясь в корне клонированного проекта выполните последовательно следующие команды:

    nvm use
    nvm install
    npm ci

    npm run build
    npm run deploy

## Видео

https://youtu.be/ZeUtu0nJKls

# [cледующий этап >>>](../8-create-keys/README.md)
