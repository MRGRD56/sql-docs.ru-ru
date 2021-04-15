---
title: Исполнение запросов XPath с пространствами имен (SQLXML)
description: Сведения о том, как включать пространства имен в запросы SQLXML XPath.
ms.date: 03/16/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: xml
ms.topic: reference
helpviewer_keywords:
- namespaces property
- XPath queries [SQLXML], SQLXML Managed Classes
- queries [SQLXML], SQLXML Managed Classes
- XPath queries [SQLXML], namespaces
- Managed Classes [SQLXML], executing XPath queries
- SQLXML Managed Classes, executing XPath queries
- namespaces [SQLXML], XPath queries
ms.assetid: c6fc46d8-6b42-4992-a8f1-a8d4b8886e6e
author: rothja
ms.author: jroth
ms.custom: seo-lt-2019
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: d596ff7ad3def95929ea41face86ca11e960fe2e
ms.sourcegitcommit: 9142bb6b80ce22eeda516b543b163eb9918bc72e
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/14/2021
ms.locfileid: "107491745"
---
# <a name="executing-xpath-queries-with-namespaces-sqlxml-managed-classes"></a>Выполнение запросов XPath с пространствами имен (управляемые классы SQLXML)
[!INCLUDE [SQL Server Azure SQL Database](../../../includes/applies-to-version/sql-asdb.md)]
  В запросы XPath могут быть включены пространства имен. Если имена элементов схемы уточнены именем пространства имен (используйте целевое пространство имен), оно должно быть указано в запросах XPath к этой схеме.  
  
 Поскольку символ-шаблон (*) в [!INCLUDE[msCoName](../../../includes/msconame-md.md)] SQLXML 4.0 не поддерживается, запрос XPath нужно задавать с указанием префикса пространства имен. Для разрешения префикса используйте свойство namespaces, чтобы указать привязку пространства имен.  
  
 В следующем примере запрос XPath задает пространства имен, используя подстановочный знак ( \* ) и функции XPath local-name () и Namespace-URI (). Этот запрос XPath возвращает все элементы, где локальное имя — **Employee** , а URI-код пространства имен — **urn: MySchema: Contacts**:  
  
```  
/*[local-name() = 'Contact' and namespace-uri() = 'urn:myschema:Contacts']  
```  
  
 В SQLXML 4.0 этот запрос XPath необходимо указывать с префиксом пространства имен. Например, **КС:контакт**, где **x** — префикс пространства имен. Рассмотрим следующую схему XSD.  
  
```  
<schema xmlns="http://www.w3.org/2001/XMLSchema"  
            xmlns:sql="urn:schemas-microsoft-com:mapping-schema"  
            xmlns:con="urn:myschema:Contacts"  
            targetNamespace="urn:myschema:Contacts">  
<complexType name="ContactType">  
  <attribute name="CID" sql:field="ContactID" type="ID"/>  
  <attribute name="FName" sql:field="FirstName" type="string"/>  
  <attribute name="LName" sql:field="LastName"/>   
</complexType>  
<element name="Contact" type="con:ContactType" sql:relation="Person.Contact"/>  
</schema>  
```  
  
 Поскольку эта схема задает целевое пространство имен, запрос XPath к этой схеме (например, «Employee») должен содержать пространство имен.  
  
 Следующие образец приложения на C# исполняет запрос XPath к предыдущей схеме XSD (MySchema.xml). Чтобы разрешить префикс, укажите привязку пространства имен с помощью свойства Namespaces объекта SqlXmlCommand.  
  
> [!NOTE]  
>  В коде необходимо задать имя экземпляра [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] в строке соединения.  
  
```  
using System;  
using Microsoft.Data.SqlXml;  
using System.IO;  
class Test  
{  
      static string ConnString = "Provider=SQLOLEDB;Server=(local);database=AdventureWorks;Integrated Security=SSPI";  
      public static int testXPath()  
      {  
         //Stream strm;  
         SqlXmlCommand cmd = new SqlXmlCommand(ConnString);  
         cmd.CommandText = "x:Contact[@CID='1']";  
         cmd.CommandType = SqlXmlCommandType.XPath;  
         cmd.RootTag = "ROOT";  
         cmd.Namespaces = "xmlns:x='urn:myschema:Contacts'";  
         cmd.SchemaPath = "MySchema.xml";  
         using (Stream strm = cmd.ExecuteStream()){  
            using (StreamReader sr = new StreamReader(strm)){  
               Console.WriteLine(sr.ReadToEnd());  
            }  
         }  
         return 0;  
      }  
      public static int Main(String[] args)  
      {  
         testXPath();  
         return 0;  
      }  
   }  
```  
  
 Чтобы проверить этот пример, необходимо установить на компьютер платформу [!INCLUDE[msCoName](../../../includes/msconame-md.md)] .NET Framework.  
  
### <a name="to-test-the-application"></a>Тестирование приложения  
  
1.  Сохраните в папке приведенную в этом примере схему XSD (MySchema.xml).  
  
2.  Сохраните код на языке C# (файл DocSample.cs), приведенный в этом разделе, в той папке, где сохранена схема (если сохранить файлы в разных папках, то придется изменить код, указав путь к каталогу, в котором находится схема сопоставления).  
  
3.  Скомпилируйте код. Чтобы скомпилировать код из командной строки, введите следующую команду.  
  
    ```  
    csc /reference:Microsoft.Data.SqlXML.dll DocSample.cs  
    ```  
  
     Будет создан исполняемый файл (DocSample.exe).  
  
4.  Запустите файл DocSample.exe из командной строки.  

