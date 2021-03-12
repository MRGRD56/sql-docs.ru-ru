---
description: Руководство по Разработка приложения .NET с помощью Always Encrypted с безопасными анклавами
title: Руководство по Разработка приложения .NET с помощью Always Encrypted с безопасными анклавами | Документация Майкрософт
ms.custom: ''
ms.date: 01/15/2021
ms.reviewer: v-daenge
ms.prod: sql
ms.prod_service: connectivity
ms.technology: connectivity
ms.tgt_pltfrm: ''
ms.topic: tutorial
author: karinazhou
ms.author: v-jizho2
ms.openlocfilehash: b2dd15961615ef4bff9c7e5e99f91844956b1895
ms.sourcegitcommit: 15c7cd187dcff9fc91f2daf0056b12ed3f0403f0
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/08/2021
ms.locfileid: "102464930"
---
# <a name="tutorial-develop-a-net-application-using-always-encrypted-with-secure-enclaves"></a>Руководство по Разработка приложения .NET с помощью Always Encrypted с безопасными анклавами

[!INCLUDE [sqlserver2019-windows-only-asdb](../../../includes/applies-to-version/sqlserver2019-windows-only-asdb.md)]

[!INCLUDE [appliesto-netfx-netcore-xxxx-md](../../../includes/appliesto-netfx-netcore-xxxx-md.md)]

В этом руководстве содержатся сведения о разработке приложения, которое выполняет запросы к базе данных, использующие безопасный анклав на стороне сервера для [Always Encrypted с защищенными анклавами](../../../relational-databases/security/encryption/always-encrypted-enclaves.md).

> [!NOTE]
> Always Encrypted с безопасными анклавами поддерживается только в Windows.

## <a name="prerequisites"></a>Предварительные требования

Перед выполнением действий, описанных в этом руководстве, убедитесь, что вы изучили одно из следующих руководств.

- [Учебник. Начало работы с Always Encrypted и безопасными анклавами в SQL Server](../../../relational-databases/security/tutorial-getting-started-with-always-encrypted-enclaves.md)
- [Учебник. Начало работы с Always Encrypted и безопасными анклавами в Базе данных SQL Azure](/azure/azure-sql/database/always-encrypted-enclaves-getting-started)

Кроме того, требуется среда Visual Studio (рекомендуется версия 2019), которую можно скачать на странице [https://visualstudio.microsoft.com/](https://visualstudio.microsoft.com). Среда разработки приложений должна использовать .NET Framework 4.6 или более поздней версии или .NET Core 2.1 или более поздней версии.

## <a name="step-1-set-up-your-visual-studio-project"></a>Шаг 1. Настройка проекта в Visual Studio

Чтобы использовать Always Encrypted с безопасными анклавами в приложении .NET Framework, необходимо убедиться, что приложение предназначено для .NET Framework 4.6 или более поздней версии. Чтобы использовать Always Encrypted с безопасными анклавами в приложении .NET Core, необходимо убедиться, что приложение предназначено для .NET Core 2.1 или более поздней версии.

Кроме того, если вы храните главный ключ столбца в Azure Key Vault, необходимо также интегрировать приложение с [Microsoft.Data.SqlClient.AlwaysEncrypted.AzureKeyVaultProvider NuGet](https://www.nuget.org/packages/Microsoft.Data.SqlClient.AlwaysEncrypted.AzureKeyVaultProvider).

1. Запустите Visual Studio.

2. Создайте проект консольного приложения C\# (.NET Framework или Core).

3. Убедитесь, что в проекте настроена по крайней мере платформа .NET Framework 4.6. или .NET Core 2.1. Щелкните правой кнопкой мыши проект в Обозревателе решений, выберите **Свойства** и укажите целевую платформу.

4. Установите следующий пакет NuGet. Щелкните **Инструменты** (главное меню) > **Диспетчер пакетов NuGet** > **Консоль диспетчера пакетов**. Выполните следующий код в консоли диспетчера пакетов.

   ```powershell
   Install-Package Microsoft.Data.SqlClient -Version 1.1.0
   ```

5. Если вы используете Azure Key Vault для хранения главных ключей столбцов, установите следующие пакеты NuGet, щелкнув **Инструменты** (главное меню) > **Диспетчер пакетов NuGet** > **Консоль диспетчера пакетов**. Выполните следующий код в консоли диспетчера пакетов.

   ```powershell
   Install-Package Microsoft.Data.SqlClient.AlwaysEncrypted.AzureKeyVaultProvider -Version 1.0.0
   Install-Package Microsoft.IdentityModel.Clients.ActiveDirectory
   ```

## <a name="step-2-implement-your-application-logic"></a>Шаг 2. Реализация логики приложения

Приложение подключится к базе данных **ContosoHR** из статьи [Руководство. Начало работы с Always Encrypted с безопасными анклавами с использованием SSMS](../../../relational-databases/security/tutorial-getting-started-with-always-encrypted-enclaves.md) или [Руководство. Начало работы с Always Encrypted с безопасными анклавами в Базе данных SQL Azure](/azure/azure-sql/database/always-encrypted-enclaves-getting-started) и выполнит запрос, содержащий предикат `LIKE` в столбце **SSN**, а также проведет сравнение диапазонов в столбце **Salary**.

1. Замените содержимое файла Program.cs (созданного в Visual Studio) на приведенный ниже код.

    ```cs
    using System;
    using Microsoft.Data.SqlClient;
    using System.Data;

    namespace ConsoleApp1
    {
        class Program
        {
            static void Main(string[] args)
            {

                // Connection string for SQL Server
                string connectionString = "Data Source = myserver; Initial Catalog = ContosoHR; Column Encryption Setting = Enabled;Attestation Protocol = HGS; Enclave Attestation Url = http://hgs.bastion.local/Attestation; Integrated Security = true";

                // Connection string for Azure SQL Database
                //string connectionString = "Data Source = myserver.database.windows.net; Initial Catalog = ContosoHR; Column Encryption Setting = Enabled;Attestation Protocol = AAS; Enclave Attestation Url = https://myattestationprovider.uks.attest.azure.net/attest/SgxEnclave; User ID=user; Password=password";

                using (SqlConnection connection = new SqlConnection(connectionString))
                {
                    connection.Open();

                    SqlCommand cmd = connection.CreateCommand();
                    cmd.CommandText = @"SELECT [SSN], [FirstName], [LastName], [Salary] FROM [HR].[Employees] WHERE [SSN] LIKE @SSNPattern AND [Salary] > @MinSalary;";

                    SqlParameter paramSSNPattern = cmd.CreateParameter();

                    paramSSNPattern.ParameterName = @"@SSNPattern";
                    paramSSNPattern.DbType = DbType.AnsiStringFixedLength;
                    paramSSNPattern.Direction = ParameterDirection.Input;
                    paramSSNPattern.Value = "%9838";
                    paramSSNPattern.Size = 11;

                    cmd.Parameters.Add(paramSSNPattern);

                    SqlParameter MinSalary = cmd.CreateParameter();

                    MinSalary.ParameterName = @"@MinSalary";
                    MinSalary.DbType = DbType.Currency;
                    MinSalary.Direction = ParameterDirection.Input;
                    MinSalary.Value = 20000;

                    cmd.Parameters.Add(MinSalary);
                    cmd.ExecuteNonQuery();

                    SqlDataReader reader = cmd.ExecuteReader();
                    while (reader.Read())

                    {
                        Console.WriteLine(reader[0] + ", " + reader[1] + ", " + reader[2] + ", " + reader[3]);
                    }
                    Console.ReadKey();
                }
            }
        }
    }
    ```

2. Обновите строку подключения к базе данных.
    1. Укажите допустимое имя сервера и параметры аутентификации базы данных.
    2. Задайте для ключевого слова `Attestation Protocol` значение:
       - `HGS` — если вы используете [!INCLUDE[ssnoversion-md](../../../includes/ssnoversion-md.md)] и службу защиты узла (HGS);
       - `AAS` — если вы используете [!INCLUDE[ssSDSfull](../../../includes/sssdsfull-md.md)] и Аттестацию Microsoft Azure.
    3. Укажите `Enclave Attestation URL` URL-адрес аттестации для своей среды.

3. Выполните сборку и запустите приложение.

## <a name="see-also"></a>См. также раздел

- [Использование Always Encrypted с поставщиком данных Microsoft .NET для SQL Server](sqlclient-support-always-encrypted.md)
- [Example demonstrating use of Azure Key Vault provider with Always Encrypted](azure-key-vault-example.md) (Пример использования поставщика Azure Key Vault с поддержкой Always Encrypted)
- [Пример использования поставщика Azure Key Vault с поддержкой Always Encrypted с безопасными анклавами](azure-key-vault-enclave-example.md)
