---
uid: web-forms/overview/deployment/web-deployment-in-the-enterprise/setting-up-the-contact-manager-solution
title: Configuration de la solution du gestionnaire de contacts | Microsoft Docs
author: jrjlee
description: Cette rubrique explique comment télécharger et configurer la solution gestionnaire de contacts pour une exécution locale sur une station de travail de développeur.
ms.author: riande
ms.date: 05/04/2012
ms.assetid: 200b973c-776b-4a9b-9e82-39fda6120a52
msc.legacyurl: /web-forms/overview/deployment/web-deployment-in-the-enterprise/setting-up-the-contact-manager-solution
msc.type: authoredcontent
ms.openlocfilehash: d9774ee01cb0515d7e733b24baa661f2648bd7c4
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78629856"
---
# <a name="setting-up-the-contact-manager-solution"></a>Configuration de la solution Gestionnaire de contacts

par [Jason Lee](https://github.com/jrjlee)

[Télécharger PDF](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> Cette rubrique explique comment télécharger et configurer la solution gestionnaire de contacts pour une exécution locale sur une station de travail de développeur.

## <a name="system-requirements"></a>Configuration système requise

Pour exécuter la solution du gestionnaire de contacts localement et effectuer les autres tâches décrites dans ce didacticiel, vous devez installer ce logiciel sur votre station de travail de développeur :

- Visual Studio 2010 Service Pack 1, édition Premium ou édition intégrale
- Services Internet (IIS) 7.5 Express
- SQL Server Express 2008 R2
- Outil de déploiement Web IIS (Web Deploy) 2,1 ou version ultérieure
- ASP.NET 4,0
- ASP.NET MVC
- .NET Framework 4
- .NET Framework 3.5 SP1

À l’exception de Visual Studio 2010, vous pouvez télécharger et installer les dernières versions de tous ces produits et composants par le biais du [Web Platform Installer](https://go.microsoft.com/?linkid=9805118).

## <a name="download-and-extract-the-solution"></a>Télécharger et extraire la solution

Vous pouvez télécharger l’exemple d’application du gestionnaire de contacts à partir de la Galerie de code MSDN [ici](https://code.msdn.microsoft.com/Deploying-Web-Applications-9d9093c0).

## <a name="configure-and-run-the-solution"></a>Configuration et exécution de la solution

Pour configurer et exécuter la solution gestionnaire de contacts sur votre ordinateur local, vous devez effectuer les étapes principales suivantes :

1. Si vous n’en avez pas déjà un, créez une base de données des services d’application ASP.NET locale avec les fonctionnalités d’appartenance et de gestion des rôles activées.
2. Modifiez les chaînes de connexion dans les fichiers *Web. config* pour pointer vers votre instance de SQL Server Express locale.
3. Exécutez la solution à partir de Visual Studio 2010.

Le reste de cette section fournit des conseils supplémentaires sur la façon d’effectuer chacune de ces tâches.

**Pour créer la base de données des services d’application**

1. Ouvrez une invite de commandes de Visual Studio 2010. Pour ce faire, dans le menu **Démarrer** , pointez **sur tous les programmes**, cliquez sur **Microsoft Visual Studio 2010**, sur **Visual Studio Tools**, puis sur invite de **commandes de Visual Studio (2010)** .
2. À l’invite de commandes, tapez la commande suivante, puis appuyez sur entrée :

    [!code-console[Main](setting-up-the-contact-manager-solution/samples/sample1.cmd)]

    1. Utilisez le commutateur **– C** pour spécifier la chaîne de connexion de votre serveur de base de données.
    2. Utilisez le commutateur **– A** pour spécifier les fonctionnalités des services d’application que vous souhaitez ajouter à la base de données. Dans ce cas, **m** indique que vous souhaitez ajouter la prise en charge pour le fournisseur d’appartenances et **r** indique que vous souhaitez ajouter la prise en charge du gestionnaire de rôles.
    3. Utilisez le commutateur **– d** pour spécifier un nom pour votre base de données de services d’application. Si vous omettez ce commutateur, l’utilitaire crée une base de données avec le nom par défaut **aspnetdb**.
3. Lorsque la base de données a été créée avec succès, l’invite de commandes affiche une confirmation.

    ![](setting-up-the-contact-manager-solution/_static/image1.png)

> [!NOTE]
> Pour plus d’informations sur l’utilitaire ASPNET\_RegSql, consultez [ASP.NET SQL Server Registration Tool (aspnet\_RegSql. exe)](https://msdn.microsoft.com/library/ms229862(v=vs.100).aspx).

L’étape suivante consiste à s’assurer que les chaînes de connexion dans la solution du gestionnaire de contacts pointent vers votre instance locale de SQL Server Express.

**Pour mettre à jour les chaînes de connexion**

1. Ouvrez la solution gestionnaire de contacts dans Visual Studio 2010.
2. Dans la fenêtre **Explorateur de solutions** , développez le projet **ContactManager. Mvc** , puis double-cliquez sur le nœud **Web. config** .

    > [!NOTE]
    > Le projet ContactManager. Mvc comprend deux fichiers *Web. config* . Vous devez modifier le fichier au niveau du projet.

    ![](setting-up-the-contact-manager-solution/_static/image2.png)
3. Dans l’élément **connectionStrings** , vérifiez que la chaîne de connexion nommée **ApplicationServices** pointe vers votre base de données ASP.net application services locale.

    [!code-xml[Main](setting-up-the-contact-manager-solution/samples/sample2.xml)]
4. Dans la fenêtre **Explorateur de solutions** , développez le projet **ContactManager. service** , puis double-cliquez sur le nœud **Web. config** .

    ![](setting-up-the-contact-manager-solution/_static/image3.png)
5. Dans l’élément **connectionStrings** , dans la chaîne de connexion nommée **ContactManagerContext**, vérifiez que la propriété de la **source de données** est définie sur votre instance locale de SQL Server Express. Vous n’avez pas besoin de modifier quoi que ce soit d’autre dans la chaîne de connexion.

    [!code-xml[Main](setting-up-the-contact-manager-solution/samples/sample3.xml)]
6. Enregistrez tous les fichiers ouverts.

Vous devez maintenant être prêt à exécuter la solution du gestionnaire de contacts sur votre ordinateur local.

> [!NOTE]
> Si vous suivez ces étapes sans créer au préalable une base de données de services d’application, ASP.NET crée la base de données la première fois que vous tentez de créer un utilisateur. Toutefois, la création manuelle de la base de données vous donne beaucoup plus de contrôle sur l’ensemble de fonctionnalités des services d’application que vous souhaitez prendre en charge.

**Pour exécuter la solution du gestionnaire de contacts**

1. Dans Visual Studio 2010, appuyez sur F5.
2. Internet Explorer démarre et demande l’URL de l’application ASP.NET MVC 3 du gestionnaire de contacts. Par défaut, l’application affiche la page **tous les contacts** .

    ![](setting-up-the-contact-manager-solution/_static/image4.png)
3. Ajoutez quelques contacts, puis vérifiez que l’application fonctionne comme prévu.

    ![](setting-up-the-contact-manager-solution/_static/image5.png)
4. Accédez à `http://localhost:50114/Account/Register` (ajustez l’URL si vous hébergez l’application sur un autre port). Ajoutez un nom d’utilisateur, une adresse de messagerie et un mot de passe, puis vérifiez que vous êtes en mesure d’inscrire correctement un compte.

    ![](setting-up-the-contact-manager-solution/_static/image6.png)
5. Accédez à `http://localhost:50114/Account/LogOn` (ajustez l’URL si vous hébergez l’application sur un autre port). Vérifiez que vous êtes en mesure d’ouvrir une session à l’aide du compte que vous venez de créer.

    ![](setting-up-the-contact-manager-solution/_static/image7.png)
6. Fermez Internet Explorer pour arrêter le débogage.

## <a name="conclusion"></a>Conclusion

À ce stade, la solution du gestionnaire de contacts doit être entièrement configurée pour s’exécuter sur votre ordinateur local. Vous pouvez utiliser la solution comme référence lorsque vous travaillez dans les autres rubriques de ce didacticiel.

La rubrique suivante, qui décrit [le fichier projet](understanding-the-project-file.md), explique comment vous pouvez utiliser les fichiers de projet de Microsoft Build Engine personnalisé (MSBuild) dans la solution du gestionnaire de contacts pour contrôler le processus de déploiement.

> [!div class="step-by-step"]
> [Précédent](the-contact-manager-solution.md)
> [Suivant](understanding-the-project-file.md)
