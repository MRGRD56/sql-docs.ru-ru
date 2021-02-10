---
description: IsolationLevelEnum
title: Исолатионлевеленум | Документация Майкрософт
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
f1_keywords:
- IsolationLevelEnum
helpviewer_keywords:
- IsolationLevelEnum enumeration [ADO]
ms.assetid: 8e17a7bc-b8a3-4ae2-b6c9-ce088ad31fdf
author: rothja
ms.author: jroth
ms.openlocfilehash: 7f01e97bea7d7e066d39898c7d76eb33ddbf710d
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/10/2021
ms.locfileid: "100041914"
---
# <a name="isolationlevelenum"></a>IsolationLevelEnum
Задает уровень изоляции транзакции для объекта [соединения](./connection-object-ado.md) .  
  
|Константа|Значение|Описание|  
|--------------|-----------|-----------------|  
|**адксактунспеЦифиед**|-1|Указывает, что поставщик использует уровень изоляции, отличный от указанного, но уровень не может быть определен.|  
|**адксактчаос**|16|Указывает, что ожидающие изменения более строго изолированных транзакций не могут быть перезаписаны.|  
|**adXactBrowse**|256|Указывает, что из одной транзакции можно просмотреть незафиксированные изменения в других транзакциях.|  
|**Connection значения adxactreaduncommitted**|256|То же, что и **адксактбровсе**.|  
|**адксакткурсорстабилити**|4096|Указывает, что из одной транзакции можно просматривать изменения в других транзакциях только после их фиксации.|  
|**adXactReadCommitted**|4096|То же, что и **адксакткурсорстабилити**.|  
|**adXactRepeatableRead**|65536|Указывает, что из одной транзакции нельзя видеть изменения, внесенные в другие транзакции, но эти запросы могут извлекать новые объекты **набора записей** .|  
|**адксактисолатед**|1048576|Указывает, что транзакции выполняются в режиме изоляции других транзакций.|  
|**адксактсериализабле**|1048576|То же, что и **адксактисолатед**.|  
  
## <a name="adowfc-equivalent"></a>Эквивалент ADO/WFC  
 Пакет: **com. MS. WFC. Data**  
  
|Константа|  
|--------------|  
|Адоенумс. IsolationLevel. не указано|  
|Адоенумс. IsolationLevel. CHAOS|  
|Адоенумс. IsolationLevel. Обзор|  
|Адоенумс. IsolationLevel. READUNCOMMITTED|  
|Адоенумс. IsolationLevel. КУРСОРСТАБИЛИТИ|  
|Адоенумс. IsolationLevel. READCOMMITTED|  
|Адоенумс. IsolationLevel. REPEATABLEREAD|  
|Адоенумс. IsolationLevel. изоляцией|  
|Адоенумс. IsolationLevel. SERIALIZABLE|  
  
## <a name="applies-to"></a>Применение  
 [Свойство IsolationLevel](./isolationlevel-property.md)