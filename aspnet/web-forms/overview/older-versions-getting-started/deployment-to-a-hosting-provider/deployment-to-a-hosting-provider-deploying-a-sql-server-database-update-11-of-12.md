---
uid: web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12
title: 'Déploiement d’une application Web ASP.NET avec SQL Server Compact à l’aide de Visual Studio ou Visual Web Developer : déploiement d’une mise à jour de base de données SQL Server-11 sur 12 | Microsoft Docs'
author: tdykstra
description: Cette série de didacticiels vous montre comment déployer (publier) un projet d’application Web ASP.NET qui comprend une base de données SQL Server Compact à l’aide de Stu...
ms.author: riande
ms.date: 11/17/2011
ms.assetid: 5e2bb092-cb22-4511-ad0a-22ae12dd99b3
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12
msc.type: authoredcontent
ms.openlocfilehash: 0894c0ac24737e66b6960ef3d48aa17f78c6aa1d
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78528125"
---
# <a name="deploying-an-aspnet-web-application-with-sql-server-compact-using-visual-studio-or-visual-web-developer-deploying-a-sql-server-database-update---11-of-12"></a>Déploiement d’une application Web ASP.NET avec SQL Server Compact à l’aide de Visual Studio ou Visual Web Developer : déploiement d’une mise à jour de base de données SQL Server-11 sur 12

par [Tom Dykstra](https://github.com/tdykstra)

[Télécharger le projet de démarrage](https://code.msdn.microsoft.com/Deploying-an-ASPNET-Web-4e31366b)

> Cette série de didacticiels vous montre comment déployer (publier) un projet d’application Web ASP.NET qui comprend une base de données SQL Server Compact à l’aide de Visual Studio 2012 RC ou Visual Studio Express 2012 RC pour le Web. Vous pouvez également utiliser Visual Studio 2010 si vous installez la mise à jour de publication Web. Pour obtenir une présentation de la série, consultez [le premier didacticiel de la série](deployment-to-a-hosting-provider-introduction-1-of-12.md).
> 
> Pour obtenir un didacticiel qui présente les fonctionnalités de déploiement introduites après la version RC de Visual Studio 2012, montre comment déployer des éditions SQL Server autres que SQL Server Compact et montre comment déployer vers des sites Web Windows Azure, consultez [déploiement web ASP.net à l’aide de Visual Studio](../../deployment/visual-studio-web-deployment/introduction.md).

## <a name="overview"></a>Présentation

Ce didacticiel vous montre comment déployer une mise à jour de base de données sur une base de données SQL Server complète. Étant donné que Migrations Code First effectue tout le travail de mise à jour de la base de données, le processus est presque identique à ce que vous avez fait pour SQL Server Compact dans le didacticiel sur le [déploiement d’une mise à jour de base de données](deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12.md) .

Rappel : Si vous recevez un message d’erreur ou si une action ne fonctionne pas au fur et à mesure que vous parcourez le didacticiel, veillez à consulter la [page de résolution des problèmes](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12.md).

## <a name="adding-a-new-column-to-a-table"></a>Ajout d’une nouvelle colonne à une table

Dans cette section du didacticiel, vous allez modifier la base de données et les modifications de code correspondantes, puis les tester dans Visual Studio en vue de les déployer dans les environnements de test et de production. La modification implique l’ajout d’une colonne `OfficeHours` à l’entité `Instructor` et l’affichage des nouvelles informations dans la page Web des **enseignants** .

Dans le projet ContosoUniversity. DAL, ouvrez *Instructor.cs* et ajoutez la propriété suivante entre les propriétés `HireDate` et `Courses` :

[!code-csharp[Main](deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12/samples/sample1.cs)]

Mettez à jour la classe d’initialiseur afin qu’elle amorce la nouvelle colonne avec les données de test. Ouvrez *Migrations\Configuration.cs* et remplacez le bloc de code qui commence `var instructors = new List<Instructor>` par le bloc de code suivant, qui comprend la nouvelle colonne :

[!code-csharp[Main](deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12/samples/sample2.cs)]

Dans le projet ContosoUniversity, ouvrez *Instructors. aspx* et ajoutez un nouveau champ de modèle pour les heures Office juste avant la balise de fermeture `</Columns>` dans le premier contrôle `GridView` :

[!code-aspx[Main](deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12/samples/sample3.aspx)]

Générez la solution.

Ouvrez la fenêtre de **console du gestionnaire de package** , puis sélectionnez CONTOSOUNIVERSITY. dal comme **projet par défaut**.

Entrez les commandes suivantes :

[!code-powershell[Main](deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12/samples/sample4.ps1)]

Exécutez l’application et sélectionnez la page **enseignants** . Le chargement de la page prend un peu plus de temps que d’habitude, car le Entity Framework recrée la base de données et l’amorce avec des données de test.

[![Instructors_page_with_office_hours](deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12/_static/image2.png)](deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12/_static/image1.png)

## <a name="deploying-the-database-update-to-the-test-environment"></a>Déploiement de la mise à jour de la base de données dans l’environnement de test

Lorsque vous utilisez Migrations Code First, la méthode de déploiement d’une modification de base de données sur SQL Server est la même que pour SQL Server Compact. Toutefois, vous devez modifier le profil de publication de test, car il est toujours configuré pour migrer de SQL Server Compact vers SQL Server.

La première étape consiste à supprimer les transformations de chaîne de connexion que vous avez créées dans le didacticiel précédent. Ces derniers ne sont plus nécessaires, car vous devez spécifier des transformations de chaîne de connexion dans le profil de publication, comme vous l’avez fait avant de configurer l’onglet **Package/Publication SQL** pour la migration vers SQL Server.

Ouvrez le fichier *Web. test. config* et supprimez l’élément `connectionStrings`. La seule transformation restante dans le fichier *Web. test. config* concerne la valeur `Environment` dans l’élément `appSettings`.

Vous pouvez maintenant mettre à jour le profil de publication et le publier dans l’environnement de test.

Ouvrez l’Assistant **publier le site Web** , puis basculez vers l’onglet **Profil** .

Sélectionnez le profil de publication de **test** .

Sélectionnez l’onglet **Paramètres**.

Cliquez sur **activer les nouvelles améliorations de la publication de base de données**.

Dans la zone chaîne de connexion pour **SchoolContext**, entrez la valeur que vous avez utilisée dans le fichier de transformation *Web. test. config* dans le didacticiel précédent :

[!code-console[Main](deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12/samples/sample5.cmd)]

Sélectionnez **exécuter migrations code First (s’exécute au démarrage de l’application)** . (Dans votre version de Visual Studio, la case à cocher peut s’intituler **appliquer migrations code First**.)

Dans la zone chaîne de connexion pour **DefaultConnection**, entrez la valeur que vous avez utilisée dans le fichier de transformation *Web. test. config* dans le didacticiel précédent :

[!code-console[Main](deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12/samples/sample6.cmd)]

Laissez **mettre à jour la base de données** vide.

Cliquez sur **Publier**.

Visual Studio déploie les modifications du code dans l’environnement de test et ouvre le navigateur sur la page d’hébergement de Contoso University.

Sélectionnez la page enseignants.

Lorsque l’application exécute cette page, elle tente d’accéder à la base de données. Migrations Code First vérifie si la base de données est en cours et constate que la dernière migration n’a pas encore été appliquée. Migrations Code First applique la dernière migration, exécute la méthode `Seed`, puis la page s’exécute normalement. La nouvelle colonne Office Hours s’affiche avec les données amorcées.

[![Instructors_page_with_OfficeHours_Test](deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12/_static/image4.png)](deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12/_static/image3.png)

## <a name="deploying-the-database-update-to-the-production-environment"></a>Déploiement de la mise à jour de la base de données dans l’environnement de production

Vous devez également modifier le profil de publication pour l’environnement de production. Dans ce cas, vous allez supprimer le profil existant et en créer un nouveau en important un fichier. publishsettings mis à jour. Le fichier mis à jour inclura la chaîne de connexion pour la base de données SQL Server sur Cytanium.

Comme vous l’avez vu quand vous avez déployé dans l’environnement de test, vous n’avez plus besoin de transformations de chaîne de connexion dans le fichier de transformation *Web. production. config* . Ouvrez ce fichier et supprimez l’élément `connectionStrings`. Les transformations restantes concernent la valeur `Environment` dans l’élément `appSettings` et l’élément `location` qui restreint l’accès aux rapports d’erreurs ELMAH.

Avant de créer un profil de publication pour la production, téléchargez un fichier. publishsettings mis à jour de la même façon que vous l’avez fait précédemment dans le didacticiel relatif au [déploiement dans l’environnement de production](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12.md) . (Dans le panneau de configuration de Cytanium, cliquez sur **sites Web**, puis cliquez sur le site Web **contosouniversity.com** . Sélectionnez l’onglet **publication Web** , puis cliquez sur **Télécharger le profil de publication pour ce site Web**.) La raison pour laquelle vous effectuez cette tâche consiste à sélectionner la chaîne de connexion de base de données dans le fichier. publishsettings. La chaîne de connexion n’était pas disponible la première fois que vous avez téléchargé le fichier, car vous utilisiez toujours SQL Server Compact et n’aviez pas encore créé la base de données SQL Server sur Cytanium.

Vous pouvez maintenant mettre à jour le profil de publication et le publier dans l’environnement de production.

Ouvrez l’Assistant **publier le site Web** , puis basculez vers l’onglet **Profil** .

Cliquez sur **gérer les profils**, puis supprimez le profil de production.

Fermez l’Assistant **publier le site Web** pour enregistrer cette modification.

Rouvrez l’Assistant **publier le site Web** , puis cliquez sur **Importer**.

Sous l’onglet **connexion** , remplacez **URL de destination** par la valeur appropriée si vous utilisez une URL temporaire.

Cliquez sur **Next**.

Dans l’onglet **paramètres** , cliquez sur **activer les nouvelles améliorations de la publication de base de données**.

Dans la liste déroulante chaîne de connexion pour **SchoolContext**, sélectionnez la chaîne de connexion Cytanium.

![Selecting_Cytanium_connection_string](deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12/_static/image5.png)

Sélectionnez **exécuter code First migrations (s’exécute au démarrage de l’application)** .

Dans la liste déroulante chaîne de connexion pour **DefaultConnection**, sélectionnez la chaîne de connexion Cytanium.

Sélectionnez l’onglet **Profil** , cliquez sur **gérer les profils**, puis renommez le profil « contosouniversity.com-Web Deploy » en « production ».

Fermez le profil de publication pour enregistrer la modification, puis rouvrez-la.

Cliquez sur **Publier**. (Pour un site Web de production réel, vous devez copier l' *application\_offline. htm* en production et la placer dans votre dossier de projet avant la publication, puis la supprimer une fois le déploiement terminé.)

Visual Studio déploie les modifications du code dans l’environnement de test et ouvre le navigateur sur la page d’hébergement de Contoso University.

Sélectionnez la page enseignants.

Migrations Code First met à jour la base de données de la même manière que dans l’environnement de test. La nouvelle colonne Office Hours s’affiche avec les données amorcées.

![Instructors_page_with_OfficeHours_Prod](deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12/_static/image6.png)

Vous avez maintenant déployé avec succès une mise à jour d’application qui comprenait une modification de base de données à l’aide d’une base de données SQL Server.

## <a name="more-information"></a>Plus d'informations

Cette série de didacticiels se termine lors du déploiement d’une application Web ASP.NET sur un fournisseur d’hébergement tiers. Pour plus d’informations sur les sujets abordés dans ces didacticiels, consultez le [plan de contenu de déploiement ASP.net](https://msdn.microsoft.com/library/bb386521(v=vs.110).aspx) sur le site Web MSDN.

## <a name="acknowledgements"></a>Remerciements

Je souhaite remercier les personnes suivantes qui ont apporté des contributions significatives au contenu de cette série de didacticiels :

- [Alberto Poblacion, MVP &amp; MCT, Espagne](https://mvp.support.microsoft.com/profile/Alberto)
- Jarod Ferguson, MVP développement de plate-forme de données, États-Unis
- Mittal dur, Microsoft
- [Kristina Olson, Microsoft](https://blogs.iis.net/krolson/default.aspx)
- [Mike pape, Microsoft](http://www.mikepope.com/blog/DisplayBlog.aspx)
- Mohit Srivastava, Microsoft
- [Raffaele Rialdi, Italie](http://www.iamraf.net/)
- [Rick Anderson, Microsoft](https://blogs.msdn.com/b/rickandy/)
- [Sayed Hashimi, Microsoft](http://sedodream.com/default.aspx)(twitter : [@sayedihashimi](http://twitter.com/sayedihashimi))
- [Scott Hanselman](http://www.hanselman.com/blog/) (twitter : [@shanselman](http://twitter.com/shanselman))
- [Scott Hunter, Microsoft](https://blogs.msdn.com/b/scothu/) (twitter : [@coolcsh](http://twitter.com/coolcsh))
- [Srđan Božović, Serbie](http://msforge.net/blogs/zmajcek/)
- [Vishal Joshi, Microsoft](http://vishaljoshi.blogspot.com/) (twitter : [@vishalrjoshi](http://twitter.com/vishalrjoshi))

> [!div class="step-by-step"]
> [Précédent](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12.md)
> [Suivant](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12.md)
