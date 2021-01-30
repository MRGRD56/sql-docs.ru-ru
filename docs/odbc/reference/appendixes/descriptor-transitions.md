---
description: Переходы дескрипторов
title: Переходы дескрипторов | Документация Майкрософт
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
helpviewer_keywords:
- state transitions [ODBC], descriptor
- transitioning states [ODBC], descriptor
- descriptor transitions [ODBC]
ms.assetid: 0cf24fe6-5e3c-45fa-81b8-4f52ddf8501d
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: d312b372a0817ae79ad07bdbf149f90ba0c71939
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/30/2021
ms.locfileid: "99194886"
---
# <a name="descriptor-transitions"></a>Переходы дескрипторов
Дескрипторы ODBC имеют следующие три состояния.  
  
|Состояние|Описание|  
|-----------|-----------------|  
|Состояния|Нераспределенный дескриптор|  
|D1i|Неявно выделенный дескриптор|  
|D1e|Явно выделенный дескриптор|  
  
 В следующих таблицах показано, как каждая функция ODBC влияет на состояние дескриптора.  
  
## <a name="sqlallochandle"></a>Функцию SQLAllocHandle  
  
|Состояния<br /><br /> Не выделено|D1i<br /><br /> Неявная|D1e<br /><br /> Явная|  
|------------------------|----------------------|----------------------|  
|D1i [1]|--|--|  
|D1e [2]|--|--|  
  
 [1] в этой строке отображаются переходы, когда *параметром handletype* был SQL_HANDLE_STMT.  
  
 [2] в этой строке отображаются переходы, когда *параметром handletype* был SQL_HANDLE_DESC.  
  
## <a name="sqlcopydesc"></a>склкопидеск  
  
|Состояния<br /><br /> Не выделено|D1i<br /><br /> Неявная|D1e<br /><br /> Явная|  
|------------------------|----------------------|----------------------|  
|IH|--|--|  
  
## <a name="sqlfreehandle"></a>SQLFreeHandle  
  
|Состояния<br /><br /> Не выделено|D1i<br /><br /> Неявная|D1e<br /><br /> Явная|  
|------------------------|----------------------|----------------------|  
|--[1]|Состояния|--|  
|IH открыт|(HY017)|Состояния|  
  
 [1] в этой строке отображаются переходы, когда *параметром handletype* был SQL_HANDLE_STMT.  
  
 [2] в этой строке отображаются переходы, когда *параметром handletype* был SQL_HANDLE_DESC.  
  
## <a name="sqlgetdescfield-and-sqlgetdescrec"></a>SQLGetDescField и SQLGetDescRec  
  
|Состояния<br /><br /> Не выделено|D1i<br /><br /> Неявная|D1e<br /><br /> Явная|  
|------------------------|----------------------|----------------------|  
|IH|--|--|  
  
## <a name="sqlsetdescfield-and-sqlsetdescrec"></a>SQLSetDescField и SQLSetDescRec  
  
|Состояния<br /><br /> Не выделено|D1i<br /><br /> Неявная|D1e<br /><br /> Явная|  
|------------------------|----------------------|----------------------|  
|IH одного|--|--|  
  
 [1] в этой строке отображаются переходы, если *дескрипторхандле* является маркером АРД, APD или IPD или (для **SQLSetDescField**), когда *дескрипторхандле* был создан маркер IRD, а *фиелдидентифиер* — SQL_DESC_ARRAY_STATUS_PTR или SQL_DESC_ROWS_PROCESSED_PTR.  
  
## <a name="all-other-odbc-functions"></a>Все остальные функции ODBC  
  
|Состояния<br /><br /> Не выделено|D1i<br /><br /> Неявная|D1e<br /><br /> Явная|  
|------------------------|----------------------|----------------------|  
|--|--|--|
