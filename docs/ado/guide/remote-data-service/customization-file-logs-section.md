---
description: Настройка раздела журналов файла
title: Раздел журналов файлов настройки | Документация Майкрософт
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 11/09/2018
ms.reviewer: ''
ms.topic: conceptual
helpviewer_keywords:
- logs section in RDS [ADO]
- customization file in RDS [ADO]
ms.assetid: a368e264-865c-41ee-be00-d9097255c2ea
author: rothja
ms.author: jroth
ms.openlocfilehash: 34485c91a20b8c5df668e5eddd11e442418844a9
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/10/2021
ms.locfileid: "100036534"
---
# <a name="customization-file-logs-section"></a>Настройка раздела журналов файла
Раздел **журналы** содержит запись файла журнала, которая указывает имя файла, записывающего ошибки во время выполнения **операции.**  
  
> [!IMPORTANT]
>  Начиная с Windows 8 и Windows Server 2012, компоненты RDS больше не включены в операционную систему Windows (Дополнительные сведения см. в статье о совместимости Windows 8 и [Windows server 2012 Cookbook](https://www.microsoft.com/download/details.aspx?id=27416) ). Клиентские компоненты RDS будут удалены в следующей версии Windows. Избегайте использования этого компонента в новых разработках и запланируйте изменение существующих приложений, в которых он применяется. Приложения, использующие RDS, должны переноситься в [службу данных WCF](/dotnet/framework/wcf/).  
  
## <a name="syntax"></a>Синтаксис  
 Запись файла журнала имеет вид:  
  
```console
  
err=  
FileName  
  
```  
  
## <a name="remarks"></a>Remarks  
  
|Часть|Описание|  
|----------|-----------------|  
|**Err**|Литеральная строка, указывающая, что это запись файла журнала.|  
|*FileName*|Полный путь и имя файла. Обычно используется имя файла **к:\мсдфмап.лог**.|  
  
 Файл журнала будет содержать имя пользователя, HRESULT, дату и время каждой ошибки.  
  
## <a name="see-also"></a>См. также:  
 [Раздел "Подключение файла настройки"](./customization-file-connect-section.md)   
 [Раздел файла настройки SQL](./customization-file-sql-section.md)   
 [Раздел UserList файла настройки](./customization-file-userlist-section.md)   
 [Настройка в отношении фактов](./datafactory-customization.md)   
 [Требуемые параметры клиента](./required-client-settings.md)   
 [Общие сведения о файле настройки](./understanding-the-customization-file.md)   
 [Создание собственного настраиваемого обработчика](./writing-your-own-customized-handler.md)