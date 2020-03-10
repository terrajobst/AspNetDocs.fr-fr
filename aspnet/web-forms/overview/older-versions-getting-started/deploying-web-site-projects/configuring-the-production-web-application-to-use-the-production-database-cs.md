---
uid: web-forms/overview/older-versions-getting-started/deploying-web-site-projects/configuring-the-production-web-application-to-use-the-production-database-cs
title: Configuration de l’application Web de production pour utiliser la base deC#données de production () | Microsoft Docs
author: rick-anderson
description: Comme indiqué dans les didacticiels précédents, il n’est pas rare que les informations de configuration diffèrent entre les environnements de développement et de production. Il s’agit de es...
ms.author: riande
ms.date: 04/23/2009
ms.assetid: 0177dabd-d888-449f-91b2-24190cf5e842
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deploying-web-site-projects/configuring-the-production-web-application-to-use-the-production-database-cs
msc.type: authoredcontent
ms.openlocfilehash: 89941bb6db52316a259ad5f5577721e36f19bd84
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78631529"
---
# <a name="configuring-the-production-web-application-to-use-the-production-database-c"></a>Configuration de l’application web de production pour l’utilisation de la base de données de production (C#)

par [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Télécharger le code](https://download.microsoft.com/download/E/6/F/E6FE3A1F-EE3A-4119-989A-33D1A9F6F6DD/ASPNET_Hosting_Tutorial_08_CS.zip) ou [Télécharger le PDF](https://download.microsoft.com/download/C/3/9/C391A649-B357-4A7B-BAA4-48C96871FEA6/aspnet_tutorial08_DBConfig_cs.pdf)

> Comme indiqué dans les didacticiels précédents, il n’est pas rare que les informations de configuration diffèrent entre les environnements de développement et de production. Cela est particulièrement vrai pour les applications Web pilotées par les données, car les chaînes de connexion de base de données diffèrent entre les environnements de développement et de production. Ce didacticiel explore les différentes façons de configurer l’environnement de production pour inclure la chaîne de connexion appropriée plus en détail.

## <a name="introduction"></a>Introduction

Les applications Web pilotées par les données utilisent généralement une base de données différente en cas de développement que dans un environnement de production. Pour les applications hébergées par un fournisseur d’hébergement Web et développées localement, la base de données de développement réside généralement sur l’ordinateur du développeur, tandis que la base de données de production est hébergée sur un serveur de base de données au niveau de l’installation de l’entreprise d’hébergement Web. Le déploiement d’une application Web pilotée par les données implique la copie de la base de données de développement sur le serveur de base de données de production. Dans le didacticiel précédent, nous avons examiné les façons d’accomplir cette étape.

L’application Web utilise les informations contenues dans une *chaîne de connexion* pour établir une connexion avec la base de données. La chaîne de connexion, qui est généralement stockée dans `Web.config`, spécifie le nom du serveur de base de données, le nom de la base de données, le contexte de sécurité et d’autres informations. Étant donné que la base de données utilisée par l’application Web varie selon que l’application Web s’exécute dans les environnements de développement ou de production, les chaînes de connexion doivent être différentes entre les deux environnements.

Il n’est pas rare que les informations de configuration diffèrent entre les environnements de développement et de production. Les *différences de configuration courantes entre* les didacticiels de développement et de production ont présenté des techniques permettant de gérer des informations de configuration distinctes entre ces deux environnements, ainsi qu’une brève discussion sur les chaînes de connexion de base de données. Ce didacticiel explore les différentes façons de configurer l’environnement de production pour inclure la chaîne de connexion appropriée plus en détail.

## <a name="examining-the-connection-string-information"></a>Examen des informations de la chaîne de connexion

La chaîne de connexion utilisée par l’application Web de révisions de livres est stockée dans le fichier de configuration de l’application, `Web.config`. `Web.config` comprend une section spéciale pour le stockage des chaînes de connexion, nommées avec prénoms [&lt;connectionStrings&gt;](https://msdn.microsoft.com/library/bf7sd233.aspx). Le fichier `Web.config` pour le site Web de révisions de livres a une chaîne de connexion définie dans cette section nommée `ReviewsConnectionString`:

[!code-xml[Main](configuring-the-production-web-application-to-use-the-production-database-cs/samples/sample1.xml)]

Chaîne de connexion-source de données = .\SQLEXPRESS ; AttachDbFilename = | DataDirectory | \Reviews.mdf ; Integrated Security = true ; User Instance = true-est composé d’un certain nombre d’options et de valeurs, avec des paires option/valeur délimitées par un point-virgule et chaque option et valeur délimitée par un signe égal. Les quatre options utilisées dans cette chaîne de connexion sont les suivantes :

- `Data Source` : spécifie l’emplacement du serveur de base de données et le nom de l’instance du serveur de base de données (le cas échéant). La valeur, `.\SQLEXPRESS`, est un exemple où il existe un serveur de base de données et un nom d’instance. La période spécifie que le serveur de base de données se trouve sur le même ordinateur que l’application. le nom de l’instance est `SQLEXPRESS`.
- `AttachDbFilename` : spécifie l’emplacement du fichier de base de données. La valeur contient l’espace réservé `|DataDirectory|`, qui est résolu en chemin d’accès complet du dossier de l’application s `App_Data` au moment de l’exécution.
- `Integrated Security` : valeur booléenne qui indique s’il faut utiliser un nom d’utilisateur/mot de passe spécifié lors de la connexion à la base de données (false) ou les informations d’identification du compte Windows actuel (true).
- `User Instance` : option de configuration spécifique aux éditions SQL Server Express qui indique s’il faut autoriser les utilisateurs non-administrateurs sur l’ordinateur local à se connecter et à se connecter à une base de données SQL Server Express Edition. Pour plus d’informations sur ce paramètre, consultez [SQL Server Express des instances d’utilisateur](https://msdn.microsoft.com/library/ms254504.aspx) .

Les options de chaîne de connexion autorisées dépendent de la base de données à laquelle vous vous connectez et du fournisseur de base de données ADO.NET utilisé. Par exemple, la chaîne de connexion pour la connexion à une base de données Microsoft SQL Server diffère de celle utilisée pour se connecter à une base de données Oracle. De même, la connexion à une base de données Microsoft SQL Server à l’aide du fournisseur SqlClient utilise une chaîne de connexion différente de celle utilisée lors de l’utilisation du fournisseur OLE-DB.

Vous pouvez créer manuellement la chaîne de connexion de base de données à l’aide d’un site tel que [connectionStrings.com](http://www.connectionstrings.com/) comme ressource pour les options disponibles. Toutefois, une approche plus simple consiste à ajouter la base de données au Explorateur de serveurs dans Visual Studio, puis à récupérer la chaîne de connexion à partir du Fenêtre Propriétés. Essayons d’utiliser cette dernière technique pour construire la chaîne de connexion au serveur de base de données de production.

Ouvrez Visual Studio, puis accédez à la fenêtre de Explorateur de serveurs (dans Visual Web Developer, cette fenêtre est appelée explorateur de base de données). Cliquez avec le bouton droit sur l’option connexions de données et choisissez l’option Ajouter une connexion dans le menu contextuel. Cela fait apparaître l’Assistant illustré à la figure 1. Choisissez la source de données appropriée, puis cliquez sur continuer.

[![choisissez d’ajouter une nouvelle base de données au Explorateur de serveurs](configuring-the-production-web-application-to-use-the-production-database-cs/_static/image2.jpg)](configuring-the-production-web-application-to-use-the-production-database-cs/_static/image1.jpg) 

**Figure 1**: choisir d’ajouter une nouvelle base de données au Explorateur de serveurs ([cliquez pour afficher l’image en taille réelle](configuring-the-production-web-application-to-use-the-production-database-cs/_static/image3.jpg))

Ensuite, spécifiez les différentes informations de connexion à la base de données (voir la figure 2). Lorsque vous vous êtes inscrit auprès de votre société d’hébergement Web, vous devez fournir des informations sur la façon de se connecter à la base de données (le nom du serveur de base de données, le nom de la base de données, le nom d’utilisateur et le mot de passe à utiliser pour se connecter à la base de données, etc.). Après avoir entré ces informations, cliquez sur OK pour terminer cet Assistant et ajouter la base de données au Explorateur de serveurs.

[![spécifier les informations de connexion à la base de données](configuring-the-production-web-application-to-use-the-production-database-cs/_static/image5.jpg)](configuring-the-production-web-application-to-use-the-production-database-cs/_static/image4.jpg) 

**Figure 2**: spécifier les informations de connexion à la base de données ([cliquez pour afficher l’image en taille réelle](configuring-the-production-web-application-to-use-the-production-database-cs/_static/image6.jpg))

La base de données de l’environnement de production doit maintenant être listée dans le Explorateur de serveurs. Sélectionnez la base de données à partir du Explorateur de serveurs et accédez à la Fenêtre Propriétés. Vous y trouverez une propriété nommée chaîne de connexion avec la chaîne de connexion de la base de données. En supposant que vous utilisez une base de données Microsoft SQL Server en production et le fournisseur SqlClient, votre chaîne de connexion doit ressembler à ce qui suit :

<strong>Source de données =<em>ServerName</em>; Catalogue initial =<em>DatabaseName</em>; Persist Security Info = true ; ID utilisateur =<em>nom d'</em>utilisateur ; Password =*mot de passe</strong>*

Où *ServerName*, *DatabaseName*, *username*et *Password* se composent des valeurs pour le nom du serveur de base de données, le nom de la base de données et le nom d’utilisateur et le mot de passe qui vous sont fournis par votre société de l’hôte Web.

## <a name="deploying-the-book-reviews-web-application"></a>Déploiement de l’application Web de révisions de livres

Le didacticiel précédent a parcouru la copie de la base de données de développement vers l’environnement de production, mais n’explore pas le déploiement de l’application pilotée par les données. À ce stade, l’environnement de production contient la base de données, mais utilise la version de l’application de révisions de livres avec des révisions statiques. Nous devons déployer la nouvelle application pilotée par les données sur le serveur de production, ainsi que les informations de configuration mises à jour.

Prenez un moment pour déployer l’application pilotée par les données de l’environnement de développement vers la production. Ce processus a été abordé en détail dans les didacticiels précédents. Si vous avez besoin d’un actualisateur, consultez les didacticiels *déploiement de votre site Web à l’aide d’un client FTP* ou *déploiement de votre site Web à l’aide de Visual Studio* . Vous devez vous assurer que la chaîne de connexion de la base de données de production est celle utilisée dans l’environnement de production, ce qui signifie qu’un autre `Web.config` fichier doit être déployé. Plus précisément, cet élément `Web.config` file s `<connectionStrings>` modifié doit contenir la chaîne de connexion à la base de données de production et doit ressembler à ce qui suit :

[!code-xml[Main](configuring-the-production-web-application-to-use-the-production-database-cs/samples/sample2.xml)]

Notez que la chaîne de connexion dans l’élément `<connectionStrings>` porte le même nom (`ReviewsConnectionString`), mais contient désormais la chaîne de connexion à la base de données de production au lieu de la chaîne de connexion de la base de données de développement.

À moins que vous n’ayez un flux de travail de déploiement plus formalisé, vous devez modifier manuellement le fichier `Web.config` pour utiliser la chaîne de connexion de la base de données de production avant de procéder au déploiement (en vous reconnectant pour revenir à l’utilisation de la chaîne de connexion de la base de données de développement) ou conserver un fichier `Web.config` distinct avec les informations de configuration de l’environnement de production

> [!NOTE]
> Si vous déployez accidentellement un fichier `Web.config` qui contient la chaîne de connexion à la base de données de développement, une erreur se produit lorsque l’application sur la production tente de se connecter à la base de données. Cette erreur se manifeste comme un `SqlException` avec un message signalant que le serveur est introuvable ou inaccessible.

Une fois que le site a été déployé en production, visitez le site de production via votre navigateur. Vous devez voir et profiter de la même expérience utilisateur que lors de l’exécution locale de l’application pilotée par les données. Bien entendu, lorsque vous accédez au site Web sur production, le site est alimenté par le serveur de base de données de production, tandis que la visite du site Web dans l’environnement de développement utilise la base de données en cours de développement. La figure 3 illustre la page d' *apprentissage vous-même ASP.NET 3,5 dans 24 heures* à partir du site Web dans l’environnement de production (Notez l’URL dans la barre d’adresse du navigateur).

[![l’application pilotée par les données est désormais disponible en production.](configuring-the-production-web-application-to-use-the-production-database-cs/_static/image8.jpg)](configuring-the-production-web-application-to-use-the-production-database-cs/_static/image7.jpg) 

**Figure 3**: l’application pilotée par les données est désormais disponible en production. ([Cliquez pour afficher l’image en taille réelle](configuring-the-production-web-application-to-use-the-production-database-cs/_static/image9.jpg))

### <a name="storing-connection-strings-in-a-separate-configuration-file"></a>Stockage des chaînes de connexion dans un fichier de configuration distinct

Une technique courante pour gérer des informations de configuration distinctes dans les environnements de développement et de production consiste à avoir deux versions du `Web.config`: une pour l’environnement de développement et une pour la production. Au moment du déploiement, la version de `Web.config` appropriée peut être copiée dans l’environnement de production. Dans l’idéal, ce processus serait automatisé dans le cadre du flux de travail de déploiement.

Au lieu de conserver deux fichiers de `Web.config` distincts, vous pouvez éventuellement fournir des différences plus granulaires. Les éléments qui composent le fichier `Web.config` peuvent être définis dans des fichiers de configuration externes qui sont ensuite référencés dans le fichier `Web.config`. En bref, vous pouvez avoir un fichier de `Web.config` pour les deux environnements qui fait référence à un fichier databaseConnectionStrings. config, qui contient les chaînes de connexion utilisées par l’application et qui seraient uniques pour chaque environnement. Je trouve qu’en séparant les informations de configuration différentes dans des fichiers distincts, vous obtenez un fichier de `Web.config` et un fichier plus simple, ce qui explique plus clairement les différences de configuration entre les environnements de développement et de production.

Pour utiliser cette technique, commencez par créer un nouveau dossier dans l’application Web nommée `ConfigSections`. Ensuite, ajoutez deux fichiers à ce nouveau dossier nommé databaseConnectionStrings. dev. config et databaseConnectionStrings. production. config. Copiez ensuite l’élément `<connectionStrings>` à partir de `Web.config` dans les fichiers databaseConnectionStrings. dev. config et databaseConnectionStrings. production. config, puis modifiez la chaîne de connexion dans le fichier databaseConnectionStrings. production. config afin qu’elle spécifie la chaîne de connexion de la base de données de production. Par exemple, le fichier databaseConnectionStrings. dev. config doit contenir uniquement l’élément `<connectionStrings>` avec une chaîne de connexion qui fait référence à la base de données de développement :

[!code-xml[Main](configuring-the-production-web-application-to-use-the-production-database-cs/samples/sample3.xml)]

De même, le fichier databaseConnectionStrings. production. config ne doit contenir qu’un seul élément `<connectionStrings>`, mais avec la chaîne de connexion de la base de données de production.

Effectuez une copie du fichier databaseConnectionStrings. dev. config et nommez-le databaseConnectionStrings. config.

> [!NOTE]
> Vous pouvez nommer un fichier de configuration autre que databaseConnectionStrings. config, si vous le souhaitez, par exemple `connectionStrings.config` ou `dbInfo.config`. Toutefois, veillez à nommer le fichier avec une extension de `.config`, car les fichiers `.config` sont, par défaut, non pris en charge par le moteur ASP.NET. Si vous nommez le fichier autre chose, comme `connectionStrings.txt`, un utilisateur peut pointer son navigateur vers [www.yoursite.com/ConfigSettings/ConnectionStrings.txt](http://www.yoursite.com/ConfigSettings/connectionStrings.txt) et afficher le contenu du fichier !

À ce stade, le dossier `ConfigSections` doit contenir trois fichiers (voir figure 4). Les fichiers databaseConnectionStrings. dev. config et databaseConnectionStrings. production. config contiennent respectivement les chaînes de connexion pour les environnements de développement et de production. Le fichier databaseConnectionStrings. config contient les informations de chaîne de connexion qui seront utilisées par l’application Web au moment de l’exécution. Par conséquent, le fichier databaseConnectionStrings. config doit être identique au fichier databaseConnectionStrings. dev. config dans l’environnement de développement, tandis que sur la production, le fichier databaseConnectionStrings. config doit être identique à databaseConnectionStrings. production. config.

[![ConfigSections](configuring-the-production-web-application-to-use-the-production-database-cs/_static/image11.jpg)](configuring-the-production-web-application-to-use-the-production-database-cs/_static/image10.jpg) 

**Figure 4**: ConfigSections ([cliquez pour afficher l’image en taille réelle](configuring-the-production-web-application-to-use-the-production-database-cs/_static/image12.jpg))

Nous devons maintenant demander à `Web.config` d’utiliser le fichier databaseConnectionStrings. config pour son magasin de chaînes de connexion. Ouvrez `Web.config` et remplacez l’élément `<connectionStrings>` existant par l’élément suivant :

[!code-xml[Main](configuring-the-production-web-application-to-use-the-production-database-cs/samples/sample4.xml)]

L’attribut `configSource` spécifie un chemin d’accès physique relatif au fichier `Web.config`. Si le fichier de `.config` externe se trouve dans le même répertoire que `Web.config`, définissez cet attribut sur le nom du fichier de `.config`. S’il est dans un sous-répertoire, comme c’est le cas avec databaseConnectionStrings. config, spécifiez le sous-dossier à l’aide d’une barre oblique inverse pour délimiter les noms de dossiers et de fichiers, par exemple ConfigSections\databaseConnectionStrings.config.

Avec cette modification, les environnements de développement et de production contiennent le même fichier de `Web.config`. À présent, la seule différence est le fichier databaseConnectionStrings. config. Copiez le fichier databaseConnectionStrings. production. config en production et renommez-le databaseConnectionStrings. config. Si, à l’avenir, il y a des modifications apportées à la chaîne de connexion de la base de données de production, vous devrez les faire passer au fichier databaseConnectionStrings. production. config, puis charger ce fichier en production, en le renommant databaseConnectionStrings. config.

> [!NOTE]
> Vous pouvez spécifier les informations de tout élément `Web.config` dans un fichier distinct et utiliser l’attribut `configSource` pour référencer ce fichier à partir de `Web.config`.

## <a name="summary"></a>Récapitulatif

Les applications pilotées par les données utilisent généralement des bases de données différentes dans les environnements de développement et de production. Par conséquent, les chaînes de connexion à la base de données stockées dans la configuration de l’application Web doivent être uniques pour chaque environnement. Dans ce didacticiel, nous avons vu comment déterminer la chaîne de connexion à la base de données de production et comment gérer les informations de chaîne de connexion uniques dans les deux environnements.

Bonne programmation !

#### <a name="further-reading"></a>informations supplémentaires

Pour plus d’informations sur les sujets abordés dans ce didacticiel, reportez-vous aux ressources suivantes :

- [Chaînes de connexion et fichiers de configuration](https://msdn.microsoft.com/library/ms254494.aspx)
- [Informations sur les chaînes de configuration de base de données @ ConnectionStrings.com](http://www.connectionstrings.com/)
- [Déplacer les paramètres hors du fichier Web. config](http://www.asp101.com/tips/index.asp?id=154)
- [Documentation technique pour l’élément &lt;connectionStrings&gt;](https://msdn.microsoft.com/library/bf7sd233.aspx)

> [!div class="step-by-step"]
> [Précédent](deploying-a-database-cs.md)
> [Suivant](configuring-a-website-that-uses-application-services-cs.md)
