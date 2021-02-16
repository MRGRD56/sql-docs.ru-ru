---
title: IServerVirtualDeviceSet2::ExecuteCompletionAgent
titlesuffix: SQL Server VDI reference
description: В этой статье приводятся справочные сведения о команде IServerVirtualDeviceSet2::ExecuteCompletionAgent.
ms.date: 08/30/2019
ms.prod: sql
ms.prod_service: backup-restore
ms.technology: backup-restore
ms.topic: reference
author: cawrites
ms.author: chadam
ms.openlocfilehash: 75b09df68be51f74cbb62dac5ddf039259ad79f1
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/10/2021
ms.locfileid: "100347105"
---
# <a name="iservervirtualdeviceset2executecompletionagent-vdi"></a>IServerVirtualDeviceSet2::ExecuteCompletionAgent (VDI)

[!INCLUDE [SQL Server](../../../includes/applies-to-version/sqlserver.md)]

Функция **ExecuteCompletionAgent** используется для реализации основного цикла агента завершения.

## <a name="syntax"></a>Синтаксис

```c
HRESULT IServerVirtualDeviceSet2::ExecuteCompletionAgent ();
```

## <a name="return-value"></a>Возвращаемое значение

Возвращает значение *HRESULT* , являющееся признаком успешного или неуспешного завершение вызова метода. Значение NOERROR указывает, что вызов метода завершился успешно. Ненулевое значение указывает, что произошла ошибка.

## <a name="remarks"></a>Remarks

Агент завершения обеспечивает механизм, с помощью которого SQL Server может синхронизироваться с завершением команд виртуального устройства. Он должен быть активирован до выполнения команд и оставаться активным до тех пор, пока все устройства не будут закрыты.

Так как серверу SQL Server может потребоваться выполнить особую инициализацию потока, этот интерфейс не запускает новый поток управления. Вместо этого SQL Server настраивает поток, а затем передает управление этой подпрограмме. Поток должен иметь возможность блокировки в механизмах межпроцессного взаимодействия (IPC) Windows NT и возможность вызывать любые функции обратного вызова, которые предоставляются командами, отправляемыми через IServerVirtualDevice::SendCommand.

Эта функция не возвращает управление, пока не будет вызвана функция IServerVirtualDeviceSet2::Close или SignalAbort.

## <a name="next-steps"></a>Дальнейшие действия

Дополнительные сведения см. в [справке по интерфейсу виртуальных устройств SQL Server](reference-virtual-device-interface.md).