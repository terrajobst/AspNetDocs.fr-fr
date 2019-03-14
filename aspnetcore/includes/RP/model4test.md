---
ms.openlocfilehash: 22c540058e0b3b9ae612f1b2612cecc08627ea2b
ms.sourcegitcommit: 24b1f6decbb17bb22a45166e5fdb0845c65af498
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/01/2019
ms.locfileid: "57063986"
---
<a name="test"></a>
### <a name="test-the-app"></a><span data-ttu-id="e7a9e-101">Tester l’application</span><span class="sxs-lookup"><span data-stu-id="e7a9e-101">Test the app</span></span>

* <span data-ttu-id="e7a9e-102">Exécutez l’application et ajoutez `/Movies` à l’URL dans le navigateur (`http://localhost:port/movies`).</span><span class="sxs-lookup"><span data-stu-id="e7a9e-102">Run the app and append `/Movies` to the URL in the browser (`http://localhost:port/movies`).</span></span>
* <span data-ttu-id="e7a9e-103">Testez le lien **Créer**.</span><span class="sxs-lookup"><span data-stu-id="e7a9e-103">Test the **Create** link.</span></span>

  ![Créer une page](../../tutorials/razor-pages/model/_static/conan.png)

<a name="scaffold"></a>

* <span data-ttu-id="e7a9e-105">Testez les liens **Modifier**, **Détails** et **Supprimer**.</span><span class="sxs-lookup"><span data-stu-id="e7a9e-105">Test the **Edit**, **Details**, and **Delete** links.</span></span>

<span data-ttu-id="e7a9e-106">Si vous obtenez l’erreur suivante, vérifiez que vous avez exécuté les migrations et mis à jour la base de données :</span><span class="sxs-lookup"><span data-stu-id="e7a9e-106">If you get the following error, verify you have run migrations and updated the database:</span></span>

```
An unhandled exception occurred while processing the request.
SqliteException: SQLite Error 1: 'no such table: Movie'.
Microsoft.Data.Sqlite.SqliteException.ThrowExceptionForRC(int rc, sqlite3 db)
```
