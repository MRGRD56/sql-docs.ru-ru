---
title: Настройка масштабируемых групп PolyBase в Windows | Документация Майкрософт
description: Настройка масштабируемой группы PolyBase для создания кластера экземпляров SQL Server. Повышает производительность запросов для больших наборов данных из внешних источников.
ms.date: 04/23/2019
ms.prod: sql
ms.technology: polybase
ms.topic: tutorial
author: MikeRayMSFT
ms.author: mikeray
ms.reviewer: ''
monikerRange: '>= sql-server-2016'
ms.openlocfilehash: 99129e6b52be36b1eb729bfda6056ec1010e7198
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/10/2021
ms.locfileid: "100351866"
---
# <a name="configure-polybase-scale-out-groups-on-windows"></a>Настройка масштабируемых групп PolyBase в Windows

[!INCLUDE [SQL Server Windows Only - ASDBMI ](../../includes/applies-to-version/sql-windows-only-asdbmi.md)]

В этой статье описывается настройка [масштабируемой группы PolyBase](polybase-scale-out-groups.md) в Windows. Эта группа позволяет создавать кластеры экземпляров SQL Server для обработки больших наборов данных из внешних источников данных (например, Hadoop или хранилища BLOB-объектов Azure), используя возможности масштабирования. Это помогает повысить производительность запросов.

## <a name="prerequisites"></a>Предварительные требования
  
- Более одного компьютера, размещенного в одном домене.  
  
- Доменная учетная запись для запуска служб PolyBase.  
  
## <a name="process-overview"></a>Общие сведения о процессе

Ниже представлена обобщенная процедура создания масштабируемой группы PolyBase. В следующем разделе приводится более подробное пошаговое руководство.
  
1. Установите одну и ту же версию SQL Server с PolyBase на все доступные компьютеры (N).
  
2. Выберите один экземпляр SQL Server в качестве головного узла. 
  
3. Добавьте в группу остальные экземпляры SQL Server в качестве вычислительных узлов с помощью [sp_polybase_join_group](../../relational-databases/system-stored-procedures/polybase-stored-procedures-sp-polybase-join-group.md).

4. Для мониторинга узлов группы используйте инструкцию [sys.dm_exec_compute_nodes (Transact-SQL)](../../relational-databases/system-dynamic-management-views/sys-dm-exec-compute-nodes-transact-sql.md).

5. Необязательный параметр. Для удаления вычислительных узлов используйте инструкцию [sp_polybase_leave_group (Transact-SQL)](../../relational-databases/system-stored-procedures/polybase-stored-procedures-sp-polybase-leave-group.md).

## <a name="example-walk-through"></a>Пошаговое руководство с примерами

В этом руководстве мы настроим группу PolyBase, используя следующие ресурсы.  
  
1. Два компьютера в домене *PQTH4A* со следующими именами:  
  
   - PQTH4A-CMP01  
  
   - PQTH4A-CMP02  
  
2. Учетная запись домена: *PQTH4A\PolyBaseUse* r  

## <a name="install-sql-server-with-polybase-on-all-machines"></a>Установка SQL Server с PolyBase на всех компьютерах

1. Запустите Setup.exe.
  
2. На странице выбора компонентов выберите пункт **Служба запросов PolyBase для внешних данных**.
  
3. На странице "Конфигурация сервера" настройте ядро SQL Server PolyBase и службу перемещения данных SQL Server PolyBase для запуска под **доменной учетной записью** PQTH4A\PolyBaseUser.
  
4. На странице настройки PolyBase включите параметр **Использовать экземпляр SQL Server как часть масштабируемой группы PolyBase**. Это позволит открыть брандмауэр для входящих подключений к службе PolyBase. Если головной узел является именованным экземпляром, необходимо вручную добавить порт SQL Server в брандмауэр Windows на головном узле и запустить на этом узле обозреватель SQL.
  
5. После завершения настройки запустите **services.msc**. Убедитесь, что запущены SQL Server, ядро PolyBase и служба перемещения данных PolyBase.
  
   ![Службы PolyBase](../../relational-databases/polybase/media/polybase-services.png "Службы PolyBase")  
  
## <a name="select-one-sql-server-as-head-node"></a>Выбор одного из серверов SQL Server в качестве головного узла  
  
По завершении установки любой из компьютеров может выполнять роль головного узла группы PolyBase. В нашем примере мы выберем в качестве головного узла MSSQLSERVER на узле PQTH4A-CMP01.
  
## <a name="add-other-sql-server-instances-as-compute-nodes"></a>Добавление в группу остальных экземпляров SQL Server в качестве вычислительных узлов  
  
1. Подключитесь к SQL Server на узле PQTH4A-CMP02.
  
2. Запустите хранимую процедуру [sp_polybase_leave_group](../../relational-databases/system-stored-procedures/polybase-stored-procedures-sp-polybase-join-group.md).

   ```sql
   -- Enter head node details:
   -- head node machine name, head node dms control channel port, head node sql server name  
    EXEC sp_polybase_join_group 'PQTH4A-CMP01', 16450, 'MSSQLSERVER';
   ```  

3. Запустите файл services.msc на вычислительном узле (PQTH4A-CMP02).
  
4. Завершите работу ядра PolyBase и перезапустите службу перемещения данных PolyBase.

> [!NOTE] 
> Когда служба Polybase Engine перезапускается или останавливается на головном узле, службы перемещения данных (DMS) останавливаются, как только закрывается канал связи между DMS и службой Polybase Engine (хранилище данных). Если ядро хранилища данных перезапускается больше 2 раз, служба DMS переходит в период покоя на 90 минут и должна подождать 90 минут для следующей попытки автоматического запуска. В такой ситуации следует вручную запустить эту службу на всех узлах.

## <a name="optional-remove-a-compute-node"></a>Необязательное действие: удаление вычислительного узла  
  
1. Подключитесь к вычислительному узлу SQL Server (PQTH4A-CMP02).
  
2. Запустите хранимую процедуру sp_polybase_leave_group.
  
    ```sql  
    EXEC sp_polybase_leave_group;  
    ```  
  
3. Запустите файл services.msc на вычислительном узле, который хотите удалить (PQTH4A-CMP02).
  
4. Запустите ядро PolyBase. Перезапустите службу перемещения данных PolyBase.
  
5. Убедитесь, что узел удален, запустив на узле PQTH4A-CMP01 динамическое административное представление sys.dm_exec_compute_nodes. Теперь узел PQTH4A-CMP02 будет функционировать как автономный головной узел.  
  
## <a name="next-steps"></a>Дальнейшие действия  

Дополнительные сведения об устранении неполадок см. в статье [PolyBase troubleshooting with dynamic management views](/previous-versions/sql/sql-server-2016/mt146389(v=sql.130)).
  
Дополнительные сведения см. в статье [Руководство по PolyBase](../../relational-databases/polybase/polybase-guide.md).