---
uid: web-forms/overview/older-versions-getting-started/deploying-web-site-projects/deploying-your-site-using-an-ftp-client-cs
title: Déploiement de votre site à l’aide d’unC#client FTP () | Microsoft Docs
author: rick-anderson
description: La façon la plus simple de déployer une application ASP.NET consiste à copier manuellement les fichiers nécessaires de l’environnement de développement vers l’environnement de production. Thi...
ms.author: riande
ms.date: 04/01/2009
ms.assetid: a3599cf7-8474-4006-954a-3bc693736b66
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deploying-web-site-projects/deploying-your-site-using-an-ftp-client-cs
msc.type: authoredcontent
ms.openlocfilehash: a3474650939ee220b3fd712e9f5a6cf3db11db09
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/28/2019
ms.locfileid: "74621144"
---
# <a name="deploying-your-site-using-an-ftp-client-c"></a>Déploiement de votre site avec un client FTP (C#)

par [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Télécharger le code](https://download.microsoft.com/download/4/5/F/45F815EC-8B0E-46D3-9FB8-2DC015CCA306/ASPNET_Hosting_Tutorial_03_CS.zip) ou [Télécharger le PDF](https://download.microsoft.com/download/E/8/9/E8920AE6-D441-41A7-8A77-9EF8FF970D8B/aspnet_tutorial03_DeployingViaFTP_cs.pdf)

> La façon la plus simple de déployer une application ASP.NET consiste à copier manuellement les fichiers nécessaires de l’environnement de développement vers l’environnement de production. Ce didacticiel montre comment utiliser un client FTP pour récupérer les fichiers de votre bureau vers le fournisseur d’hébergement Web.

## <a name="introduction"></a>Introduction

Le didacticiel précédent a introduit une simple application Web ASP.NET, composée de quelques pages ASP.NET, d’une page maître, d’une classe de `Page` de base personnalisée, d’un certain nombre d’images et de trois feuilles de style CSS. Nous sommes maintenant prêts à déployer cette application sur un fournisseur d’hébergement Web. à ce stade, l’application sera accessible à toute personne disposant d’une connexion à Internet !

À partir de nos discussions dans le didacticiel [*détermination des fichiers à déployer*](determining-what-files-need-to-be-deployed-cs.md) , nous savons quels fichiers doivent être copiés sur le fournisseur de l’hôte Web. (N’oubliez pas que les fichiers copiés varient selon que votre application est compilée explicitement ou automatiquement.) Mais comment faire pour récupérer les fichiers de l’environnement de développement (notre poste de travail) jusqu’à l’environnement de production (le serveur Web géré par le fournisseur de l’hôte Web) ? [ **F** Ile **T** ransfer **P** rotocol (FTP)](http://en.wikipedia.org/wiki/File_Transfer_Protocol) est un protocole couramment utilisé pour copier des fichiers d’un ordinateur à un autre sur un réseau. Une autre option est FrontPage Server Extensions (FPSE). Ce didacticiel se concentre sur l’utilisation du logiciel client FTP autonome pour déployer les fichiers nécessaires de l’environnement de développement vers l’environnement de production.

> [!NOTE]
> Visual Studio comprend des outils pour la publication de sites Web via FTP. ces outils, ainsi qu’un aperçu des outils qui utilisent FPSE, sont traités dans le didacticiel suivant.

Pour copier les fichiers à l’aide de FTP, nous avons besoin d’un *client FTP* sur l’environnement de développement. Un client FTP est une application conçue pour copier des fichiers à partir de l’ordinateur sur lequel il est installé sur un ordinateur qui exécute un *serveur FTP*. (Si votre fournisseur d’hébergement Web prend en charge les transferts de fichiers via FTP, le plus souvent, un serveur FTP est en cours d’exécution sur leurs serveurs Web.) Un certain nombre d’applications clientes FTP sont disponibles. Votre navigateur Web peut même doubler en tant que client FTP. Mon client FTP préféré, et celui que je vais utiliser pour ce didacticiel, est [FileZilla](http://filezilla-project.org/), un client FTP gratuit et open source qui est disponible pour Windows, Linux et Mac. Tout client FTP fonctionnera, cependant, n’hésitez pas à utiliser le client avec lequel vous êtes le plus à l’aise.

Si vous suivez cette procédure, vous devez créer un compte avec un fournisseur d’hébergement Web pour pouvoir suivre ce didacticiel ou ceux qui suivent. Comme indiqué dans le didacticiel précédent, il existe un Gaggle de sociétés de fournisseurs d’hébergement Web avec un large éventail de prix, de fonctionnalités et de qualité de service. Pour cette série de didacticiels, j’utilise la [remise ASP.net](http://discountasp.net) comme fournisseur d’hébergement Web, mais vous pouvez suivre la procédure avec n’importe quel fournisseur d’hôte Web, à condition qu’ils prennent en charge la version ASP.net dans laquelle votre site est développé. (Ces didacticiels ont été créés à l’aide de ASP.NET 3,5.) En outre, étant donné que nous allons copier les fichiers sur le fournisseur d’hôte Web à l’aide de FTP dans ce didacticiel et à l’avenir, il est impératif que votre fournisseur d’hébergement Web prenne en charge l’accès FTP à leurs serveurs Web. Pratiquement tous les fournisseurs d’hébergement Web offrent cette fonctionnalité, mais vous devez vérifier avant de vous inscrire.

## <a name="deploying-the-book-review-web-application-project"></a>Déploiement du projet d’application Web de révision de livre

Rappelez-vous qu’il existe deux versions de l’application Web de revue de livre : une implémentée à l’aide du modèle de projet d’application Web (BookReviewsWAP) et l’autre à l’aide du modèle de projet de site Web (BookReviewsWSP). Le type de projet détermine si le site est compilé automatiquement ou explicitement, et que ce modèle de compilation dicte les fichiers qui doivent être déployés. Par conséquent, nous examinerons le déploiement des projets BookReviewsWAP et BookReviewsWSP séparément, en commençant par le BookReviewsWAP. Prenez un moment pour télécharger ces deux applications ASP.NET si vous ne l’avez pas déjà fait.

Lancez le projet BookReviewsWAP en accédant au dossier `BookReviewsWAP` et en double-cliquant sur le fichier `BookReviewsWAP.sln`. Avant de déployer le projet, il est important de le générer pour s’assurer que toutes les modifications apportées au code source sont incluses dans l’assembly compilé. Pour générer le projet, accédez au menu Générer et choisissez l’option de menu Générer BookReviewsWAP. Cela compile le code source du projet en un assembly unique, `BookReviewsWAP.dll`, qui est placé dans le dossier `Bin`.

Nous sommes maintenant prêts à déployer les fichiers nécessaires. Lancez votre client FTP et connectez-vous au serveur Web au niveau de votre fournisseur d’hébergement Web. (Lorsque vous vous inscrivez auprès d’une société d’hébergement Web, vous recevez un message électronique vous informant sur la façon de se connecter au serveur FTP, y compris l’adresse du serveur FTP, ainsi qu’un nom d’utilisateur et un mot de passe.)

Copiez les fichiers suivants à partir de votre bureau vers le dossier du site Web racine au niveau de votre fournisseur d’hébergement Web. Lorsque vous utilisez FTP dans le serveur Web sur le fournisseur de l’hôte Web, vous êtes probablement dans le répertoire racine du site Web. Toutefois, certains fournisseurs d’hôtes Web ont un sous-dossier nommé `www` ou `wwwroot` qui sert de dossier racine pour les fichiers de votre site Web. Enfin, lorsque vous FTPing les fichiers, vous devrez peut-être créer la structure de dossiers correspondante dans l’environnement de production (le dossier `Bin`, le dossier `Fiction`, le dossier `Images`, etc.).

- `~/Default.aspx`
- `~/About.aspx`
- `~/Site.master`
- `~/Web.config`
- `~/Web.sitemap`
- Le contenu complet du dossier `Styles`
- Le contenu complet du dossier `Images` (et de son sous-dossier, `BookCovers`)
- `~/Fiction/Default.aspx`
- `~/Fiction/Blaze.aspx`
- `~/Tech/Default.aspx`
- `~/Tech/CYOW.aspx`
- `~/Tech/TYASP35.aspx`
- `~/Bin/BookReviewsWAP.dll`

La figure 1 montre FileZilla après que les fichiers nécessaires ont été copiés. FileZilla affiche les fichiers sur l’ordinateur local à gauche et les fichiers sur l’ordinateur distant à droite. Comme le montre la figure 1, les fichiers de code source ASP.NET, tels que `About.aspx.cs`, se trouvent sur l’ordinateur local (l’environnement de développement) mais n’ont pas été copiés sur le fournisseur de l’hôte Web (environnement de production), car les fichiers de code n’ont pas besoin d’être déployés lors de l’utilisation de la compilation explicite.

> [!NOTE]
> La présence des fichiers de code source sur le serveur de production n’a pas de nuire, car ils sont ignorés. ASP.NET interdit les requêtes HTTP aux fichiers de code source par défaut, de sorte que même si les fichiers de code source sont présents sur le serveur de production, ils ne sont pas accessibles aux visiteurs de votre site Web. (Autrement dit, si un utilisateur tente de visiter `http://www.yoursite.com/Default.aspx.cs`, une page d’erreur s’affiche pour vous expliquer que ces types de fichiers-`.cs` fichiers-sont interdits.)

[![utiliser un client FTP pour copier les fichiers nécessaires à partir de votre bureau vers le serveur Web au niveau du fournisseur d’hébergement Web](deploying-your-site-using-an-ftp-client-cs/_static/image2.png)](deploying-your-site-using-an-ftp-client-cs/_static/image1.png)

**Figure 1**: utiliser un client FTP pour copier les fichiers nécessaires à partir de votre bureau vers le serveur Web sur le fournisseur de l’hôte Web ([cliquez pour afficher l’image en taille réelle](deploying-your-site-using-an-ftp-client-cs/_static/image3.png))

Après avoir déployé votre site, prenez un moment pour tester le site. Si vous avez acheté un nom de domaine et que vous avez correctement configuré les paramètres DNS, vous pouvez accéder à votre site en entrant votre nom de domaine. Votre fournisseur d’hébergement Web doit également vous avoir fourni une URL vers votre site, *qui ressemble à*un nom de service. *webhostprovider*. com ou *webhostprovider*. com/*AccountName*. Par exemple, l’URL de mon compte sur discount ASP.NET est la suivante : `http://httpruntime.web703.discountasp.net`.

La figure 2 illustre le site de révisions de livres déployé. Notez que je l’affiche sur la page de remise ASP. Les serveurs du réseau, sur `http://httpruntime.web703.discountasp.net`. À ce moment-là, toute personne disposant d’une connexion à Internet pourrait afficher mon site Web ! Comme nous l’avons prévu, le site se présente comme s’il s’agissait d’un test dans l’environnement de développement.

> [!NOTE]
> Si vous recevez une erreur lors de l’affichage de votre application, prenez un moment pour vous assurer que vous avez déployé le jeu de fichiers approprié. Ensuite, vérifiez le message d’erreur pour voir s’il révèle des indices sur le problème. Après cela, vous pouvez vous tourner vers le support technique de votre entreprise Web hôte ou poster votre question sur le forum approprié sur les [Forums ASP.net](https://forums.asp.net/).

[![le site de révisions de livres est désormais accessible à toute personne disposant d’une connexion Internet](deploying-your-site-using-an-ftp-client-cs/_static/image5.png)](deploying-your-site-using-an-ftp-client-cs/_static/image4.png)

**Figure 2**: le site de révisions de livres est désormais accessible à toute personne disposant d’une connexion Internet ([cliquez pour afficher l’image en taille réelle](deploying-your-site-using-an-ftp-client-cs/_static/image6.png))

## <a name="deploying-the-book-review-web-site-project"></a>Déploiement du projet de site Web de révision des livres

Lors du déploiement d’une application ASP.NET qui utilise la compilation automatique, telle que le projet de site Web BookReviewsWSP, il n’existe aucun assembly compilé dans le dossier `Bin`. Par conséquent, les fichiers de code source de l’application Web doivent être déployés dans l’environnement de production. Passons en revue ce processus.

Comme pour le projet d’application Web, il est judicieux de commencer par créer l’application avant de la déployer. Alors que la génération d’un projet de site Web ne crée pas d’assembly, il vérifie toutes les erreurs de compilation dans la page. Mieux pour trouver ces erreurs maintenant plutôt que d’avoir un visiteur pour votre site, Découvrez-les pour vous !

Une fois que vous avez correctement créé le projet, utilisez votre client FTP pour copier les fichiers suivants dans le dossier du site Web racine au niveau de votre fournisseur d’hébergement Web. Vous devrez peut-être créer la structure de dossiers correspondante sur l’environnement de production.

> [!NOTE]
> Si vous avez déjà déployé le projet BookReviewsWAP tout en conservant le déploiement du projet BookReviewsWSP, commencez par supprimer tous les fichiers sur le serveur Web qui ont été téléchargés lors du déploiement de BookReviewsWAP, puis déployez les fichiers pour BookReviewsWSP.

- `~/Default.aspx`
- `~/Default.aspx.cs`
- `~/About.aspx`
- `~/About.aspx.cs`
- `~/Site.master`
- `~/Site.master.cs`
- `~/Web.config`
- `~/Web.sitemap`
- Le contenu complet du dossier `Styles`
- Le contenu complet du dossier `Images` (et de son sous-dossier, `BookCovers`)
- `~/App_Code/BasePage.cs`
- `~/Fiction/Default.aspx`
- `~/Fiction/Default.aspx.cs`
- `~/Fiction/Blaze.aspx`
- `~/Fiction/Blaze.aspx.cs`
- `~/Tech/Default.aspx`
- `~/Tech/Default.aspx.cs`
- `~/Tech/CYOW.aspx`
- `~/Tech/CYOW.aspx.cs`
- `~/Tech/TYASP35.aspx`
- `~/Tech/TYASP35.aspx.cs`

La figure 3 montre FileZilla après avoir copié les fichiers nécessaires. Comme vous pouvez le voir, les fichiers de code source ASP.NET, tels que `About.aspx.cs`, sont présents sur l’ordinateur local (l’environnement de développement) et le fournisseur de l’hôte Web (l’environnement de production), car les fichiers de code doivent être déployés lors de l’utilisation de la compilation automatique.

[![utiliser un client FTP pour copier les fichiers nécessaires à partir de votre bureau vers le serveur Web au niveau du fournisseur d’hébergement Web](deploying-your-site-using-an-ftp-client-cs/_static/image8.png)](deploying-your-site-using-an-ftp-client-cs/_static/image7.png)

**Figure 3**: utiliser un client FTP pour copier les fichiers nécessaires à partir de votre bureau vers le serveur Web sur le fournisseur de l’hôte Web ([cliquez pour afficher l’image en taille réelle](deploying-your-site-using-an-ftp-client-cs/_static/image9.png))

L’expérience utilisateur n’est pas affectée par le modèle de compilation de l’application. Les mêmes pages ASP.NET sont accessibles et elles s’affichent et se comportent de la même façon que le site Web ait été créé à l’aide du modèle de projet d’application Web ou du modèle de projet de site Web.

### <a name="updating-a-web-application-on-production"></a>Mise à jour d’une application Web en production

Le développement et le déploiement d’applications Web ne sont pas un processus unique. Par exemple, lors de la création du site Web de revue de livres, j’ai créé les différentes pages et j’ai écrit le code associé sur mon ordinateur personnel (l’environnement de développement). Après avoir atteint un certain état stable, j’ai déployé mon application afin que d’autres personnes puissent visiter le site et lire mes avis. Mais le déploiement ne marque pas la fin de mon développement sur ce site. Je peux ajouter d’autres révisions de livres ou implémenter de nouvelles fonctionnalités, telles que permettre à mes visiteurs d’évaluer les livres ou de laisser leurs commentaires. Ces améliorations sont développées sur l’environnement de développement et, une fois terminées, doivent être déployées. Le développement et le déploiement, par conséquent, sont cycliques. Vous développez une application, puis la déployez. Lorsque le site est en ligne et en production, de nouvelles fonctionnalités sont ajoutées et des bogues sont corrigés au fil du temps, ce qui nécessite le redéploiement de l’application. Et ainsi de suite.

Comme vous pouvez vous y attendre, lorsque vous redéployez une application Web, il vous suffit de copier les fichiers nouveaux et modifiés. Il n’est pas nécessaire de redéployer les pages inchangées ou les fichiers de prise en charge côté serveur ou côté client (même si cela n’est pas affecté).

> [!NOTE]
> Gardez à l’esprit que lorsque vous utilisez la compilation explicite, chaque fois que vous ajoutez une nouvelle page ASP.NET au projet ou que vous apportez des modifications au code, vous devez régénérer votre projet, ce qui met à jour l’assembly dans le dossier `Bin`. Par conséquent, vous devrez copier cet assembly mis à jour en production lors de la mise à jour d’une application Web en production (avec les autres contenus nouveaux et mis à jour).

Sachez également que toute modification apportée au `Web.config` ou aux fichiers du répertoire `Bin` s’arrête et redémarre le pool d’applications du site Web. Si votre état de session est stocké à l’aide du mode `InProc` (valeur par défaut), les visiteurs de votre site perdent leur état de session chaque fois que ces fichiers de clé sont modifiés. Pour éviter ce piège, envisagez de stocker la session à l’aide des modes `StateServer` ou `SQLServer`. Pour plus d’informations sur cette rubrique [, consultez modes d’état de session](https://msdn.microsoft.com/library/ms178586.aspx).

Enfin, gardez à l’esprit que le redéploiement d’une application peut prendre de quelques secondes à plusieurs minutes, selon le nombre et la taille des fichiers qui doivent être copiés dans l’environnement de production. Pendant ce temps, les utilisateurs visitant votre site peuvent rencontrer des erreurs ou un comportement étrange. Vous pouvez « désactiver » l’intégralité de votre application en ajoutant une page nommée `App_Offline.htm` au répertoire racine de votre application, qui explique à vos utilisateurs que le site est arrêté en vue d’une maintenance (ou autre) et qu’il sera bientôt sauvegardé. Lorsque le `App_Offline.htm` fichier est présent, le runtime ASP.NET redirige toutes les demandes entrantes vers cette page.

## <a name="summary"></a>Récapitulatif

Le déploiement d’une application Web consiste à copier les fichiers nécessaires de l’environnement de développement vers l’environnement de production. Le moyen le plus courant de transférer les fichiers sur un réseau est le protocole FTP (FTP), et la plupart des fournisseurs d’hôtes Web prennent en charge l’accès FTP à leurs serveurs Web. Dans ce didacticiel, nous avons vu comment utiliser un client FTP pour déployer les fichiers nécessaires sur le serveur Web. Une fois déployé, le site Web peut être visité par toute personne disposant d’une connexion à Internet.

Bonne programmation !

### <a name="further-reading"></a>informations supplémentaires

Pour plus d’informations sur les sujets abordés dans ce didacticiel, reportez-vous aux ressources suivantes :

- [L’application\_offline. htm et contourner la fonctionnalité « Erreurs conviviales d’Internet Explorer »](https://weblogs.asp.net/scottgu/App_5F00_Offline.htm-and-working-around-the-_2200_IE-Friendly-Errors_2200_-feature)
- [Modes d’état de session](https://msdn.microsoft.com/library/ms178586.aspx)

> [!div class="step-by-step"]
> [Précédent](determining-what-files-need-to-be-deployed-cs.md)
> [Suivant](deploying-your-site-using-visual-studio-cs.md)
