---
title: Подключение к главному кластеру больших данных и кластеру больших данных HDFS
description: Сведения о том, как подключиться к главному экземпляру SQL Server и шлюзу HDFS/Spark для кластера больших данных SQL Server.
author: MikeRayMSFT
ms.author: mikeray
ms.reviewer: mihaelab
ms.date: 11/04/2019
ms.topic: conceptual
ms.prod: sql
ms.technology: big-data-cluster
ms.openlocfilehash: b7f05551da025f513cd69ddf592002126c6f05c3
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/10/2021
ms.locfileid: "100039071"
---
# <a name="connect-to-a-sql-server-big-data-cluster-with-azure-data-studio"></a>Подключение к кластеру больших данных SQL Server с помощью Azure Data Studio

[!INCLUDE[SQL Server 2019](../includes/applies-to-version/sqlserver2019.md)]

В этой статье описывается, как подключиться к [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ver15.md)] из Azure Data Studio.

## <a name="prerequisites"></a>Предварительные требования

- Развернутый [кластер больших данных SQL Server 2019](deployment-guidance.md).
- [Средства для работы с большими данными SQL Server 2019](deploy-big-data-tools.md)
   - **Azure Data Studio**
   - **Расширение SQL Server 2019**
   - **kubectl**
   - **azdata**

## <a name="connect-to-the-cluster"></a><a id="master"></a> Подключение к кластеру

Чтобы подключиться к кластеру больших данных с помощью Azure Data Studio, установите новое подключение к главному экземпляру SQL Server в кластере. Это делается так.

1. Найдите конечную точку главного экземпляра SQL Server:

   ```
   azdata bdc endpoint list -e sql-server-master
   ```

   > [!TIP]
   > Дополнительные сведения об извлечении конечных точек см. в разделе [Извлечение конечных точек](deployment-guidance.md#endpoints).

1. В Azure Data Studio нажмите **F1** > **Создать подключение**.

1. В разделе **Тип подключения** выберите **Microsoft SQL Server**.

1. Введите название конечной точки, которую вы нашли для главного экземпляра SQL Server, в текстовом поле **Имя сервера** (например: **\<IP_Address\>,31433**). 

1. Выберите тип проверки подлинности. Для основного экземпляра SQL Server, выполняемого в кластере больших данных, поддерживаются только **проверка подлинности Windows** и **вход с учетными данными SQL**. 

1. Если вы используете вход с учетными данными SQL, введите **Имя пользователя** и **Пароль** из этих данных.

   > [!TIP]
   > По умолчанию имя пользователя **SA** отключено во время развертывания кластера больших данных. Новый пользователь sysadmin подготавливается во время развертывания; ему присваивается имя и пароль, соответствующие переменным среды **AZDATA_USERNAME** и **AZDATA_PASSWORD**, которые были заданы до или во время развертывания.

1. В поле **Имя базы данных** укажите в качестве целевой одну из ваших реляционных баз данных.

   ![Подключение к главному экземпляру](./media/connect-to-big-data-cluster/connect-to-cluster.png)

1. Нажмите **Подключить**. Откроется **панель мониторинга сервера**.

В выпуске Azure Data Studio, который вышел в феврале 2019 года, после установления подключения к главному экземпляру SQL Server вы также сможете работать с шлюзом HDFS/Spark. Это значит, что вам не потребуется отдельное подключение для HDFS и Spark, которое описывается в следующем разделе.

- В обозревателе объектов появится новый узел **Службы данных**. В вызываемом с помощью правой кнопки мыши меню этого узла будут представлены задачи для работы с кластером больших данных, например для создания новых записных книжек или отправки заданий Spark. 
- Узел **Службы данных** также содержит папку **HDFS**, что позволяет изучать содержимое HDFS и выполнять стандартные задачи, затрагивающие HDFS (например, создание внешней таблицы или открытие записной книжки для анализа содержимого HDFS).
- Если установлено расширение, **панель мониторинга сервера** для подключения также содержит вкладки **Кластер больших данных SQL Server** и **SQL Server 2019**.

   ![Узел служб данных Azure Data Studio](./media/connect-to-big-data-cluster/connect-data-services-node.png)

## <a name="next-steps"></a>Дальнейшие действия

Дополнительные сведения о [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ver15.md)] см. в статье [Что такое [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ver15.md)]](big-data-cluster-overview.md).
