---
uid: aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/source-control
title: Contrôle de la source (création d’applications Cloud réalistes avec Azure) | Microsoft Docs
author: MikeWasson
description: La création d’applications Cloud réelles avec Azure e-book est basée sur une présentation développée par Scott Guthrie. Il décrit 13 modèles et pratiques qui peuvent le faire...
ms.author: riande
ms.date: 06/23/2015
ms.assetid: 2a0370d3-c2fb-4bf3-88b8-aad5a736c793
msc.legacyurl: /aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/source-control
msc.type: authoredcontent
ms.openlocfilehash: a6f445e46d41b646cf6c25af2e65bc73e831d5ed
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/28/2019
ms.locfileid: "74583712"
---
# <a name="source-control-building-real-world-cloud-apps-with-azure"></a>Contrôle de la source (création d’applications Cloud réalistes avec Azure)

par [Mike Wasson](https://github.com/MikeWasson), [Rick Anderson]((https://twitter.com/RickAndMSFT)), [Tom Dykstra](https://github.com/tdykstra)

[Télécharger le projet Fix it](https://code.msdn.microsoft.com/Fix-It-app-for-Building-cdd80df4) ou [Télécharger le livre électronique](https://blogs.msdn.com/b/microsoft_press/archive/2014/07/23/free-ebook-building-cloud-apps-with-microsoft-azure.aspx)

> La **création d’applications Cloud réelles avec Azure** e-book est basée sur une présentation développée par Scott Guthrie. Il décrit 13 modèles et pratiques qui peuvent vous aider à réussir le développement d’applications Web pour le Cloud. Pour plus d’informations sur l’e-Book, consultez [le premier chapitre](introduction.md).

Le contrôle de code source est essentiel pour tous les projets de développement Cloud, et pas seulement pour les environnements d’équipe. Vous ne pensez pas que la modification du code source ou même d’un document Word sans une fonction d’annulation et des sauvegardes automatiques, et le contrôle de code source vous offre ces fonctions au niveau du projet, où elles peuvent gagner du temps en cas de problème. Avec les services de contrôle de code source Cloud, vous n’avez plus à vous soucier de la configuration complexe, et vous pouvez utiliser Azure Repos contrôle de code source gratuitement pour 5 utilisateurs.

La première partie de ce chapitre explique trois bonnes pratiques principales à garder à l’esprit :

- [Traitez les scripts d’automatisation comme du code source](#scripts) et versionez-les avec le code de votre application.
- [Ne jamais vérifier les secrets](#secrets) (données sensibles telles que les informations d’identification) dans un référentiel de code source.
- [Configurez des branches sources](#devops) pour activer le flux de travail DevOps.

Le reste de ce chapitre fournit des exemples d’implémentation de ces modèles dans Visual Studio, Azure et Azure Repos :

- [Ajouter des scripts au contrôle de code source dans Visual Studio](#vsscripts)
- [Stocker des données sensibles dans Azure](#appsettings)
- [Utiliser Git dans Visual Studio et Azure Repos](#gittfs)

<a id="scripts"></a>
## <a name="treat-automation-scripts-as-source-code"></a>Traiter les scripts d’automatisation comme du code source

Lorsque vous travaillez sur un projet Cloud, vous modifiez fréquemment les choses et vous souhaitez être en mesure de réagir rapidement aux problèmes signalés par vos clients. Répondre rapidement implique l’utilisation de scripts d’automatisation, comme expliqué dans le chapitre [automatiser tout](automate-everything.md) . Tous les scripts que vous utilisez pour créer votre environnement, y déployer, le mettre à l’échelle, etc., doivent être synchronisés avec le code source de votre application.

Pour maintenir la synchronisation des scripts avec le code, stockez-les dans votre système de contrôle de code source. Ensuite, si vous avez besoin de restaurer des modifications ou d’effectuer un correctif rapide pour le code de production, ce qui est différent du code de développement, il n’est pas nécessaire de perdre du temps à essayer de trouver les paramètres qui ont été modifiés ou les membres de l’équipe qui ont des copies de la version dont vous avez besoin. Vous êtes certain que les scripts dont vous avez besoin sont synchronisés avec la base de code dont vous avez besoin pour, et vous êtes assuré que tous les membres de l’équipe travaillent avec les mêmes scripts. Ensuite, que vous ayez besoin d’automatiser les tests et le déploiement d’un correctif pour la production ou le développement de nouvelles fonctionnalités, vous disposerez du script approprié pour le code qui doit être mis à jour.

<a id="secrets"></a>
## <a name="dont-check-in-secrets"></a>Ne pas vérifier les secrets

Un référentiel de code source est généralement accessible à un trop grand nombre de personnes pour qu’il soit un emplacement sécurisé approprié pour les données sensibles telles que les mots de passe. Si les scripts s’appuient sur des secrets tels que des mots de passe, paramétrez ces paramètres afin qu’ils ne soient pas enregistrés dans le code source et stockez vos secrets ailleurs.

Par exemple, Azure vous permet de télécharger des fichiers qui contiennent des paramètres de publication afin d’automatiser la création de profils de publication. Ces fichiers incluent les noms d’utilisateur et les mots de passe autorisés à gérer vos services Azure. Si vous utilisez cette méthode pour créer des profils de publication et si vous archivez ces fichiers dans le contrôle de code source, toute personne disposant d’un accès à votre référentiel peut voir ces noms d’utilisateur et mots de passe. Vous pouvez stocker le mot de passe en toute sécurité dans le profil de publication, car il est chiffré et il se trouve dans un fichier *. pubxml. User* qui, par défaut, n’est pas inclus dans le contrôle de code source.

<a id="devops"></a>
## <a name="structure-source-branches-to-facilitate-devops-workflow"></a>Structurer des branches source pour faciliter le workflow DevOps

La façon dont vous implémentez des branches dans votre référentiel affecte votre capacité à développer de nouvelles fonctionnalités et à résoudre les problèmes de production. Voici un modèle utilisé par de nombreuses équipes de taille moyenne :

![Structure de branche source](source-control/_static/image1.png)

La branche principale correspond toujours au code qui est en production. Les branches sous Master correspondent aux différentes étapes du cycle de vie du développement. La branche de développement est l’endroit où vous implémentez de nouvelles fonctionnalités. Pour une petite équipe, vous pouvez simplement avoir le maître et le développement, mais nous recommandons souvent aux utilisateurs de disposer d’une branche intermédiaire entre le développement et le maître. Vous pouvez utiliser la mise en lots pour le test d’intégration final avant qu’une mise à jour ne soit déplacée en production.

Pour les grandes équipes, il peut y avoir des branches distinctes pour chaque nouvelle fonctionnalité. pour une petite équipe, vous pouvez avoir tout le monde dans la branche de développement.

Si vous disposez d’une branche pour chaque fonctionnalité, lorsque la fonctionnalité A est prête, vous fusionnez les modifications de code source dans la branche de développement et les autres branches de fonctionnalités. Ce processus de fusion de code source peut prendre du temps et, pour éviter ce travail, tout en conservant des fonctionnalités distinctes, certaines équipes implémentent une *[alternative appelée «](http://en.wikipedia.org/wiki/Feature_toggle)* indicateurs de *fonctionnalité*». Cela signifie que tout le code de toutes les fonctionnalités se trouve dans la même branche, mais que vous activez ou désactivez chaque fonctionnalité à l’aide de commutateurs dans le code. Par exemple, supposons que la fonctionnalité A est un nouveau champ pour les tâches corriger l’application, et que la fonctionnalité B ajoute des fonctionnalités de mise en cache. Le code des deux fonctionnalités peut être dans la branche de développement, mais l’application n’affichera que le nouveau champ quand une variable a la valeur true, et elle utilisera uniquement la mise en cache quand une variable différente aura la valeur true. Si la fonctionnalité A n’est pas prête à être promue mais que la fonctionnalité B est prête, vous pouvez promouvoir tout le code en production avec la fonctionnalité A et le commutateur de fonctionnalité B activé. Vous pouvez ensuite terminer la fonctionnalité A et la promouvoir ultérieurement, tout cela sans fusion de code source.

Que vous utilisiez ou non des branches ou des basculements pour des fonctionnalités, une structure de branchement comme celle-ci vous permet de transmettre votre code du développement à la production d’une manière agile et reproductible.

Cette structure vous permet également de réagir rapidement aux commentaires des clients. Si vous devez effectuer un rapide correctif pour la production, vous pouvez également le faire de manière efficace. Vous pouvez créer une branche hors du serveur maître ou intermédiaire et, quand elle est prête, la fusionner dans les branches de développement et de fonctionnalité.

![branche du correctif](source-control/_static/image2.png)

Sans une structure de création de branche comme celle-ci avec sa séparation des branches de production et de développement, un problème de production peut vous permettre de devoir promouvoir de nouveaux codes de fonctionnalités avec votre correctif de production. Le nouveau code de fonctionnalité n’est peut-être pas entièrement testé et prêt pour la production et vous devrez peut-être effectuer de nombreuses tâches de sauvegarde qui ne sont pas prêtes. Ou vous devrez peut-être retarder votre correctif pour tester les modifications et les préparer au déploiement.

Vous verrez ensuite des exemples d’implémentation de ces trois modèles dans Visual Studio, Azure et Azure Repos. Il s’agit d’exemples plutôt que d’instructions détaillées sur la procédure à suivre. pour obtenir des instructions détaillées qui fournissent tout le contexte nécessaire, consultez la section [ressources](#resources) à la fin du chapitre.

<a id="vsscripts"></a>
## <a name="add-scripts-to-source-control-in-visual-studio"></a>Ajouter des scripts au contrôle de code source dans Visual Studio

Vous pouvez ajouter des scripts au contrôle de code source dans Visual Studio en les incluant dans un dossier de solution Visual Studio (en supposant que votre projet se trouve dans le contrôle de code source). Voici une façon d’y parvenir.

Créez un dossier pour les scripts dans votre dossier de solution (le même dossier que celui qui contient votre fichier *. sln* ).

![Dossier Automation](source-control/_static/image3.png)

Copiez les fichiers de script dans le dossier.

![Contenu du dossier Automation](source-control/_static/image4.png)

Dans Visual Studio, ajoutez un dossier de solution au projet.

![Sélection du menu nouveau dossier de solution](source-control/_static/image5.png)

Et ajoutez les fichiers de script au dossier de la solution.

![Sélection du menu Ajouter un élément existant](source-control/_static/image6.png)

![Ajouter un élément existant (boîte de dialogue)](source-control/_static/image7.png)

Les fichiers de script sont désormais inclus dans votre projet et le contrôle de code source effectue le suivi des modifications apportées à leur version, ainsi que des modifications du code source correspondantes.

<a id="appsettings"></a>
## <a name="store-sensitive-data-in-azure"></a>Stocker des données sensibles dans Azure

Si vous exécutez votre application sur un site Web Azure, un moyen d’éviter le stockage des informations d’identification dans le contrôle de code source consiste à les stocker dans Azure à la place.

Par exemple, l’application Fix it stocke dans son fichier Web. config deux chaînes de connexion dont les mots de passe sont en production et une clé qui donne accès à votre compte de stockage Azure.

[!code-xml[Main](source-control/samples/sample1.xml?highlight=2-3,11)]

Si vous placez des valeurs de production réelles pour ces paramètres dans votre fichier *Web. config* , ou si vous les placez dans le fichier *Web. Release. config* pour configurer une transformation Web. config pour les insérer au cours du déploiement, elles sont stockées dans le référentiel source. Si vous entrez les chaînes de connexion de la base de données dans le profil de publication de production, le mot de passe sera dans votre fichier *. pubxml* . (Vous pouvez exclure le fichier *. pubxml* du contrôle de code source, mais vous perdez l’avantage de partager tous les autres paramètres de déploiement.)

Azure vous offre une alternative pour les sections **appSettings** et les chaînes de connexion du fichier *Web. config* . Voici la partie correspondante de l’onglet **configuration** d’un site Web dans le portail de gestion Azure :

![appSettings et connectionStrings dans le portail](source-control/_static/image8.png)

Lorsque vous déployez un projet sur ce site Web et que l’application s’exécute, les valeurs que vous avez stockées dans Azure remplacent toutes les valeurs figurant dans le fichier Web. config.

Vous pouvez définir ces valeurs dans Azure à l’aide du portail de gestion ou de scripts. Le script d’automatisation de la création de l’environnement que vous avez vu dans le chapitre [automatiser tout](automate-everything.md) crée une Azure SQL Database, obtient les chaînes de connexion de SQL Database et de stockage, puis stocke ces secrets dans les paramètres de votre site Web.

[!code-powershell[Main](source-control/samples/sample2.ps1)]

[!code-powershell[Main](source-control/samples/sample3.ps1)]

Notez que les scripts sont paramétrables afin que les valeurs réelles ne soient pas conservées dans le référentiel source.

Lorsque vous exécutez localement dans votre environnement de développement, l’application lit votre fichier Web. config local et votre chaîne de connexion pointe vers une base de données locale SQL Server dans le dossier de données de l' *application\_* de votre projet Web. Lorsque vous exécutez l’application dans Azure et que l’application tente de lire ces valeurs à partir du fichier Web. config, ce qu’elle récupère et utilise, les valeurs stockées pour le site Web, et non pas le contenu du fichier Web. config.

<a id="gittfs"></a>
## <a name="use-git-in-visual-studio-and-azure-devops"></a>Utiliser Git dans Visual Studio et Azure DevOps

Vous pouvez utiliser n’importe quel environnement de contrôle de code source pour implémenter la structure de branche DevOps présentée précédemment. Pour les équipes distribuées, un [système de contrôle de version distribué](http://en.wikipedia.org/wiki/Distributed_revision_control) (menés par) peut fonctionner de manière optimale. pour les autres équipes, un [système centralisé](http://en.wikipedia.org/wiki/Revision_control) peut fonctionner mieux.

[Git](http://git-scm.com/) est un système de contrôle de version distribué populaire. Lorsque vous utilisez git pour le contrôle de code source, vous disposez d’une copie complète du référentiel avec l’intégralité de son historique sur votre ordinateur local. De nombreuses personnes préfèrent qu’il soit plus facile de continuer à travailler lorsque vous n’êtes pas connecté au réseau. vous pouvez continuer à effectuer des validations et des restaurations, créer et basculer des branches, et ainsi de suite. Même lorsque vous êtes connecté au réseau, il est plus facile et plus rapide de créer des branches et de changer de branche lorsque tout est local. Vous pouvez également effectuer des validations et des restaurations locales sans avoir d’impact sur les autres développeurs. Et vous pouvez effectuer des validations par lots avant de les envoyer au serveur.

[Azure repos](/azure/devops/repos/index?view=vsts) offre à la fois [git](/azure/devops/repos/git/?view=vsts) et [Team Foundation version Control](/azure/devops/repos/tfvc/index?view=vsts) (TFVC ; contrôle de code source centralisé). Commencez à utiliser Azure DevOps [ici](https://app.vsaex.visualstudio.com/signup).

Visual Studio 2017 comprend une [prise en charge](https://msdn.microsoft.com/library/hh850437.aspx)intégrée de git de première classe. Voici une démonstration rapide de son fonctionnement.

Avec un projet ouvert dans Visual Studio, cliquez avec le bouton droit sur la solution dans **Explorateur de solutions**, puis choisissez **Ajouter la solution au contrôle de code source**.

![Ajouter la solution au contrôle de code source](source-control/_static/image9.png)

Visual Studio vous demande si vous souhaitez utiliser TFVC (gestion de version centralisée) ou git.

![Choisir le contrôle de code source](source-control/_static/image10.png)

Lorsque vous sélectionnez git et que vous cliquez sur **OK**, Visual Studio crée un nouveau référentiel Git local dans votre dossier de solution. Le nouveau référentiel n’a pas encore de fichiers ; vous devez les ajouter au référentiel en exécutant une validation git. Cliquez avec le bouton droit sur la solution dans **Explorateur de solutions**, puis cliquez sur **valider**.

![Valider](source-control/_static/image11.png)

Visual Studio effectue automatiquement la modification de tous les fichiers projet pour la validation et les répertorie dans **Team Explorer** dans le volet **modifications incluses** . (Si vous ne vouliez pas inclure certains éléments dans la validation, vous pouvez les sélectionner, cliquer avec le bouton droit, puis cliquer sur **exclure**.)

![Team Explorer](source-control/_static/image12.png)

Entrez un commentaire de validation et cliquez sur **valider**, et Visual Studio exécute la validation et affiche l’ID de validation.

![Modifications de Team Explorer](source-control/_static/image13.png)

Maintenant, si vous modifiez du code afin qu’il soit différent de ce qui se trouve dans le référentiel, vous pouvez facilement afficher les différences. Cliquez avec le bouton droit sur un fichier que vous avez modifié, sélectionnez **comparer avec non modifié**, et vous disposez d’un affichage de comparaison qui affiche votre modification non validée.

![Comparer avec non modifié](source-control/_static/image14.png)

![Diff qui présente des modifications](source-control/_static/image15.png)

Vous pouvez facilement voir les modifications que vous effectuez et les archiver.

Supposons que vous deviez créer une branche : vous pouvez également le faire dans Visual Studio. Dans **Team Explorer**, cliquez sur **nouvelle branche**.

![Team Explorer une nouvelle branche](source-control/_static/image16.png)

Entrez un nom de branche, cliquez sur **créer une branche**. Si vous avez sélectionné **extraire une branche**, Visual Studio extrait automatiquement la nouvelle branche.

![Team Explorer une nouvelle branche](source-control/_static/image17.png)

Vous pouvez maintenant apporter des modifications aux fichiers et les archiver dans cette branche. Vous pouvez facilement basculer entre les branches et Visual Studio synchronise automatiquement les fichiers avec la branche que vous avez extraite. Dans cet exemple, le titre de la page Web dans *\_Layout. cshtml* a été remplacé par « Hot Fix 1 » dans HotFix1 Branch.

![Branche Hotfix1](source-control/_static/image18.png)

Si vous revenez à la branche principale, le contenu du fichier *\_Layout. cshtml* revient automatiquement à ce qu’ils se trouvent dans la branche principale.

![Branche principale](source-control/_static/image19.png)

C’est un exemple simple de la façon dont vous pouvez créer rapidement une branche et basculer entre les branches. Cette fonctionnalité permet un flux de travail hautement agile à l’aide de la structure de branche et des scripts d’automatisation présentés dans le chapitre [automatiser tout](automate-everything.md) . Par exemple, vous pouvez travailler dans la branche de développement, créer une branche de correction à chaud à partir de Master, basculer vers la nouvelle branche, apporter vos modifications et les valider, puis revenir à la branche de développement et poursuivre ce que vous étiez en train de faire.

Ce que vous avez vu ici, c’est la façon dont vous utilisez un référentiel Git local dans Visual Studio. Dans un environnement d’équipe, vous transformez également les modifications vers un référentiel commun. Les outils Visual Studio vous permettent également de pointer vers un référentiel git distant. Vous pouvez utiliser GitHub.com à cet effet, ou vous pouvez utiliser [git et Azure repos](/azure/devops/repos/git/overview?view=vsts) intégrés à toutes les autres fonctionnalités Azure DevOps, telles que les éléments de travail et le suivi des bogues.

Il ne s’agit pas de la seule façon dont vous pouvez implémenter une stratégie de branchement agile, bien sûr. Vous pouvez activer le même flux de travail agile à l’aide d’un référentiel de contrôle de code source centralisé.

## <a name="summary"></a>Récapitulatif

Mesurez la réussite de votre système de contrôle de code source en fonction de la rapidité avec laquelle vous pouvez apporter une modification et la mettre en ligne de façon sécurisée et prévisible. Si vous êtes effrayé pour apporter une modification, car vous devez effectuer un ou deux tests manuels sur celui-ci, vous pouvez vous demander ce que vous devez faire au cours d’une opération ou d’un test pour que vous puissiez effectuer cette modification en quelques minutes ou au pire pas plus d’une heure. Une stratégie pour effectuer cette tâche consiste à implémenter l’intégration continue et la livraison continue, que nous aborderons dans le [chapitre suivant](continuous-integration-and-continuous-delivery.md).

<a id="resources"></a>
## <a name="resources"></a>Ressources

Pour plus d’informations sur les stratégies de branchement, consultez les ressources suivantes :

- [Création d’un pipeline de mise en version avec Team Foundation Server 2012](https://msdn.microsoft.com/library/dn449957.aspx). Documentation Microsoft Patterns and Practices. Pour plus d’informations sur les stratégies de branchement, consultez le chapitre 6. Les avocats de fonctionnalités s’activent sur les branches de fonctionnalités et, si des branches pour des fonctionnalités sont utilisées, préconisent leur conservation de courte durée (heures ou jours au plus).
- [Guide de gestion de version](https://aka.ms/vsarsolutions). Guide des stratégies de branchement par les ALM Rangers. Consultez Branching Strategies. pdf sous l’onglet downloads (Téléchargements).
- [Développement logiciel avec basculement des fonctionnalités](https://msdn.microsoft.com/magazine/dn683796.aspx). Article de MSDN Magazine.
- [Bascule de fonctionnalité](http://martinfowler.com/bliki/FeatureToggle.html). Présentation des indicateurs de fonctionnalité/de fonctionnalités sur le blog de Martin Fowler.
- [Active/désactive les branches de fonctionnalités vs](http://geekswithblogs.net/Optikal/archive/2013/02/10/152069.aspx). Un autre billet de blog sur les fonctionnalités bascule, par Dylan Smith.

Pour plus d’informations sur la gestion des informations sensibles qui ne doivent pas être conservées dans les référentiels de contrôle de code source, consultez les ressources suivantes :

- [Meilleures pratiques pour le déploiement de mots de passe et d’autres données sensibles dans ASP.net et Azure App service](../../../../identity/overview/features-api/best-practices-for-deploying-passwords-and-other-sensitive-data-to-aspnet-and-azure.md).
- [Sites Web Azure : fonctionnement des chaînes d’application et des chaînes de connexion](https://azure.microsoft.com/blog/2013/07/17/windows-azure-web-sites-how-application-strings-and-connection-strings-work/). Explique la fonctionnalité Azure qui remplace les données `appSettings` et `connectionStrings` dans le fichier *Web. config* .
- [Configuration personnalisée et paramètres d’application dans les sites Web Azure-avec Stefan Schackow](https://azure.microsoft.com/documentation/videos/configuration-and-app-settings-of-azure-web-sites/).

Pour plus d’informations sur les autres méthodes permettant de conserver les informations sensibles dans le contrôle de code source, consultez [ASP.NET MVC : conserver les paramètres privés en dehors du contrôle de code source](http://typecastexception.com/post/2014/04/06/ASPNET-MVC-Keep-Private-Settings-Out-of-Source-Control.aspx).

> [!div class="step-by-step"]
> [Précédent](automate-everything.md)
> [Suivant](continuous-integration-and-continuous-delivery.md)
