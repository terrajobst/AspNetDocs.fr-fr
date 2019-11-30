---
uid: web-forms/overview/deployment/visual-studio-web-deployment/deploying-a-database-update
title: 'Déploiement Web ASP.NET à l’aide de Visual Studio : déploiement d’une mise à jour de base de données | Microsoft Docs'
author: tdykstra
description: Cette série de didacticiels vous montre comment déployer (publier) une application Web ASP.NET sur Azure App Service Web Apps ou sur un fournisseur d’hébergement tiers, par utilisez...
ms.author: riande
ms.date: 02/15/2013
ms.assetid: 9cad0833-486a-4474-a7f3-7715542ec4ce
msc.legacyurl: /web-forms/overview/deployment/visual-studio-web-deployment/deploying-a-database-update
msc.type: authoredcontent
ms.openlocfilehash: 805eb84c24764cf921291f89054435601dbac48e
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/28/2019
ms.locfileid: "74636828"
---
# <a name="aspnet-web-deployment-using-visual-studio-deploying-a-database-update"></a>Déploiement Web ASP.NET à l’aide de Visual Studio : déploiement d’une mise à jour de base de données

par [Tom Dykstra](https://github.com/tdykstra)

[Télécharger le projet de démarrage](https://go.microsoft.com/fwlink/p/?LinkId=282627)

> Cette série de didacticiels vous montre comment déployer (publier) une application Web ASP.NET sur Azure App Service Web Apps ou sur un fournisseur d’hébergement tiers, à l’aide de Visual Studio 2012 ou de Visual Studio 2010. Pour plus d’informations sur la série, consultez [le premier didacticiel de la série](introduction.md).

## <a name="overview"></a>Vue d'ensemble de

Dans ce didacticiel, vous apportez une modification à la base de données et des modifications de code connexes, vous testez les modifications dans Visual Studio, puis vous déployez la mise à jour dans les environnements de test, intermédiaire et de production.

Le didacticiel explique d’abord comment mettre à jour une base de données gérée par Migrations Code First, puis comment mettre à jour une base de données à l’aide du fournisseur dbDacFx.

Rappel : Si vous recevez un message d’erreur ou si une action ne fonctionne pas au fur et à mesure que vous parcourez le didacticiel, veillez à consulter la [page de résolution des problèmes](troubleshooting.md).

## <a name="deploy-a-database-update-by-using-code-first-migrations"></a>Déployer une mise à jour de base de données à l’aide de Migrations Code First

Dans cette section, vous allez ajouter une colonne de date de naissance à la classe de base `Person` pour les entités `Student` et `Instructor`. Ensuite, vous mettez à jour la page qui affiche les données de l’instructeur afin qu’il affiche la nouvelle colonne. Enfin, vous déployez les modifications apportées à test, intermédiaire et production.

### <a name="add-a-column-to-a-table-in-the-application-database"></a>Ajouter une colonne à une table dans la base de données d’application

1. Dans le projet *ContosoUniversity. dal* , ouvrez *Person.cs* et ajoutez la propriété suivante à la fin de la classe `Person` (il doit y avoir deux accolades fermantes après) :

    [!code-csharp[Main](deploying-a-database-update/samples/sample1.cs)]

    Ensuite, mettez à jour la méthode `Seed` afin qu’elle fournisse une valeur pour la nouvelle colonne. Ouvrez *Migrations\Configuration.cs* et remplacez le bloc de code qui commence `var instructors = new List<Instructor>` par le bloc de code suivant, qui comprend les informations de date de naissance :

    [!code-csharp[Main](deploying-a-database-update/samples/sample2.cs)]
2. Générez la solution, puis ouvrez la fenêtre de **console du gestionnaire de package** . Assurez-vous que ContosoUniversity. DAL est toujours sélectionné comme **projet par défaut**.
3. Dans la fenêtre de la **console du gestionnaire de package** , sélectionnez **ContosoUniversity. dal** comme **projet par défaut**, puis entrez la commande suivante :

    [!code-powershell[Main](deploying-a-database-update/samples/sample3.ps1)]

    Une fois cette commande terminée, Visual Studio ouvre le fichier de classe qui définit la nouvelle `DbMigration` classe, et dans la méthode `Up` vous pouvez voir le code qui crée la nouvelle colonne. La méthode `Up` crée la colonne lorsque vous implémentez la modification, et la méthode `Down` supprime la colonne lorsque vous restaurez la modification.

    ![AddBirthDate_migration_code](deploying-a-database-update/_static/image1.png)
4. Générez la solution, puis entrez la commande suivante dans la fenêtre de la **console du gestionnaire de package** (Assurez-vous que le projet CONTOSOUNIVERSITY. dal est toujours sélectionné) :

    [!code-powershell[Main](deploying-a-database-update/samples/sample4.ps1)]

    Le Entity Framework exécute la méthode `Up`, puis exécute la méthode `Seed`.

### <a name="display-the-new-column-in-the-instructors-page"></a>Afficher la nouvelle colonne dans la page des enseignants

1. Dans le projet ContosoUniversity, ouvrez *Instructors. aspx* et ajoutez un nouveau champ de modèle pour afficher la date de naissance. Ajoutez-le entre ceux pour la date d’embauche et l’attribution de bureau :

    [!code-aspx[Main](deploying-a-database-update/samples/sample5.aspx?highlight=9-17)]

    (Si la mise en retrait du code est désynchronisée, vous pouvez appuyer sur CTRL-K, puis sur CTRL + D pour reformater automatiquement le fichier.)
2. Exécutez l’application et cliquez sur le lien **enseignants** .

    Lorsque la page se charge, vous voyez qu’elle contient le champ nouvelle date de naissance.

    ![Page des enseignants avec DateNaissance](deploying-a-database-update/_static/image2.png)
3. Fermez le navigateur.

### <a name="deploy-the-database-update"></a>Déployer la mise à jour de la base de données

1. Dans **Explorateur de solutions** sélectionnez le projet ContosoUniversity.
2. Dans la barre d’outils **publication Web en un clic** , cliquez sur le profil de publication de **test** , puis sur **publier le site Web**. (Si la barre d’outils est désactivée, sélectionnez le projet ContosoUniversity dans **Explorateur de solutions**.)

    Visual Studio déploie l’application mise à jour et le navigateur s’ouvre sur la page d’hébergement.
3. Exécutez la page des **enseignants** pour vérifier que la mise à jour a été correctement déployée.

    Lorsque l’application tente d’accéder à la base de données pour cette page, Code First met à jour le schéma de la base de données et exécute la méthode `Seed`. Lorsque la page s’affiche, la colonne de **Date de naissance** attendue contient des dates.
4. Dans la barre d’outils **publication Web en un clic** , cliquez sur le profil de publication **intermédiaire** , puis sur **publier le site Web**.
5. Exécutez la page des **instructeurs** dans l’environnement intermédiaire pour vérifier que la mise à jour a été correctement déployée.
6. Dans la barre d’outils **publication Web en un clic** , cliquez sur le profil de publication de **production** , puis cliquez sur **publier le site Web**.
7. Exécutez la page des **enseignants** en production pour vérifier que la mise à jour a été correctement déployée.

    Pour une mise à jour réelle de l’application de production qui comprend une modification de base de données, vous devez également mettre l’application hors connexion au cours du déploiement à l’aide de l’application *\_offline. htm*, comme vous l’avez vu dans le didacticiel précédent.

## <a name="deploy-a-database-update-by-using-the-dbdacfx-provider"></a>Déployer une mise à jour de base de données à l’aide du fournisseur dbDacFx

Dans cette section, vous allez ajouter une colonne *Commentaires* à la table *utilisateur* de la base de données d’appartenance et créer une page qui vous permet d’afficher et de modifier des commentaires pour chaque utilisateur. Ensuite, vous déployez les modifications apportées à test, intermédiaire et production.

### <a name="add-a-column-to-a-table-in-the-membership-database"></a>Ajouter une colonne à une table dans la base de données d’appartenance

1. Dans Visual Studio, ouvrez **Explorateur d’objets SQL Server**.
2. Développez (base de données locale **) \v11.0**, développez **bases de données**, développez **ASPNET-ContosoUniversity** ( **et non ASPNET-ContosoUniversity-prod**), puis développez **tables**.

    Si vous ne voyez pas la zone **(\v11.0)** sous le nœud **SQL Server** , cliquez avec le bouton droit sur le nœud **SQL Server** , puis cliquez sur **Ajouter un SQL Server**. Dans la boîte de dialogue **se connecter au serveur** , entrez (base de données locale *) \v11.0* comme **nom de serveur**, puis cliquez sur **se connecter**.

    Si vous ne voyez pas **ASPNET-ContosoUniversity**, exécutez le projet et connectez-vous à l’aide des informations d’identification de l' *administrateur* (le mot de passe est *devpwd*), puis actualisez la fenêtre de **Explorateur d’objets SQL Server** .
3. Cliquez avec le bouton droit sur le tableau **utilisateurs** , puis cliquez sur **Concepteur de vues**.

    ![Concepteur de vues SSOX](deploying-a-database-update/_static/image3.png)
4. Dans le concepteur, ajoutez une colonne de *Commentaires* et définissez-la comme *nvarchar (128)* et Nullable, puis cliquez sur **mettre à jour**.

    ![Ajout d’une colonne de commentaires](deploying-a-database-update/_static/image4.png)
5. Dans la zone **aperçu des mises à jour de la base de données** , cliquez sur **mettre à jour la base de données**.

    ![Aperçu des mises à jour de la base de données](deploying-a-database-update/_static/image5.png)

### <a name="create-a-page-to-display-and-edit-the-new-column"></a>Créer une page pour afficher et modifier la nouvelle colonne

1. Dans **Explorateur de solutions**, cliquez avec le bouton droit sur le dossier du **compte** dans le projet ContosoUniversity, cliquez sur **Ajouter**, puis sur **nouvel élément**.
2. Créez un nouveau **formulaire Web à l’aide de la page maître** et nommez-le *UserInfo. aspx*. Acceptez le fichier *site. Master* par défaut en tant que page maître.
3. Copiez le balisage suivant dans l’élément `MainContent` `Content` (le dernier des 3 éléments `Content`) :

    [!code-aspx[Main](deploying-a-database-update/samples/sample6.aspx)]
4. Cliquez avec le bouton droit sur la page *UserInfo. aspx* , puis cliquez sur **afficher dans le navigateur**.
5. Connectez-vous avec vos informations d’identification de l’utilisateur *administrateur* (le mot de passe est *devpwd*) et ajoutez des commentaires à l’utilisateur pour vérifier que la page fonctionne correctement.

    ![Page UserInfo](deploying-a-database-update/_static/image6.png)
6. Fermez le navigateur.

## <a name="deploy-the-database-update"></a>Déployer la mise à jour de la base de données

Pour procéder au déploiement à l’aide du fournisseur dbDacFx, il vous suffit de sélectionner l’option **mettre à jour la base de données** dans le profil de publication. Toutefois, pour le déploiement initial lorsque vous avez utilisé cette option, vous avez également configuré des scripts SQL supplémentaires à exécuter : ceux-ci se trouvent toujours dans le profil et vous devrez les empêcher de s’exécuter à nouveau.

1. Ouvrez l’Assistant **publier le site Web** en cliquant avec le bouton droit sur le projet ContosoUniversity, puis en cliquant sur **publier**.
2. Sélectionnez le profil **test** .
3. Cliquez sur l’onglet **paramètres** .
4. Sous **DefaultConnection**, sélectionnez **mettre à jour la base de données**.
5. Désactivez les scripts supplémentaires que vous avez configurés pour s’exécuter pour le déploiement initial :

    1. Cliquez sur **configurer les mises à jour de la base de données**.
    2. Dans la boîte de dialogue **configurer les mises à jour de la base de données** , désactivez les cases à cocher en regard de *Grant. SQL* et *ASPNET-Data-dev. SQL*.
    3. Cliquez sur **Fermer**.
6. Cliquez sur l’onglet **Aperçu** .
7. Sous **bases de données** et à droite de **DefaultConnection**, cliquez sur le lien **aperçu de la base de données** .

    ![Aperçu de la base de données](deploying-a-database-update/_static/image7.png)

    La fenêtre d’aperçu affiche le script qui sera exécuté dans la base de données de destination pour que le schéma de base de données corresponde au schéma de la base de données source. Le script inclut une commande ALTER TABLE qui ajoute la nouvelle colonne.
8. Fermez la boîte **de dialogue Aperçu de la base de données** , puis cliquez sur **publier**.

    Visual Studio déploie l’application mise à jour et le navigateur s’ouvre sur la page d’hébergement.
9. Exécutez la page UserInfo (ajouter *Account/UserInfo. aspx* à l’URL de la page d’hébergement) pour vérifier que la mise à jour a été correctement déployée. Vous devez vous connecter en entrant *admin* et *devpwd*.

    Les données des tables ne sont pas déployées par défaut, et vous n’avez pas configuré de script de déploiement de données à exécuter. vous ne trouverez donc pas le commentaire que vous avez ajouté au développement. Vous pouvez ajouter un nouveau commentaire maintenant dans l’environnement intermédiaire pour vérifier que la modification a été déployée dans la base de données et que la page fonctionne correctement.
10. Suivez la même procédure pour effectuer le déploiement dans un environnement intermédiaire et de production.

    N’oubliez pas de désactiver les scripts supplémentaires. La seule différence par rapport au profil test est que vous ne désactivez qu’un seul script dans les profils intermédiaire et de production, car ils ont été configurés pour s’exécuter uniquement *ASPNET-prod-Data. SQL*.

    Les informations d’identification pour la mise en lots et la production sont admin et prodpwd.

    Pour une mise à jour réelle de l’application de production qui comprend une modification de base de données, vous devez également mettre l’application hors connexion pendant le déploiement en téléchargeant l’application *\_offline. htm* avant de la publier et de la supprimer, comme vous l’avez vu dans [le didacticiel précédent](deploying-a-code-update.md).

## <a name="summary"></a>Récapitulatif

Vous avez maintenant déployé une mise à jour d’application qui comprenait une modification de base de données à l’aide des Migrations Code First et du fournisseur dbDacFx.

![Page des enseignants avec DateNaissance](deploying-a-database-update/_static/image8.png)

![Page UserInfo](deploying-a-database-update/_static/image9.png)

Le didacticiel suivant vous montre comment exécuter des déploiements à l’aide de la ligne de commande.

> [!div class="step-by-step"]
> [Précédent](deploying-a-code-update.md)
> [Suivant](command-line-deployment.md)
