---
title: Переименование экземпляра отказоустойчивого кластера
description: В этой статье описывается, как переименовать экземпляр SQL Server, являющийся частью отказоустойчивого кластера. Эта операция отличается от переименования изолированного экземпляра.
ms.custom: seo-lt-2019
ms.date: 12/13/2019
ms.prod: sql
ms.reviewer: ''
ms.technology: failover-cluster-instance
ms.topic: how-to
helpviewer_keywords:
- clusters [SQL Server], virtual servers
- renaming virtual servers
- virtual servers [SQL Server], failover clustering
- failover clustering [SQL Server], virtual servers
ms.assetid: 2a49d417-25fb-4760-8ae5-5871bfb1e6f3
author: cawrites
ms.author: chadam
ms.openlocfilehash: 42fd6dbb5053993d5ad6c2859460e164f476bac1
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/10/2021
ms.locfileid: "100354019"
---
# <a name="rename-a-sql-server-failover-cluster-instance"></a>переименовать экземпляр отказоустойчивого кластера SQL Server
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sqlserver.md)]
  Если экземпляр [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] является частью отказоустойчивого кластера, процесс переименования виртуального сервера отличается от процесса переименования изолированного экземпляра. Дополнительные сведения см. в статье [Переименование компьютера, на который установлен изолированный экземпляр SQL Server](../../../database-engine/install-windows/rename-a-computer-that-hosts-a-stand-alone-instance-of-sql-server.md).  
  
 Имя виртуального сервера всегда совпадает с сетевым именем SQL-сервера (сетевое имя виртуального SQL-сервера). Несмотря на то, что имя виртуального сервера можно изменить, изменить имя экземпляра нельзя. Например, можно изменить имя виртуального сервера «VS1\экземпляр1» на другое имя, например «SQL35\экземпляр1», но часть имени, содержащая имя экземпляра (т. е. экземпляр1) останется неизменной.  
  
 Прежде чем приступить к процессу переименования, обратите внимание на следующие пункты.  
  
-   [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] не поддерживает переименование серверов, участвующих в репликации, за исключением случаев использования доставки журналов с репликацией. Сервер-получатель в доставке журналов может быть переименован, если сервер-источник окончательно потерян. Дополнительные сведения см. в статье [Репликация и доставка журналов (SQL Server)](../../../database-engine/log-shipping/log-shipping-and-replication-sql-server.md).  
  
-   При переименовании виртуального сервера, настроенного для использования зеркального отображения базы данных, прежде чем приступить к переименованию, необходимо отключить зеркальное отображение базы данных, а затем заново установить зеркальное отображение базы данных с новым именем виртуального сервера. Метаданные для зеркального отображения базы данных не будут обновлены автоматически для отражения нового имени виртуального сервера.  
  
### <a name="to-rename-a-virtual-server"></a>Переименование виртуального сервера  
  
1.  С помощью программы «Администратор кластера» измените сетевое имя SQL Server.  
  
2.  Переведите ресурс с этим сетевым именем в режим «вне сети». При этом ресурсы [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] и другие зависимые ресурсы переводятся в режим «вне сети».  
  
3.  Переведите ресурсы [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] обратно в режим «в сети».  
  
## <a name="verify-the-renaming-operation"></a>Проверка операции переименования  
 После того как виртуальный сервер переименован, любые соединения, использовавшие старое имя, должны осуществляться с использованием нового имени.  
  
 Чтобы убедиться в том, что операция переименования завершена, выберите необходимые данные из **@@servername** или **sys.servers**. Функция **@@servername** возвращает новое имя виртуального сервера, а в таблице **sys.servers** отображается новое имя виртуального сервера. Чтобы проверить правильность отработки отказа с новым именем, попытайтесь создать сбойную ситуацию на ресурсе [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] через другие узлы.  
  
 Для установления соединения с любого узла, расположенного в том же кластере, новое имя можно использовать немедленно. Однако для соединений с другого компьютера, использующих новое имя, новое имя не может быть использовано для подключения к серверу до тех пор, пока оно не будет видимо с этого клиентского компьютера. До того момента, как новое имя будет распространено по всей сети, может пройти от нескольких секунд до 3-5 минут (в зависимости от конфигурации сети). Кроме того, может потребоваться дополнительное время на то, чтобы старое имя виртуального сервера перестало быть видимо в сети.  
  
 Чтобы уменьшить задержки сетевого распространения операции переименования виртуального сервера, выполните следующие шаги.  
  
#### <a name="to-minimize-network-propagation-delay"></a>Минимизация задержки сетевого распространения  
  
1.  Выполните следующие команды в командной строке на узле сервера:  
  
    ```  
    ipconfig /flushdns  
    ipconfig /registerdns  
    nbtstat -RR  
    ```  
  
## <a name="additional-considerations-after-the-renaming-operation"></a>Дополнительные меры после операции переименования  
 После изменения сетевого имени кластера отработки отказа необходимо выполнить проверку и следующие инструкции для включения всех сценариев в агенте [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] и службах [!INCLUDE[ssASnoversion](../../../includes/ssasnoversion-md.md)].  
  
 **Служба агента [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)].** Проверьте и выполните дополнительные действия для службы агента [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)].  
  
-   Исправьте параметры в реестре, если агент SQL Server настроен на пересылку событий. Дополнительные сведения см. в статье [Назначение сервера пересылки событий (среда SQL Server Management Studio)](../../../ssms/agent/designate-an-events-forwarding-server-sql-server-management-studio.md).  
  
-   Исправьте имена экземпляра главного сервера (MSX) и целевых серверов (TSX), если сетевое имя компьютеров / кластера было изменено. Дополнительные сведения см. в следующих разделах:  
  
    -   [Отключение нескольких целевых серверов от главного](../../../ssms/agent/defect-multiple-target-servers-from-a-master-server.md)  
  
    -   [Создание многосерверной среды](../../../ssms/agent/create-a-multiserver-environment.md)  
  
-   Измените конфигурацию доставки журналов, чтобы для резервного копирования и восстановления журналов использовалось обновленное имя сервера. Дополнительные сведения см. в следующих разделах:  
  
    -   [Настройка доставки журналов (SQL Server)](../../../database-engine/log-shipping/configure-log-shipping-sql-server.md)  
  
    -   [Удаление доставки журналов (SQL Server)](../../../database-engine/log-shipping/remove-log-shipping-sql-server.md)  
  
-   Обновите шаги заданий, зависящие от имени сервера. Дополнительные сведения см. в статье [Manage Job Steps](../../../ssms/agent/manage-job-steps.md).  
  
## <a name="see-also"></a>См. также:  
 [Переименование компьютера, на который установлен изолированный экземпляр SQL Server](../../../database-engine/install-windows/rename-a-computer-that-hosts-a-stand-alone-instance-of-sql-server.md)  
  
