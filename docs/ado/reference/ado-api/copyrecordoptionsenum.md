---
description: CopyRecordOptionsEnum
title: Копирекордоптионсенум | Документация Майкрософт
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
f1_keywords:
- CopyRecordOptionsEnum
helpviewer_keywords:
- CopyRecordOptionsEnum enumeration [ADO]
ms.assetid: 2fa4eec5-d50b-4fd3-8ae7-40af441ba12b
author: rothja
ms.author: jroth
ms.openlocfilehash: 2afe394c4b6251ea21d7245d55ab905ba6347fcc
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/30/2021
ms.locfileid: "99167684"
---
# <a name="copyrecordoptionsenum"></a>CopyRecordOptionsEnum
Задает поведение метода [копирекорд](./copyrecord-method-ado.md) .  
  
|Константа|Значение|Описание|  
|--------------|-----------|-----------------|  
|**adCopyAllowEmulation**|4|Указывает, что поставщик *источника* пытается имитировать копирование с помощью операций скачивания и отправки, если этот метод завершается сбоем из-за того, что *целевой объект* находится на другом сервере или обслуживается другим поставщиком, чем *источник*. Обратите внимание, что различные возможности поставщика могут препятствовать повышению производительности или потере данных.|  
|**adCopyNonRecursive**|2|Копирует текущий каталог, но не все его подкаталоги в место назначения. Операция копирования не является рекурсивной.|  
|**adCopyOverWrite**|1|Перезаписывает файл или каталог, если *целевой* объект указывает на существующий файл или каталог.|  
|**adCopyUnspecified**|-1|По умолчанию. Выполняет операцию копирования по умолчанию: операция завершается ошибкой, если целевой файл или каталог уже существует, а операция копируется рекурсивно.|  
  
## <a name="adowfc-equivalent"></a>Эквивалент ADO/WFC  
 Эти константы не имеют эквивалентов ADO/WFC.  
  
## <a name="applies-to"></a>Применение  
 [Метод CopyRecord (ADO)](./copyrecord-method-ado.md)