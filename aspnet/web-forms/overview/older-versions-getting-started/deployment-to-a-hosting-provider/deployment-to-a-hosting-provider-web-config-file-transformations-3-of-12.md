---
uid: web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-web-config-file-transformations-3-of-12
title: 'Déploiement d’une application Web ASP.NET avec SQL Server Compact à l’aide de Visual Studio ou Visual Web Developer : transformations de fichier Web. config-3 sur 12 | Microsoft Docs'
author: tdykstra
description: Cette série de didacticiels vous montre comment déployer (publier) un projet d’application Web ASP.NET qui comprend une base de données SQL Server Compact à l’aide de Stu...
ms.author: riande
ms.date: 11/17/2011
ms.assetid: 2b0df3d9-450b-4ea6-b315-4c9650722cad
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-web-config-file-transformations-3-of-12
msc.type: authoredcontent
ms.openlocfilehash: 9e7902bcf8a16c154aee1a982824bfaedeea7d9d
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78634924"
---
# <a name="deploying-an-aspnet-web-application-with-sql-server-compact-using-visual-studio-or-visual-web-developer-webconfig-file-transformations---3-of-12"></a>Déploiement d’une application Web ASP.NET avec SQL Server Compact à l’aide de Visual Studio ou Visual Web Developer : transformations de fichier Web. config-3 sur 12

par [Tom Dykstra](https://github.com/tdykstra)

[Télécharger le projet de démarrage](https://code.msdn.microsoft.com/Deploying-an-ASPNET-Web-4e31366b)

> Cette série de didacticiels vous montre comment déployer (publier) un projet d’application Web ASP.NET qui comprend une base de données SQL Server Compact à l’aide de Visual Studio 2012 RC ou Visual Studio Express 2012 RC pour le Web. Vous pouvez également utiliser Visual Studio 2010 si vous installez la mise à jour de publication Web. Pour obtenir une présentation de la série, consultez [le premier didacticiel de la série](deployment-to-a-hosting-provider-introduction-1-of-12.md).
> 
> Pour obtenir un didacticiel qui présente les fonctionnalités de déploiement introduites après la version RC de Visual Studio 2012, montre comment déployer des éditions SQL Server autres que SQL Server Compact et montre comment déployer vers Azure App Service Web Apps, consultez [déploiement Web ASP.net à l’aide de Visual Studio](../../deployment/visual-studio-web-deployment/introduction.md).

## <a name="overview"></a>Présentation

Ce didacticiel vous montre comment automatiser le processus de modification du fichier *Web. config* lorsque vous le déployez dans différents environnements de destination. La plupart des applications ont des paramètres dans le fichier *Web. config* qui doivent être différents lorsque l’application est déployée. L’automatisation du processus d’apport de ces modifications vous évite de devoir les exécuter manuellement à chaque fois que vous déployez, ce qui serait fastidieux et sujet aux erreurs.

Rappel : Si vous recevez un message d’erreur ou si une action ne fonctionne pas au fur et à mesure que vous parcourez le didacticiel, veillez à consulter la [page de résolution des problèmes](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12.md).

## <a name="webconfig-transformations-versus-web-deploy-parameters"></a>Transformations Web. config et paramètres de Web Deploy

Il existe deux façons d’automatiser le processus de modification des paramètres du fichier *Web. config* : les [transformations Web. config](https://msdn.microsoft.com/library/dd465326.aspx) et les [paramètres de Web Deploy](https://msdn.microsoft.com/library/ff398068.aspx). Un fichier de transformation *Web. config* contient un balisage XML qui spécifie comment modifier le fichier *Web. config* lors de son déploiement. Vous pouvez spécifier des modifications différentes pour des configurations de build spécifiques et pour des profils de publication spécifiques. Les configurations de build par défaut sont Debug et Release, et vous pouvez créer des configurations de build personnalisées. Un profil de publication correspond généralement à un environnement de destination. (Pour plus d’informations sur les profils de publication, consultez le didacticiel [déploiement sur IIS en tant qu’environnement de test](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12.md) .)

Web Deploy paramètres peuvent être utilisés pour spécifier de nombreux types de paramètres qui doivent être configurés lors du déploiement, y compris les paramètres qui se trouvent dans les fichiers *Web. config* . Lorsqu’ils sont utilisés pour spécifier des modifications de fichier *Web. config* , les paramètres de Web Deploy sont plus complexes à configurer, mais ils sont utiles lorsque vous ne connaissez pas la valeur à définir tant que vous n’avez pas déployé. Par exemple, dans un environnement d’entreprise, vous pouvez créer un *package de déploiement* et le transmettre à une personne du service informatique pour l’installer en production, et cette personne doit être en mesure d’entrer des chaînes de connexion ou des mots de passe que vous ne connaissez pas.

Pour le scénario couvert par ce didacticiel, vous savez tout ce qui doit être fait dans le fichier *Web. config* . vous n’avez donc pas besoin d’utiliser des paramètres de Web Deploy. Vous allez configurer certaines transformations qui diffèrent en fonction de la configuration de build utilisée, et d’autres qui diffèrent en fonction du profil de publication utilisé.

## <a name="creating-transformation-files-for-publish-profiles"></a>Création de fichiers de transformation pour les profils de publication

Dans **Explorateur de solutions**, développez *Web. config* pour voir les fichiers de transformation *Web. Debug. config* et *Web. Release. config* qui sont créés par défaut pour les deux configurations de build par défaut.

![Web.config_transform_files](deployment-to-a-hosting-provider-web-config-file-transformations-3-of-12/_static/image1.png)

Vous pouvez créer des fichiers de transformation pour des configurations de build personnalisées en cliquant avec le bouton droit sur le fichier Web. config et en choisissant **Ajouter des transformations de configuration** dans le menu contextuel, mais pour ce didacticiel, vous n’avez pas besoin de le faire.

Vous avez besoin de deux fichiers de transformation supplémentaires pour configurer les modifications liées à la destination de déploiement plutôt qu’à la configuration de Build. Un exemple typique de ce type de paramètre est un point de terminaison WCF qui est différent pour le test et la production. Dans les didacticiels ultérieurs, vous allez créer des profils de publication nommés test et production. vous avez donc besoin d’un fichier *Web. test. config* et d’un fichier *Web. production. config* .

Les fichiers de transformation qui sont liés aux profils de publication doivent être créés manuellement. Dans **Explorateur de solutions**, cliquez avec le bouton droit sur le projet ContosoUniversity et sélectionnez **ouvrir le dossier dans l’Explorateur Windows**.

![Open_folder_in_Windows_Explorer](deployment-to-a-hosting-provider-web-config-file-transformations-3-of-12/_static/image2.png)

Dans l' **Explorateur Windows**, sélectionnez le fichier *Web. Release. config* , copiez le fichier, puis collez-y deux copies. Renommez ces copies *Web. production. config* et *Web. test. config*, puis fermez l' **Explorateur Windows**.

Dans **Explorateur de solutions**, cliquez sur **Actualiser** pour afficher les nouveaux fichiers.

Sélectionnez les nouveaux fichiers, cliquez avec le bouton droit, puis cliquez sur **inclure dans le projet** dans le menu contextuel.

![Inclusion de fichiers de configuration de test et de production dans le projet](deployment-to-a-hosting-provider-web-config-file-transformations-3-of-12/_static/image3.png)

Pour empêcher le déploiement de ces fichiers, sélectionnez-les dans **Explorateur de solutions**, puis dans la **fenêtre Propriétés** , affectez **la valeur** **aucun**à la propriété **action de génération** . (Les fichiers de transformation basés sur les configurations de build ne peuvent pas être déployés automatiquement.)

Vous êtes maintenant prêt à entrer des transformations *Web. config* dans les fichiers de transformation *Web. config* .

## <a name="limiting-error-log-access-to-administrators"></a>Limitation de l’accès du journal des erreurs aux administrateurs

En cas d’erreur pendant l’exécution de l’application, l’application affiche une page d’erreur générique à la place de la page d’erreur générée par le système et utilise le [package NuGet ELMAH](http://www.hanselman.com/blog/NuGetPackageOfTheWeek7ELMAHErrorLoggingModulesAndHandlersWithSQLServerCompact.aspx) pour la journalisation des erreurs et la création de rapports. L’élément `customErrors` dans le fichier *Web. config* spécifie la page d’erreurs :

[!code-xml[Main](deployment-to-a-hosting-provider-web-config-file-transformations-3-of-12/samples/sample1.xml)]

Pour afficher la page d’erreur, modifiez temporairement l’attribut `mode` de l’élément `customErrors` de « RemoteOnly » en « on » et exécutez l’application à partir de Visual Studio. Provoque une erreur en demandant une URL non valide, telle que *Studentsxxx. aspx*. Au lieu d’une page d’erreur générée par IIS « page introuvable », vous voyez la page *GenericErrorPage. aspx* .

[![Error_page](deployment-to-a-hosting-provider-web-config-file-transformations-3-of-12/_static/image5.png)](deployment-to-a-hosting-provider-web-config-file-transformations-3-of-12/_static/image4.png)

Pour afficher le journal des erreurs, remplacez tout ce qui se trouve dans l’URL après le numéro de port par *ELMAH. axd* (pour l’exemple dans la capture d’écran, `http://localhost:51130/elmah.axd`) et appuyez sur entrée :

[![Elmah_log_page](deployment-to-a-hosting-provider-web-config-file-transformations-3-of-12/_static/image7.png)](deployment-to-a-hosting-provider-web-config-file-transformations-3-of-12/_static/image6.png)

N’oubliez pas de rétablir le mode « RemoteOnly » de l’élément `customErrors` lorsque vous avez terminé.

Sur votre ordinateur de développement, il est commode d’autoriser un accès gratuit à la page du journal des erreurs, mais en production qui serait un risque pour la sécurité. Pour le site de production, vous pouvez ajouter une règle d’autorisation qui limite l’accès au journal des erreurs uniquement aux administrateurs en configurant une transformation dans le fichier *Web. production. config* .

Ouvrez le *fichier Web. production. config* et ajoutez un nouvel élément `location` immédiatement après la balise d’ouverture `configuration`, comme indiqué ici. (Assurez-vous que vous ajoutez uniquement l’élément `location` et non le balisage environnant qui est indiqué ici uniquement pour fournir un contexte.)

[!code-xml[Main](deployment-to-a-hosting-provider-web-config-file-transformations-3-of-12/samples/sample2.xml?highlight=2-9)]

La valeur d’attribut `Transform` « Insert » entraîne l’ajout de cet élément `location` en tant que frère aux éléments `location` existants dans le fichier *Web. config* . (Il existe déjà un élément `location` qui spécifie les règles d’autorisation pour la page **mettre à jour les crédits** .) Lorsque vous testez le site de production après le déploiement, vous devez effectuer un test pour vérifier que cette règle d’autorisation est effective.

Vous n’avez pas besoin de restreindre l’accès au journal des erreurs dans l’environnement de test. vous n’avez donc pas besoin d’ajouter ce code au fichier *Web. test. config* .

> [!NOTE] 
> 
> **Note de sécurité** N’affichez jamais les détails de l’erreur au public dans une application de production ou stockez ces informations dans un emplacement public. Les attaquants peuvent utiliser les informations d’erreur pour détecter les vulnérabilités dans un site. Si vous utilisez ELMAH dans votre propre application, veillez à étudier les façons dont ELMAH peut être configuré pour réduire les risques de sécurité. L’exemple ELMAH dans ce didacticiel ne doit pas être considéré comme une configuration recommandée. C’est un exemple qui a été choisi afin d’illustrer comment gérer un dossier dans lequel l’application doit pouvoir créer des fichiers.

## <a name="setting-an-environment-indicator"></a>Définition d’un indicateur d’environnement

Un scénario courant consiste à avoir des paramètres de fichier *Web. config* qui doivent être différents dans chaque environnement sur lequel vous déployez. Par exemple, une application qui appelle un service WCF peut avoir besoin d’un point de terminaison différent dans les environnements de test et de production. L’application Contoso University comprend également un paramètre de ce type. Ce paramètre contrôle un indicateur visible sur les pages d’un site qui vous indique l’environnement dans lequel vous vous trouvez, tel que le développement, le test ou la production. La valeur du paramètre détermine si l’application ajoute « (dev) » ou « (test) » à l’en-tête principal de la page maître de *site. Master* :

[![Environment_indicator](deployment-to-a-hosting-provider-web-config-file-transformations-3-of-12/_static/image9.png)](deployment-to-a-hosting-provider-web-config-file-transformations-3-of-12/_static/image8.png)

L’indicateur d’environnement est omis lorsque l’application s’exécute en production.

Les pages Web de Contoso University lisent une valeur définie dans `appSettings` dans le fichier *Web. config* afin de déterminer l’environnement dans lequel l’application s’exécute :

[!code-xml[Main](deployment-to-a-hosting-provider-web-config-file-transformations-3-of-12/samples/sample3.xml)]

La valeur doit être « test » dans l’environnement de test, et « prod » dans l’environnement de production.

Ouvrez *Web. production. config* et ajoutez un élément `appSettings` immédiatement avant la balise d’ouverture de l’élément `location` que vous avez ajouté précédemment :

[!code-xml[Main](deployment-to-a-hosting-provider-web-config-file-transformations-3-of-12/samples/sample4.xml)]

La `xdt:Transform` valeur d’attribut « SetAttributes » indique que l’objectif de cette transformation est de modifier les valeurs d’attribut d’un élément existant dans le fichier *Web. config* . La valeur de l’attribut `xdt:Locator` « match (Key) » indique que l’élément à modifier est celui dont l’attribut `key` correspond à l’attribut `key` spécifié ici. Le seul autre attribut de l’élément `add` est `value`et c’est ce qui sera modifié dans le fichier *Web. config* déployé. Ce code fait en sorte que l’attribut `value` de l’élément `appSettings` `Environment` soit défini sur « prod » dans le fichier *Web. config* déployé en production.

Ensuite, appliquez la même modification au fichier *Web. test. config* , sauf si vous affectez à la `value` la valeur « test » au lieu de « prod ». Lorsque vous avez terminé, la section `appSettings` dans *Web. test. config* se présente comme dans l’exemple suivant :

[!code-xml[Main](deployment-to-a-hosting-provider-web-config-file-transformations-3-of-12/samples/sample5.xml)]

## <a name="disabling-debug-mode"></a>Désactivation du mode débogage

Pour une version Release, vous ne souhaitez pas que le débogage soit activé quel que soit l’environnement sur lequel vous effectuez le déploiement. Par défaut, le fichier de transformation *Web. Release. config* est automatiquement créé avec le code qui supprime l’attribut `debug` de l’élément `compilation` :

[!code-xml[Main](deployment-to-a-hosting-provider-web-config-file-transformations-3-of-12/samples/sample6.xml)]

L’attribut `Transform` entraîne l’omission de l’attribut `debug` dans le fichier *Web. config* déployé chaque fois que vous déployez une version Release.

Cette même transformation est dans les fichiers de transformation de test et de production, car vous les avez créés en copiant le fichier de transformation de la version. Vous n’avez pas besoin de la Dupliquer. Ouvrez chacun de ces fichiers, supprimez l’élément de **compilation** , puis enregistrez et fermez chaque fichier.

## <a name="setting-connection-strings"></a>Définition des chaînes de connexion

Dans la plupart des cas, vous n’avez pas besoin de configurer des transformations de chaîne de connexion, car vous pouvez spécifier des chaînes de connexion dans le profil de publication. Toutefois, il existe une exception quand vous déployez une base de données SQL Server Compact et que vous utilisez Migrations Entity Framework Code First pour mettre à jour la base de données sur le serveur de destination. Pour ce scénario, vous devez spécifier une chaîne de connexion supplémentaire qui sera utilisée sur le serveur pour mettre à jour le schéma de la base de données. Pour configurer cette transformation, ajoutez un élément **&lt;connectionStrings&gt;** immédiatement après la balise d’ouverture **&lt;&gt;de configuration** dans les fichiers de transformation *Web. test. config* et *Web. production. config* :

[!code-xml[Main](deployment-to-a-hosting-provider-web-config-file-transformations-3-of-12/samples/sample7.xml)]

L’attribut `Transform` spécifie que cette chaîne de connexion sera ajoutée à l’élément *connectionStrings* dans le fichier *Web. config* déployé. (Le processus de publication crée automatiquement cette chaîne de connexion supplémentaire si elle n’existe pas, mais l’attribut **providerName** est défini par défaut sur `System.Data.SqlClient`, ce qui ne fonctionne pas pour SQL Server Compact. En ajoutant la chaîne de connexion manuellement, vous empêchez le processus de déploiement de créer un élément de chaîne de connexion avec un nom de fournisseur incorrect.)

Vous avez maintenant spécifié toutes les transformations *Web. config* dont vous avez besoin pour déployer l’application Contoso University à des fins de test et de production. Dans le didacticiel suivant, vous devez prendre en charge les tâches de configuration du déploiement qui requièrent la définition des propriétés du projet.

## <a name="more-information"></a>Plus d'informations

Pour plus d’informations sur les sujets traités dans ce didacticiel, consultez le scénario de transformation Web. config dans le [plan de contenu de déploiement ASP.net](https://msdn.microsoft.com/library/bb386521.aspx).

> [!div class="step-by-step"]
> [Précédent](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12.md)
> [Suivant](deployment-to-a-hosting-provider-configuring-project-properties-4-of-12.md)
