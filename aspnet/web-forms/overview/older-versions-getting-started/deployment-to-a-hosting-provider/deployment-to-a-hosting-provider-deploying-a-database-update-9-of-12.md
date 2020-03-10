---
uid: web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12
title: 'Déploiement d’une application Web ASP.NET avec SQL Server Compact à l’aide de Visual Studio ou Visual Web Developer : déploiement d’une mise à jour de base de données-9 sur 12 | Microsoft Docs'
author: tdykstra
description: Cette série de didacticiels vous montre comment déployer (publier) un projet d’application Web ASP.NET qui comprend une base de données SQL Server Compact à l’aide de Stu...
ms.author: riande
ms.date: 11/17/2011
ms.assetid: a8d776af-4735-4612-87f6-9f326587f2d3
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12
msc.type: authoredcontent
ms.openlocfilehash: 3385e1019d9e7a9263fd9513c39b25eb85febbb5
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78564441"
---
# <a name="deploying-an-aspnet-web-application-with-sql-server-compact-using-visual-studio-or-visual-web-developer-deploying-a-database-update---9-of-12"></a>Déploiement d’une application Web ASP.NET avec SQL Server Compact à l’aide de Visual Studio ou Visual Web Developer : déploiement d’une mise à jour de base de données-9 sur 12

par [Tom Dykstra](https://github.com/tdykstra)

[Télécharger le projet de démarrage](https://code.msdn.microsoft.com/Deploying-an-ASPNET-Web-4e31366b)

> Cette série de didacticiels vous montre comment déployer (publier) un projet d’application Web ASP.NET qui comprend une base de données SQL Server Compact à l’aide de Visual Studio 2012 RC ou Visual Studio Express 2012 RC pour le Web. Vous pouvez également utiliser Visual Studio 2010 si vous installez la mise à jour de publication Web. Pour obtenir une présentation de la série, consultez [le premier didacticiel de la série](deployment-to-a-hosting-provider-introduction-1-of-12.md).
> 
> Pour obtenir un didacticiel qui présente les fonctionnalités de déploiement introduites après la version RC de Visual Studio 2012, montre comment déployer des éditions SQL Server autres que SQL Server Compact et montre comment déployer vers Azure App Service Web Apps, consultez [déploiement Web ASP.net à l’aide de Visual Studio](../../deployment/visual-studio-web-deployment/introduction.md).

## <a name="overview"></a>Présentation

Dans ce didacticiel, vous apportez une modification à la base de données et des modifications de code connexes, vous testez les modifications dans Visual Studio, puis vous déployez la mise à jour à la fois dans les environnements de test et de production.

Rappel : Si vous recevez un message d’erreur ou si une action ne fonctionne pas au fur et à mesure que vous parcourez le didacticiel, veillez à consulter la [page de résolution des problèmes](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12.md).

## <a name="adding-a-new-column-to-a-table"></a>Ajout d’une nouvelle colonne à une table

Dans cette section, vous allez ajouter une colonne de date de naissance à la classe de base `Person` pour les entités `Student` et `Instructor`. Ensuite, vous mettez à jour la page qui affiche les données de l’instructeur afin qu’il affiche la nouvelle colonne.

Dans le projet *ContosoUniversity. dal* , ouvrez *Person.cs* et ajoutez la propriété suivante à la fin de la classe `Person` (il doit y avoir deux accolades fermantes après) :

[!code-csharp[Main](deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12/samples/sample1.cs)]

Ensuite, mettez à jour la méthode Seed afin qu’elle fournisse une valeur pour la nouvelle colonne. Ouvrez *Migrations\Configuration.cs* et remplacez le bloc de code qui commence `var instructors = new List<Instructor>` par le bloc de code suivant, qui comprend les informations de date de naissance :

[!code-csharp[Main](deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12/samples/sample2.cs)]

Dans le projet ContosoUniversity, ouvrez *Instructors. aspx* et ajoutez un nouveau champ de modèle pour afficher la date de naissance. Ajoutez-le entre ceux pour la date d’embauche et l’attribution de bureau :

[!code-aspx[Main](deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12/samples/sample3.aspx)]

(Si la mise en retrait du code est désynchronisée, vous pouvez appuyer sur CTRL-K, puis sur CTRL + D pour reformater automatiquement le fichier.)

Générez la solution, puis ouvrez la fenêtre de **console du gestionnaire de package** . Assurez-vous que ContosoUniversity. DAL est toujours sélectionné comme **projet par défaut**.

Dans la fenêtre de la **console du gestionnaire de package** , sélectionnez **ContosoUniversity. dal** comme **projet par défaut**, puis entrez la commande suivante :

[!code-powershell[Main](deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12/samples/sample4.ps1)]

Une fois cette commande terminée, Visual Studio ouvre le fichier de classe qui définit la nouvelle `DbMigration` classe, et dans la méthode `Up` vous pouvez voir le code qui crée la nouvelle colonne.

![AddBirthDate_migration_code](deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12/_static/image1.png)

Générez la solution, puis entrez la commande suivante dans la fenêtre de la **console du gestionnaire de package** (Assurez-vous que le projet CONTOSOUNIVERSITY. dal est toujours sélectionné) :

[!code-powershell[Main](deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12/samples/sample5.ps1)]

Une fois la commande terminée, exécutez l’application et sélectionnez la page des enseignants. Lorsque la page se charge, vous voyez qu’elle contient le champ nouvelle date de naissance.

[![Instructors_page_with_birth_date](deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12/_static/image3.png)](deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12/_static/image2.png)

## <a name="deploying-the-database-update-to-the-test-environment"></a>Déploiement de la mise à jour de la base de données dans l’environnement de test

Dans **Explorateur de solutions** sélectionnez le projet ContosoUniversity.

Dans la barre d’outils **publication Web en un clic** , sélectionnez le profil de publication de **test** , puis cliquez sur **publier le site Web**. (Si la barre d’outils est désactivée, sélectionnez le projet ContosoUniversity dans **Explorateur de solutions**.)

Visual Studio déploie l’application mise à jour et le navigateur s’ouvre sur la page d’hébergement. Exécutez la page des enseignants pour vérifier que la mise à jour a été correctement déployée. Lorsque l’application tente d’accéder à la base de données pour cette page, Code First met à jour le schéma de la base de données et exécute la méthode `Seed`. Lorsque la page s’affiche, la colonne de **Date de naissance** attendue contient des dates.

[![Instructors_page_with_birth_date_Test](deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12/_static/image5.png)](deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12/_static/image4.png)

## <a name="deploying-the-database-update-to-the-production-environment"></a>Déploiement de la mise à jour de la base de données dans l’environnement de production

Vous pouvez maintenant déployer en production. La seule différence est que vous allez utiliser l' *application\_offline. htm* pour empêcher les utilisateurs d’accéder au site et ainsi mettre à jour la base de données pendant que vous déployez les modifications. Pour le déploiement de production, procédez comme suit :

- Chargez l' *application\_fichier offline. htm* sur le site de production.
- Dans Visual Studio, choisissez le profil de production dans la barre d’outils **publication Web en un clic** , puis cliquez sur **publier le site Web**.
- Supprimez l' *application\_fichier offline. htm* du site de production.

> [!NOTE]
> Lorsque votre application est en cours d’utilisation dans l’environnement de production, vous devez implémenter un plan de sauvegarde. Autrement dit, vous devez régulièrement copier les fichiers *School-prod. sdf* et *ASPNET-prod. sdf* à partir du site de production vers un emplacement de stockage sécurisé, et vous devez conserver plusieurs générations de ces sauvegardes. Lorsque vous mettez à jour la base de données, vous devez effectuer une copie de sauvegarde immédiatement avant la modification. Ensuite, si vous faites une erreur et que vous ne la Découvrez pas tant que vous ne l’avez pas déployée en production, vous pourrez toujours récupérer la base de données dans l’État où elle se trouvait avant qu’elle ne soit endommagée.

Lorsque Visual Studio ouvre l’URL de la page d’hébergement dans le navigateur, la page *application\_offline. htm* s’affiche. Après avoir supprimé l' *application\_fichier offline. htm* , vous pouvez à nouveau accéder à votre page d’hébergement pour vérifier que la mise à jour a été correctement déployée.

[![Instructors_page_with_birth_date_Prod](deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12/_static/image7.png)](deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12/_static/image6.png)

Vous avez maintenant déployé une mise à jour d’application qui comprenait une modification de base de données à la fois test et production. Le didacticiel suivant vous montre comment migrer votre base de données de SQL Server Compact vers SQL Server Express et SQL Server.

> [!div class="step-by-step"]
> [Précédent](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12.md)
> [Suivant](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12.md)
