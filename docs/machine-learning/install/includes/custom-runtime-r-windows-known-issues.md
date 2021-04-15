---
ms.prod: sql
ms.technology: machine-learning-services
ms.date: 04/07/2021
ms.topic: include
author: anmunde
ms.author: anmunde
ms.reviewer: dphansen
ms.openlocfilehash: dbfc7de86a0ecf06c2eca0c6d49e96ca8e3ea39e
ms.sourcegitcommit: 09122d02fc3d86c6028366653337c083da8a3f4c
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/08/2021
ms.locfileid: "107072391"
---
## <a name="known-issues"></a>Известные проблемы

Если вы используете среду выполнения R, входящую в состав [Служб машинного обучения SQL Server](../../sql-server-machine-learning-services.md), задав значение `C:\Program Files\Microsoft SQL Server\MSSQL15.<INSTANCE_NAME>\R_SERVICES` для параметра `R_HOME` при регистрации расширения языка, то при выполнении любого внешнего пользовательского скрипта R с помощью хранимой процедуры [sp_execute_external script](../../../relational-databases/system-stored-procedures/sp-execute-external-script-transact-sql.md) может возникнуть следующая ошибка.

*Ошибка: нехватка памяти (достигнуто ограничение?)*

Чтобы устранить эту проблему, выполните следующие действия.
 1. Установите подходящее значение для переменной среды `R_NSIZE`, указывающей количество объектов фиксированного размера (`cons cells`), например, `200000`.
 1. Перезапустите службу **панели запуска** и выполните скрипт еще раз.