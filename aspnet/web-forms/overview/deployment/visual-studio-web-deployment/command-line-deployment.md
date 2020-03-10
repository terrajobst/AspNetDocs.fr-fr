---
uid: web-forms/overview/deployment/visual-studio-web-deployment/command-line-deployment
title: 'Déploiement Web ASP.NET à l’aide de Visual Studio : déploiement à partir de la ligne de commande | Microsoft Docs'
author: tdykstra
description: Cette série de didacticiels vous montre comment déployer (publier) une application Web ASP.NET sur Azure App Service Web Apps ou sur un fournisseur d’hébergement tiers, par utilisez...
ms.author: riande
ms.date: 02/15/2013
ms.assetid: 82b8dea0-f062-4ee4-8784-3ffa30fbb1ca
msc.legacyurl: /web-forms/overview/deployment/visual-studio-web-deployment/command-line-deployment
msc.type: authoredcontent
ms.openlocfilehash: 13cfe4492398b59f2c80394689cc113ccb218c60
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78630920"
---
# <a name="aspnet-web-deployment-using-visual-studio-command-line-deployment"></a>Déploiement Web ASP.NET à l’aide de Visual Studio : déploiement à partir de la ligne de commande

par [Tom Dykstra](https://github.com/tdykstra)

[Télécharger le projet de démarrage](https://go.microsoft.com/fwlink/p/?LinkId=282627)

> Cette série de didacticiels vous montre comment déployer (publier) une application Web ASP.NET sur Azure App Service Web Apps ou sur un fournisseur d’hébergement tiers, à l’aide de Visual Studio 2012 ou de Visual Studio 2010. Pour plus d’informations sur la série, consultez [le premier didacticiel de la série](introduction.md).

## <a name="overview"></a>Présentation

Ce didacticiel vous montre comment appeler le pipeline de publication Web de Visual Studio à partir de la ligne de commande. Cela est utile pour les scénarios où vous souhaitez [automatiser le processus de déploiement](../../../../aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/continuous-integration-and-continuous-delivery.md) au lieu de le faire manuellement dans Visual Studio, généralement à l’aide d’un [système de contrôle de version de code source](../../../../aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/source-control.md).

## <a name="make-a-change-to-deploy"></a>Apporter une modification au déploiement

Actuellement, la page about affiche le code du modèle.

![Page about avec code de modèle](command-line-deployment/_static/image1.png)

Vous allez remplacer cela par du code qui affiche un résumé de l’inscription d’un étudiant.

Ouvrez la page *about. aspx* , supprimez tout le balisage à l’intérieur de l’élément `MainContent` `Content` et insérez le balisage suivant à la place :

[!code-aspx[Main](command-line-deployment/samples/sample1.aspx)]

Exécutez le projet et sélectionnez la page **à propos** de.

![Page About](command-line-deployment/_static/image2.png)

## <a name="deploy-to-test-by-using-the-command-line"></a>Déployer pour effectuer des tests à l’aide de la ligne de commande

Si vous ne déployez pas une autre modification de base de données, désactivez le déploiement de base de données dbDacFx pour la base de données ASPNET-ContosoUniversity. Ouvrez l’Assistant **publier le site Web** et, dans chacun des trois profils de publication, désactivez la case à cocher **mettre à jour la base de données** sous l’onglet **paramètres** .

Dans la page de démarrage de Windows 8, recherchez **invite de commandes développeur pour VS2012**.

Cliquez avec le bouton droit sur l’icône de **invite de commandes développeur pour VS2012** , puis cliquez sur **exécuter en tant qu’administrateur**.

Entrez la commande suivante à l’invite de commandes, en remplaçant le chemin d’accès au fichier solution par le chemin d’accès à votre fichier solution :

[!code-console[Main](command-line-deployment/samples/sample2.cmd)]

MSBuild génère la solution et la déploie dans l’environnement de test.

![Sortie de la ligne de commande](command-line-deployment/_static/image3.png)

Ouvrez un navigateur et accédez à `http://localhost/ContosoUniversity`, puis cliquez sur la page à **propos** de pour vérifier que le déploiement a réussi.

Si vous n’avez créé aucun étudiant dans le cadre d’un test, vous verrez une page vide sous le titre **statistiques du corps** de l’étudiant. Accédez à la page des **étudiants** , cliquez sur **Ajouter un élève**, ajoutez des élèves, puis revenez à la page à **propos** de pour afficher les statistiques des étudiants.

![Page à propos de dans l’environnement de test](command-line-deployment/_static/image4.png)

## <a name="key-command-line-options"></a>Options de ligne de commande de clé

La commande que vous avez entrée a passé le chemin d’accès au fichier solution et deux propriétés à MSBuild :

[!code-console[Main](command-line-deployment/samples/sample3.cmd)]

### <a name="deploying-the-solution-versus-deploying-individual-projects"></a>Déploiement de la solution et déploiement de projets individuels

Si vous spécifiez le fichier solution, tous les projets de la solution sont générés. Si vous avez plusieurs projets Web dans la solution, le comportement MSBuild suivant s’applique :

- Les propriétés que vous spécifiez sur la ligne de commande sont passées à chaque projet. Par conséquent, chaque projet Web doit avoir un profil de publication portant le nom que vous spécifiez. Si vous spécifiez `/p:PublishProfile=Test`, chaque projet Web doit avoir un profil de publication nommé *test*.
- Vous pouvez publier un projet avec succès alors qu’un autre n’est pas encore généré. Pour plus d’informations, consultez le thread StackOverflow [MSBuild échoue avec deux packages](http://stackoverflow.com/questions/14226451/msbuild-fails-with-two-packages).

Si vous spécifiez un projet individuel plutôt qu’une solution, vous devez ajouter un paramètre qui spécifie la version de Visual Studio. Si vous utilisez Visual Studio 2012, la ligne de commande est similaire à l’exemple suivant :

[!code-console[Main](command-line-deployment/samples/sample4.cmd?highlight=1)]

Le numéro de version de Visual Studio 2010 est 10,0. Pour plus d’informations, consultez [compatibilité des projets Visual Studio et VisualStudioVersion](http://sedodream.com/2012/08/19/VisualStudioProjectCompatabilityAndVisualStudioVersion.aspx) sur le blog de Sayed Hashimi.

### <a name="specifying-the-publish-profile"></a>Spécification du profil de publication

Vous pouvez spécifier le profil de publication par nom ou par le chemin d’accès complet au fichier *. pubxml* , comme indiqué dans l’exemple suivant :

[!code-console[Main](command-line-deployment/samples/sample5.cmd?highlight=1)]

### <a name="web-publish-methods-supported-for-command-line-publishing"></a>Méthodes de publication Web prises en charge pour la publication à partir de la ligne de commande

Trois méthodes de publication sont prises en charge pour la publication à partir de la ligne de commande :

- `MSDeploy`-publiez à l’aide de Web Deploy.
- `Package`-publiez en créant un package Web Deploy. Vous devez installer le package séparément de la commande MSBuild qui le crée.
- `FileSystem`-publier en copiant les fichiers dans un dossier spécifié.

### <a name="specifying-the-build-configuration-and-platform"></a>Spécification de la configuration et de la plateforme de build

La configuration et la plateforme de build doivent être définies dans Visual Studio ou sur la ligne de commande. Les profils de publication incluent des propriétés nommées `LastUsedBuildConfiguration` et `LastUsedPlatform`, mais vous ne pouvez pas définir ces propriétés afin de déterminer la façon dont le projet est généré. Pour plus d’informations, consultez [MSBuild : comment définir la propriété de configuration sur le](http://sedodream.com/2012/10/27/MSBuildHowToSetTheConfigurationProperty.aspx) blog de Sayed Hashimi.

## <a name="deploy-to-staging"></a>Déployer vers un environnement intermédiaire

Pour effectuer un déploiement sur Azure, vous devez ajouter le mot de passe à la ligne de commande. Si vous avez enregistré le mot de passe dans le profil de publication dans Visual Studio, il est stocké sous forme chiffrée dans le fichier *. pubxml. User* . MSBuild n’accède pas à ce fichier quand vous effectuez un déploiement de ligne de commande. vous devez donc passer le mot de passe dans un paramètre de ligne de commande.

1. Copiez le mot de passe dont vous avez besoin à partir du fichier *. publishsettings* que vous avez téléchargé précédemment pour le site Web intermédiaire. Le mot de passe est la valeur de l’attribut `userPWD` pour l’élément `publishProfile` Web Deploy.

    ![Mot de passe Web Deploy](command-line-deployment/_static/image5.png)
2. Dans la page de démarrage de Windows 8, recherchez **invite de commandes développeur pour VS2012**, puis cliquez sur l’icône pour ouvrir l’invite de commandes. (Vous n’avez pas besoin de l’ouvrir en tant qu’administrateur cette fois, car vous n’êtes pas déployé sur IIS sur l’ordinateur local.)
3. Entrez la commande suivante à l’invite de commandes, en remplaçant le chemin d’accès au fichier solution par le chemin d’accès à votre fichier de solution et par le mot de passe avec votre mot de passe :

    [!code-console[Main](command-line-deployment/samples/sample6.cmd)]

    Notez que cette ligne de commande comprend un paramètre supplémentaire : `/p:AllowUntrustedCertificate=true`. Lors de l’écriture de ce didacticiel, la propriété `AllowUntrustedCertificate` doit être définie lors de la publication sur Azure à partir de la ligne de commande. Lorsque le correctif pour ce bogue est publié, vous n’avez pas besoin de ce paramètre.
4. Ouvrez un navigateur et accédez à l’URL de votre site intermédiaire, puis cliquez sur la page à **propos** de pour vérifier que le déploiement a réussi.

    Comme vous l’avez vu précédemment pour l’environnement de test, vous devrez peut-être créer des élèves pour afficher des statistiques sur la page **à propos** de.

## <a name="deploy-to-production"></a>Déployer en production

Le processus de déploiement en production est similaire au processus de mise en lots.

1. Copiez le mot de passe dont vous avez besoin à partir du fichier *. publishsettings* que vous avez téléchargé précédemment pour le site Web de production.
2. Ouvrez **invite de commandes développeur pour VS2012**.
3. Entrez la commande suivante à l’invite de commandes, en remplaçant le chemin d’accès au fichier solution par le chemin d’accès à votre fichier de solution et par le mot de passe avec votre mot de passe :

    [!code-console[Main](command-line-deployment/samples/sample7.cmd)]

    Pour un site de production réel, en cas de modification d’une base de données, vous devez généralement copier l' *application\_fichier offline. htm* sur le site avant le déploiement et la supprimer après un déploiement réussi.
4. Ouvrez un navigateur et accédez à l’URL de votre site intermédiaire, puis cliquez sur la page à **propos** de pour vérifier que le déploiement a réussi.

## <a name="summary"></a>Récapitulatif

Vous avez maintenant déployé une mise à jour d’application à l’aide de la ligne de commande.

![Page à propos de dans l’environnement de test](command-line-deployment/_static/image6.png)

Dans le didacticiel suivant, vous verrez un exemple d’extension du pipeline de publication Web. L’exemple vous montre comment déployer des fichiers qui ne sont pas inclus dans le projet.

> [!div class="step-by-step"]
> [Précédent](deploying-a-database-update.md)
> [Suivant](deploying-extra-files.md)
