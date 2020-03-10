---
uid: web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12
title: 'Déploiement d’une application Web ASP.NET avec SQL Server Compact à l’aide de Visual Studio ou Visual Web Developer : déploiement d’une mise à jour de code uniquement-8 sur 12 | Microsoft Docs'
author: tdykstra
description: Cette série de didacticiels vous montre comment déployer (publier) un projet d’application Web ASP.NET qui comprend une base de données SQL Server Compact à l’aide de Stu...
ms.author: riande
ms.date: 11/17/2011
ms.assetid: ddf6252f-9413-4c0c-a360-2cef8d231717
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12
msc.type: authoredcontent
ms.openlocfilehash: e4d094ef84a747c36ce05ddb0e3d1ce0391d5605
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78564406"
---
# <a name="deploying-an-aspnet-web-application-with-sql-server-compact-using-visual-studio-or-visual-web-developer-deploying-a-code-only-update---8-of-12"></a>Déploiement d’une application Web ASP.NET avec SQL Server Compact à l’aide de Visual Studio ou Visual Web Developer : déploiement d’une mise à jour de code uniquement-8 sur 12

par [Tom Dykstra](https://github.com/tdykstra)

[Télécharger le projet de démarrage](https://code.msdn.microsoft.com/Deploying-an-ASPNET-Web-4e31366b)

> Cette série de didacticiels vous montre comment déployer (publier) un projet d’application Web ASP.NET qui comprend une base de données SQL Server Compact à l’aide de Visual Studio 2012 RC ou Visual Studio Express 2012 RC pour le Web. Vous pouvez également utiliser Visual Studio 2010 si vous installez la mise à jour de publication Web. Pour obtenir une présentation de la série, consultez [le premier didacticiel de la série](deployment-to-a-hosting-provider-introduction-1-of-12.md).
> 
> Pour obtenir un didacticiel qui présente les fonctionnalités de déploiement introduites après la version RC de Visual Studio 2012, montre comment déployer des éditions SQL Server autres que SQL Server Compact et montre comment déployer vers Azure App Service Web Apps, consultez [déploiement Web ASP.net à l’aide de Visual Studio](../../deployment/visual-studio-web-deployment/introduction.md).

## <a name="overview"></a>Présentation

Après le déploiement initial, votre travail de maintenance et de développement de votre site Web continue, et avant que vous ne souhaitiez déployer une mise à jour. Ce didacticiel vous guide tout au long du processus de déploiement d’une mise à jour de votre code d’application. Cette mise à jour n’implique pas de modification de la base de données. Pour plus d’informations sur le déploiement d’une modification de base de données, consultez le didacticiel suivant.

Rappel : Si vous recevez un message d’erreur ou si une action ne fonctionne pas au fur et à mesure que vous parcourez le didacticiel, veillez à consulter la [page de résolution des problèmes](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12.md).

## <a name="making-a-code-change"></a>Modification du code

En guise d’exemple simple de mise à jour de votre application, vous allez ajouter à la page des **enseignants** une liste de cours traités par l’instructeur sélectionné.

Si vous exécutez la **page des** formateurs, vous remarquerez qu’il y a des liens **sélectionnés** dans la grille, mais qu’ils ne font rien d’autre que de rendre l’arrière-plan de la ligne grisé.

[![Instructors_page](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12/_static/image2.png)](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12/_static/image1.png)

À présent, vous allez ajouter le code qui s’exécute lorsque vous cliquez sur le lien **Sélectionner** et afficher la liste des cours traités par l’instructeur sélectionné.

Dans *Instructors. aspx*, ajoutez le balisage suivant juste après le contrôle **ErrorMessageLabel** `Label` :

[!code-aspx[Main](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12/samples/sample1.aspx)]

Exécutez la page et sélectionnez un formateur. Vous voyez une liste des cours enseignés par cet instructeur.

[![Instructors_page_with_courses](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12/_static/image4.png)](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12/_static/image3.png)

## <a name="deploying-the-code-update-to-the-test-environment"></a>Déploiement de la mise à jour du code dans l’environnement de test

Le déploiement sur l’environnement de test est un simple problème d’exécution de la publication en un clic. Pour accélérer ce processus, vous pouvez utiliser la barre d’outils de **publication Web en un clic** .

Dans le menu **affichage** , choisissez **barres d’outils** , puis sélectionnez **publier sur le Web**.

![Selecting_One_Click_Publish_toolbar](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12/_static/image5.png)

Dans **Explorateur de solutions**, sélectionnez le projet ContosoUniversity.

la barre d’outils **publication Web en un clic** , choisissez le profil de publication de **test** , puis cliquez sur **publier le site Web** (l’icône avec des flèches pointant vers la gauche et la droite).

![Web_One_Click_Publish_toolbar](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12/_static/image6.png)

Visual Studio déploie l’application mise à jour et le navigateur s’ouvre automatiquement sur la page d’hébergement. Exécutez la page enseignants et sélectionnez un formateur pour vérifier que la mise à jour a été correctement déployée.

[![Instructors_page_with_courses_Test](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12/_static/image8.png)](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12/_static/image7.png)

En règle générale, vous effectuez des tests de régression (autrement dit, vous testez le reste du site pour vous assurer que la nouvelle modification n’a pas permis de rompre les fonctionnalités existantes). Mais pour ce didacticiel, vous allez ignorer cette étape et procéder au déploiement de la mise à jour en production.

## <a name="preventing-redeployment-of-the-initial-database-state-to-production"></a>Empêcher le redéploiement de l’état initial de la base de données en production

Dans une application réelle, les utilisateurs interagissent avec votre site de production après votre déploiement initial, et les bases de données sont remplies avec des données actives. Par conséquent, vous ne souhaitez pas redéployer la base de données d’appartenance dans son état initial, ce qui efface toutes les données actives. Étant donné que les bases de données SQL Server Compact sont des fichiers du dossier *application\_Data* , vous devez éviter cela en modifiant les paramètres de déploiement afin que les fichiers du dossier *application\_Data* ne soient pas déployés.

Ouvrez la fenêtre **Propriétés du projet** pour le projet ContosoUniversity, puis sélectionnez l’onglet **Package/Publication Web** . Assurez-vous que la zone de liste déroulante **configuration** a la valeur **actif (version finale)** ou **mise en sortie** sélectionnée, puis sélectionnez **exclure les fichiers du dossier de données de l’application\_** .

![Exclude_files_from_the_App_Data_folder](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12/_static/image9.png)

Si vous décidez de déployer une version de débogage à l’avenir, il est judicieux d’apporter la même modification à la configuration de build Debug : modifiez la **configuration** pour **Déboguer** , puis sélectionnez **exclure les fichiers du dossier de données de l’application\_** .

Enregistrez et fermez l’onglet **Package/Publication Web** .

> [!NOTE] 
> 
> [!IMPORTANT]
> Assurez-vous que vous n’avez pas **supprimé des fichiers supplémentaires à la destination** sélectionnée dans vos profils de publication. Si vous sélectionnez cette option, le processus de déploiement supprime les bases de données que vous avez dans l’application\_données du site déployé, et supprime le dossier de données de l’application\_lui-même.

## <a name="preventing-user-access-to-the-production-site-during-update"></a>Empêcher l’accès des utilisateurs au site de production pendant la mise à jour

La modification que vous déployez maintenant est une simple modification apportée à une seule page. Mais parfois, vous déployez des modifications plus importantes et, dans ce cas, le site peut se comporter de façon étrange si un utilisateur demande une page avant que le déploiement ne soit terminé. Pour éviter cela, vous pouvez utiliser une *application\_fichier offline. htm* . Quand vous placez un fichier nommé *app\_offline. htm* dans le dossier racine de votre application, IIS affiche automatiquement ce fichier au lieu d’exécuter votre application. Pour empêcher l’accès pendant le déploiement, vous placez l' *application\_offline. htm* dans le dossier racine, vous exécutez le processus de déploiement, puis vous supprimez l' *application\_offline. htm*.

Dans **Explorateur de solutions**, cliquez avec le bouton droit sur la solution (et non sur l’un des projets) et sélectionnez **nouveau dossier de solution**.

![Creating_a_solution_folder](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12/_static/image10.png)

Nommez le dossier *SolutionFiles*.

Dans le nouveau dossier, créez une page HTML nommée *app\_offline. htm*. Remplacez le contenu existant par le balisage suivant :

[!code-html[Main](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12/samples/sample2.html)]

Vous pouvez copier l' *application\_fichier. htm hors connexion* sur le site à l’aide d’une connexion FTP ou de l’utilitaire **Gestionnaire de fichiers** dans le panneau de configuration du fournisseur d’hébergement. Pour ce didacticiel, vous allez utiliser le **Gestionnaire de fichiers**.

Ouvrez le panneau de configuration et sélectionnez **Gestionnaire de fichiers** comme vous l’avez fait dans le didacticiel [déploiement dans l’environnement de production](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12.md) . Sélectionnez **contosouniversity.com** , puis **wwwroot** pour accéder au dossier racine de votre application, puis cliquez sur **Télécharger**.

[![Upload_button_in_File_Manager](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12/_static/image12.png)](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12/_static/image11.png)

Dans la boîte de dialogue **Télécharger un fichier** , sélectionnez le fichier *app\_offline. htm* , puis cliquez sur **Télécharger**.

[![Upload_dialog_box_in_File_Manager](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12/_static/image14.png)](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12/_static/image13.png)

Accédez à l'URL de votre site. Vous voyez que la page *application\_offline. htm* s’affiche désormais à la place de votre page d’hébergement.

[![App_offline. htm_page_in_production](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12/_static/image16.png)](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12/_static/image15.png)

Vous êtes maintenant prêt à effectuer le déploiement en production.

## <a name="deploying-the-code-update-to-the-production-environment"></a>Déploiement de la mise à jour du code dans l’environnement de production

Dans la barre d’outils **publication Web en un clic** , choisissez le profil de publication de **production** , puis cliquez sur **publier le site Web**.

Visual Studio déploie l’application mise à jour et ouvre le navigateur sur la page d’hébergement du site. L' *application\_fichier. htm hors connexion* s’affiche. Avant de pouvoir effectuer un test pour vérifier que le déploiement a réussi, vous devez supprimer l' *application\_fichier offline. htm* .

Revenez à l’application **Gestionnaire de fichiers** dans le panneau de configuration. Sélectionnez **contosouniversity.com** et **wwwroot**, sélectionnez **app\_offline. htm**, puis cliquez sur **supprimer**.

[![Deleting_app_offline. htm](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12/_static/image18.png)](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12/_static/image17.png)

Dans le navigateur, ouvrez la page des enseignants dans le site public et sélectionnez un formateur pour vérifier que la mise à jour a été correctement déployée.

[![Instructors_page_with_courses_Prod](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12/_static/image20.png)](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12/_static/image19.png)

Vous avez maintenant déployé une mise à jour d’application qui n’impliquait pas de modification de base de données. Le didacticiel suivant vous montre comment déployer une modification de base de données.

> [!div class="step-by-step"]
> [Précédent](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12.md)
> [Suivant](deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12.md)
