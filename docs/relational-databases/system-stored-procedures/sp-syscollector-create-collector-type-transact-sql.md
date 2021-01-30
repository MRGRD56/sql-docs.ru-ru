---
description: sp_syscollector_create_collector_type (Transact-SQL)
title: sp_syscollector_create_collector_type (Transact-SQL) | Документация Майкрософт
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_syscollector_create_collector_type
- sp_syscollector_create_collector_type_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sp_syscollector_create_collector_type
- data collector [SQL Server], stored procedures
ms.assetid: 568e9119-b9b0-4284-9cef-3878c691de5f
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 5431d45716aa0c1c685f303c3ee4b5d3cec67a13
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/30/2021
ms.locfileid: "99200517"
---
# <a name="sp_syscollector_create_collector_type-transact-sql"></a>sp_syscollector_create_collector_type (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Создает тип сборщика данных. Тип сборщика — это логическая оболочка [!INCLUDE[ssIS](../../includes/ssis-md.md)] пакетов, которая предоставляет реальный механизм для сбора данных и их передачи в хранилище управляющих данных.  
  
 ![Значок ссылки на раздел](../../database-engine/configure-windows/media/topic-link.gif "Значок ссылки на раздел") [Синтаксические обозначения в Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Синтаксис  
  
```  
  
sp_syscollector_create_collector_type   
    [ [@collector_type_uid = ] 'collector_type_uid' OUTPUT ]  
    , [ @name = ] 'name'  
    , [ [ @parameter_schema = ] 'parameter_schema' ]  
    , [ [ @parameter_formatter = ] 'parameter_formatter' ]  
    , [ @collection_package_id = ] 'collection_package_id'  
    , [ @upload_package_id = ] 'upload_package_id'  
```  
  
## <a name="arguments"></a>Аргументы  
 [ @collector_type_uid =] "*collector_type_uid*"  
 Идентификатор GUID типа сборщика. *collector_type_uid* имеет тип **uniqueidentifier** , и, если он равен null, он будет автоматически создан и возвращен в качестве выходных данных.  
  
 [ @name =] "*имя*"  
 Имя типа сборщика. Аргумент *Name* имеет тип **sysname** и должен быть указан.  
  
 [ @parameter_schema =] "*parameter_schema*"  
 Схема XML для этого типа сборщика. *parameter_schema* имеет **Формат XML** и значение по умолчанию NULL.  
  
 [ @parameter_formatter =] "*parameter_formatter*"  
 Шаблон, применяемый для преобразования XML с целью его использования на странице свойств набора элементов сбора. *parameter_formatter* имеет **Формат XML** и значение по умолчанию NULL.  
  
 [ @collection_package_id =] *collection_package_id*  
 Локальный уникальный идентификатор, указывающий на пакет сбора [!INCLUDE[ssIS](../../includes/ssis-md.md)], используемый в данном наборе элементов сбора. *collection_package_id* является **уникуеидентифер** и является обязательным.  
  
 [ @upload_package_id =] *upload_package_id*  
 Локальный уникальный идентификатор, указывающий на пакет передачи [!INCLUDE[ssIS](../../includes/ssis-md.md)], используемый в данном наборе элементов сбора. *upload_package_id* имеет тип **uniqueidentifier** и является обязательным.  
  
## <a name="return-code-values"></a>Значения кода возврата  
 **0** (успешное завершение) или **1** (сбой)  
  
## <a name="permissions"></a>Разрешения  
 Для выполнения этой процедуры требуется членство в предопределенной роли базы данных dc_admin (с разрешением EXECUTE).  
  
## <a name="example"></a>Пример  
 Создает тип сборщика «Универсальный запрос T-SQL».  
  
```  
EXEC sp_syscollector_create_collector_type  
@collector_type_uid = '302E93D1-3424-4be7-AA8E-84813ECF2419',  
@name = 'Generic T-SQL Query Collector Type',  
@parameter_schema = '<?xml version="1.0" encoding="utf-8"?>  
  <xs:schema xmlns:xs="http://www.w3.org/2001/XMLSchema" targetNamespace="DataCollectorType">  
    <xs:element name="TSQLQueryCollector">  
      <xs:complexType>  
        <xs:sequence>  
          <xs:element name="Query" minOccurs="1" maxOccurs="unbounded">  
            <xs:complexType>  
              <xs:sequence>  
                <xs:element name="Value" type="xs:string" />  
                <xs:element name="OutputTable" type="xs:string" />  
              </xs:sequence>  
            </xs:complexType>  
          </xs:element>  
          <xs:element name="Databases" minOccurs="0" maxOccurs="1">  
            <xs:complexType>  
              <xs:sequence>  
                <xs:element name="Database" minOccurs="0" maxOccurs="unbounded" type="xs:string" />  
              </xs:sequence>  
              <xs:attribute name="UseSystemDatabases" type="xs:boolean" use="optional" />  
              <xs:attribute name="UseUserDatabases" type="xs:boolean" use="optional" />  
            </xs:complexType>  
          </xs:element>  
        </xs:sequence>  
      </xs:complexType>  
    </xs:element>  
  </xs:schema>',  
@collection_package_id = '292B1476-0F46-4490-A9B7-6DB724DE3C0B',  
@upload_package_id = '6EB73801-39CF-489C-B682-497350C939F0';  
GO  
```  
  
## <a name="see-also"></a>См. также:  
 [Системные хранимые процедуры (Transact-SQL)](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)   
 [Сбор данных](../../relational-databases/data-collection/data-collection.md)  
  
  
