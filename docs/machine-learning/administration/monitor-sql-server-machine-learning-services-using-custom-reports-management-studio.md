---
title: Мониторинг выполнения скриптов с помощью пользовательских отчетов
description: Пользовательские отчеты в SQL Server Management Studio (SSMS) можно использовать для отслеживания выполнения внешних скриптов (Python и R), мониторинга используемых ресурсов, диагностики проблем и настройки производительности в службах машинного обучения SQL Server.
ms.prod: sql
ms.technology: machine-learning-services
ms.date: 10/14/2020
ms.topic: how-to
author: dphansen
ms.author: davidph
ms.custom: seo-lt-2019
monikerRange: '>=sql-server-2016||>=sql-server-linux-ver15||=azuresqldb-mi-current'
ms.openlocfilehash: e0c3874177691832bd6fa66f81c3fe94c726ff76
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/10/2021
ms.locfileid: "100337079"
---
# <a name="monitor-python-and-r-script-execution-using-custom-reports-in-sql-server-management-studio"></a>Мониторинг выполнения скриптов Python и R с помощью пользовательских отчетов в SQL Server Management Studio
[!INCLUDE [SQL Server 2016 SQL MI](../../includes/applies-to-version/sqlserver2016-asdbmi.md)]

Пользовательские отчеты в [SQL Server Management Studio (SSMS)](../../ssms/download-sql-server-management-studio-ssms.md) можно использовать для отслеживания выполнения внешних скриптов (Python и R), мониторинга используемых ресурсов, диагностики проблем и настройки производительности в [службах машинного обучения SQL Server](../sql-server-machine-learning-services.md).

В этих отчетах можно просмотреть следующие сведения:

- активные сеансы Python или R;
- параметры конфигурации для экземпляра;
- статистика выполнения заданий машинного обучения;
- расширенные события для служб R Services;
- пакеты Python или R, установленные в текущем экземпляре.

В этой статье содержатся сведения об установке и использовании пользовательских отчетов для служб машинного обучения SQL Server в Windows.

Дополнительные сведения об отчетах в SQL Server Management Studio см. в статье [Пользовательские отчеты в среде Management Studio](../../ssms/object/custom-reports-in-management-studio.md).

## <a name="how-to-install-the-reports"></a>Установка отчетов

Отчеты разработаны с помощью служб SQL Server Reporting Services, однако их можно использовать непосредственно из SQL Server Management Studio. Устанавливать службы Reporting Services в экземпляре SQL Server необязательно.

Чтобы использовать эти отчеты, выполните следующие действия.

1. Скачайте [пользовательские отчеты SSMS](https://github.com/Microsoft/sql-server-samples/tree/master/samples/features/machine-learning-services/ssms-custom-reports) для служб машинного обучения SQL Server с сайта GitHub.

   ::: moniker range="=azuresqldb-mi-current"
   >[!NOTE]
   > Пользовательский отчет **ML Services — Configure Instance** не поддерживается в Управляемом экземпляре SQL Azure.
   ::: moniker-end

2. Копирование отчетов в Management Studio

    1. Найдите папку настраиваемых отчетов, используемую SQL Server Management Studio. По умолчанию пользовательские отчеты хранятся в этой папке (где **user_name** — это имя пользователя Windows):

        `C:\Users\user_name\Documents\SQL Server Management Studio\Custom Reports`

       Можно указать другую папку или создать вложенные папки.

    2. Скопируйте скачанные RDL-файлы в папку пользовательских отчетов.

3. Запуск пользовательских отчетов в среде Management Studio

    1. В Management Studio щелкните правой кнопкой мыши узел **Базы данных** для экземпляра, на котором вы хотите запустить отчеты.

    2. Щелкните **Отчеты**, а затем **Настраиваемые отчеты**.

    3. В диалоговом окне **Открыть файл** найдите папку настраиваемых отчетов.

    4. Выберите один из загруженных ранее RDL-файлов и нажмите кнопку **Открыть**.

## <a name="reports"></a>Отчеты

[В репозитории пользовательских отчетов SSMS на сайте GitHub](https://github.com/Microsoft/sql-server-samples/tree/master/samples/features/machine-learning-services/ssms-custom-reports) находятся перечисленные далее отчеты.

| Report | Описание |
|-|-|
| Активные сеансы | Текущие пользователи, которые подключены к экземпляру SQL Server и выполняют скрипт Python или R. |
| Конфигурация | Параметры установки служб машинного обучения и свойства среды выполнения Python или R. |
| Настройка экземпляра | Настройка служб машинного обучения. |
| Статистика выполнения | Статистика выполнения для служб машинного обучения. Например, можно получить общее число выполнений внешних скриптов и число параллельных выполнений. |
| Расширенные события | Доступные расширенные события для получения дополнительных сведений о выполнении внешних скриптов. |
| Пакеты | Список пакетов R или Python, установленных в экземпляре SQL Server, и их свойств, таких как версия и имя. |
| Использование ресурсов | Просмотр использования ресурсов ЦП, памяти, операций ввода-вывода в SQL Server и выполнения внешних скриптов. Кроме того, можно просмотреть настройки памяти для внешних пулов ресурсов. |

## <a name="next-steps"></a>Дальнейшие действия

- [Мониторинг служб машинного обучения SQL Server с помощью динамических административных представлений](monitor-sql-server-machine-learning-services-using-dynamic-management-views.md)
- [Мониторинг скриптов Python и R с использованием расширенных событий в Службах машинного обучения SQL Server](extended-events.md)
