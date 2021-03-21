---
title: MSSQLSERVER_50000 | Документация Майкрософт
description: Произошла ошибка при попытке установить или обновить SQL Server Native Client. См. объяснение ошибки и возможные способы ее устранения.
ms.custom: ''
ms.date: 04/04/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics, pdw
ms.reviewer: ''
ms.technology: supportability
ms.topic: reference
helpviewer_keywords:
- 50000 [SQL Server Native Client setup error]
ms.assetid: 5426d87a-d5d9-4984-b211-b07d69e834a2
author: MashaMSFT
ms.author: mathoma
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 32015c0727ec44bd802ddb5f7cf5eb4c35ab2ea8
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/20/2021
ms.locfileid: "104735033"
---
# <a name="error-mssqlserver_50000-in-sql-server-native-client"></a>Ошибка MSSQLSERVER_50000 в SQL Server Native Client 
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

    
## <a name="details"></a>Сведения  
  
| attribute | Значение |  
| :-------- | :---- |  
|Название продукта|SQL Server|  
|Версия продукта|11.0|  
|Идентификатор события|50 000|  
|Источник события|SETUP|  
|Компонент|Собственный клиент [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]|  
|Символическое имя||  
|Текст сообщения|Произошла сетевая ошибка при попытке чтения из файла «%.*ls».|  
  
## <a name="explanation"></a>Объяснение  
 Была сделана попытка установить (или обновить) собственный клиент [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] на компьютер, где собственный клиент [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] уже установлен и где существующая установка производилась из MSI-файла, который был переименован из sqlncli.msi.  
  
## <a name="user-action"></a>Действие пользователя  
 Для разрешения этой ошибки удалите существующую версию собственного клиента [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Чтобы предотвратить эту ошибку, не устанавливайте собственный клиент [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] из MSI-файла, который не называется sqlncli.msi.  
  
## <a name="internal-only"></a>Только для внутреннего использования  
  
