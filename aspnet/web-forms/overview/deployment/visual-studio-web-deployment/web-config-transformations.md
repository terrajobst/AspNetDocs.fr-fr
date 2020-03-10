---
uid: web-forms/overview/deployment/visual-studio-web-deployment/web-config-transformations
title: 'Déploiement Web ASP.NET à l’aide de Visual Studio : transformations de fichiers Web. config | Microsoft Docs'
author: tdykstra
description: Cette série de didacticiels vous montre comment déployer (publier) une application Web ASP.NET sur Azure App Service Web Apps ou sur un fournisseur d’hébergement tiers, par utilisez...
ms.author: riande
ms.date: 02/15/2013
ms.assetid: 5a2a927b-14cb-40bc-867a-f0680f9febd7
msc.legacyurl: /web-forms/overview/deployment/visual-studio-web-deployment/web-config-transformations
msc.type: authoredcontent
ms.openlocfilehash: a9d39547c94a63003442ba6fe1257693dde24b05
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78632831"
---
# <a name="aspnet-web-deployment-using-visual-studio-webconfig-file-transformations"></a>Déploiement Web ASP.NET à l’aide de Visual Studio : transformations de fichiers Web. config

par [Tom Dykstra](https://github.com/tdykstra)

[Télécharger le projet de démarrage](https://go.microsoft.com/fwlink/p/?LinkId=282627)

> Cette série de didacticiels vous montre comment déployer (publier) une application Web ASP.NET sur Azure App Service Web Apps ou sur un fournisseur d’hébergement tiers, à l’aide de Visual Studio 2012 ou de Visual Studio 2010. Pour plus d’informations sur la série, consultez [le premier didacticiel de la série](introduction.md).

## <a name="overview"></a>Présentation

Ce didacticiel vous montre comment automatiser le processus de modification du fichier *Web. config* lorsque vous le déployez dans différents environnements de destination. La plupart des applications ont des paramètres dans le fichier *Web. config* qui doivent être différents lorsque l’application est déployée. L’automatisation du processus d’apport de ces modifications vous évite de devoir les exécuter manuellement à chaque fois que vous déployez, ce qui serait fastidieux et sujet aux erreurs.

Rappel : Si vous recevez un message d’erreur ou si une action ne fonctionne pas au fur et à mesure que vous parcourez le didacticiel, veillez à consulter la [page de résolution des problèmes](troubleshooting.md).

## <a name="webconfig-transformations-versus-web-deploy-parameters"></a>Transformations Web. config et paramètres de Web Deploy

Il existe deux façons d’automatiser le processus de modification des paramètres du fichier *Web. config* : les [transformations Web. config](https://msdn.microsoft.com/library/dd465326.aspx) et les [paramètres de Web Deploy](https://msdn.microsoft.com/library/ff398068.aspx). Un fichier de transformation *Web. config* contient un balisage XML qui spécifie comment modifier le fichier *Web. config* lors de son déploiement. Vous pouvez spécifier des modifications différentes pour des configurations de build spécifiques et pour des profils de publication spécifiques. Les configurations de build par défaut sont Debug et Release, et vous pouvez créer des configurations de build personnalisées. Un profil de publication correspond généralement à un environnement de destination. (Pour plus d’informations sur les profils de publication, consultez le didacticiel [déploiement sur IIS en tant qu’environnement de test](deploying-to-iis.md) .)

Web Deploy paramètres peuvent être utilisés pour spécifier de nombreux types de paramètres qui doivent être configurés lors du déploiement, y compris les paramètres qui se trouvent dans les fichiers *Web. config* . Lorsqu’ils sont utilisés pour spécifier des modifications de fichier *Web. config* , les paramètres de Web Deploy sont plus complexes à configurer, mais ils sont utiles lorsque vous ne connaissez pas la valeur à définir tant que vous n’avez pas déployé. Par exemple, dans un environnement d’entreprise, vous pouvez créer un *package de déploiement* et le transmettre à une personne du service informatique pour l’installer en production, et cette personne doit être en mesure d’entrer des chaînes de connexion ou des mots de passe que vous ne connaissez pas.

Pour le scénario couvert par cette série de didacticiels, vous savez à l’avance tout ce qui doit être fait dans le fichier *Web. config.* vous n’avez donc pas besoin d’utiliser des paramètres de Web Deploy. Vous allez configurer certaines transformations qui diffèrent en fonction de la configuration de build utilisée, et d’autres qui diffèrent en fonction du profil de publication utilisé.

<a id="watransforms"></a>

## <a name="specifying-webconfig-settings-in-azure"></a>Spécification des paramètres Web. config dans Azure

Si les paramètres du fichier *Web. config* que vous souhaitez modifier se trouvent dans le `<connectionStrings>` ou l’élément `<appSettings>`, et si vous déployez sur Web Apps dans Azure App service, vous disposez d’une autre option pour automatiser les modifications au cours du déploiement. Vous pouvez entrer les paramètres que vous souhaitez appliquer dans Azure sous l’onglet **configurer** de la page du portail de gestion pour votre application Web (faites défiler jusqu’à la section **paramètres d’application** et **chaînes de connexion** ). Lorsque vous déployez le projet, Azure applique automatiquement les modifications. Pour plus d’informations, consultez [sites Web Windows Azure : fonctionnement des chaînes d’application et des chaînes de connexion](https://blogs.msdn.com/b/windowsazure/archive/2013/07/17/windows-azure-web-sites-how-application-strings-and-connection-strings-work.aspx).

## <a name="default-transformation-files"></a>Fichiers de transformation par défaut

Dans **Explorateur de solutions**, développez *Web. config* pour voir les fichiers de transformation *Web. Debug. config* et *Web. Release. config* qui sont créés par défaut pour les deux configurations de build par défaut.

![Web.config_transform_files](web-config-transformations/_static/image1.png)

Vous pouvez créer des fichiers de transformation pour les configurations de build personnalisées en cliquant avec le bouton droit sur le fichier Web. config et en choisissant **Ajouter des transformations de configuration** dans le menu contextuel. Pour ce didacticiel, vous n’avez pas besoin de le faire, et l’option de menu est désactivée, car vous n’avez créé aucune configuration de build personnalisée.

Plus tard, vous allez créer trois fichiers de transformation supplémentaires, un pour chaque profil de publication de test, intermédiaire et de production. Un exemple classique de paramètre que vous géreriez dans un fichier de transformation de profil de publication, car il dépend de l’environnement de destination est un point de terminaison WCF qui est différent pour le test et la production. Vous allez créer des fichiers de transformation de profil de publication dans des didacticiels ultérieurs après avoir créé les profils de publication avec lesquels ils sont associés.

## <a name="disable-debug-mode"></a>Désactiver le mode débogage

L’attribut `debug` est un exemple de paramètre qui dépend de la configuration de build plutôt que de l’environnement de destination. Pour une version Release, vous souhaitez généralement désactiver le débogage quel que soit l’environnement sur lequel vous effectuez le déploiement. Par conséquent, par défaut, les modèles de projet Visual Studio créent des fichiers de transformation *Web. Release. config* contenant du code qui supprime l’attribut `debug` de l’élément `compilation`. Voici le *fichier Web. Release. config*par défaut : en plus d’un exemple de code de transformation qui est commenté, il comprend du code dans l’élément `compilation` qui supprime l’attribut `debug` :

[!code-xml[Main](web-config-transformations/samples/sample1.xml?highlight=18)]

L’attribut `xdt:Transform="RemoveAttributes(debug)"` spécifie que vous souhaitez que l’attribut `debug` soit supprimé de l’élément `system.web/compilation` dans le fichier *Web. config* déployé. Cette opération sera effectuée chaque fois que vous déploierez une version Release.

## <a name="limit-error-log-access-to-administrators"></a>Limiter l’accès du journal des erreurs aux administrateurs

En cas d’erreur pendant l’exécution de l’application, l’application affiche une page d’erreur générique à la place de la page d’erreur générée par le système et utilise le [package NuGet ELMAH](http://www.hanselman.com/blog/NuGetPackageOfTheWeek7ELMAHErrorLoggingModulesAndHandlersWithSQLServerCompact.aspx) pour la journalisation des erreurs et la création de rapports. L’élément `customErrors` dans le fichier *Web. config* de l’application spécifie la page d’erreurs :

[!code-xml[Main](web-config-transformations/samples/sample2.xml)]

Pour afficher la page d’erreur, modifiez temporairement l’attribut `mode` de l’élément `customErrors` de « RemoteOnly » en « on » et exécutez l’application à partir de Visual Studio. Provoque une erreur en demandant une URL non valide, telle que *Studentsxxx. aspx*. Au lieu d’une page d’erreur générée par IIS « la ressource est introuvable », vous voyez la page *GenericErrorPage. aspx* .

![Page d’erreur](web-config-transformations/_static/image2.png)

Pour afficher le journal des erreurs, remplacez tout ce qui se trouve dans l’URL après le numéro de port par *ELMAH. axd* (par exemple, `http://localhost:51130/elmah.axd`) et appuyez sur entrée :

![Page ELMAH](web-config-transformations/_static/image3.png)

N’oubliez pas de rétablir le mode « RemoteOnly » de l’élément `customErrors` lorsque vous avez terminé.

Sur votre ordinateur de développement, il est commode d’autoriser un accès gratuit à la page du journal des erreurs, mais en production qui serait un risque pour la sécurité. Pour le site de production, vous souhaitez ajouter une règle d’autorisation qui limite l’accès du journal des erreurs aux administrateurs et s’assurer que la restriction fonctionne également dans les environnements test et intermédiaire. C’est pourquoi il s’agit d’une autre modification que vous souhaitez implémenter chaque fois que vous déployez une version Release, et donc qu’elle appartient au fichier *Web. Release. config* .

Ouvrez le *fichier Web. Release. config* et ajoutez un nouvel élément `location` immédiatement avant la balise de fermeture `configuration`, comme indiqué ici.

[!code-xml[Main](web-config-transformations/samples/sample3.xml?highlight=27-34)]

La valeur d’attribut `Transform` « Insert » entraîne l’ajout de cet élément `location` en tant que frère aux éléments `location` existants dans le fichier *Web. config* . (Il existe déjà un élément `location` qui spécifie les règles d’autorisation pour la page **mettre à jour les crédits** .)

Vous pouvez maintenant afficher un aperçu de la transformation pour vous assurer que vous l’avez correctement codée.

Dans **Explorateur de solutions**, cliquez avec le bouton droit sur *Web. Release. config* , puis cliquez sur Aperçu de la **transformation**.

![Menu Aperçu de la transformation](web-config-transformations/_static/image4.png)

Une page s’ouvre, qui vous montre le fichier *Web. config* de développement à gauche et à quoi ressemble le fichier *Web. config* déployé à droite, avec les modifications mises en surbrillance.

![Aperçu de la transformation de débogage](web-config-transformations/_static/image5.png)

![Aperçu de la transformation d’emplacement](web-config-transformations/_static/image6.png)

(Dans la version préliminaire, vous remarquerez peut-être des modifications supplémentaires pour lesquelles vous n’avez pas écrit de transformations : celles-ci impliquent généralement la suppression d’espaces qui n’affectent pas les fonctionnalités.)

Lorsque vous testez le site après le déploiement, vous devez également effectuer un test pour vérifier que la règle d’autorisation est effective.

> [!NOTE] 
> 
> **Note de sécurité** N’affichez jamais les détails de l’erreur au public dans une application de production ou stockez ces informations dans un emplacement public. Les attaquants peuvent utiliser les informations d’erreur pour détecter les vulnérabilités dans un site. Si vous utilisez ELMAH dans votre propre application, configurez ELMAH pour réduire les risques de sécurité. L’exemple ELMAH dans ce didacticiel ne doit pas être considéré comme une configuration recommandée. C’est un exemple qui a été choisi afin d’illustrer comment gérer un dossier dans lequel l’application doit pouvoir créer des fichiers. Pour plus d’informations, consultez [sécurisation du point de terminaison ELMAH](https://code.google.com/p/elmah/wiki/SecuringErrorLogPages).

## <a name="a-setting-that-youll-handle-in-publish-profile-transformation-files"></a>Un paramètre que vous gérerez dans les fichiers de transformation de profil de publication

Un scénario courant consiste à avoir des paramètres de fichier *Web. config* qui doivent être différents dans chaque environnement sur lequel vous déployez. Par exemple, une application qui appelle un service WCF peut avoir besoin d’un point de terminaison différent dans les environnements de test et de production. L’application Contoso University comprend également un paramètre de ce type. Ce paramètre contrôle un indicateur visible sur les pages d’un site qui vous indique l’environnement dans lequel vous vous trouvez, tel que le développement, le test ou la production. La valeur du paramètre détermine si l’application ajoute « (dev) » ou « (test) » à l’en-tête principal de la page maître de *site. Master* :

![Indicateur d’environnement](web-config-transformations/_static/image7.png)

L’indicateur d’environnement est omis lorsque l’application est exécutée dans un environnement intermédiaire ou de production.

Les pages Web de Contoso University lisent une valeur définie dans `appSettings` dans le fichier *Web. config* afin de déterminer l’environnement dans lequel l’application s’exécute :

[!code-xml[Main](web-config-transformations/samples/sample4.xml)]

La valeur doit être « test » dans l’environnement de test, et « prod » pour la mise en lots et la production.

Le code suivant dans un fichier de transformation implémente cette transformation :

[!code-xml[Main](web-config-transformations/samples/sample5.xml)]

La `xdt:Transform` valeur d’attribut « SetAttributes » indique que l’objectif de cette transformation est de modifier les valeurs d’attribut d’un élément existant dans le fichier *Web. config* . La valeur de l’attribut `xdt:Locator` « match (Key) » indique que l’élément à modifier est celui dont l’attribut `key` correspond à l’attribut `key` spécifié ici. Le seul autre attribut de l’élément `add` est `value`et c’est ce qui sera modifié dans le fichier *Web. config* déployé. Le code présenté ici entraîne la définition de l’attribut `value` du `Environment` `appSettings` élément sur « test » dans le fichier *Web. config* qui est déployé.

Cette transformation appartient aux fichiers de transformation de profil de publication, que vous n’avez pas encore créés. Vous allez créer et mettre à jour les fichiers de transformation qui implémentent cette modification lorsque vous créez les profils de publication pour les environnements de test, intermédiaire et de production. Vous le ferez dans les didacticiels [déployer sur IIS](deploying-to-iis.md) et [déployer en production](deploying-to-production.md) .

> [!NOTE]
> Étant donné que ce paramètre se trouve dans l’élément `<appSettings>`, vous disposez d’une autre solution pour spécifier la transformation lors du déploiement sur Web Apps dans Azure App Service consultez [spécification des paramètres Web. config dans Azure](#watransforms) précédemment dans cette rubrique.

## <a name="setting-connection-strings"></a>Définition des chaînes de connexion

Bien que le fichier de transformation par défaut contienne un exemple qui montre comment mettre à jour une chaîne de connexion, dans la plupart des cas, vous n’avez pas besoin de configurer des transformations de chaînes de connexion, car vous pouvez spécifier des chaînes de connexion dans le profil de publication. Vous le ferez dans les didacticiels [déployer sur IIS](deploying-to-iis.md) et [déployer en production](deploying-to-production.md) .

## <a name="summary"></a>Récapitulatif

Vous avez maintenant fait autant que possible avec les transformations *Web. config* avant de créer les profils de publication, et vous avez vu un aperçu de ce qui se trouve dans le fichier Web. config déployé.

![Aperçu de la transformation d’emplacement](web-config-transformations/_static/image8.png)

Dans le didacticiel suivant, vous devez prendre en charge les tâches de configuration du déploiement qui requièrent la définition des propriétés du projet.

## <a name="more-information"></a>Plus d'informations

Pour plus d’informations sur les sujets traités dans ce didacticiel, consultez [utilisation des transformations Web. config pour modifier les paramètres dans le fichier Web. config de destination ou le fichier app. config lors du déploiement](https://go.microsoft.com/fwlink/p/?LinkId=282413#transforms) dans le plan de contenu de déploiement Web pour Visual Studio et ASP.net.

> [!div class="step-by-step"]
> [Précédent](preparing-databases.md)
> [Suivant](project-properties.md)
