---
uid: web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-configuring-project-properties-4-of-12
title: 'Déploiement d’une application Web ASP.NET avec SQL Server Compact à l’aide de Visual Studio ou Visual Web Developer : configuration des propriétés d’un projet-4 sur 12 | Microsoft Docs'
author: tdykstra
description: Cette série de didacticiels vous montre comment déployer (publier) un projet d’application Web ASP.NET qui comprend une base de données SQL Server Compact à l’aide de Stu...
ms.author: riande
ms.date: 11/17/2011
ms.assetid: 8b013630-842c-4d44-a6fc-c6be43e7210f
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-configuring-project-properties-4-of-12
msc.type: authoredcontent
ms.openlocfilehash: 6e63e75dca3d776fb9a1bd7e420ef48891daac69
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/28/2019
ms.locfileid: "74569811"
---
# <a name="deploying-an-aspnet-web-application-with-sql-server-compact-using-visual-studio-or-visual-web-developer-configuring-project-properties---4-of-12"></a>Déploiement d’une application Web ASP.NET avec SQL Server Compact à l’aide de Visual Studio ou Visual Web Developer : configuration des propriétés d’un projet-4 sur 12

par [Tom Dykstra](https://github.com/tdykstra)

[Télécharger le projet de démarrage](https://code.msdn.microsoft.com/Deploying-an-ASPNET-Web-4e31366b)

> Cette série de didacticiels vous montre comment déployer (publier) un projet d’application Web ASP.NET qui comprend une base de données SQL Server Compact à l’aide de Visual Studio 2012 RC ou Visual Studio Express 2012 RC pour le Web. Vous pouvez également utiliser Visual Studio 2010 si vous installez la mise à jour de publication Web. Pour obtenir une présentation de la série, consultez [le premier didacticiel de la série](deployment-to-a-hosting-provider-introduction-1-of-12.md).
> 
> Pour obtenir un didacticiel qui présente les fonctionnalités de déploiement introduites après la version RC de Visual Studio 2012, montre comment déployer des éditions SQL Server autres que SQL Server Compact et montre comment déployer vers Azure App Service Web Apps, consultez [déploiement Web ASP.net à l’aide de Visual Studio](../../deployment/visual-studio-web-deployment/introduction.md).

## <a name="overview"></a>Vue d'ensemble de

Certaines options de déploiement sont configurées dans les propriétés du projet qui sont stockées dans le fichier projet (fichier *. csproj* ou *. vbproj* ). Dans la plupart des cas, les valeurs par défaut de ces paramètres sont celles que vous souhaitez, mais vous pouvez utiliser l’interface utilisateur des **Propriétés du projet** intégrée à Visual Studio pour utiliser ces paramètres si vous devez les modifier. Dans ce didacticiel, vous allez examiner les paramètres de déploiement dans les **Propriétés du projet**. Vous créez également un fichier d’espace réservé qui entraîne le déploiement d’un dossier vide.

## <a name="configuring-deployment-settings-in-the-project-properties-window"></a>Configuration des paramètres de déploiement dans la fenêtre Propriétés du projet

La plupart des paramètres qui affectent ce qui se produit pendant le déploiement sont inclus dans le profil de publication, comme vous le verrez dans les didacticiels suivants. Certains paramètres que vous devez connaître se trouvent dans les onglets **Package/Publication** de la fenêtre **Propriétés du projet** . Ces paramètres sont spécifiés pour chaque configuration de build, autrement dit, vous pouvez avoir des paramètres différents pour une version Release que vous avez pour une version Debug.

Dans **Explorateur de solutions**, cliquez avec le bouton droit sur le projet **ContosoUniversity** , sélectionnez **Propriétés**, puis sélectionnez l’onglet **Package/Publication Web** .

![Package_Publish_Web_tab](deployment-to-a-hosting-provider-configuring-project-properties-4-of-12/_static/image1.png)

Quand la fenêtre est affichée, elle affiche par défaut les paramètres de la configuration de build actuellement active pour la solution. Si la zone de **configuration** n’indique pas **active (version Release)** , sélectionnez mettre en **sortie** pour afficher les paramètres de la configuration de build Release. Vous allez déployer des builds de version dans vos environnements de test et de production.

![Package_Publish_Web_tab_selecting_Release](deployment-to-a-hosting-provider-configuring-project-properties-4-of-12/_static/image2.png)

Avec l’option **active (version finale)** ou **mise en sortie** sélectionnée, vous voyez les valeurs qui sont effectives lorsque vous déployez à l’aide de la configuration de build Release :

- Dans la zone **éléments à déployer** , **seuls les fichiers nécessaires à l’exécution de l’application** sont sélectionnés. D’autres options sont **tous les fichiers de ce projet** ou **tous les fichiers de ce dossier de projet**. En laissant la sélection par défaut inchangée, vous évitez de déployer les fichiers de code source, par exemple. Ce paramètre est la raison pour laquelle les dossiers qui contiennent le SQL Server Compact fichiers binaires devaient être inclus dans le projet. Pour plus d’informations sur ce paramètre, consultez **pourquoi tous les fichiers de mon dossier de projet ne sont-ils pas déployés ?** dans le [Forum aux questions sur le déploiement de projet d’application Web ASP.net](https://msdn.microsoft.com/library/ee942158.aspx).
- L’option **exclure les symboles de débogage générés** est sélectionnée. Vous ne devez pas déboguer lorsque vous utilisez cette configuration de Build.
- **L’option exclure les fichiers du dossier de données de l’application\_** n’est pas sélectionnée. Votre fichier SQL Server Compact pour la base de données d’appartenance se trouve dans ce dossier et vous devez le déployer. Lorsque vous déployez des mises à jour qui n’incluent pas de modifications de base de données, vous activez cette case à cocher.
- **Précompiler cette application avant la publication** n’est pas sélectionnée. Dans la plupart des scénarios, il n’est pas nécessaire de précompiler des projets d’application Web. Pour plus d’informations sur cette option, consultez [onglet Package/Publication Web, propriétés du projet](https://msdn.microsoft.com/library/dd410108(v=vs.110).aspx) et [paramètres de précompilation avancés](https://msdn.microsoft.com/library/hh475319(v=vs.110).aspx).
- L’option **inclure toutes les bases de données configurées dans l’onglet Package/Publication SQL** est sélectionnée, mais cette option n’a aucun effet maintenant, car vous ne configurez pas l’onglet **Package/Publication SQL** . Cet onglet est destiné à une méthode de déploiement de base de données héritée qui était la seule option pour le déploiement de bases de données SQL Server. Vous allez utiliser l’onglet **Package/Publication SQL** dans le didacticiel [migration vers SQL Server](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12.md) .
- La section des **paramètres du package de déploiement Web** ne s’applique pas, car vous utilisez la publication en un clic dans ces didacticiels.

Modifiez la zone de liste déroulante **configuration** pour déboguer pour afficher les paramètres par défaut pour les versions Debug. Les valeurs sont les mêmes, à la différence près que l' **exclusion des symboles de débogage générés** est désactivée pour que vous puissiez déboguer lorsque vous déployez une version Debug.

## <a name="making-sure-that-the-elmah-folder-gets-deployed"></a>Vérification du déploiement du dossier ELMAH

Comme vous l’avez vu dans le didacticiel précédent, le [package NuGet ELMAH](http://www.hanselman.com/blog/NuGetPackageOfTheWeek7ELMAHErrorLoggingModulesAndHandlersWithSQLServerCompact.aspx) fournit des fonctionnalités de journalisation des erreurs et de création de rapports. Dans l’application Contoso University, ELMAH a été configuré pour stocker les détails de l’erreur dans un dossier nommé *ELMAH*:

![Dossier ELMAH](deployment-to-a-hosting-provider-configuring-project-properties-4-of-12/_static/image3.png)

L’exclusion de fichiers ou dossiers spécifiques du déploiement est une exigence courante. un autre exemple est un dossier dans lequel les utilisateurs peuvent charger des fichiers. Vous ne souhaitez pas que les fichiers journaux ou les fichiers téléchargés qui ont été créés dans votre environnement de développement soient déployés en production. Et si vous déployez une mise à jour en production, vous ne souhaitez pas que le processus de déploiement supprime les fichiers qui existent en production. (Selon la façon dont vous définissez une option de déploiement, si un fichier existe sur le site de destination, mais pas sur le site source lorsque vous déployez, Web Deploy le supprime de la destination.)

Comme vous l’avez vu précédemment dans ce didacticiel, l’option **éléments à déployer** de l’onglet **Package/Publication Web** est définie sur **uniquement les fichiers nécessaires à l’exécution de cette application**. Par conséquent, les fichiers journaux créés par ELMAH dans le développement ne seront pas déployés, ce que vous souhaitez faire. (À déployer, ils doivent être inclus dans le projet et leur propriété d' **action de génération** doit être définie sur **content**. Pour plus d’informations, consultez **pourquoi tous les fichiers de mon dossier de projet ne sont-ils pas déployés ?** dans le [FAQ sur le déploiement d’un projet d’application Web ASP.net](https://msdn.microsoft.com/library/ee942158.aspx)). Toutefois, Web Deploy ne crée pas de dossier dans le site de destination, à moins qu’il y ait au moins un fichier à copier. Par conséquent, vous allez ajouter un fichier *. txt* au dossier pour qu’il serve d’espace réservé afin que le dossier soit copié.

Dans **Explorateur de solutions**, cliquez avec le bouton droit sur le dossier *ELMAH* , sélectionnez **Ajouter un nouvel élément**, puis créez un fichier nommé *PlaceHolder. txt*. Placez le texte suivant : « il s’agit d’un fichier d’espace réservé pour garantir que le dossier est déployé ». et enregistrez le fichier. C’est tout ce que vous devez faire pour vous assurer que Visual Studio déploie ce fichier et le dossier dans lequel il se trouve, car la propriété **action de génération** des fichiers *. txt* est définie sur **contenu** par défaut.

Vous avez maintenant terminé toutes les tâches de configuration du déploiement. Dans le didacticiel suivant, vous allez déployer le site Contoso University dans l’environnement de test et le tester.

> [!div class="step-by-step"]
> [Précédent](deployment-to-a-hosting-provider-web-config-file-transformations-3-of-12.md)
> [Suivant](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12.md)
