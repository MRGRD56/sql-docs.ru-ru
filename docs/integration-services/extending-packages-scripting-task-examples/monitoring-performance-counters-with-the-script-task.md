---
description: Наблюдение за счетчиками производительности в задаче «Скрипт»
title: Наблюдение за счетчиками производительности в задаче "Скрипт" | Документы Майкрософт
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: reference
dev_langs:
- VB
helpviewer_keywords:
- performance counters [Integration Services]
- SSIS Script task, performance counters
- custom performance counters [Integration Services]
- Script task [Integration Services], examples
- Script task [Integration Services], performance counters
- counters [Integration Services]
ms.assetid: 86609bf1-cae6-435e-a58d-41bdfc521e94
author: chugugrace
ms.author: chugu
ms.openlocfilehash: b05cfaa612a9e08fb503ee9d4448f871d9cd7904
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/10/2021
ms.locfileid: "100344840"
---
# <a name="monitoring-performance-counters-with-the-script-task"></a>Наблюдение за счетчиками производительности в задаче «Скрипт»

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]


  Администраторам необходимо наблюдать за производительностью пакетов служб [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)], выполняющих сложные преобразования больших объемов данных. Пространство имен **System.Diagnostics** платформы [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[dnprdnshort](../../includes/dnprdnshort-md.md)] предоставляет классы для использования существующих счетчиков производительности и создания собственных счетчиков производительности.  
  
 Счетчики производительности хранят сведения о производительности приложения, которые можно использовать для анализа производительности программного обеспечения по времени. За счетчиками производительности можно наблюдать локально или удаленно с помощью средства **Системный монитор**. Значения счетчиков производительности можно сохранить в переменных, чтобы далее в пакете можно было организовать ветвление потока управления.  
  
 В качестве альтернативы счетчикам производительности можно вызывать событие <xref:Microsoft.SqlServer.Dts.Runtime.IDTSComponentEvents.FireProgress%2A> через свойство <xref:Microsoft.SqlServer.Dts.Tasks.ScriptTask.ScriptObjectModel.Events%2A> объекта **Dts**. Событие <xref:Microsoft.SqlServer.Dts.Runtime.IDTSComponentEvents.FireProgress%2A> возвращает среде выполнения служб [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] как сведения о добавочном ходе выполнения, так и процент завершения задачи.  
  
> [!NOTE]  
>  Если нужно создать задачу, которую будет удобно использовать в нескольких пакетах, рекомендуется начать разработку пользовательской задачи с этого образца задачи «Скрипт». Дополнительные сведения см. в разделе [Разработка пользовательской задачи](../../integration-services/extending-packages-custom-objects/task/developing-a-custom-task.md).  
  
## <a name="description"></a>Description  
 Следующий пример создает пользовательский счетчик производительности и увеличивает его значение. Во-первых, пример определяет, существует ли счетчик производительности. Если счетчик производительности отсутствует, скрипт вызывает метод **Create** объекта **PerformanceCounterCategory** для его создания. После создания счетчика производительности скрипт увеличивает его значение. Наконец, пример следует рекомендациям и вызывает метод **Close** для счетчика производительности, когда он становится ненужным.  
  
> [!NOTE]  
>  Чтобы создать новую категорию счетчика производительности и сам счетчик производительности, необходимы права администратора. Кроме того, новая категория и счетчик производительности после создания сохраняются на компьютере.  
  
#### <a name="to-configure-this-script-task-example"></a>Настройка этого образца задачи «Скрипт»  
  
-   Используйте инструкцию **Imports** в коде, чтобы импортировать пространство имен **System.Diagnostics**.  
  
### <a name="example-code"></a>Пример кода  
  
```vb  
Public Sub Main()  
  
    Dim myCounter As PerformanceCounter  
  
    Try  
        'Create the performance counter if it does not already exist.  
        If Not _  
        PerformanceCounterCategory.Exists("TaskExample") Then  
            PerformanceCounterCategory.Create("TaskExample", _  
                "Task Performance Counter Example", "Iterations", _  
                "Number of times this task has been called.")  
        End If  
  
        'Initialize the performance counter.  
        myCounter = New PerformanceCounter("TaskExample", _  
            "Iterations", String.Empty, False)  
  
        'Increment the performance counter.  
        myCounter.Increment()  
  
         myCounter.Close()  
        Dts.TaskResult = ScriptResults.Success  
    Catch ex As Exception  
        Dts.Events.FireError(0, _  
            "Task Performance Counter Example", _  
            ex.Message & ControlChars.CrLf & ex.StackTrace, _  
            String.Empty, 0)  
        Dts.TaskResult = ScriptResults.Failure  
    End Try  
  
End Sub  
```  
  
```csharp  
  
public class ScriptMain  
{  
  
public void Main()  
        {  
  
            PerformanceCounter myCounter;  
  
            try  
            {  
                //Create the performance counter if it does not already exist.  
                if (!PerformanceCounterCategory.Exists("TaskExample"))  
                {  
                    PerformanceCounterCategory.Create("TaskExample", "Task Performance Counter Example", "Iterations", "Number of times this task has been called.");  
                }  
  
                //Initialize the performance counter.  
                myCounter = new PerformanceCounter("TaskExample", "Iterations", String.Empty, false);  
  
                //Increment the performance counter.  
                myCounter.Increment();  
  
                myCounter.Close();  
                Dts.TaskResult = (int)ScriptResults.Success;  
            }  
            catch (Exception ex)  
            {  
                Dts.Events.FireError(0, "Task Performance Counter Example", ex.Message + "\r" + ex.StackTrace, String.Empty, 0);  
                Dts.TaskResult = (int)ScriptResults.Failure;  
            }  
  
            Dts.TaskResult = (int)ScriptResults.Success;  
        }  
  
```  
