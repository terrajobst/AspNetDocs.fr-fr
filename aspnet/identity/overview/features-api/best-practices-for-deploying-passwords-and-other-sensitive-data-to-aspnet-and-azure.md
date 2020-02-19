---
uid: identity/overview/features-api/best-practices-for-deploying-passwords-and-other-sensitive-data-to-aspnet-and-azure
title: Déploiement de mots de passe et d’autres données sensibles dans ASP.NET et Azure App Service-ASP.NET 4. x
author: Rick-Anderson
description: Ce didacticiel montre comment votre code peut stocker et accéder en toute sécurité aux informations sécurisées. Le point le plus important est que vous ne devez jamais stocker les mots de passe ou autres...
ms.author: riande
ms.date: 05/21/2015
ms.assetid: 97902c66-cb61-4d11-be52-73f962f2db0a
ms.custom: seoapril2019
msc.legacyurl: /identity/overview/features-api/best-practices-for-deploying-passwords-and-other-sensitive-data-to-aspnet-and-azure
msc.type: authoredcontent
ms.openlocfilehash: 8356a90611f791779cc4ff4730038d82cd76242f
ms.sourcegitcommit: 7709c0a091b8d55b7b33bad8849f7b66b23c3d72
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/19/2020
ms.locfileid: "77457048"
---
# <a name="best-practices-for-deploying-passwords-and-other-sensitive-data-to-aspnet-and-azure-app-service"></a>Bonnes pratiques pour le déploiement des mots de passe et d’autres données sensibles sur ASP.NET et Azure App Service

par [Rick Anderson](https://twitter.com/RickAndMSFT)

> Ce didacticiel montre comment votre code peut stocker et accéder en toute sécurité aux informations sécurisées. Le point le plus important est que vous ne devez jamais stocker les mots de passe ou d’autres données sensibles dans le code source, et vous ne devez pas utiliser les clés secrètes de production en mode de développement et de test.
> 
> L’exemple de code est une application console tâche Web simple et une application ASP.NET MVC qui doit accéder à un mot de passe de chaîne de connexion à la base de données, Twilio, Google et SendGrid clés sécurisées.
> 
> Les paramètres locaux et PHP sont également mentionnés.

- [Utilisation de mots de passe dans l’environnement de développement](#pwd)
- [Utilisation des chaînes de connexion dans l’environnement de développement](#con)
- [Applications console des tâches Web](#wj)
- [Déploiement de secrets sur Azure](#da)
- [Remarques sur les versions locale et PHP](#not)
- [Ressources supplémentaires](#addRes)

<a id="pwd"></a>
## <a name="working-with-passwords-in-the-development-environment"></a>Utilisation de mots de passe dans l’environnement de développement

Les didacticiels affichent fréquemment des données sensibles dans le code source, avec un inconvénient que vous ne devez jamais stocker des données sensibles dans le code source. Par exemple, le didacticiel mon [application ASP.NET MVC 5 avec SMS et E-mail 2FA](../../../mvc/overview/security/aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication.md) présente les éléments suivants dans le fichier *Web. config* :

[!code-xml[Main](best-practices-for-deploying-passwords-and-other-sensitive-data-to-aspnet-and-azure/samples/sample1.xml)]

Le fichier *Web. config* étant du code source, ces secrets ne doivent jamais être stockés dans ce fichier. Heureusement, l’élément `<appSettings>` a un attribut `file` qui vous permet de spécifier un fichier externe qui contient des paramètres de configuration d’application sensibles. Vous pouvez déplacer tous vos secrets vers un fichier externe tant que le fichier externe n’est pas archivé dans votre arborescence source. Par exemple, dans le balisage suivant, le fichier *AppSettingsSecrets. config* contient tous les secrets de l’application :

[!code-xml[Main](best-practices-for-deploying-passwords-and-other-sensitive-data-to-aspnet-and-azure/samples/sample2.xml)]

Le balisage dans le fichier externe (*AppSettingsSecrets. config* dans cet exemple) est le même balisage que celui du fichier *Web. config* :

[!code-xml[Main](best-practices-for-deploying-passwords-and-other-sensitive-data-to-aspnet-and-azure/samples/sample3.xml)]

Le runtime ASP.NET fusionne le contenu du fichier externe avec le balisage dans &lt;élément appSettings&gt;. Le runtime ignore l’attribut de fichier si le fichier spécifié est introuvable.

> [!WARNING]
> Sécurité : n’ajoutez pas votre fichier *secrets. config* à votre projet ou archivez-le dans le contrôle de code source. Par défaut, Visual Studio définit la `Build Action` sur `Content`, ce qui signifie que le fichier est déployé. Pour plus d’informations [, voir pourquoi tous les fichiers de mon dossier de projet ne sont-ils pas déployés ?](https://msdn.microsoft.com/library/ee942158(v=vs.110).aspx#can_i_exclude_specific_files_or_folders_from_deployment) Bien que vous puissiez utiliser n’importe quelle extension pour le fichier *secrets. config* , il est préférable de conserver it *. config*, car les fichiers de configuration ne sont pas pris en charge par IIS. Notez également que le fichier *AppSettingsSecrets. config* est constitué de deux niveaux de répertoire allant du fichier *Web. config* . il est donc complètement hors du répertoire de la solution. En déplaçant le fichier hors du répertoire de la solution, &quot;git Add \*&quot; ne pas l’ajouter à votre référentiel.

<a id="con"></a>
## <a name="working-with-connection-strings-in-the-development-environment"></a>Utilisation des chaînes de connexion dans l’environnement de développement

Visual Studio crée de nouveaux projets ASP.NET qui [utilisent la](https://blogs.msdn.com/b/sqlexpress/archive/2011/07/12/introducing-localdb-a-better-sql-express.aspx)base de données locale. La base de données locale a été créée spécifiquement pour l’environnement de développement. Cela ne nécessite pas de mot de passe. par conséquent, vous n’avez rien à faire pour empêcher la vérification des secrets dans votre code source. Certaines équipes de développement utilisent les versions complètes de SQL Server (ou d’autres SGBD) qui requièrent un mot de passe.

Vous pouvez utiliser l’attribut `configSource` pour remplacer l’ensemble du balisage `<connectionStrings>`. Contrairement à l’attribut `<appSettings>` `file` qui fusionne le balisage, l’attribut `configSource` remplace le balisage. Le balisage suivant montre l’attribut `configSource` dans le fichier *Web. config* :

[!code-xml[Main](best-practices-for-deploying-passwords-and-other-sensitive-data-to-aspnet-and-azure/samples/sample4.xml?highlight=1)]

> [!NOTE]
> Si vous utilisez l’attribut `configSource` comme indiqué ci-dessus pour déplacer vos chaînes de connexion vers un fichier externe et que Visual Studio crée un nouveau site Web, il ne pourra pas détecter que vous utilisez une base de données, et vous n’aurez pas la possibilité de configurer la base de données lorsque vous publiez sur Azure à partir de Visual Studio. Si vous utilisez l’attribut `configSource`, vous pouvez utiliser PowerShell pour créer et déployer votre site Web et votre base de données, ou vous pouvez créer le site Web et la base de données dans le portail avant de publier. Le script [New-AzureWebsitewithDB. ps1](https://gallery.technet.microsoft.com/scriptcenter/Ultimate-Create-Web-SQL-DB-9e0fdfd3) créera un nouveau site Web et une nouvelle base de données.

> [!WARNING]
> Sécurité : contrairement au fichier *AppSettingsSecrets. config* , le fichier de chaînes de connexion externe doit se trouver dans le même répertoire que le fichier *Web. config* racine. vous devez donc prendre des précautions pour éviter de l’archiver dans votre référentiel source.

> [!NOTE]
> **Avertissement de sécurité sur le fichier de secrets :** Une meilleure pratique consiste à ne pas utiliser les secrets de production dans le test et le développement. L’utilisation de mots de passe de production dans les tests ou le développement entraîne des fuites.

<a id="wj"></a>
## <a name="webjobs-console-apps"></a>Applications console des tâches Web

Le fichier *app. config* utilisé par une application console ne prend pas en charge les chemins d’accès relatifs, mais il prend en charge les chemins absolus. Vous pouvez utiliser un chemin d’accès absolu pour déplacer vos secrets en dehors de votre répertoire de projet. Le balisage suivant montre les secrets dans le fichier *C:\secrets\AppSettingsSecrets.config* et les données non sensibles dans le fichier *app. config* .

[!code-xml[Main](best-practices-for-deploying-passwords-and-other-sensitive-data-to-aspnet-and-azure/samples/sample5.xml?highlight=2)]

<a id="da"></a>
## <a name="deploying-secrets-to-azure"></a>Déploiement de secrets sur Azure

Lorsque vous déployez votre application Web sur Azure, le fichier *AppSettingsSecrets. config* n’est pas déployé (c’est ce que vous voulez). Vous pouvez accéder à la [portail de gestion Azure](https://azure.microsoft.com/services/management-portal/) et les définir manuellement pour effectuer cette opération :

1. Accédez à [https://portal.azure.com](https://portal.azure.com)et connectez-vous avec vos informations d’identification Azure.
2. Cliquez sur **parcourir &gt; Web Apps**, puis cliquez sur le nom de votre application Web.
3. Cliquez sur **tous les paramètres &gt; paramètres**de l’application.

Les **paramètres** de l’application et les valeurs de **chaîne de connexion** remplacent les mêmes paramètres dans le fichier *Web. config* . Dans notre exemple, nous n’avons pas déployé ces paramètres dans Azure, mais si ces clés figuraient dans le fichier *Web. config* , les paramètres affichés sur le portail sont prioritaires.

Une bonne pratique consiste à suivre un [flux de travail DevOps](../../../aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/automate-everything.md) et à utiliser [Azure PowerShell](https://azure.microsoft.com/documentation/articles/install-configure-powershell/) (ou un autre Framework comme [chef](http://www.opscode.com/chef/) ou [marionnette](http://puppetlabs.com/puppet/what-is-puppet)) pour automatiser la définition de ces valeurs dans Azure. Le script PowerShell suivant utilise [Export-CliXml](http://www.powershellcookbook.com/recipe/PukO/securely-store-credentials-on-disk) pour exporter les secrets chiffrés sur le disque :

[!code-powershell[Main](best-practices-for-deploying-passwords-and-other-sensitive-data-to-aspnet-and-azure/samples/sample6.ps1)]

Dans le script ci-dessus, « Name » est le nom de la clé secrète, par exemple «&quot;FB\_AppSecret&quot; ou «TwitterSecret ». Vous pouvez afficher le fichier. Credentials créé par le script dans votre navigateur. L’extrait de code ci-dessous teste chaque fichier d’informations d’identification et définit les secrets de l’application Web nommée :

[!code-powershell[Main](best-practices-for-deploying-passwords-and-other-sensitive-data-to-aspnet-and-azure/samples/sample7.ps1)]

> [!WARNING]
> Sécurité : n’incluez pas de mots de passe ou d’autres secrets dans le script PowerShell, ce qui a pour effet d’utiliser un script PowerShell pour déployer des données sensibles. L’applet de commande [d’obtention d’informations d’identification](https://technet.microsoft.com/library/hh849815.aspx) fournit un mécanisme sécurisé pour obtenir un mot de passe. L’utilisation d’une invite d’interface utilisateur peut empêcher la fuite d’un mot de passe.

### <a name="deploying-db-connection-strings"></a>Déploiement de chaînes de connexion à la base de BD

Les chaînes de connexion à la base de base sont gérées de la même façon que les paramètres d’application. Si vous déployez votre application Web à partir de Visual Studio, la chaîne de connexion est configurée pour vous. Vous pouvez le vérifier dans le portail. La méthode recommandée pour définir la chaîne de connexion est avec PowerShell. Pour obtenir un exemple de script PowerShell, crée un site Web et une base de données et définit la chaîne de connexion dans le site Web, téléchargez [New-AzureWebsitewithDB. ps1](https://gallery.technet.microsoft.com/scriptcenter/Ultimate-Create-Web-SQL-DB-9e0fdfd3) à partir de la [bibliothèque de scripts Azure](https://gallery.technet.microsoft.com/scriptcenter/site/search?f%5B0%5D.Type=RootCategory&amp;f%5B0%5D.Value=WindowsAzure).

<a id="not"></a>
## <a name="notes-for-php"></a>Notes pour PHP

Étant donné que les paires clé-valeur pour les **paramètres d’application** et les **chaînes de connexion** sont stockées dans des variables d’environnement sur Azure App service, les développeurs qui utilisent des infrastructures d’applications Web (telles que PHP) peuvent facilement récupérer ces valeurs. Consultez le billet de blog [sites Web Windows Azure de Stefan Schackow : comment les chaînes d’application et les chaînes de connexion fonctionnent,](https://azure.microsoft.com/blog/2013/07/17/windows-azure-web-sites-how-application-strings-and-connection-strings-work/) qui montre un extrait de code php permettant de lire les paramètres d’application et les chaînes de connexion.

## <a name="notes-for-on-premises-servers"></a>Notes pour les serveurs locaux

Si vous effectuez un déploiement sur des serveurs Web locaux, vous pouvez sécuriser les secrets en [chiffrant les sections de configuration des fichiers de configuration](https://msdn.microsoft.com/library/ff647398.aspx). Vous pouvez également utiliser la même approche que celle recommandée pour les sites Web Azure : conserver les paramètres de développement dans les fichiers de configuration et utiliser des valeurs de variables d’environnement pour les paramètres de production. Dans ce cas, toutefois, vous devez écrire le code de l’application pour une fonctionnalité automatique dans sites Web Azure : récupérer des paramètres à partir de variables d’environnement et utiliser ces valeurs à la place des paramètres du fichier de configuration, ou utiliser les paramètres du fichier de configuration quand les variables d’environnement sont introuvables.

<a id="addRes"></a>
## <a name="additional-resources"></a>Ressources supplémentaires

Pour obtenir un exemple de script PowerShell qui crée une application Web et une base de données, définit la chaîne de connexion + paramètres de l’application, téléchargez [New-AzureWebsitewithDB. ps1](https://gallery.technet.microsoft.com/scriptcenter/Ultimate-Create-Web-SQL-DB-9e0fdfd3) à partir de la [bibliothèque de scripts Azure](https://gallery.technet.microsoft.com/scriptcenter/site/search?f%5B0%5D.Type=RootCategory&amp;f%5B0%5D.Value=WindowsAzure). 

Consultez [sites Web Windows Azure de Stefan Schackow : fonctionnement des chaînes d’application et des chaînes de connexion](https://azure.microsoft.com/blog/2013/07/17/windows-azure-web-sites-how-application-strings-and-connection-strings-work/)

Merci à Barry Dorrans ( [@blowdart](https://twitter.com/blowdart) ) et à Carlos farre pour la révision.
