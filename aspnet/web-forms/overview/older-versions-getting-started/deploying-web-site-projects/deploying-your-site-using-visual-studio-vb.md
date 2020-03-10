---
uid: web-forms/overview/older-versions-getting-started/deploying-web-site-projects/deploying-your-site-using-visual-studio-vb
title: Déploiement de votre site à l’aide de Visual Studio (VB) | Microsoft Docs
author: rick-anderson
description: Visual Studio comprend des outils pour le déploiement d’un site Web. En savoir plus sur ces outils dans ce didacticiel.
ms.author: riande
ms.date: 04/01/2009
ms.assetid: 977105f3-7987-4e50-8be7-afb53b4ca28a
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deploying-web-site-projects/deploying-your-site-using-visual-studio-vb
msc.type: authoredcontent
ms.openlocfilehash: 6c71e36a8a434947882cc767cd2f903ff6e8d422
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78587212"
---
# <a name="deploying-your-site-using-visual-studio-vb"></a>Déploiement de votre site avec Visual Studio (VB)

par [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Télécharger le code](https://download.microsoft.com/download/4/5/F/45F815EC-8B0E-46D3-9FB8-2DC015CCA306/ASPNET_Hosting_Tutorial_04_VB.zip) ou [Télécharger le PDF](https://download.microsoft.com/download/E/8/9/E8920AE6-D441-41A7-8A77-9EF8FF970D8B/aspnet_tutorial04_DeployingViaVS_vb.pdf)

> Visual Studio comprend des outils pour le déploiement d’un site Web. En savoir plus sur ces outils dans ce didacticiel.

## <a name="introduction"></a>Introduction

Le didacticiel précédent a vu comment déployer une application Web ASP.NET simple sur un fournisseur d’hébergement Web. Plus précisément, le didacticiel a montré comment utiliser un client FTP tel que FileZilla pour transférer les fichiers nécessaires de l’environnement de développement vers l’environnement de production. Visual Studio offre également des outils intégrés pour faciliter le déploiement sur un fournisseur d’hébergement Web. Ce didacticiel examine deux de ces outils : l’outil Copier le site Web, dans lequel vous pouvez déplacer des fichiers vers et à partir d’un serveur Web distant à l’aide du protocole FTP ou FrontPage Server Extensions ; et l’outil de publication, qui copie l’intégralité du site Web à un emplacement spécifié.

> [!NOTE]
> Les autres outils liés au déploiement proposés par Visual Studio incluent [le complément projets d’installation Web](https://msdn.microsoft.com/library/wx3b589t.aspx) et [projets de déploiement Web](https://www.microsoft.com/downloads/details.aspx?FamilyId=0AA30AE8-C73B-4BDD-BB1B-FE697256C459&amp;displaylang=en) . Les projets d’installation Web compressent le contenu et les informations de configuration d’un site Web dans un seul fichier MSI. Cette option est particulièrement utile pour les sites Web déployés au sein d’un intranet ou pour les sociétés qui vendent une application Web pré-packagée que les clients installent sur leurs propres serveurs Web. Le complément projets de déploiement Web est un complément Visual Studio qui facilite la spécification des différences de configuration entre les builds pour les environnements de développement et les environnements de production. Les projets d’installation Web ne sont pas abordés dans cette série de didacticiels. Les projets de déploiement Web sont résumés dans les différences de configuration courantes entre les didacticiels de [*développement et de production*](common-configuration-differences-between-development-and-production-vb.md) .

## <a name="deploying-your-site-using-the-copy-web-site-tool"></a>Déploiement de votre site à l’aide de l’outil Copier le site Web

L’outil Copier le site Web de Visual Studio présente des fonctionnalités similaires à celles d’un client FTP autonome. En résumé, l’outil Copier le site Web vous permet de vous connecter à un site Web distant via FTP ou FrontPage Server Extensions. À l’instar de l’interface utilisateur de FileZilla, l’interface utilisateur copier le site Web se compose de deux volets : le volet gauche répertorie les fichiers locaux, tandis que le volet droit répertorie ces fichiers sur le serveur de destination.

> [!NOTE]
> L’outil Copier le site Web est uniquement disponible pour les projets de site Web. Visual Studio offre cet outil lorsque vous travaillez avec un projet d’application Web.

Jetons un coup d’œil à l’utilisation de l’outil Copier le site Web pour publier l’application de révision de livre en production. Étant donné que l’outil Copier le site Web fonctionne uniquement avec des projets qui utilisent le modèle de projet de site Web, nous pouvons uniquement les examiner à l’aide de cet outil avec le projet BookReviewsWSP. Ouvrez ce projet.

Lancez le projet d’outil Copier le site Web en cliquant sur l’icône copier le site Web dans le Explorateur de solutions (cette icône est entourée de la figure 1). vous pouvez également sélectionner l’option Copier le site Web dans le menu du site Web. L’une ou l’autre approche lance l’interface utilisateur copier le site Web illustrée dans la figure 1. seul le volet gauche de la figure 1 est rempli parce que nous n’avons pas encore pu vous connecter à un serveur distant.

[![l’interface utilisateur de l’outil Copier le site Web est divisée en deux volets](deploying-your-site-using-visual-studio-vb/_static/image2.png)](deploying-your-site-using-visual-studio-vb/_static/image1.png)

**Figure 1**: l’interface utilisateur de l’outil Copier le site Web est divisée en deux volets ([cliquez pour afficher l’image en taille réelle](deploying-your-site-using-visual-studio-vb/_static/image3.png))

Pour déployer notre site, nous devons tout d’abord vous connecter au fournisseur d’hébergement Web. Cliquez sur le bouton se connecter en haut de l’interface utilisateur copier le site Web. La boîte de dialogue Ouvrir le site Web s’affiche, comme illustré à la figure 2.

Vous pouvez vous connecter au site Web de destination en sélectionnant l’une des quatre options à partir de la gauche :

- **Système de fichiers** : sélectionnez cette option pour déployer votre site vers un dossier ou un partage réseau accessible à partir de votre ordinateur.
- **IIS local** : utilisez cette option pour déployer le site sur le serveur Web IIS installé sur votre ordinateur.
- **Site FTP** : Connectez-vous à un site Web distant via FTP.
- **Site distant** : Connectez-vous à un site Web à distance à l’aide de FrontPage Server Extensions.

La plupart des fournisseurs d’hébergement Web prennent en charge FTP, mais ils offrent moins de prise en charge des extensions serveur FrontPage. Pour cette raison, j’ai sélectionné l’option site FTP, puis j’ai entré les informations de connexion, comme illustré à la figure 2.

[![spécifier le site Web de destination](deploying-your-site-using-visual-studio-vb/_static/image5.png)](deploying-your-site-using-visual-studio-vb/_static/image4.png)

**Figure 2**: spécifier le site Web de destination ([cliquez pour afficher l’image en taille réelle](deploying-your-site-using-visual-studio-vb/_static/image6.png))

Une fois que vous êtes connecté, l’outil Copier le site Web charge les fichiers sur le site distant dans le volet droit et indique l’état de chaque fichier : nouveau, supprimé, modifié ou inchangé. Vous pouvez copier un fichier du site local vers le site distant, ou vice-versa.

Nous allons ajouter une nouvelle page au projet BookReviewsWSP, puis la déployer afin de pouvoir voir l’outil Copier le site Web en action. Créez une nouvelle page ASP.NET dans Visual Studio dans le répertoire racine nommé `Privacy.aspx`. Faites en sorte que la page utilise la page maître `Site.master` et ajoutez la politique de confidentialité de votre site à cette page. La figure 3 montre Visual Studio une fois cette page créée.

[![ajouter une nouvelle page nommée &lt;code&gt;privacy. aspx&lt;/code&gt; dans le dossier racine du site Web](deploying-your-site-using-visual-studio-vb/_static/image8.png)](deploying-your-site-using-visual-studio-vb/_static/image7.png)

**Figure 3**: ajouter une nouvelle Page nommée `Privacy.aspx` au dossier racine du site Web ([cliquez pour afficher l’image en taille réelle](deploying-your-site-using-visual-studio-vb/_static/image9.png))

Ensuite, revenez à l’interface utilisateur copier le site Web. Comme le montre la figure 4, le volet gauche comprend désormais les nouveaux fichiers-`Policy.aspx` et `Policy.aspx.vb`. De plus, ces fichiers sont marqués d’une icône en flèche et d’un état nouveau, indiquant qu’ils existent sur le site local, mais pas sur le site distant.

[![l’outil Copier le site Web comprend la nouvelle &lt;code&gt;confidentialité. aspx&lt;la page/code&gt; dans le volet gauche](deploying-your-site-using-visual-studio-vb/_static/image11.png)](deploying-your-site-using-visual-studio-vb/_static/image10.png)

**Figure 4**: l’outil Copier le site Web comprend la Page nouveau `Privacy.aspx` dans le volet gauche ([cliquez pour afficher l’image en taille réelle](deploying-your-site-using-visual-studio-vb/_static/image12.png))

Pour déployer les nouveaux fichiers, sélectionnez-les, puis cliquez sur l’icône de flèche pour les transférer vers le site distant. Une fois le transfert terminé, le `Policy.aspx` et les fichiers `Policy.aspx.vb` existent sur les sites locaux et distants avec l’État inchangé.

En plus de répertorier les nouveaux fichiers, l’outil Copier le site Web met en évidence tous les fichiers qui diffèrent entre les sites locaux et distants. Pour voir cela en action, revenez à la page `Privacy.aspx` et ajoutez quelques mots supplémentaires à la politique de confidentialité. Enregistrez la page, puis revenez à l’outil Copier le site Web. Comme le montre la figure 5, la page `Privacy.aspx` dans le volet gauche a l’état modifié, ce qui indique qu’elle n’est pas synchronisée avec le site distant.

[![l’outil Copier le site Web indique que la &lt;code&gt;privacy. aspx&lt;la page/code&gt; a été modifiée](deploying-your-site-using-visual-studio-vb/_static/image14.png)](deploying-your-site-using-visual-studio-vb/_static/image13.png)

**Figure 5**: l’outil Copier le site Web indique que la Page de `Privacy.aspx` a été modifiée ([cliquez pour afficher l’image en taille réelle](deploying-your-site-using-visual-studio-vb/_static/image15.png))

L’outil Copier le site Web indique également si un fichier a été supprimé depuis la dernière opération de copie. Supprimer le `Privacy.aspx` du projet local et actualiser l’outil Copier le site Web. Les fichiers `Privacy.aspx` et `Privacy.aspx.vb` restent listés dans le volet gauche, mais ont un état supprimé indiquant qu’ils ont été supprimés depuis la dernière opération de copie.

## <a name="publishing-a-web-application"></a>Publication d’une application Web

Une autre façon de déployer votre application Web à partir de Visual Studio consiste à utiliser l’option publier, accessible via le menu Générer. L’option de publication compile explicitement l’application, puis copie tous les fichiers nécessaires jusqu’au site distant spécifié. Comme nous le verrons bientôt, l’option de publication est plus pointue que l’outil Copier le site Web. Tandis que l’outil Copier le site Web vous permet d’examiner les fichiers sur les sites locaux et distants et vous permet de télécharger des fichiers individuels en fonction des besoins, l’option publier déploie l’application Web entière.

En plus de copier tous les fichiers nécessaires sur le site distant spécifié, l’option de publication compile également explicitement l’application. Étant donné que les projets d’application Web doivent être compilés explicitement, il n’est pas surprenant que l’option de publication soit disponible pour les projets d’application Web. Ce qui peut être un peu surprenant, c’est que l’option de publication est également disponible pour les projets de site Web. Comme indiqué dans le didacticiel [*détermination des fichiers à déployer*](determining-what-files-need-to-be-deployed-vb.md) , les projets de site Web peuvent être compilés explicitement à l’aide d’un processus appelé *pré-compilation*. Ce didacticiel se concentre sur l’utilisation de l’option de publication avec des projets d’application Web. un futur didacticiel explore la précompilation. à ce stade, nous allons revenir à l’utilisation de l’option de publication avec les projets de site Web.

> [!NOTE]
> Alors que l’option publier est disponible dans Visual Studio pour les projets de site Web et les projets d’application Web, Visual Web Developer propose uniquement l’option de publication pour les projets d’application Web.

Examinons le déploiement de l’application de révisions de livres à l’aide de l’option publier. Commencez par ouvrir BookReviewsWAP (le projet d’application Web) dans Visual Studio. Dans le menu publier, choisissez le projet générer BookReviewsWAP. Cela ouvre une boîte de dialogue qui vous invite à entrer l’emplacement cible, parmi d’autres options de configuration (voir figure 6). À l’instar de l’outil Copier le site Web, vous pouvez entrer un emplacement qui pointe vers un dossier local, un site Web local sur IIS, un site Web distant qui prend en charge FrontPage Server Extensions ou une adresse de serveur FTP. Vous pouvez choisir de remplacer ou non les fichiers sur le serveur Web distant par les fichiers déployés ou de supprimer tout le contenu sur le site distant avant la publication. Vous pouvez également spécifier s’il faut copier :

- Seuls les fichiers du projet ont besoin d’exécuter l’application, ce qui omet le code source et les fichiers associés au projet inutiles.
- Tous les fichiers projet, y compris les fichiers de code source et les fichiers projet Visual Studio, comme le fichier solution.
- Tous les fichiers du dossier source du projet, qui copie tous les fichiers dans le dossier du projet source, qu’ils soient inclus ou non dans le projet.

Il existe également une option permettant de télécharger le contenu du dossier `App_Data`.

[![spécifier le site Web de destination](deploying-your-site-using-visual-studio-vb/_static/image17.png)](deploying-your-site-using-visual-studio-vb/_static/image16.png)

**Figure 6**: spécifier le site Web de destination ([cliquez pour afficher l’image en taille réelle](deploying-your-site-using-visual-studio-vb/_static/image18.png))

Pour l’application de revue de livre, le site distant contient les fichiers déployés lors de la copie du projet BookReviewsWSP via l’outil Copier le site Web. Par conséquent, l’option de publication commence par supprimer tout le contenu existant. En outre, nous allons simplement copier les fichiers nécessaires plutôt que d’encombrer l’environnement de production avec des fichiers de code source et de projet inutiles. Après avoir spécifié ces options, cliquez sur le bouton publier. Au cours des prochaines secondes, Visual Studio déploie les fichiers nécessaires sur le site de destination, en affichant sa progression dans la fenêtre sortie.

La figure 7 montre les fichiers sur le site FTP une fois l’opération de publication terminée. Notez que seules les pages de balisage et les fichiers de prise en charge côté client et serveur nécessaires ont été téléchargés.

[![seuls les fichiers nécessaires ont été publiés dans l’environnement de production](deploying-your-site-using-visual-studio-vb/_static/image20.png)](deploying-your-site-using-visual-studio-vb/_static/image19.png)

**Figure 7**: seuls les fichiers nécessaires ont été publiés dans l’environnement de production ([cliquez pour afficher l’image en taille réelle](deploying-your-site-using-visual-studio-vb/_static/image21.png))

L’option publier est un outil moins nuanced que l’outil Copier le site Web. Tandis que l’outil Copier le site Web vous permet d’inspecter les fichiers sur les sites locaux et distants et de voir en quoi ils diffèrent, l’option publier ne fournit aucune interface de ce type. En outre, l’outil Copier le site Web vous permet d’effectuer des modifications uniques, de charger ou de supprimer des fichiers individuels. L’option de publication n’autorise pas ce contrôle affiné. au lieu de cela, il publie l’application *entière* . Ce comportement a ses avantages et ses inconvénients. Du côté plus, vous savez quand vous utilisez l’option de publication vous n’oublierez pas de charger un fichier important. Mais envisagez ce qui se passe si vous avez apporté une petite modification à un site Web de très grande taille. avec l’option publier, vous ne pouvez pas mettre à jour cette ou deux pages qui ont été modifiées, mais vous devez au contraire attendre que Visual Studio déploie l’ensemble du site.

Il n’est pas rare qu’il y ait certains fichiers dont le contenu diffère entre les environnements de production et de développement. Un exemple de clé est le fichier de configuration de l’application, `Web.config`. Étant donné que l’option de publication copie aveuglément les fichiers de l’application Web, elle remplace les fichiers de configuration personnalisés de l’environnement de production par la version dans l’environnement de développement. Le didacticiel suivant explore ce sujet plus en détail et propose des conseils pour le déploiement d’une application Web lorsque ces différences existent.

## <a name="summary"></a>Récapitulatif

Le déploiement d’un site Web consiste à copier les fichiers nécessaires de l’environnement de développement vers l’environnement de production. Le didacticiel précédent a montré comment transférer des fichiers à l’aide d’un client FTP tel que FileZilla. Ce didacticiel a examiné deux outils de déploiement dans Visual Studio : l’outil Copier le site Web et l’option publier. L’outil Copier le site Web est similaire à un client FTP, car il dispose d’une interface à deux volets répertoriant les fichiers sur l’ordinateur local et un ordinateur distant spécifié, ce qui facilite le chargement ou le téléchargement de fichiers entre les deux ordinateurs. L’option de publication est un outil plus pointu qui compile explicitement le projet, puis déploie l’application entière dans la destination spécifiée.

Bonne programmation !

### <a name="further-reading"></a>informations supplémentaires

Pour plus d’informations sur les sujets abordés dans ce didacticiel, reportez-vous aux ressources suivantes :

- [Copie du site Web avec l’outil Copier le site Web](https://msdn.microsoft.com/library/1cc82atw.aspx)
- [Comment faire : déployer un site Web à l’aide de l’outil Copier le site Web](../../../videos/how-do-i/how-do-i-deploy-a-web-site-using-the-copy-web-site-tool.md) (vidéo)
- [Comment : publier des projets d’application Web](https://msdn.microsoft.com/library/aa983453.aspx)
- [Comment : publier des sites Web](https://msdn.microsoft.com/library/20yh9f1b.aspx)
- [Projets d’installation et de déploiement dans Visual Studio](https://msdn.microsoft.com/library/wx3b589t.aspx)

> [!div class="step-by-step"]
> [Précédent](deploying-your-site-using-an-ftp-client-vb.md)
> [Suivant](common-configuration-differences-between-development-and-production-vb.md)
