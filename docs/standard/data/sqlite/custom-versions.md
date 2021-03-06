---
title: Пользовательские версии SQLite
ms.date: 12/13/2019
description: Узнайте, как использовать пользовательскую версию собственной библиотеки SQLite.
ms.openlocfilehash: 8a2646138ea9dbecf412a2e8e0e347e2d71a5b0b
ms.sourcegitcommit: 30a558d23e3ac5a52071121a52c305c85fe15726
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 12/25/2019
ms.locfileid: "75450390"
---
# <a name="custom-sqlite-versions"></a>Пользовательские версии SQLite

Microsoft. Data. SQLite построен на основе Склитепклрав. Пользовательские версии библиотеки SQLite можно использовать с помощью пакета или путем настройки поставщика Склитепклрав.

## <a name="bundles"></a>Пакеты

Склитепклрав предоставляет пакеты пакетов, которые упрощают создание правильных зависимостей на разных платформах.

Основной пакет Microsoft. Data. SQLite по умолчанию переносится в Склитепклрав. bundle_e_sqlite3.

Чтобы использовать другой набор, установите `Microsoft.Data.Sqlite.Core` пакет, а также пакет набора, который вы хотите использовать. Пакеты автоматически инициализируются с помощью Microsoft. Data. SQLite.

| Пакет | Описание |
| --- | --- |
| Склитепклрав. bundle_e_sqlite3 | Обеспечивает последовательную версию SQLite на всех платформах. Включает в себя FTS4, FTS5, JSON1 и | Расширения дерева R *. Это значение по умолчанию. |
| Склитепклрав. bundle_green | То же, что и bundle_e_sqlite3, за исключением iOS, где она использует библиотеку System SQLite. |
| Склитепклрав. bundle_zetetic | Использует официальные сборки СклЦифер из Зететик (не включена). |
| Склитепклрав. bundle_winsqlite3 | Использует winsqlite3. dll, библиотеку System SQLite в Windows 10. |
| Склитепклрав. bundle_e_sqlcipher | Предоставляет неофициальную сборку с открытым исходным кодом СклЦифер. |

Например, чтобы использовать неофициальную сборку с открытым исходным кодом для СклЦифер, используйте следующие команды.

### <a name="net-core-clitabnetcore-cli"></a>[Интерфейс командной строки .NET Core](#tab/netcore-cli)

```dotnetcli
dotnet add package Microsoft.Data.Sqlite.Core
dotnet add package SQLitePCLRaw.bundle_e_sqlcipher
```

### <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

``` PowerShell
Install-Package Microsoft.Data.Sqlite.Core
Install-Package SQLitePCLRaw.bundle_e_sqlcipher
```

---

## <a name="sqlitepclraw-providers"></a>Поставщики Склитепклрав

Вы можете использовать собственную сборку SQLite, используя пакет `SQLitePCLRaw.provider.dynamic_cdecl`. В этом случае вы несете ответственность за развертывание собственной библиотеки с приложением. Обратите внимание, что сведения о развертывании собственных библиотек в приложении значительно зависят от платформы и среды выполнения .NET, которые вы используете.

Сначала необходимо реализовать Ижетфунктионпоинтер. Реализация является довольно тривиальной в .NET Core.

[!code-csharp[](../../../../samples/snippets/standard/data/sqlite/SystemLibrarySample/Program.cs?name=snippet_NativeLibraryAdapter)]

Затем настройте поставщик Склитепклрав. Убедитесь, что это делается до того, как в приложении будет использоваться Microsoft. Data. SQLite. Кроме того, не используйте пакет пакета Склитепклрав, который может переопределять поставщик.

[!code-csharp[](../../../../samples/snippets/standard/data/sqlite/SystemLibrarySample/Program.cs?name=snippet_SetProvider)]
