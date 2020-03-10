---
uid: web-forms/overview/deployment/configuring-team-foundation-server-for-web-deployment/creating-a-build-definition-that-supports-deployment
title: Création d’une définition de build qui prend en charge le déploiement | Microsoft Docs
author: jrjlee
description: Si vous souhaitez exécuter tout type de build dans Team Foundation Server (TFS) 2010, vous devez créer une définition de build dans votre projet d’équipe. Cette rubrique des...
ms.author: riande
ms.date: 05/04/2012
ms.assetid: fe47a018-f6d0-4979-80e7-5b1fa75a5865
msc.legacyurl: /web-forms/overview/deployment/configuring-team-foundation-server-for-web-deployment/creating-a-build-definition-that-supports-deployment
msc.type: authoredcontent
ms.openlocfilehash: e11c91a824446572aaf0b3bc6954b9b8ffb4eaff
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78603998"
---
# <a name="creating-a-build-definition-that-supports-deployment"></a>Création d’une définition de build qui prend en charge le déploiement

par [Jason Lee](https://github.com/jrjlee)

[Télécharger PDF](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> Si vous souhaitez exécuter tout type de build dans Team Foundation Server (TFS) 2010, vous devez créer une définition de build dans votre projet d’équipe. Cette rubrique décrit comment créer une nouvelle définition de build dans TFS et comment contrôler le déploiement Web dans le cadre du processus de génération dans Team Build.

Cette rubrique fait partie d’une série de didacticiels basés sur les exigences de déploiement en entreprise d’une société fictive nommée Fabrikam, Inc. Cette série de didacticiels utilise un&#x2014;exemple de solution de [gestion des contacts](../web-deployment-in-the-enterprise/the-contact-manager-solution.md)&#x2014;pour représenter une application Web avec un niveau de complexité réaliste, y compris une application ASP.NET MVC 3, un service Windows Communication Foundation (WCF) et un projet de base de données.

La méthode de déploiement au cœur de ces didacticiels est basée sur l’approche de fichier de projet fractionné décrite dans [fonctionnement du fichier projet](../web-deployment-in-the-enterprise/understanding-the-project-file.md), dans lequel le processus de génération et de déploiement est&#x2014;contrôlé par deux fichiers projet qui contiennent des instructions de génération qui s’appliquent à chaque environnement de destination et un qui contient des paramètres de génération et de déploiement spécifiques à l’environnement. Au moment de la génération, le fichier projet spécifique à l’environnement est fusionné dans le fichier projet indépendant de l’environnement pour former un ensemble complet d’instructions de génération.

## <a name="task-overview"></a>Vue d’ensemble des tâches

Une définition de build est le mécanisme qui contrôle la manière et le moment où les builds se produisent pour les projets d’équipe dans TFS. Chaque définition de build spécifie :

- Les éléments que vous souhaitez générer, comme les fichiers solution Visual Studio ou les fichiers projet Microsoft Build Engine (MSBuild) personnalisés.
- Critères qui déterminent à quel moment une génération doit avoir lieu, comme les déclencheurs manuels, l’intégration continue (CI) ou les archivages contrôlés.
- Emplacement auquel Team Build doit envoyer les sorties de génération, y compris les artefacts de déploiement tels que les packages Web et les scripts de base de données.
- Durée de conservation de chaque Build.
- Divers autres paramètres du processus de génération.

> [!NOTE]
> Pour plus d’informations sur les définitions de build, consultez [définir votre processus de génération](https://msdn.microsoft.com/library/ms181715.aspx).

Cette rubrique vous montre comment créer une définition de build qui utilise CI, afin qu’une build soit déclenchée quand un développeur Archive un nouveau contenu. Si la génération est réussie, le service de build exécute un fichier projet personnalisé pour déployer la solution dans un environnement de test.

Lorsque vous déclenchez une build, ces actions doivent se produire :

- Tout d’abord, Team Build doit générer la solution. Dans le cadre de ce processus, Team Build appellera le pipeline de publication Web pour générer des packages de déploiement Web pour chacun des projets d’application Web de la solution. Team Build exécute également tous les tests unitaires associés à la solution.
- En cas d’échec de la génération de la solution, Team Build ne doit effectuer aucune action supplémentaire. Les échecs des tests unitaires doivent être traités comme un échec de Build.
- Si la génération de solution est réussie, Team Build doit exécuter le fichier projet personnalisé qui contrôle le déploiement de la solution. Dans le cadre de ce processus, Team Build appellera l’outil de déploiement Web (Web Deploy) Internet Information Services (IIS) pour installer les applications Web packagées sur les serveurs Web de destination, et il appellera l’utilitaire VSDBCMD. exe pour exécuter la création de la base de données. scripts sur les serveurs de base de données de destination.

Cela illustre le processus :

![](creating-a-build-definition-that-supports-deployment/_static/image1.png)

L’exemple de solution du [Gestionnaire de contacts](../web-deployment-in-the-enterprise/the-contact-manager-solution.md) comprend un fichier projet MSBuild personnalisé, *Publish. proj*, que vous pouvez exécuter à partir de MSBuild ou Team Build. Comme décrit dans [la rubrique fonctionnement du processus de génération](../web-deployment-in-the-enterprise/understanding-the-build-process.md), ce fichier projet définit la logique qui déploie vos packages et bases de données Web dans un environnement cible. Le fichier comprend une logique qui omet le processus de génération et d’empaquetage s’il s’exécute dans Team Build, en laissant uniquement les tâches de déploiement à exécuter. Cela est dû au fait que lorsque vous automatisez le déploiement de cette manière, vous souhaiterez généralement vous assurer que la solution se génère correctement et passe tous les tests unitaires avant le début du processus de déploiement.

La section suivante explique comment implémenter ce processus en créant une nouvelle définition de Build.

> [!NOTE]
> Cette procédure&#x2014;dans laquelle un processus automatisé unique génère, teste et déploie une solution&#x2014;est susceptible d’être le plus adapté au déploiement dans des environnements de test. Pour les environnements intermédiaires et de production, il est beaucoup plus probable de vouloir déployer du contenu à partir d’une build précédente que vous avez déjà vérifiée et validée dans un environnement de test. Cette approche est décrite dans la rubrique suivante, [déploiement d’une build spécifique](deploying-a-specific-build.md).

### <a name="who-performs-this-procedure"></a>Qui effectue cette procédure ?

En règle générale, un administrateur TFS effectue cette procédure. Dans certains cas, un responsable de l’équipe de développement peut assumer la responsabilité de la collection de projets d’équipe dans TFS. Pour créer une nouvelle définition de build, vous devez être membre du groupe administrateurs de build de la **collection de projets** pour la collection de projets d’équipe qui contient votre solution.

## <a name="create-a-build-definition-for-ci-and-deployment"></a>Créer une définition de build pour l’intégration continue et le déploiement

La procédure suivante décrit comment créer une définition de build qui est déclenchée par CI. Si la génération est réussie, la solution est déployée à l’aide de la logique dans un fichier projet MSBuild personnalisé.

**Pour créer une définition de build pour l’intégration continue et le déploiement**

1. Dans Visual Studio 2010, dans la fenêtre **Team Explorer** , développez le nœud de votre projet d’équipe, cliquez avec le bouton droit sur **Builds**, puis cliquez sur **nouvelle définition de build**.

    ![](creating-a-build-definition-that-supports-deployment/_static/image2.png)
2. Sous l’onglet **général** , attribuez un nom à la définition de build (par exemple, **DeployToTest**) et une description facultative.
3. Sous l’onglet **déclencheur** , sélectionnez les critères sur lesquels vous souhaitez déclencher une nouvelle Build. Par exemple, si vous souhaitez générer la solution et le déployer dans l’environnement de test chaque fois qu’un développeur Archive un nouveau code, sélectionnez **intégration continue**.
4. Sous l’onglet **valeurs par défaut des builds** , dans la zone **copier la sortie de la génération dans le dossier de dépôt suivant** , tapez le chemin d’accès UNC (Universal Naming Convention) de votre dossier de dépôt (par exemple, **\\TFSBUILD\Drops**).

    ![](creating-a-build-definition-that-supports-deployment/_static/image3.png)

    > [!NOTE]
    > Cet emplacement de dépôt stocke plusieurs builds, selon la stratégie de rétention que vous configurez. Lorsque vous souhaitez publier des artefacts de déploiement à partir d’une build spécifique dans un environnement intermédiaire ou de production, c’est là que vous les trouvez.
5. Sous l’onglet **processus** , dans la liste déroulante **fichier de processus de génération** , laissez le fichier **DefaultTemplate. Xaml** sélectionné. Il s’agit de l’un des modèles de processus de génération par défaut qui sont ajoutés à tous les nouveaux projets d’équipe.
6. Dans le tableau **paramètres du processus de génération** , cliquez sur la ligne **éléments à générer** , puis cliquez sur le bouton de **sélection** .

    ![](creating-a-build-definition-that-supports-deployment/_static/image4.png)
7. Dans la boîte de dialogue **éléments à générer** , cliquez sur **Ajouter**.
8. Accédez à l’emplacement de votre fichier de solution, puis cliquez sur **OK**.

    ![](creating-a-build-definition-that-supports-deployment/_static/image5.png)
9. Dans la boîte de dialogue **éléments à générer** , cliquez sur **Ajouter**.
10. Dans la liste déroulante **éléments de type** , sélectionnez **fichiers projet MSBuild**.
11. Accédez à l’emplacement du fichier projet personnalisé avec lequel vous contrôlez le processus de déploiement, sélectionnez le fichier, puis cliquez sur **OK**.

    ![](creating-a-build-definition-that-supports-deployment/_static/image6.png)
12. La boîte **de dialogue éléments à générer** doit maintenant afficher deux éléments. Cliquez sur **OK**.

    ![](creating-a-build-definition-that-supports-deployment/_static/image7.png)
13. Sous l’onglet **processus** , dans le tableau **paramètres du processus de génération** , développez la section **avancé** .
14. Dans la ligne **arguments MSBuild** , ajoutez les arguments de ligne de commande MSBuild requis par l' *un* des éléments de la génération. Dans le scénario de solution du gestionnaire de contacts, ces arguments sont requis :

    [!code-console[Main](creating-a-build-definition-that-supports-deployment/samples/sample1.cmd)]

    ![](creating-a-build-definition-that-supports-deployment/_static/image8.png)
15. Dans cet exemple :

    1. Les arguments **DeployOnBuild = true** et **DeployTarget = package** sont requis lorsque vous générez la solution du gestionnaire de contacts. Cela indique à MSBuild de créer des packages de déploiement Web après avoir généré chaque projet d’application Web, comme décrit dans [génération et empaquetage de projets d’application Web](../web-deployment-in-the-enterprise/building-and-packaging-web-application-projects.md).
    2. L’argument **TargetEnvPropsFile** est obligatoire lorsque vous générez le fichier *Publish. proj* . Cette propriété indique l’emplacement du fichier de configuration spécifique à l’environnement, comme décrit dans [fonctionnement du processus de génération](../web-deployment-in-the-enterprise/understanding-the-build-process.md).
16. Sous l’onglet **stratégie de rétention** , configurez le nombre de builds de chaque type que vous souhaitez conserver en fonction des besoins.
17. Cliquez sur **Enregistrer**.

## <a name="queue-a-build"></a>Mettre une build en file d'attente

À ce stade, vous avez créé au moins une nouvelle définition de Build. Le processus de génération que vous avez défini s’exécutera maintenant en fonction des déclencheurs que vous avez spécifiés dans la définition de Build.

Si vous avez configuré votre définition de build pour utiliser CI, vous pouvez tester votre définition de build de deux manières :

- Archivez du contenu dans le projet d’équipe pour déclencher une génération automatique.
- Mettre en file d’attente une build manuellement.

**Pour mettre une build en file d’attente manuellement**

1. Dans la fenêtre **Team Explorer** , cliquez avec le bouton droit sur la définition de build, puis cliquez sur **mettre en file d’attente une nouvelle build**.

    ![](creating-a-build-definition-that-supports-deployment/_static/image9.png)
2. Dans la boîte de dialogue **mettre en file d’attente** , passez en revue les propriétés de build, puis cliquez sur **file d’attente**.

    ![](creating-a-build-definition-that-supports-deployment/_static/image10.png)

Pour examiner la progression et le résultat d’une build&#x2014;, qu’elle ait été déclenchée manuellement ou automatiquement&#x2014;, double-cliquez sur la définition de build dans la fenêtre **Team Explorer** . L’onglet Explorateur de **Build** s’ouvre.

![](creating-a-build-definition-that-supports-deployment/_static/image11.png)

À partir de là, vous pouvez résoudre les problèmes de build ayant échoué. Si vous double-cliquez sur une build individuelle, vous pouvez afficher des informations récapitulatives et cliquer pour accéder à des fichiers journaux détaillés.

![](creating-a-build-definition-that-supports-deployment/_static/image12.png)

Vous pouvez utiliser ces informations pour résoudre les problèmes de build ayant échoué et résoudre les problèmes avant d’essayer une autre Build.

> [!NOTE]
> Les builds qui exécutent la logique de déploiement risquent d’échouer jusqu’à ce que vous ayez accordé au serveur de build toutes les autorisations requises dans l’environnement de destination. Pour plus d’informations, consultez [Configuration des autorisations pour le déploiement Team Build](configuring-permissions-for-team-build-deployment.md).

## <a name="monitor-the-build-process"></a>Surveiller le processus de génération

TFS fournit une large gamme de fonctionnalités pour vous aider à surveiller le processus de génération. Par exemple, TFS peut vous envoyer un e-mail ou afficher des alertes dans la zone de notification de la barre des tâches lorsqu’une build est terminée. Pour plus d’informations, consultez [exécuter et surveiller les builds](https://msdn.microsoft.com/library/ms181721.aspx).

## <a name="conclusion"></a>Conclusion

Cette rubrique a décrit comment créer une définition de build dans TFS. La définition de build est configurée pour l’élément de configuration, de sorte que le processus de génération s’exécute chaque fois qu’un développeur Archive le contenu dans le projet d’équipe. La définition de build exécute un fichier projet MSBuild personnalisé pour déployer des packages Web et des scripts de base de données dans un environnement de serveur cible.

Pour qu’un déploiement automatisé aboutisse dans le cadre d’un processus de génération, vous devez accorder les autorisations appropriées au compte de service de build sur les serveurs Web cibles et le serveur de base de données cible. La dernière rubrique de ce didacticiel, [Configuration des autorisations pour le déploiement de Team Build](configuring-permissions-for-team-build-deployment.md), explique comment identifier et configurer les autorisations requises pour le déploiement automatisé à partir d’un serveur Team Build.

## <a name="further-reading"></a>informations supplémentaires

Pour plus d’informations sur la création de définitions de build, consultez [créer une définition de build de base](https://msdn.microsoft.com/library/ms181716.aspx) et [définir votre processus de génération](https://msdn.microsoft.com/library/ms181715.aspx). Pour plus d’informations sur la mise en file d’attente des builds, consultez [mettre en file d’attente une build](https://msdn.microsoft.com/library/ms181722.aspx).

> [!div class="step-by-step"]
> [Précédent](configuring-a-tfs-build-server-for-web-deployment.md)
> [Suivant](deploying-a-specific-build.md)
