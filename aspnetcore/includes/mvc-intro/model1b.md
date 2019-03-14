---
ms.openlocfilehash: 1c342231905775938715280681ea2b4cf6ebc9bc
ms.sourcegitcommit: 24b1f6decbb17bb22a45166e5fdb0845c65af498
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/01/2019
ms.locfileid: "57047936"
---
Ajoutez les propriétés suivantes à la classe `Movie` :

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie22/Models/Movie.cs?name=snippet1)]

La classe `Movie` contient :

* Le champ `Id`, qui est nécessaire à la base de données pour la clé primaire.
* `[DataType(DataType.Date)]`:  l’attribut [DataType](/dotnet/api/microsoft.aspnetcore.mvc.dataannotations.internal.datatypeattributeadapter) spécifie le type de données (`Date`). Avec cet attribut :

  * L’utilisateur n’est pas obligé d’entrer les informations de temps dans le champ de date.
  * Seule la date est affichée, pas les informations de temps.

Les [DataAnnotations](/dotnet/api/system.componentmodel.dataannotations) sont traitées dans un prochain didacticiel.