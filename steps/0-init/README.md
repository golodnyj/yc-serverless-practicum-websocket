# Настройка окружения для практикума

## Оглавление
1. [Предварительная инсталляция](##Предварительная-инсталляция)
2. [Получение логина и пароля](##Получение-логина-и-пароля)
3. [Подключение к чату практикума](##Подключение-к-чату-практикума)

## Предварительная инсталляция

Для работы вам потребуются:
- IntelliJ IDEA Community Edition (или можно использовать WebStorm, любую другую среду разработки с поддержкой typescript)
- curl
- git
- yc (Yandex Cloud CLI)
- aws (Amazon Web Services CLI)
- ydb (YDB CLI)
- Node.js == 16.16.0

Ниже описаны шаги для их установки на различных операционных системах.

### MacOS
#### Установите утилиту brew

Установите [brew](https://brew.sh):

```bash
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"
```

#### IntelliJ IDEA Community Edition

Скачайте и установите дистрибутив IntelliJ IDEA Community Edition, дистрибутив скачать можно [здесь](https://www.jetbrains.com/ru-ru/idea/download/#section=mac).
Или можно скачать и установить дистрибутив WebStorm, дистрибутив скачать можно [здесь](https://www.jetbrains.com/ru-ru/webstorm/download/#section=mac). 

#### Установите утилиты curl и git

```bash
brew install curl git
```

#### Установите утилиту yc CLI

Установите [yc CLI](https://cloud.yandex.ru/docs/cli/operations/install-cli#interactive):

```bash
curl -sSL https://storage.yandexcloud.net/yandexcloud-yc/install.sh | bash
exec -l $SHELL
yc version
```

Настройте профиль по [инструкции](https://cloud.yandex.ru/docs/cli/operations/profile/profile-create#interactive-create), логин и пароль вы получите в письме.

#### aws CLI

Установите [aws CLI](https://docs.aws.amazon.com/cli/latest/userguide/install-cliv2-mac.html):

```bash
curl "https://awscli.amazonaws.com/AWSCLIV2.pkg" -o "AWSCLIV2.pkg"
sudo installer -pkg AWSCLIV2.pkg -target /
```

Конфигурирование обычно делается по [инструкции](https://cloud.yandex.ru/docs/ydb/quickstart/document-api/aws-setup), 
но в этом практикуме, настройку мы будем делать на одном из следующих шагов.  

#### ydb CLI

Установите [ydb CLI](https://ydb.tech/ru/docs/reference/ydb-cli/install):

```bash
curl -sSL https://storage.yandexcloud.net/yandexcloud-ydb/install.sh | bash
exec -l $SHELL 
ydb version
```

## Получение логина и пароля


## Подключение к чату практикума

Вся совместная работа, будет проходить в чате практикума в телеграм, [подключитесь в чат](https://t.me/YandexCloudFunctions/21064).


