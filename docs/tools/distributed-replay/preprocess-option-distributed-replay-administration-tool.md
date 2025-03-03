---
title: Параметр предварительной обработки
titleSuffix: SQL Server Distributed Replay
description: Средство распределенного воспроизведения Microsoft SQL Server (DReplay.exe) — это программа командной строки для взаимодействия с контроллером распределенного воспроизведения.
ms.prod: sql
ms.prod_service: sql-tools
ms.technology: tools-other
ms.topic: conceptual
author: markingmyname
ms.author: maghan
ms.reviewer: ''
ms.custom: seo-lt-2019
ms.date: 03/14/2017
ms.openlocfilehash: 34c4b1432fa9373e4952bb6b11264f5cec6e452a
ms.sourcegitcommit: 9413ddd8071da8861715c721b923e52669a921d8
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/04/2021
ms.locfileid: "101836867"
---
# <a name="preprocess-option-distributed-replay-administration-tool"></a>Параметр предварительной обработки (средство администрирования распределенного воспроизведения)
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  Средство администрирования распределенного воспроизведения [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (**DReplay.exe**) представляет собой программу командной строки, которая используется для взаимодействия с контроллером распределенного воспроизведения. В этом разделе описан параметр командной строки **preprocess** и соответствующий синтаксис.  
  
 Параметр **preprocess** запускает предварительную обработку. На этом этапе контроллер подготавливает для воспроизведения на целевом сервере входные данные трассировки.  
  
 ![Значок ссылки на раздел](../../database-engine/configure-windows/media/topic-link.gif "Значок ссылки на раздел") Дополнительные сведения о синтаксических обозначениях, используемых в синтаксисе средства администрирования, см. в статье [Синтаксические обозначения в Transact-SQL (Transact-SQL)](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md).  
  
## <a name="syntax"></a>Синтаксис  
  
```  
  
dreplay preprocess [-m controller] -i input_trace_file  
    -d controller_working_dir [-c config_file] [-f status_interval]  
```  
  
#### <a name="parameters"></a>Параметры  
 **-m** _контроллер_  
 Задает имя компьютера для контроллера. Локальный компьютер можно указать как «`localhost`» или «`.`».  
  
 Если параметр **-m** не задан, то используется локальный компьютер.  
  
 **-i** _input_trace_file_  
 Задает полный путь к входному файлу трассировки на контроллере, например `D:\Mytrace.trc`. Параметр **-i** является обязательным.  
  
 При наличии в том же каталоге файлов продолжения они загружаются и используются автоматически. Файлы должны соответствовать соглашению об именовании переключения на файл продолжения, например: `Mytrace.trc`, `Mytrace_1.trc`, `Mytrace_2.trc`, `Mytrace_3.trc`, … `Mytrace_n.trc`.  
  
> [!NOTE]  
>  При использовании средства администрирования на компьютере, отличном от контроллера, необходимо скопировать файлы входных данных трассировки на контроллер, чтобы в этом параметре можно было использовать локальный путь.  
  
 **-d** _controller_working_dir_  
 Указывает каталог на контроллере, где будет сохранен промежуточный файл. Параметр **-d** является обязательным.  
  
 К нему предъявляются следующие требования.  
  
-   Каталог должен находиться на контроллере.  
  
-   Необходимо указать полный путь, начиная с буквы диска (например, `c:\WorkingDir`).  
  
-   Путь не должен завершаться обратной косой чертой «`\`».  
  
-   Пути в формате UNC не поддерживаются.  
  
 **-c** _config_file_  
 Полный путь к файлу конфигурации предварительной обработки. Используется для указания расположения конфигурации предварительной обработки, сохраненной в другом месте. Этот параметр может быть путем в формате UNC или задавать локальный путь на компьютере, на котором выполняется средство администрирования.  
  
 Если фильтрация не требуется или не нужно изменять максимальное время простоя, то указывать параметр **-c** не обязательно.  
  
 Без параметра **-c** используется файл конфигурации предварительной обработки по умолчанию — `DReplay.exe.preprocess.config`.  
  
 **-f** _status_interval_  
 Указывает частоту (в секундах) отображения сообщений о состоянии.  
  
 Если параметр **-f** не задан, интервал по умолчанию составляет 30 секунд.  
  
## <a name="examples"></a>Примеры  
 В этом примере предварительная подготовка запускается со всеми параметрами по умолчанию. Значение `localhost` указывает, что служба контроллера запущена на том же компьютере, что и средство администрирования. Параметр *input_trace_file* задает расположение входных данных трассировки — `c:\mytrace.trc`. Так как фильтрация файлов трассировки не используется, указывать параметр **-c** не обязательно.  
  
```  
dreplay preprocess -m localhost -i c:\mytrace.trc -d c:\WorkingDir  
```  
  
 В этом примере запускается этап предварительной подготовки и указывается измененный файл конфигурации предварительной обработки. В отличие от предыдущего примера параметр **-c** используется для указания измененного файла конфигурации, сохраненного в другом расположении. Пример:  
  
```  
dreplay preprocess -m localhost -i c:\mytrace.trc -d c:\WorkingDir -c c:\DReplay.exe.preprocess.config  
```  
  
 В измененном файле конфигурации предварительной обработки добавлено условие фильтра, которое позволяет отфильтровать системные сеансы во время распределенного воспроизведения. Фильтр добавляется путем изменения элемента `<PreprocessModifiers>` в файле конфигурации предварительной обработки `DReplay.exe.preprocess.config`.  
  
 В следующем примере показан измененный файл конфигурации.  
  
```  
<?xml version='1.0'?>  
<Options>  
    <PreprocessModifiers>  
        <IncSystemSession>No</IncSystemSession>  
        <MaxIdleTime>-1</MaxIdleTime>  
    </PreprocessModifiers>  
</Options>  
```  
  
## <a name="permissions"></a>Разрешения  
 Средство администрирования должно запускаться как интерактивный пользователь, с учетной записью локального пользователя или пользователя домена. Для использования учетной записи локального пользователя средство администрирования и контроллер должны быть запущены на одном компьютере.  
  
 Дополнительные сведения см. в статье [Distributed Replay Security](../../tools/distributed-replay/distributed-replay-security.md).  
  
## <a name="see-also"></a>См. также:  
 [Подготовка входных данных трассировки](../../tools/distributed-replay/prepare-the-input-trace-data.md)   
 [Распределенное воспроизведение SQL Server](../../tools/distributed-replay/sql-server-distributed-replay.md)   
 [Настройка распределенного воспроизведения](../../tools/distributed-replay/configure-distributed-replay.md)  
  
  
