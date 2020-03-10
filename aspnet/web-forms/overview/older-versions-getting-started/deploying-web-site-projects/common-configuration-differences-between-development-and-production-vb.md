---
uid: web-forms/overview/older-versions-getting-started/deploying-web-site-projects/common-configuration-differences-between-development-and-production-vb
title: Différences de configuration courantes entre le développement et la production (VB) | Microsoft Docs
author: rick-anderson
description: Dans les didacticiels précédents, nous avons déployé notre site Web en copiant tous les fichiers pertinents de l’environnement de développement vers l’environnement de production. Toutefois, je...
ms.author: riande
ms.date: 04/01/2009
ms.assetid: 548e75f6-4d6c-4cb4-8da8-417915eb8393
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deploying-web-site-projects/common-configuration-differences-between-development-and-production-vb
msc.type: authoredcontent
ms.openlocfilehash: cc65af6eb4fca8b3b805e11e26da468a958a4221
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78632292"
---
# <a name="common-configuration-differences-between-development-and-production-vb"></a>Différences de configuration courantes entre le développement et la production (VB)

par [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Télécharger PDF](https://download.microsoft.com/download/E/8/9/E8920AE6-D441-41A7-8A77-9EF8FF970D8B/aspnet_tutorial05_ConfigDifferences_vb.pdf)

> Dans les didacticiels précédents, nous avons déployé notre site Web en copiant tous les fichiers pertinents de l’environnement de développement vers l’environnement de production. Toutefois, il n’est pas rare qu’il y ait des différences de configuration entre les environnements, ce qui nécessite que chaque environnement ait un fichier Web. config unique. Ce didacticiel examine les différences de configuration classiques et examine les stratégies de gestion des informations de configuration distinctes.

## <a name="introduction"></a>Introduction

Les deux derniers didacticiels ont parcouru le déploiement d’une application Web simple. Le didacticiel [*déploiement de votre site à l’aide d’un client FTP*](deploying-your-site-using-an-ftp-client-vb.md) a montré comment utiliser un client FTP autonome pour copier les fichiers nécessaires de l’environnement de développement jusqu’à la production. Le didacticiel précédent, [*déploiement de votre site à l’aide de Visual Studio*](deploying-your-site-using-visual-studio-vb.md), a examiné le déploiement à l’aide de l’outil Copier le site Web de Visual Studio et de l’option publier. Dans les deux didacticiels, chaque fichier de l’environnement de production était une copie d’un fichier dans l’environnement de développement. Toutefois, il n’est pas rare que les fichiers de configuration dans l’environnement de production diffèrent de ceux de l’environnement de développement. La configuration d’une application Web est stockée dans le fichier `Web.config` et comprend généralement des informations sur les ressources externes, telles que les serveurs de base de données, Web et de messagerie. Il présente également le comportement de l’application dans certaines situations, telles que la conduite à adopter lorsqu’une exception non gérée se produit.

Lors du déploiement d’une application Web, il est important que les informations de configuration correctes se terminent dans l’environnement de production. Dans la plupart des cas, le fichier `Web.config` dans l’environnement de développement ne peut pas être copié dans l’environnement de production tel quel. Au lieu de cela, une version personnalisée de `Web.config` doit être chargée en production. Ce didacticiel passe brièvement en revue certaines des différences de configuration les plus courantes. Elle résume également certaines techniques permettant de gérer des informations de configuration différentes entre les environnements.

## <a name="typical-configuration-differences-between-the-development-and-production-environments"></a>Différences de configuration typiques entre les environnements de développement et de production

Le fichier `Web.config` comprend un assortiment d’informations de configuration pour une application ASP.NET. Certaines de ces informations de configuration sont les mêmes, quel que soit l’environnement. Par exemple, les paramètres d’authentification et les règles d’autorisation d’URL sont épelés dans le `<authentication>` du fichier `Web.config` et les éléments `<authorization>` sont généralement les mêmes, quel que soit l’environnement. Mais d’autres informations de configuration, telles que des informations sur les ressources externes, diffèrent généralement en fonction de l’environnement.

Les chaînes de connexions de base de données sont un exemple d’informations de configuration qui diffère selon l’environnement. Lorsqu’une application Web communique avec un serveur de base de données, elle doit d’abord établir une connexion et se faire par le biais d’une [chaîne de connexion](http://www.connectionstrings.com/Articles/Show/what-is-a-connection-string). Bien qu’il soit possible de coder en dur la chaîne de connexion à la base de données directement dans les pages Web ou le code qui se connecte à la base de données, il est préférable de la placer `Web.config`[élément`<connectionStrings>`](https://msdn.microsoft.com/library/bf7sd233.aspx) afin que les informations de la chaîne de connexion se trouvent dans un emplacement unique et centralisé. Souvent, une base de données différente est utilisée pendant le développement que celle utilisée en production. par conséquent, les informations de chaîne de connexion doivent être uniques pour chaque environnement.

> [!NOTE]
> Les prochains didacticiels explorent le déploiement d’applications pilotées par les données. à ce stade, nous allons étudier les spécificités de la manière dont les chaînes de connexion de base de données sont stockées dans le fichier de configuration.

Le comportement prévu des environnements de développement et de production diffère considérablement. Une application Web dans l’environnement de développement est en cours de création, de test et de débogage par un petit groupe de développeurs. Dans l’environnement de production, la même application est visitée par de nombreux utilisateurs simultanés. ASP.NET comprend un certain nombre de fonctionnalités qui aident les développeurs à tester et à déboguer une application, mais ces fonctionnalités doivent être désactivées pour des raisons de performances et de sécurité dans l’environnement de production. Examinons quelques-uns de ces paramètres de configuration.

### <a name="configuration-settings-that-impact-performance"></a>Paramètres de configuration qui ont un impact sur les performances

Quand une page ASP.NET est visitée pour la première fois (ou la première fois qu’elle a changé), son balisage déclaratif doit être converti en une classe et cette classe doit être compilée. Si l’application Web utilise la compilation automatique, la classe code-behind de la page doit également être compilée. Vous pouvez configurer un assortiment d’options de compilation via l' [élément`<compilation>`](https://msdn.microsoft.com/library/s10awwz0.aspx)du fichier `Web.config`.

L’attribut debug est l’un des attributs les plus importants de l’élément `<compilation>`. Si l’attribut `debug` a la valeur « true », les assemblys compilés incluent des symboles de débogage, qui sont nécessaires lors du débogage d’une application dans Visual Studio. Toutefois, les symboles de débogage augmentent la taille de l’assembly et imposent des exigences supplémentaires en mémoire lors de l’exécution du code. En outre, lorsque l’attribut `debug` est défini sur « true », tout contenu retourné par `WebResource.axd` n’est pas mis en cache, ce qui signifie que chaque fois qu’un utilisateur visite une page, il doit télécharger à nouveau le contenu statique renvoyé par `WebResource.axd`.

> [!NOTE]
> `WebResource.axd` est un gestionnaire HTTP intégré introduit dans ASP.NET 2,0 que les contrôles serveur utilisent pour récupérer des ressources incorporées, telles que des fichiers de script, des images, des fichiers CSS et d’autres contenus. Pour plus d’informations sur le fonctionnement de `WebResource.axd` et sur la façon dont vous pouvez l’utiliser pour accéder à des ressources incorporées à partir de vos contrôles serveur personnalisés, consultez [accès aux ressources incorporées via une URL à l’aide de `WebResource.axd`](http://aspnet.4guysfromrolla.com/articles/080906-1.aspx).

L’attribut `debug` de l’élément `<compilation>` est généralement défini sur « true » dans l’environnement de développement. En fait, cet attribut doit avoir la valeur « true » pour pouvoir déboguer une application Web ; Si vous essayez de déboguer une application ASP.NET à partir de Visual Studio et que l’attribut `debug` est défini sur « false », Visual Studio affiche un message expliquant que l’application ne peut pas être déboguée tant que l’attribut `debug` n’est pas défini sur « true » et que vous proposez d’apporter cette modification pour vous.

Vous ne devez **jamais** avoir l’attribut `debug` défini sur « true » dans un environnement de production en raison de son impact sur les performances. Pour une discussion plus approfondie sur ce sujet, consultez le billet de blog de [Scott Guthrie](https://weblogs.asp.net/scottgu/), [ne pas exécuter les applications ASP.net de production avec `debug="true"` activé](https://weblogs.asp.net/scottgu/442448).

### <a name="custom-errors-and-tracing"></a>Suivi des erreurs et des erreurs personnalisées

Lorsqu’une exception non gérée se produit dans une application ASP.NET, elle est propagée vers le runtime à partir duquel l’un des trois événements se produit :

- Un message d’erreur d’exécution générique s’affiche. Cette page informe l’utilisateur qu’une erreur d’exécution s’est produite, mais ne fournit pas de détails sur l’erreur.
- Un message détails de l’exception s’affiche, qui comprend des informations sur l’exception qui vient d’être levée.
- Une page d’erreur personnalisée s’affiche, qui est une page ASP.NET que vous créez et qui affiche les messages de votre choix.

Ce qui se passe en face d’une exception non gérée dépend de la [section`<customErrors>`](https://msdn.microsoft.com/library/h0hfz6fc.aspx)du fichier `Web.config`.

Lors du développement et du test d’une application, elle permet d’afficher les détails d’une exception dans le navigateur. Toutefois, l’indication des détails de l’exception dans une application en production est un risque potentiel pour la sécurité. En outre, il est plus plat et rend votre site Web plus professionnel. Dans l’idéal, dans l’éventualité d’une exception non gérée, une application Web dans l’environnement de développement affichera les détails de l’exception, tandis que la même application en production affichera une page d’erreurs personnalisée.

> [!NOTE]
> Le paramètre de section `<customErrors>` par défaut affiche le message détails de l’exception uniquement lorsque la page est visitée via localhost, et affiche la page d’erreur d’exécution générique dans le cas contraire. Ce n’est pas idéal, mais il est utile de savoir que le comportement par défaut ne révèle pas les détails de l’exception pour les visiteurs non locaux. Un futur didacticiel examine plus en détail la section `<customErrors>` et indique comment afficher une page d’erreurs personnalisée lorsqu’une erreur se produit en production.

Une autre fonctionnalité ASP.NET utile pendant le développement est le suivi. Le suivi, s’il est activé, enregistre les informations relatives à chaque demande entrante et fournit une page Web spéciale, `Trace.axd`, pour afficher les détails de la demande récente. Vous pouvez activer et configurer le suivi par le biais de l' [élément`<trace>`](https://msdn.microsoft.com/library/6915t83k.aspx) dans `Web.config`.

Si vous activez le suivi, assurez-vous qu’il est désactivé en production. Étant donné que les informations de trace incluent les cookies, les données de session et d’autres informations potentiellement sensibles, il est important de désactiver le suivi en production. La bonne nouvelle est que, par défaut, le suivi est désactivé et que le fichier `Trace.axd` est uniquement accessible via localhost. Si vous modifiez ces paramètres par défaut dans le développement, assurez-vous qu’ils sont désactivés en production.

## <a name="techniques-for-maintaining-separate-configuration-information"></a>Techniques pour la conservation d’informations de configuration distinctes

Le fait de disposer de différents paramètres de configuration dans les environnements de développement et de production complique le processus de déploiement. Dans les deux didacticiels précédents, le processus de déploiement impliquait la copie de tous les fichiers nécessaires du développement à la production, mais cette approche ne fonctionne que si les informations de configuration sont les mêmes dans les deux environnements. Il existe diverses techniques pour déployer une application avec des informations de configuration variables. Nous allons cataloguer certaines de ces options pour les applications Web hébergées.

### <a name="manually-deploying-the-production-environment-configuration-file"></a>Déploiement manuel du fichier de configuration de l’environnement de production

L’approche la plus simple consiste à gérer deux versions du fichier `Web.config` : une pour l’environnement de développement et une pour l’environnement de production. Le déploiement d’un site en production implique la copie de tous les fichiers sur le serveur de production dans l’environnement de développement *, à l’exception* du fichier `Web.config`. Au lieu de cela, le fichier de `Web.config` spécifique à l’environnement de production est copié en production.

Cette approche n’est pas très sophistiquée, mais elle est facile à implémenter, car les informations de configuration changent rarement. Il fonctionne mieux pour les applications avec une petite équipe de développement hébergées sur un serveur Web unique et dont les informations de configuration sont rarement modifiées. C’est le plus tenable lors du déploiement manuel des fichiers d’application à l’aide d’un client FTP autonome. Lors de l’utilisation de l’outil Copier le site Web de Visual Studio ou de l’option de publication, vous devez d’abord remplacer le fichier `Web.config` spécifique au déploiement par le fichier spécifique à la production avant de procéder au déploiement, puis les permuter une fois le déploiement terminé.

### <a name="change-the-configuration-during-the-build-or-deployment-process"></a>Modifier la configuration pendant le processus de génération ou de déploiement

Les discussions, jusqu’à présent, ont assumé un processus de génération et de déploiement ad hoc. De nombreux projets de logiciels plus volumineux ont des processus plus formalisés qui utilisent des outils open source, de base ou tiers. Pour ces projets, vous pouvez probablement personnaliser le processus de génération ou de déploiement pour modifier correctement les informations de configuration avant qu’elles ne soient envoyées en production. Si vous générez votre application Web à l’aide de [MSBuild](http://en.wikipedia.org/wiki/MSBuild), [nant](http://nant.sourceforge.net/)ou d’un autre outil de génération, vous pouvez probablement ajouter une étape de génération pour modifier le fichier `Web.config` afin d’inclure les paramètres spécifiques à la production. Ou votre flux de travail de déploiement peut se connecter par programmation au serveur de contrôle de code source et récupérer le fichier de `Web.config` approprié.

L’approche réelle pour obtenir les informations de configuration appropriées en production varie largement en fonction de vos outils et de votre flux de travail. Par conséquent, nous n’aborderons pas ce sujet plus en détail. Si vous utilisez un outil de génération populaire comme MSBuild ou NAnt, vous trouverez des articles sur le déploiement et des didacticiels spécifiques à ces outils via une recherche Web.

### <a name="managing-configuration-differences-via-the-web-deployment-project-add-in"></a>Gestion des différences de configuration via le complément de projet de déploiement Web

Dans 2006, Microsoft a publié le complément de projet développement Web pour Visual Studio 2005. Un complément pour Visual Studio 2008 a été publié dans 2008. Ce complément permet aux développeurs ASP.NET de créer un projet de déploiement Web distinct avec leur projet d’application Web qui, lorsqu’il est généré, compile explicitement l’application Web et copie les fichiers à déployer dans un répertoire de sortie local. Le projet d’application Web utilise MSBuild en arrière-plan.

Par défaut, le fichier `Web.config` de l’environnement de développement est copié dans le répertoire de sortie, mais vous pouvez configurer le projet de déploiement Web pour personnaliser le

informations de configuration qui sont copiées dans ce répertoire des manières suivantes :

- Via `Web.config` remplacement de section de fichier, dans lequel vous spécifiez la section à remplacer et un fichier XML qui contient le texte de remplacement.
- En fournissant un chemin d’accès à un fichier source de configuration externe. Lorsque cette option est sélectionnée, le projet de déploiement Web copie un fichier `Web.config` particulier dans le répertoire de sortie (plutôt que le fichier `Web.config` utilisé dans l’environnement de développement).
- En ajoutant des règles personnalisées au fichier MSBuild utilisé par le projet de déploiement Web.

Pour déployer l’application Web, créez le projet de déploiement Web, puis copiez les fichiers du dossier de sortie du projet dans l’environnement de production.

Pour en savoir plus sur l’utilisation du projet de déploiement Web, consultez [cet article sur les projets de déploiement Web](https://msdn.microsoft.com/magazine/cc163448.aspx) du numéro d’avril 2007 de [MSDN Magazine](https://msdn.microsoft.com/magazine/default.aspx), ou consultez les liens dans la section de lecture supplémentaire à la fin de ce didacticiel.

> [!NOTE]
> Vous ne pouvez pas utiliser le projet de déploiement Web avec Visual Web Developer, car le projet de déploiement Web est implémenté en tant que complément Visual Studio et les éditions Visual Studio Express (y compris Visual Web Developer) ne prennent pas en charge les compléments.

## <a name="summary"></a>Récapitulatif

Les ressources externes et le comportement d’une application Web en cours de développement sont généralement différents de ceux de la même application en production. Par exemple, les chaînes de connexion à la base de données, les options de compilation et le comportement lorsqu’une exception non gérée se produit généralement diffèrent d’un environnement à l’autre. Le processus de déploiement doit prendre en compte ces différences. Comme nous l’avons vu dans ce didacticiel, l’approche la plus simple consiste à copier manuellement un autre fichier de configuration dans l’environnement de production. Des solutions plus élégantes sont possibles lors de l’utilisation du complément de projet de déploiement Web ou avec un processus de génération ou de déploiement plus formalisé qui peut prendre en charge ces personnalisations.

Bonne programmation !

### <a name="further-reading"></a>informations supplémentaires

Pour plus d’informations sur les sujets abordés dans ce didacticiel, reportez-vous aux ressources suivantes :

- [Description des chaînes de connexion](http://www.connectionstrings.com/Articles/Show/what-is-a-connection-string)
- [Chaînes de connexion de base de données @ ConnectionStrings.com](http://www.connectionstrings.com/)
- [Ne pas exécuter les applications ASP.NET de production avec `debug="true"` activé](https://weblogs.asp.net/scottgu/Don_1920_t-run-production-ASP.NET-Applications-with-debug_3D001D20_true_1D20_-enabled)
- [Réponse gracieuse aux exceptions non gérées-affichage des pages d’erreurs conviviales](http://aspnet.4guysfromrolla.com/articles/090606-1.aspx)
- [Comment faire : utiliser un projet de déploiement Web Visual Studio 2008](../../../videos/how-do-i/how-do-i-use-a-visual-studio-2008-web-deployment-project.md)
- [Paramètres de configuration de clé lors du déploiement d’une base de données](http://aspnet.4guysfromrolla.com/articles/121008-1.aspx)
- [Projets de déploiement Web Visual studio 2008 télécharger](https://www.microsoft.com/downloads/details.aspx?FamilyId=0AA30AE8-C73B-4BDD-BB1B-FE697256C459&amp;displaylang=en) | [projets de déploiement web Visual Studio 2005](https://download.microsoft.com/download/9/4/9/9496adc4-574e-4043-bb70-bc841e27f13c/WebDeploymentSetup.msi)
- [Projets de déploiement Web vs 2008](https://weblogs.asp.net/scottgu/archive/2005/11/06/429723.aspx) | [prise en charge des projets de déploiement Web vs 2008 publiée](https://weblogs.asp.net/scottgu/archive/2008/01/28/vs-2008-web-deployment-project-support-released.aspx)
- [Projets de déploiement web](https://msdn.microsoft.com/magazine/cc163448.aspx)

> [!div class="step-by-step"]
> [Précédent](deploying-your-site-using-visual-studio-vb.md)
> [Suivant](core-differences-between-iis-and-the-asp-net-development-server-vb.md)
