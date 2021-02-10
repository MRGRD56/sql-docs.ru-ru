---
description: Событие FetchProgress (ADO)
title: Событие Фетчпрогресс (ADO) | Документация Майкрософт
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
f1_keywords:
- FetchProgress
- Recordset::FetchProgress
helpviewer_keywords:
- FetchProgress event [ADO]
ms.assetid: 301716fd-81fc-40eb-8a04-221ef7ab410e
author: rothja
ms.author: jroth
ms.openlocfilehash: fa10ddcef5adfd340d2cbd690ff54d8ab62f35ff
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/10/2021
ms.locfileid: "100034074"
---
# <a name="fetchprogress-event-ado"></a>Событие FetchProgress (ADO)
Событие **фетчпрогресс** вызывается периодически во время длительной асинхронной операции для сообщения о том, сколько строк в данный момент было извлечено в [набор записей](../../../ado/reference/ado-api/recordset-object-ado.md).  
  
## <a name="syntax"></a>Синтаксис  
  
```  
  
FetchProgress Progress, MaxProgress, adStatus, pRecordset  
```  
  
#### <a name="parameters"></a>Параметры  
 *Ход выполнения*  
 Значение **типа Long** , указывающее количество записей, которые в настоящее время были извлечены операцией выборки.  
  
 *макспрогресс*  
 Значение **типа Long** , указывающее максимальное число записей, ожидаемых для извлечения.  
  
 *адстатус*  
 Значение состояния [евентстатусенум](../../../ado/reference/ado-api/eventstatusenum.md) .  
  
 *предшнур*  
 Объект **набора записей** , представляющий собой объект, для которого извлекаются записи.  
  
## <a name="remarks"></a>Remarks  
 При использовании **фетчпрогресс** с дочерним **набором записей** имейте в виду, что значения параметров *Progress* и *макспрогресс* являются производными от базового набора строк [службы курсора](../../../ado/guide/appendixes/microsoft-cursor-service-for-ole-db-ado-service-component.md) . Возвращаемые значения представляют общее количество записей в базовом наборе строк, а не только число записей в текущей главе.  
  
> [!NOTE]
>  Чтобы использовать **фетчпрогресс** с Microsoft Visual Basic, требуется Visual Basic 6,0 или более поздней версии.  
  
## <a name="see-also"></a>См. также:  
 [Пример модели событий ADO (Visual c++)](../../../ado/reference/ado-api/ado-events-model-example-vc.md)   
 [Общие сведения об обработчике событий ADO](../../../ado/guide/data/ado-event-handler-summary.md)
