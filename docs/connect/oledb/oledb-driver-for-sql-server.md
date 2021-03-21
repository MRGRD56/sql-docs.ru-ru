---
title: Драйвер Microsoft OLE DB для SQL Server
description: Microsoft OLE DB Driver for SQL Server обеспечивает подключение к SQL Server и Базе данных SQL Azure через стандартные API OLE DB.
ms.custom: ''
ms.date: 05/06/2020
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics, pdw
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
helpviewer_keywords:
- MSOLEDBSQL, about OLE DB Driver for SQL Server
- OLE DB Driver for SQL Server, about OLE DB Driver for SQL Server
- data access [OLE DB Driver for SQL Server], about OLE DB Driver for SQL Server
- data access [OLE DB Driver for SQL Server]
- OLE DB Driver for SQL Server
- MSOLEDBSQL
- native data access [OLE DB Driver for SQL Server]
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 5ed2642e679cb9822f98285c7dd13f1bdfeda6ef
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/20/2021
ms.locfileid: "104748305"
---
# <a name="microsoft-ole-db-driver-for-sql-server"></a>Драйвер Microsoft OLE DB для SQL Server
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

[!INCLUDE[Driver_OLEDB_Download](../../includes/driver_oledb_download.md)]

Драйвер OLE DB для SQL Server — это изолированный прикладной программный интерфейс (API) для доступа к данным, используемый в OLE DB, который появился в [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)]. Драйвер OLE DB для SQL Server предоставляется в формате одной библиотеки динамической компоновки (DLL). Также он предоставляет новые расширенные функциональные возможности, поставляемые компонентами доступа к данным Windows (выделенное административное соединение Windows, ранее — компоненты доступа к данным компонентов MDAC). Драйвер OLE DB для SQL Server может применяться для создания новых или усовершенствования существующих приложений, которым требуется доступ к новым функциям [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)], таким как множественный активный результирующий набор (MARS), пользовательские типы, уведомления о запросах, изоляция моментальных снимков и поддержка типа данных XML.  
  
> [!NOTE]  
> Список различий между драйверами OLE DB для SQL Server и Windows DAC, а также сведения о проблемах, которые следует учесть перед переносом приложения Windows DAC на драйвер OLE DB для SQL Server, можно найти в статье [Обновление приложения с переходом от MDAC на драйвер OLE DB для SQL Server](../oledb/applications/updating-an-application-to-oledb-driver-for-sql-server-from-mdac.md).  

> [!IMPORTANT]
> Предыдущие поставщики [Microsoft OLE DB для SQL Server](../../ado/guide/appendixes/microsoft-ole-db-provider-for-sql-server.md) (SQLOLEDB) и [собственный клиент OLE DB для SQL Server](../../relational-databases/native-client/sql-server-native-client.md) (SQLNCLI) объявляются нерекомендуемыми для новых разработок.
  
 Драйвер OLE DB для SQL Server может использоваться совместно с основными службами OLE DB, поставляемыми с компонентами доступа к данным Windows DAC, но это не является обязательным требованием. Выбор того, использовать или нет основные службы, зависит от требований отдельного приложения (например, требуется ли пул соединений).  
  
 Приложения ADO могут использовать драйвер OLE DB для SQL Server, но мы рекомендуем использовать для ADO ключевое слово строки подключения **DataTypeCompatibility** (или соответствующее свойство **DataSource**). При использовании драйвера OLE DB для SQL Server приложения ADO могут использовать новые функции, добавленные в [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)] и доступные в драйвере OLE DB для SQL Server через ключевые слова строки подключения, свойства OLE DB или [!INCLUDE[tsql](../../includes/tsql-md.md)]. Дополнительные сведения об использовании этих функций с ADO см. в статье [Использование объектов ADO с драйвером OLE DB для SQL Server](../oledb/applications/using-ado-with-oledb-driver-for-sql-server.md).  
  
 Драйвер OLE DB для SQL Server призван обеспечить упрощенный собственный доступ к данным SQL Server через OLE DB. Он позволяет разрабатывать и развивать новые функции доступа к данным без изменения текущих компонентов выделенного административного соединения Windows, которые теперь являются частью платформы Microsoft Windows.  
  
 OLE DB Driver for SQL Server использует компоненты доступа к данным Windows DAC, но явно не зависит от их конкретных версий. Драйвер OLE DB для SQL Server можно использовать с любой версией выделенного административного соединения Windows, которая устанавливается операционной системой, поддерживаемой драйвером OLE DB для SQL Server.  

 ## <a name="different-generations-of-ole-db-drivers"></a>Разные поколения драйверов OLE DB

Существует три поколения поставщиков Microsoft OLE DB для SQL Server.

### <a name="1-microsoft-ole-db-provider-for-sql-server-sqloledb"></a>1. Поставщик Microsoft OLE DB для SQL Server (SQLOLEDB)
[Поставщик Microsoft OLE DB для SQL Server](../../ado/guide/appendixes/microsoft-ole-db-provider-for-sql-server.md) (SQLOLEDB) по-прежнему поставляется в составе [компонентов доступа к данным Windows](/previous-versions/windows/desktop/ms692897(v=vs.85)). Он больше не поддерживается и мы не рекомендуем использовать этот драйвер для разработки новых приложений.

### <a name="2-sql-server-native-client-snac"></a>2. SQL Server Native Client (SNAC)
Начиная с версии [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)], [собственный клиент SQL Server (SNAC)](../../relational-databases/native-client/sql-server-native-client.md) включает интерфейс поставщика OLE DB (SQLNCLI) и поставщик OLE DB, который поставляется с [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)] через [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)].

[С 2011 года он считается устаревшими](/archive/blogs/sqlnativeclient/microsoft-is-aligning-with-odbc-for-native-relational-data-access) и мы не рекомендуем использовать этот драйвер для разработки новых приложений. Дополнительные сведения о жизненном цикле SNAC и доступных для скачивания файлах см. в [описании жизненного цикла SNAC](/archive/blogs/sqlreleaseservices/snac-lifecycle-explained).

### <a name="3-microsoft-ole-db-driver-for-sql-server-msoledbsql"></a>3. Драйвер Microsoft OLE DB для SQL Server (MSOLEDBSQL)
Поддержка OLE DB [возобновлена](/archive/blogs/sqlnativeclient/announcing-the-new-release-of-ole-db-driver-for-sql-server) с момента выпуска поставщика в 2018 г.

Новый поставщик OLE DB называется драйвером Microsoft OLE DB для SQL Server (MSOLEDBSQL). Этот новый поставщик будет далее дополняться всеми новыми функциями сервера.

> [!NOTE]
> Чтобы использовать новый драйвер Майкрософт OLE DB для SQL Server в существующих приложениях, следует запланировать преобразование строк подключения из форматов SQLOLEDB и SQLNCLI в MSOLEDBSQL.
  
## <a name="in-this-section"></a>В этом разделе  
[Когда использовать драйвер OLE DB для SQL Server](../oledb/when-to-use-oledb-driver-for-sql-server.md)  
 Обсуждается место драйвера OLE DB для SQL Server среди технологий доступа к данным корпорации Майкрософт, дается его сравнение с компонентами доступа к данным Windows DAC и ADO.NET, а также предоставляются указания, помогающие решить, какую технологию доступа к данным следует использовать.  
  
 [OLE DB Driver for SQL Server features](../oledb/features/oledb-driver-for-sql-server-features.md ) (Функции драйвера OLE DB для SQL Server)  
 Здесь описаны возможности, которые поддерживаются драйвером OLE DB для SQL Server.  
  
 [Создание приложений с помощью драйвера OLE DB для SQL Server](../oledb/applications/building-applications-with-oledb-driver-for-sql-server.md)  
 Представлены общие сведения о разработке с помощью драйвера OLE DB для SQL Server, включая отличия от компонентов доступа к данным Windows DAC, используемые компоненты и способ использования совместно с ADO.  
  
 В этом разделе также обсуждается установка и развертывание драйвера OLE DB для SQL Server, в том числе способ распространения библиотеки драйвера OLE DB для SQL Server.  
  
 [Требования к системе для драйвера OLE DB для SQL Server](../oledb/system-requirements-for-oledb-driver-for-sql-server.md)  
 Здесь описаны системные ресурсы, которые нужны для использования драйвера OLE DB для SQL Server.  
  
 [OLE DB Driver for SQL Server programming](../oledb/ole-db/oledb-driver-for-sql-server-programming.md) (Программирование драйвера OLE DB для SQL Server)  
 Предоставляются сведения об использовании драйвера OLE DB для SQL Server.  
  
 [Finding more OLE DB Driver for SQL Server information](../oledb/finding-more-oledb-driver-for-sql-server-information.md) (Дополнительные сведения о драйвере OLE DB для SQL Server)  
 Предоставляются дополнительные ресурсы со сведениями о драйвере OLE DB для SQL Server, в том числе ссылки на внешние ресурсы и дополнительные сведения.  
  
  
## <a name="see-also"></a>См. также раздел  
 [Updating an application from SQL Server 2005 Native Client](../oledb/applications/updating-an-application-from-sql-server-2005-native-client.md)   (Обновление приложения с переходом от SQL Server 2005 Native Client)  
 [Инструкции по OLE DB](../oledb/ole-db-how-to/ole-db-how-to-topics.md)  
  
  
