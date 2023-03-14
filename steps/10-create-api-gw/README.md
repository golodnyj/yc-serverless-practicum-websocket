# Создадим экземпляр API GW
## Внесите изменения в конфигурацию API GW

Ранее фреймворком были созданы два сервисных аккаунта :
* `apigw-s3-viewer` с правами `storage.viewer`, чтобы доступаться до объектов в Object Storage
* `apigw-fn-caller` с правами `serverless.functions.invoker`, чтобы запускать триггеры и функции к ним привязанные

Сохраним себе переменные:

    echo "export APIGW_S3_VIEWER_ID=$(jq -r <<<  \
    "$(yc iam service-account list --format json | jq '.[]' -c | grep apigw-s3-viewer)" .id)"  \
    >> ~/.bashrc && . ~/.bashrc

    echo "export APIGW_FN_CALLER_ID=$(jq -r <<<  \
    "$(yc iam service-account list --format json | jq '.[]' -c | grep apigw-fn-caller)" .id)"  \
    >> ~/.bashrc && . ~/.bashrc

## Внесите изменения в конфигурацию API GW

Внесем изменения в конфигурацию для API GW, для этого воспользуемся подсказкой из файла `apigw-example.yml`, 
находясь в каталоге текущего шага выполните команду:

    cp apigw-example.yml apigw.yml

Нам потребуются id разных ресурсов созданных ранее: 

    echo $APIGW_S3_VIEWER_ID
    echo $APIGW_FN_CALLER_ID

    yc storage bucket list
    yc serverless function list

Теперь внесите в файл `apigw.yml` следующие изменения:

1. Во всех строках `bucket: serverless-game-files` замените название бакета на ваше, в моем случае `sls-game-files-golodnyj` — ваше оригинальное название бакета созданное ранее.
2. Во всех строках `service_account_id: <sa-id-for-object-storage>` замените `<sa-id-for-object-storage>` на значение переменной `$APIGW_S3_VIEWER_ID`
3. Во всех строках `service_account_id: <sa-id-for-functions>` замените `<sa-id-for-functions>` на значение переменной `$APIGW_FN_CALLER_ID`
4. В строке `function_id: <yandex-cloud-nodejs-dev-get-state-function-id>` замените `<yandex-cloud-nodejs-dev-get-state-function-id>` на значение id функции `yandex-cloud-nodejs-dev-get-state`
5. В строке `function_id: <yandex-cloud-nodejs-dev-get-config-function-id>` замените `<yandex-cloud-nodejs-dev-get-config-function-id>` на значение id функции `yandex-cloud-nodejs-dev-get-config`
6. В строке `function_id: <yandex-cloud-nodejs-dev-move-function-id>` замените `<yandex-cloud-nodejs-dev-move-function-id>` на значение id функции `yandex-cloud-nodejs-dev-move`
7. В строке `function_id: <yandex-cloud-nodejs-dev-login-function-id>` замените `<yandex-cloud-nodejs-dev-login-function-id>` на значение id функции `yandex-cloud-nodejs-dev-login`
8. В строке `function_id: <yandex-cloud-nodejs-dev-ws-message-function-id>` замените `<yandex-cloud-nodejs-dev-ws-message-function-id>` на значение id функции `yandex-cloud-nodejs-dev-ws-message`
9. В строке `function_id: <yandex-cloud-nodejs-dev-ws-connect-function-id>` замените `<yandex-cloud-nodejs-dev-ws-connect-function-id>` на значение id функции `yandex-cloud-nodejs-dev-ws-connect`
10. В строке `function_id: <yandex-cloud-nodejs-dev-ws-disconnect-function-id>` замените `<yandex-cloud-nodejs-dev-ws-disconnect-function-id>` на значение id функции `yandex-cloud-nodejs-dev-ws-disconnect`
11. В строке `function_id: <yandex-cloud-nodejs-dev-auth-function-id>` замените `<yandex-cloud-nodejs-dev-auth-function-id>` на значение id функции `yandex-cloud-nodejs-dev-auth`

## Разверните экземпляр API GW

    yc serverless api-gateway create \
    --name serverless-game-api \
    --spec=apigw.yml \
    --description "for serverless-game-api"

Если вы что-то в спецификации неправильно заполнили и захотите ее прегрузить воспользуйтесь следующей командой:

    yc serverless api-gateway update \
    --name serverless-game-api \
    --spec=apigw.yml 

