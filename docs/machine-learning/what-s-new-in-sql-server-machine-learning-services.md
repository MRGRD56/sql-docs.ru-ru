---
title: Новые возможности служб машинного обучения SQL Server
titleSuffix: ''
description: Объявления о новых возможностях для каждого выпуска служб машинного обучения SQL Server и SQL Server 2016 R.
ms.date: 11/17/2020
ms.topic: overview
author: dphansen
ms.author: davidph
ms.custom: sqlseattle
ms.prod: sql
ms.technology: machine-learning-services
monikerRange: '>=sql-server-2016||>=sql-server-linux-ver15'
ms.openlocfilehash: 31ecd76efc4796aebe2a3ee40a8bf40891d38a83
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/10/2021
ms.locfileid: "100272935"
---
# <a name="whats-new-in-sql-server-machine-learning-services"></a>Новые возможности служб машинного обучения SQL Server
[!INCLUDE [SQL Server 2016 and later](../includes/applies-to-version/sqlserver2016.md)]

В этих статьях описываются новые возможности и функции, включенные в различные версии [служб машинного обучения SQL Server](sql-server-machine-learning-services.md). Возможности машинного обучения добавляются в SQL Server в каждом выпуске, так как мы продолжаем расширять и углублять интеграцию между платформой данных, расширенной аналитикой и обработкой и анализом данных. 

::: moniker range=">=sql-server-ver15||>=sql-server-linux-ver15"
## <a name="new-in-sql-server-2019"></a>Новые возможности в SQL Server 2019

В этом выпуске добавляются наиболее востребованные возможности для операций машинного обучения Python и R в SQL Server. Дополнительные сведения обо всех возможностях этого выпуска см. в статьях [Новые возможности SQL Server 2019](../sql-server/what-s-new-in-sql-server-ver15.md) и [Заметки о выпуске SQL Server 2019](../sql-server/sql-server-version-15-release-notes.md).

> [!NOTE]
> Документацию с описанием новых возможностей Java в SQL Server 2019 см. в статье [Новые возможности расширений языка SQL Server](../language-extensions/language-extensions-whats-new.md).

Ниже описаны новые возможности Служб машинного обучения SQL Server, доступные в **Windows** и **Linux**.

- В Службы машинного обучения для Python и R была добавлена поддержка платформы Linux. Начните работу с [установки Служб машинного обучения SQL Server в Linux](../linux/sql-server-linux-setup-machine-learning.md).
- [Подключение к SQL Server из скрипта Python или R с замыканием на себя](connect/loopback-connection.md). 
- [CREATE EXTERNAL LIBRARY (Transact-SQL)](../t-sql/statements/create-external-library-transact-sql.md) для Python и R.
- [sp_execute_external_script](../relational-databases/system-stored-procedures/sp-execute-external-script-transact-sql.md) вводит два новых параметра, которые позволяют легко создавать несколько моделей на основе секционированных данных. Дополнительные сведения см. в учебнике [Создание моделей на основе секций в R](tutorials/r-tutorial-create-models-per-partition.md).
- Поддержка отказоустойчивого кластера доступна для службы панели запуска, если служба панели запуска SQL Server запущена на всех узлах. Дополнительные сведения см. в статье [Установка отказоустойчивого кластера SQL Server](../sql-server/failover-clusters/install/sql-server-failover-cluster-installation.md).
- Изменения в механизме изоляции для Служб машинного обучения. Дополнительные сведения см. в статье [SQL Server 2019 в Windows: изменения в изоляции в Службах машинного обучения](install/sql-server-machine-learning-services-2019.md).

::: moniker-end

::: moniker range=">=sql-server-2017"
## <a name="new-in-sql-server-2017"></a>Новые возможности в SQL Server 2017

В этом выпуске добавляется [поддержка Python и ведущие в отрасли алгоритмы машинного обучения](https://cloudblogs.microsoft.com/sqlserver/2017/04/19/python-in-sql-server-2017-enhanced-in-database-machine-learning/). Переименованный в соответствии с новой областью, SQL Server 2017 вводит [Службы машинного обучения SQL Server (в базе данных)](sql-server-machine-learning-services.md) с языковой поддержкой Python и R. 

Объявления о возможностях см. в статье [Новые возможности SQL Server 2017](../sql-server/what-s-new-in-sql-server-2017.md).

### <a name="r-enhancements"></a>Усовершенствования R

Компонент R Служб машинного обучения SQL Server является следующим поколением служб SQL Server 2016 R Services с обновленными версиями базовых пакетов R, RevoScaler и других.

Новые возможности для R включают [**управление пакетами**](package-management/install-r-packages-with-tsql.md) со следующими особенностями: 

+ Роли базы данных помогают администраторам баз данных управлять пакетами и назначать разрешения для установки пакетов.
+ [CREATE EXTERNAL LIBRARY](../t-sql/statements/create-external-library-transact-sql.md) помогает администраторам баз данных управлять пакетами на знакомом языке T-SQL.
+ Функции [RevoScaleR](package-management/install-r-packages-with-revoscaler.md) позволяют устанавливать, удалять и просматривать пакеты, принадлежащие пользователям. Дополнительные сведения см. в статье [Использование функций RevoScaleR для поиска или установки пакетов R в SQL Server](package-management/install-r-packages-with-revoscaler.md).

### <a name="r-libraries"></a>Библиотеки R

| Пакет | Описание |
|---------|-------------|
| [**MicrosoftML**](r/ref-r-microsoftml.md) | В этом выпуске MicrosoftML входит в установку R по умолчанию, поэтому не придется выполнять обновление, как в предыдущей версии SQL Server 2016 R Services. MicrosoftML предоставляет современные алгоритмы машинного обучения и преобразования данных, которые можно масштабировать или запускать в удаленных контекстах вычислений. Алгоритмы включают настраиваемые глубокие нейронные сети, быстрые деревья и леса принятия решений, линейную регрессию и логистическую регрессию.  |

### <a name="python-integration-for-in-database-analytics"></a>Интеграция Python для аналитики в базе данных

Python — это язык, обеспечивающий превосходную гибкость и эффективность для выполнения различных задач машинного обучения. Библиотеки с открытым исходным кодом для Python включают несколько платформ для настраиваемых нейронных сетей, а также популярные библиотеки для обработки естественного языка. 

Поскольку Python интегрирован с ядром СУБД, аналитику можно приблизить к самим данным, избежав затрат и рисков, связанных с их перемещением. Вы можете развертывать решения для машинного обучения на основе Python с помощью таких средств, как Visual Studio. Рабочие приложения могут получать прогнозы, модели или визуальные элементы из среды выполнения Python 3.5 с помощью методов доступа к данным SQL Server.

Интеграция T-SQL и Python поддерживается с помощью системной хранимой процедуры [sp_execute_external_script](../relational-databases/system-stored-procedures/sp-execute-external-script-transact-sql.md). С помощью этой хранимой процедуры можно вызвать любой код Python. Код выполняется в безопасной двойной архитектуре, которая позволяет развертывать модели и сценарии Python корпоративного класса, вызываемые из приложения с помощью простой хранимой процедуры. Повышение производительности осуществляется с помощью потоковой передачи данных из SQL в процессы Python и параллелизации кольца MPI.

Вы можете использовать функцию T-SQL [PREDICT](../t-sql/queries/predict-transact-sql.md), чтобы выполнить [собственную оценку](predictions/native-scoring-predict-transact-sql.md) в предварительно обученной модели, которая ранее сохранялась в требуемом двоичном формате.

### <a name="python-libraries"></a>Библиотеки Python

| Пакет | Описание |
|---------|-------------|
|[**revoscalepy**](python/ref-py-revoscalepy.md)| Эквивалент RevoScaleR в Python. Вы можете создавать модели Python для линейных и логистических регрессий, деревьев принятия решений, улучшенных деревьев и случайных лесов, которые подлежат параллелизации и могут выполняться в удаленных контекстах вычислений. Этот пакет поддерживает использование нескольких источников данных и удаленных контекстов вычислений. Специалист по анализу и обработке данных или разработчик может выполнять код Python на удаленном SQL Server, чтобы исследовать данные или создавать модели без перемещения данных. |
|[**microsoftml**](python/ref-py-microsoftml.md) |Эквивалент пакета MicrosoftML R в Python. |

### <a name="pre-trained-models"></a>Предварительно обученная модель

[**Предварительно обученные модели**](install/sql-pretrained-models-install.md) доступны как для Python, так и для R. Используйте эти модели для распознавания изображений и анализа положительной и отрицательной тональности, чтобы создавать прогнозы на основе собственных данных. 

### <a name="standalone-server-as-a-shared-feature-in-sql-server-setup"></a>Изолированный сервер как общий компонент в установке SQL Server

В этом выпуске также добавлен [Сервер машинного обучения SQL Server (изолированный)](r/r-server-standalone.md) — полностью независимый сервер обработки и анализа данных, поддерживающий статистический и прогнозирующий анализ в R и Python. Как и в случае со службами R, этот сервер является следующей версией SQL Server 2016 R Server (изолированный). С помощью изолированного сервера можно распространять и масштабировать решения R или Python без зависимостей от SQL Server.
::: moniker-end

::: moniker range=">=sql-server-2016"
## <a name="new-in-sql-server-2016"></a>Новые возможности SQL Server 2016

В этом выпуске появились возможности машинного обучения в SQL Server с помощью **SQL Server 2016 R Services** — модуля аналитики в базе данных для обработки скрипта R в данных, хранящихся в экземпляре ядра СУБД.

Кроме того, был выпущен **SQL Server 2016 R Server (изолированный)** как способ установки R Server на Windows Server. Изначально установка SQL Server предоставляла только один способ установки R Server для Windows. В более поздних выпусках разработчики и специалисты по обработке и анализу данных, которым требовался R Server в Windows, могли использовать другой изолированный установщик для достижения той же цели. Изолированный сервер в SQL Server функционально эквивалентен изолированному серверному продукту, [Microsoft R Server для Windows](/machine-learning-server/install/r-server-install-windows).

Объявления о возможностях см. в статье [Новые возможности SQL Server 2016](../sql-server/what-s-new-in-sql-server-2016.md).

| Release |Обновление компонента |
|---------|----------------|
| Добавление единиц емкости | [**Оценка в реальном времени**](predictions/real-time-scoring.md) использует собственные библиотеки C++ для чтения модели, хранящейся в оптимизированном двоичном формате, а затем создает прогнозы без вызова среды выполнения R. Это значительно ускоряет операции оценки. С помощью оценки в реальном времени можно запустить хранимую процедуру или выполнить оценку в реальном времени в коде R. Оценка в реальном времени также доступна для SQL Server 2016, если экземпляр обновлен до последней версии [!INCLUDE[rsql-platform-md](../includes/rsql-platform-md.md)]. |
| Начальный выпуск | [**Интеграция R для аналитики в базе данных**](r/sql-server-r-services.md). <br/><br/> Пакеты R для вызова функций R в T-SQL и наоборот. Функции RevoScaleR предоставляют аналитику R в масштабе путем разделения данных на части компонентов, координации и управления распределенной обработкой, а затем агрегирования результатов. В SQL Server 2016 R Services (в базе данных) подсистема RevoScaleR интегрирована с экземпляром ядра СУБД, объединяя данные и аналитику в одном контексте обработки. <br/><br/>Интеграция T-SQL и R с помощью [sp_execute_external_script](../relational-databases/system-stored-procedures/sp-execute-external-script-transact-sql.md). С помощью этой хранимой процедуры можно вызвать любой код R. Эта безопасная инфраструктура позволяет развертывать модели и сценарии R корпоративного класса, вызываемые из приложения с помощью простой хранимой процедуры. Повышение производительности осуществляется с помощью потоковой передачи данных из SQL в процессы R и параллелизации кольца MPI. <br/><br/>Вы можете использовать функцию T-SQL [PREDICT](../t-sql/queries/predict-transact-sql.md), чтобы выполнить [собственную оценку](predictions/native-scoring-predict-transact-sql.md) в предварительно обученной модели, которая ранее сохранялась в требуемом двоичном формате.|

::: moniker-end

::: moniker range=">=sql-server-2017"
## <a name="linux-support"></a>Поддержка Linux

SQL Server 2019 добавляет поддержку Linux для R и Python при установке пакетов машинного обучения с помощью экземпляра ядра СУБД. Дополнительные сведения см. в статье [Установка служб машинного обучения SQL Server на Linux](../linux/sql-server-linux-setup-machine-learning.md).

В Linux SQL Server 2017 не имеет интеграции с R или Python, но вы можете использовать [собственную оценку](predictions/native-scoring-predict-transact-sql.md) в Linux, так как эта функция доступна через T-SQL [PREDICT](../t-sql/queries/predict-transact-sql.md) в Linux. Собственная оценка обеспечивает высокий уровень производительности для предварительно обученной модели без вызова или даже необходимости среды выполнения R.
::: moniker-end

## <a name="next-steps"></a>Дальнейшие действия

+ [Установка служб машинного обучения SQL Server (в базе данных)](install/sql-machine-learning-services-windows-install.md)
