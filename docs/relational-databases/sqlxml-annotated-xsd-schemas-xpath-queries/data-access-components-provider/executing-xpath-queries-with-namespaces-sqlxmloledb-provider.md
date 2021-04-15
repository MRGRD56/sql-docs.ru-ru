---
title: Выполнение запросов XPath с пространствами имен (SQLXMLOLEDB)
description: Узнайте, как указать пространства имен в SQLXML 4,0 при выполнении запросов XPath с помощью поставщика SQLXMLOLEDB.
ms.date: 03/16/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: xml
ms.topic: reference
helpviewer_keywords:
- SQLXMLOLEDB Provider, executing XPath queries
- namespaces property
- queries [SQLXML], SQLXMLOLEDB Provider
- XPath queries [SQLXML], namespaces
- XPath queries [SQLXML], SQLXMLOLEDB Provider
- namespaces [SQLXML], XPath queries
ms.assetid: 024a4b7d-435d-47ba-9e80-2c2f640108f5
author: rothja
ms.author: jroth
ms.custom: seo-lt-2019
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 1cfd963299f9e9364759bf588e4df774c1db72ef
ms.sourcegitcommit: 9142bb6b80ce22eeda516b543b163eb9918bc72e
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/14/2021
ms.locfileid: "107491972"
---
# <a name="executing-xpath-queries-with-namespaces-sqlxmloledb-provider"></a>Выполнение запросов XPath с пространствами имен (поставщик SQLXMLOLEDB)
[!INCLUDE [SQL Server Azure SQL Database](../../../includes/applies-to-version/sql-asdb.md)]
  В запросы XPath могут быть включены пространства имен. Если для элементов схемы указано пространство имен (то есть в случае включения целевого пространства имен), то запросы XPath к схеме должны указывать данное пространство имен.  
  
 Так как использование символа-шаблона (*) не поддерживается в SQLXML 4.0, необходимо указать запрос XPath с помощью префикса пространства имен. Для разрешения этого префикса используйте свойство namespaces, чтобы указать привязку пространства имен.  
  
 В следующем примере запрос XPath задает пространства имен, используя подстановочный знак ( \* ) и функции XPath local-name () и Namespace-URI (). Этот запрос XPath возвращает все элементы, в которых локальным именем является **Contact** , а URI пространства имен — **urn: MySchema: Contacts**.  
  
```  
/*[local-name() = 'Contact' and namespace-uri() = 'urn:myschema:Contacts']  
```  
  
 В SQLXML 4.0 данный запрос XPath должен указываться с префиксом пространства имен. Например, **КС:контакт**, где **x** — префикс пространства имен. Рассмотрим следующую схему XSD.  
  
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
  
 Поскольку эта схема определяет целевое пространство имен, запрос XPath (например «Employee») к схеме должен включать пространство имен.  
  
 Это образец приложения [!INCLUDE[msCoName](../../../includes/msconame-md.md)] Visual Basic, которое выполняет запрос XPath (x:Сотрудник) к предыдущей схеме XSD. Для разрешения префикса привязка пространства имен задается с помощью свойства Namespaces.  
  
> [!NOTE]  
>  В коде необходимо задать имя экземпляра [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] в строке соединения. Кроме того, в данном примере задается использование собственного клиента [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] (SQLNCLI11) для поставщика данных, для которого необходимо установить дополнительное клиентское сетевое ПО. Дополнительные сведения см. в разделе [требования к системе для SQL Server Native Client](../../../relational-databases/native-client/system-requirements-for-sql-server-native-client.md).  
  
```  
Option Explicit  
Private Sub Form_Load()  
    Dim con As New ADODB.Connection  
    Dim cmd As New ADODB.Command  
    Dim stm As New ADODB.Stream  
    con.Open "provider=SQLXMLOLEDB.4.0;Data Provider=SQLNCLI11;Data Source=SqlServerName;Initial Catalog=AdventureWorks;Integrated Security=SSPI;"  
    Set cmd.ActiveConnection = con  
    stm.Open  
    cmd.Properties("Output Stream").Value = stm  
    cmd.Properties("Output Encoding") = "utf-8"  
    cmd.Properties("Mapping schema") = "C:\DirectoryPath\con-ex.xml"  
    cmd.Properties("namespaces") = "xmlns:x='urn:myschema:Contacts'"  
    '  Debug.Print "Set Command Dialect to DBGUID_XPATH"  
    cmd.Dialect = "{ec2a4293-e898-11d2-b1b7-00c04f680c56}"  
    cmd.CommandText = "x:Contact"  
    cmd.Execute , , adExecuteStream   
    stm.Position = 0  
    Debug.Print stm.ReadText(adReadAll)  
End Sub  
```  
  
### <a name="to-test-this-application"></a>Проверка данного приложения  
  
1.  Сохраните в папке образец схемы XSD.  
  
2.  Создайте исполняемый проект Visual Basic и скопируйте в него код. При необходимости измените путь к указанному каталогу.  
  
3.  Добавьте следующую ссылку на проект:  
  
    ```  
    "Microsoft ActiveX Data Objects 2.8 Library"  
    ```  
  
4.  Запустите приложение.  

 Частичный результат:  
  
```  
<y0:Employee xmlns:y0="urn:myschema:Contacts"   
             LName="Achong" CID="1" FName="Gustavo"/>  
<y0:Employee xmlns:y0="urn:myschema:Employees"   
             LName="Abel" CID="2" FName="Catherine"/>  
```  
  
 Префиксы, сформированные в XML-документе, произвольны, но соответствуют этому же пространству имен.  
  
  
