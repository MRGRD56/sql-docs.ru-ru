---
description: Ведение журнала в задаче «Скрипт»
title: Ведение журнала в задаче "Скрипт" | Документы Майкрософт
ms.custom: ''
ms.date: 03/06/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: reference
dev_langs:
- VB
helpviewer_keywords:
- SQL Server Integration Services packages, logs
- SSIS packages, logs
- logs [Integration Services], scripts
- Integration Services packages, logs
- Log method
- SSIS Script task, logs
- Script task [Integration Services], logs
- packages [Integration Services], logs
ms.assetid: 2e11fc15-df18-4309-bd2d-fc58aa4b9b7a
author: chugugrace
ms.author: chugu
ms.openlocfilehash: 2ea76691d3fb5cf6f3fcdd4ba5043d9af8b0484b
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/10/2021
ms.locfileid: "100351549"
---
# <a name="logging-in-the-script-task"></a>Ведение журнала в задаче «Скрипт»

[!INCLUDE[sqlserver-ssis](../../../includes/applies-to-version/sqlserver-ssis.md)]


  Ведение журнала в пакетах служб [!INCLUDE[ssISnoversion](../../../includes/ssisnoversion-md.md)] позволяет записывать для последующего анализа извлекаемую из стандартных событий или определяемых пользователем сообщений подробную информацию о ходе выполнения, полученных результатах и возникших проблемах. Задача "Скрипт" может использовать метод <xref:Microsoft.SqlServer.Dts.Tasks.ScriptTask.ScriptObjectModel.Log%2A> объекта **Dts** для записи в журнал определяемых пользователем данных. Если ведение журнала разрешено и событие **ScriptTaskLogEntry** выбрано для записи в журнал на вкладке **Подробности** диалогового окна **Настройка журналов служб SSIS**, однократный вызов метода <xref:Microsoft.SqlServer.Dts.Tasks.ScriptTask.ScriptObjectModel.Log%2A> сохраняет сведения о событиях во всех регистраторах, настроенных для данной задачи.  
  
> [!NOTE]  
>  Хотя журнал можно вести непосредственно из задачи «Скрипт», можно реализовать события вместо ведения журнала. При использовании событий можно не только включить запись в журнал сообщений о событиях, но и реагировать на события с помощью обработчиков событий, определяемых пользователем или заданных по умолчанию.  
  
 Дополнительные сведения о ведении журналов см. в разделе [Ведение журналов в службах Integration Services (SSIS)](../../../integration-services/performance/integration-services-ssis-logging.md).  
  
## <a name="logging-example"></a>Пример ведения журнала  
 Следующий пример иллюстрирует ведение журнала из задачи «Скрипт» путем записи в журнал значения, представляющего число обработанных строк.  
  
```vb  
Public Sub Main()  
  
    Dim rowsProcessed As Integer = 100  
    Dim emptyBytes(0) As Byte  
  
    Try  
        Dts.Log("Rows processed: " & rowsProcessed.ToString, _  
            0, _  
            emptyBytes)  
        Dts.TaskResult = ScriptResults.Success  
    Catch ex As Exception  
        'An error occurred.  
        Dts.Events.FireError(0, "Script Task Example", _  
            ex.Message & ControlChars.CrLf & ex.StackTrace, _  
            String.Empty, 0)  
        Dts.TaskResult = ScriptResults.Failure  
    End Try  
  
End Sub  
```  
  
```csharp  
using System;  
using System.Data;  
using Microsoft.SqlServer.Dts.Runtime;  
  
public class ScriptMain  
{  
  
    public void Main()  
        {  
            //  
            int rowsProcessed = 100;  
            byte[] emptyBytes = new byte[0];  
  
            try  
            {  
                Dts.Log("Rows processed: " + rowsProcessed.ToString(), 0, emptyBytes);  
                Dts.TaskResult = (int)ScriptResults.Success;  
            }  
            catch (Exception ex)  
            {  
                //An error occurred.  
                Dts.Events.FireError(0, "Script Task Example", ex.Message + "\r" + ex.StackTrace, String.Empty, 0);  
                Dts.TaskResult = (int)ScriptResults.Failure;  
            }  
  
        }  
```  
  
 }  
  
## <a name="see-also"></a>См. также:  
 [Ведение журналов в службах Integration Services (SSIS)](../../../integration-services/performance/integration-services-ssis-logging.md)  
  
  
