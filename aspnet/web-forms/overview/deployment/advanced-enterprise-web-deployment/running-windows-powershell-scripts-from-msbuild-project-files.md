---
uid: web-forms/overview/deployment/advanced-enterprise-web-deployment/running-windows-powershell-scripts-from-msbuild-project-files
title: Exécution de scripts Windows PowerShell à partir de fichiers projet MSBuild | Microsoft Docs
author: jrjlee
description: Cette rubrique explique comment exécuter un script Windows PowerShell dans le cadre d’un processus de génération et de déploiement. Vous pouvez exécuter un script localement (en d’autres termes, sur le b...
ms.author: riande
ms.date: 05/04/2012
ms.assetid: 55f1ae45-fcb5-43a9-8415-fa5b935fc9c9
msc.legacyurl: /web-forms/overview/deployment/advanced-enterprise-web-deployment/running-windows-powershell-scripts-from-msbuild-project-files
msc.type: authoredcontent
ms.openlocfilehash: 7b09c07b8b7c2a61ca534f7a66a929593f3d04ca
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78548509"
---
# <a name="running-windows-powershell-scripts-from-msbuild-project-files"></a>Exécution de scripts Windows PowerShell à partir de fichiers projet MSBuild

par [Jason Lee](https://github.com/jrjlee)

[Télécharger PDF](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> Cette rubrique explique comment exécuter un script Windows PowerShell dans le cadre d’un processus de génération et de déploiement. Vous pouvez exécuter un script localement (en d’autres termes, sur le serveur de builds) ou à distance, comme sur un serveur Web de destination ou un serveur de base de données.
> 
> Il existe de nombreuses raisons pour lesquelles vous pouvez souhaiter exécuter un script Windows PowerShell de suite au déploiement. Vous pouvez, par exemple, décider d'effectuer les opérations suivantes :
> 
> - Ajoutez une source d’événement personnalisée au registre.
> - Générez un répertoire de système de fichiers pour les téléchargements.
> - Nettoyez les répertoires de Build.
> - Écrire des entrées dans un fichier journal personnalisé.
> - Envoyer des e-mails invitant les utilisateurs à une application Web nouvellement approvisionnée.
> - Créez des comptes d’utilisateur avec les autorisations appropriées.
> - Configurer la réplication entre des instances de SQL Server.
> 
> Cette rubrique vous montre comment exécuter des scripts Windows PowerShell localement et à distance à partir d’une cible personnalisée dans un fichier projet Microsoft Build Engine (MSBuild).

Cette rubrique fait partie d’une série de didacticiels basés sur les exigences de déploiement en entreprise d’une société fictive nommée Fabrikam, Inc. Cette série de didacticiels utilise un&#x2014;exemple de solution de [gestion des contacts](../web-deployment-in-the-enterprise/the-contact-manager-solution.md)&#x2014;pour représenter une application Web avec un niveau de complexité réaliste, y compris une application ASP.NET MVC 3, un service Windows Communication Foundation (WCF) et un projet de base de données.

La méthode de déploiement au cœur de ces didacticiels est basée sur l’approche de fichier de projet fractionné décrite dans [fonctionnement du fichier projet](../web-deployment-in-the-enterprise/understanding-the-project-file.md), dans lequel le processus de génération est contrôlé&#x2014;par deux fichiers projet qui contiennent des instructions de génération qui s’appliquent à chaque environnement de destination, et un qui contient des paramètres de génération et de déploiement spécifiques à l’environnement. Au moment de la génération, le fichier projet spécifique à l’environnement est fusionné dans le fichier projet indépendant de l’environnement pour former un ensemble complet d’instructions de génération.

## <a name="task-overview"></a>Vue d’ensemble des tâches

Pour exécuter un script Windows PowerShell dans le cadre d’un processus de déploiement automatisé ou en une seule étape, vous devez effectuer ces tâches de haut niveau :

- Ajoutez le script Windows PowerShell à votre solution et au contrôle de code source.
- Créez une commande qui appelle votre script Windows PowerShell.
- Échappez les caractères XML réservés dans votre commande.
- Créez une cible dans votre fichier projet MSBuild personnalisé et utilisez la tâche **Exec** pour exécuter votre commande.

Cette rubrique vous montre comment effectuer ces procédures. Les tâches et procédures pas à pas de cette rubrique partent du principe que vous êtes déjà familiarisé avec les cibles et les propriétés MSBuild et que vous comprenez comment utiliser un fichier projet MSBuild personnalisé pour piloter un processus de génération et de déploiement. Pour plus d’informations, consultez [comprendre le fichier projet](../web-deployment-in-the-enterprise/understanding-the-project-file.md) et [comprendre le processus de génération](../web-deployment-in-the-enterprise/understanding-the-build-process.md).

## <a name="creating-and-adding-windows-powershell-scripts"></a>Création et ajout de scripts Windows PowerShell

Les tâches de cette rubrique utilisent un exemple de script Windows PowerShell nommé **LogDeploy. ps1** pour illustrer comment exécuter des scripts à partir de MSBuild. Le script **LogDeploy. ps1** contient une fonction simple qui écrit une entrée sur une seule ligne dans un fichier journal :

[!code-powershell[Main](running-windows-powershell-scripts-from-msbuild-project-files/samples/sample1.ps1)]

Le script **LogDeploy. ps1** accepte deux paramètres. Le premier paramètre représente le chemin d’accès complet au fichier journal auquel vous souhaitez ajouter une entrée, et le deuxième paramètre représente la destination de déploiement que vous souhaitez enregistrer dans le fichier journal. Lorsque vous exécutez le script, il ajoute une ligne au fichier journal au format suivant :

[!code-html[Main](running-windows-powershell-scripts-from-msbuild-project-files/samples/sample2.html)]

Pour mettre le script **LogDeploy. ps1** à la disposition de MSBuild, vous devez :

- Ajoutez le script au contrôle de code source.
- Ajoutez le script à votre solution dans Visual Studio 2010.

Vous n’avez pas besoin de déployer le script avec le contenu de votre solution, même si vous envisagez d’exécuter le script sur le serveur de builds ou sur un ordinateur distant. L’une des options consiste à ajouter le script à un dossier de solution. Dans l’exemple du gestionnaire de contacts, étant donné que vous souhaitez utiliser le script Windows PowerShell dans le cadre du processus de déploiement, il est judicieux d’ajouter le script au dossier de la solution de publication.

![](running-windows-powershell-scripts-from-msbuild-project-files/_static/image1.png)

Le contenu des dossiers de solution est copié vers les serveurs de builds en tant que matériel source. Toutefois, ils ne font pas partie d’une sortie de projet.

## <a name="executing-a-windows-powershell-script-on-the-build-server"></a>Exécution d’un script Windows PowerShell sur le serveur de builds

Dans certains scénarios, vous pouvez exécuter des scripts Windows PowerShell sur l’ordinateur qui génère vos projets. Par exemple, vous pouvez utiliser un script Windows PowerShell pour nettoyer les dossiers de génération ou écrire des entrées dans un fichier journal personnalisé.

En termes de syntaxe, l’exécution d’un script Windows PowerShell à partir d’un fichier projet MSBuild revient à exécuter un script Windows PowerShell à partir d’une invite de commandes normale. Vous devez appeler l’exécutable PowerShell. exe et utiliser le commutateur **– Command** pour fournir les commandes que vous souhaitez que Windows PowerShell exécute. (Dans Windows PowerShell v2, vous pouvez également utiliser le commutateur **– file** ). La commande doit être au format suivant :

[!code-console[Main](running-windows-powershell-scripts-from-msbuild-project-files/samples/sample3.cmd)]

Exemple :

[!code-console[Main](running-windows-powershell-scripts-from-msbuild-project-files/samples/sample4.cmd)]

Si le chemin d’accès à votre script comprend des espaces, vous devez placer le chemin d’accès de fichier entre guillemets simples, précédé d’un signe &. Vous ne pouvez pas utiliser des guillemets doubles, car vous les avez déjà utilisés pour encadrer la commande :

[!code-console[Main](running-windows-powershell-scripts-from-msbuild-project-files/samples/sample5.cmd)]

Quelques considérations supplémentaires sont à prendre en compte lorsque vous appelez cette commande à partir de MSBuild. Tout d’abord, vous devez inclure l’indicateur **-non interactif** pour vous assurer que le script s’exécute en mode silencieux. Ensuite, vous devez inclure l’indicateur **– ExecutionPolicy** avec une valeur d’argument appropriée. Cette option spécifie la stratégie d’exécution que Windows PowerShell appliquera à votre script et vous permet de remplacer la stratégie d’exécution par défaut, qui peut empêcher l’exécution de votre script. Vous pouvez choisir parmi les valeurs d’arguments suivantes :

- La valeur **Unrestricted** permet à Windows PowerShell d’exécuter votre script, que le script soit signé ou non.
- La valeur **RemoteSigned** permettra à Windows PowerShell d’exécuter des scripts non signés qui ont été créés sur l’ordinateur local. Toutefois, les scripts qui ont été créés ailleurs doivent être signés. (Dans la pratique, il est très peu probable que vous ayez créé un script Windows PowerShell localement sur un serveur de builds).
- La valeur **AllSigned** permettra à Windows PowerShell d’exécuter uniquement des scripts signés.

La stratégie d’exécution par défaut est **Restricted**, ce qui empêche Windows PowerShell d’exécuter des fichiers de script.

Enfin, vous devez placer dans une séquence d’échappement tous les caractères XML réservés qui se produisent dans votre commande Windows PowerShell :

- Remplacer les guillemets simples par **&amp;apos ;**
- Remplacer les guillemets doubles par **&amp;quot ;**
- Remplacez perluète par **&amp;amp ;**

- Lorsque vous apportez ces modifications, votre commande ressemble à ceci :

[!code-console[Main](running-windows-powershell-scripts-from-msbuild-project-files/samples/sample6.cmd)]

Dans votre fichier projet MSBuild personnalisé, vous pouvez créer une cible et utiliser la tâche **Exec** pour exécuter cette commande :

[!code-xml[Main](running-windows-powershell-scripts-from-msbuild-project-files/samples/sample7.xml)]

Dans cet exemple, Notez que :

- Toutes les variables, comme les valeurs de paramètres et l’emplacement de l’exécutable Windows PowerShell, sont déclarées en tant que propriétés MSBuild.
- Des conditions sont incluses pour permettre aux utilisateurs de remplacer ces valeurs à partir de la ligne de commande.
- La propriété **MSDeployComputerName** est déclarée ailleurs dans le fichier projet.

Quand vous exécutez cette cible dans le cadre de votre processus de génération, Windows PowerShell exécute votre commande et écrit une entrée de journal dans le fichier que vous avez spécifié.

## <a name="executing-a-windows-powershell-script-on-a-remote-computer"></a>Exécution d’un script Windows PowerShell sur un ordinateur distant

Windows PowerShell peut exécuter des scripts sur des ordinateurs distants via [Windows Remote Management](https://msdn.microsoft.com/library/windows/desktop/aa384426.aspx) (WinRM). Pour ce faire, vous devez utiliser l’applet de [commande Invoke-Command](https://technet.microsoft.com/library/dd347578.aspx) . Cela vous permet d’exécuter votre script sur un ou plusieurs ordinateurs distants sans copier le script sur les ordinateurs distants. Les résultats sont retournés à l’ordinateur local à partir duquel vous avez exécuté le script.

> [!NOTE]
> Avant d’utiliser l’applet de **commande Invoke-Command** pour exécuter des scripts Windows PowerShell sur un ordinateur distant, vous devez configurer un écouteur WinRM pour qu’il accepte les messages distants. Pour ce faire, vous pouvez exécuter la commande **WinRM quickconfig** sur l’ordinateur distant. Pour plus d’informations, consultez [installation et configuration de Windows Remote Management](https://msdn.microsoft.com/library/windows/desktop/aa384372(v=vs.85).aspx).

Dans une fenêtre Windows PowerShell, vous devez utiliser cette syntaxe pour exécuter le script **LogDeploy. ps1** sur un ordinateur distant :

[!code-powershell[Main](running-windows-powershell-scripts-from-msbuild-project-files/samples/sample8.ps1)]

> [!NOTE]
> Il existe plusieurs autres façons d’utiliser **Invoke-Command** pour exécuter un fichier de script, mais cette approche est la plus simple lorsque vous devez fournir des valeurs de paramètre et gérer des chemins d’accès avec des espaces.

Lorsque vous exécutez cette commande à partir d’une invite de commandes, vous devez appeler l’exécutable Windows PowerShell et utiliser le paramètre **– Command** pour fournir les instructions suivantes :

[!code-console[Main](running-windows-powershell-scripts-from-msbuild-project-files/samples/sample9.cmd)]

Comme précédemment, vous devez fournir des commutateurs supplémentaires et échapper tous les caractères XML réservés quand vous exécutez la commande à partir de MSBuild :

[!code-console[Main](running-windows-powershell-scripts-from-msbuild-project-files/samples/sample10.cmd)]

Enfin, comme précédemment, vous pouvez utiliser la tâche **Exec** dans une cible MSBuild personnalisée pour exécuter votre commande :

[!code-xml[Main](running-windows-powershell-scripts-from-msbuild-project-files/samples/sample11.xml)]

Quand vous exécutez cette cible dans le cadre de votre processus de génération, Windows PowerShell exécute votre script sur l’ordinateur que vous avez spécifié dans l’argument **– ComputerName** .

## <a name="conclusion"></a>Conclusion

Cette rubrique a décrit comment exécuter un script Windows PowerShell à partir d’un fichier projet MSBuild. Vous pouvez utiliser cette approche pour exécuter un script Windows PowerShell, localement ou sur un ordinateur distant, dans le cadre d’un processus de génération et de déploiement automatisé ou en une seule étape.

## <a name="further-reading"></a>informations supplémentaires

Pour obtenir des conseils sur la signature des scripts Windows PowerShell et sur la gestion des stratégies d’exécution, consultez [exécution de scripts Windows PowerShell](https://technet.microsoft.com/library/ee176949.aspx). Pour obtenir des conseils sur l’exécution de commandes Windows PowerShell à partir d’un ordinateur distant, consultez [exécution de commandes à distance](https://technet.microsoft.com/library/dd819505.aspx).

Pour plus d’informations sur l’utilisation de fichiers de projet MSBuild personnalisés pour contrôler le processus de déploiement, consultez [comprendre le fichier projet](../web-deployment-in-the-enterprise/understanding-the-project-file.md) et [comprendre le processus de génération](../web-deployment-in-the-enterprise/understanding-the-build-process.md).

> [!div class="step-by-step"]
> [Précédent](taking-web-applications-offline-with-web-deploy.md)
> [Suivant](troubleshooting-the-packaging-process.md)
