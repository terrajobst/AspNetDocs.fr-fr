---
uid: web-forms/overview/older-versions-getting-started/deploying-web-site-projects/deploying-a-database-vb
title: Déploiement d’une base de données (VB) | Microsoft Docs
author: rick-anderson
description: Le déploiement d’une application Web ASP.NET implique l’obtention des fichiers et ressources nécessaires de l’environnement de développement vers l’environnement de production. Pour da...
ms.author: riande
ms.date: 04/23/2009
ms.assetid: 96ac3e69-04c7-4917-ad06-5f8968c3fbf1
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deploying-web-site-projects/deploying-a-database-vb
msc.type: authoredcontent
ms.openlocfilehash: 8221025bec06e052016070f74deabb3e6d936045
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/28/2019
ms.locfileid: "74643429"
---
# <a name="deploying-a-database-vb"></a>Déploiement d’une base de données (VB)

par [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Télécharger le code](https://download.microsoft.com/download/E/6/F/E6FE3A1F-EE3A-4119-989A-33D1A9F6F6DD/ASPNET_Hosting_Tutorial_07_VB.zip) ou [Télécharger le PDF](https://download.microsoft.com/download/C/3/9/C391A649-B357-4A7B-BAA4-48C96871FEA6/aspnet_tutorial07_DeployDB_vb.pdf)

> Le déploiement d’une application Web ASP.NET implique l’obtention des fichiers et ressources nécessaires de l’environnement de développement vers l’environnement de production. Pour les applications Web pilotées par les données, cela comprend le schéma et les données de la base de données. Ce didacticiel est le premier d’une série qui explore les étapes nécessaires pour déployer correctement la base de données de l’environnement de développement vers la production.

### <a name="introduction"></a>Introduction

Le déploiement d’une application Web ASP.NET implique l’obtention des fichiers et ressources nécessaires de l’environnement de développement vers l’environnement de production. Au cours des six derniers didacticiels, nous avons examiné le déploiement d’une application Web de révisions de livres simple. Ce site de démonstration a été constitué d’un certain nombre de ressources côté serveur : les pages ASP.NET, les fichiers de configuration, un fichier `Web.sitemap`, etc., ainsi que les ressources côté client, telles que les images et les fichiers CSS. Mais qu’en est-il des applications Web pilotées par les données ? Quelles étapes supplémentaires doivent être prises pour déployer une application Web qui utilise une base de données ?

Dans les prochains didacticiels, nous traiterons les étapes nécessaires au déploiement d’une application Web pilotée par les données. Ce didacticiel commence par examiner comment obtenir un schéma et un contenu de base de données à partir de l’environnement de développement vers l’environnement de production, tandis que le didacticiel suivant examine les modifications de configuration nécessaires. Après cela, nous examinerons les défis liés au déploiement d’une base de données qui utilise le Services d’application (appartenance, rôles, profil, etc.).

## <a name="examining-the-updated-book-reviews-web-application"></a>Examen de l’application Web mise à jour des revues de livres

Afin d’illustrer le déploiement d’une application Web pilotée par les données, j’ai mis à jour l’application Web de révisions de livres à partir d’un site Web simple et statique vers une application pilotée par les données. Comme précédemment, les deux versions de l’application dans ce didacticiel sont téléchargées : une qui utilise le modèle de projet d’application Web et une autre qui utilise le modèle de projet de site Web.

L’application Web mise à jour des revues de livres utilise une base de données [SQL Server 2008 Express Edition](https://www.microsoft.com/express/sql/default.aspx) , qui est stockée dans le dossier du `App_Data` du site (`~/App_Data/Reviews.mdf`). Si SQL Server 2008 est installé sur votre ordinateur, la démonstration doit s’exécuter sans erreur. Si vous disposez d’une version antérieure de SQL Server vous pouvez installer le SQL Server gratuit 2008 Express Edition ou vous pouvez utiliser les scripts de base de données disponibles dans ce didacticiel pour créer la base de données vous-même.

La base de données `Reviews.mdf` contient quatre tables :

- `Genres` : comprend un enregistrement pour chaque genre, comme la technologie, la fiction et l’entreprise.
- `Books` : comprend un enregistrement pour chaque revue, avec des colonnes comme `Title`, `GenreId`, `ReviewDate`et `Review`, entre autres.
- `Authors` : contient des informations sur chaque auteur ayant participé à un livre révisé.
- `BooksAuthors`-une table de jointure plusieurs-à-plusieurs qui spécifie les auteurs qui ont écrit les livres.

La figure 1 illustre un diagramme ER de ces quatre tables.

[![la base de données des applications Web des revues de livres est composée de quatre tables](deploying-a-database-vb/_static/image2.jpg)](deploying-a-database-vb/_static/image1.jpg) 

**Figure 1**: la base de données de l’application Web des revues de livres se compose de quatre tables ([cliquez pour afficher l’image en taille réelle](deploying-a-database-vb/_static/image3.jpg))

La version précédente du site Web de révisions de livres avait une page ASP.NET distincte pour chaque livre. Par exemple, il existait une page nommée `~/Tech/TYASP35.aspx` qui contenait la revue pour *vous ASP.NET 3,5 en 24 heures*. Cette nouvelle version pilotée par les données du site Web contient les analyses stockées dans la base de données et une seule page ASP.NET, Review. aspx ? ID =*BookID*, qui affiche la révision du livre spécifié. De même, il existe une page genre. aspx ? ID =*genreId* qui répertorie les livres consultés dans le genre spécifié.

Les figures 2 et 3 montrent les pages `Genre.aspx` et `Review.aspx` en action. Notez l’URL dans la barre d’adresses de chaque page. Dans la figure 2, il s genre. aspx ? ID = 85d164ba-1123-4c47-82a0-c8ec75de7e0e. Étant donné que 85d164ba-1123-4c47-82a0-c8ec75de7e0e est la valeur `GenreId` pour le genre de technologie, l’en-tête de la page s lit « revues technologiques » et la liste à puces énumère les révisions sur le site qui se trouvent sous ce genre.

[![la page genre de technologie](deploying-a-database-vb/_static/image5.jpg)](deploying-a-database-vb/_static/image4.jpg) 

**Figure 2**: page genre de technologie ([cliquez pour afficher l’image en taille réelle](deploying-a-database-vb/_static/image6.jpg))

[![la révision pour enseigner vous-même ASP.NET 3,5 en 24 heures](deploying-a-database-vb/_static/image8.jpg)](deploying-a-database-vb/_static/image7.jpg) 

**Figure 3**: révision pour *enseigner vous-même ASP.net 3,5 en 24 heures* ([cliquez pour afficher l’image en taille réelle](deploying-a-database-vb/_static/image9.jpg))

L’application Web de révisions de livres comprend également une section d’administration dans laquelle les administrateurs peuvent ajouter, modifier et supprimer des genres, des révisions et des informations sur l’auteur. À l’heure actuelle, tout visiteur peut accéder à la section Administration. Dans un prochain didacticiel, nous allons ajouter la prise en charge des comptes d’utilisateur et autoriser uniquement les utilisateurs autorisés dans les pages d’administration.

Si vous téléchargez l’application de révisions de livres, gardez à l’esprit que son objectif est de montrer comment déployer une application pilotée par les données. Il ne présente pas les meilleures pratiques en ce qui concerne la conception de l’application. Par exemple, il n’existe pas de couche d’accès aux données (DAL) distincte. les pages ASP.NET communiquent directement avec la base de données par le biais du contrôle SqlDataSource ou du code ADO.NET dans leurs classes code-behind. Pour plus d’informations sur la création d’applications pilotées par les données à l’aide d’une architecture à plusieurs niveaux, reportez-vous à mes didacticiels sur l' [ *utilisation des données* ](../../data-access/index.md).

## <a name="databases-on-development-versus-production"></a>Bases de données sur le développement et la production

Lorsque vous démarrez le développement sur une application Web pilotée par les données, vous devez spécifier une chaîne de connexion de base de données, qui fournit les détails de l’application sur la façon de se connecter à la base de données. Cette chaîne de connexion spécifie, entre autres, le serveur de base de données, le nom de la base de données et les informations de sécurité. La plupart du temps, la base de données utilisée par l’application pendant le développement est différente de la base de données utilisée lorsqu’elle est en production. L’utilisation de différentes bases de données pour le développement et la production présente de nombreux avantages. Le fait de disposer d’une base de données différente dans le développement signifie que vous ne devez pas avoir à vous soucier de la modification ou de la suppression accidentelle des données actives. Elle vous permet également de placer des données de test factices ou d’apporter des modifications avec rupture au modèle de données sans avoir à vous soucier des effets sur l’application en production. L’inconvénient de disposer d’une base de données différente dans les environnements de développement et de production est que, lorsque l’application est déployée, la base de données et toutes les modifications pertinentes apportées au schéma ou aux données de la base de données doivent également être déployées.

Avant le premier déploiement, il n’existe qu’une seule instance de la base de données, et cette instance se trouve dans l’environnement de développement. Lorsque vous déployez l’application en production pour la première fois, nous devons non seulement copier les fichiers nécessaires côté serveur et côté client, mais également copier la base de données de l’environnement de développement vers l’environnement de production. C’est là que nous nous prévoyons d’utiliser l’application Web de révisions de livres : la base de données réside dans le dossier `App_Data` de notre environnement de développement, mais n’a pas encore été envoyé à l’environnement de production.

Une fois que l’application a été déployée, il existe deux copies de la base de données. À mesure que l’application arrive à maturité, de nouvelles fonctionnalités peuvent être ajoutées, ce qui nécessite une modification du modèle de données (par exemple, l’ajout de nouvelles colonnes aux tables existantes, la modification des colonnes existantes, l’ajout de nouvelles tables, etc.). Lorsque l’application Web est déployée ensuite, les modifications appliquées à la base de données dans l’environnement de développement depuis le dernier déploiement doivent être appliquées à la base de données de production. Certaines stratégies de gestion de ce processus sont décrites dans un prochain didacticiel. Ce didacticiel se concentre sur le déploiement de l’intégralité de la base de données de l’environnement de développement vers la production.

## <a name="deploying-the-database-to-the-production-environment"></a>Déploiement de la base de données dans l’environnement de production

Le reste de ce didacticiel explique comment déployer la base de données de l’environnement de développement vers l’environnement de production. Si vous le souhaitez, vous devez vous assurer que votre compte avec votre fournisseur d’hébergement Web comprend la prise en charge de la base de données Microsoft SQL Server. Vous devez également avoir des informations à portée de main, à savoir le nom du serveur de base de données, le nom de la base de données, ainsi que le nom d’utilisateur et le mot de passe utilisés pour se connecter à la base de données.

Comme indiqué plus haut dans ce didacticiel, la base de données des sites Web des revues de livres est une base de données SQL Server 2008 Express, stockée dans le dossier `App_Data`. La raison pour laquelle le déploiement d’une telle base de données serait simple est de copier le dossier `App_Data` de l’environnement de développement vers l’environnement de production. Toutefois, la plupart des fournisseurs d’hôtes Web ne prennent pas en charge les bases de données d’hébergement dans le dossier `App_Data` pour des raisons de sécurité. Au lieu de cela, les hôtes Web fournissent un compte sur un serveur de base de données SQL Server au sein de leur environnement. Le déploiement de la base de données à partir de votre environnement de développement vers l’environnement de production nécessite l’inscription de votre base de données sur le serveur de base de données de votre hôte Web.

Comment faire passer votre base de données de l’environnement de développement à l’environnement de production ? Il existe plusieurs façons d’accomplir cela en fonction des services que votre hôte Web offre. Avec certains ordinateurs hôtes, tels que DiscountASP.NET, vous pouvez FTP une sauvegarde de la base de données ou du fichier de `.mdf` réel sur votre site Web, puis, à partir du panneau de configuration, restaurer le fichier de sauvegarde ou attacher le fichier `.mdf` au serveur de base de données SQL Server. Avec ces outils, le déploiement de la base de données est aussi simple que la copie du dossier `App_Data` dans l’environnement de production, puis son attachement via le panneau de configuration. C’est peut-être le moyen le plus simple et le plus rapide de publier votre base de données pour la première fois.

Une autre approche consiste à utiliser l’Assistant Publication de base de données. L’Assistant Publication de base de données est une application de bureau Windows qui génère les commandes SQL pour créer le schéma de votre base de données (tables, procédures stockées, vues, fonctions définies par l’utilisateur, etc.) et, éventuellement, les données de ses tables. Vous pouvez ensuite vous connecter à votre serveur de base de données fournisseur d’hébergement Web par le biais de SQL Server Management Studio puis exécuter ce script pour dupliquer la base de données en production. Mieux encore, si votre fournisseur d’hébergement Web prend en charge les [services de publication de base de données](http://www.codeplex.com/sqlhost/Wiki/View.aspx?title=Database%20Publishing%20Services&amp;referringTitle=Home) Microsoft, vous pouvez faire en sorte que le script généré par l’Assistant Publication de base de données soit automatiquement exécuté sur le serveur de base de données en votre nom. Étant donné que l’Assistant Publication de base de données génère un script qui crée le schéma et les données de la base de données, il fonctionne, que votre fournisseur d’hébergement Web offre des fonctionnalités telles que l’attachement d’un fichier de `.mdf` chargé.

### <a name="generating-the-sql-commands-to-create-the-database-schema-and-data-using-the-database-publishing-wizard"></a>Génération des commandes SQL pour créer le schéma et les données de la base de données à l’aide de l’Assistant Publication de base de données

Passons en revue l’utilisation de l’Assistant Publication de base de données pour déployer la base de données de révisions de livres en production. Si vous utilisez Visual Studio 2008 ou ultérieur, l’Assistant Publication de base de données est déjà installé. Si vous utilisez Visual Studio 2005, vous devrez d’abord [Télécharger et installer](https://www.microsoft.com/downloads/details.aspx?familyid=56E5B1C5-BF17-42E0-A410-371A838E570A&amp;displaylang=en) l’Assistant.

Ouvrez Visual Studio et accédez à la base de données `Reviews.mdf`. Si vous utilisez Visual Web Developer, accédez à la explorateur de base de données ; Si vous utilisez Visual Studio, utilisez l’Explorateur de serveurs. La figure 4 illustre la base de données `Reviews.mdf` dans le explorateur de base de données dans Visual Web Developer. Comme le montre la figure 4, la base de données `Reviews.mdf` est composée de quatre tables, de trois procédures stockées et d’une fonction définie par l’utilisateur.

[![Localisez la base de données dans le explorateur de base de données ou Explorateur de serveurs](deploying-a-database-vb/_static/image11.jpg)](deploying-a-database-vb/_static/image10.jpg) 

**Figure 4**: localiser la base de données dans le explorateur de base de données ou Explorateur de serveurs ([cliquez pour afficher l’image en taille réelle](deploying-a-database-vb/_static/image12.jpg))

Cliquez avec le bouton droit sur le nom de la base de données et choisissez l’option « publier sur le fournisseur » dans le menu contextuel. Cette opération lance l’Assistant Publication de base de données (voir la figure 5). Cliquez sur suivant pour passer au-delà de l’écran de démarrage.

[![l’écran de démarrage de l’Assistant Publication de base de données](deploying-a-database-vb/_static/image14.jpg)](deploying-a-database-vb/_static/image13.jpg) 

**Figure 5**: écran de démarrage de l’Assistant Publication de base de données ([cliquez pour afficher l’image en taille réelle](deploying-a-database-vb/_static/image15.jpg))

Le deuxième écran de l’Assistant répertorie les bases de données accessibles à l’Assistant Publication de base de données et vous permet de choisir entre l’exécution d’un script pour tous les objets de la base de données sélectionnée et la sélection des objets à écrire. Sélectionnez la base de données appropriée et laissez l’option « générer un script pour tous les objets dans la base de données sélectionnée » activée.

> [!NOTE]
> Si vous recevez l’erreur « il n’y a pas d’objets dans la base de données *DatabaseName* des types scriptable par cet Assistant » quand vous cliquez sur suivant dans l’écran illustré à la figure 6, assurez-vous que le chemin d’accès à votre fichier de base de données n’est pas trop long. Comme indiqué dans [cet élément de discussion](http://www.codeplex.com/sqlhost/Thread/View.aspx?ThreadId=11014) sur la page projet de l’Assistant Publication de base de données, cette erreur peut survenir si le chemin d’accès au fichier de base de données est trop long.

[![l’écran de démarrage de l’Assistant Publication de base de données](deploying-a-database-vb/_static/image17.jpg)](deploying-a-database-vb/_static/image16.jpg) 

**Figure 6**: écran de démarrage de l’Assistant Publication de base de données ([cliquez pour afficher l’image en taille réelle](deploying-a-database-vb/_static/image18.jpg))

Dans l’écran suivant, vous pouvez générer un fichier de script ou, si votre hôte Web le prend en charge, publier la base de données directement sur le serveur de base de données de votre fournisseur d’hébergement Web. Comme le montre la figure 7, le script est écrit dans le fichier `C:\REVIEWS.MDF.sql`.

[![écrire le script de la base de données dans un fichier ou le publier directement sur votre fournisseur d’hébergement Web](deploying-a-database-vb/_static/image20.jpg)](deploying-a-database-vb/_static/image19.jpg) 

**Figure 7**: générer un script de la base de données dans un fichier ou la publier directement sur votre fournisseur d’hôte Web ([cliquez pour afficher l’image en taille réelle](deploying-a-database-vb/_static/image21.jpg))

L’écran suivant vous invite à fournir une série d’options de script. Vous pouvez spécifier si le script doit inclure des instructions DROP pour supprimer ces objets existants. La valeur par défaut est true, ce qui est parfait lorsque vous déployez une base de données pour la première fois. Vous pouvez également spécifier si la base de données cible est SQL Server 2000, SQL Server 2005 ou SQL Server 2008. Enfin, vous pouvez indiquer s’il faut écrire le schéma et les données, uniquement les données ou uniquement le schéma. Le schéma est la collection d’objets de base de données, les tables, les procédures stockées, les vues, etc. Les données sont les informations qui résident dans les tables.

Comme illustré à la figure 8, j’ai reçu l’Assistant configuré pour supprimer les objets de base de données existants, à générer un script pour une base de données SQL Server 2008 et à publier le schéma et les données.

[![spécifier les options de publication](deploying-a-database-vb/_static/image23.jpg)](deploying-a-database-vb/_static/image22.jpg) 

**Figure 8**: spécifier les options de publication ([cliquez pour afficher l’image en taille réelle](deploying-a-database-vb/_static/image24.jpg))

Les deux écrans finaux résument les actions qui sont sur le lieu de prendre, puis affichent l’état des scripts. Le résultat net de l’exécution de l’Assistant est que nous disposons d’un fichier de script qui contient les commandes SQL nécessaires pour créer la base de données en production et la remplir avec les mêmes données que lors du développement.

### <a name="executing-the-sql-commands-on-the-production-environment-database"></a>Exécution des commandes SQL sur la base de données de l’environnement de production

Maintenant que nous disposons du script qui contient les commandes SQL permettant de créer la base de données et de ses données, il ne reste plus qu’à exécuter le script sur la base de données de production. Certains fournisseurs d’hôtes Web proposent une zone de texte dans le panneau de configuration, dans laquelle vous pouvez entrer des commandes SQL à exécuter sur votre base de données. Si vous avez un fichier de script très volumineux, cette option peut ne pas fonctionner (le `REVIEWS.MDF.sql` fichier de script a une taille supérieure à 425 Ko, par exemple).

Une meilleure approche consiste à se connecter directement au serveur de base de données de production à l’aide d’SQL Server Management Studio (SSMS). Si vous avez une édition non Express de SQL Server installée sur votre ordinateur, vous avez probablement déjà installé SSMS. Dans le cas contraire, vous pouvez [Télécharger et installer](https://www.microsoft.com/downloads/details.aspx?FamilyId=C243A5AE-4BD1-4E3D-94B8-5A0F62BF7796&amp;displaylang=en) une copie gratuite de SQL Server Management Studio Express Edition.

Lancez SSMS et connectez-vous à votre serveur de base de données de l’hôte Web à l’aide des informations fournies par votre fournisseur d’hébergement Web.

[![se connecter au serveur de base de données de votre fournisseur d’hébergement Web](deploying-a-database-vb/_static/image26.jpg)](deploying-a-database-vb/_static/image25.jpg) 

**Figure 9**: connexion à votre serveur de base de données fournisseur d’hébergement Web ([cliquez pour afficher l’image en taille réelle](deploying-a-database-vb/_static/image27.jpg))

Développez l’onglet bases de données et recherchez votre base de données. Cliquez sur le bouton nouvelle requête dans le coin supérieur gauche de la barre d’outils, collez les commandes SQL à partir du fichier de script créé par l’Assistant Publication de base de données, puis cliquez sur le bouton Exécuter pour exécuter ces commandes sur le serveur de base de données de production. Si votre fichier de script est particulièrement volumineux, l’exécution des commandes peut prendre plusieurs minutes.

[![se connecter au serveur de base de données de votre fournisseur d’hébergement Web](deploying-a-database-vb/_static/image29.jpg)](deploying-a-database-vb/_static/image28.jpg) 

**Figure 10**: se connecter au serveur de base de données de votre fournisseur d’hébergement Web ([cliquez pour afficher l’image en taille réelle](deploying-a-database-vb/_static/image30.jpg))

C’est tout ! À ce stade, la base de données de développement a été dupliquée en production. Si vous actualisez la base de données dans SSMS, vous devriez voir les nouveaux objets de base de données. La figure 11 montre les tables, les procédures stockées et les fonctions définies par l’utilisateur de la base de données de production, qui reflètent celles-ci sur la base de données de développement. Et parce que nous avons demandé à l’Assistant Publication de base de données de publier les données, les tables des bases de données de production ont les mêmes données que les tables des bases de données de développement au moment de l’exécution de l’Assistant. La figure 12 montre les données dans la table `Books` de la base de données de production.

[![les objets de base de données ont été dupliqués dans la base de données de production](deploying-a-database-vb/_static/image32.jpg)](deploying-a-database-vb/_static/image31.jpg) 

**Figure 11**: les objets de base de données ont été dupliqués dans la base de données de production ([cliquez pour afficher l’image en taille réelle](deploying-a-database-vb/_static/image33.jpg))

[![la base de données de production contient les mêmes données que celles de la base de données de développement](deploying-a-database-vb/_static/image35.jpg)](deploying-a-database-vb/_static/image34.jpg) 

**Figure 12**: la base de données de production contient les mêmes données que celles de la base de données de développement ([cliquez pour afficher l’image en taille réelle](deploying-a-database-vb/_static/image36.jpg))

À ce stade, nous avons uniquement déployé la base de données de développement en production. Nous n’avons pas encore abordé le déploiement de l’application Web elle-même ou examiné les modifications de configuration nécessaires pour que l’application en production utilise la base de données de production. Nous aborderons ces problèmes dans le prochain didacticiel.

## <a name="summary"></a>Récapitulatif

Le déploiement d’une application Web pilotée par les données nécessite la copie de la base de données utilisée pendant le développement vers l’environnement de production. De nombreux fournisseurs d’hébergement Web proposent des outils permettant de simplifier le processus de déploiement d’une base de données. Par exemple, avec DiscountASP.NET, vous pouvez FTP votre base de données `.mdf` fichier (ou une sauvegarde), puis attacher la base de données au serveur de base de données à partir du panneau de configuration. Une autre option qui fonctionne indépendamment des fonctionnalités proposées par votre fournisseur d’hôte Web est l’outil Assistant Publication de bases de données Microsoft s, qui génère un script de commandes SQL pour créer le schéma et les données de la base de données de développement. Une fois que ce script a été généré, vous pouvez l’exécuter sur la base de données de production.

Maintenant que la base de données des applications Web est en production, nous pouvons déployer l’application. Toutefois, les informations de configuration de l’application Web s spécifient la chaîne de connexion à la base de données, et cette chaîne de connexion fait référence à la base de données de développement. Nous devons mettre à jour ces informations de chaîne de connexion lors du déploiement du site en production. Le didacticiel suivant examine ces différences de configuration et décrit les étapes nécessaires à la publication du site de révisions de livres piloté par les données en production.

Bonne programmation !

### <a name="further-reading"></a>informations supplémentaires

Pour plus d’informations sur les sujets abordés dans ce didacticiel, reportez-vous aux ressources suivantes :

- [Télécharger l’Assistant Publication de base de données Microsoft SQL Server 1,1](https://www.microsoft.com/downloads/details.aspx?familyid=56E5B1C5-BF17-42E0-A410-371A838E570A&amp;displaylang=en)
- [Télécharger le Microsoft SQL Server Management Studio Express Edition](https://www.microsoft.com/downloads/details.aspx?FamilyId=C243A5AE-4BD1-4E3D-94B8-5A0F62BF7796&amp;displaylang=en)

> [!div class="step-by-step"]
> [Précédent](core-differences-between-iis-and-the-asp-net-development-server-vb.md)
> [Suivant](configuring-the-production-web-application-to-use-the-production-database-vb.md)
