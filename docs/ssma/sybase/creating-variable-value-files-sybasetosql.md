---
description: Создание файлов переменных значений (SybaseToSQL)
title: Создание файлов переменных значений (SybaseToSQL) | Документация Майкрософт
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: ssma
ms.topic: conceptual
helpviewer_keywords:
- Sybase Console,Creating Variable Value Files
- Sybase Console,Variable Value File Validation
ms.assetid: 395be464-4b19-44f7-91e5-b8876d6743dc
author: nahk-ivanov
ms.author: alexiva
ms.openlocfilehash: 2f2d3a53cb12a0d7762137a44cc12ec6f06a5d81
ms.sourcegitcommit: 22dacedeb6e8721e7cdb6279a946d4002cfb5da3
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/14/2020
ms.locfileid: "92036703"
---
# <a name="creating-variable-value-files-sybasetosql"></a>Создание файлов переменных значений (SybaseToSQL)
Файл значения переменной — это XML-файл, состоящий из значений параметров таких команд, как, имя исходного или целевого сервера, которое часто меняются при миграции с одного сервера на другой. При выполнении большого количества миграций базы данных создается несколько переменных файлов для хранения значений каждого исходного сервера и на них указывают ссылки в файле главного сценария с помощью параметра **-v** в командной строке. Это помогает поддерживать статические значения в нескольких файлах скриптов с переменными значениями в нескольких файлах переменных.  
  
> [!NOTE]  
> 1.  Имена переменных добавляются с префиксом в символ $ (доллар). Если переменным не присвоено значение в файле значения переменной, то во время синтаксического анализа файла скрипта будет возникать ошибка, что привело к остановке процесса выполнения консоли.  
> 2.  Escape-символ для **$** имеет значение **$$** . Если значение переменной или статического значения параметра содержит **$** символ (доллар), то **$$** необходимо указать, что он будет обрабатываться как символ, а не как переменная.  
> 3.  Для целей обслуживания переменные могут быть объявлены внутри `'variable-group'` элементов для логического разделения определяемых пользователем переменных.  Использование этого элемента не является обязательным.  
  
**Примеры:**  
  
**Пример 1**.  
  
```xml  
<!--Sample of variable value file commands-->  
  
<variables>  
  
  <variable-group name="ProjectSpecs">  
  
    <variable name="$project_folder$" value="<project-folder>"/>  
  
    <variable name="$project_name$" value="<project-name>"/>  
  
    <variable name="$project_overwrite$" value="<true/false>"/>  
  
    <variable name="$project_type$" value="<project-type>"/>  
  
  </variable-group>  
  
</variables>  
```  
**Пример 2.**  
  
```xml  
<!--Sample of variable value file commands-->  
  
<variables>  
  
  <variable-group name="SQLServerParams">  
  
    <variable-group name="SqlServerConnectionParams">  
  
      <variable name="$TargetUserName$" value="<user-name>"/>  
  
      <variable name="$TargetServerName$" value="<server-name>"/>  
  
      <variable name="$TargetDB$" value="<database-name>"/>  
  
      <variable name="$TargetPassword$" value="<password>"/>  
  
      <variable name="$TrustedConnection$" value="<true/false>"/>  
  
    </variable-group>  
  
    <variable-group name="SqlServerObjectParams">  
  
      <variable name="$ObjectName1$" value="<object-name>"/>  
  
      <variable name="$ObjectName2$" value="<object-name>"/>  
  
    </variable-group>  
  
  </variable-group>  
  
</variables>  
```  
  
## <a name="variable-value-file-validation"></a>Проверка файла значения переменной  
Пользователь может легко проверить свой файл значения переменной в файле определения схемы **консолескриптвариаблессчема. xsd** , доступном в папке Schemas.  
  
## <a name="next-step"></a>Следующий шаг  
Следующий шаг в работе консоли — [Создание файлов подключения к серверу &#40;SybaseToSQL&#41;](../../ssma/sybase/creating-the-server-connection-files-sybasetosql.md)  
  
## <a name="see-also"></a>См. также:  
[Создание серверных файлов (Sybase)](./creating-the-server-connection-files-sybasetosql.md)  
