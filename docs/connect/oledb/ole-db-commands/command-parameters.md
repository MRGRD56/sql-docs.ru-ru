---
title: Параметры команд (драйвер OLE DB)| Документация Майкрософт
description: Сведения о параметрах команды, в том числе о типах, которые OLE DB Driver for SQL Server поддерживает для инструкции SQL и команды procedure-call.
ms.custom: ''
ms.date: 06/14/2018
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics, pdw
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
helpviewer_keywords:
- parameters [OLE DB Driver for SQL Server]
- OLE DB Driver for SQL Server, parameters
- OLE DB Driver for SQL Server, commands
- parameters [OLE DB Driver for SQL Server], OLE DB
- commands [OLE DB]
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: a36e123a61cc31e80eb667aab51082870f7970de
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/20/2021
ms.locfileid: "104737844"
---
# <a name="command-parameters"></a>Параметры команд
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

[!INCLUDE[Driver_OLEDB_Download](../../../includes/driver_oledb_download.md)]

  Параметры помечаются в тексте команды знаками вопроса. Например, следующая инструкция SQL помечена для одного входного параметра:  
  
```  
{call SalesByCategory('Produce', ?)}  
```  
  
 Для повышения производительности путем уменьшения сетевого трафика драйвер OLE DB для SQL Server наследует сведения о параметрах автоматически, только если перед выполнением команды вызывается метод **ICommandWithParameters::GetParameterInfo** или **ICommandPrepare::Prepare**. Это означает, что OLE DB Driver for SQL Server не выполняет следующие действия автоматически:  
  
-   не проверяет правильность типа данных, указанного с помощью **ICommandWithParameters::SetParameterInfo**;  
  
-   не сопоставляет тип DBTYPE, указанный в данных привязки метода доступа, с правильным типом данных [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] для параметра.  
  
 Приложения будут получать возможные ошибки или потери точности одним из этих методов, если они указывают типы данных, несовместимые с типом данных [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] для параметра.  
  
 Чтобы гарантировать, что подобная ситуация не произойдет, для приложения необходимо:  
  
-   Проверить, совпадает ли тип данных *pwszDataSourceType* с типом данных [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] для параметра при жестком задании метода **ICommandWithParameters::SetParameterInfo**.  
  
-   Проверить, что тип значения DBTYPE, связанного с параметром, совпадает с типом данных [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] для жестко запрограммированного метода доступа.  
  
-   Настроить приложение для вызова метода **ICommandWithParameters::GetParameterInfo** так, чтобы поставщик мог автоматически получать типы данных [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] для параметров. Обратите внимание, что это приведет к дополнительному обмену данными с сервером.  
  
> [!NOTE]  
>  Поставщик не поддерживает вызов метода **ICommandWithParameters::GetParameterInfo** для любой инструкции [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] UPDATE или DELETE, содержащей предложение FROM; для любой инструкции SQL, которая зависит от вложенного запроса, содержащего параметры; для инструкций SQL, содержащих маркеры параметров в обоих сравниваемых выражениях или предикат с квантором; или запросов, в которых один из параметров является параметром функции. При обработке пакета инструкций SQL поставщик также не поддерживает вызов метода **ICommandWithParameters::GetParameterInfo** для маркеров параметров в выражениях после обработки первой инструкции в пакете. В команде [!INCLUDE[tsql](../../../includes/tsql-md.md)] не допускаются комментарии (/* \*/).  
  
 OLE DB Driver for SQL Server поддерживает входные параметры в командах инструкций SQL. OLE DB Driver for SQL Server поддерживает для команд вызова процедур входные, выходные и входные-выходные параметры. Значения выходных параметров возвращаются приложению либо при выполнении (только если не возвращаются наборы строк), либо когда все возвращаемые наборы строк уже использованы приложением. Чтобы проверить допустимость возвращаемых значений, используется **IMultipleResults** для принудительного потребления набора строк.  
  
 Имена параметров хранимых процедур не обязательно указывать в структуре DBPARAMBINDINFO. Чтобы показать, что драйвер OLE DB для SQL Server не должен пропускать имя параметра и должен использовать только порядковый номер, указанный в элементе *rgParamOrdinals* метода **ICommandWithParameters::SetParameterInfo**, используется значение NULL элемента *pwszName*. Если текст команды содержит как именованные, так и неименованные параметры, все неименованные параметры должны быть указаны до именованных параметров.  
  
 Если указано имя параметра хранимой процедуры, драйвер OLE DB для SQL Server проверяет, является ли имя допустимым. OLE DB Driver for SQL Server возвращает ошибку при получении от потребителя неверного имени параметра.  
  
> [!NOTE]  
>  Для предоставления поддержки XML [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] и пользовательских типов драйвер OLE DB для SQL Server реализует новый интерфейс [ISSCommandWithParameters](../../oledb/ole-db-interfaces/isscommandwithparameters-ole-db.md).  
  
## <a name="see-also"></a>См. также:  
 [Команды](../../oledb/ole-db-commands/commands.md)  
  
  
