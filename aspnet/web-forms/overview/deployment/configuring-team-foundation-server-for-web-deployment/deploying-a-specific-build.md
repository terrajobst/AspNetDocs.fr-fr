---
uid: web-forms/overview/deployment/configuring-team-foundation-server-for-web-deployment/deploying-a-specific-build
title: Déploiement d’une build spécifique | Microsoft Docs
author: jrjlee
description: Cette rubrique explique comment déployer des packages Web et des scripts de base de données à partir d’une build précédente spécifique vers une nouvelle destination, comme un standard intermédiaire ou de production...
ms.author: riande
ms.date: 05/04/2012
ms.assetid: c979535f-48a3-4ec4-a633-a77889b86ddb
msc.legacyurl: /web-forms/overview/deployment/configuring-team-foundation-server-for-web-deployment/deploying-a-specific-build
msc.type: authoredcontent
ms.openlocfilehash: 6bede6b36c24ade928ab052e14daec1e017bd0b2
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78639670"
---
# <a name="deploying-a-specific-build"></a>Déploiement d’une build spécifique

par [Jason Lee](https://github.com/jrjlee)

[Télécharger PDF](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> Cette rubrique explique comment déployer des packages Web et des scripts de base de données à partir d’une build précédente spécifique vers une nouvelle destination, comme un environnement intermédiaire ou de production.

Cette rubrique fait partie d’une série de didacticiels basés sur les exigences de déploiement en entreprise d’une société fictive nommée Fabrikam, Inc. Cette série de didacticiels utilise un&#x2014;exemple de solution de [gestion des contacts](../web-deployment-in-the-enterprise/the-contact-manager-solution.md)&#x2014;pour représenter une application Web avec un niveau de complexité réaliste, y compris une application ASP.NET MVC 3, un service Windows Communication Foundation (WCF) et un projet de base de données.

La méthode de déploiement au cœur de ces didacticiels est basée sur l’approche de fichier de projet fractionné décrite dans [fonctionnement du fichier projet](../web-deployment-in-the-enterprise/understanding-the-project-file.md), dans lequel le processus de génération et de déploiement est&#x2014;contrôlé par deux fichiers projet qui contiennent des instructions de génération qui s’appliquent à chaque environnement de destination et un qui contient des paramètres de génération et de déploiement spécifiques à l’environnement. Au moment de la génération, le fichier projet spécifique à l’environnement est fusionné dans le fichier projet indépendant de l’environnement pour former un ensemble complet d’instructions de génération.

## <a name="task-overview"></a>Vue d’ensemble des tâches

Jusqu’à présent, les rubriques de ce didacticiel sont axées sur la création, l’empaquetage et le déploiement d’applications et de bases de données Web dans le cadre d’un processus automatisé ou à étape unique. Toutefois, dans certains scénarios courants, vous souhaiterez sélectionner les ressources que vous déployez à partir d’une liste de builds dans un dossier de dépôt. En d’autres termes, la build la plus récente peut ne pas être la build que vous souhaitez déployer.

Examinez le scénario d’intégration continue (CI) décrit dans la rubrique précédente, [création d’une définition de build qui prend en charge le déploiement](creating-a-build-definition-that-supports-deployment.md). Vous avez créé une définition de build dans Team Foundation Server (TFS) 2010. Chaque fois qu’un développeur consulte le code dans TFS, Team Build génère votre code, crée des packages Web et des scripts de base de données dans le cadre du processus de génération, exécute des tests unitaires et déploie vos ressources dans un environnement de test. Selon la stratégie de rétention que vous avez configurée lors de la création de la définition de build, TFS conservera un certain nombre de builds précédentes.

![](deploying-a-specific-build/_static/image1.png)

À présent, supposons que vous avez effectué des tests de vérification et de validation sur l’une de ces builds dans votre environnement de test et que vous êtes prêt à déployer votre application dans un environnement intermédiaire. En attendant, les développeurs peuvent avoir archivé du nouveau code. Vous ne souhaitez pas régénérer la solution et effectuer le déploiement dans l’environnement intermédiaire, et vous ne souhaitez pas déployer la build la plus récente dans l’environnement intermédiaire. Au lieu de cela, vous souhaitez déployer la build spécifique que vous avez vérifiée et validée sur les serveurs de test.

Pour ce faire, vous devez indiquer à l’Microsoft Build Engine (MSBuild) où trouver les packages Web et les scripts de base de données générés par une build spécifique.

## <a name="overriding-the-outputroot-property"></a>Substitution de la propriété OutputRoot

Dans l' [exemple de solution](../web-deployment-in-the-enterprise/the-contact-manager-solution.md), le fichier *Publish. proj* déclare une propriété nommée **OutputRoot**. Comme son nom l’indique, il s’agit du dossier racine qui contient tous les éléments générés par le processus de génération. Dans le fichier *Publish. proj* , vous pouvez voir que la propriété **OutputRoot** fait référence à l’emplacement racine pour toutes les ressources de déploiement.

> [!NOTE]
> **OutputRoot** est un nom de propriété couramment utilisé. Les C# fichiers projet Visual et Visual Basic déclarent également cette propriété pour stocker l’emplacement racine pour toutes les sorties de génération.

[!code-xml[Main](deploying-a-specific-build/samples/sample1.xml)]

Si vous souhaitez que votre fichier projet déploie des packages Web et des scripts de base&#x2014;de données à partir d’un autre emplacement&#x2014;, comme les sorties d’une build TFS précédente, il vous suffit de remplacer la propriété **OutputRoot** . Vous devez définir la valeur de la propriété sur le dossier de build approprié sur le serveur Team Build. Si vous exécutiez MSBuild à partir de la ligne de commande, vous pouvez spécifier une valeur pour **OutputRoot** comme argument de ligne de commande :

[!code-console[Main](deploying-a-specific-build/samples/sample2.cmd)]

Toutefois, dans la pratique, vous souhaiterez également ignorer la cible&#x2014;de génération il n’y a aucun point dans la génération de votre solution si vous n’envisagez pas d’utiliser les sorties de génération. Pour ce faire, vous pouvez spécifier les cibles que vous souhaitez exécuter à partir de la ligne de commande :

[!code-console[Main](deploying-a-specific-build/samples/sample3.cmd)]

Toutefois, dans la plupart des cas, vous souhaiterez créer votre logique de déploiement dans une définition de build TFS. Cela permet aux utilisateurs disposant de l’autorisation **Builds de file d’attente** de déclencher le déploiement à partir d’une installation de Visual Studio avec une connexion au serveur TFS.

## <a name="creating-a-build-definition-to-deploy-specific-builds"></a>Création d’une définition de build pour déployer des builds spécifiques

La procédure suivante décrit comment créer une définition de build qui permet aux utilisateurs de déclencher des déploiements dans un environnement intermédiaire à l’aide d’une seule commande.

Dans ce cas, vous ne souhaitez pas que la définition de build génère&#x2014;réellement tout ce que vous voulez qu’elle exécute la logique de déploiement dans votre fichier projet personnalisé. Le fichier *Publish. proj* contient la logique conditionnelle qui ignore la cible de **génération** si le fichier est en cours d’exécution dans Team Build. Pour ce faire, il évalue la propriété intégrée **BuildingInTeamBuild** , qui prend automatiquement la valeur **true** si vous exécutez votre fichier projet dans Team Build. Par conséquent, vous pouvez ignorer le processus de génération et exécuter simplement le fichier projet pour déployer une build existante.

**Pour créer une définition de build afin de déclencher le déploiement manuellement**

1. Dans Visual Studio 2010, dans la fenêtre **Team Explorer** , développez le nœud de votre projet d’équipe, cliquez avec le bouton droit sur **Builds**, puis cliquez sur **nouvelle définition de build**.

    ![](deploying-a-specific-build/_static/image2.png)
2. Sous l’onglet **général** , attribuez un nom à la définition de build (par exemple, **DeployToStaging**) et une description facultative.
3. Sous l’onglet **déclencheur** , sélectionnez **manuel : les archivages ne déclenchent pas une nouvelle build**.
4. Sous l’onglet **valeurs par défaut des builds** , dans la zone **copier la sortie de la génération dans le dossier de dépôt suivant** , tapez le chemin d’accès UNC (Universal Naming Convention) de votre dossier de dépôt (par exemple, **\\TFSBUILD\Drops**).

    ![](deploying-a-specific-build/_static/image3.png)
5. Sous l’onglet **processus** , dans la liste déroulante **fichier de processus de génération** , laissez le fichier **DefaultTemplate. Xaml** sélectionné. Il s’agit de l’un des modèles de processus de génération par défaut qui sont ajoutés à tous les nouveaux projets d’équipe.
6. Dans le tableau **paramètres du processus de génération** , cliquez sur la ligne **éléments à générer** , puis cliquez sur le bouton de **sélection** .

    ![](deploying-a-specific-build/_static/image4.png)
7. Dans la boîte de dialogue **éléments à générer** , cliquez sur **Ajouter**.
8. Dans la liste déroulante **éléments de type** , sélectionnez **fichiers projet MSBuild**.
9. Accédez à l’emplacement du fichier projet personnalisé avec lequel vous contrôlez le processus de déploiement, sélectionnez le fichier, puis cliquez sur **OK**.

    ![](deploying-a-specific-build/_static/image5.png)
10. Dans la boîte de dialogue **éléments à générer** , cliquez sur **OK**.
11. Dans le tableau **paramètres du processus de génération** , développez la section **avancé** .
12. Dans la ligne **arguments MSBuild** , spécifiez l’emplacement de votre fichier projet spécifique à l’environnement et ajoutez un espace réservé pour l’emplacement de votre dossier de génération :

    [!code-console[Main](deploying-a-specific-build/samples/sample4.cmd)]

    ![](deploying-a-specific-build/_static/image6.png)

    > [!NOTE]
    > Vous devrez remplacer la valeur **OutputRoot** chaque fois que vous mettrez une build en file d’attente. Cela est abordé dans la procédure suivante.
13. Cliquez sur **Enregistrer**.

Quand vous déclenchez une génération, vous devez mettre à jour la propriété **OutputRoot** pour qu’elle pointe vers la build que vous souhaitez déployer.

**Pour déployer une build spécifique à partir d’une définition de build**

1. Dans la fenêtre **Team Explorer** , cliquez avec le bouton droit sur la définition de build, puis cliquez sur **mettre en file d’attente une nouvelle build**.

    ![](deploying-a-specific-build/_static/image7.png)
2. Dans la boîte de dialogue **mettre en file d’attente** , sous l’onglet **paramètres** , développez la section **avancé** .
3. Dans la ligne **arguments MSBuild** , remplacez la valeur de la propriété **OutputRoot** par l’emplacement de votre dossier de génération. Exemple :

    [!code-console[Main](deploying-a-specific-build/samples/sample5.cmd)]

    ![](deploying-a-specific-build/_static/image8.png)

    > [!NOTE]
    > Veillez à inclure une barre oblique finale à la fin du chemin d’accès à votre dossier de génération.
4. Cliquez sur **file d’attente**.

Lorsque vous placez la build en file d’attente, le fichier projet déploie les scripts de base de données et les packages Web à partir du dossier de dépôt de build que vous avez spécifié dans la propriété **OutputRoot** .

## <a name="conclusion"></a>Conclusion

Cette rubrique explique comment publier des ressources de déploiement, telles que des packages Web et des scripts de base de données, à partir d’une build précédente spécifique à l’aide du modèle de déploiement de fichier de projet fractionné. Il a expliqué comment substituer la propriété **OutputRoot** et comment incorporer la logique de déploiement dans une définition de build TFS.

## <a name="further-reading"></a>informations supplémentaires

Pour plus d’informations sur la création de définitions de build, consultez [créer une définition de build de base](https://msdn.microsoft.com/library/ms181716.aspx) et [définir votre processus de génération](https://msdn.microsoft.com/library/ms181715.aspx). Pour plus d’informations sur la mise en file d’attente des builds, consultez [mettre en file d’attente une build](https://msdn.microsoft.com/library/ms181722.aspx).

> [!div class="step-by-step"]
> [Précédent](creating-a-build-definition-that-supports-deployment.md)
> [Suivant](configuring-permissions-for-team-build-deployment.md)
