---
ms.openlocfilehash: 934db70fe49ba9a5330b25596dc694e0ac50c04e
ms.sourcegitcommit: 24b1f6decbb17bb22a45166e5fdb0845c65af498
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/01/2019
ms.locfileid: "57037896"
---
Par [Rick Anderson](https://twitter.com/RickAndMSFT)

Dans cette section, vous allez ajouter des classes pour la gestion des films dans une base de données. Vous utilisez ces classes avec [Entity Framework Core](/ef/core) (EF Core) pour travailler avec une base de données. EF Core est un framework de mappage relationnel d’objets qui simplifie le code d’accès aux données à écrire.

Les classes de modèle que vous créez portent le nom de classes OCT (« Objet CLR Traditionnel »), car elles n’ont pas de dépendances envers EF Core. Elles définissent les propriétés des données stockées dans la base de données.

Dans ce didacticiel, vous écrivez d’abord les classes du modèle, puis EF Core crée la base de données. Une autre approche que nous ne décrivons pas ici consiste à [générer les classes de modèle à partir d’une base de données existante](/ef/core/get-started/aspnetcore/existing-db).

[Affichez ou téléchargez](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie) l’exemple de code.
