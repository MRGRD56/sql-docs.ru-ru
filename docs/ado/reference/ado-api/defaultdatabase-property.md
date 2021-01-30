---
description: Свойство DefaultDatabase
title: DefaultDatabase, свойство | Документация Майкрософт
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
f1_keywords:
- Connection15::DefaultDatabase
helpviewer_keywords:
- DefaultDatabase property
ms.assetid: 41e8a8dd-e69c-4a09-8736-93502e01961c
author: rothja
ms.author: jroth
ms.openlocfilehash: 8cff37ae3bc0940a62e2acc9af3ac6c25d1379a4
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/30/2021
ms.locfileid: "99167586"
---
# <a name="defaultdatabase-property"></a>Свойство DefaultDatabase
Указывает базу данных по умолчанию для объекта [соединения](../../../ado/reference/ado-api/connection-object-ado.md) .  
  
## <a name="settings-and-return-values"></a>Параметры и возвращаемые значения  
 Задает или возвращает **строковое** значение, результатом которого является имя базы данных, доступной из поставщика.  
  
## <a name="remarks"></a>Замечания  
 Используйте свойство **DefaultDatabase** , чтобы задать или вернуть имя базы данных по умолчанию для конкретного объекта **соединения** .  
  
 При наличии базы данных по умолчанию строки SQL могут использовать неполный синтаксис для доступа к объектам в этой базе данных. Для доступа к объектам в базе данных, отличной от указанной в свойстве **DefaultDatabase** , необходимо указать имена объектов с нужным именем базы данных. При подключении поставщик будет записывать сведения о базе данных по умолчанию в свойство **DefaultDatabase** . Некоторые поставщики разрешают только одну базу данных на одно соединение. в этом случае изменить свойство **DefaultDatabase** нельзя.  
  
 Некоторые источники данных и поставщики могут не поддерживать эту функцию и могут возвращать ошибку или пустую строку.  
  
> [!NOTE]
>  **Использование удаленной службы данных** Это свойство недоступно для объекта **подключения** на стороне клиента.  
  
## <a name="applies-to"></a>Применение  
 [Объект Connection (ADO)](../../../ado/reference/ado-api/connection-object-ado.md)  
  
## <a name="see-also"></a>См. также:  
 [Пример свойств provider и DefaultDatabase (Visual Basic)](../../../ado/reference/ado-api/provider-and-defaultdatabase-properties-example-vb.md)   
 [Примеры свойств Provider и DefaultDatabase (Visual C++)](../../../ado/reference/ado-api/provider-and-defaultdatabase-properties-example-vc.md)   
