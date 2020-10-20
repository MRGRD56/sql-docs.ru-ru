---
title: Подключение к каталогу SSIS (SSISDB) в Azure | Документы Майкрософт
description: Получите сведения, необходимые для подключения к каталогу SSIS (SSISDB), размещенному на сервере базы данных SQL Azure.
ms.date: 09/23/2018
ms.topic: conceptual
ms.prod: sql
ms.prod_service: integration-services
ms.custom: ''
ms.technology: integration-services
author: swinarko
ms.author: sawinark
ms.reviewer: maghan
ms.openlocfilehash: b73efafc87f456ef8728a3bf1b5e4eec0b954782
ms.sourcegitcommit: cfa04a73b26312bf18d8f6296891679166e2754d
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/19/2020
ms.locfileid: "92194126"
---
# <a name="connect-to-the-ssis-catalog-ssisdb-in-azure"></a>Подключение к каталогу SSIS (SSISDB) в Azure

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]



Получите сведения, необходимые для подключения к каталогу SSIS (SSISDB), размещенному на сервере базы данных SQL Azure. Для подключения вам потребуется следующее:
- Полное имя сервера
- Имя базы данных
- Данные для входа 

> [!IMPORTANT]
> Сейчас невозможно создать базу данных каталога SSISDB в базе данных SQL Azure независимо от создания среды выполнения интеграции Azure-SSI в Фабрике данных Azure. Среда Azure-SSIS IR — это среда выполнения, в которой выполняются пакеты SSIS в Azure. Пошаговые инструкции см. в статье [Развертывание и запуск пакета служб SSIS в Azure](/azure/data-factory/tutorial-create-azure-ssis-runtime-portal). 

## <a name="prerequisites"></a>Предварительные требования
Прежде чем начать, убедитесь в наличии SQL Server Management Studio (SSMS) версии 17.2 или более поздней. Если база данных каталога SSISDB размещается в Управляемом экземпляре SQL, убедитесь в наличии SSMS версии 17.6 или более поздней. Чтобы скачать последнюю версию SSMS, перейдите на страницу [скачивания SQL Server Management Studio (SSMS)](../../ssms/download-sql-server-management-studio-ssms.md).

## <a name="get-the-connection-info-from-the-azure-portal"></a>Получение сведений о подключении с портала Azure
1. Войдите на [портал Azure](https://portal.azure.com/).
2. На портале Azure выберите **Базы данных SQL** в левом меню и щелкните базу данных `SSISDB` на странице **Базы данных SQL**. 
3. На странице **Обзор** для базы данных `SSISDB` просмотрите полное имя сервера, как показано на следующем изображении. Наведите указатель мыши на имя сервера, чтобы отобразить параметр **Щелкните, чтобы скопировать**.

    ![Сведения о соединении с сервером](media/ssis-azure-connect-to-catalog-database/server-name.png) 

4. Если вы забыли учетные данные входа на сервер базы данных SQL, перейдите на страницу этого сервера. Там можно узнать имя администратора сервера и при необходимости сбросить пароль.

## <a name="connect-with-ssms"></a>Подключение к SSMS
1. Откройте среду SQL Server Management Studio.

2. **Подключение к серверу**. В диалоговом окне **Соединение с сервером** введите следующие данные:

   | Параметр       | Рекомендуемое значение | Описание | 
   | ------------ | ------------------ | ------------------------------------------------- | 
   | **Тип сервера** | Компонент Database Engine | Это значение обязательно. |
   | **Имя сервера** | Полное имя сервера | Имя должно быть в следующем формате: **mysqldbserver.database.windows.net**. |
   | **Аутентификация** | Проверка подлинности SQL Server | |
   | **Имя входа** | Учетная запись администратора сервера | Это учетная запись, которая была указана при создании сервера. |
   | **Пароль** | Пароль для учетной записи администратора сервера | Это пароль, указанный при создании сервера. |

    ![Подключение к серверу с помощью SSMS](media/ssis-azure-connect-to-catalog-database/ssisdb-connect-1.png)

3. **Подключитесь к базе данных SSISDB**. Чтобы развернуть диалоговое окно **Соединение с сервером**, выберите **Параметры**. В развернутом окне **Соединение с сервером** откройте вкладку **Свойства подключения**. В поле **Подключение к базе данных** выберите или введите `SSISDB`.

    > [!IMPORTANT]
    > Если вы не выбрали `SSISDB` при подключении, каталог служб SSIS в обозревателе объектов может не отображаться.

    ![Выбор базы данных SSISDB для подключения](media/ssis-azure-connect-to-catalog-database/ssisdb-connect-2.png)

4. В этом случае выберите **Подключиться**.

5. В обозревателе объектов разверните узел **Каталоги служб Integration Services** и затем узел **SSISDB** для просмотра объектов в базе данных каталога служб SSIS.

    ![Поиск базы данных SSISDB в обозревателе объектов в SSMS](media/ssis-azure-connect-to-catalog-database/ssisdb-connect-3.png)

## <a name="next-steps"></a>Дальнейшие действия
- Разверните пакет. Дополнительные сведения см. в разделе [Развертывание проекта служб SSIS с помощью SQL Server Management Studio (SSMS)](../ssis-quickstart-deploy-ssms.md).
- Запустите пакет. Дополнительные сведения см. в разделе [Выполнение пакета служб SSIS с помощью SQL Server Management Studio (SSMS)](../ssis-quickstart-run-ssms.md).
- Создайте расписание для пакета. Дополнительные сведения см. в разделе [Планирование выполнения пакетов служб SSIS в Azure](ssis-azure-schedule-packages.md).