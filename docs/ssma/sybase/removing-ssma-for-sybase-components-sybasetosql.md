---
description: Удаление компонентов SSMA для Sybase (SybaseToSQL)
title: Удаление SSMA для компонентов Sybase (SybaseToSQL) | Документация Майкрософт
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: ssma
ms.topic: conceptual
ms.assetid: aec09593-17d9-4ec2-ac56-3cd8851406fd
author: nahk-ivanov
ms.author: alexiva
ms.openlocfilehash: 02a782e583463e5a986e3c7c1a42152fc27b14cd
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/10/2021
ms.locfileid: "100069949"
---
# <a name="removing-ssma-for-sybase-components-sybasetosql"></a>Удаление компонентов SSMA для Sybase (SybaseToSQL)
Когда вы завершите миграцию баз данных из Sybase адаптивного сервера предприятия (ASE) в [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , может потребоваться удалить компоненты SSMA. Вы можете удалить клиентские компоненты в любое время, но не следует удалять пакет расширений, если не [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] уверены, что перенесенные базы данных больше не используют функции в схеме **ssma_syb** базы данных **сисдб** .  
  
## <a name="uninstalling-the-ssma-for-sybase-client"></a>Удаление SSMA для клиента Sybase  
Удалить SSMA можно с помощью компонента " **Установка и удаление программ**".  
  
**Удаление SSMA**  
  
1.  На панели управления и откройте элемент **Установка и удаление программ**.  
  
2.  Выберите **Помощник по миграции Microsoft SQL Server для Sybase**, а затем нажмите кнопку **Удалить**.  
  
3.  Чтобы подтвердить удаление SSMA, нажмите кнопку **Да**.  
  
## <a name="uninstalling-the-extension-pack"></a>Удаление пакета расширений  
Если вы уверены, что перенесенные базы данных не используют объекты в схеме **sysdb.ssma_syb** , можно удалить пакет расширений с помощью компонента " **Установка и удаление программ**".  
  
Удаление пакета расширений  
  
1.  На панели управления и откройте элемент **Установка и удаление программ**.  
  
2.  Выберите **Помощник по миграции Microsoft SQL Server для пакета расширения Sybase** и нажмите кнопку **Удалить**.  
  
3.  Чтобы подтвердить удаление пакета расширений, нажмите кнопку **Да**.  
  
4.  На странице экземпляры с скриптами базы данных служебной программы нажмите кнопку **Далее**.  
  
5.  На странице Параметры соединения выберите метод проверки подлинности и нажмите кнопку **Далее**.  
  
    При проверке подлинности Windows будут использоваться учетные данные Windows для входа на экземпляр SQL Server. При выборе SQL Server проверка подлинности необходимо ввести SQL Server имя входа и пароль.  
  
6.  На странице операция завершена нажмите кнопку **ОК**.  
  
7.  На странице Готово нажмите кнопку **выход**.  
  
После удаления можно убедиться, что **sysdb.ssma_sybная** схема и, возможно, вся база данных **сисдб** , была удалена с помощью [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] . Однако при использовании других продуктов SSMA они также используют базу данных **сисдб** . Если база данных существует и вы уверены, что другие базы данных не ссылаются на объекты в этой базе данных, можно отключить базу данных.  
  
## <a name="see-also"></a>См. также:  
[Установка SSMA для клиента Sybase &#40;SybaseToSQL&#41;](../../ssma/sybase/installing-ssma-for-sybase-client-sybasetosql.md)  
[Установка компонентов SSMA на SQL Server &#40;SybaseToSQL&#41;](../../ssma/sybase/installing-ssma-components-on-sql-server-sybasetosql.md)  
  
