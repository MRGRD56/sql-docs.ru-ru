---
description: MoveRecordOptionsEnum
title: Моверекордоптионсенум | Документация Майкрософт
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
f1_keywords:
- MoveRecordOptionsEnum
helpviewer_keywords:
- MoveRecordOptionsEnum enumeration [ADO]
ms.assetid: f53c2ce4-1021-4a45-92b8-775e8bebad99
author: rothja
ms.author: jroth
ms.openlocfilehash: 8a6556944a56cd695fe4b336f7080c9ea79f0f4c
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/30/2021
ms.locfileid: "99170781"
---
# <a name="moverecordoptionsenum"></a>MoveRecordOptionsEnum
Задает поведение метода [MoveRecord](./moverecord-method-ado.md) объекта [записи](./record-object-ado.md) .  
  
|Константа|Значение|Описание|  
|--------------|-----------|-----------------|  
|**adMoveUnspecified**|-1|По умолчанию. Выполняет операцию перемещения по умолчанию: операция завершается ошибкой, если целевой файл или каталог уже существует, а операция обновляет гипертекстовые ссылки.|  
|**adMoveOverWrite**|1|Перезаписывает целевой файл или каталог, даже если он уже существует.|  
|**adMoveDontUpdateLinks**|2|Изменяет поведение метода **MoveRecord** по умолчанию, не обновляя гипертекстовые ссылки исходной **записи**. Поведение по умолчанию зависит от возможностей поставщика. Операция перемещения обновляет ссылки, если поставщик поддерживается. Если поставщик не может исправить ссылки или это значение не указано, перемещение проходит успешно, даже если ссылки не были исправлены.|  
|**adMoveAllowEmulation**|4|Запрашивает, что поставщик пытается имитировать перемещение (с помощью операций скачивания, отправки и удаления). Если попытка перемещения **записи** завершается ошибкой, так как URL-адрес назначения находится на другом сервере или обслуживается другим поставщиком, отличным от поставщика источника, это может привести к увеличению задержки или потере данных из-за различных возможностей поставщика при перемещении ресурсов между поставщиками.|  
  
## <a name="adowfc-equivalent"></a>Эквивалент ADO/WFC  
 Эти константы не имеют эквивалентов ADO/WFC.  
  
## <a name="applies-to"></a>Применение  
 [Метод MoveRecord (ADO)](./moverecord-method-ado.md)