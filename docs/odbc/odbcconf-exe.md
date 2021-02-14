---
description: ODBCCONF.EXE
title: ODBCCONF.EXE | Документация Майкрософт
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: conceptual
helpviewer_keywords:
- odbcconf.exe
ms.assetid: 3bf2be83-61f9-4183-836b-85204ac7116a
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: e62fec06a10316bb3b773cf599f11c23ba376cfd
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/10/2021
ms.locfileid: "100075050"
---
# <a name="odbcconfexe"></a>ODBCCONF.EXE
ODBCCONF.exe — это программа командной строки, которая позволяет настраивать драйверы ODBC и имена источников данных.  
  
> [!NOTE]  
>  ODBCCONF.exe будут удалены в будущих версиях компонентов доступа к данным Windows. Избегайте использования этой функции и запланируйте изменение приложений, которые в настоящее время используют эту функцию. Для управления драйверами и источниками данных можно использовать команды PowerShell. Дополнительные сведения об этих командах PowerShell см. в разделе [командлеты Windows Data Access Components](/powershell/module/wdac).  
  
## <a name="syntax"></a>Синтаксис  
  
```console  
ODBCCONF [switches] action  
```  
  
## <a name="arguments"></a>Аргументы  
 *аргументы*  
 Ноль или более параметров переключения. Список доступных параметров см. в подразделе "Примечания" Далее в этом разделе.  
  
 *action*  
 Одно действие для выполнения. Список доступных параметров см. в разделе "Примечания".  
  
## <a name="remarks"></a>Remarks  
 Доступны следующие параметры:  
  
|Параметр|Описание|  
|------------|-----------------|  
|/A {*Action*}|Укажите действие.<br /><br /> Параметр/a является необязательным, если указано только одно действие.|  
|/?|Отображение сведений об использовании для ODBCCONF.EXE.|  
|/C|Обработка продолжится при сбое действия.|  
|/E|Очистка файла ответов, указанного параметром/F, после завершения обработки.|  
|/F|Используйте файл ответов, например `odbcconf /F my.rsp` .<br /><br /> мой. rsp может выглядеть следующим образом: `REGSVR c:\my.dll`<br /><br /> /A не используется в файле ответов.|  
|/H|Отображение сведений об использовании (Справка). Этот параметр совпадает с/?.|  
|/L [*mode*] *имя_файла*|Отправка выходных данных программы в файл в одном из трех режимов: обычная (n), Verbose (v) и Debug (d). В режиме отладки записываются библиотеки DLL, загруженные odbcconf.exe.<br /><br /> Если указать/L без режима, файл журнала будет пустым.<br /><br /> Например, **/лв log.txt**.|  
|/R|Действие будет выполнено после перезагрузки.|  
|/S|Автоматический режим Не отображать сообщения об ошибках.|  
  
 Доступны перечисленные ниже действия.  
  
|Действие|Описание|  
|------------|-----------------|  
|КОНФИГДРИВЕР *driver_name * * параметры конфигурации, относящиеся к драйверу*|Загружает соответствующую библиотеку DLL установки драйвера и вызывает функцию **конфигдривер** .<br /><br /> Эквивалент [функции склконфигдривер](../odbc/reference/syntax/sqlconfigdriver-function.md).<br /><br /> Пример:<br /><br /> /A {КОНФИГДРИВЕР "имя драйвера" "Кптимеаут = 60"}<br /><br /> /A {КОНФИГДРИВЕР "имя драйвера" "Дриверодбквер = 03.80"}|  
|CONFIGDSN *driver_name* DSN =*имя* &#124; *атрибуты*|Добавляет или изменяет системный источник данных.<br /><br /> Эквивалент [функции SQLConfigDataSource](../odbc/reference/syntax/sqlconfigdatasource-function.md).<br /><br /> Пример:<br /><br /> /A {CONFIGDSN "SQL Server" "DSN = Name &#124; Server = SRV"}|  
|КОНФИГСИСДСН *driver_name* DSN =*имя* &#124; *атрибуты*|Добавляет или изменяет системный источник данных.<br /><br /> Эквивалент [функции SQLConfigDataSource](../odbc/reference/syntax/sqlconfigdatasource-function.md).<br /><br /> Пример:<br /><br /> /A {КОНФИГСИСДСН "SQL Server" "DSN = Name &#124; Server = SRV"}|  
|инсталлдривер|Эквивалентно [функции склинсталлдриверекс](../odbc/reference/syntax/sqlinstalldriverex-function.md).<br /><br /> Дополнительные сведения о синтаксисе пар «ключевое слово-значение», переданном в ИНСТАЛЛДРИВЕР, см. в разделе [подразделы спецификации драйвера](../odbc/reference/install/driver-specification-subkeys.md).<br /><br /> Пример:<br /><br /> /A {ИНСТАЛЛДРИВЕР "драйвер &#124; драйвер =c:\your.dll &#124; Setup =c:\your.dll &#124; Апилевел = 2 &#124; Коннектфунктионс = YYY &#124; Дриверодбквер = 03.50 &#124; Филеусаже = 0 &#124; Скллевел = 1"}|  
|*Путь к драйверу конфигурации инсталлтранслатор Translator * **|Добавляет сведения о трансляторе в раздел реестра **HKEY_LOCAL_MACHINE\SOFTWARE\ODBC\ODBCINST.INI \Одбк Translator** .<br /><br /> Эквивалентно [функции склинсталлтранслаторекс](../odbc/reference/syntax/sqlinstalltranslatorex-function.md).<br /><br /> Сведения о синтаксисе пар «ключевое слово-значение», переданном в ИНСТАЛЛДРИВЕР, см. в статье [подразделы спецификации переводчика](../odbc/reference/install/translator-specification-subkeys.md).<br /><br /> Пример:<br /><br /> /A {ИНСТАЛЛТРАНСЛАТОР "мой транслятор &#124; переводчик =c:\my.dll &#124; Setup =c:\my.dll"}|  
|*Библиотека DLL* регсвр|Регистрирует библиотеку DLL.<br /><br /> Эквивалентно regsvr32.exe.<br /><br /> Пример:<br /><br /> /A {РЕГСВР c:\my.dll}|  
|сетфиледсндир|Если HKEY_LOCAL_MACHINE\SOFTWARE\ODBC\ODBC.INI файл \ОДБК Дсн\дефаултдсндир не существует, действие СЕТФИЛЕДСНДИР создаст его и присвоит ему значение в HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion\CommonFilesDir, добавляя к источникам \Одбк\дата.<br /><br /> Значение в HKEY_LOCAL_MACHINE\SOFTWARE\ODBC\ODBC.INI \ОДБК File Дсн\дефаултдсндир указывает расположение по умолчанию, используемое администратором источника данных ODBC при создании файлового источника данных.<br /><br /> Пример:<br /><br /> /A {СЕТФИЛЕДСНДИР}|  
  
## <a name="see-also"></a>См. также:  
 [Microsoft Open Database Connectivity (ODBC)](../odbc/microsoft-open-database-connectivity-odbc.md)
