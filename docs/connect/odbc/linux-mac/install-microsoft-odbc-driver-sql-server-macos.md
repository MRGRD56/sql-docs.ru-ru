---
title: Установка Microsoft ODBC Driver for SQL Server (macOS)
description: Узнайте, как установить Microsoft ODBC Driver for SQL Server на клиентах macOS, чтобы обеспечить подключение к базе данных.
ms.date: 02/12/2021
ms.prod: sql
ms.prod_service: connectivity
ms.technology: connectivity
ms.topic: conceptual
helpviewer_keywords:
- driver, installing
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 2f07e613a6bd5ce8416c4b2c015749a1e631ea3f
ms.sourcegitcommit: c83c17e44b5e1e3e2a3b5933c2a1c4afb98eb772
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/15/2021
ms.locfileid: "100525148"
---
# <a name="install-the-microsoft-odbc-driver-for-sql-server-macos"></a>Установка Microsoft ODBC Driver for SQL Server (macOS)

В этой статье объясняется, как установить Microsoft ODBC Driver for SQL Server в macOS. В ней также содержатся инструкции для необязательных средств командной строки для SQL Server (`bcp` и `sqlcmd`) и заголовков разработки unixODBC.

В этой статье приведены команды для установки драйвера ODBC из оболочки bash. Сведения о том, как загрузить пакеты напрямую, см. в разделе [Скачивание драйвера ODBC Driver for SQL Server](../download-odbc-driver-for-sql-server.md).

> [!Note]
> Драйвер Microsoft ODBC для SQL Server в macOS поддерживается только в 64-разрядной архитектуре. Apple M1 не поддерживается.

## <a name="microsoft-odbc-17"></a>Microsoft ODBC 17

Чтобы установить Microsoft ODBC Driver 17 для SQL Server в macOS, выполните следующие команды:

```bash
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install.sh)"
brew tap microsoft/mssql-release https://github.com/Microsoft/homebrew-mssql-release
brew update
HOMEBREW_NO_ENV_FILTERING=1 ACCEPT_EULA=Y brew install msodbcsql17 mssql-tools
```

> [!IMPORTANT]
> Если вы установили пакет `msodbcsql` версии 17, который был доступен непродолжительное время, его следует удалить перед установкой пакета `msodbcsql17`. Это позволит избежать конфликтов. Пакет `msodbcsql17` можно установить параллельно с пакетом `msodbcsql` версии 13.

## <a name="previous-versions"></a>Предыдущие версии

В следующих разделах приведены инструкции по установке предыдущих версий драйвера Microsoft ODBC в macOS.

## <a name="odbc-131"></a><a id="13.1"></a> ODBC 13.1

Используйте следующие команды для установки драйвера Microsoft ODBC Driver 13.1 for SQL Server в OS X 10.11 (El Capitan) и macOS 10.12 (Sierra):

```bash
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install.sh)"
brew tap microsoft/mssql-release https://github.com/Microsoft/homebrew-mssql-release
brew update
brew install msodbcsql@13.1.9.2 mssql-tools@14.0.6.0
```

## <a name="driver-files"></a>Файлы драйвера

Драйвер ODBC в macOS состоит из следующих компонентов.

|Компонент|Описание|  
|---------------|-----------------|  
|libmsodbcsql.17.dylib или libmsodbcsql.13.dylib|Файл динамической библиотеки (`dylib`), содержащий все функциональные возможности драйвера. Этот файл устанавливается в папке `/usr/local/lib/`.|  
|`msodbcsqlr17.rll` либо `msodbcsqlr13.rll`|Сопутствующий файл ресурса для библиотеки драйвера. Этот файл устанавливается в папке `[driver .dylib directory]../share/msodbcsql17/resources/en_US/` для версии 17 драйвера и в папке `[driver .dylib directory]../share/msodbcsql/resources/en_US/` для версии 13. | 
|msodbcsql.h|Файл заголовка, содержащий все новые определения, необходимые для использования драйвера.<br /><br /> **Примечание.**  Нельзя сочетать в одной программе ссылки на msodbcsql.h и odbcss.h.<br /><br /> Файл msodbcsql.h устанавливается в папке `/usr/local/include/msodbcsql17/` для версии 17 драйвера и в папке `/usr/local/include/msodbcsql/` для версии 13. |
|LICENSE.txt|Текстовый файл с условиями лицензионного соглашения. Этот файл помещается в папку `/usr/local/share/doc/msodbcsql17/` для версии 17 драйвера и в папку `/usr/local/share/doc/msodbcsql/` для версии 13. |
|RELEASE_NOTES|Текстовый файл с заметками о выпуске. Этот файл помещается в папку `/usr/local/share/doc/msodbcsql17/` для версии 17 драйвера и в папку `/usr/local/share/doc/msodbcsql/` для версии 13. |

## <a name="resource-file-loading"></a>Загрузка файла ресурсов

Чтобы драйвер работал, он должен загрузить файл ресурсов. Этот файл имеет имя `msodbcsqlr17.rll` или `msodbcsqlr13.rll` в зависимости от версии драйвера. Файл `.rll` располагается по пути относительно расположения самого драйвера (`so` или `dylib`), указанного в таблице выше. Кроме того, начиная с версии 17.1 драйвер пытается загрузить файл `.rll` из каталога по умолчанию, если его не удалось загрузить по относительному пути. Путь к файлу ресурсов по умолчанию в macOS: `/usr/local/share/msodbcsql17/resources/en_US/`

## <a name="troubleshooting"></a>Устранение неполадок

Некоторые пользователи столкнулись с проблемой при попытке подключения после установки драйвера ODBC и получили сообщение об ошибке следующего вида: `"[01000] [unixODBC][Driver Manager]Can't open lib 'ODBC Driver 17 for SQL Server' : file not found (0) (SQLDriverConnect)"`. Возможно, это произошло потому, что не удалось найти зарегистрированные драйверы из-за неправильной настройки unixODBC. В таких случаях проблему можно устранить, создав пару символических ссылок.

```bash
sudo ln -s /usr/local/etc/odbcinst.ini /etc/odbcinst.ini
sudo ln -s /usr/local/etc/odbc.ini /etc/odbc.ini
```

Сведения о других ситуациях, в которых не удается установить подключение к SQL Server с помощью драйвера ODBC, см. в статье, посвященной [устранению известных неполадок подключения](known-issues-in-this-version-of-the-driver.md#connectivity).

## <a name="next-steps"></a>Дальнейшие действия

После установки драйвера можно попробовать [пример приложения C++ ODBC](../../odbc/cpp-code-example-app-connect-access-sql-db.md). Подробнее о разработке приложений ODBC см. в разделе [Разработка приложений](../../../odbc/reference/develop-app/developing-applications.md).

Дополнительные сведения см. в статьях с [заметками о выпуске](release-notes-odbc-sql-server-linux-mac.md) и [требованиями к системе](system-requirements.md) для драйвера ODBC.
