---
uid: web-forms/overview/older-versions-getting-started/deploying-web-site-projects/determining-what-files-need-to-be-deployed-cs
title: Détermination des fichiers qui doivent être déployésC#() | Microsoft Docs
author: rick-anderson
description: Les fichiers qui doivent être déployés à partir de l’environnement de développement vers l’environnement de production dépendent en partie du fait que l’application ASP.NET a été générée...
ms.author: riande
ms.date: 04/01/2009
ms.assetid: f8d78a88-cc91-40d8-bce3-3d7954f6033b
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deploying-web-site-projects/determining-what-files-need-to-be-deployed-cs
msc.type: authoredcontent
ms.openlocfilehash: 1dd4a1179d32f776626c08a07205dc9aabed588d
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/28/2019
ms.locfileid: "74596393"
---
# <a name="determining-what-files-need-to-be-deployed-c"></a>Détermination des fichiers qui doivent être déployés (C#)

par [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Télécharger le code](https://download.microsoft.com/download/4/5/F/45F815EC-8B0E-46D3-9FB8-2DC015CCA306/ASPNET_Hosting_Tutorial_02_CS.zip) ou [Télécharger le PDF](https://download.microsoft.com/download/E/8/9/E8920AE6-D441-41A7-8A77-9EF8FF970D8B/aspnet_tutorial02_FilesToDeploy_cs.pdf)

> Les fichiers qui doivent être déployés à partir de l’environnement de développement vers l’environnement de production dépendent en partie du fait que l’application ASP.NET a été créée à l’aide du modèle de site Web ou du modèle d’application Web. En savoir plus sur ces deux modèles de projet et sur la manière dont le modèle de projet affecte le déploiement.

## <a name="introduction"></a>Introduction

Le déploiement d’une application Web ASP.NET implique la copie des fichiers liés à ASP.NET à partir de l’environnement de développement vers l’environnement de production. Les fichiers associés à ASP.NET incluent le balisage de page Web ASP.NET et le code, ainsi que les fichiers de prise en charge côté client et côté serveur. Les fichiers de prise en charge côté client sont les fichiers référencés par vos pages Web et envoyés directement aux images de navigateur, aux fichiers CSS et aux fichiers JavaScript, par exemple. Les fichiers de prise en charge côté serveur incluent ceux qui sont utilisés pour traiter une demande côté serveur. Il s’agit notamment des fichiers de configuration, des services Web, des fichiers de classe, des DataSets typés et des fichiers de LINQ to SQL, entre autres.

En général, tous les fichiers de prise en charge côté client doivent être copiés de l’environnement de développement vers l’environnement de production, mais les fichiers de prise en charge côté serveur sont copiés selon que vous compilez explicitement le code côté serveur dans un assembly (fichier `.dll`) ou si ces assemblys sont générés automatiquement. Ce didacticiel met en évidence les fichiers qui doivent être déployés lorsque vous compilez explicitement le code dans un assembly et que cette étape de compilation se produit automatiquement.

## <a name="explicit-compilation-versus-automatic-compilation"></a>Compilation explicite et compilation automatique

Les pages Web ASP.NET sont divisées en balises déclaratives et en code source. La partie de la balise déclarative comprend le code HTML, les contrôles Web et la syntaxe DataBinding. la partie code contient des gestionnaires d’événements écrits en Visual Basic C# ou du code. Le balisage et les parties de code sont généralement séparés en différents fichiers : `WebPage.aspx` contient le balisage déclaratif pendant que `WebPage.aspx.cs` héberge le code.

Prenons l’exemple d’une page ASP.NET nommée Clock. aspx qui contient un contrôle Label dont la propriété Text est définie sur la date et l’heure actuelles du chargement de la page. La partie de balisage déclarative (en `Clock.aspx`) contiendrait le balisage pour un contrôle Web d’étiquette-`<asp:Label runat="server" id="TimeLabel" />`, tandis que la partie de code (dans `Clock.aspx.cs`) aurait un `Page_Load` gestionnaire d’événements avec le code suivant :

[!code-csharp[Main](determining-what-files-need-to-be-deployed-cs/samples/sample1.cs)]

Pour que le moteur ASP.NET puisse traiter une demande pour cette page, la partie code de la page (fichier `WebPage.aspx.cs`) doit d’abord être compilée. Cette compilation peut se produire de manière explicite ou automatique.

Si la compilation se produit explicitement, le code source de l’application entière est compilé en un ou plusieurs assemblys (`.dll` fichiers) situés dans le répertoire de `Bin` de l’application. Si la compilation se produit automatiquement, l’assembly généré automatiquement résultant est, par défaut, placé dans le dossier `Temporary ASP.NET` Files, qui se trouve dans `%WINDOWS%\Microsoft.NET\Framework\` *&lt;version&gt;* , bien que cet emplacement soit configurable via l' [élément`<compilation>`](https://msdn.microsoft.com/library/s10awwz0.aspx) de `Web.config`. Avec la compilation explicite, vous devez prendre une mesure pour compiler le code de l’application ASP.NET dans un assembly, et cette étape se produit avant le déploiement. Avec la compilation automatique, le processus de compilation se produit sur le serveur Web lors du premier accès à la ressource.

Quel que soit le modèle de compilation que vous utilisez, la partie du balisage de toutes les pages ASP.NET (fichiers `WebPage.aspx`) doit être copiée dans l’environnement de production. Avec la compilation explicite, vous devez copier les assemblys dans le dossier `Bin`, mais vous n’avez pas besoin de copier les parties de code des pages ASP.NET (les fichiers `WebPage.aspx.cs`). Avec la compilation automatique, vous devez copier les fichiers de partie de code afin que le code soit présent et puisse être compilé automatiquement lorsque la page est visitée. La partie du balisage de chaque page Web ASP.NET comprend une directive `@Page` avec des attributs qui indiquent si le code associé à la page a déjà été compilé explicitement ou s’il doit être compilé automatiquement. Par conséquent, l’environnement de production peut fonctionner avec un modèle de compilation en toute transparence et vous n’avez pas besoin d’appliquer des paramètres de configuration spéciaux pour indiquer que la compilation explicite ou automatique est utilisée.

Le tableau 1 résume les différents fichiers à déployer lors de l’utilisation de la compilation explicite par rapport à la compilation automatique. Notez que, quel que soit le modèle de compilation utilisé, vous devez toujours déployer les assemblys dans le dossier `Bin`, si ce dossier existe. Le dossier `Bin` contient les assemblys spécifiques à l’application Web, qui incluent le code source compilé lors de l’utilisation du modèle de compilation explicite. Le répertoire `Bin` contient également les assemblys d’autres projets et les assemblys Open source ou tiers que vous utilisez, et ceux-ci doivent se trouver sur le serveur de production. Par conséquent, en règle générale, copiez le dossier `Bin` jusqu’à la production lors du déploiement. (Si vous utilisez le modèle de compilation automatique et que vous n’utilisez pas d’assembly externe, vous n’avez pas de répertoire `Bin`, ce qui est OK !)

| **Modèle de compilation** | **Déployer le fichier de partie de balisage ?** | **Déployer le fichier de code source ?** | **Déployer des assemblys dans `Bin` Directory ?** |
| --- | --- | --- | --- |
| Compilation explicite | Oui | Non | Oui |
| Compilation automatique | Oui | Oui | Oui (s’il existe) |

**Tableau 1 :** Les fichiers que vous déployez dépendent du modèle de compilation utilisé.

## <a name="taking-a-trip-down-memory-lane"></a>Passage en aval de la mémoire

L’approche de compilation utilisée dépend, en partie, du mode de gestion de l’application ASP.NET dans Visual Studio. Sa. Dans l’année 2000, il existe quatre versions différentes de Visual Studio : Visual Studio .NET 2002, Visual Studio .NET 2003, Visual Studio 2005 et Visual Studio 2008. Les applications ASP.NET gérées par Visual Studio .NET 2002 et 2003 utilisent le *modèle de projet d’application Web*. Les principales fonctionnalités du modèle de projet d’application Web sont les suivantes :

- Les fichiers qui composent le projet sont définis dans un fichier projet unique. Les fichiers non définis dans le fichier projet ne sont pas considérés comme faisant partie de l’application Web par Visual Studio.
- Utilise la compilation explicite. La génération du projet compile les fichiers de code du projet dans un assembly unique qui est placé dans le dossier `Bin`.

Lorsque Microsoft a publié Visual Studio 2005, il a supprimé la prise en charge du modèle de projet d’application Web et l’a remplacé par le modèle de projet de site Web. Le modèle de projet de site Web se différencie du modèle de projet d’application Web des manières suivantes :

- Au lieu d’avoir un seul fichier projet qui épele les fichiers du projet, le système de fichiers est utilisé à la place. En bref, tous les fichiers du dossier d’application Web (ou des sous-dossiers) sont considérés comme faisant partie du projet.
- La génération d’un projet dans Visual Studio ne crée pas d’assembly dans le répertoire `Bin`. Au lieu de cela, la génération d’un projet de site Web signale toutes les erreurs au moment de la compilation.
- Prise en charge de la compilation automatique. Les projets de site Web sont généralement déployés en copiant le balisage et le code source dans l’environnement de production, bien que le code puisse être précompilé (compilation explicite).

Microsoft a réactivé le modèle de projet d’application Web lors de la publication de Visual Studio 2005 Service Pack 1. Toutefois, Visual Web Developer n’a continué qu’à prendre en charge le modèle de projet de site Web. La bonne nouvelle, c’est que cette limitation a été supprimée avec Visual Web Developer 2008 Service Pack 1. Aujourd’hui, vous pouvez créer des applications ASP.NET dans Visual Studio (et Visual Web Developer) à l’aide du modèle de projet d’application Web ou du modèle de projet de site Web. Les deux modèles ont leurs avantages et inconvénients. Reportez-vous à [Introduction aux projets d’application Web : comparaison des projets de site Web et des projets d’application Web](https://msdn.microsoft.com/library/aa730880.aspx#wapp_topic5) pour comparer les deux modèles et aider à déterminer le modèle de projet le mieux adapté à votre situation.

## <a name="exploring-the-sample-web-application"></a>Exploration de l’exemple d’application Web

Le téléchargement de ce didacticiel comprend une application ASP.NET appelée révisions de livres. Le site Web imite un site Web de loisir que quelqu’un peut créer pour partager ses revues de livres avec la communauté en ligne. Cette application Web ASP.NET est très simple et comprend les ressources suivantes :

- `Web.config`, le fichier de configuration de l’application.
- Page maître (`Site.master`).
- Sept pages ASP.NET différentes : 

    - ~`/Default.aspx`-la page d’accueil du site.
    - ~`/About.aspx`-page « à propos du site ».
    - ~`/Fiction/Default.aspx`-une page répertoriant les livres fictifs qui ont été examinés. 

        - ~`/Fiction/Blaze.aspx` : un examen de Richard *Bachman.*
    - ~/`Tech/Default.aspx`-une page répertoriant les livres technologiques qui ont été examinés. 

        - ~/`Tech/CYOW.aspx`-révision de *la création de votre propre site Web*.
        - ~/`Tech/TYASP35.aspx`-une revue de *ASP.NET 3,5 en 24 heures*.
- Trois fichiers CSS différents dans le dossier styles.
- Quatre fichiers image : un logo ASP.NET et des images des couvertures des trois manuels consultés, tous situés dans le dossier `Images`.
- Un fichier `Web.sitemap`, qui définit le plan du site et est utilisé pour afficher les menus dans les pages `Default.aspx` dans le répertoire racine, ainsi que les dossiers `Fiction` et `Tech`.
- Fichier de classe nommé `BasePage.cs` qui définit une classe de `Page` de base. Cette classe étend les fonctionnalités de la classe `Page` en définissant automatiquement la propriété `Title` en fonction de la position de la page dans le plan de site. En résumé, toute classe code-behind ASP.NET qui étend `BasePage` (au lieu de `System.Web.UI.Page`) aura son titre défini sur une valeur en fonction de sa position dans le plan de site. Par exemple, lorsque vous affichez la page ~/`Tech/CYOW.aspx`, le titre est défini sur « personnel : technologie : créer votre propre site Web ».

La figure 1 montre une capture d’écran du site Web de révisions de livres quand vous l’affichez dans un navigateur. Ici, vous voyez la page ~/`Tech/TYASP35.aspx`, qui examine le livre *enseigner vous-même ASP.NET 3,5 en 24 heures*. Le plan de navigation qui s’étend sur le haut de la page et le menu dans la colonne de gauche sont basés sur la structure de plan de site définie dans `Web.sitemap`. L’image dans le coin supérieur droit est l’une des images de couverture de livre situées dans le dossier `Images`. L’apparence du site Web est définie par le biais de règles de feuille de style en cascade, épelées par les fichiers CSS du dossier styles, tandis que la mise en page de la page principale est définie dans la page maître `Site.master`.

[![le site Web de révisions de livres offre des avis sur un assortiment de titres](determining-what-files-need-to-be-deployed-cs/_static/image2.png)](determining-what-files-need-to-be-deployed-cs/_static/image1.png)

**Figure 1 :** Le site Web de révisions de livres offre des avis sur un assortiment de titres ([cliquez pour afficher l’image en taille réelle](determining-what-files-need-to-be-deployed-cs/_static/image3.png))

Cette application n’utilise pas de base de données ; Chaque revue est implémentée sous la forme d’une page Web distincte dans l’application. Ce didacticiel (et les prochains didacticiels) vous guide dans le déploiement d’une application Web qui n’a pas de base de données. Toutefois, dans un prochain didacticiel, nous améliorerons cette application pour stocker les revues, les commentaires des lecteurs et d’autres informations dans une base de données, et nous explorerons les étapes à effectuer pour déployer correctement une application Web pilotée par les données.

> [!NOTE]
> Ces didacticiels portent sur l’hébergement d’applications ASP.NET avec un fournisseur d’hébergement Web et n’explorent pas les sujets connexes comme ASP. Système de plan de site NET ou à l’aide d’une classe de `Page` de base. Pour plus d’informations sur ces technologies et pour plus d’informations sur les autres rubriques traitées dans ce didacticiel, reportez-vous à la section de lecture supplémentaire à la fin de chaque didacticiel.

Ce didacticiel contient deux copies de l’application Web, chacune étant implémentée en tant que type de projet Visual Studio différent : BookReviewsWAP, un projet d’application Web et BookReviewsWSP, un projet de site Web. Les deux projets ont été créés avec Visual Web Developer 2008 SP1 et utilisent ASP.NET 3,5 SP1. Pour travailler avec ces projets, commencez par décompresser le contenu sur votre bureau. Pour ouvrir le projet d’application Web (BookReviewsWAP), accédez au dossier BookReviewsWAP, puis double-cliquez sur le fichier solution, `BookReviewsWAP.sln`. Pour ouvrir le projet de site Web (BookReviewsWSP), lancez Visual Studio, puis, dans le menu fichier, choisissez l’option ouvrir le site Web, accédez au dossier `BookReviewsWSP` sur votre bureau, puis cliquez sur OK.

Les deux sections restantes de ce didacticiel présentent les fichiers que vous devrez copier dans l’environnement de production lors du déploiement de l’application. Les deux didacticiels suivants : *[déploiement de votre site à l’aide de FTP](deploying-your-site-using-an-ftp-client-cs.md)* et *[déploiement de votre site à l’aide de Visual Studio](deploying-your-site-using-visual-studio-cs.md)* , montrent les différentes façons de copier ces fichiers sur un fournisseur d’hébergement Web.

## <a name="determining-the-files-to-deploy-for-the-web-application-project"></a>Détermination des fichiers à déployer pour le projet d’application Web

Le modèle de projet d’application Web utilise la compilation explicite-le code source du projet est compilé dans un assembly unique chaque fois que vous générez l’application. Cette compilation comprend les fichiers code-behind des pages ASP.NET (~/`Default.aspx.cs`, ~/`About.aspx.cs`, etc.), ainsi que la classe `BasePage.cs`. L’assembly résultant est nommé BookReviewsWAP. dll et se trouve dans le répertoire `Bin` de l’application.

La figure 2 montre les fichiers qui composent le projet d’application Web de révisions de livres.

[![la Explorateur de solutions répertorie les fichiers qui composent le projet d’application Web](determining-what-files-need-to-be-deployed-cs/_static/image5.png)](determining-what-files-need-to-be-deployed-cs/_static/image4.png)

**Figure 2**: le Explorateur de solutions répertorie les fichiers qui composent le projet d’application Web

Pour déployer une application ASP.NET développée à l’aide du modèle de projet d’application Web, commencez par créer l’application afin de compiler explicitement le code source le plus récent dans un assembly. Ensuite, copiez les fichiers suivants dans l’environnement de production :

- Les fichiers qui contiennent le balisage déclaratif pour chaque page ASP.NET, par exemple ~/`Default.aspx`, ~/`About.aspx`, etc. Copiez également le balisage déclaratif pour les pages maîtres et les contrôles utilisateur.
- Les assemblys (fichiers`.dll`) dans le dossier `Bin`. Vous n’avez pas besoin de copier les fichiers de base de données du programme (`.pdb`) ou les fichiers XML que vous pouvez trouver dans le répertoire `Bin`.

Vous n’avez pas besoin de copier les fichiers de code source des pages ASP.NET vers l’environnement de production, ni de copier le fichier de classe `BasePage.cs`.

> [!NOTE]
> Comme le montre la figure 2, la classe `BasePage` est implémentée sous la forme d’un fichier de classe dans le projet, placé dans le dossier nommé `HelperClasses`. Quand le projet est compilé, le code dans le fichier `BasePage.cs` est compilé avec les classes code-behind des pages ASP.NET dans l’assembly unique, `BookReviewsWAP.dll.` ASP.NET possède un dossier spécial nommé `App_Code` conçu pour contenir des fichiers de classe pour les projets de site Web. Le code dans le dossier `App_Code` est automatiquement compilé et ne doit donc pas être utilisé avec des projets d’application Web. Au lieu de cela, vous devez placer les fichiers de classe de votre application dans un dossier normal nommé `HelperClasses`ou `Classes`, ou un nom similaire. Vous pouvez également placer des fichiers de classe dans un projet de bibliothèque de classes distinct.

Outre la copie des fichiers de balisage liés à ASP.NET et de l’assembly dans le dossier `Bin`, vous devez également copier les fichiers de prise en charge côté client (les images et les fichiers CSS), ainsi que les autres fichiers de prise en charge côté serveur, `Web.config` et `Web.sitemap`. Ces fichiers de prise en charge côté client et côté serveur doivent être copiés dans l’environnement de production, que vous utilisiez la compilation explicite ou automatique.

## <a name="determining-the-files-to-deploy-for-the-web-site-project-files"></a>Détermination des fichiers à déployer pour les fichiers de projet de site Web

Le modèle de projet de site Web prend en charge la compilation automatique, une fonctionnalité qui n’est pas disponible lors de l’utilisation du modèle de projet d’application Web. Avec la compilation explicite, vous devez compiler le code source de votre projet dans un assembly et copier cet assembly dans l’environnement de production. En revanche, avec la compilation automatique, vous copiez simplement le code source dans l’environnement de production et il est compilé par le runtime à la demande, selon les besoins.

L’option de menu Générer dans Visual Studio est présente dans les projets d’application Web et les projets de site Web. La génération d’un projet d’application Web compile le code source du projet dans un assembly unique situé dans le répertoire `Bin` ; la génération d’un projet de site Web vérifie toute erreur au moment de la compilation, mais ne crée pas d’assembly. Pour déployer une application ASP.NET développée à l’aide du modèle de projet de site Web, il vous suffit de copier les fichiers appropriés dans l’environnement de production, mais je vous encourage à créer d’abord le projet pour vous assurer qu’il n’y a pas d’erreurs au moment de la compilation.

La figure 3 montre les fichiers qui composent le projet de site Web de révisions de livres.

 [![la Explorateur de solutions répertorie les fichiers qui composent le projet de site Web](determining-what-files-need-to-be-deployed-cs/_static/image7.png)](determining-what-files-need-to-be-deployed-cs/_static/image6.png) 

**Figure 3**: le Explorateur de solutions répertorie les fichiers qui composent le projet de site Web

Le déploiement d’un projet de site Web implique la copie de tous les fichiers liés à ASP.NET dans l’environnement de production, y compris les pages de balisage pour les pages ASP.NET, les pages maîtres et les contrôles utilisateur, ainsi que leurs fichiers de code. Vous devez également copier tous les fichiers de classe, tels que BasePage.cs. Notez que le fichier `BasePage.cs` se trouve dans le dossier `App_Code`, qui est un dossier ASP.NET spécial utilisé dans les projets de site Web pour les fichiers de classe. Le dossier spécial doit également être créé en production, car les fichiers de classe dans le dossier `App_Code` de l’environnement de développement doivent être copiés dans le dossier `App_Code` en production.

En plus de copier le balisage ASP.NET et les fichiers de code source, vous devez également copier les fichiers de prise en charge côté client (les images et les fichiers CSS), ainsi que les autres fichiers de prise en charge côté serveur, `Web.config` et `Web.sitemap`.

> [!NOTE]
> Les projets de site Web peuvent également utiliser la compilation explicite. Un futur didacticiel vous apprendra à compiler explicitement un projet de site Web.

## <a name="summary"></a>Récapitulatif

Le déploiement d’une application ASP.NET consiste à copier les fichiers nécessaires de l’environnement de développement vers l’environnement de production. L’ensemble précis des fichiers qui doivent être synchronisés varie selon que le code de l’application ASP.NET est explicitement ou automatiquement compilé. La stratégie de compilation employée est influencée par le fait que Visual Studio est configuré pour gérer l’application ASP.NET à l’aide du modèle de projet d’application Web ou du modèle de projet de site Web.

Le modèle de projet d’application Web utilise la compilation explicite et compile le code du projet dans un assembly unique dans le dossier `Bin`. Lors du déploiement de l’application, la partie du balisage des pages ASP.NET et le contenu du dossier `Bin` doivent être transmis à l’environnement de production. code source de l’application : les fichiers de code et les classes code-behind, par exemple, n’ont pas besoin d’être copiés dans l’environnement de production.

Le modèle de projet de site Web utilise la compilation automatique par défaut, bien qu’il soit possible de compiler explicitement un projet de site Web, comme nous le verrons dans d’autres didacticiels. Le déploiement d’une application ASP.NET qui utilise la compilation automatique nécessite que la partie du balisage *et* le code source doivent être copiés dans l’environnement de production. Le code est automatiquement compilé sur l’environnement de production lorsqu’il est demandé pour la première fois.

Maintenant que nous avons examiné les fichiers qui doivent être synchronisés entre les environnements de développement et de production, nous sommes prêts à déployer l’application de révisions de livres sur un fournisseur d’hébergement Web.

Bonne programmation !

### <a name="further-reading"></a>informations supplémentaires

Pour plus d’informations sur les sujets abordés dans ce didacticiel, reportez-vous aux ressources suivantes :

- [Présentation de la compilation ASP.NET](https://msdn.microsoft.com/library/ms178466.aspx)
- [Contrôles utilisateur ASP.NET](https://msdn.microsoft.com/library/y6wb1a0e.aspx)
- [Examen d’ASP. Navigation sur le site NET](http://aspnet.4guysfromrolla.com/articles/111605-1.aspx)
- [Présentation des projets d’application Web](https://msdn.microsoft.com/library/aa730880.aspx)
- [Didacticiels sur les pages maîtres](../master-pages/creating-a-site-wide-layout-using-master-pages-cs.md)
- [Partage de code entre pages](https://quickstarts.asp.net/QuickStartv20/aspnet/doc/pages/code.aspx)
- [Utilisation d’une classe de base personnalisée pour les classes code-behind de vos pages ASP.NET](http://aspnet.4guysfromrolla.com/articles/041305-1.aspx)
- [Système de projet de site Web de Visual Studio 2005 : qu’est-ce que c’est ?](https://weblogs.asp.net/scottgu/archive/2005/08/21/423201.aspx)
- [Procédure pas à pas : conversion d’un projet de site Web en projet d’application Web dans Visual Studio](https://msdn.microsoft.com/library/aa983476.aspx)

> [!div class="step-by-step"]
> [Précédent](asp-net-hosting-options-cs.md)
> [Suivant](deploying-your-site-using-an-ftp-client-cs.md)
