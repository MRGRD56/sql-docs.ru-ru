---
title: SQLPackage.exe
description: Узнайте, как автоматизировать задачи разработки баз данных с помощью SqlPackage.exe. Ознакомьтесь с примерами и параметрами публикации, свойствами и переменными SQLCMD.
ms.prod: sql
ms.prod_service: sql-tools
ms.technology: tools-other
ms.topic: conceptual
ms.assetid: 198198e2-7cf4-4a21-bda4-51b36cb4284b
author: dzsquared
ms.author: drskwier
ms.reviewer: maghan; sstein
ms.date: 3/10/2021
ms.openlocfilehash: fda6b222d5c3ab84a153825c6e8f6e56c187375a
ms.sourcegitcommit: 81ee3cd57526d255de93afb84186074a3fb9885f
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/10/2021
ms.locfileid: "102622620"
---
# <a name="sqlpackageexe"></a>SQLPackage.exe

Программа командной строки **SqlPackage.exe** автоматизирует следующие задачи разработки баз данных.  
  
- [Версия.](#version) Возвращает номер сборки приложения SqlPackage.  Этот параметр добавлен в версии 18.6.

- [Extract](sqlpackage-extract.md): Создает файл приложения уровня данных (DACPAC), содержащий только схему или схему и пользовательские данные из подключенной базы данных SQL.  
  
- [Publish](sqlpackage-publish.md): выполняет добавочное обновление схемы базы данных в соответствии со схемой исходного DACPAC-файла. Если база данных не существует на сервере, операция публикации создаст ее. В противном случае обновляется существующая база данных.  
  
- [Export](sqlpackage-export.md): экспортирует подключенную базу данных SQL, включая схему базы данных и пользовательские данные, в BACPAC-файл.  
  
- [Import](sqlpackage-import.md): импортирует схему и данные таблиц из BACPAC-файла в новую пользовательскую базу данных.  
  
- [DeployReport](sqlpackage-deploy-drift-report.md): создает XML-отчет по изменениям, которые должны быть внесены в результате публикации.  
  
- [DriftReport](sqlpackage-deploy-drift-report.md): создает XML-отчет по изменениям, которые были внесены в зарегистрированную базу данных со времени ее последней регистрации.  
  
- [Script](sqlpackage-script.md): создает скрипт добавочного обновления на языке Transact-SQL, который обновляет схему целевой базы данных до соответствия схеме базы данных-источника.  
  
Программа командной строки **SqlPackage.exe** позволяет указывать эти действия вместе с соответствующими параметрами и свойствами.  

**[Скачать последнюю версию](sqlpackage-download.md)** . Подробнее см. в [заметках о выпуске](release-notes-sqlpackage.md).
  
## <a name="command-line-syntax"></a>Синтаксис командной строки

Программа **SqlPackage.exe** инициирует действия, заданные с использованием параметров, свойств и переменных SQLCMD, указанных в командной строке.  
  
```
SqlPackage {parameters}{properties}{SQLCMD Variables}  
```

### <a name="usage-examples"></a>Примеры использования

**Создание сравнения баз данных с помощью DACPAC-файлов с выходными данными скрипта SQL**

Сначала создайте DACPAC-файл с последними изменениями базы данных:

```
sqlpackage.exe /TargetFile:"C:\sqlpackageoutput\output_current_version.dacpac" /Action:Extract /SourceServerName:"." /SourceDatabaseName:"Contoso.Database"
 ```
 
Затем создайте DACPAC-файл целевого объекта базы данных (без изменений):

 ```
 sqlpackage.exe /TargetFile:"C:\sqlpackageoutput\output_target.dacpac" /Action:Extract /SourceServerName:"." /SourceDatabaseName:"Contoso.Database"
 ```

Создайте скрипт SQL, который создает различия между двумя DACPAC-файлами:

```
sqlpackage.exe /Action:Script /SourceFile:"C:\sqlpackageoutput\output_current_version.dacpac" /TargetFile:"C:\sqlpackageoutput\output_target.dacpac" /TargetDatabaseName:"Contoso.Database" /OutputPath:"C:\sqlpackageoutput\output.sql"
 ```

 ## <a name="version"></a>Версия

Выводит версию sqlpackage в виде номера сборки.  Может использоваться в интерактивных запросах, а также в [автоматизированных конвейерах](sqlpackage-pipelines.md).

```
sqlpackage.exe /Version
 ```


## <a name="exit-codes"></a>Коды выхода

Команды, возвращающие следующие коды выхода:

- 0 = успешное завершение;
- ненулевое значение = сбой.


## <a name="parameters"></a>Параметры
Некоторые параметры являются общими для действий SqlPackage. Ниже приведена таблица с описанием параметров. Дополнительные сведения см. на страницах с описанием определенных действий.

| Параметр | Краткая форма | [Извлечение](sqlpackage-extract.md#parameters-for-the-extract-action) | [Опубликовать](sqlpackage-publish.md#parameters-for-the-publish-action) | [Экспорт](sqlpackage-export.md#parameters-for-the-export-action) | [Импорт](sqlpackage-import.md#parameters-for-the-import-action) | [DeployReport](sqlpackage-deploy-drift-report.md#deployreport-action-parameters) | [DriftReport](sqlpackage-deploy-drift-report.md#driftreport-action-parameters) | [Сценарий](sqlpackage-script.md#parameters-for-the-script-action) |
|---|---|---|---|---|---|---|---|---|
|**/AccessToken:**|**/at**| x | x | x | x | x | x | x |
|**/ClientId:**|**/cid**| | x | | | | | |
|**/DeployScriptPath:**|**/dsp**| | x | | | | | x |
|**/DeployReportPath:**|**/drp**| | x | | | | | x |
|**/Diagnostics:**|**/d**| x | x | x | x | x | x | x |
|**/DiagnosticsFile:**|**/df**| x | x | x | x | x | x | x |
|**/MaxParallelism:**|**/mp**| x | x | x | x | x | x | x |
|**/OutputPath:**|**/op**|  |  |  | | x | x | x |
|**/OverwriteFiles:**|**/of**| x | x | x | | x | x | x |
|**/Profile:**|**/pr**| | x | | | x | | x |
|**/Properties:**|**/p**| x | x | x | x | x | | x |
|**/Quiet:**|**/q**| x | x | x | x | x | x | x |
|**/Secret:**|**/secr**| | x | | | | | |
|**/SourceConnectionString:**|**/scs**| x | x | x | | x | | x |
|**/SourceDatabaseName:**|**/sdn**| x | x | x | | x | | x |
|**/SourceEncryptConnection:**|**/sec**| x | x | x | | x | | x |
|**/SourceFile:**|**/sf**| | x | | x | x | | x |
|**/SourcePassword:**|**/sp**| x | x | x | | x | | x |
|**/SourceServerName:**|**/ssn**| x | x | x | | x | | x |
|**/SourceTimeout:**|**/st**| x | x | x | | x | | x |
|**/SourceTrustServerCertificate:**|**/stsc**| x | x | x | | x | | x |
|**/SourceUser:**|**/su**| x | x | x | | x | | x |
|**/TargetConnectionString:**|**/tcs**| | | | x | x | x | x |
|**/TargetDatabaseName:**|**/tdn**| | x | | x | x | x | x |
|**/TargetEncryptConnection:**|**/tec**| | x | | x | x | x | x |
|**/TargetFile:**|**/tf**| x | | x | | x | | x |
|**/TargetPassword:**|**/tp**| | x | | x | x | x | x |
|**/TargetServerName:**|**/tsn**| | x | | x | x | x | x |
|**/TargetTimeout:**|**/tt**| | x | | x | x | x | x |
|**/TargetTrustServerCertificate:**|**/ttsc**| | x | | x | x | x | x |
|**/TargetUser:**|**/tu**| | x | | x | x | x | x |
|**/TenantId:**|**/tid**| x | x | x | x | x | x | x |
|**/UniversalAuthentication:**|**/ua**| x | x | x | x | x | x | x |
|**/Variables:**|**/v**| | | | | x | | x |

## <a name="properties"></a>Свойства
Некоторые свойства являются общими для действий SqlPackage.  Ниже приведена таблица с описанием свойств. Дополнительные сведения см. на страницах с описанием определенных действий.

| Свойство | [Извлечение](sqlpackage-extract.md#properties-specific-to-the-extract-action) | [Опубликовать](sqlpackage-publish.md#properties-specific-to-the-publish-action) | [Экспорт](sqlpackage-export.md#properties-specific-to-the-export-action) | [Импорт](sqlpackage-import.md#properties-specific-to-the-import-action) | [DeployReport](sqlpackage-deploy-drift-report.md#deployreport-action-properties) | [Сценарий](sqlpackage-script.md#properties-specific-to-the-script-action) |
|---|---|---|---|---|---|---|
|AdditionalDeploymentContributorArguments=(STRING)| | x | | | x | x |
|AdditionalDeploymentContributors=(STRING)| | x | | | x | x |
|AdditionalDeploymentContributorPaths=(STRING)| | x | | | x | x |
|AllowDropBlockingAssemblies=(BOOLEAN)| | x | | | x | x |
|AllowIncompatiblePlatform=(BOOLEAN)| | x | | | x | x |
|AllowUnsafeRowLevelSecurityDataMovement=(BOOLEAN)| | x | | | x | x |
|AzureSharedAccessSignatureToken=(STRING)| | x | | | | |
|AzureStorageBlobEndpoint=(STRING)| x | x | | | | |
|AzureStorageContainer=(STRING)| x | x | | | | |
|AzureStorageKey=(STRING)| x | x | | | | |
|AzureStorageRootPath=(STRING)| x | x | | | | |
|BackupDatabaseBeforeChanges=(BOOLEAN)| | x | | | x | x |
|BlockOnPossibleDataLoss=(BOOLEAN 'True')| | x | | | x | x |
|BlockWhenDriftDetected=(BOOLEAN 'True')| | x | | | x | x |
|CommandTimeout=(INT32 '60')| x | x | x | x | x | x |
|CommentOutSetVarDeclarations=(BOOLEAN)| | x | | | x | x |
|CompareUsingTargetCollation=(BOOLEAN)| | x | | | x | x |
|CreateNewDatabase=(BOOLEAN)| | x | | | x | x |
|DacApplicationDescription=(STRING)| x | | | | | |
|DacApplicationName=(STRING)| x | | | | | |
|DacMajorVersion=(INT32 '1')| x | | | | | |
|DacMinorVersion=(INT32 '0')| x | | | | | |
|DatabaseEdition=(ENUM 'Default')| | x | | x | x | x |
|DatabaseLockTimeout=(INT32 '60')| x | x | x | | x | x |
|DatabaseMaximumSize=(INT32)| | x | | x | x | x |
|DatabaseServiceObjective=(STRING)| | x | | x | x | x |
|DeployDatabaseInSingleUserMode=(BOOLEAN)| | x | | | x | x |
|DisableAndReenableDdlTriggers=(BOOLEAN 'True')| | x | | | x | x |
|DisableIndexesForDataPhase=(BOOLEAN 'True')| | | | x | | |
|DoNotAlterChangeDataCaptureObjects=(BOOLEAN 'True')| | x | | | x | x |
|DoNotAlterReplicatedObjects=(BOOLEAN 'True')| | x | | | x | x |
|DoNotDropObjectType=(STRING)| | x | | | x | x |
|DoNotDropObjectTypes=(STRING)| | x | | | x | x |
|DropConstraintsNotInSource=(BOOLEAN 'True')| | x | | | x | x |
|DropDmlTriggersNotInSource=(BOOLEAN 'True')| | x | | | x | x |
|DropExtendedPropertiesNotInSource=(BOOLEAN 'True')| | x | | | x | x |
|DropIndexesNotInSource=(BOOLEAN 'True')| | x | | | x | x |
|DropObjectsNotInSource=(BOOLEAN)| | x | | | x | x |
|DropPermissionsNotInSource=(BOOLEAN)| | x | | | x | x |
|DropRoleMembersNotInSource=(BOOLEAN)| | x | | | x | x |
|DropStatisticsNotInSource=(BOOLEAN 'True')| | x | | | x | x |
|ExcludeObjectType=(STRING)| | x | | | x | x |
|ExcludeObjectTypes=(STRING)| | x | | | x | x |
|ExtractAllTableData=(BOOLEAN)| x | | | | | |
|ExtractApplicationScopedObjectsOnly=(BOOLEAN 'True')| x | | | | | |
|ExtractReferencedServerScopedElements=(BOOLEAN 'True')| x | | | | | |
|ExtractUsageProperties=(BOOLEAN)| x | | | | | |
|GenerateSmartDefaults=(BOOLEAN)| | x | | | x | x |
|IgnoreAnsiNulls=(BOOLEAN 'True')| | x | | | x | x |
|IgnoreAuthorizer=(BOOLEAN)| | x | | | x | x |
|IgnoreColumnCollation=(BOOLEAN)| | | | | x | x |
|IgnoreColumnOrder=(BOOLEAN)| | x | | | x | x |
|IgnoreComments=(BOOLEAN)| | x | | | x | x |
|IgnoreCryptographicProviderFilePath=(BOOLEAN 'True')| | x | | | x | x |
|IgnoreDdlTriggerOrder=(BOOLEAN)| | x | | | x | x |
|IgnoreDdlTriggerState=(BOOLEAN)| | x | | | x | x |
|IgnoreDefaultSchema=(BOOLEAN)| | x | | | x | x |
|IgnoreDmlTriggerOrder=(BOOLEAN)| | x | | | x | x |
|IgnoreDmlTriggerState=(BOOLEAN)| | x | | | x | x |
|IgnoreExtendedProperties=(BOOLEAN)| x | x | | | x | x |
|IgnoreFileAndLogFilePath=(BOOLEAN 'True')| | x | | | x | x |
|IgnoreFilegroupPlacement=(BOOLEAN 'True')| | x | | | x | x |
|IgnoreFileSize=(BOOLEAN 'True')| | x | | | x | x |
|IgnoreFillFactor=(BOOLEAN 'True')| | x | | | x | x |
|IgnoreFullTextCatalogFilePath=(BOOLEAN 'True')| | x | | | x | x |
|IgnoreIdentitySeed=(BOOLEAN)| | x | | | x | x |
|IgnoreIncrement=(BOOLEAN)| | x | | | x | x |
|IgnoreIndexOptions=(BOOLEAN)| | x | | | x | x |
|IgnoreIndexPadding=(BOOLEAN 'True')| | x | | | x | x |
|IgnoreKeywordCasing=(BOOLEAN 'True')| | x | | | x | x |
|IgnoreLockHintsOnIndexes=(BOOLEAN)| | x | | | x | x |
|IgnoreLoginSids=(BOOLEAN 'True')| | x | | | x | x |
|IgnoreNotForReplication=(BOOLEAN)| | x | | | x | x |
|IgnoreObjectPlacementOnPartitionScheme=(BOOLEAN 'True')| | x | | | x | x |
|IgnorePartitionSchemes=(BOOLEAN)| | x | | | x | x |
|IgnorePermissions=(BOOLEAN 'True')| x | x | | | x | x |
|IgnoreQuotedIdentifiers=(BOOLEAN 'True')| | x | | | x | x |
|IgnoreRoleMembership=(BOOLEAN)| | x | | | x | x |
|IgnoreRouteLifetime=(BOOLEAN 'True')| | x | | | x | x |
|IgnoreSemicolonBetweenStatements=(BOOLEAN 'True')| | x | | | x | x |
|IgnoreTableOptions=(BOOLEAN)| | x | | | x | x |
|IgnoreTablePartitionOptions=(BOOLEAN)| | x | | | x | x |
|IgnoreUserLoginMappings=(BOOLEAN)| x | | | | | |
|IgnoreUserSettingsObjects=(BOOLEAN)| | x | | | x | x |
|IgnoreWhitespace=(BOOLEAN 'True')| | x | | | x | x |
|IgnoreWithNocheckOnCheckConstraints=(BOOLEAN)| | x | | | x | |
|IgnoreWithNocheckOnForeignKeys=(BOOLEAN)| | x | | | x | |
|ImportContributorArguments=(STRING)| | | | x | | |
|ImportContributors=(STRING)| | | | x | | |
|ImportContributorPaths=(STRING)| | | | x | | |
|IncludeCompositeObjects=(BOOLEAN)| | x | | | x | x |
|IncludeTransactionalScripts=(BOOLEAN)| | x | | | x | x |
|LongRunningCommandTimeout=(INT32)| x | x | x | x | x | x |
|NoAlterStatementsToChangeClrTypes=(BOOLEAN)| | x | | | x | x |
|PopulateFilesOnFileGroups=(BOOLEAN 'True')| | x | | | x | x |
|RebuildIndexesOfflineForDataPhase=(BOOLEAN 'False')| | | | x | | |
|RegisterDataTierApplication=(BOOLEAN)| | x | | | x | x |
|RunDeploymentPlanExecutors=(BOOLEAN)| | x | | | x | x |
|ScriptDatabaseCollation=(BOOLEAN)| | x | | | x | x |
|ScriptDatabaseCompatibility=(BOOLEAN)| | x | | | x | x |
|ScriptDatabaseOptions=(BOOLEAN 'True')| | x | | | x | x |
|ScriptDeployStateChecks=(BOOLEAN)| | x | | | x | x |
|ScriptFileSize=(BOOLEAN)| | x | | | x | x |
|ScriptNewConstraintValidation=(BOOLEAN 'True')| | x | | | x | x |
|ScriptRefreshModule=(BOOLEAN 'True')| | x | | | x | x |
|Storage=({File&#124;Memory} 'File')| x | x | x | x | x | x |
|TableData=(STRING)| x | | x | | | |
|TargetEngineVersion=(ENUM 'Latest')| | | x | | | |
|TempDirectoryForTableData=(STRING)| x | | x | | | |
|TreatVerificationErrorsAsWarnings=(BOOLEAN)| | x | | | x | x |
|UnmodifiableObjectWarnings=(BOOLEAN 'True')| | x | | | x | x |
|VerifyCollationCompatibility=(BOOLEAN 'True')| | x | | | x | x |
|VerifyDeployment=(BOOLEAN 'True')| | x | | | x | x |
|VerifyExtraction=(BOOLEAN)| x | | | | | |
|VerifyFullTextDocumentTypesSupported=(BOOLEAN)| | | x | | | |


## <a name="next-steps"></a>Дальнейшие действия

- Дополнительные сведения об [извлечении SqlPackage](sqlpackage-extract.md)
- Дополнительные сведения о [публикации SqlPackage](sqlpackage-publish.md)
- Дополнительные сведения об [экспорте SqlPackage](sqlpackage-export.md)
- Дополнительные сведения об [импорте SqlPackage](sqlpackage-import.md)
