---
uid: web-pages/overview/getting-started/introducing-aspnet-web-pages-2/publishing
title: Présentation de la pages Web ASP.NET de la publication d’un site à l’aide de WebMatrix | Microsoft Docs
author: Rick-Anderson
description: Ce didacticiel est la dernière version de l’ensemble de didacticiels qui introduit pages Web ASP.NET et Microsoft WebMatrix. Il explique comment publier votre site t...
ms.author: riande
ms.date: 05/28/2015
ms.assetid: 7e85c70e-1a88-4408-8b3d-29611c7713ed
msc.legacyurl: /web-pages/overview/getting-started/introducing-aspnet-web-pages-2/publishing
msc.type: authoredcontent
ms.openlocfilehash: 49a841dbda183bf1d59153b83f694c9f517e0b94
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78633615"
---
# <a name="introducing-aspnet-web-pages---publishing-a-site-by-using-webmatrix"></a>Présentation de la pages Web ASP.NET de la publication d’un site à l’aide de WebMatrix

par [Tom FitzMacken](https://github.com/tfitzmac)

> Ce didacticiel est la dernière version de l’ensemble de didacticiels qui introduit pages Web ASP.NET et Microsoft WebMatrix. Il explique comment publier votre site sur Internet afin que d’autres personnes puissent l’utiliser. Il part du principe que vous avez terminé la série en [créant une apparence cohérente pour les Sites pages Web ASP.net](https://go.microsoft.com/fwlink/?LinkId=251585).
> 
> Vous allez apprendre à publier votre site à l’aide de :
> 
> - Microsoft Azure
> - Entreprise d’hébergement Web

## <a name="about-publishing-your-site"></a>À propos de la publication de votre site

Jusqu’à maintenant, vous avez effectué tout votre travail sur un ordinateur local, y compris le test de vos pages. Pour exécuter vos pages<em>. cshtml</em> , vous avez utilisé le serveur Web intégré à WebMatrix, à savoir IIS Express. Mais bien entendu, personne ne peut voir le site que vous avez créé, à l’exception de vous. Pour permettre à d’autres personnes de travailler avec votre site, vous devez le publier sur Internet.

À moins que vous n’ayez déjà accès à un serveur Web public, la publication signifie que vous devez disposer d’un compte avec une *plateforme Cloud* ou un *fournisseur d’hébergement*. Une plateforme Cloud, telle que Microsoft Azure, fournit une infrastructure à la demande pour vos applications. Un fournisseur d’hébergement est une société qui possède des serveurs Web accessibles publiquement et qui vous loueront de l’espace pour votre site. Les plans d’hébergement s’exécutent à partir de quelques dollars par mois (voire gratuitement) pour les petits sites à plusieurs centaines de dollars par mois pour les sites Web commerciaux à volume élevé.

> [!NOTE]
> Vous pouvez avoir accès à un serveur Web public par le biais du fournisseur de services Internet (ISP) que vous utilisez pour obtenir le service Internet chez vous. Toutefois, votre fournisseur d’hébergement doit prendre en charge pages Web ASP.NET. De nombreux ISP ne le sont pas, mais il est toujours intéressant de le vérifier.

Dans ce didacticiel, nous allons vous fournir une vue d’ensemble de la publication. Il n’est pas pratique de fournir des détails exacts pour tout, car le processus diffère légèrement pour chaque fournisseur d’hébergement. Mais vous allez avoir une bonne idée du fonctionnement du processus.

Ce didacticiel contient quatre sections :

1. [Configuration de la page par défaut](#defaultpage)
2. Publication (choisissez l’une des options suivantes)  
 a. [Publication de votre site sur Microsoft Azure](#azure)  
 b. [Publication de votre site sur une société d’hébergement Web](#host)
3. [Mise à jour du site actif : republication](#update)

<a id="defaultpage"></a>
## <a name="setting-up-the-default-page"></a>Configuration de la page par défaut

Quand un utilisateur accède à l’adresse de base de votre site Web, la page par défaut de votre site s’affiche pour l’utilisateur. Par exemple, lorsque *default. htm* est défini comme page par défaut pour le site sur `www.contoso.com`, la navigation vers `www.contoso.com` revient à accéder à `www.contoso.com/Default.htm`.

Actuellement, votre site utilise **default. cshtml** comme page par défaut. Cette page convient à votre page par défaut, mais dans ce didacticiel, vous n’avez ajouté aucun contenu à cette page pour qu’elle affiche une page vierge. Ouvrez default. cshtml et remplacez le contenu par le code suivant.

[!code-cshtml[Main](publishing/samples/sample1.cshtml)]

Votre site est maintenant prêt pour la publication. Tout d’abord, vous verrez comment déployer le site sur Azure, puis comment le déployer dans une société d’hébergement Web. L’une ou l’autre option fonctionne pour votre site Web, et vous devez uniquement suivre l’une des options de déploiement.

<a id="azure"></a>
## <a name="publishing-your-site-to-microsoft-azure"></a>Publication de votre site sur Microsoft Azure

Ce didacticiel vous montrera tout d’abord comment déployer votre site sur Microsoft Azure. En vous connectant avec un compte Microsoft, vous pouvez créer jusqu’à 10 sites gratuits sur Azure. Ces sites gratuits offrent un moyen pratique de tester vos sites. Vous pouvez toujours supprimer cet exemple de site ultérieurement pour éviter d’utiliser tous vos sites gratuits. Vous pouvez créer un compte d’essai gratuit en quelques minutes. Pour plus d’informations, consultez [Essai gratuit Azure](https://azure.microsoft.com/free/?WT.mc_id=A443DD604).

Dans le ruban WebMatrix, cliquez sur le bouton **publier** .

![Bouton « publier » dans le ruban WebMatrix](publishing/_static/image1.png)

La boîte de dialogue **publier votre site** s’affiche. Si vous n’êtes pas connecté à votre compte Microsoft, la boîte de dialogue contient un lien **prise en main d’Azure** . Cliquez sur ce lien.

![Publier votre site](publishing/_static/image2.png)

Si vous n’êtes pas connecté à un compte Microsoft, vous avez à nouveau la possibilité de vous connecter. Vous devez vous connecter à un compte Microsoft pour publier votre site sur Azure.

![Se connecter](publishing/_static/image3.png)

Une fois connecté à votre compte Microsoft, la boîte de dialogue contient des liens permettant de créer un nouveau site sur Azure ou de se connecter à l’un de vos sites existants sur Azure.

![Créer un site](publishing/_static/image4.png)

Sélectionnez **créer un nouveau site**.

Si vous avez nommé votre projet **WebPagesMovies**, le nom par défaut de votre site sera **webpagesmovies.azurewebsites.net**. Ce nom par défaut n’est probablement pas disponible, comme indiqué par le point d’exclamation rouge.

![nom du site Web par défaut](publishing/_static/image5.png)

Remplacez le nom du site par un nom qui est disponible, puis sélectionnez un emplacement proche de votre emplacement.

![nom du site modifié](publishing/_static/image6.png)

Cliquez sur **OK**.

WebMatrix permet d’effectuer un test pour déterminer si le serveur est compatible avec votre site.

![test de compatibilité](publishing/_static/image7.png)

Sélectionnez **Continuer**.

Les résultats du test de compatibilité sont affichés.

![résultat de la compatibilité](publishing/_static/image8.png)

Sélectionnez **Continuer**.

WebMatrix affiche les fichiers et les bases de données qui seront publiés sur le site. Comme c’est la première fois que vous publiez le site, tous les fichiers sont répertoriés. Vous pouvez décocher un fichier qui n’est pas prêt à être publié. Dans les publications suivantes, seuls les fichiers qui ont été modifiés seront affichés. Consultez [mise à jour du site actif : republication](#update).

![Publier l’aperçu](publishing/_static/image9.png)

Sélectionnez **Continuer**.

Une fois que le site a été déployé sur Azure, un message s’affiche pour indiquer que le déploiement est terminé.

![publication terminée](publishing/_static/image10.png)

Votre site et votre base de données ont été publiés sur Azure et sont désormais disponibles pour le public. Cliquez sur le lien dans le message indiquant que la publication est terminée, et vous verrez maintenant votre site déployé. Vous ou quiconque disposant d’un accès à Internet pouvez ajouter ou modifier des enregistrements dans la base de données.

![](publishing/_static/image11.png)

<a id="host"></a>
## <a name="publishing-your-site-to-a-web-hosting-company"></a>Publication de votre site sur une société d’hébergement Web

Si vous décidez de ne pas publier sur Azure, vous pouvez publier votre site vers une société d’hébergement Web.

Cliquez sur le lien **Rechercher l’hébergement Web** .

![Bouton « Rechercher l’hébergement Web » dans la boîte de dialogue Paramètres de publication](publishing/_static/image12.png)

Vous accédez à une page sur le site Microsoft qui répertorie les fournisseurs d’hébergement qui prennent en charge ASP.NET.

![Page sur le site Microsoft qui répertorie les fournisseurs d’hébergement](publishing/_static/image13.png)

Évidemment, il peut être difficile de savoir maintenant exactement quelles fonctionnalités d’hébergement vous pouvez avoir besoin à long terme. Voici quelques points à prendre en compte :

- Dans le cadre du site WebPagesMovies, vous n’avez pas besoin d’avoir un module complémentaire distinct pour SQL Server, ce qui est souvent un coût supplémentaire. Dans votre site, vous utilisez l’édition SQL Server Compact, qui est autonome. Toutefois, vous aurez peut-être besoin d’un accès SQL Server pour de futurs travaux de site Web. Si vous pensez que c’est possible, assurez-vous que vous pouvez ajouter SQL Server fonctionnalité ultérieurement.
- Vérifiez si le fournisseur d’hébergement prend en charge le protocole de publication Web Deploy. Vous pouvez publier à l’aide du protocole FTP, mais il est plus pratique d’utiliser Web Deploy.

Certains sites proposent une période d’évaluation gratuite. Une version d’évaluation gratuite est un bon moyen d’essayer la publication et l’hébergement, alors que vous avez toujours expérimentation avec WebMatrix et pages Web ASP.NET.

Choisissez celui que vous souhaitez. Pour ce didacticiel, nous avons sélectionné DiscountASP.NET, car pendant la création du didacticiel, cette entreprise avait une promotion qui permettait aux utilisateurs d’héberger un site gratuitement pendant quelques mois.

> [!NOTE]
> Le choix d’un fournisseur d’hébergement pour ce didacticiel ne doit pas être interprété comme une endossement de l’entreprise. Toutefois, nous avons dû choisir un exemple, et DiscountASP.NET est l’une des nombreuses entreprises qui prennent en charge pages Web ASP.NET et le protocole Web Deploy pour la publication.

En général, une fois que vous vous êtes inscrit auprès du fournisseur d’hébergement, la société vous envoie un e-mail contenant un nom d’utilisateur et un mot de passe, l’URL du serveur Web, etc. Si la société d’hébergement prend en charge Web Deploy protocole, elle peut vous envoyer un fichier contenant les paramètres de publication, ou vous permettre d’en télécharger un. Un fichier de paramètres de publication simplifie le processus pour vous.

Une fois que vous êtes inscrit et que vous êtes prêt à publier, cliquez sur le bouton **publier** dans le ruban WebMatrix. La boîte de dialogue **paramètres de publication** s’affiche.

Si le fournisseur d’hébergement vous a envoyé un fichier de paramètres de publication, cliquez sur le lien **Importer les paramètres de publication** et importez le fichier. Si vous n’avez pas de fichier de paramètres de publication, renseignez les champs en utilisant les valeurs que la société d’hébergement vous a envoyées par courrier électronique. Voici à quoi peut ressembler la boîte de dialogue **paramètres de publication** lorsque vous avez terminé :

![Les paramètres de publication sont renseignés dans la boîte de dialogue « Paramètres de publication »](publishing/_static/image14.png)

Cliquez sur **valider la connexion**. Si tout est correct, la boîte de dialogue signale **correctement la connexion**, ce qui signifie qu’elle peut communiquer avec le serveur du fournisseur d’hébergement.

![Message de réussite si les paramètres de publication sont corrects](publishing/_static/image15.png)

En cas de problème, WebMatrix fait de son mieux pour vous indiquer le problème :

![Message d’erreur en cas de problème avec les paramètres de publication](publishing/_static/image16.png)

Cliquez sur **Save** pour enregistrer vos paramètres. WebMatrix propose d’effectuer un test pour s’assurer qu’il peut communiquer correctement avec le site d’hébergement :

![Offre de message pour effectuer un test du processus de publication](publishing/_static/image17.png)

Cliquez sur **Oui**. WebMatrix charge certains fichiers d’exemple dans le fournisseur d’hébergement. Lorsque le test de compatibilité est terminé, WebMatrix signale les résultats :

![Résultats du test de publication](publishing/_static/image18.png)

Si vous êtes prêt, continuez et cliquez sur **Continuer** pour démarrer le processus de publication pour le réel. WebMatrix détermine les fichiers qui se trouvent dans votre site et qui sont déjà sur le serveur hôte (pour le moment, aucun) et vous donne un aperçu du processus de publication :

![Aperçu des fichiers que le processus de publication doit télécharger](publishing/_static/image19.png)

La liste des fichiers à publier comprend les pages Web que vous avez créées comme *movies. cshtml*. La liste comprend également des fichiers pour les applications auxiliaires que vous avez installées, les fichiers à exécuter SQL Server Compact édition de votre base de données, et ainsi de suite. Par conséquent, le processus de publication initial peut être substantiel.

Cliquez sur **Continuer**. WebMatrix copie vos fichiers sur le serveur du fournisseur d’hébergement. Une fois l’opération terminée, les résultats sont signalés dans la barre d’État :

![Message de la barre d’État lorsque le processus de publication s’est terminé avec succès](publishing/_static/image20.png)

Pour afficher votre site actif, cliquez sur le lien dans la barre d’État. Ajoutez des *films* à l’URL pour voir le fichier *movies. cshtml* que vous avez créé :

![Site actif avec la page films](publishing/_static/image21.png)

<a id="update"></a>
## <a name="updating-the-live-site-republishing"></a>Mise à jour du site actif : republication

Une fois que vous avez publié votre site (vers Azure ou une société d’hébergement Web), il existe deux copies de l’informatique &mdash; la version sur votre ordinateur et la version sur le fournisseur de services. Vous souhaiterez probablement poursuivre le développement du site (si rien d’autre, dans le cadre de l’ensemble suivant du didacticiel). Dans ce cas, vous devez republier votre site pour copier les modifications de votre ordinateur vers le fournisseur de services. Le processus de publication dans WebMatrix peut déterminer les fichiers qui ont été modifiés sur votre site et publier uniquement ces fichiers.

Pour voir comment fonctionne la republication, ouvrez le site *movies. cshtml* , apportez une petite modification, puis enregistrez le fichier. Par exemple, remplacez le titre par `Movies - Updated`.

Cliquez sur le bouton **publier** dans le ruban. WebMatrix détermine ce qui a changé et vous montre un aperçu des fichiers qu’il va publier.

![La boîte de dialogue « publier » qui indique les fichiers modifiés prêts pour la republication](publishing/_static/image22.png)

> [!IMPORTANT] 
> 
> Par défaut, WebMatrix publie votre base de données (fichier *. sdf* ) uniquement la première fois que vous publiez le site. Une fois que votre site est publié et que les utilisateurs interagissent avec le site Web, la base de données sur le site actif dispose généralement des données réelles du site. Vous devez veiller à ne pas remplacer la base de données active par le fichier *. sdf* qui se trouve sur votre ordinateur, qui contient généralement uniquement des données de test. C’est pour cette raison que la **publication d’avertissement remplace toutes les bases de données distantes**et pourquoi la case à cocher de *WebPagesMovies. sdf* est désactivée par défaut.

Cliquez sur **Continuer**. WebMatrix publie les fichiers modifiés et affiche un message de réussite, comme c’est le cas lors de la première publication.

Accédez au site actif (vous pouvez cliquer sur le lien dans le message de réussite s’il est toujours affiché) et vérifier que votre modification a été publiée.

> [!TIP] 
> 
> **Modification à distance de fichiers**
> 
> Comme alternative à la modification de votre site, puis à la republication, vous pouvez modifier les fichiers distants directement dans WebMatrix. Dans ce scénario, vous ouvrez un fichier qui se trouve sur le fournisseur de services, et WebMatrix en télécharge une copie que vous pouvez modifier. Chaque fois que vous enregistrez le fichier, WebMatrix envoie les modifications au site.
> 
> La modification à distance est un moyen facile d’apporter des modifications à votre site actif. Toutefois, les modifications que vous apportez de cette façon ne sont pas synchronisées avec les fichiers de votre site local. Pour synchroniser les fichiers locaux avec le site distant, vous pouvez télécharger les fichiers distants. Ce processus fonctionne de façon très similaire à la publication, sauf en sens inverse.
> 
> Nous ne décrirons pas ici plus d’informations sur les fonctionnalités d’édition à distance et de téléchargement à distance de WebMatrix. Elles sont très utiles si plusieurs personnes doivent travailler sur le même site sur différents ordinateurs. Pour plus d’informations, consultez [publier et modifier un site distant avec WebMatrix 2 Beta](https://go.microsoft.com/fwlink/?LinkId=251591).

## <a name="additional-resources"></a>Ressources supplémentaires

- [ASP.net webmatrix pages Web ASP.net forum](https://forums.asp.net/1224.aspx/1?WebMatrix+and+ASP+NET+Web+Pages), un endroit idéal pour poser des questions et obtenir des réponses.

> [!div class="step-by-step"]
> [Précédent](layouts.md)
