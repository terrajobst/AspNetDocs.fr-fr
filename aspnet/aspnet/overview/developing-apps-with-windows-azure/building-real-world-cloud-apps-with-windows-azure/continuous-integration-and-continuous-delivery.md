---
uid: aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/continuous-integration-and-continuous-delivery
title: Intégration continue et livraison continue (création d’applications Cloud réalistes avec Azure) | Microsoft Docs
author: MikeWasson
description: La création d’applications Cloud réelles avec Azure e-book est basée sur une présentation développée par Scott Guthrie. Il décrit 13 modèles et pratiques qui peuvent le faire...
ms.author: riande
ms.date: 06/12/2014
ms.assetid: eaece9f5-f80c-428b-b771-5db66d275b7d
msc.legacyurl: /aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/continuous-integration-and-continuous-delivery
msc.type: authoredcontent
ms.openlocfilehash: cf3c65ef95528173eed3fb08984035b2512861c4
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78617837"
---
# <a name="continuous-integration-and-continuous-delivery-building-real-world-cloud-apps-with-azure"></a>Intégration continue et livraison continue (création d’applications Cloud réalistes avec Azure)

par [Mike Wasson](https://github.com/MikeWasson), [Rick Anderson](https://twitter.com/RickAndMSFT), [Tom Dykstra](https://github.com/tdykstra)

[Télécharger le projet Fix it](https://code.msdn.microsoft.com/Fix-It-app-for-Building-cdd80df4) ou [Télécharger le livre électronique](https://blogs.msdn.com/b/microsoft_press/archive/2014/07/23/free-ebook-building-cloud-apps-with-microsoft-azure.aspx)

> La **création d’applications Cloud réelles avec Azure** e-book est basée sur une présentation développée par Scott Guthrie. Il décrit 13 modèles et pratiques qui peuvent vous aider à réussir le développement d’applications Web pour le Cloud. Pour plus d’informations sur l’e-Book, consultez [le premier chapitre](introduction.md).

Les deux premiers modèles de processus de développement recommandés ont été d' [automatiser tout](automate-everything.md) le [contrôle de code source](source-control.md), et le troisième modèle de processus les combine. L’intégration continue signifie que chaque fois qu’un développeur Archive du code dans le référentiel source, une build est déclenchée automatiquement. La livraison continue (CD) va encore plus loin : après la réussite d’une génération et de tests unitaires automatisés, vous déployez automatiquement l’application dans un environnement où vous pouvez effectuer des tests plus approfondis.

Le Cloud vous permet de réduire le coût de la maintenance d’un environnement de test, car vous payez uniquement pour les ressources d’environnement à condition que vous les utilisiez. Votre processus de CD peut configurer l’environnement de test lorsque vous en avez besoin, et vous pouvez mettre l’environnement en service lorsque vous avez terminé le test.

## <a name="continuous-integration-and-continuous-delivery-workflow"></a>Flux de travail d’intégration continue et de livraison continue

En général, nous vous recommandons d’effectuer une livraison continue dans vos environnements de développement et intermédiaires. La plupart des équipes, même chez Microsoft, requièrent un processus de révision et d’approbation manuel pour le déploiement en production. Pour un déploiement de production, vous souhaiterez peut-être vous assurer qu’il se produit lorsque des personnes clés de l’équipe de développement sont disponibles pour la prise en charge ou pendant des périodes de faible trafic. Mais rien ne vous empêche d’automatiser complètement vos environnements de développement et de test, de sorte que tous les développeurs doivent vérifier une modification et un environnement est configuré pour les tests d’acceptation.

Le diagramme suivant d' [un livre électronique Microsoft Patterns and Practices sur la livraison continue](https://aka.ms/ReleasePipeline) illustre un flux de travail standard. Cliquez sur l’image pour afficher sa taille complète dans son contexte d’origine.

[flux de travail de livraison continue ![](continuous-integration-and-continuous-delivery/_static/image1.png)](https://msdn.microsoft.com/library/dn449955.aspx)

## <a name="how-the-cloud-enables-cost-effective-ci-and-cd"></a>Comment le Cloud active la solution de contrôle d’intégration et de livraison de CD

L’automatisation de ces processus dans Azure est simple. Étant donné que vous exécutez tout dans le Cloud, vous n’avez pas besoin d’acheter ou de gérer des serveurs pour vos builds ou vos environnements de test. Et vous n’avez pas besoin d’attendre la disponibilité d’un serveur pour effectuer vos tests sur. Avec chaque Build que vous effectuez, vous pouvez créer un environnement de test dans Azure à l’aide de votre script d’automatisation, exécuter des tests d’acceptation ou des tests plus approfondis, puis, lorsque vous avez terminé, simplement le détacher. Et si vous exécutez ce serveur uniquement pendant 2 heures ou 8 heures ou une journée, la somme d’argent que vous devez payer pour celui-ci est minime, car vous payez uniquement pour la durée d’exécution réelle d’une machine. Par exemple, l’environnement requis pour l’application Fix it coûte fondamentalement 1% par heure si vous passez d’un niveau au niveau gratuit. Au cours d’un mois, si vous n’avez exécuté l’environnement qu’une heure à la fois, votre environnement de test aurait probablement coûté moins qu’une latte que vous achetez à Starbucks.

## <a name="azure-devops-services"></a>Azure DevOps Services 

Azure DevOps Services fournit un certain nombre de fonctionnalités pour vous aider dans le développement d’applications, de la planification au déploiement.

- Il prend en charge à la fois le contrôle de code source git (distribué) et TFVC (centralisé).
- Il offre un service de build élastique, ce qui signifie qu’il crée dynamiquement des serveurs de builds lorsqu’ils sont nécessaires et les met en baisse lorsqu’ils sont terminés. Vous pouvez lancer automatiquement une build quand un utilisateur archive des modifications du code source et que vous n’avez pas besoin d’allouer et de payer pour vos propres serveurs de build qui s’inactivent la plupart du temps. Le service de build est gratuit tant que vous ne dépassez pas un certain nombre de builds. Si vous envisagez d’effectuer un volume élevé de builds, vous pouvez payer un peu plus pour les serveurs de build réservés.
- Il prend en charge la livraison continue vers Azure.
- Il prend en charge les tests de charge automatisés. Le test de charge est essentiel pour une application Cloud, mais il est souvent négligé jusqu’à ce qu’il soit trop tard. Le test de charge simule une utilisation intensive d’une application par des milliers d’utilisateurs, ce qui vous permet de trouver des goulots d’étranglement et d’améliorer le débit, avant de publier l’application en production.
- Il prend en charge la collaboration de salle d’équipe, qui facilite la communication et la collaboration en temps réel pour les petites équipes Agile.
- Il prend en charge la gestion de projet agile.

Pour plus d’informations sur les fonctionnalités d’intégration et de remise continues de Azure DevOps Services, consultez [la documentation Azure DevOps](/azure/devops/index).

Si vous recherchez une solution de gestion de projet, de collaboration d’équipe et de contrôle de code source, consultez Azure DevOps Services. Inscrivez-vous à [Azure DevOps services](https://dev.azure.com/).

## <a name="summary"></a>Récapitulatif

Les trois premiers modèles de développement Cloud ont été sur la façon d’implémenter un processus de développement reproductible, fiable et prévisible avec une durée de cycle faible. Dans le [chapitre suivant](web-development-best-practices.md) , nous commençons à examiner les modèles d’architecture et de codage.

## <a name="resources"></a>Ressources

Pour plus d’informations, consultez [déployer une application Web dans Azure App service](https://azure.microsoft.com/documentation/articles/web-sites-deploy/).

Consultez également les ressources suivantes :

- [Création d’un pipeline de mise en version avec Team Foundation Server 2012](https://aka.ms/ReleasePipeline). L’E-Book, les laboratoires pratiques et les exemples de code par Microsoft Patterns and Practices, fournit une présentation détaillée de la livraison continue. Couvre l’utilisation de Visual Studio Lab Management et de Visual Studio Release Management.
- [ALM Rangers DevOps outils et conseils](https://aka.ms/vsarsolutions/). Les classeurs ALM ont introduit la solution d’accompagnement DevOps Workbench et des conseils pratiques en collaboration avec le manuel patterns &amp; Practices, *qui crée un pipeline de mise en œuvre avec tfs 2012*, pour commencer à apprendre les concepts de DevOps &amp; Release Management pour TFS 2012 et pour lancer les pneus. Ce guide montre comment générer une fois et déployer dans plusieurs environnements.
- [Test de la livraison continue avec Visual Studio 2012](https://msdn.microsoft.com/library/jj159345.aspx). Livre électronique par Microsoft Patterns and Practices, explique comment intégrer des tests automatisés avec une livraison continue.
- [WindowsAzureDeploymentTracker](https://github.com/RyanTBerry/WindowsAzureDeploymentTracker). Code source d’un outil conçu pour capturer une build à partir de TFS (en fonction d’une étiquette), la générer, la créer dans un package, autoriser une personne du rôle DevOps à configurer des aspects spécifiques de celle-ci et l’envoyer dans Azure. L’outil effectue le suivi du processus de déploiement afin d’autoriser les opérations à « restaurer » sur une version précédemment déployée. L’outil n’a pas de dépendances externes et peut fonctionner de manière autonome à l’aide des API TFS et du kit de développement logiciel (SDK) Azure.
- [Livraison continue : publications de logiciels fiables via l’automatisation de la génération, du test et du déploiement](https://www.amazon.com/Continuous-Delivery-Deployment-Automation-Addison-Wesley/dp/0321601912/ref=sr_1_1?s=books&amp;ie=UTF8&amp;qid=1377126361). Livrez par Jez de la modeste.
- [Release ! concevez et déployez des logiciels prêts](https://www.amazon.com/Release-It-Production-Ready-Pragmatic-Programmers/dp/0978739213)pour la production. Livre de Michael T. Nygard.

> [!div class="step-by-step"]
> [Précédent](source-control.md)
> [Suivant](web-development-best-practices.md)
