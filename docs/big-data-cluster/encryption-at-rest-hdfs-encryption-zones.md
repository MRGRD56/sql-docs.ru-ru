---
title: Рекомендации по использованию зон шифрования HDFS в Кластерах больших данных SQL Server
titleSuffix: SQL Server big data clusters
description: В этой статье показано, как использовать функцию зон шифрования HDFS в BDC SQL Server.
author: DaniBunny
ms.author: dacoelho
ms.reviewer: mihaelab
ms.date: 10/19/2020
ms.topic: tutorial
ms.prod: sql
ms.technology: big-data-cluster
ms.openlocfilehash: 4c56d065de396a282c97c0723f118e5911617544
ms.sourcegitcommit: e8c0c04eb7009a50cbd3e649c9e1b4365e8994eb
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/14/2021
ms.locfileid: "100489298"
---
# <a name="sql-server-big-data-clusters-hdfs-encryption-zones-usage-guide"></a>Рекомендации по использованию зон шифрования HDFS в Кластерах больших данных SQL Server

[!INCLUDE[SQL Server 2019](../includes/applies-to-version/sqlserver2019.md)]

В этом руководстве показано, как использовать функции шифрования неактивных данных Кластеров больших данных SQL Server для шифрования папок HDFS с помощью зон шифрования. В нем также рассматриваются задачи управления ключами HDFS.

Обратите внимание, что уже подключенная зона шифрования по умолчанию __```/securelake```__ готова к использованию. Она создана с помощью созданного системой 256-битного ключа с именем __securelakekey__. Этот ключ можно использовать для создания дополнительных зон шифрования.

## <a name="prerequisites"></a><a id="prereqs"></a> Предварительные требования

- [Кластер больших данных SQL Server CU8+](release-notes-big-data-cluster.md) с интеграцией [Active Directory](active-directory-prerequisites.md).
- Пользователь с правами администратора.
- [!INCLUDE[azdata](../includes/azure-data-cli-azdata.md)] настроенный и зарегистрированный в кластере в режиме AD.

## <a name="create-an-encryption-zone-using-the-provided-system-managed-key"></a>Создание зоны шифрования с помощью предоставленного системой управляемого ключа

1. Создание папки HDFS

   ```console
   azdata bdc hdfs mkdir -p /user/zone/folder
   ```

1. Выдайте команду на создание зоны шифрования, чтобы зашифровать папку с помощью ключа __securelakekey__.

   ```console
   azdata bdc hdfs encryption-zone create --path /user/zone/folder --keyname securelakekey
   ```

## <a name="create-a-custom-new-key-and-encryption-zone"></a>Создание нового ключа пользователя и зоны шифрования

1. Используйте следующий шаблон для создания 256-битного ключа.

   ```console
   azdata bdc hdfs key create -n mydatalakekey
   ```

1. Создайте и зашифруйте новый путь HDFS с помощью ключа пользователя.

   ```console
   azdata bdc hdfs encryption-zone create --path /user/mydatalake --keyname mydatalakekey
   ```

## <a name="hdfs-key-rotation-and-encryption-zone-re-encryption"></a>Смена ключей HDFS и повторное шифрование зоны шифрования

1. Создайте новую версию __securelakekey__ с новым материалом ключа.

   ```console
   azdata hdfs bdc key roll -n securelakekey
   ```

1. Повторно зашифруйте зону шифрования, связанную с указанным выше ключом.

   ```console
   azdata bdc hdfs encryption-zone reencrypt --path /securelake --action start
   ```

## <a name="hdfs-key-and-encryption-zone-monitoring"></a>Мониторинг ключей HDFS и зоны шифрования

1. Для наблюдения за состоянием повторного шифрования зоны шифрования используйте следующую команду. 

   ```console
   azdata bdc hdfs encryption-zone status
   ```

1. Для получения сведений о шифровании файла в зоне шифрования используйте следующую команду.

   ```console
   azdata bdc hdfs encryption-zone get-file-encryption-info --path /securelake/data.csv
   ```

1. Для вывода списка всех зон шифрования используйте следующую команду.

   ```console
   azdata bdc hdfs encryption-zone list
   ```

1. Для вывода списка всех доступных ключей HDFS используйте следующую команду.

   ```console
   azdata bdc hdfs key list
   ```

1. Чтобы создать пользовательский ключ для шифрования HDFS, используйте следующую команду. Возможны размеры: 128, 192, 256. Размер по умолчанию — 256.

   ```console
   azdata hdfs key create --name key1 --size 256
   ```

## <a name="next-steps"></a>Дальнейшие действия

Используйте azdata с Кластерами больших данных: [Что такое [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ver15.md)]?](big-data-cluster-overview.md).

Используйте azdata со [службами данных с поддержкой Azure Arc](/azure/azure-arc/data/)
