---
uid: web-forms/overview/older-versions-getting-started/deploying-web-site-projects/precompiling-your-website-cs
title: Précompilation de votre siteC#Web () | Microsoft Docs
author: rick-anderson
description: 'Visual Studio offre aux développeurs ASP.NET deux types de projets : les projets d’application Web (WAP) et les projets de site Web (WSP). L’une des principales différences betwe...'
ms.author: riande
ms.date: 06/09/2009
ms.assetid: ecd5a4de-beb7-4d1d-bbbb-e31003633267
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deploying-web-site-projects/precompiling-your-website-cs
msc.type: authoredcontent
ms.openlocfilehash: cb42398f44f3cd16ab83a9976e7b34f132384456
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78573506"
---
# <a name="precompiling-your-website-c"></a>Précompilation de votre site web (C#)

par [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Télécharger le code](https://download.microsoft.com/download/1/0/C/10CC829F-A808-4302-97D3-59989B8F9C01/ASPNET_Hosting_Tutorial_15_CS.zip) ou [Télécharger le PDF](https://download.microsoft.com/download/5/C/5/5C57DB8C-5DEA-4B3A-92CA-4405544D313B/aspnet_tutorial15_Precompiling_cs.pdf)

> Visual Studio offre aux développeurs ASP.NET deux types de projets : les projets d’application Web (WAP) et les projets de site Web (WSP). L’une des principales différences entre les deux types de projets est que les WAP doivent avoir le code compilé explicitement avant le déploiement, tandis que le code d’un WSP peut être automatiquement compilé sur le serveur Web. Toutefois, il est possible de précompiler un WSP avant son déploiement. Ce didacticiel explore les avantages de la précompilation et montre comment précompiler un site Web dans Visual Studio et à partir de la ligne de commande.

## <a name="introduction"></a>Introduction

Visual Studio offre aux développeurs ASP.NET deux types de projets différents : les projets d’application Web (WAP) et les projets de site Web (WSP). L’une des principales différences entre ces types de projets est que les WAP requièrent une *compilation explicite* , tandis que wsp utilisent la *compilation automatique*, par défaut. Avec les WAP, vous compilez le code de l’application Web dans un assembly unique, qui est créé dans le dossier `Bin` du site Web. Le déploiement consiste à copier le contenu du balisage (fichiers `.aspx.ascx`et `.master`) dans le projet, ainsi que l’assembly dans le dossier `Bin` ; les fichiers de classe code-behind n’ont pas besoin d’être déployés. En revanche, vous déployez wsp en copiant les pages de balisage et leurs classes code-behind correspondantes dans l’environnement de production. Les classes code-behind sont compilées à la demande sur le serveur Web.

> [!NOTE]
> Reportez-vous à la section « compilation explicite et compilation automatique » dans le [didacticiel *détermination des fichiers qui doivent être déployés* ](determining-what-files-need-to-be-deployed-cs.md) pour plus d’informations sur les différences entre les modèles de projet, la compilation explicite et automatique et la manière dont le modèle de compilation affecte le déploiement.

L’option de compilation automatique est simple à utiliser. Il n’y a pas d’étape de compilation explicite et seuls les fichiers qui ont été modifiés doivent être déployés, tandis que la compilation explicite nécessite le déploiement des pages de balisage modifiées et de l’assembly juste-à-compilé. Toutefois, le déploiement automatique présente deux inconvénients potentiels :

- Étant donné que les pages doivent être automatiquement compilées lorsqu’elles sont visitées pour la première fois, il peut y avoir un délai bref mais notable quand une page ASP.NET est demandée pour la première fois après son déploiement.
- La compilation automatique requiert que le balisage déclaratif et le code source soient présents sur le serveur Web. Il peut s’agir d’une option non attrayante si vous envisagez de vendre l’application Web aux clients qui l’installeront sur leurs serveurs Web.

Si l’une des deux défaillances ci-dessus est l’analyseur de transactions, vous pouvez basculer vers le modèle WAP ou *précompiler* le wsp avant le déploiement. Ce didacticiel examine les options de précompilation les mieux adaptées à un site Web hébergé et décrit le processus de précompilation et le déploiement d’un site Web précompilé.

## <a name="an-overview-of-aspnet-code-generation-and-compilation"></a>Vue d’ensemble de la génération et de la compilation de code ASP.NET

Avant d’examiner les options de précompilation disponibles, voyons tout d’abord la génération et la compilation du code qui se produit lorsqu’une page ASP.NET est demandée pour la première fois depuis sa création ou sa dernière mise à jour. Comme vous le savez, les pages ASP.NET sont composées de deux parties : le balisage déclaratif dans le fichier `.aspx` ; et une partie de code source, généralement dans un fichier de classe code-behind distinct (`.aspx.cs`). Les étapes effectuées par le runtime quand une page ASP.NET est demandée dépendent du modèle de compilation de l’application.

Avec les WAP, le code source de la page doit être compilé explicitement dans un assembly unique avant d’être déployé. Pendant le déploiement, cet assembly et les différentes pages de balisage sont copiés dans l’environnement de production. Lorsqu’une requête arrive au serveur Web pour une page ASP.NET, le runtime crée une instance de la classe code-behind de la page et appelle sa méthode `ProcessRequest`, qui démarre le cycle de vie de la page et, finalement, génère le contenu de la page, qui est retourné au demandeur. Le runtime peut fonctionner avec la classe code-behind de la page ASP.NET, car la classe code-behind a déjà été compilée dans un assembly avant son déploiement.

Avec wsp et la compilation automatique, il n’y a pas d’étape de compilation explicite avant le déploiement. Au lieu de cela, le déploiement consiste à copier le contenu déclaratif et le contenu du code source dans l’environnement de production. Quand une requête arrive au serveur Web pour une page ASP.NET pour la première fois depuis la création ou la dernière mise à jour de la page, le runtime doit d’abord compiler la classe code-behind dans un assembly. Cet assembly compilé est enregistré dans le dossier `%WINDIR%\Microsoft.NET\Framework\v2.0.50727\Temporary ASP.NET Files`, même si l’emplacement de ce dossier peut être personnalisé à l’aide de l’élément `<compilation tempDirectory="" />` de `<system.web>`, généralement dans `Web.config`. Étant donné que l’assembly est enregistré sur le disque, il n’a pas besoin d’être recompilé lors des demandes suivantes sur la même page.

> [!NOTE]
> Comme vous pouvez vous y attendre, il y a un léger retard lors de la demande d’une page pour la première fois (ou pour la première fois, car elle a été modifiée) dans un site qui utilise la compilation automatique, car il faut un instant au serveur pour compiler le code de la page et enregistrer l’assembly résultant dans libérer.

En résumé, avec la compilation explicite, vous devez compiler le code source du site Web avant le déploiement, ce qui évite au runtime d’avoir à effectuer cette étape. Avec la compilation automatique, le Runtime gère la compilation du code source des pages, mais avec un léger coût d’initialisation pour la première visite de la page depuis sa création ou sa dernière mise à jour.

Mais qu’en est-il de la partie déclarative des pages ASP.NET (le fichier `.aspx`) ? Il est évident qu’il existe une relation entre les fichiers `.aspx` et le code dans leurs classes code-behind, car les contrôles Web définis dans le balisage déclaratif sont accessibles dans le code. Il est également évident que le contenu des fichiers `.aspx` influence le balisage généré par la page. Comment le runtime fonctionne-t-il avec la syntaxe de texte, HTML et de contrôle Web définie dans le fichier `.aspx` pour générer le contenu rendu de la page demandée ?

Je ne veux pas être trop Sidetracked sur les détails d’implémentation de bas niveau, qui varient entre les WAP et les wsp, mais en bref, le runtime génère automatiquement un fichier de classe qui contient les différents contrôles Web comme des membres et des méthodes protégés. Ce fichier généré est implémenté en tant que *classe partielle* de la classe code-behind correspondante. (Les[classes partielles](http://www.dotnet-guide.com/partialclasses.html) permettent de répartir le contenu d’une classe unique sur plusieurs fichiers.) Par conséquent, la classe code-behind est définie à deux emplacements : dans le fichier `.aspx.cs` que vous créez et dans cette classe générée automatiquement créée par le Runtime. Cette classe générée automatiquement est stockée dans le dossier `%WINDIR%\Microsoft.NET\Framework\v2.0.50727\Temporary ASP.NET Files`.

L’important est ici que, pour qu’une page ASP.NET soit restituée par le runtime, ses parties de code source et déclaratives doivent être compilées dans un assembly. Avec les WAP, le code source est compilé explicitement dans un assembly avant le déploiement, mais le balisage déclaratif doit encore être converti en code et compilé par le runtime sur le serveur Web. Avec WSP à l’aide de la compilation automatique, le code source et le balisage déclaratif doivent être compilés par le serveur Web.

Il est possible d’utiliser la compilation explicite avec le modèle WSP. Vous pouvez compiler explicitement la partie du code source, comme avec le modèle WAP. En outre, vous pouvez également compiler le balisage déclaratif.

## <a name="precompilation-options"></a>Options de précompilation

Le .NET Framework est fourni avec un [outil de compilation ASP.net (`aspnet_compiler.exe`)](https://msdn.microsoft.com/library/ms229863.aspx) qui vous permet de compiler le code source (et même le contenu) d’une application ASP.net générée à l’aide du modèle wsp. Cet outil a été publié avec la version de .NET Framework 2,0 et se trouve dans le dossier `%WINDIR%\Microsoft.NET\Framework\v2.0.50727` ; elle peut être utilisée à partir de la ligne de commande ou lancée à partir de Visual Studio via l’option publier le site Web du menu Générer.

L’outil de compilation fournit deux formes générales de compilation : précompilation sur place et précompilation pour le déploiement. Avec la précompilation sur place, vous exécutez l’outil `aspnet_compiler.exe` à partir de la ligne de commande et spécifiez le chemin d’accès au répertoire virtuel ou au chemin d’accès physique d’un site Web qui se trouve sur votre ordinateur. L’outil de compilation compile ensuite chaque page ASP.NET dans le projet, en stockant la version compilée dans le dossier `%WINDIR%\Microsoft.NET\Framework\v2.0.50727\Temporary ASP.NET Files` comme si les pages avaient été visitées pour la première fois à partir d’un navigateur. La précompilation sur place peut accélérer la première requête effectuée sur des pages ASP.NET récemment déployées sur votre site, car elle évite au runtime d’avoir à effectuer cette étape. Toutefois, la précompilation sur place n’est pas utile pour la majorité des sites Web hébergés, car elle nécessite que vous puissiez exécuter des programmes à partir de la ligne de commande du serveur Web. Dans les environnements d’hébergement partagés, ce niveau d’accès n’est pas autorisé.

> [!NOTE]
> Pour plus d’informations sur la précompilation sur place, consultez [Comment : précompiler des sites Web ASP.net](https://msdn.microsoft.com/library/ms227972.aspx) et [précompilation dans ASP.NET 2,0](http://www.odetocode.com/Articles/417.aspx).

Au lieu de compiler les pages du site Web dans le dossier `Temporary ASP.NET Files`, la précompilation du déploiement compile les pages dans un répertoire de votre choix et dans un format qui peut être déployé dans l’environnement de production.

Il existe deux types de précompilation pour le déploiement que nous explorons dans ce didacticiel : précompilation avec une interface utilisateur pouvant être mise à jour, et précompilation avec une interface utilisateur ne pouvant pas être mise à jour. La précompilation avec une interface utilisateur pouvant être mise à jour laisse le balisage déclaratif dans les fichiers `.aspx`, `.ascx`et `.master`, ce qui permet à un développeur d’afficher et, le cas échéant, de modifier le balisage déclaratif sur le serveur de production. La précompilation avec une interface utilisateur qui ne peut pas être mise à jour génère `.aspx` pages qui sont vides et supprime les fichiers `.ascx` et `.master`, masquant ainsi le balisage déclaratif et empêchant un développeur de le modifier à partir de l’environnement de production.

### <a name="precompiling-for-deployment-with-an-updatable-user-interface"></a>Précompilation pour le déploiement avec une interface utilisateur pouvant être mise à jour

La meilleure façon de comprendre la précompilation pour le déploiement consiste à voir un exemple en action. Commençons par précompiler les évaluations du livre WSP pour le déploiement à l’aide d’une interface utilisateur pouvant être mise à jour. L’outil de compilation ASP.NET peut être appelé à partir du menu générer de Visual Studio ou à partir de la ligne de commande. Cette section examine l’utilisation de l’outil dans Visual Studio. la section « précompilation à partir de la ligne de commande » examine l’exécution de l’outil compilateur à partir de la ligne de commande.

Ouvrez l’évaluation de livre WSP dans Visual Studio, accédez au menu Générer, puis sélectionnez l’option de menu publier le site Web. La boîte de dialogue publier le site Web (voir la **figure 1**) s’ouvre, dans laquelle vous pouvez spécifier l’emplacement cible, que l’interface utilisateur du site précompilé puisse être mise à jour et d’autres options de l’outil compilateur. L’emplacement cible peut être un serveur Web distant ou un serveur FTP, mais pour l’instant, choisissez un dossier sur le disque dur de votre ordinateur. Étant donné que nous souhaitons précompiler le site avec une interface utilisateur pouvant être mise à jour, laissez la case à cocher « Autoriser ce site précompilé à être mis à jour » activée, puis cliquez sur OK.

[![](precompiling-your-website-cs/_static/image2.png)](precompiling-your-website-cs/_static/image1.png)

**Figure 1**: l’outil de compilation ASP.net va précompiler votre site Web à l’emplacement cible spécifié  
 ([Cliquez pour afficher l’image en taille réelle](precompiling-your-website-cs/_static/image3.png))

> [!NOTE]
> L’option publier le site Web dans le menu Générer n’est pas disponible dans Visual Web Developer. Si vous utilisez Visual Web Developer, vous devrez utiliser la version de ligne de commande de l’outil de compilation ASP.NET, qui est abordé dans la section « précompilation à partir de la ligne de commande ».

Après avoir précompilé le site Web, accédez à l’emplacement cible que vous avez entré dans la boîte de dialogue publier le site Web. Prenez un moment pour comparer le contenu de ce dossier avec le contenu de votre site Web. La **figure 2** illustre le dossier du site Web de révisions de livres. Notez qu’il contient à la fois des fichiers `.aspx` et `.aspx.cs`. Notez également que le répertoire `Bin` comprend un seul fichier, `Elmah.dll`, que nous avons ajouté dans le [didacticiel précédent](logging-error-details-with-elmah-cs.md) .

[![](precompiling-your-website-cs/_static/image5.png)](precompiling-your-website-cs/_static/image4.png)

**Figure 2**: le répertoire du projet contient des fichiers `.aspx` et `.aspx.cs` ; le dossier `Bin` contient uniquement `Elmah.dll`  
 ([Cliquez pour afficher l’image en taille réelle](precompiling-your-website-cs/_static/image6.png))

La **figure 3** montre le dossier d’emplacement cible dont le contenu a été créé par l’outil de compilation ASP.net. Ce dossier ne contient pas de fichiers code-behind. En outre, le répertoire `Bin` de ce dossier comprend plusieurs assemblys et deux fichiers `.compiled` en plus de l’assembly `Elmah.dll`.

[![](precompiling-your-website-cs/_static/image8.png)](precompiling-your-website-cs/_static/image7.png)

**Figure 3**: le dossier emplacement cible contient les fichiers pour le déploiement  
 ([Cliquez pour afficher l’image en taille réelle](precompiling-your-website-cs/_static/image9.png))

Contrairement à la compilation explicite dans les WAP, la précompilation pour le processus de déploiement ne crée pas un assembly pour l’ensemble du site. Au lieu de cela, il regroupe plusieurs pages dans chaque assembly. Elle compile également le fichier `Global.asax` (le cas échéant) dans son propre assembly, ainsi que toutes les classes dans le dossier `App_Code`. Les fichiers qui contiennent le balisage déclaratif pour les pages Web ASP.NET, les contrôles utilisateur et les pages maîtres (respectivement`.aspx`, `.ascx`et les fichiers `.master`) sont copiés tels quels dans le répertoire de l’emplacement cible. De même, le fichier `Web.config` est copié directement, avec tous les fichiers statiques, tels que les images, les classes CSS et les fichiers PDF. Pour obtenir une description plus formelle de la manière dont l’outil de compilation gère différents types de fichiers, consultez [gestion des fichiers pendant la précompilation ASP.net](https://msdn.microsoft.com/library/e22s60h9.aspx).

> [!NOTE]
> Vous pouvez faire en sorte que l’outil de compilation crée un assembly par page ASP.NET, contrôle utilisateur ou page maître en activant la case à cocher « utilisation de noms fixes et d’assemblys de page unique » dans la boîte de dialogue publier le site Web. Le fait de compiler chaque page ASP.NET dans son propre assembly permet un contrôle plus précis du déploiement. Par exemple, si vous avez mis à jour une seule page Web ASP.NET et que vous avez besoin de déployer cette modification, vous devez uniquement déployer le fichier `.aspx` de cette page et l’assembly associé à l’environnement de production. Pour plus d’informations, consultez [Comment : générer des noms fixes avec l’outil de Compilation ASP.net](https://msdn.microsoft.com/library/ms228040.aspx) .

Le répertoire emplacement cible contient également un fichier qui ne fait pas partie du projet Web précompilé, à savoir `PrecompiledApp.config`. Ce fichier informe le runtime ASP.NET que l’application a été précompilée et si elle a été précompilée avec une interface utilisateur pouvant être mise à jour ou midi.

Enfin, prenez un moment pour ouvrir l’un des fichiers de `.aspx` dans l’emplacement cible à l’aide de Visual Studio ou de l’éditeur de texte de votre choix. En cas de précompilation pour le déploiement avec une interface utilisateur pouvant être mise à jour, les pages ASP.NET dans le répertoire emplacement cible contiennent exactement le même balisage que les fichiers correspondants dans le site Web.

### <a name="precompiling-for-deployment-with-a-non-updatable-user-interface"></a>Précompilation pour le déploiement avec une interface utilisateur qui ne peut pas être mise à jour

L’outil compilateur ASP.NET peut également être utilisé pour précompiler un site en vue d’un déploiement avec une interface utilisateur qui ne peut pas être mise à jour. La précompilation du site avec une interface utilisateur qui ne peut pas être mise à jour fonctionne comme la précompilation avec une interface utilisateur pouvant être mise à jour, à la différence près que les pages ASP.NET, les contrôles utilisateur et les pages maîtres dans le répertoire cible sont supprimés de leur balisage. Pour précompiler un site Web en vue d’un déploiement avec une interface utilisateur qui ne peut pas être mise à jour, choisissez l’option publier le site Web dans le menu Générer, mais décochez l’option « autoriser ce site précompilé à être mis à jour » (voir **figure 4**).

[![](precompiling-your-website-cs/_static/image11.png)](precompiling-your-website-cs/_static/image10.png)

**Figure 4**: décocher l’option « autoriser ce site précompilé à être mis à jour » pour effectuer une précompilation avec une interface utilisateur qui ne peut pas être mise à jour  
 ([Cliquez pour afficher l’image en taille réelle](precompiling-your-website-cs/_static/image12.png))

La **figure 5** montre le dossier d’emplacement cible après la précompilation avec une interface utilisateur qui ne peut pas être mise à jour.

[![](precompiling-your-website-cs/_static/image14.png)](precompiling-your-website-cs/_static/image13.png)

**Figure 5**: dossier d’emplacement cible pour le déploiement avec une interface utilisateur qui ne peut pas être mise à jour  
 ([Cliquez pour afficher l’image en taille réelle](precompiling-your-website-cs/_static/image15.png))

Comparez la **figure 3** à la **figure 5**. Alors que les deux dossiers peuvent paraître identiques, Notez que le dossier d’interface utilisateur qui ne peut pas être mis à jour n’a pas de page maître, `Site.master`. Et, alors que la **figure 5** comprend les différentes pages ASP.net, si vous affichez le contenu de ces fichiers, vous verrez qu’ils ont été supprimés de leur balisage déclaratif et remplacés par le texte de l’espace réservé : « il s’agit d’un fichier de marqueur généré par l’outil de précompilation et ne doit pas être supprimé ! »

[![](precompiling-your-website-cs/_static/image17.png)](precompiling-your-website-cs/_static/image16.png)

**Figure 5**: le balisage déclaratif a été supprimé des pages ASP.net

Les dossiers `Bin` des **figures 3** et **5** diffèrent considérablement. En plus des assemblys, le dossier `Bin` de la **figure 5** comprend un fichier `.compiled` pour chaque page ASP.net, contrôle utilisateur et page maître.

La précompilation d’un site avec une interface utilisateur qui ne peut pas être mise à jour est utile dans les situations où vous ne souhaitez pas que le contenu des pages ASP.NET soit modifié par la personne ou la société qui installe ou gère le site Web dans l’environnement de production. Si vous créez une application Web ASP.NET que vous vendez aux clients pour l’installer sur leurs propres serveurs Web, vous souhaiterez peut-être vous assurer qu’ils ne modifient pas l’apparence de votre site en modifiant directement les pages de `.aspx` que vous leur fournissez. En précompilant votre site Web avec une interface utilisateur qui ne peut pas être mise à jour, vous livrez l’espace réservé `.aspx` pages dans le cadre de l’installation, ce qui empêche vos clients d’examiner ou de modifier leur contenu.

### <a name="precompiling-from-the-command-line"></a>Précompilation à partir de la ligne de commande

En arrière-plan, la boîte de dialogue publier le site Web de Visual Studio appelle l’outil de compilation ASP.NET (`aspnet_compiler.exe`) pour précompiler le site Web. Vous pouvez également appeler cet outil à partir de la ligne de commande. En fait, si vous utilisez Visual Web Developer, vous devrez exécuter l’outil compilateur à partir de la ligne de commande, car le menu générer de Visual Web Developer n’inclut pas l’option publier le site Web.

Pour utiliser l’outil compilateur à partir de la ligne de commande, commencez par déposer jusqu’à la ligne de commande et accédez au répertoire Framework, `%WINDIR%\Microsoft.NET\Framework\v2.0.50727`. Ensuite, entrez l’instruction suivante dans la ligne de commande :

`aspnet_compiler -p "physical_path_to_app" -v / -f -u "target_location_folder"`

La commande ci-dessus lance l’outil compilateur ASP.NET (`aspnet_compiler.exe`) et, via le commutateur `-p`, lui demande de précompiler le site Web associé au *chemin d’accès physique\_\_à l’application\_* . Cette valeur sera semblable à `C:\MySites\BookReviews`et doit être délimitée par des guillemets.

Le commutateur `-v` spécifie le répertoire virtuel du site. Si votre site est inscrit en tant que site Web par défaut dans la métabase IIS, vous pouvez omettre le commutateur `-p` et spécifier simplement le répertoire virtuel de l’application. Si vous utilisez le commutateur `-p`, la valeur qui se poursuit avec le commutateur `-v` indique la racine du site Web et est utilisée pour résoudre les références à la racine de l’application. Par exemple, si vous spécifiez une valeur de `-v /MySite`, les références de l’application à `~/path/file` seront résolues comme `~/MySite/path/file`. Étant donné que le site de révisions de livres se trouve dans le répertoire racine de ma société d’hébergement Web, j’ai utilisé le commutateur `-v /`.

Le commutateur `-f`, s’il est présent, indique à l’outil de compilation de remplacer l' *emplacement de\_cible\_* répertoire du dossier s’il existe déjà. Si vous omettez le commutateur `-f` et que le dossier emplacement cible existe déjà, l’outil de compilation se ferme avec l’erreur : «erreur ASPRUNTIME : le répertoire cible n’est pas vide. Supprimez-le manuellement ou choisissez une autre cible.»

Le commutateur `-u`, s’il est présent, indique à l’outil de créer une interface utilisateur pouvant être mise à jour. Omettez ce commutateur pour précompiler le site avec une interface utilisateur qui ne peut pas être mise à jour.

Enfin, l' *emplacement cible\_\_dossier* est le chemin d’accès physique au répertoire de l’emplacement cible. Cette valeur sera semblable à `C:\MySites\Output\BookReviews`et doit être délimitée par des guillemets.

## <a name="deploying-the-precompiled-website"></a>Déploiement du site Web précompilé

À ce stade, nous avons vu comment utiliser l’outil de compilation ASP.NET pour précompiler un site Web à l’aide des options d’interface utilisateur pouvant être mises à jour et non pouvant être mises à jour. Toutefois, nos exemples ont jusqu’à présent précompiler le site Web dans un dossier local, et non dans l’environnement de production. La bonne nouvelle, c’est que le déploiement du site Web précompilé est un simple et peut être effectué par le biais de Visual Studio ou par le biais d’un autre mécanisme de copie de fichiers, par exemple à partir d’un client FTP autonome.

La boîte de dialogue publier le site Web (illustrée dans la **figure 1**) possède une option emplacement cible, qui indique l’emplacement de copie des fichiers du site Web précompilés. Cet emplacement peut être un serveur Web distant ou un serveur FTP. L’entrée d’un serveur distant dans cette zone de texte permet de précompiler et de déployer le site Web sur le serveur spécifié en une seule étape. Vous pouvez également précompiler le site Web dans un dossier local, puis copier manuellement le contenu de ce dossier dans l’environnement de production via FTP ou une autre approche.

Le fait que le site Web précompilé soit déployé automatiquement via la boîte de dialogue publier le site Web de Visual Studio est utile pour les sites simples où il n’existe aucune différence de configuration entre les environnements de développement et de production. Toutefois, comme indiqué dans les [ *différences de configuration courantes entre* les didacticiels de développement et de production](common-configuration-differences-between-development-and-production-cs.md) , il n’est pas rare que ces différences existent. Par exemple, l’application Web de révisions de livres utilise une autre base de données dans l’environnement de production que dans l’environnement de développement. Lorsque Visual Studio publie le site Web sur un serveur distant, il copie aveuglément les informations du fichier de configuration dans l’environnement de développement.

Pour les sites présentant des différences de configuration entre les environnements de développement et de production, il peut être préférable de précompiler le site dans un répertoire local, de copier les fichiers de configuration spécifiques à la production, puis de copier le contenu de la sortie précompilée dans production.

Pour obtenir un actualisateur sur la copie des fichiers de l’environnement de développement vers l’environnement de production, reportez-vous aux didacticiels [*déploiement de votre site Web à l’aide d’un client FTP*](deploying-your-site-using-an-ftp-client-cs.md) et [*déploiement de votre site Web à l’aide de Visual Studio*](determining-what-files-need-to-be-deployed-cs.md) .

## <a name="summary"></a>Récapitulatif

ASP.NET prend en charge deux modes de compilation : automatique et explicite. Comme indiqué dans les didacticiels précédents, les projets d’application Web (WAP) utilisent la compilation explicite, tandis que les projets de site Web (WSP) utilisent la compilation automatique par défaut. Toutefois, il est possible de compiler explicitement un WSP avant le déploiement à l’aide de l’outil de compilation ASP.NET.

Ce didacticiel se concentre sur la précompilation de l’outil de compilation pour la prise en charge du déploiement. Lors de la précompilation pour le déploiement, l’outil de compilation crée un dossier d’emplacement cible, compile le code source de l’application Web spécifiée et copie ces assemblys compilés et les fichiers de contenu dans le dossier d’emplacement cible. L’outil de compilation peut être configuré pour créer une interface utilisateur pouvant être mise à jour ou non pouvant être mise à jour. En cas de précompilation avec une option d’interface utilisateur qui ne peut pas être mise à jour, le balisage déclaratif dans les fichiers de contenu est supprimé. En résumé, la précompilation vous permet de déployer votre application basée sur un projet de site Web sans inclure les fichiers de code source et le balisage déclaratif supprimé, si vous le souhaitez.

Bonne programmation !

### <a name="further-reading"></a>informations supplémentaires

Pour plus d’informations sur les sujets abordés dans ce didacticiel, reportez-vous aux ressources suivantes :

- [Précompilation du site Web ASP.NET](https://msdn.microsoft.com/library/ms228015.aspx)
- [Code-behind et compilation dans ASP.NET 2,0](https://msdn.microsoft.com/magazine/cc163675.aspx)
- [Précompilation dans ASP.NET](http://www.odetocode.com/Articles/417.aspx)
- [Options de site précompilé dans ASP.NET](http://www.dotnetperls.com/precompiled)

> [!div class="step-by-step"]
> [Précédent](logging-error-details-with-elmah-cs.md)
> [Suivant](users-and-roles-on-the-production-website-cs.md)
