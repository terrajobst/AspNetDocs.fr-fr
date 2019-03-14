---
ms.openlocfilehash: 22c540058e0b3b9ae612f1b2612cecc08627ea2b
ms.sourcegitcommit: 24b1f6decbb17bb22a45166e5fdb0845c65af498
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/01/2019
ms.locfileid: "57063986"
---
<a name="test"></a>
### <a name="test-the-app"></a>Tester l’application

* Exécutez l’application et ajoutez `/Movies` à l’URL dans le navigateur (`http://localhost:port/movies`).
* Testez le lien **Créer**.

  ![Créer une page](../../tutorials/razor-pages/model/_static/conan.png)

<a name="scaffold"></a>

* Testez les liens **Modifier**, **Détails** et **Supprimer**.

Si vous obtenez l’erreur suivante, vérifiez que vous avez exécuté les migrations et mis à jour la base de données :

```
An unhandled exception occurred while processing the request.
SqliteException: SQLite Error 1: 'no such table: Movie'.
Microsoft.Data.Sqlite.SqliteException.ThrowExceptionForRC(int rc, sqlite3 db)
```
