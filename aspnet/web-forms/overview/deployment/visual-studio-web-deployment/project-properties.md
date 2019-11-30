---
uid: web-forms/overview/deployment/visual-studio-web-deployment/project-properties
title: 'Déploiement Web ASP.NET à l’aide de Visual Studio : propriétés du projet | Microsoft Docs'
author: tdykstra
description: Cette série de didacticiels vous montre comment déployer (publier) une application Web ASP.NET sur Azure App Service Web Apps ou sur un fournisseur d’hébergement tiers, par utilisez...
ms.author: riande
ms.date: 02/15/2013
ms.assetid: ec1cec4c-a75f-47af-a2ba-b1e2f971d24b
msc.legacyurl: /web-forms/overview/deployment/visual-studio-web-deployment/project-properties
msc.type: authoredcontent
ms.openlocfilehash: b2811791a897c9166f6222c23dddc6921e5267ab
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/28/2019
ms.locfileid: "74614952"
---
# <a name="aspnet-web-deployment-using-visual-studio-project-properties"></a>Déploiement Web ASP.NET à l’aide de Visual Studio : propriétés du projet

par [Tom Dykstra](https://github.com/tdykstra)

[Télécharger le projet de démarrage](https://go.microsoft.com/fwlink/p/?LinkId=282627)

> Cette série de didacticiels vous montre comment déployer (publier) une application Web ASP.NET sur Azure App Service Web Apps ou sur un fournisseur d’hébergement tiers, à l’aide de Visual Studio 2012 ou de Visual Studio 2010. Pour plus d’informations sur la série, consultez [le premier didacticiel de la série](introduction.md).

## <a name="overview"></a>Vue d'ensemble de

Certaines options de déploiement sont configurées dans les propriétés du projet qui sont stockées dans le fichier projet (fichier *. csproj* ou *. vbproj* ). Dans la plupart des cas, les valeurs par défaut de ces paramètres sont celles que vous souhaitez, mais vous pouvez utiliser l’interface utilisateur des **Propriétés du projet** intégrée à Visual Studio pour utiliser ces paramètres si vous devez les modifier. Dans ce didacticiel, vous allez examiner les paramètres de déploiement dans les **Propriétés du projet**. Vous créez également un fichier d’espace réservé qui entraîne le déploiement d’un dossier vide.

## <a name="configure-deployment-settings-in-the-project-properties-window"></a>Configurer les paramètres de déploiement dans la fenêtre Propriétés du projet

La plupart des paramètres qui affectent ce qui se produit pendant le déploiement sont inclus dans le profil de publication, comme vous le verrez dans les didacticiels suivants. Certains paramètres que vous devez connaître se trouvent dans les onglets **Package/Publication** de la fenêtre **Propriétés du projet** . Ces paramètres sont spécifiés pour chaque configuration de build, autrement dit, vous pouvez avoir des paramètres différents pour une version Release que vous avez pour une version Debug.

Dans **Explorateur de solutions**, cliquez avec le bouton droit sur le projet **ContosoUniversity** , sélectionnez **Propriétés**, puis sélectionnez l’onglet **Package/Publication Web** .

![Onglet Package/Publication web](project-properties/_static/image1.png)

Quand la fenêtre est affichée, elle affiche par défaut les paramètres de la configuration de build actuellement active pour la solution. Si la zone de **configuration** n’indique pas **active (version Release)** , sélectionnez mettre en **sortie** pour afficher les paramètres de la configuration de build Release. Vous allez déployer des builds de version dans vos environnements de test et de production.

![Sélection de la configuration de build Release](project-properties/_static/image2.png)

Avec l’option **active (version finale)** ou **mise en sortie** sélectionnée, vous voyez les valeurs qui sont effectives lorsque vous déployez à l’aide de la configuration de build Release :

- Dans la zone **éléments à déployer** , **seuls les fichiers nécessaires à l’exécution de l’application** sont sélectionnés. D’autres options sont **tous les fichiers de ce projet** ou **tous les fichiers de ce dossier de projet**. En laissant la sélection par défaut inchangée, vous évitez de déployer les fichiers de code source, par exemple. Ce paramètre est la raison pour laquelle les dossiers qui contiennent le SQL Server Compact fichiers binaires devaient être inclus dans le projet. Pour plus d’informations sur ce paramètre, consultez **pourquoi tous les fichiers de mon dossier de projet ne sont-ils pas déployés ?** dans le [Forum aux questions sur le déploiement de projet d’application Web ASP.net](https://msdn.microsoft.com/library/ee942158.aspx).
- L’option **exclure les symboles de débogage générés** est sélectionnée. Vous ne devez pas déboguer lorsque vous utilisez cette configuration de Build.
- **L’option inclure toutes les bases de données configurées dans l’onglet Package/Publication SQL** est sélectionnée. Spécifie si Visual Studio doit déployer les bases de données ainsi que les fichiers. Bien que l’étiquette de case à cocher mentionne uniquement l’onglet **Package/Publication SQL** , la désactivation de cette case à cocher désactive également le déploiement de base de données configuré dans le profil de publication. Vous effectuerez cette opération ultérieurement. la case à cocher doit donc rester activée. L’onglet **Package/Publication SQL** est utilisé pour une méthode de publication de base de données héritée que vous n’utiliserez pas dans ces didacticiels.
- La section des **paramètres du package de déploiement Web** ne s’applique pas, car vous utilisez la publication en un clic dans ces didacticiels.

Modifiez la zone de liste déroulante **configuration** pour déboguer pour afficher les paramètres par défaut pour les versions Debug. Les valeurs sont les mêmes, à la différence près que l' **exclusion des symboles de débogage générés** est désactivée pour que vous puissiez déboguer lorsque vous déployez une version Debug.

## <a name="make-sure-that-the-elmah-folder-gets-deployed"></a>Assurez-vous que le dossier ELMAH est déployé

Comme vous l’avez vu dans le didacticiel précédent, le [package NuGet ELMAH](http://www.hanselman.com/blog/NuGetPackageOfTheWeek7ELMAHErrorLoggingModulesAndHandlersWithSQLServerCompact.aspx) fournit des fonctionnalités de journalisation des erreurs et de création de rapports. Dans l’application Contoso University, ELMAH a été configuré pour stocker les détails de l’erreur dans un dossier nommé *ELMAH*:

![Dossier ELMAH](project-properties/_static/image3.png)

L’exclusion de fichiers ou dossiers spécifiques du déploiement est une exigence courante. un autre exemple est un dossier dans lequel les utilisateurs peuvent charger des fichiers. Vous ne souhaitez pas que les fichiers journaux ou les fichiers téléchargés qui ont été créés dans votre environnement de développement soient déployés en production. Et si vous déployez une mise à jour en production, vous ne souhaitez pas que le processus de déploiement supprime les fichiers qui existent en production. (Selon la façon dont vous définissez une option de déploiement, si un fichier existe sur le site de destination, mais pas sur le site source lorsque vous déployez, Web Deploy le supprime de la destination.)

Comme vous l’avez vu précédemment dans ce didacticiel, l’option **éléments à déployer** de l’onglet **Package/Publication Web** est définie sur **uniquement les fichiers nécessaires à l’exécution de cette application**. Par conséquent, les fichiers journaux créés par ELMAH dans le développement ne seront pas déployés, ce que vous souhaitez faire. (À déployer, ils doivent être inclus dans le projet et leur propriété d' **action de génération** doit être définie sur **content**. Pour plus d’informations, consultez **pourquoi tous les fichiers de mon dossier de projet ne sont-ils pas déployés ?** dans le [FAQ sur le déploiement d’un projet d’application Web ASP.net](https://msdn.microsoft.com/library/ee942158.aspx)). Toutefois, Web Deploy ne crée pas de dossier dans le site de destination, à moins qu’il y ait au moins un fichier à copier. Par conséquent, vous allez ajouter un fichier *. txt* au dossier pour qu’il serve d’espace réservé afin que le dossier soit copié.

Dans **Explorateur de solutions**, cliquez avec le bouton droit sur le dossier *ELMAH* , sélectionnez **Ajouter un nouvel élément**, puis créez un fichier texte nommé *PlaceHolder. txt*. Placez le texte suivant : « il s’agit d’un fichier d’espace réservé pour garantir que le dossier est déployé ». et enregistrez le fichier. C’est tout ce que vous devez faire pour vous assurer que Visual Studio déploie ce fichier et le dossier dans lequel il se trouve, car la propriété **action de génération** des fichiers *. txt* est définie sur **contenu** par défaut.

## <a name="summary"></a>Récapitulatif

Vous avez maintenant terminé toutes les tâches de configuration du déploiement. Dans le didacticiel suivant, vous allez déployer le site Contoso University dans l’environnement de test et le tester.

> [!div class="step-by-step"]
> [Précédent](web-config-transformations.md)
> [Suivant](deploying-to-iis.md)
