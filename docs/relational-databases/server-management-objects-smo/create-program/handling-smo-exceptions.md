---
description: Обработка исключений SMO
title: Обработка исключений SMO | Документация Майкрософт
ms.custom: ''
ms.date: 08/06/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: ''
ms.topic: reference
helpviewer_keywords:
- SMO [SQL Server], exceptions
- exceptions [SMO]
- SQL Server Management Objects, exceptions
- inner exceptions [SMO]
ms.assetid: 4c725ff2-6588-44ca-b86a-87979e164153
author: markingmyname
ms.author: maghan
monikerRange: =azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 66956ff6d6b8d904ba20cb68ce012194d6069e75
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 12/14/2020
ms.locfileid: "97463035"
---
# <a name="handling-smo-exceptions"></a>Обработка исключений SMO
[!INCLUDE [SQL Server ASDB, ASDBMI, ASDW ](../../../includes/applies-to-version/sql-asdb-asdbmi-asa.md)]

  В управляемом коде при возникновении ошибок вызываются исключения. Методы и свойства объектов SMO не сообщают об ошибке или успешном выполнении в возвращаемом значении. Исключения могут быть перехвачены и обработаны в обработчике исключений.  
  
 В объектах SMO существуют различные классы исключений. Сведения об исключении можно извлечь из свойств исключения, таких как свойство **Message** , которое предоставляет текстовое сообщение об исключении.  
  
 Инструкции обработки исключения зависят от конкретного языка программирования. Например, в языке [!INCLUDE[msCoName](../../../includes/msconame-md.md)] Visual Basic это инструкция **Catch** .  
  
## <a name="inner-exceptions"></a>Внутренние исключения  
 Исключения могут быть общими и конкретными. Общие исключения содержат набор конкретных исключений. Несколько инструкций **Catch** можно использовать, чтобы обработать ожидаемые ошибки и позволить остальным ошибкам пройти через обработку общих исключений. Исключения часто происходят в каскадной последовательности. Часто исключение объекта SMO может быть вызвано исключением SQL. Чтобы это обнаружить, можно последовательно использовать свойство **InnerException** , чтобы определить исходное исключение, которое вызвало конечное исключение верхнего уровня.  
  
> [!NOTE]  
>  Исключение **SqlException** объявляется в пространстве имен **System. Data. SqlClient** .  
  
 ![Диаграмма, показывающая уровни, из которых могут вызываться исключения](../../../relational-databases/server-management-objects-smo/create-program/media/exception-flow.gif "Диаграмма, показывающая уровни, из которых могут вызываться исключения")  
  
 Диаграмма показывает поток исключений по уровням приложения.  
  
## <a name="example"></a>Пример  
 Чтобы использовать какой-либо из представленных примеров кода, нужно выбрать среду, шаблон и язык программирования, с помощью которых будет создаваться приложение. Дополнительные сведения см. [в статье Создание проекта Visual C&#35; SMO в Visual Studio .NET](../../../relational-databases/server-management-objects-smo/how-to-create-a-visual-csharp-smo-project-in-visual-studio-net.md).
  
## <a name="catching-an-exception-in-visual-basic"></a>Перехват исключения на языке Visual Basic  
 В этом примере кода показано, как использовать метод **try... Перехватить... Оператор finally** [!INCLUDE[vbprvb](../../../includes/vbprvb-md.md)] для перехвата исключения SMO. Все исключения объектов SMO имеют тип SmoException и перечислены в справке по объектам SMO. Последовательность внутренних исключений отображается, чтобы показать основание ошибки. Дополнительные сведения см. в документации по среде [!INCLUDE[vbprvb](../../../includes/vbprvb-md.md)] .NET.  
  
```VBNET
'This sample requires the Microsoft.SqlServer.Management.Smo.Agent namespace is included.
'Connect to the local, default instance of SQL Server.
Dim srv As Server
srv = New Server
'Define an Operator object variable by supplying the parent SQL Agent and the name arguments in the constructor.
'Note that the Operator type requires [] parenthesis to differentiate it from a Visual Basic key word.
Dim op As [Operator]
op = New [Operator](srv.JobServer, "Test_Operator")
op.Create()
'Start exception handling.
Try
    'Create the operator again to cause an SMO exception.
    Dim opx As OperatorCategory
    opx = New OperatorCategory(srv.JobServer, "Test_Operator")
    opx.Create()
    'Catch the SMO exception
Catch smoex As SmoException
    Console.WriteLine("This is an SMO Exception")
    'Display the SMO exception message.
    Console.WriteLine(smoex.Message)
    'Display the sequence of non-SMO exceptions that caused the SMO exception.
    Dim ex As Exception
    ex = smoex.InnerException
    Do While ex.InnerException IsNot (Nothing)
        Console.WriteLine(ex.InnerException.Message)
        ex = ex.InnerException
    Loop
    'Catch other non-SMO exceptions.
Catch ex As Exception
    Console.WriteLine("This is not an SMO exception.")
End Try
``` 
  
## <a name="catching-an-exception-in-visual-c"></a>Перехват исключения на языке Visual C#  
 В этом примере кода показано, как использовать метод **try... Перехватить... Наконец** , инструкция Visual C# для перехвата исключения SMO. Все исключения объектов SMO имеют тип SmoException и перечислены в справке по объектам SMO. Последовательность внутренних исключений отображается, чтобы показать основание ошибки. Дополнительные сведения см. в документации по языку Visual C#.  
  
```csharp  
{   
//This sample requires the Microsoft.SqlServer.Management.Smo.Agent namespace to be included.   
//Connect to the local, default instance of SQL Server.   
Server srv;   
srv = new Server();   
//Define an Operator object variable by supplying the parent SQL Agent and the name arguments in the constructor.   
//Note that the Operator type requires [] parenthesis to differentiate it from a Visual Basic key word.   
op = new Operator(srv.JobServer, "Test_Operator");   
op.Create();   
//Start exception handling.   
try {   
    //Create the operator again to cause an SMO exception.   
    OperatorCategory opx;   
    opx = new OperatorCategory(srv.JobServer, "Test_Operator");   
    opx.Create();   
}   
//Catch the SMO exception   
catch (SmoException smoex) {   
    Console.WriteLine("This is an SMO Exception");   
   //Display the SMO exception message.   
   Console.WriteLine(smoex.Message);   
   //Display the sequence of non-SMO exceptions that caused the SMO exception.   
   Exception ex;   
   ex = smoex.InnerException;   
   while (!object.ReferenceEquals(ex.InnerException, (null))) {   
      Console.WriteLine(ex.InnerException.Message);   
      ex = ex.InnerException;   
    }   
    }   
   //Catch other non-SMO exceptions.   
   catch (Exception ex) {   
      Console.WriteLine("This is not an SMO exception.");   
}   
}  
```  
  
  
